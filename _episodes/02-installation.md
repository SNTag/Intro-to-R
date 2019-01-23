---
title: R and RStudio installation
teaching: 5
exercises: 0
questions:
- How do I install R and RStudio?
- How do I install other packages in R?
objectives:
- Be able to install or update R and RStudio. 
- Be familiar with installing packages in R. 
keypoints: 
- R can be installed via the Linux package managers, or downloaded and installed
  from the web.
- RStudio is a powerful interface for R programming.
- Like Linux, R has a system for downloading and installing packages from the R
  package repositories.
- CRAN is the main repository for R packages.
- The `library()` function allows one to load an installed package in the current R
  session.
-  The `installed.packages()` function allows us to view what packages have been
  installed on our system, while `sessionInfo()` allows us to view what
  packages have been loaded in the current session of R.

---

## Installing R and RStudio

>
> ## R and RStudio have already been installed on the computers in the laboratory
>
{: .callout}

Like other Linux packages, R can be installed using a package manager. In Ubuntu, this can be
done using the following command:

~~~
sudo apt-get $HOME install r-base-core
~~~
{: .language-bash}

If you are using a MacOSX or Microsoft Windows system, you can download the latest binary
for your computer by following the links at the [R project home][Rproject]


Although R comes with a user interface and supports graphical output, the window of the
core interface on Linux contains only the R interpreter. We will use a more powerful and integrated
interface, *RStudio*. On top of being able to interpret R code, RStudio also contains
a code editor where one can write scripts prior to execution, a plotting area where one
can see the graphic output, and various tools for inspecting code and data. RStudio can be
installed by following the appropriate download link at https://www.rstudio.com/.

Once installed, you can start RStudio by clicking on the RStudio icon in the **Applications** list. 

> ## A note on versions
> It is important to note that sometimes, the version of R available on the repository is
> behind (that is, older than) the current version of R. In this case, one can simply
> update the list of repositories so that the package manager will download from the R
> site (which contains the latest version) instead of getting from *apt* only. This can be
> done by editing the file */etc/apt/sources.list*, a text file that contains the list of
> all repositories *apt* will search. The following line needs to be inserted into the
> sources list for an Ubuntu system:
> 
> ~~~
> > deb http://cran.rstudio.com/bin/linux/ubuntu xenial/
> ~~~
> {: .language-bash}
> 
> Once this is done, you will need to update *apt* by using the command, as well as allow
> `apt` to download from the new source. This is done using the following commands: 
>
>~~~
> gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9 
> gpg -a --export E084DAB9 | sudo apt-key add 
> sudo apt-get update
>~~~
> {: .language-bash}
> 
> Thereafter, the previous instructions to install R applies. 
{: .callout}

## R packages

The functionality in R is organised into packages. When you type a function like `foo()`,
R looks for the definition of the function through a search the *loaded packages*. (You can see the
loaded package search list with `search()`). If the package you want to use is not
installed on your computer, you first have to *install* it into your local package
library, and then have to load it from the library into your search path.


## Installing packages in R 

Installing packages in R, like in Linux, is done using a *package repository*. The main
repository for R packages is **CRAN** (The Comprehensive R Archive Network). However,
CRAN is by no means the only repository -- increasingly, people are also hosting packages
on *Github*, and there are other more specialized repositories as well. 

### Installing packages from CRAN 

Installing packages from CRAN is relatively straightforward, and can be achieved with a single function: 

```{r}
install.packages("package of interest")
```

It is important to realize the name of the package is (1) case sensitive, and (2) placed
within quotes. The reason for the latter is because if there is no quotes placed around
the name, R will assume that you are referring to a variable (next chapter), which might
not exist because you did not create it yet or because the value of the variable is an
invalid package.

## Loading packages 

In R, packages are loaded from the library into your search path using the `library()` function, as follows: 

```
library("package")
```
{: .language-R}


Importantly, packages are loaded only in the current session of R. If another instance of
R is started or the current session of R is restarted, these packages will no longer be
loaded and will need to be re-loaded again.

By default, packages are loaded into the *second* position on the search path, right after
`.GlobalEnv`. This means the most recently loaded package is the first package searched
after the current global environment. 


One can get a snapshot of the current session, including all currently loaded packages,
using `sessionInfo()`, which gives output like:

~~~
R version 3.3.2 (2016-10-31)
Platform: x86_64-pc-linux-gnu (64-bit)
Running under: Ubuntu 16.04.1 LTS

locale:
 [1] LC_CTYPE=en_SG.UTF-8       LC_NUMERIC=C              
 [3] LC_TIME=en_SG.UTF-8        LC_COLLATE=en_SG.UTF-8    
 [5] LC_MONETARY=en_SG.UTF-8    LC_MESSAGES=en_SG.UTF-8   
 [7] LC_PAPER=en_SG.UTF-8       LC_NAME=C                 
 [9] LC_ADDRESS=C               LC_TELEPHONE=C            
[11] LC_MEASUREMENT=en_SG.UTF-8 LC_IDENTIFICATION=C       

attached base packages:
[1] stats     graphics  grDevices utils     datasets  methods   base     

other attached packages:
[1] ggplot2_2.2.0

loaded via a namespace (and not attached):
[1] tools_3.3.2
~~~
{: output}

## Attached packages vs packages loaded via namespace

From the output above, you can see that I have loaded the ggplot2 package, which is a
package used extensively for data visualization. This information is found in the *other
attached packages* section. However, you will also notice that there are a whole bunch
of packages that are listed in the *loaded via a namespace (and not attached)* section.
These are packages that are suggested by ggplot2. Unlike the functions in ggplot2
however, I can't use the names of the functions in those packages without using the name
of the package that defines them. 


The `sessionInfo()` output not only tells us what package is loaded, but also
the version numbers of packages used in the session. This  information is really
useful when you run into problems and want to ask for help on the various
forums, as some of the problems reported might have been fixed in later versions
of the package, and is critical for being able to perform reproducable research.

[Rproject]: https://www.r-project.org/
