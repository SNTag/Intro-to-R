---
title: Data types in R
teaching: 5
exercises: 0
questions: 
- How is data represented in R?
- How do we extract specific entries in R?
objectives: 
- Recognise the different data types in R.
- Be able to index and subset different classes of data in R. 
keypoints: 
- Both data frame and matrix are two dimensional objects with rows and columns. 
- Data frames can have different modes of data for different columns, while all the
  entries in the matrix must be of the same mode.
- Subsetting can be done with [] or [[]] in R.
---

## R data has types

Objects in R represent data, and data comes in different types. The data type may by
simple types, such as *numeric*, *logical*, *character*, etc., or complex types like data
frames, lists, and sophisticated objects. In R, variable typing is dynamic: you don't have
to specify the type of a variable before you assign a value to it. But the data type is
important, because it determines what operations are valid: you can take the sum of
numeric data, but not character data.


## Data classes in R

## Vectors

All data in R is actually stored as vectors. A simple number, for example, is a vector of
length 1. So is a simple quoted string.  Try it!

```r
> 5
[1] 5
```

The `[1]` shown before `5` indicates the index of the first entry of the vector. If the
vector were long enough to spill over multiple lines, the index of the first entry on each
line would be indicated. For example, the integers from 1 to 100 (inclusive) can be
represented by the convenient shorthand:

```r
> x <- 1:100
> x
  [1]   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17  18
 [19]  19  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35  36
 [37]  37  38  39  40  41  42  43  44  45  46  47  48  49  50  51  52  53  54
 [55]  55  56  57  58  59  60  61  62  63  64  65  66  67  68  69  70  71  72
 [73]  73  74  75  76  77  78  79  80  81  82  83  84  85  86  87  88  89  90
 [91]  91  92  93  94  95  96  97  98  99 100
```

To access an element of a vector, use the index of the desired elements in square
brackets, e.g., `x[10]` for the tenth element of `x`.  The first element of R objects is
element 1.

A vector contains data of a single basic data type. Basic data types in R include the
following

### Logical 

**Logical** data can only take on TRUE or FALSE values, and are commonly encountered when we test if a certain condition is fulfilled. For example: 

~~~
> 5 > 4
[1] TRUE

> class(5 > 4)
[1] "logical"
~~~
{: R}

Although the test returns a value of TRUE, this TRUE is not in fact a character object but a logical object. 

### Characters 

The **character** data type holds ordinary strings of characters. A single string, such as "hello", is a character vector of length 1. 

### Numeric and integers

The **numeric** data type is used for ordinary real numbers, including integers. You can
also use the **integer** data type if the rare case where you know that's appropriate and
useful. The default in R, however, is for numeric data to use the numeric data type, even if handed an
integer. You can force the conversion with an "L" after the number, or by using `as.integer()`.

~~~
> x <- 3.14
> class(x)
[1] "numeric"
> class(3)
[1] "numeric"
> class(3L)
[1] "integer"
> class(as.integer(3))
[1] "integer"
~~~

**Warning**: Converting a number fo an integer **rounds down**. For example:

~~~
> as.integer(3.6)
[1] 3
~~~
 {: R}
 

This behavior is extremely important to bear in mind, as it defies common mathematics
(which will have yielded 4 because 3.6 should have been rounded up instead).

> ## Converting between classes 
> 
> R will implicitly convert data between different classes to try to Do The Right Thing.
> For example, adding an integer to a numeric will promote the integer to a numeric, and
> result in a numeric, even though adding two integers would result in an integer.
> ~~~
> > class(3L + 3L)
> [1] "integer"
> > class(3L + 3)
> [1] "numeric"
> ~~~
> {: R}
>
> A user can often force explicit conversion between different classes of data using the `as.*()`
> functions. For example, we were able to convert a numeric value
> to an integer using `as.integer()`. R will try to convert the class accordingly,
> but where it cannot do so, will return you **NA** with a warning message. For example:
>
> ~~~
>  > as.integer("TRUE")
> [1] NA
> Warning message:
> NAs introduced by coercion 
> ~~~
> {: R}
> 
> This conversion cannot be done because TRUE cannot be represented as a number. Instead,
> R will convert it to NA.
{: .callout}

## Errors and warnings in R

Two types of messages will be encountered in R: error messages and warning messages.
Although both occur when there is something wrong, they have slightly different behaviors.
Errors will terminate the runnin code (that is, the code will not be evaluated at all), while
warnings will not. Hence, it is important to always check for the output of scripts that
were completed with warnings, as the script might not have done what you had intended for
it in the first place.


## Working with vectors

### Creating vectors 

Previously, only single values were assigned to a variable. A single value is a vector
with a length of one. Often, it is useful to create vectors with more than one element. If
we tried to just separate entries with a comma, we will get an error message. Instead, we
use the `c()` function to combine values into a vector:

~~~
demoVector <- c(1,2,3,4,5)
~~~
{: .R}

## Lists

Ordinary vectors in R can only contain simple data types, including the data types shown
above, and the elements of an ordinary vector must all be of the same basic data type.
*Lists* relax both these restrictions: a list can contain elements of any data type, even
complex data types such as other lists, and the elements of a list can be of different
data types. This allows for important and powerful abstractions.

Suppose we have three character vectors, each containing the names of different types of organisms.

~~~
birds <- c("myna","sparrow","swift")
insects <- c("cockroach","butterflies","caterpillar")
mammals <- c("hamster","rats","humans") 

# Combine these into a list.
animals <- list(birds, insects, mammals)
~~~
{: .R}

The list `animals` will look like this:

~~~
> animals
[[1]]
[1] "myna"    "sparrow" "swift"

[[2]]
[1] "cockroach"   "butterflies" "caterpillar"

[[3]]
[1] "hamster" "rats"    "humans" 
~~~
{: .output} 

We can name each vector in the list as follows: 

~~~
names(animals) <- c("birds", "insects", "mammals")
~~~

When we look at the list again, the output becomes the follows: 

~~~
> animals
$birds
[1] "myna"    "sparrow" "swift"

$insects
[1] "cockroach"   "butterflies" "caterpillar"

$mammals
[1] "hamster" "rats"    "humans" 
~~~
{: .R}

The addition of names to the elements of the list facilitates subsetting, which will be discussed later.
 
## Matrix

A matrix is a two-dimensional vector that has rows and columns. Like vectors, all the
entries in a matrix must be the same basic data type -- that is, either numeric, integer,
logical or character. If there are more than one data type, R will simply convert
everything to a compatible data type. A matrix can be created with the `matrix()` function
which has the general usage as shown:

~~~
Usage:

     matrix(data = NA, nrow = 1, ncol = 1, byrow = FALSE,
            dimnames = NULL)

~~~
{: output}
 
The `nrow` and `ncol` argument specifies the number of rows and columns respectively, while the `byrow` argument indicates if the matrix will be filled row-wise or column wise. The behavior of the byrow argument can be seen below: 

~~~
> matrix(1:20, nrow=2, ncol=10, byrow=TRUE)
     [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10]
[1,]    1    2    3    4    5    6    7    8    9    10
[2,]   11   12   13   14   15   16   17   18   19    20

> matrix(1:20, nrow=2, ncol=10, byrow=FALSE)
     [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10]
[1,]    1    3    5    7    9   11   13   15   17    19
[2,]    2    4    6    8   10   12   14   16   18    20
~~~
{: R}

## Data frame 

Similar to a matrix, a data frame has both row and columns. However, it is more general
than a matrix, and allows for different columns to have different types of data. This
makes the data frame one of the most versatile data types in R, and one of the most
widely-used data types as well. One can create a data frame using the `data.frame()`
function, which has the usage

~~~
Usage:

     data.frame(..., row.names = NULL, check.rows = FALSE,
                check.names = TRUE, fix.empty.names = TRUE,
                stringsAsFactors = default.stringsAsFactors())

~~~
{: output} 

For example, the following can be used to construct a data frame where we explicitly gave row names and column names to both rows and columns. 

~~~
> data.frame(Gender=c("male","female"),
		       Count=c(10,5), 
		       row.names=c("M","F"))
  Gender Count
M   male    10
F female     5
~~~ 
{: output}

## Subsetting in R 
Subsetting is an important capability of any programming language. That is, how can we extract specific entries from a vector, list, matrix or dataframe? 

In R, subsetting can be done using `[n]`, where *n* is the *n*-th entry that one wishes to extract. It is also worth noting that in R, the first entry starts from 1 and not zero like in other languages (such as *C*). For example, 

~~~
> demoVector <- 1:10
> demoVector
 [1]  1  2  3  4  5  6  7  8  9 10
> demoVector[5]
[1] 5
~~~ 
{: R}


For 2 dimensional objects such as data frames and matrix, one will use `[m,n]` to extract the entry in the *m*-th row and *n*-th coumn of the array. 

For example, consider the following data frame:

~~~
> demoData
    A  B  C
1   1 11 21
2   2 12 22
3   3 13 23
4   4 14 24
5   5 15 25
6   6 16 26
7   7 17 27
8   8 18 28
9   9 19 29
10 10 20 30
~~~
{: output} 

If we want to extract the entry in the first row and first column, this is done by using the following: 

~~~
> demoData[1,1]
[1] 1
~~~

To subset the entire row, we simply leave the column index empty, as such:

~~~
> demoData[1,]
  A  B  C
1 1 11 21
~~~
{: R}

The same principle applies also if one wishes to subset the entire column, but instead of subsetting using the row index (as we did previously), one will subset using the column index. 

Other than subsetting data frames using the column index, one can also subset using the column name. For example, we can select the first column of *demoData* using the following as well. *There, however, is no way to select using row names.*

~~~
demoData$A
~~~
{: .R} 

Although subsetting lists is similar to subsetting vectors, there are slight differences. Consider the following: 

~~~
> animals
$birds
[1] "myna"    "sparrow" "swift"  

$insects
[1] "cockroach"   "butterflies" "caterpillar"

$mammals
[1] "hamster" "rats"    "humans" 

~~~
{: .output}

We retrieve a list slice with the single square bracket "[]" operator, but use the "[[]]"" operator to refer to a member of the list directly. Consider the following examples: 

~~~
> animals[2]
$insects
[1] "cockroach"   "butterflies" "caterpillar"

> animals[[2]]
[1] "cockroach"   "butterflies" "caterpillar"
~~~
{: .R}

In order to modify a member of the list, we use both "[[]]" and "[]" together. For example, to change the entry *butterflies* in *insects* (2nd entry of the 2nd member), we do the following:

~~~
animals[[2]][2] <- "butterfly"
~~~
{: .R}

The result is as follows: 

~~~
> animals
$birds
[1] "myna"    "sparrow" "swift"  

$insects
[1] "cockroach"   "butterfly"   "caterpillar"

$mammals
[1] "hamster" "rats"    "humans" 
~~~
{: .output}
