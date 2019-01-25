---
title: "Additional resources for R programming"
teaching: 5 
exercises: 0
questions: 
- "Where can I find help?" 
- "How can I find out more about R programming?"
- "What are some useful resources for a beginner to R?"
objectives: 
- "Be aware of the wealth of resources available for seeking help in R." 
keypoints:
- "There is a wealth of resources to seek help in R, in large part facilitated by the active R user community."
---
## Getting help in R

### Documentations
R functions are required to be documented. The documentation of a function can be accessed using the following: 

~~~
help(function)
~~~
{: R}
This will give us the following

~~~
mean                   package:base                    R Documentation
Arithmetic Mean
Description:
     Generic function for the (trimmed) arithmetic mean.
Usage:

     mean(x, ...)
     
     ## Default S3 method:
     mean(x, trim = 0, na.rm = FALSE, ...)     
Arguments:
       x: An R object.  Currently there are methods for numeric/logical
          vectors and date, date-time and time interval objects.
          Complex vectors are allowed for ‘trim = 0’, only.

    trim: the fraction (0 to 0.5) of observations to be trimmed from
          each end of ‘x’ before the mean is computed.  Values of trim
          outside that range are taken as the nearest endpoint.

   na.rm: a logical value indicating whether ‘NA’ values should be
          stripped before the computation proceeds.
     ...: further arguments passed to or from other methods.
Value:
     If ‘trim’ is zero (the default), the arithmetic mean of the values
     in ‘x’ is computed, as a numeric or complex vector of length one.
     If ‘x’ is not logical (coerced to numeric), numeric (including
     integer) or complex, ‘NA_real_’ is returned, with a warning.
     If ‘trim’ is non-zero, a symmetrically trimmed mean is computed
     with a fraction of ‘trim’ observations deleted from each end
~~~
{: output}
These documentations are usually very lengthy and detailed, and can sometimes be technical to read and understand. However, the value of these documentations lies precisely in its details: these documentations show all arguments that a function can take, including default values. 

### Forums 
There are various forums that contain a wealth of information about R. In fact, it is safe to say that most, if not all, questions that one comes across in R will likely have been asked before in one or more of these forums (unless you are reporting a new bug). Two of the most active forums are StackOverflow and the R-mailing list. More often than not, a quick Google search will usually turn up some results to our queries in one or both of these forums. 

## More resources for R programming

### Online courses 
I found one particular online course offered by DataCamp to be very accessible for people who want to be more familiar with the basics of R. The course can be found at the URL: https://www.datacamp.com/getting-started?step=2&track=r. One additional advantage of the course is that it tailors to people of different levels/competencies.

### Online tutorials
For those who are more inclined to reading, the following sites serve as good tutorials for introducing R:
1. R-tutor, accessible at  http://www.r-tutor.com/r-introduction
2. Quick-R, accessible at http://www.statmethods.net/
3. The official R manual, accessible at https://cran.r-project.org/doc/manuals/r-release/R-intro.pdf

The last of the three is incredibly lengthy(running some 100 pages), but extremely detailed and makes for good reading. 

### R-bloggers
This is a blog site dedicated to discussing R. Because it is assumed that most people viewing the site are already proficient in R, it is not necessarily the best place to seek help. However, there are many informative articles discussing various aspects of R programming, often complete with code snippets for one who is interested in trying things out. For that reason, I am including this resource in the list of resources for R programming.

{% include links.md %}
