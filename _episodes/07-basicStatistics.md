---
title: "Performing basic statistics in R" 
teaching: 10
exercises: 10
questions:
- "How can we calculate the mean and standard deviation of a dataset?"
- "How do we quantify how correlated two variables are in R?"
objectives: 
- "Be able to perform a wide range of basic statistical computations in R." 
- "Be able to extend, using lessons taught earlier on the use of apply, basic statistics function that use a numeric vector as an input to numeric data frames and matrices."
- "Be aware of the importance of knowing the data at hand, including the nature of the data one is working with."
keypoints:
- "The `summary` function is used to provide a quick overview of numeric data frames."
- "The same function in R can give different results depending on the data type."
- "mean and standard deviation for a vector are calculated using the `mean` and `sd` functions respectively."
- "`apply` can be used to extend the `mean` and `sd` functions to perform summary statistics on data frames or matrices."
- "`cor` is used to calculate correlation coefficient between two vectors, or if a matrix is provided, pairwise correlation coefficients between variables."
- "It is important to know what type of data one has on hand, as functions require specific data types as inputs."
- "Default options might not always be the most appropriate for the data at hand, and hence it is important to know what the defaults are when writing scripts." 
---
# Statistics in R 
As the language favored by statisticians, R has a wealth of statistical functions (and a even greater wealth of packages that incorporate more advanced statistical methods). In this section, we will provide an overview of some basic statistics that can be done in R. 

> ## Setting up 
>
> For this section and the next, we will require some toy data to work with. We will use the famous *iris* dataset that is provided in R. To load the dataset, simply issue the command `data(iris)`. This will create an object *iris* in the workspace. Throughout, we will refer to this dataset in the code snippets provided.
{: .callout}

## Summarizing data and descriptive statistics
The `summary()` function provides a way to quickly summarize the data at hand. The following is the output from `summary()` when we run it on *iris*:

~~~
> summary(iris)
  Sepal.Length    Sepal.Width     Petal.Length    Petal.Width 
 Min.   :4.300   Min.   :2.000   Min.   :1.000   Min.   :0.100 
 1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600   1st Qu.:0.300 
 Median :5.800   Median :3.000   Median :4.350   Median :1.300 
 Mean   :5.843   Mean   :3.057   Mean   :3.758   Mean   :1.199 
 3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100   3rd Qu.:1.800 
 Max.   :7.900   Max.   :4.400   Max.   :6.900   Max.   :2.500 
       Species 
 setosa    :50 
 versicolor:50 
 virginica :50
~~~

Noticeably, R was able to figure out that the data in the last column (that of species information) cannot be represented using summary statistics such as the mean or the median because it is a categorical variable. This is another key feature of R: the same function can have different effects depending on the type of data the function is being applied on. For further illustration, lets see what happens if *iris* is to be a matrix instead of a data frame: 

~~~ 
 > summary(as.matrix(iris))
  Sepal.Length  Sepal.Width  Petal.Length  Petal.Width       Species 
 5.0    :10    3.0    :26   1.4    :13    0.2    :29   setosa    :50 
 5.1    : 9    2.8    :14   1.5    :13    1.3    :13   versicolor:50 
 6.3    : 9    3.2    :13   4.5    : 8    1.5    :12   virginica :50 
 5.7    : 8    3.4    :12   5.1    : 8    1.8    :12
 6.7    : 8    3.1    :11   1.3    : 7    1.4    : 8 
 5.5    : 7    2.9    :10   1.6    : 7    2.3    : 8 
 (Other):99    (Other):64   (Other):94    (Other):68 
~~~
{: >R}

Instead of summarizing it using summary statistics such as the mean and the median as observed in the previous output, *summary* now returns us the frequency of each observed value. 

Two other important statistics that are commonly calculated are the **mean** and the **standard deviation**. Unlike `summary()` that can be called on dataframes and matrices, the `mean()` and the `sd()` functions require the input be a numeric vector. 

> ## Finding row and column means 
>
> Although `mean()` is used to calculate the mean of a numeric vector, a common task is to calculate the average of columns and/or row. There are two ways to do so: one using `apply()` and one using a built in R function. Using apply to find the column mean, one can do the following:

~~~
apply(iris[,1:4], 2, mean)
~~~
> The snippet can be read as: "On the first 4 columns from *iris*, calculate the mean along the columns (margin=2)". Instead of using '2' as the margin, one can use '1' if they are interested in row means. 
>
> One can alternatively use the built in R functions `colMeans()` and `rowMeans()` respectivey for the same purpose. 

> ## Finding column and row standard deviations
>
> There does not exist any function analgous to `rowMeans()` and `colMeans()` for the calculation of standard deviation. How will you write a script to calculate the row and column standard deviation? 
> *Hint: You can use apply to do so.*
{: .challenge}  

## Measuring correlations 

From the manual

~~~
cor                   package:stats                    R Documentation
Correlation, Variance and Covariance (Matrices)
Description:

     'var', 'cov' and 'cor' compute the variance of 'x' and the
     covariance or correlation of 'x' and 'y' if these are vectors.  If
     'x' and 'y' are matrices then the covariances (or correlations)
     between the columns of 'x' and the columns of 'y' are computed.

     'cov2cor' scales a covariance matrix into the corresponding
     correlation matrix _efficiently_.
~~~
{: .output}

We can calculate very quickly and efficiently the correlation coefficients between two variables using the cor. For example, to find how well correlated sepal and petal width are, we can do the following: 

~~~
> cor(iris$Sepal.Width, iris$Petal.Width)
[1] -0.3661259
~~~
{: .R}

In the above snippet, we calculated the correlation between two variables -- sepal width and petal width. What if we wanted to calculate the pairwise correlation of all the different variables? Do we calculate all the possible pairs, and then tabulate our own table? Fortunately, this is not the case. From the manual, we see that 'x' can be a dataframe or a matrix. Furthermore, we also know that if x is a data frame or a matrix, we get pairwise correlations. So for instance

~~~
> cor(iris)
Error in cor(iris) : 'x' must be numeric
~~~
{: .R}

What was happening? As it turns out, the error was thrown because column 5 of our data contains information about Species. However, this information is not stored as a numeric data. As a result, the correlation cannot be calculated. Throwing a quick fix by excluding the problematic column (or including only the first 4 columns):

~~~
> cor(iris[,1:4])
             Sepal.Length Sepal.Width Petal.Length Petal.Width
Sepal.Length    1.0000000  -0.1175698    0.8717538   0.8179411
Sepal.Width    -0.1175698   1.0000000   -0.4284401  -0.3661259
Petal.Length    0.8717538  -0.4284401    1.0000000   0.9628654
Petal.Width     0.8179411  -0.3661259    0.9628654   1.0000000
~~~
{: .R}

Instead of a number, `cor()` now provides us a correlation matrix between all 4 variables. Quite handy! 

>## The importance of knowing what the default options are
>It is important to be aware of what the output represents. Specifically, because we did not specify the type of correlation coefficient, R simply used the default (which is the Pearson correlation coefficient). However, it is also able to calculate a few other correlation coefficients, including the Spearman correlation coefficient and Kendall's tau. The Pearson correlation coefficient might not always be the best or even the appropriate measure. This highlights the need to know and understand what exactly are the default options when we do any sort of analysis in R. 

Calculating correlations is one of the most common tasks in analysis. However, it is important to be aware of the limitations of using correlation coefficients as well as the underlying assumptions for each type of correlation coefficient. 




