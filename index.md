![logo](/images/clanrobin_logo.jpg)
# Exploring Statistical Learning and Data Science
Here you can find my different tinkerings with a variety of statistical learning packages, along with some of my own astronomical research.

## Stanford Stats Course
Stanford University has a self-paced statistics course run by Prof Trevor Hastie and Prof Robert Tibshirani.
The course covers a wide variety of statistical learning packages and shows how these tools can be implemented in R. I highly recommend it. To show my progress, I've added the R code that comes with the course with some of my own tweaks and my attempts at the questions at the end of each chapter. I can't guarantee that my approach is 100% correct and it most likely isn't the most efficient code but the point here is to demonstrate working with R to try to solve problems.

For each chapter, I've listed an overview of what was covered and a link to the R code to demonstrate some of those topics. For the early chapters I've converted the original .R code to .Rmd for ease of viewing.

### Overview of Statistical Learning
Chapter 2 of the course, covering Regression Models, Dimensionality and Structured Models, Model Selection and Bias-Variance Tradeoff, Classification, and an introduction to R.
<!-- My R code on [StatsCourse_Ch2](/docs/ch2.html) -->

### Linear Regression
Chapter 3 covers: Simple Linear Regression, Hypothesis Testing and Confidence Intervals, Multiple Linear Regression, Extensions of the linear model and Linear Regression in R.
<!-- My R code on [StatsCourse_Ch3](/docs/ch3.html) -->

### Classification
Chapter 4 covers: Logistic Regression, Multivariate Logistic Regression, Case-control sampling and multi-class, Discriminat Analysis, Gaussian Discriminat Analysis (one and many variables), Quadratic Discriminant Analysis and Naive Bayes, Classification in R.
<!-- My R code on [StatsCourse_Ch4](/docs/ch4.html) -->

### Resampling Methods
Chapter 5 covers: Cross-Validation, K-fold Cross-Validation, the right and wrong way, The Bootstrap, Resampling in R.
<!-- My R code on [StatsCourse_Ch5](/docs/ch5.html) -->

### Linear Model Selection and Regularization
Chapter 6 covers: Best-Subset selection, Stepwise selection, Backward stepwise selection, Estimating test error, Validation and cross-validation, shrinkage methods and ridge regression, The Lasso, Tuning parameter selection, Dimension Reduction methods, Principal Components Regression and Partial Least Squares, Model selection in R.

My R code on [StatsCourse_Ch6](/docs/ch6.html)

### Moving Beyond Linearity
Chapter 7 covers: Polynomials and Step functions, Piecewise-Polynomials and Splines, Smoothing Splines, Generative Additive Models and Local Regression, Nonlinear functions in R.

My R code on [StatsCourse_Ch7](/docs/ch7.html)

### Tree-Based Methods
Chapter 8 covers: Tree-based methods, classification trees, Bagging and Random Forest, Boosting, Tree-based methods in R.

My R code on [StatsCourse_Ch8](/docs/ch8.html)

### Support Vector Machines
Chapter 9 covers: Optimal Separating Hyperplanes, Support Vector Classifier, Feature expansion and the SVM, Comparison with Logistic Regression, SVMs in R.

My R code on [StatsCourse_Ch9](/docs/ch9.html)

### Unsupervised Learning
Chapter 10 covers: Principal Components, Higher Order Principal Components, k-means Clustering, Hierarchical Clustering, Unsupervised in R.

My R code on [StatsCourse_Ch10](/docs/ch10.html)

## Scikit-learn
My page on [Scikit-learn](/scikit-learn/scikit-learn.md) 

<!--
[scikit-learn](scikit-learn.org)

Let's start with the Scikit-learn routines, to be honest, I tried skimming ahead on these examples just so I could run some code but ended up, very quickly, having no idea what was going on. So let's try and pull all the pieces together.



## The Iris dataset: 
### [data and plots](]http://scikit-learn.org/stable/auto_examples/datasets/plot_iris_dataset.html)
So this dataset is used to show primarily how Support Vector Machines are good at finding and identifying sub-groups within a given dataset. In this example, based on the properties of three different types of Irises, SVMs are able to find a robust way to determine which Iris is which.

So scikit-learn tries to teach us something about SVM's here, so let's have a look at what is going on.

To start with let's fiddle a bit with the plots which just present the data (see link in "data and plots").
The first bit plots the physical parameters being measured, namely Sepal and Petal length and width in centimetres (as best as I can guess). The Sepal, for those curious, is bascially the bud casing of the flower that folds back when the flower blooms and the petals appear.

#### Petal data plots
![Petals](/images/Iris_petals_dimension.png)

#### Sepal data plots
![Petals](/images/Iris_sepals_dimension.png)

#### Principal Component Analysis Eigenvectors
![Petals](/images/Iris_PCA_directions.png)

On the scikit-learn page, they only plot the Sepal data which does show that the red points are distinct from the grey and orange ones but not as clearly as can be seen in the Petal data. So it is good we had a look to see whether determining a "red" type of Iris is really challenging or not.

From the first steps of the PCA analysis we can see that there is a plane which provides a fairly good separation between "grey" and "orange" irises. Let's see if we can explore that a bit further.

## The digits dataset: 
### [data and examples](]http://scikit-learn.org/stable/auto_examples/classification/plot_digits_classification.html#)
The other dataset is used to show how machine learning can help you classify something like handwritten numbers and identify which number you think it might be. 

-->

<!--
### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/clanrobin/clanrobin.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
-->
