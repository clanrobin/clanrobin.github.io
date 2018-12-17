
# Gaussian Mixture Model with pygmmis

As part of a recent research effort we have been exploring using a Gaussian Mixture Model to identify clusters within our data. In the scenario presented below, we let our code generate 3 random Gaussian overdensities with foreground screen of points and then apply the clustering algorithm to try to recover them.

The basic clustering algorithms are not designed to tell you how many clusters there are, but rather if combined with a diagnostic tool like the Akaike Information Criterion or the Bayes Information Criterion can provide a way to discern the optimum number of clusters.

In this example, we're not using the typical sklearn GMM package because our problem needs to be able to handle noise, poorly sampled Gaussians and truncated clusters.

The top of the code imports all the relevant packages and then we'll define some functions.

**an example plot is at the end of the page**


```python
import warnings
warnings.filterwarnings("ignore", category=DeprecationWarning)

import numpy as np
from matplotlib import pyplot as plt
import pandas as pd
import seaborn as sns

from astropy.stats import mad_std
from numpy.random import RandomState

import pygmmis
from sklearn.metrics.pairwise import euclidean_distances
import sys, os
```

## Generating the model and defining the Bayes Information Criterion

* ISLR_BIC is taken from the book "An Introduction to Statistical Learning by Gareth James, Daniela Witten, Trevor Hastie and Robert Tibshirani"

* covariance_matrix, x_standard_dev, y_standard_dev and rotate are setup to allow the test Gaussian overdensities to be built on the fly.

* sse is used inside the calculation of the ISLR_BIC



```python
def ISLR_BIC(k,n, sse, sigma):
    BIC = (sse + np.log(n)*k*sigma**2)/n #there is a *sigma**2 too
    return BIC

def covariance_matrix(xstd,ystd, corr):
    return [[xstd**2          , xstd*ystd*corr],
        [xstd*ystd*corr,           ystd**2]]

def x_standard_dev(rh, pixscale):
    return rh*60./pixscale

def y_standard_dev(rh, ellip, pixscale):
    return (1-ellip)*rh*60./pixscale

def rotate(x,y,mean,theta):

    phi = np.radians(theta)
    newx = ((x-mean[0])*np.cos(phi) - (y - mean[1])*np.sin(phi)) + mean[0]
    newy = ((y-mean[1])*np.cos(phi) + (x - mean[0])*np.sin(phi)) + mean[1]

    return newx,newy

def sse(X, X_centres):
    #sum of residual errors when comparing the y position of the points with the cluster centre.
    sse = sum(euclidean_distances(X, [X_centres]))
    return sse

```

## Generating the Gaussian clusters to run through the clustering algorithm

Our problem has some very specific dimensionality to it, so here we set that up and also add a foreground screen of points.


```python
naxis = [4251.,4101.] #size in pixels
mean = [naxis[0]/2., naxis[1]/2.]
pixscale = 0.08 #arcseconds per pixel

rh = 0.77 #in arcminutes
rh_scale = 1.0*rh

#generate smooth random field for foreground noise
pix_area = naxis[0]*naxis[1]
arcmin_area = 1.1*naxis[0]*pixscale*1.1*naxis[1]*pixscale/3600.
stars_per_square_arcminute = 13
foreground_stars = int(stars_per_square_arcminute * arcmin_area)

instances = 1

for pp in range(instances):

    print('Testing instance: ', pp)

    #let's make the background 10% larger then slide it back 5%
    fore_x = np.random.random(size=foreground_stars)* naxis[0]*1.1 - naxis[0]*0.05
    fore_y = np.random.random(size=foreground_stars)* naxis[1]*1.1 - naxis[1]*0.05

    fore_xy = np.array(list(zip(fore_x,fore_y)))
    
    blobs = 3 # here you can set how many Gaussian overdensities you want in the field
    blob_n = int((2110 - foreground_stars)/blobs) # and how many points each overdensity should have

    n = 2110
    
    print(n)

    for i in range(blobs):
        mean = np.random.random(size=2)*naxis
        ellip = np.random.random(size=1)*0.5
        rh = np.random.random(size=1)*rh_scale
        xstd = x_standard_dev(rh, pixscale)
        ystd = y_standard_dev(rh, ellip, pixscale)
        theta = np.random.random(size=1)*180.
        corr = 0.0
        covs = covariance_matrix(xstd[0],ystd[0],corr)
        x,y = np.random.multivariate_normal(mean, covs, int(blob_n)).T

        #rotate blobs by theta
        newx, newy = rotate(x,y,mean,theta)

        X0 = np.array(list(zip(newx,newy)))
        if i == 0:
            X = X0
        else:
            X = np.concatenate((X,X0))
        print(i, np.shape(X), xstd, ystd, mean, ellip)

    ##add foreground
    X = np.concatenate((X,fore_xy))

```

## Set up the clustering algorithm

Now that we've generated X with contains in it, the randomly generated clusters and a foreground/background screen of points for noise, we can set up the basic parameters of the clustering algorithm.




```python
    footprint = X.min(axis=0), X.max(axis=0)
    amp = foreground_stars/float(len(X))
    print('amp is: ', amp)

    bg = pygmmis.Background(footprint, amp=amp)

    D=2
    seeing = 0.4 #arcseconds

    dispersion = seeing/pixscale

    w = 0.1    # minimum covariance regularization, same units as data
    cutoff = 3# segment the data set into neighborhood within 5 sigma around components
    tol = 1e-3 # tolerance on logL to terminate EM

    # define RNG for deterministic behavior
    
    seed = 402
    #seed = int(np.random.random(size=1)[0]*100000)
    print(seed)
    rng = RandomState(seed)

    k_max = 10 # maximum number of clusters to test
    steps = 100 # number of times we check a given combination of objects.
    n = len(X)

```

## Running the clustering algorithm

At this point we're ready to go, we can now test our distribution for 1 to k_max clusters where for each set we repeat it "Steps" number of times to make sure we aren't getting stuck in some local minima.

We also need to do some house keeping by creating some empty arrays we can populate with the results as we generate them. I also include some checks on the analysis to avoid any results being "empty" or "NaN".



```python
    if (pp == 0):
        big_array = np.empty(shape=(instances, k_max, k_max, steps, n+7))
        big_array.fill(np.nan)

        big_ISLRbic_array = np.empty((instances, k_max, steps))
        big_ISLRbic_array.fill(np.nan)


    sse_s = np.empty(shape= (k_max,k_max,steps)) # empty array to store the sse's for each run of clusters.
    sse_s.fill(np.nan)

    for k in range(1,k_max+1):

        print('Number of Clusters being tested: ', k)


        for l in range(steps):

            total_sse= 0
            checker = 0
            while True:
                gmm = pygmmis.GMM(K=k,D=D)

                logL, U = pygmmis.fit(gmm, X,
                init_method='minmax',
                covar_callback=None,
                w=w,
                cutoff=cutoff,
                background=bg,
                tol=tol,
                rng=rng)

                if (checker == 20):
                    sys.exit(1)
                elif (k == 1):
                    break
                elif (np.isnan(np.max(gmm.mean[k-1])) == False):
                    #print("all is good", gmm.mean[k-1], k-1)
                    break
                elif (np.isnan(np.max(gmm.mean[k-1])) == True):
                    print("trying again ", k,l, gmm.mean[k-1], checker)
                    checker += 1

            variables = k*(1. + D*(D+1)/2.)

            for j in range(k): #process all clusters
                #store the amp, mean and covars
                big_array[pp,k-1,j,l,0] = gmm.amp[j]
                big_array[pp,k-1,j,l,1:3] = gmm.mean[j]
                big_array[pp,k-1,j,l,3:5] = gmm.covar[j][0]
                big_array[pp,k-1,j,l,5:7] = gmm.covar[j][1]
                big_array[pp,k-1,j,l,7:][U[j]] =  1.0


                weights = np.ones((len(X[U[j]]),2))
                #X_centres = weights*gmm.mean[j]
                if len(U[j]) != 0:
                    total_sse += sse(X[U[j]], gmm.mean[j])
                else:
                    #total_sse += (total_sse + 0.001)/(float(j) + 0.001)
                    print('Empty Array', k, j, l, gmm.mean[j])
                    total_sse = np.nan
                    continue

            num = big_array[pp,k-1,:,l,7:].size - np.isnan(big_array[pp,k-1,:,l,7:]).sum()

            big_ISLRbic_array[pp, k-1, l] = ISLR_BIC(variables, num, total_sse, dispersion)

```

## Plotting the Results

It's difficult to plot everything in this sort of analysis so instead we're going to make a series of subplots to capture different aspects of this problem.

Firstly, we will make a triangle plot to show the **best** result for each cluster test. Keep in mind we ask the clustering algorithm if you have to fit **1** cluster, where would it be. Now we plot the best case for 1 cluster. Then we ask if you have to fit **2** clusters, where would it be. Now we plot the best case for 2 clusters and so on down to k_max clusters.

Secondly, we will present the Bayes Information Criterion figure (ISLR_BIC), but we need to collate the results, so for each cluster set we plot the **median** BIC and the **standard deviation** around that point. Sometimes it's good to have a few other tricks up your sleeve, so there is also a way to plot the **Median Absolute Deviation** instead.
I've also tried to foolproof this by making sure than any NaNs don't trip us up.

Thirdly, we will also plot the **true** distribution of points with the background for comparison.

Finally, since the mean and std deviation sometimes obscure what is really happening with the data, we'll add a violin plot so we can keep an eye on how the results are distributed within each cluster set.



```python
    fig = plt.figure(figsize=(14,14))
    fig.suptitle('Test ' + str(pp))
    plt.subplot(3,3,3)
    plt.title('Actual distribution')
    plt.xlabel('x')
    plt.ylabel('y')
    plt.ylim(0.0, naxis[1])
    plt.xlim(0.0, naxis[0])

    for i in range(blobs):

        plt.plot(X[(i*(blob_n+1)):((i+1)*blob_n),0],X[(i*(blob_n+1)):((i+1)*blob_n),1],'.', alpha=0.1)

    plt.plot(fore_x,fore_y, '.', alpha=0.1)

    median_ISLRbic_array = np.nanmedian(big_ISLRbic_array[pp,::],axis=1)
    mean_ISLRbic_array = np.nanmean(big_ISLRbic_array[pp,::],axis=1)
    mad_ISLRbic_array = mad_std(big_ISLRbic_array[pp,::],axis=1, ignore_nan=True)
    std_ISLRbic_array = np.nanstd(big_ISLRbic_array[pp,::],axis=1)
    std_upper_ISLRbic = (mean_ISLRbic_array+std_ISLRbic_array)/np.nanmax(mean_ISLRbic_array)
    std_lower_ISLRbic = (mean_ISLRbic_array-std_ISLRbic_array)/np.nanmax(mean_ISLRbic_array)
    upper_ISLRbic = (median_ISLRbic_array+mad_ISLRbic_array)/np.nanmax(median_ISLRbic_array)
    lower_ISLRbic = (median_ISLRbic_array-mad_ISLRbic_array)/np.nanmax(median_ISLRbic_array)

    for kk in range(1,k_max+1):

        minni = np.nanargmin(big_ISLRbic_array[pp,kk-1,:])

        for jj in range(kk):

            ax = plt.subplot(k_max, k_max, (kk-1)*k_max + 1 + jj*1)
            plt.ylim(footprint[0][1], footprint[1][1])
            plt.xlim(footprint[0][0], footprint[1][0])
            if kk < k_max:
                ax.set_xticklabels([])
                ax.tick_params(bottom="off")
            else:
                ax.set_xlabel(str(jj))

            if jj > 0:
                ax.set_yticklabels([])
                ax.tick_params(left="off", bottom="off")
            else:
                ax.set_ylabel(str(kk), rotation=0)

            if (kk == k_max):
                ax.tick_params(bottom="on")

            plt.plot(X[big_array[pp,kk-1, jj,minni, 7:]==1][:,0], X[big_array[pp,kk-1, jj,minni, 7:]==1][:,1], '.', alpha=0.1)



    plt.rcParams['font.family'] = 'serif'
    plt.rcParams['font.size'] = 6
    plt.rcParams['xtick.labelsize'] = 'small'
    plt.rcParams['ytick.labelsize'] = 'small'
    plt.rcParams['axes.xmargin'] = 0.08

    fig.text(0.5, 0.07, 'Cluster Number', ha='center', size='x-large')
    fig.text(0.07, 0.5, 'Number of Clusters', va='center', rotation='vertical', size='x-large')

    ax = plt.subplot(3,3,6)

    ax.yaxis.tick_right()
    ax.set_xticks((range(1,k_max+1,1)))
    ax.set_xticklabels(range(1,k_max+1))

    medmad = 0

    if (medmad == 0):
        print("I'm going to plot the mean + std")
        ax.set_title('Mean BIC values +/- STD')
        ax.plot(range(1, k_max+1), mean_ISLRbic_array/np.nanmax(mean_ISLRbic_array), label='ISLR BIC')
        ax.fill_between( range(1,k_max+1), std_upper_ISLRbic,std_lower_ISLRbic, alpha=0.2 )


    else:
        print("I'm going to plot the median + add")
        ax.set_title('Median BIC values +/- MAD')
        ax.plot(range(1, k_max+1), median_ISLRbic_array/np.nanmax(median_ISLRbic_array), label='ISLR BIC')
        ax.fill_between( range(1,k_max+1), upper_ISLRbic,lower_ISLRbic, alpha=0.2 )

    ax.legend(loc=0)
    ax.set_xlabel('Number of Clusters')


    ax = plt.subplot(3,3,2)
    ax.set_title("BIC ViolinPlot")
    ax.tick_params(axis="x",direction="in", pad=-10)
    ax.tick_params(left="off")
    df = pd.DataFrame(big_ISLRbic_array[pp,::].T)
    df.columns = ['1','2','3','4','5','6','7','8','9','10']
    sns.violinplot(data=df, inner=None, width=1.9)
    ax.plot(range(k_max), df.median(axis=0),'.', mfc='white', mec='black')
    ax.set_ylim(top=1.1*max(df.median(axis=0)))
    ax.set_yticklabels([])

    plotnum = 0
    filepath = '/your/path/'
    variant = 'your_file_name_'


    plotname = str(variant) + str(pp) +'_'+ str(plotnum) + '.png'
    while os.path.exists(filepath+plotname):
        plotnum+=1
        plotname = str(variant) + str(pp) +'_'	+ str(plotnum) + '.png'

    print('Saving file to: ',filepath+plotname)
    fig.savefig(filepath+plotname)
#     plt.close('all')

plt.show()

```

## Saving the Results

For good measure let's save the results to file so we can dig around in more detail later if we so choose.



```python
bdf = pd.DataFrame([], columns = ['Instance', 'Number of Clusters', 'Cluster Number', 'Step', 'Amplitude', 'X Mean', 'Y Mean', 'X Covariance', 'XY Covariance', 'YX Covariance','Y Covariance', 'Members'])

for pp in range(instances):
    for k in range(1,k_max+1):
        for l in range(steps):
            for j in range(k):
                row = [[ pp, k, j, l, big_array[pp,k-1,j,l,0],big_array[pp,k-1,j,l,1],big_array[pp,k-1,j,l,2],big_array[pp,k-1,j,l,3],big_array[pp,k-1,j,l,4],big_array[pp,k-1,j,l,5],big_array[pp,k-1,j,l,6], n - np.isnan(big_array[pp,k-1,j,l,7:]).sum() ]]
                bdf = bdf.append(pd.DataFrame(row, columns=['Instance', 'Number of Clusters', 'Cluster Number', 'Step', 'Amplitude', 'X Mean', 'Y Mean', 'X Covariance', 'XY Covariance', 'YX Covariance','Y Covariance', 'Members']), ignore_index=True, sort=False)
                
                
                
bdf.to_csv(filepath + 'bdf.csv')

```

## Example Figure
![Clustering Figure](https://clanrobin.github.io/Clustering/SMS_pygmmisTest_SmallBlobsExample.png)
