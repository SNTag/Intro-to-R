---
title: "Automating tasks in R using loops"
teaching: 10
exercises: 15
questions:
- "How do we automate tasks in R?"
objectives: 
- "Be able to write a basic loop for performing repetitive computations."
keypoints:
- "Control structures allow us to automate tasks in R, as long as certain conditions are met."
- "If-else differs from for and while loops in that the block of code evaluated depends on the result of the logical test."
- "The apply family of functions are a powerful way of replacing loops in R."  
---
# Control structures 
Control structures is one of the most fundamental and important pieces of any programming language. It allows us to automate tasks that have to be done repetitively, as long as a given condition is not met. In R -- just like in other programming languages, there are three key control structures, namely:
1. *for* loops;
2. *while* loops;
3. *if-else* statements. 
We will discuss how each of the following is used in R. However, the principles behind these three control structures are shared across most, if not all, programming languages and hence understanding how they work is imperative.  

## *for* loops 
In programming, there is a principle called DRY -- also known as "Don't Repeat Yourself". Consider the following example where one needs to print the statement "The year is [year]" where year is between 2010 and 2015. If we had to do it manually, we will end up doing the following: 

~~~
(Taken from https://www.r-bloggers.com/how-to-write-the-first-for-loop-in-r/)
print(paste("The year is", 2010))
"The year is 2010"
print(paste("The year is", 2011))
"The year is 2011"
print(paste("The year is", 2012))
"The year is 2012"
print(paste("The year is", 2013))
"The year is 2013"
print(paste("The year is", 2014))
"The year is 2014"
print(paste("The year is", 2015))
"The year is 2015"
~~~
{: .R}

As is evident, the above is tedious as we need to type the same thing over and over again while only varying the year. This is in clear violation of the DRY principle. The correct way to do it is to use a *for* loop, as such:

~~~
for (year in c(2010,2011,2012,2013,2014,2015)){
  print(paste("The year is", year))
}
~~~
{: .R} 

After executing this code, we will get the following: 

~~~
"The year is 2010"
"The year is 2011"
"The year is 2012"
"The year is 2013"
"The year is 2014"
"The year is 2015"
~~~
{: .output}

How do we understand what the *for* loop is doing? One way to do it is to read it as follows: 

~~~
for every entry in the vector (here, the vector contains the individual years), the code is executed and the variable (in this case, year) is substituted in the code block (that is, year in the paste gets substituted with the present value of year). Once the evaluation is completed for every entry in the vector, the code stops and exits.  
~~~

The general skeleton of a *for* loop is as such:

~~~
for (i in <vector>){
	<code chunk here>
}
~~~
{: .R}

>## Writing your first *for* loop
>
> Using a *for* loop, print the first 10 natural numbers (1 to 10). 
{: .challenge}


>## Variables in *for* loops 
>
> In our example, we used the variable *year*. Any variables can be used, with the most common being *i*. The variable in the *for* loop serves as a counter to keep track of which entry is being evaluated. 


## *while* loops
*While* loops evaluate for as long as the logical test returns a TRUE. This is unlike a *for* loop which only iterates along a vector. 

>## Infinite loops
>
> This occurs when the condition is never met, and the task just keeps repeating forever till the evaluation is killed. Consider the following example: 

~~~
y <- 1
x <- 5
while (y <= 5){
	x <- x + 1
	print(x)
}
~~~
{: .R}
>The above code will loop infinitely because *y* will forever be 1. This is because *y* is never updated in the *while* loop. On the other hand, *x* will increase till the evaluation gets killed manually because its value is being increased by 1 at each successive iteration. It is a common mistake to make to not increase the value of *y* (which serves as the counter) hence leading to an infinite loop. Instead, the correct code will be as follows:

~~~
y <- 1
x <- 5
while (y <= 5){
	x <- x + 1
	print(x)
	y <- y + 1
}
~~~
{: .R}

> The addition of the last line `y <- y + 1` updates *y* at the conclusion of each iteration so that the conditional test *y <= 5* will eventually evaluate to FALSE and hence terminating the loop. 

> ## Writing your first *while* loop
>
> Write a short script that prints out all the numbers between 10 and 1000, both numbers inclusive? 
>
> *Hint: Initialize the variable with a value of 10*
>
>The print command will print out the value of a given variable.*
{: .challenge}

## *if-else* statements
Unlike the earlier two control structures which performs a task repetitively, an *if-else* statement usually only does one task depending on whether the logical test used as the condition is true.  For example:

~~~
x <- 5
> if(x < 6){print("FALSE")} else {print("TRUE")}
~~~
{: .R}


Evaluating the above if-else statement returns the following: 

~~~
[1] "FALSE"
~~~
{: .output}

The above demonstrates how the if-else statement works. First, the condition will be tested to see if it evaluates to TRUE or FALSE. If the condition is met (that is, the logical test evaluates to TRUE), the code in the *if* block is evaluate. Otherwise, the code in the *else* block gets evaluated. For example: 

~~~
x<-7
> if(x<6){print("FALSE")} else {print("TRUE")}
[1] "TRUE"
~~~
{: .R}

One handy way of writing the if-else block is to use the `ifelse()` function, which is easier to understand, looks neater and has the added advantage of being faster to evaluate than an if-else block. 
 
## The *apply* family of functions 
Although loops are powerful, they are slow to evaluate in R. The preferred approach for many tasks is to use a function from the *apply* family of functions. To find out what they are, simply type `??apply`. The following output results:

~~~
base::apply             Apply Functions Over Array Margins
  Aliases: apply
base::by                Apply a Function to a Data Frame Split by
                        Factors
base::eapply            Apply a Function Over Values in an Environment
  Aliases: eapply
base::lapply            Apply a Function over a List or Vector
  Aliases: lapply, sapply, vapply
base::mapply            Apply a Function to Multiple List or Vector
                        Arguments
  Aliases: mapply
base::rapply            Recursively Apply a Function to a List
  Aliases: rapply
base::.subset           Internal Objects in Package 'base'
  Aliases: .mapply
base::tapply            Apply a Function Over a Ragged Array
  Aliases: tapply
~~~
{: .output} 

Although there are numerous functions in the *apply* family of functions, there are two functions that are used very often. They are (1) apply and (2) lapply. For that reason, we will focus on their usage below. 

### *apply*
From the manual,

~~~
apply                   package:base                   R Documentation

Apply Functions Over Array Margins

Description:

     Returns a vector or array or list of values obtained by applying a
     function to margins of an array or matrix.

Usage:

     apply(X, MARGIN, FUN, ...)
~~~
{: .output}

From the usage, we know that `apply()` requires 3 components: a data frame or matrix *x*, a margin and a function to apply along the margins. So what are margins?

Simply, margins refer to rows and/or columns. For example: 

~~~
(Taken from https://nsaunders.wordpress.com/2010/08/20/a-brief-introduction-to-apply-in-r/)
# create a matrix of 10 rows x 2 columns
m <- matrix(c(1:10, 11:20), nrow = 10, ncol = 2)
# mean of the rows
apply(m, 1, mean)
 [1]  6  7  8  9 10 11 12 13 14 15
# mean of the columns
apply(m, 2, mean)
[1]  5.5 15.5
~~~
{: .R}
If we want to apply the function along the rows, then the margin is "1". Similarly, if we want to apply the function along the columns, then the margin will be two. We can apply a value along both rows and columns (that is, to every single element) by specifying the margin to be both "1" and "2" (that is, `c(1,2)`). 

### *lapply*
From the manual

~~~
lapply                  package:base                   R Documentation

Apply a Function over a List or Vector

Description:

     ‘lapply’ returns a list of the same length as ‘X’, each element of
     which is the result of applying ‘FUN’ to the corresponding element
     of ‘X’.
~~~
{: .output}

Because it is quite well explained, it is easy to see what `lapply()` is used for. While `apply()` is used for dataframes or matrices, `lapply()` is used for lists.  The following example will illustrate what `lapply()` does:

~~~
(Taken from https://nsaunders.wordpress.com/2010/08/20/a-brief-introduction-to-apply-in-r/)
# create a list with 2 elements
l <- list(a = 1:10, b = 11:20)
# the mean of the values in each element
lapply(l, mean)
$a
[1] 5.5
 
$b
[1] 15.5
 
# the sum of the values in each element
lapply(l, sum)
$a
[1] 55
 
$b
[1] 155
~~~
{: .R}