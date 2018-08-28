# Welcome to Clan Robin
![logo](/images/clanrobin_logo.jpg)

## Stanford Stats Course Homework
My R code on [StatsCourse_Ch6](/docs/ch6.Rmd)

## Scikit-learn
My page on [Scikit-learn](/clanrobin.github.io/scikit.md)

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
