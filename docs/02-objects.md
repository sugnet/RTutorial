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
#> Error in parse(text = input): <text>:1:19: unexpected symbol
#> 1: c("u",'v',"'w'",""x
#>                       ^
```

``` r
c("u",'v',"'w'",'"x"','"y"',''z'') -> cc
#> Error in parse(text = input): <text>:1:31: unexpected symbol
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
aa
#>  [1]  0.65250220  0.60091175 -1.40751916 -1.39704169
#>  [5]  0.67446492 -1.62221791  0.03345993 -0.65813113
#>  [9]  0.38767442 -0.27054430
bb
#> [1] "a" "b" "c"
objects()[3]
#> [1] "bb"
parse(text=objects()[3])
#> expression(bb)
eval(parse(text=objects()[3]))
#> [1] "a" "b" "c"
rm(a,b)
#> Warning in rm(a, b): object 'a' not found
#> Warning in rm(a, b): object 'b' not found
rm(aa,bb)
objects()
#> [1] "Aa" "cc" "yy"
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
b <- factor(c("a","b","c"))
class(b)
#> [1] "factor"
b
#> [1] a b c
#> Levels: a b c
mode(a)
#> [1] "numeric"
mode(b)
#> [1] "numeric"
length(a)
#> [1] 10
length(b)
#> [1] 3
dim(a)
#> NULL
mat <- matrix(1:12,nrow=4)
mat
#>      [,1] [,2] [,3]
#> [1,]    1    5    9
#> [2,]    2    6   10
#> [3,]    3    7   11
#> [4,]    4    8   12
dim(mat)
#> [1] 4 3
mode(mat)
#> [1] "numeric"
logic <- c(TRUE,TRUE,FALSE,TRUE)
mode(logic)
#> [1] "logical"
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
typeof(y)
#> [1] "integer"
class(x)
#> [1] "numeric"
class(y)
#> [1] "integer"
mode(x)
#> [1] "numeric"
mode(y)
#> [1] "numeric"
```

(f)	Objects in R are *<span style="color:#FF9966">vectors</span>*,  *<span style="color:#FF9966">functions</span>* or *<span style="color:#FF9966">lists</span>*.  There are no scalars - instead vectors of length one are used. In addition to the above three types, there are several other types of objects.

(g)	Objects that are created during a session are permanently stored in the <span style="color:#FFB3B3">.RData</span> file in the folder containing the workspace (unless not saved at termination).

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

##	Introduction to functions in R

We introduced R functions in section \@ref(FunctionIntro). The basic structure of an R function is as follows:


``` r
func.name <- function(list of arguments)
{
  # R code
}
```

When the function `func.name()` is called, the code in `{ }` is executed.

The arguments of a function can be inspected by using the command


``` r
args(name of function)
```

The function `str(x)` provides information on the object `x`.  If `x` is a function its output is similar to that of `args()`. Default values are given to function arguments using the construction (`argument name = value`). It is good programming practice to make extensively use of comments to describe arguments and / or what a particular chunk of code does.
What is the usage of the following function:


``` r
cube <- function(a) a^3
```

In the above function the argument a is called a *<span style="color:#FF9966">dummy argument</span>*. What will happen to an object `a` in the working directory?

Functions are called by replacing the *<span style="color:#FF9966">formal arguments</span>* by the *<span style="color:#FF9966">actual arguments</span>*.  This can be done *<span style="color:#FF9966">by position</span>* or *<span style="color:#FF9966">by name</span>*.  *Hint*: It is less error prone to call functions using named arguments. Create the following function


``` r
Demofunc <- function(vec = 1:10, m,k)
 { # Function to subtract a specified constant from
   # each element of a given vector and after subtraction
   # divide each element by a second specified constant.
   # The result of the above transformation is returned.
 (vec - m)/ k 
}
```

Execute the following function calls and explain the output

``` r
Demofunc(3, 2, 5)
#> [1] 0.2
Demofunc(2,5)
#> Error in Demofunc(2, 5): argument "k" is missing, with no default
Demofunc(m = 2, k = 5)
#>  [1] -0.2  0.0  0.2  0.4  0.6  0.8  1.0  1.2  1.4  1.6
Demofunc(m = 2, k = 5, vec = 1:100)
#>   [1] -0.2  0.0  0.2  0.4  0.6  0.8  1.0  1.2  1.4  1.6  1.8
#>  [12]  2.0  2.2  2.4  2.6  2.8  3.0  3.2  3.4  3.6  3.8  4.0
#>  [23]  4.2  4.4  4.6  4.8  5.0  5.2  5.4  5.6  5.8  6.0  6.2
#>  [34]  6.4  6.6  6.8  7.0  7.2  7.4  7.6  7.8  8.0  8.2  8.4
#>  [45]  8.6  8.8  9.0  9.2  9.4  9.6  9.8 10.0 10.2 10.4 10.6
#>  [56] 10.8 11.0 11.2 11.4 11.6 11.8 12.0 12.2 12.4 12.6 12.8
#>  [67] 13.0 13.2 13.4 13.6 13.8 14.0 14.2 14.4 14.6 14.8 15.0
#>  [78] 15.2 15.4 15.6 15.8 16.0 16.2 16.4 16.6 16.8 17.0 17.2
#>  [89] 17.4 17.6 17.8 18.0 18.2 18.4 18.6 18.8 19.0 19.2 19.4
#> [100] 19.6
```

Note the use of  `prompt()` and `package.skeleton()` to provide a new function with a help-file.

The final expression in an R function is automatically returned when the function completes execution.


``` r
my.func <- function(a=5) 
{  a+2
}
my.func()
#> [1] 7
```

When a function consists of a single line, it can be written more succinctly


``` r
my.func <- function(a=5) {  a+2  }
my.func()
#> [1] 7
```

or even without the `{ }`:


``` r
my.func <- function(a=5) a+2
my.func()
#> [1] 7
```

In general, functions will consist of more lines of code and often multiple outputs are returned. If only a single output object needs to be returned, the object can be created in the last line of the code


``` r
my.func <- function(a=5)
  {  number <- (a+3)^2
     number/a
  }
my.func()
#> [1] 12.8
```

or with a `return()` statement:


``` r
my.func <- function(a=5)
  {  number <- (a+3)^2
     return(number/a)
  }
my.func()
#> [1] 12.8
```

In general, all the outputs are combined and returned as a `list`. The final expression in the function creates the list object:


``` r
my.func <- function(a=5)
  {  number <- (a+3)^2
     list(number/a)
  }
my.func()
#> [[1]]
#> [1] 12.8
```

To return multiple outputs, the list is simply extended as shown below:


``` r
my.func <- function(a=5)
  {  number <- (a+3)^2
     list(number, number/a)
  }
my.func()
#> [[1]]
#> [1] 64
#> 
#> [[2]]
#> [1] 12.8
```

It is good practice to name the output objects in the list, such as:


``` r
my.func <- function(a=5)
  {  number <- (a+3)^2
     list(number = number, ratio = number/a)
  }
my.func()
#> $number
#> [1] 64
#> 
#> $ratio
#> [1] 12.8
```

Finally, to place the output into an object for further processing, the function is assigned to an object name:


``` r
my.func <- function(a=5)
  {  number <- (a+3)^2
     list(number = number, ratio = number/a)
  }
out <- my.func()
out
#> $number
#> [1] 64
#> 
#> $ratio
#> [1] 12.8
```

##	How R finds data { #findData }

In order to understand how objects are found by R it is necessary to have some understanding of the concepts

*	Environment
*	Frame
*	Search path
*	Parent environment 
*	Inheritance.

The mechanism that R uses to organize objects is based on frames and environments. A *<span style="color:#FF9966">frame</span>* is a collection of named objects and an *<span style="color:#FF9966">environment</span>* consists of a frame together with a pointer or reference to another environment called the *<span style="color:#FF9966">parent environment</span>*. Environments are nested so that the *<span style="color:#FF9966">parent environment</span>* is the environment that directly contains the current environment. At the start of an R session a *<span style="color:#3399FF">workspace</span>* is created which always has an associate environment, the *<span style="color:#FF9966">global environment</span>*. The global environment occupies the first position on the *<span style="color:#FF9966">search path</span>* and is accessed by a call to `globalenv()`. Packages and databases can be added to the search path by a call to `attach()` and removed from the search path by a call to `detach()`. 

*	What is an R *<span style="color:#FF9966">package</span>*?  What is the difference between *<span style="color:#FF9966">installing</span>* and *<span style="color:#FF9966">loading</span>* a package?
*	Work through the following example:


``` r
search()
#> [1] ".GlobalEnv"        "package:stats"    
#> [3] "package:graphics"  "package:grDevices"
#> [5] "package:utils"     "package:datasets" 
#> [7] "package:methods"   "Autoloads"        
#> [9] "package:base"
```

To attach the package `MASS`


``` r
library (MASS)
```

By default `MASS` is attached in the second position in the search path.


``` r
search()
#>  [1] ".GlobalEnv"        "package:MASS"     
#>  [3] "package:stats"     "package:graphics" 
#>  [5] "package:grDevices" "package:utils"    
#>  [7] "package:datasets"  "package:methods"  
#>  [9] "Autoloads"         "package:base"
```

We use `detach` to remove `MASS` from the search path.


``` r
detach("package:MASS")
search()
#> [1] ".GlobalEnv"        "package:stats"    
#> [3] "package:graphics"  "package:grDevices"
#> [5] "package:utils"     "package:datasets" 
#> [7] "package:methods"   "Autoloads"        
#> [9] "package:base"
```

To obtain the parent of the global environment


``` r
parent.env(.GlobalEnv)
#> <environment: package:stats>
#> attr(,"name")
#> [1] "package:stats"
#> attr(,"path")
#> [1] "C:/Program Files/R/R-4.5.1/library/stats"
parent.env(parent.env(.GlobalEnv))
#> <environment: package:graphics>
#> attr(,"name")
#> [1] "package:graphics"
#> attr(,"path")
#> [1] "C:/Program Files/R/R-4.5.1/library/graphics"
parent.env(parent.env(parent.env(.GlobalEnv)))
#> <environment: package:grDevices>
#> attr(,"name")
#> [1] "package:grDevices"
#> attr(,"path")
#> [1] "C:/Program Files/R/R-4.5.1/library/grDevices"
environmentName(parent.env(parent.env(parent.env(.GlobalEnv))))
#> [1] "package:grDevices"
```

When the R evaluator looks for an object and it cannot find the name in the global environment it will search the parent of the global environment. It will carry on the search along the search path until the first occurrence of the name.  If the name is not found it will return the message `Error: object 'xx' not found`. The usage of the double colon `::` and the triple colon `:::` is to access the intended object when more than one object with the same name exist on the search path.  These two operators use the *<span style="color:#FF9966">namespace</span>* facility of R packages. The namespace of a package allow the creator of a package to hide functions and data that are meant only for internal use; it provides a way through the operators `::` and `:::` to an object within a particular package. Thus a namespace prevent functions from breaking down when a user selects a name that clashes with one in the package. The double-colon operator `::` selects objects from a particular namespace. Only functions that are exported from the package can be retrieved in this way.  The triple-colon operator `:::` acts like the double-colon operator but also allows access to hidden objects. Packages are often inter-dependent, and loading one may cause others to be automatically loaded. Such automatically loaded packages are not added to the search list. 

We note that the *<span style="color:#FF9966">function</span>* call `getAnywhere()`, which searches multiple packages can be used for finding hidden objects. When a function is called, R creates a new (temporary) environment which is enclosed in the current (calling) environment. Objects created in the new environment are not available in the parent environment and dies with it when the function terminates. Objects in the calling environment are available for use in the new environment created when a function is called. 

Similarly, when an *<span style="color:#FF9966">expression</span>* is evaluated a hierarchy of environments is created. Search for objects continue up this hierarchy and if necessary to the global environment and from there up onto the search path.

* Study the use of the arguments `pos`, `all.names`, and `pattern` of the function `objects()`.
*	Study the behaviour of the functions `conflicts()` and `exists()` in the examples below:


``` r
conflicts()
#> [1] "body<-"    "kronecker" "plot"
conflicts(detail=TRUE)
#> $`package:graphics`
#> [1] "plot"
#> 
#> $`package:methods`
#> [1] "body<-"    "kronecker"
#> 
#> $`package:base`
#> [1] "body<-"    "kronecker" "plot"
exists("kronecker")
#> [1] TRUE
exists("kronecker", where = 1)
#> [1] TRUE
exists("kronecker", where = 1, inherits = FALSE)
#> [1] FALSE
exists("kronecker", where = 2)
#> [1] TRUE
exists("kronecker", where = 2, inherits = FALSE)
#> [1] FALSE
exists("kronecker", where = 7, inherits = FALSE)
#> [1] TRUE
exists("kronecker", where = 8, inherits = FALSE)
#> [1] FALSE
exists("kronecker", where = 9, inherits = FALSE)
#> [1] TRUE
```

* Study the above code carefully and then explain what inheritance does.
* The example below leads to the same conclusion as above but is more complicated at this stage.  Its behaviour will become clear as we work through the coming chapters.


``` r
sapply(search(), function(x) exists("kronecker", where = x, inherits=FALSE))
#>        .GlobalEnv     package:stats  package:graphics 
#>             FALSE             FALSE             FALSE 
#> package:grDevices     package:utils  package:datasets 
#>             FALSE             FALSE             FALSE 
#>   package:methods         Autoloads      package:base 
#>              TRUE             FALSE              TRUE
```

* Direct access to objects down the search path can be achieved with the function `get()`. 
The function `get()` takes as its first argument the name of an object as a character string. The optional argument `pos` can be used to specify where on the search list to look for the object.  As an illustration explain the outcomes of the following function calls:


``` r
get ("%o%") 
#> function (X, Y) 
#> outer(X, Y)
#> <bytecode: 0x0000015754e8b060>
#> <environment: namespace:base>
mean <- mean (rnorm (1000))
get (mean)
#> Error in get(mean): invalid first argument
get ("mean") 
#> [1] 0.01177483
get ("mean", pos = 1) 
#> [1] 0.01177483
get ("mean", pos = 2)
#> function (x, ...) 
#> UseMethod("mean")
#> <bytecode: 0x0000015752f17530>
#> <environment: namespace:base>
rm (mean)
```

* Instead of attaching databases the function `with()` is often to be preferred. Discuss the usage of `with()` by referring to the instructions:


``` r
with (beaver1, mean(time))
#> [1] 1312.018
with (beaver2, mean(time))
#> [1] 1446.2
```

##	The organisation of data (data structures)

Study the help files of `list()`, `matrix()`, `data.frame()` and `c()` carefully.

A *<span style="color:#FF9966">list</span>* is created with the function `list()`.  A list is the basic means of storing a collection of data objects in R when the modes and/or lengths of the objects are different. List elements are accessed using `[[ ]]` or `$` when the objects are named. List objects are named using the construction


``` r
my.list <- list(name1 = 1:10, name2 = mean)
my.list
#> $name1
#>  [1]  1  2  3  4  5  6  7  8  9 10
#> 
#> $name2
#> function (x, ...) 
#> UseMethod("mean")
#> <bytecode: 0x0000015752f17530>
#> <environment: namespace:base>
```

and elements are retrieved using the instruction


``` r
my.list[[2]]
#> function (x, ...) 
#> UseMethod("mean")
#> <bytecode: 0x0000015752f17530>
#> <environment: namespace:base>
my.list$name2
#> function (x, ...) 
#> UseMethod("mean")
#> <bytecode: 0x0000015752f17530>
#> <environment: namespace:base>
```

A *<span style="color:#FF9966">matrix</span>* in R is a rectangular collection of data, all of the same mode (e.g. numeric, character/text or logical). It is formed with the construction 


``` r
my.matrix <- matrix(1:12, ncol=3, nrow=4, byrow=FALSE)
my.matrix
#>      [,1] [,2] [,3]
#> [1,]    1    5    9
#> [2,]    2    6   10
#> [3,]    3    7   11
#> [4,]    4    8   12
```

Matrix elements are accessed using `my.matrix[i,j]`. The functions `nrow()`, `ncol()`, `dim()`, `dimnames()`, `colnames()` and `rownames()` are useful when working with matrices.

A *<span style="color:#FF9966">dataframe</span>* is also a rectangular collection of data but the columns can be of different modes. It can be regarded as a cross between a list and a matrix. Dataframes are constructed with the function `data.frame()`.

Study the help files of the above functions. 
 
##	Time series
Study the usage of the function `ts()`.

## The functions `as.xxx()` and `is.xxx()`

The function `as.xxx()` transforms an object as best as possible to a specified type e.g. `as.matrix(mydata)` transforms the numerical dataframe to a numerical matrix. `is.xxx()` tests if the argument is of a certain type e.g. `is.matrix(mydata)` evaluates to false if `mydata` does not satisfy all the conditions of a matrix.

## Simple manipulations; numbers and vectors
	
* Explain vector calculations and the recycling principle by referring to the example below.


``` r
c(1,3,5,9) + c(1,2,3)
#> Warning in c(1, 3, 5, 9) + c(1, 2, 3): longer object length
#> is not a multiple of shorter object length
#> [1]  2  5  8 10
```

* Logical vectors. Explain the behaviour of the instruction below


``` r
sum (c (TRUE, FALSE, TRUE, TRUE, FALSE))
#> [1] 3
```

* Missing values: `NA` (indicate a missing value in the data),  `NaN` (not a number)


``` r
10/0
#> [1] Inf
0/0
#> [1] NaN
```

* Character vectors:  see section \@ref(character)

* Subscripting vectors: see section \@ref(vectorSubscripting)

## Objects, their modes and attributes

* Vector elements must be of same mode: logical, numeric, complex, character
*	Empty object; once created (e.g.  `xx <- numeric()`) components may be added (e.g. `xx[5] <- 22`)
*	Getting and setting attributes: The functions `attr()` and `attributes()`
*	Class of an object and the function  `unclass()` for removing class.

## Representation of objects
We have already seen that a representation of an object can be obtained by calling (entering) its name:


``` r
cars
#>    speed dist
#> 1      4    2
#> 2      4   10
#> 3      7    4
#> 4      7   22
#> 5      8   16
#> 6      9   10
#> 7     10   18
#> 8     10   26
#> 9     10   34
#> 10    11   17
#> 11    11   28
#> 12    12   14
#> 13    12   20
#> 14    12   24
#> 15    12   28
#> 16    13   26
#> 17    13   34
#> 18    13   34
#> 19    13   46
#> 20    14   26
#> 21    14   36
#> 22    14   60
#> 23    14   80
#> 24    15   20
#> 25    15   26
#> 26    15   54
#> 27    16   32
#> 28    16   40
#> 29    17   32
#> 30    17   40
#> 31    17   50
#> 32    18   42
#> 33    18   56
#> 34    18   76
#> 35    18   84
#> 36    19   36
#> 37    19   46
#> 38    19   68
#> 39    20   32
#> 40    20   48
#> 41    20   52
#> 42    20   56
#> 43    20   64
#> 44    22   66
#> 45    23   54
#> 46    24   70
#> 47    24   92
#> 48    24   93
#> 49    24  120
#> 50    25   85
```

It is often not convenient to have a full representation returned of an object as above. The functions `head()`, `str()` and `summary()` are available for extracting a partial representation of an object: 


``` r
head(cars)
#>   speed dist
#> 1     4    2
#> 2     4   10
#> 3     7    4
#> 4     7   22
#> 5     8   16
#> 6     9   10
summary(cars)
#>      speed           dist       
#>  Min.   : 4.0   Min.   :  2.00  
#>  1st Qu.:12.0   1st Qu.: 26.00  
#>  Median :15.0   Median : 36.00  
#>  Mean   :15.4   Mean   : 42.98  
#>  3rd Qu.:19.0   3rd Qu.: 56.00  
#>  Max.   :25.0   Max.   :120.00
str(cars)
#> 'data.frame':	50 obs. of  2 variables:
#>  $ speed: num  4 4 7 7 8 9 10 10 10 11 ...
#>  $ dist : num  2 10 4 22 16 10 18 26 34 17 ...
```

There are many more R functions provided for getting information of what an R object represents. Some of these functions like `mode()`, `class()`, `length()`, `levels()`, `is.xxx()` and `as.xxx()`  have already been encountered and others will be given in the chapters to come.    


``` r
length(cars) 
#> [1] 2
length(as.matrix(cars))
#> [1] 100
dim(cars)
#> [1] 50  2
is.matrix(cars)
#> [1] FALSE
is.data.frame(cars)
#> [1] TRUE
is.list(cars)
#> [1] TRUE
mode(cars)
#> [1] "list"
class(cars)
#> [1] "data.frame"
levels(cars)
#> NULL
```

##	Exercise

::: {style="color: #80CC99;"}

### Exercise

According to the central limit theorem (CLT) the distribution of the sum (or mean) of independently, identically distributed stochastic variables converges to a normal distribution with an increase in the number variables. The binomial distribution can be expressed as the sum of independently, identically distributed Bernoulli stochastic variables and therefore converges in distribution to the normal distribution. The lognormal distribution in contrast cannot be expressed as a sum.

Make use of the function `rbinom()` to generate a sample of size 10 from a binomial distribution modelling 20 coin flips with a probability of $0.4$ for returning “heads”. Use the function `hist()` to graph the results. Repeat with sample sizes $50$, $100$, $1000$, $10000$ and $100000$. 
Repeat the whole study with a success probability of $0.5$, $0.3$, $0.1$ and $0.05$. Discuss your findings.

Now repeat the same exercise using (a) the lognormal distribution with the function `rlnorm()` and (b) the uniform distribution over the interval $[10; 25]$ with the function `runif(min = 10, max = 25)`. Comment on your findings.

### Exercise

Assume that a random sample of size $n$ is available from a certain distribution. A bootstrap sample is obtained by sampling with replacement a sample of size $n$ from the given sample. One of the uses of the bootstrap is to obtain an estimate of the standard error of a statistic. For example, a bootstrap estimate of the standard error of $\bar{X}$ can be obtained as follows:

*	Generate independently of each other $B$ bootstrap samples.
*	Calculate the mean of the B bootstrap samples, i.e. calculate $\bar{x}_1^*, \bar{x}_2^*, \dots, \bar{x}_B^*$.
*	Calculate $\bar{\bar{x}} = \frac{1}{B} \sum_{i=1}^{B}{\bar{x}_i^*}$.
*	Calculate $\hat{se}(b) = \sqrt{\frac{1}{B-1} \sum_{i=1}^{B}{(\bar{x}_i^*-\bar{\bar{x}})^2}}$.

(a)	Generate a random sample of size $25$ from a $normal (100; 255)$ distribution.

(b)	Use R to obtain graphical representations and statistics of the characteristics of the sample.

(c) Program the necessary instructions in R to obtain bootstrap estimates of the standard error of the sample mean as well as the sample median. Use $50$, $100$, $500$ and $1000$ for $B$ (the number of bootstrap repetitions). How do your answers compare with what is theoretically expected?

(d)	Program the necessary R instructions to obtain graphical representations of the bootstrap distribution in (c).

### Exercise

Generate a random sample of size $50$ from a multivariate normal distribution with mean vector $(118, 396, 118, 400)$ and a covariance matrix so that the variances of the variables are given by $778$, $1810$, $580$ and $2535$ respectively. Variables 1 and 2 have a covariance of $-642.5$ and variables 3 and 4 have a covariance of $-670$.  The other variables are uncorrelated. Store the sample as a matrix object and then program the necessary R instructions to calculate the sample covariance matrix and sample mean vector.

### Exercise

Execute the instruction `set.seed(101023)`.

Next, obtain $400$ random $normal (0; 1)$ values and arrange them in a matrix with $20$ rows and $20$ columns. Finally, write an R function to calculate and return (i) the sum of all the elements in the matrix, (ii) the eigenvalues of the matrix, (iii) the inverse of the matrix as well as (iv) the rank of the matrix <span style="text-decoration:underline">making use of the eigenvalues</span>. *Hint*: Read the help of the functions `eigen()` and `solve()`.)

:::



