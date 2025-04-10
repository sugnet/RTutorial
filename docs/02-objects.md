# Managing objects {#objects}

After completing the introductory chapter you now know how to

* initialize an R session; 
* save your workspace;
* open an existing project;
*	execute simple tasks in R to obtain numerical, text or graphical results;
*	obtain help.

You know also that everything in R can be considered as some kind of an object. In this chapter the focus is on what properties the different objects have and how to manage objects in the workspace. 

##	Instructions and objects in R

###	General

Recall that

*	instructions are separated by a semi-colon or start on new lines;
*	the `#` symbol marks the rest of the line as comments;
*	the default R (primary) prompt is `>`; the secondary default prompt is `+`;
*	use of  `<-` to create objects.  (The equality sign (`=`) will also be accepted. However, avoid this practice and use

    + `=` only for function arguments;
    + `<-` for assignment;
    + `==` 	for comparison / control structures);

*	the use of  `->`  for assigning left-hand side to the name on right-hand side.
* the use of function `assign()` for assigning names to objects. (to be discussed in detail in Chapter \@ref(operators))

##### Examples {-}


``` r
aa <- 1:10
```

Assigning numeric vector to name "aa".  Assignment takes place in global environment.


``` r
Aa <- seq(from = 1,to = 10,by = 0.01); yy <- c("a","b","c")
c("a","b","c") -> bb 
```

Assigning character vector to name "bb".  


``` r
assign("aa", rnorm(10), pos = 1)
```

Note the use of the argument `pos`, " " or ' ' are used for characters. Be careful when mixing single quotes and double quotes. See below.


``` r
c("u",'v',"'w'",""x"",'"y"',''z'') -> cc
#> Error: <text>:1:19: unexpected symbol
#> 1: c("u",'v',"'w'",""x
#>                       ^
```

``` r
c("u",'v',"'w'",'"x"','"y"',''z'') -> cc
#> Error: <text>:1:31: unexpected symbol
#> 1: c("u",'v',"'w'",'"x"','"y"',''z
#>                                   ^
```

``` r
c("u",'v',"'w'",'"x"','"y"','z') -> cc 
cc
#> [1] "u"     "v"     "'w'"   "\"x\"" "\"y\"" "z"
```

*	Explain error message above.
*	Explain backslash above.


``` r
objects()
#> [1] "aa" "Aa" "bb" "cc" "yy"
```

``` r
aa
#>  [1] -0.5501535 -0.6306530 -0.6083626  1.3631381  1.7990231
#>  [6] -0.3212747  0.9019752 -2.4216251  0.9643477  0.4887088
```

``` r
bb
#> [1] "a" "b" "c"
```

``` r
objects()[3]
#> [1] "bb"
```

``` r
parse(text=objects()[3])
#> expression(bb)
```

``` r
eval(parse(text=objects()[3]))
#> [1] "a" "b" "c"
```

``` r
rm(a,b)
#> Warning in rm(a, b): object 'a' not found
#> Warning in rm(a, b): object 'b' not found
```

``` r
rm(aa,bb)
objects()
#> [1] "Aa" "cc" "yy"
```

``` r
rm("cc")
objects()
#> [1] "Aa" "yy"
```

###	Objects in R

(a)	Everything is an object but there are many different types of objects.

(b)	Study and also take note of the following *<span style="color:#FF9966">naming conventions</span>*:

*	Allowed are upper or lower case letters, numbers 0 – 9, full stop(s) and underscore(s).
*	Must not begin with a number.
*	R is case sensitive i.e. `John`  and `john` refer to different objects.
*	Use full stops (periods) or underscores to break up a name into meaningful words.
*	Avoid `c`, `s`, `t`, `C`, `F`,  `T`,  `diff`  as well as other reserved words for naming an object.  

(c)	The use of the functions `conflicts()` and `find()` when naming objects. The instruction `conflicts (detail = TRUE)` outputs details on whether and where objects with identical names exist on the search path e.g.


``` r
conflicts(detail=TRUE)
#> $`package:graphics`
#> [1] "plot"
#> 
#> $`package:methods`
#> [1] "body<-"    "kronecker"
#> 
#> $`package:base`
#> [1] "body<-"    "kronecker" "plot"
```

The instruction `find ("object")` outputs details on whether and where objects with the name object exist on the search path e.g.


``` r
find("kronecker")
#> [1] "package:methods" "package:base"
```

(d)	Objects can possess several attributes e.g.  

*	mode    (The way an object is internally stored)
*	length
*	names
*	dim
*	class

#### Examples {-}


``` r
a <- 1:10
class(a)
#> [1] "integer"
```

``` r
b <- factor(c("a","b","c"))
class(b)
#> [1] "factor"
```

``` r
b
#> [1] a b c
#> Levels: a b c
```

``` r
mode(a)
#> [1] "numeric"
```

``` r
mode(b)
#> [1] "numeric"
```

``` r
length(a)
#> [1] 10
```

``` r
length(b)
#> [1] 3
```

``` r
dim(a)
#> NULL
```

``` r
mat <- matrix(1:12,nrow=4)
mat
#>      [,1] [,2] [,3]
#> [1,]    1    5    9
#> [2,]    2    6   10
#> [3,]    3    7   11
#> [4,]    4    8   12
```

``` r
dim(mat)
#> [1] 4 3
```

``` r
mode(mat)
#> [1] "numeric"
```

``` r
logic <- c(TRUE,TRUE,FALSE,TRUE)
mode(logic)
#> [1] "logical"
```

``` r
class(logic)
#> [1] "logical"
```

<div style="margin-left: 30px; margin-right: 20px;">
Levels show that it is a categorical variable (object).

Mode `numeric` tells us that the categorical variable (object) `b` is internally stored as a set of numeric codes.
</div>

(e)	Special attention is given to the class and mode of integers. An object of type integer is stored internally more effectively than an integer represented in double format.


``` r
x <- 5
y <- 5L
typeof(x)
#> [1] "double"
```

``` r
typeof(y)
#> [1] "integer"
```

``` r
class(x)
#> [1] "numeric"
```

``` r
class(y)
#> [1] "integer"
```

``` r
mode(x)
#> [1] "numeric"
```

``` r
mode(y)
#> [1] "numeric"
```

(f)	Objects in R are *<span style="color:#FF9966">vectors</span>*,  *<span style="color:#FF9966">functions</span>* or *<span style="color:#FF9966">lists</span>*.  There are no scalars - instead vectors of length one are used. In addition to the above three types, there are several other types of objects.

(g)	Objects that are created during a session are permanently stored in the <span style="color:#CC99FF">.RData</span> file in the folder containing the workspace (unless not saved at termination).

(h)	Objects that are created within a function exist only for as long as the function is being executed. 

(i)	Use of `rm()` and `rm(list = ListOfNames)` to remove objects from the workspace.

(j)	Use of `objects()` or equivalently `ls()` to obtain a list of object names in a data base (by default the workspace). Note the optional arguments `pos`, `all.names` and `pattern` to specify which database to be considered and what object names to include. 

(k)	How can an object be printed to the screen?

(l)	*<span style="color:#FF9966">Warning:</span>* If a new object is assigned to a name that already exists in the working directory the old object is overwritten without warning and it cannot be retrieved again.

### Data in R

(a)	R has several built-in data sets. Use `?datasets` and/or `library(help= "datasets")` for details.  Note that the two instructions return different information.

(b)	Study the help file of `c()`.

(c)	Study the help file of `scan()`.

(d)	Study the help files of `read.table()` and `read.csv()`. Care must be taken with data containing characters (text) and categorical variables. Reading data into R will be discussed in detail in Chapter \@ref(data).

###	Generation of data

Study the operators and functions `:`,  `seq()`, `rep()`, `rev()`, `rnorm()`, `runif()` with the following instructions:


``` r
1:10
8:3
seq(from=1, to=10, length=10)
seq(from=2, to=10, length=5)
rev(10:1)
rnorm (20, mean=50, sd=5)
runif (10, min=1, max=3)
```

The function `rmvnorm()` for generating multivariate normal samples is in the `mvtnorm` R package.  This package must first be loaded by using the instruction


``` r
library(mvtnorm)
```

Alternatively, for generating multivariate normally data there is also a function `mvrnorm()` in R package `MASS`.

## R Function Basics

We introduced R functions in section \@ref(FunctionIntro). The basic structure of an R function is as follows:


``` r
func.name <- function(list of arguments)
{
  # R code
}
```

When the function `func.name()` is called, the code in `{ }` is executed. Let us have another look at our confidence interval function, with a small change in the list of arguments:


``` r
conf.int <- function (x, alpha = 0.95)
{
  x.mean <- mean(x)   
  x.sd <- sd(x)   
  tail.prob <- 1 - alpha
  t.perc <- qt(1 - tail.prob/2,19) 
  print (1 - tail.prob/2)
  left.boundary <- x.mean - (x.sd/sqrt(length(x)))*t.perc 
  right.boundary <- x.mean + (x.sd/sqrt(length(x)))*t.perc
  list (lower = left.boundary, upper = right.boundary)  
}
```

The argument `x` does not have a default value. The function cannot be executed without specifying a vector of values for `x`. The argument `alpha` has a default value of $0.95$ which means by default a $95\%$ confidence interval will be created. It is however up to the user to specify an alternative confidence level, should they wish.


``` r
conf.int()
#> Error in conf.int(): argument "x" is missing, with no default
```

``` r
conf.int(x = sleep[,1])
#> [1] 0.975
#> $lower
#> [1] 0.5955845
#> 
#> $upper
#> [1] 2.484416
```

``` r
conf.int(x = iris[1:50,1], alpha = 0.9)
#> [1] 0.95
#> $lower
#> [1] 4.919803
#> 
#> $upper
#> [1] 5.092197
```
