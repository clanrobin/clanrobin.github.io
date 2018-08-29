## Scikit-learn
[Homepage of scikit-learn](scikit-learn.org)

Let's start with the Scikit-learn routines, to be honest, I tried skimming ahead on these examples just so I could run some code but ended up, very quickly, having no idea what was going on. So let's try and pull all the pieces together.



## The Iris dataset: 
### [data and plots](]http://scikit-learn.org/stable/auto_examples/datasets/plot_iris_dataset.html)
So this dataset is used to show primarily how Principal Component Analysis is good at finding and identifying sub-groups within a given dataset. In this example, based on the properties of three different types of Irises, SVMs are able to find a robust way to determine which Iris is which.

So scikit-learn tries to teach us something about PCA's here, so let's have a look at what is going on.

To start with let's fiddle a bit with the plots which just present the data (see link in "data and plots").
The first bit plots the physical parameters being measured, namely Sepal and Petal length and width in centimetres (as best as I can guess). The Sepal, for those curious, is bascially the bud casing of the flower that folds back when the flower blooms and the petals appear.

#### Petal data plots
![Petals](https://clanrobin.github.io/images/Iris_petals_dimension.png)

#### Sepal data plots
![Petals](https://clanrobin.github.io/images/Iris_sepals_dimension.png)

#### Principal Component Analysis Eigenvectors
![Petals](https://clanrobin.github.io/images/Iris_PCA_directions.png)

On the scikit-learn page, they only plot the Sepal data which does show that the red points are distinct from the grey and orange ones but not as clearly as can be seen in the Petal data. So it is good we had a look to see whether determining a "red" type of Iris is really challenging or not.

From the first steps of the PCA analysis we can see that there is a plane which provides a fairly good separation between "grey" and "orange" irises. Let's see if we can explore that a bit further.

## The digits dataset: 
### [data and examples](]http://scikit-learn.org/stable/auto_examples/classification/plot_digits_classification.html#)
The other dataset is used to show how machine learning can help you classify something like handwritten numbers and identify which number you think it might be. 


