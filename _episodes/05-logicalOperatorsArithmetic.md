---
title: Logical and arithmetic operations in R
teaching: 5
exercies: 5
questions: 
   - How do we do mathematical operations in R?
   - How can we use R to check for different conditions?
   - How do we determine if some elements are found in our data?

objectives:
   - Be able to perform basic arithmetic in R using standard symbols.
   - Be aware of different logical tests in R.
   - Perform value matching using %in% and grep, knowing the difference between the two operations.

keypoints:
   - Arithmetic operations are performed element wise in R.
   - "`%in%` is used to find exact matches, while `grep` is used for inexact matches."
   - "`which` is used to return the index of matches found using %in%."
---

## Arithmetic in R

Basic arithmetic operations on numeric data use standard calculator symbols such as `+`,
`-`, `*`, and `/` for addition, subtraction, multiplication and division
respectively. Another operation that might be of interest is the *modulus*
operator `%%`, which allows us to calculate the remainder of a division. For
instance:

~~~
> 5 %% 2
[1] 1
~~~
{: .R} 

In order to raise a numeric value to a power, we simply use `^`. For example, 

~~~
> 3^2
[1] 9
~~~
{: .R}
 
More information on arithmetic can be found at
https://stat.ethz.ch/R-manual/R-devel/library/base/html/Arithmetic.html.
 
What happens if we are operating on a vector of numbers, rathre than a single number?
Consider the following vector:

~~~
> a <- c(2,2,5,5,6)
~~~
{: .R}

If we want to find the remainder of each term after dividing by 2, we can do the following: 

~~~
> a %% 2
[1] 0 0 1 1 0
~~~
{: .R}

Operations in R are performed element wise. This is shown in the following: 

~~~
> a <- c(2,2,5,5,6)
> b <- c(2,2,2,3,2)
> a %% b
[1] 0 0 1 2 0
~~~
{: .R}

The element wise nature of operation in R has some implications on its behavior. Consider the following: 

~~~
> a %% c(2,3)
~~~
{: .R}

The output we will get is the following

~~~
[1] 0 2 1 2 0
Warning message:
In a %% c(2, 3) :
  longer object length is not a multiple of shorter object length
~~~
{: .output}

Noticeably, R returns a cryptic warning message. What was done behind the hood was this: 

~~~
> a %% c(2,3,2,3,2)
~~~
{: .R}

In effect, what R did was to repeat the shorter object until the two objects are
of the same length and then do the computations element-wise. This is referred
to as "element recycling", and can be useful, but as you can see can also be a bit
counter-intuitive. The error message was simply telling us that R was not able
to repeat the entire shorter vector `n` times, and hence had to truncate some
entries in the shorter vector when doing the operation.

> ## Arithmetic operations on two dimensional objects
>
> What happens when we try to divide a 2 dimensional object such as a matrix by:
>
> 1. An object of equal dimension;
> 2. A vector of length one (that is, a single number);
> 3. An object of different dimension? 
{: .challenge}

One commonly used arithmetic operation in bioinformatics is the log operation.
This can be done using the `log()` function.

>## Changing the base of a logarithm in R
>
> What is the default base used for log in R? How could you take a logarithm
> using a different base?
> 
> *Hint: Use help(log) for more information on the log function.* 
{: .challenge}

## Logical operators in R

Logical operations are used to determine if a condition is TRUE or FALSE. As
such, logical operations return logical values only. The common logical
operations are as follows:

|Symbol|Interpretation       |
|----------|-----------------|
|>         | greater than  	 |
|>=        |greater than or equal to|
|<         | less than       |
|<=        |less than or equal to|
|==        |equal to        |
|!=        |   Not equal to |
|!         | not  (this is a negation operator an inverts the logical value of whatever follows) |


All but the last are *infix* operators: they are placed between the variables or
values on which they operate, e.g, `x + y`.  The negation operator is a *prefix*
operator; it negates the logical value of whatever immediately follows.

As shown above, most of the logical operators are similar to what we use in
everyday mathematics. Also as in ordinary mathematics, parentheses can be used
to enforce order of evaluation. The major notational difference is the use of
`==` to test for equality instead of `=`. This is common in computer languages
because `=` is often used in computer languages as an assignment operator. (R
can use `=` for variable assignment, but `<-` is preferred by most R style
guides.)




## Value matching in R

Suppose we are interested in whether a vector contains certain values. There are
several ways to determine this. The first is the `match()` function, which
searches for exact matches of elements. For example:

~~~
a <- c(1,2,3,4,5,6,7,10)
a %in% c(3,4)
[1] FALSE FALSE  TRUE  TRUE FALSE FALSE FALSE FALSE
~~~
{: .R}

The above snippet can be read as "Do the elements in vector *a* match our
desired values (that is, 3 and 4)?". The result is a logical vector showing
which are `TRUE` (that is, which entries match the values of interest). We can
directly perform subsetting to keep only the entries that matches the values of
interest:

~~~
> a[a %in% c(10)]
[1] 10
~~~
{: .output}

On the other hand, to get only the indexes of the match, we will need to combine `%in%` with another function, `which()`, as such: 

~~~
> which(a %in% c(7,10))
[1] 7 8
~~~
{: .output}

meaning the values `7` and `10` can be found in the 7th and 8th elements of `a`.

We can combine the logical prefix operator `!` with `which()` to find out which entries **do not match** our query. For example: 

~~~
> which(!a %in% c(7,10))
[1] 1 2 3 4 5 6
~~~
{: .output}

> ## Try it
> 
> How do we find out the indexes of the entries in *a* that are not 3 or 4?
>
> *Hint: use the `!` operator*
{: .challenge}

> ## Try it
>
> Let `x` be a vector of numbers from 1:100.
> How do we find all the even numbers in `x`?
>
> *Hint: Use the `%%` operator to identify the even numbers*
{: .challenge}

## Inexact matches

The examples above all find exact matches, but `grep()` can be used to find inexact matches. For example: 

~~~
> a <- c("TP63","TP53", "CDK1", "Ras", "pTP53")
> grep("TP", a)
[1] 1 2 5
~~~ 
{: .R}

In this example, we first created a vector of gene names. The `grep()`
function can be understood as: which entries in vector `a` contain the pattern
"TP"?. `grep` will return a numeric vector containing the indexes of the
matches, which is immensely useful for subsetting (remember that subsetting of
vectors require us to only provide the index corresponding to the entries of
interest).

## A note on regular expressions

`grep()` is in fact searching using **regular expressions**. **Regular
expressions** (*regex* in short) are sequences of characters that define a
formal search pattern. Regular expressions provide powerful pattern matching
capabilities in many computer languages, so it it useful to learn about the
patterns. In this case, the ordinary letters "TP" search for the exact
substring, but other patterns are possible.  For example, `grep("^TP", a)`
anchors the pattern to the *beginning* of each string, and returns only `1 2`.
The fifth entry (`pTP53`) does not match because the "TP" pattern is not at the
beginning of the string.

A version of `grep` is found in bash. For those interested, it is worth
learning more about the different regular expressions that can be used with
`grep`. More can be found at [This link](http://www.memoryhacking.com/Misc/Tut/Wildcards%20and%20Regular%20Expressions.htm)
