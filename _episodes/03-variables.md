---
title: Variables and assignments in R
teaching: 5
exercises: 0
questions:
- What are variables and why use them?
- How do we assign values to variables?
objectives:
- Understand what variables are and how values are assigned to variables.
- Differentiate global and local variables, and understand how to assign a variable within a function as a global variable. 
- How do we read data into R, or export data from R?

keypoints:
- Variables are used to store values in a script.
- Local variables can only be referred to within the environment it was found in, while
  global environments can be referred to anywhere.
- Variable *values* can be updated by referencing itself and then assigning the new value to itself.

---


## Variables: What and why

The concept of variables is found in all programming languages. For example, the following
example shows a variable that is assigned the value of 5 (`<-` is the assignment operator)

~~~
x <- 5
print(x) 
~~~
{: R} 

~~~
[1] 5
~~~
{: output}

Thereafter, we can  use *x* instead of 5 throughout our script. 


> Variables are used to store information to be referenced and manipulated in a computer
> program. They also provide a way of labeling data with a descriptive name, so our
> programs can be understood more clearly by the reader and ourselves. It is helpful to
> think of variables as containers that hold information. Their sole purpose is to label
> and store data in memory. This data can then be used throughout your program. (Taken
> from https://launchschool.com/books/ruby/read/variables)

Variables are used because they can be manipulated easily throughout our program. For example: 

~~~
## finding the average of 10 numbers
x <- c(3,9,5,1,4,7,2,5,3,5)
sum(x)/length(x)
~~~
{: R}

~~~
4.4
~~~
{: output}


There are two huge advantages in using variables: 

1. Not having to 'hard-code' values in our scripts. This is especially important when a
   single variable is referenced more than once in the script -- in that case, we only
   need to change the initial value assigned to the variable instead of changing the value
   everywhere in the script.
2. Legibility. With good variable names, the script becomes more legible as it is apparent
   to other readers what is being done/evaluated at each step of the script.

>## Variable names in R
>
> There are a few rules that needs to be observed when naming variables, namely:
>
>1. Variable names may not start with a number.
>2. Variable names cannot contain mathematical symbols such as "+", "-","*", "/".
>3. Variable names are case-sensitive.
>4. Variable names that start with "." are hidden in the global environment (more on environments later)

## Assigning values to variables

Assignment can be done using either `=` or `<-`. For example: 

~~~
x = 5
x <- 5
~~~
{: R}

In both cases, the variable `x` will be assigned the value of 5. Stylistically, we use
`<-` in R for assignment so as to avoid confusion with `==`, which is used to test for the
equality of 2 objects.

Variables hold values, but the values can be anything supported by R. For example, the
following variable `average` is a function that calculates the average for a vector of
numbers (more on that in the next section).

~~~
numbers  <- c(10, 20, 30, 40, 50, 60, 70, 80)
average <- function(x){
	return(sum(x)/length(x))
}
result <- average(numbers)
result
~~~
{: R}

Executing the above script will yield the following: 

~~~
[1] 45
~~~
{: R}

In the example above, we:

1. Created a variable *numbers*, which was a vector of numbers
2. Created a variable *average*, which is a function that calculates the average of a vector of numbers
3. Assigned the result of passing *numbers* through our function *average* to a new variable *result*

## Variable type and scope

The value of any variable has a *data type*, and the variable itself has a *scope*. Data
types are covered in the next section.

The *scope* of a variable determines when or if a variable has a particular value.
*Global* variables are available throughout the execution of a progam. If you declare a
global variable `foo` and refer to it later, or in some function, or in some package, the 
value of that variable will be found. *Local* variables, on the other hand, are released
or unavailable to parts of the program. Local variables, therefore, allow isolation of
different parts of a program from each other.

To understand what is happening, consider the code below:

```r
> test <- function() {
	localVar <- 10
	return(localVar)
}

> test()
[1] 10

>localVar
Error: object 'localVar' not found
```

The above example gives us some insights into the differences between local and global
variables. In the first line, we defined a new variable `test` that happens to be a
function. The assignment operator `<-` always assigns the variable within the current
*environment*, which is in this cae the global environment. Any variable found in the
global environment can be accessed from anywhere (even within functions). However, within
the function `test()` we defined a new variable `localVar`. This variable is defined
only within the local environment, which in this case, is the environment 
of the `test()` function, so `localVar` has no meaning outside of `test()`.

If we want to make a variable defined within a function accessibly globally, we can (but
probably should not) do the following:

~~~
> test <- function(){
	localVar <<- 10
	return (localVar)
}

> test()
[1] 10

> localVar
[1] 10
~~~
{: R}

Notice that instead of using `<-`, we used `<<-`. The assignment operator `<<-` is used to
assign values to the global environment as opposed to `<-` which only assigns to variables
in the current environment.  *Generally this is a bad idea to modify another environment*.

## Variables can be updated 

What happens in the following code snippet? 

~~~
x <- 5
x <- x + 3
~~~
{: R}
n
Will `x` be 5, or 8? Trying it at the R prompt shows the following

~~~
> x
[1] 8
~~~
{: output}

Initially, we had assigned `x` with a value of 5. However, in the second line, we then
re-assigned `x` to be `x + 3`, which is interpreted to be `5 + 3` since `x` was assigned the
value of 5.

In other words, `x` was updated because we had re-assigned the result to itself. This is
an important concept, and is useful especially when we use control structures (loops,
later).

Of course, if we want to preserve the value of `x` throughout, all we need to do is to
assign the output of any operations referring to `x` as another variable. For example:

~~~
x <- 5
y <- x + 3 
~~~
{: R}

This way, we can constantly refer to `x` throughout our script and be sure the value was
unchanged since the initial assignment of value.

## Reading and writing data in R 

To read in tabular data such as a tab-separated data file, we will use `read.table()`. The
usage for `read.table()` is as follows:

~~~
Description:

     Reads a file in table format and creates a data frame from it,
     with cases corresponding to lines and variables to fields in the
     file.

Usage:

     read.table(file, header = FALSE, sep = "", quote = "\"'",
                dec = ".", numerals = c("allow.loss", "warn.loss", "no.loss"),
                row.names, col.names, as.is = !stringsAsFactors,
                na.strings = "NA", colClasses = NA, nrows = -1,
                skip = 0, check.names = TRUE, fill = !blank.lines.skip,
                strip.white = FALSE, blank.lines.skip = TRUE,
                comment.char = "#",
                allowEscapes = FALSE, flush = FALSE,
                stringsAsFactors = default.stringsAsFactors(),
                fileEncoding = "", encoding = "unknown", text, skipNul = FALSE)
~~~
{: .output}

For example, to read the file `iris.tsv` in the *data* folder, we will do the following: 

```r
iris <- read.table("data/iris.tsv")
```

Conversely, we can write data from R out using `write.table()` , as follows: 

~~~
write.table(iris, "data/iris.txt")
~~~
{: .R}

The snippet above will write a new file, *iris.txt* in the *data* folder.

In both `read.table()` and `write.table()`, the default path is the present working
directory. We can specify alternative directories by providing either the absolute or the
relative path of our desired directory.

> ## A note on working directories 
>
> The working directory is the present directory in which you are working. By default,
> when you start R, the working directory will be the directory in which you started R.
> You can find your current working directory by using the command `getwd()`. To change
> your working directory, use `setwd(dir)`. Both absolute and relative paths can be used
> in `setwd()`
> 
> ## A note on file separators
> 
> Two of the most commonly used separators for tabular data are TAB (*.txt*, *.tsv*)and
> COMMA (*.csv*). By default, R assumes that files you are reading in and files you are
> writing out are TAB separated. You can change this default behaviour using the `sep`
> argument in both `read.table` and `write.table` commands.
{: .callout}
