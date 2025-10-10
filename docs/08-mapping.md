# Vectorized programming and mapping functions {#mapping}

In this chapter we continue the study the art of R programming. An important topic is a set of tools operating on objects like matrices, dataframes and lists as wholes. 

## Mapping functions to a matrix

(a)	What is understood by a mapping function and of what use are such functions?

(b)	The function `apply()`.

    (i)	What three arguments are required?
    
    (ii) Suppose the third argument is a function. How are the arguments of this function used within `apply()`?

<div style="margin-left: 40px; margin-right: 20px;">
* What is the result of the instruction `apply(is.na(x),2,all)`?

* What is the result of the instruction `x[ ,!apply(is.na(x), 2,all)]`?

* What is the result of the instruction `x[ ,!apply(is.na(x), 2,any)]`?

* Set the random seed to 137921. Obtain a matrix $\mathbf{A}:10 \times 6$ of random $n(0, 1)$ values. Use `apply()` to find the $10\%$ trimmed mean of each row.
</div>    
    
(c)	The function `sweep()`.

    (i)	What arguments are required?

    (i)	What are the similarities and differences between the arguments of `sweep()` and `apply()`?

    (iii)	Normalise the columns of a given matrix to have zero means and unit variances using `scale()`, `apply()` and `sweep()`. Which method is the fastest?
    
(d)	The function `ifelse()`.

The usage is illustrated in the following diagram.


\includegraphics[width=1\linewidth]{pics/ifelse} 

<div style="margin-left: 25px; margin-right: 20px;">
(i)	Note the difference between the function `ifelse()` and the control statement: `if` - `else`.

(ii)	What arguments are required?

(iii)	Study the help file in detail.
</div>    

(e)	The function `outer()`.

    (i)	What arguments are required?
    
    (ii)	Revise our previous example of `outer()` when constructing a perspective plot with `persp()`.
    
(f)	Work through the following examples and note in particular how the above functions are used together:

    (i)	Find the maximum value(s) in each column of the `LifeCycleSavings` data set.
    
    (ii) Use `apply()` together with `cut()` to divide each column of the LifeCycleSaving data set  into low, medium and high.
    
    (iii) Use `apply()` to plot each column of the `LifeCycleSaving` data set against the ratio of `pop75` to `pop15` on the x-axis. 
    
    (iv) Use `apply()` to find the coefficient of variation of each column of the `LifeCycleSaving` data set.
    
    (v)	Use `apply()` together with `cbind()` and `rbind()` to obtain a table of the minimum and  the maximum values of each column of the LifeCycleSaving data set. 
    
    (vi)	Repeat (v) using the airquality data set with and without the elimination of the NAs by using an appropriate function definition in the call to `apply()`.
    
    (vii)	Use `sweep()` to convert the `LifeCycleSaving` data set into standardized scores. Could `apply()` also be used for this task? Discuss.
    (viii) Use `ifelse()` to  convert negative values in a given vector to zero leaving positive values and missing values unchanged.  Illustrate.                              

## Mapping functions to vectors, dataframes and lists

(a)	Study the functions `lapply()`, `sapply()` and `split()`.

(b)	Carefully study what is produced by the command


``` r
lapply (split (data.frame (state.x77),   
               cut (data.frame (state.x77)$Illiteracy, 3)), pairs)
```

![](08-mapping_files/figure-latex/splitExample-1.pdf)<!-- --> ![](08-mapping_files/figure-latex/splitExample-2.pdf)<!-- --> ![](08-mapping_files/figure-latex/splitExample-3.pdf)<!-- --> 

```
#> $`(0.498,1.27]`
#> NULL
#> 
#> $`(1.27,2.03]`
#> NULL
#> 
#> $`(2.03,2.8]`
#> NULL
```

<div style="margin-left: 25px; margin-right: 20px;">
Note: in order to see all graphs in the R-GUI it is necessary to issue the command
</div>    


``` r
par(ask=TRUE) 
```

<div style="margin-left: 25px; margin-right: 20px;">
before calling the function `lapply()`.
</div>    

(c)	Use `lapply()` to produce histograms of each of the variables in the `state.x77` data set such that each histogram has as title the correct variable name. The $x$- and $y$-axis must also be labelled correctly.

## The functions: `mapply()`, `rapply()` and `Vectorize()`

(a)	To apply a function to more than one list, `mapply()` is a multivariate version of `sapply()`. The first argument to `mapply()` is a function followed by the arguments for that function. The first argument function is applied to each of the elements in the following arguments.


``` r
mapply (function (x,y,z) {x+y+z}, x = c(2, 3), y = c(4,5), z = c(1,8))
#> [1]  7 16
mapply (function(x,y,z) { list (min (c(x,y,z)), max (c(x,y,z))) }, 
        x = c(2, 3), y = c(4, 5), z = c(1, 8))
#>      [,1] [,2]
#> [1,] 1    3   
#> [2,] 4    8
```

(b)	Study the help-files of `rapply()` and `Vectorize()`.

## The mapping function tapply() for grouped data

(a)	Study the arguments of `tapply()`.

(b)	Consider the `LifeCycleSavings` data set. Create an object `ddpigrp` that groups the `LifeCycleSavings` data into four groups G1, G2, G3 and G4 such that G1 members have `ddpi` within $(0, 2.0]$, G2 members have `ddpi` within $(2.0, 3.5]$,  G3 members have `ddpi` within $(3.5, 5.0]$, and G4 members have `ddpi`  larger than $5.0$. Use  `tapply()` to obtain the mean aggregate personal savings of each of  the groups defined by `ddpigrp`.

(c)	If it is needed to break down a vector by more than one categorical variable, a list containing the grouping variables is used as the second argument to `tapply()`. Illustrate this by finding the mean aggregate personal savings of the groups in `ddpigrp` broken down by the `pop15` rating.

(d)	In order to use `tapply()` on more than one variable simultaneously `apply()` can be used to map `tapply()` to each of the variables in turn. Study the following command and its output carefully:


``` r
ddpigrp <- cut (LifeCycleSavings$ddpi, 
                breaks = c(0, 2, 3.5, 5, max(LifeCycleSavings$ddpi)),
                labels = paste0 ("G", 1:4))
apply (LifeCycleSavings [,c (1, 3, 4)], 2, function(x) 
                                           tapply (x, ddpigrp, mean)) 
#>           sr    pop75       dpi
#> G1  7.855385 1.790769  712.1677
#> G2  8.230625 2.456250 1497.0731
#> G3 11.959000 3.189000 1569.4910
#> G4 11.831818 1.834545  584.6964
```

(e) If `tapply()` is called without a third argument it returns a vector of the same length than its first argument containing an index into the output that normally would be produced.  Illustrate this behaviour and discuss its usage.

## The control of execution flow statement if-else and the control functions `ifelse()` and `switch()`

(a)	The primary tool for conditional computations is the `if` statement. It takes the form:


``` default
if (logical condition evaluating to either TRUE or FALSE)
	{
     First set consisting of one or more R expressions
 	}
else
	{
     Second set consisting of one or more R expressions
	} 
Expression3
```

(b)	In the above the `else` and its accompanying expression(s) are optional.

(c)	If-else statements can be nested.

(d)	Remember that the function `ifelse()` operates on objects as wholes as illustrated below:


``` r
xx <- matrix(1:25, ncol=5)
xx
#>      [,1] [,2] [,3] [,4] [,5]
#> [1,]    1    6   11   16   21
#> [2,]    2    7   12   17   22
#> [3,]    3    8   13   18   23
#> [4,]    4    9   14   19   24
#> [5,]    5   10   15   20   25
ifelse(xx < 10, 0, 1)
#>      [,1] [,2] [,3] [,4] [,5]
#> [1,]    0    0    1    1    1
#> [2,]    0    0    1    1    1
#> [3,]    0    0    1    1    1
#> [4,]    0    0    1    1    1
#> [5,]    0    1    1    1    1
```

(e)	Note that the function `match()` can be used as an alternative to multiple if-else statements in certain cases. The function `match()` takes as first argument a vector, `x`, of values to be matched and as second argument, `table`, a vector of possible values to be matched against. A third argument `nomatch = NA` specifies the return value if no match occurs.  See the example below:


``` r
match (c (1:5, 3), c (2, 3))
#> [1] NA  1  2 NA NA  2
match (c (1:5, 3), c (2, 3), nomatch = 0)
#> [1] 0 1 2 0 0 2
match (c (1:5, 3), c (3, 2), nomatch = 0)
#> [1] 0 2 1 0 0 1
```

(f)	The following example provides an illustration of the usage of `match()`:


``` r
month.num <- 5:9
month.name <- c("May", "June", "July", "Aug", "Sept")
new.vec <-  month.name [match (airquality [, "Month"], month.num)]
out <- data.frame (airquality [ ,1:5], MonthName = new.vec, 
                   Day = airquality$Day)
out[c(1:5,148:153), ]
#>     Ozone Solar.R Wind Temp Month MonthName Day
#> 1      41     190  7.4   67     5       May   1
#> 2      36     118  8.0   72     5       May   2
#> 3      12     149 12.6   74     5       May   3
#> 4      18     313 11.5   62     5       May   4
#> 5      NA      NA 14.3   56     5       May   5
#> 148    14      20 16.6   63     9      Sept  25
#> 149    30     193  6.9   70     9      Sept  26
#> 150    NA     145 13.2   77     9      Sept  27
#> 151    14     191 14.3   75     9      Sept  28
#> 152    18     131  8.0   76     9      Sept  29
#> 153    20     223 11.5   68     9      Sept  30
```

(g)	The function `switch()` provides an alternative to a set of nested if-else statements. It takes as first argument, `EXPR`, an integer value or a character string and as second argument, `...`, the list of alternatives. As an illustration:


``` r
centre <- function(x, type) 
  { switch(type,
           mean = mean(x),
           median = median(x),
           trimmed = mean(x, trim = 0.1))
  }

x <- rcauchy(10)
x
#>  [1] -0.6897862  0.9203964 -3.4916787  8.3234230  0.1589843
#>  [6] -5.1391375 -2.1279538 -9.5079710  7.2078543 -0.1195617
centre(x,"mean")
#> [1] -0.4465431
centre(x,"median")
#> [1] -0.4046739
centre(x,"trimmed")
#> [1] -0.4101104
```

(h)	The two logical control operators `&&` and `||` are useful when using if-else statements. These two operators operate on logical expressions in contrast to the operators `&` and `|` which operate on vectors/matrices.

## Loops in R

(a)	`for` loops: The general form is


``` default
for (name in values)
      { expression(s)
      }
```

<div style="margin-left: 25px; margin-right: 20px;">
This type of loop is useful if it is known in advance *<span style="color:#FF9966">how many times</span>* the statements in the loop are to be performed. In the above definition values can be either a vector or a list with elements not  restricted to be numeric:
</div>   


``` r
for (i in 1:26) cat(i, letters[i],"\n")
#> 1 a 
#> 2 b 
#> 3 c 
#> 4 d 
#> 5 e 
#> 6 f 
#> 7 g 
#> 8 h 
#> 9 i 
#> 10 j 
#> 11 k 
#> 12 l 
#> 13 m 
#> 14 n 
#> 15 o 
#> 16 p 
#> 17 q 
#> 18 r 
#> 19 s 
#> 20 t 
#> 21 u 
#> 22 v 
#> 23 w 
#> 24 x 
#> 25 y 
#> 26 z
for (letter in letters) cat(letter, "\n")
#> a 
#> b 
#> c 
#> d 
#> e 
#> f 
#> g 
#> h 
#> i 
#> j 
#> k 
#> l 
#> m 
#> n 
#> o 
#> p 
#> q 
#> r 
#> s 
#> t 
#> u 
#> v 
#> w 
#> x 
#> y 
#> z
```

<div style="margin-left: 25px; margin-right: 20px;">
Consider a list consisting of several matrices, each with different numbers of rows but the same number of columns. Write an R function that will create a single matrix consisting of all the elements of the given list concatenated by rows.
</div>   

(b) `while` loops: The general form is


``` default
while (condition)
        { expression(s)
        }
```

<div style="margin-left: 25px; margin-right: 20px;">
This type of loop continues while condition evaluates to TRUE.
</div>   

(c)	Control inside loops: `next` and `break`

<div style="margin-left: 25px; margin-right: 20px;">
The command `next` is used to skip over any remaining statements in the loop and continue executing. The command `break` causes the immediate exit from the loop. In nested loops these commands apply to the most recently opened loop.
</div>   

(d)	`repeat` loops: The general form is


``` default
repeat { expression(s)
       }
```

<div style="margin-left: 25px; margin-right: 20px;">
This type of loop continues until a break command is encountered.
</div>   

(e)	Remember that many operations that might be handled by loops can be more efficiently performed in R by using the subscripting tools discussed earlier.

(f)	As a further example we will consider the calculation of the Pearson chi-squared statistic for the test of independence in a two-way classification table:

$$
\chi^2_p = \sum_{i=1}^r \sum_{j=1}^c \frac{(f_{ij}-e_{ij})^2}{e_{ij}}
$$


with $e_{ij} = \frac{f_{i.}f_{.j}}{f_{..}}$ the expected frequencies. This statistic can be calculated in R without using loops as follows:


``` r
fi. <- ftable %*% rep (1, ncol (ftable))
f.j <- rep (1, nrow (ftable)) %*% ftable
e <- (fi. %*% f.j)/sum(fi.)
X2p <- sum ( (ftable-e)^2 /e)
```

Explicit loops in R can potentially be expensive in terms of time and memory. The functions `apply()`, `tapply()`, `sapply()` and `lapply()` should be used instead if possible. The expected frequencies in the previous example can, for example, be obtained as follows:


``` r
e.freq <- outer (apply (ftable, 1, sum),  apply (ftable, 2, sum)) / sum(ftable)
```

## The execution time of R tasks

The functions `system.time()` and `proc.time()` provide information regarding the execution of R tasks. 

(a)	`proc.time` determines how much real and CPU time (in seconds) the currently running R process has already take:


``` r
proc.time()   # called with no arguments
#>    user  system elapsed 
#>    0.25    0.03    3.42
```

(b) `system.time(expr)` calls the function `proc.time()`, evaluates `expr`, and then calls `proc.time()` once more, returning the difference between the two `proc.time()` calls:


``` r
system.time (hist (rev (sort (rnorm (1000000)))))
```

![](08-mapping_files/figure-latex/systemtimeExample-1.pdf)<!-- --> 

```
#>    user  system elapsed 
#>    0.09    0.03    0.21
```

<div style="margin-left: 25px; margin-right: 20px;">
Note that user and system times do not necessarily add up to elapsed time exactly.
</div>   

(c)	Write the necessary code using `proc.time()` directly to obtain the execution time of `hist (rev (sort (rnorm (1000000))))`.

(d)	As an application of `system.time()` and `proc.time()` perform the following simulation study: Given a covariance matrix $\mathbf{S}:p \times p$ the task is to compute the corresponding correlation matrix. The execution times of the following three methods are to be compared:

    (i)	Direct elementwise calculation of $r_{ij} = \frac{s_{ij}}{\sqrt{s_{ii}s_{jj}}}$ using two nested for loops;
    
    (ii) Two applications of `sweep()`;
    
    (iii) Matrix multiplication where $\mathbf{R}:p \times p = [diag(\mathbf{S})]^{-\frac{1}{2}} \mathbf{S} [diag(\mathbf{S})]^{-\frac{1}{2}}$  where $diag(\mathbf{A})$ denotes the diagonal matrix formed from $\mathbf{A}:p \times p$ by setting all its off-diagonal elements equal to zero.

<div style="margin-left: 25px; margin-right: 20px;">
Use `var()` and `rnorm()` to compute covariance matrices of different sizes $p$ from samples varying in size $n$. Study the role of $n$ and $p$ in the effectiveness (economy in execution time) of the above three methods. Display the results graphically. Remember that for valid comparisons the three methods must be executed with identical samples.
</div>   
   
## The calling of functions with argument lists

(a)	The function `do.call()` provides an alternative to the usual method of calling  functions by name. It allows specifying the name of the function with its arguments in the form of a list:


``` r
mean ( c (1:100, 500), trim=0.1)
#> [1] 51
do.call ("mean", list( c (1:100, 500), trim=0.1))
#> [1] 51
```

(b)	 How does `do.call()` differ from the function `call()`?

(c)	As an illustration of the usage of `do.call()` study the following example: 


``` r
na.pattern <- function(frame)
{ nas <- is.na (frame)
  storage.mode (nas) <- "integer"
  table (do.call ("paste", c(as.data.frame(nas), sep = "")))
}
na.pattern(as.data.frame(airquality))
#> 
#> 000000 010000 100000 110000 
#>    111      5     35      2
```

<div style="margin-left: 25px; margin-right: 20px;">
What can be learned from the above output?
</div>   

(d)	What is the difference between `as.integer()`, `storage.mode() <– "integer"`, `storage.mode()` and `mode()`?

## Evaluating R strings a commands

Recall from Figure \@ref(fig:expression) that the function `parse(text = "3 + 4")` returns the unevaluated expression `3 + 4`. In order to evaluate the expression use function `eval()`: `eval (parse (text = "3 + 4"))` returns `7`.

## Object oriented programming in R

Suppose we would like to investigate the body of function `plot()`. We know that this can be done by entering the function’s name at the R prompt:


``` r
plot
#> function (x, y, ...) 
#> UseMethod("plot")
#> <bytecode: 0x00000262a8958d80>
#> <environment: namespace:base>
```

The presence of `UseMethod("plot")` shows that `plot()` is a *<span style="color:#FF9966">generic</span>* function.  The *<span style="color:#FF9966">class</span>* of an object determines how it will be treated by a generic function i.e. what *<span style="color:#FF9966">method</span>* will be applied to it.  Function `setClass()` is used for setting the class attribute of an object. Function `methods()` is used to find out (a) what is the repertoire of methods of a generic function and (b) what methods are available for a certain class:


``` r
methods(plot) # repertoire of methods for FUNCTION plot()
#>  [1] plot.acf*           plot.data.frame*   
#>  [3] plot.decomposed.ts* plot.default       
#>  [5] plot.dendrogram*    plot.density*      
#>  [7] plot.ecdf           plot.factor*       
#>  [9] plot.formula*       plot.function      
#> [11] plot.hclust*        plot.histogram*    
#> [13] plot.HoltWinters*   plot.isoreg*       
#> [15] plot.lm*            plot.medpolish*    
#> [17] plot.mlm*           plot.ppr*          
#> [19] plot.prcomp*        plot.princomp*     
#> [21] plot.profile*       plot.profile.nls*  
#> [23] plot.raster*        plot.spec*         
#> [25] plot.stepfun        plot.stl*          
#> [27] plot.table*         plot.ts            
#> [29] plot.tskernel*      plot.TukeyHSD*     
#> see '?methods' for accessing help and source code
methods(class="lm")  # what methods are available for CLASS lm
#>  [1] add1           alias          anova         
#>  [4] case.names     coerce         confint       
#>  [7] cooks.distance deviance       dfbeta        
#> [10] dfbetas        drop1          dummy.coef    
#> [13] effects        extractAIC     family        
#> [16] formula        hatvalues      influence     
#> [19] initialize     kappa          labels        
#> [22] logLik         model.frame    model.matrix  
#> [25] nobs           plot           predict       
#> [28] print          proj           qr            
#> [31] residuals      rstandard      rstudent      
#> [34] show           simulate       slotsFromS3   
#> [37] summary        variable.names vcov          
#> see '?methods' for accessing help and source code
```

In broad terms there are currently three types of classes in use in R: The old classes or S3 classes and the newer S4 and S5 (also called *<span style="color:#FF9966">reference classes</span>*) classes. The newer classes can contain one or more *<span style="color:#FF9966">slots</span>* which can be accessed using the operator `@`. Central to the concept of object oriented programming is that a method can inherit from another method. The function `NextMethod()` provides a mechanism for *<span style="color:#FF9966">inheritance</span>*.

(a)	As an example of a generic function study the example in the help file of the function `all.equal()`.

(b)	R provides many more facilities for writing object oriented functions. Consult the [R Language Definition Manual](https://cran.r-project.org/doc/manuals/r-release/R-lang.pdf) Chapter 5: Object-Oriented Programming for further details.

(c)	A statistical investigation is often concerned with survey or questionnaire data where respondents must select one of several categorical alternatives. The `questdata` below shows the responses made by 10 respondents on four questions. The alternatives for each question were measured on a five point categorical scale. We can refer to the `questdata dataframe` as the full data. This form of representing the data is not an effective way of storing the data when the number of respondents is large. A more compact way of saving the data without any loss in information is to store the data in the form of a *<span style="color:#FF9966">response pattern</span>* matrix or dataframe.  The first row of `questdata` constitutes one particular response pattern namely `("b" "c" "a" "d")`. A response pattern matrix (dataframe) shows all the unique response patterns together with the frequency with which each of the different response patterns has occurred.  Your challenge is to provide the necessary R functions to convert the full data into a response pattern representation, and conversely to recover the full data from its response pattern representation.  


``` r
questdata <- rbind (c("b", "c", "a", "d"),
                    c("d", "d", "c", "a"),
                    c("a", "d", "c", "e"),
                    c("a", "d", "c", "e"),
                    c("b", "c", "a", "d"),
                    c("a", "d", "c", "e"),
                    c("b", "c", "a", "d"),
                    c("d", "d", "c", "a"),
                    c("c", "b", "a", "e"),
                    c("b", "c", "a", "d"))
colnames(questdata) <- c("Q1", "Q2", "Q3", "Q4")
```

<div style="margin-left: 40px; margin-right: 20px;">
(i)	Create the R object `questdata` and then give the following instructions:


``` r
unique (questdata [,1])
#> [1] "b" "d" "a" "c"
duplicated (questdata)
#>  [1] FALSE FALSE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE
#> [10]  TRUE
duplicated (questdata, MARGIN = 1)
#>  [1] FALSE FALSE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE
#> [10]  TRUE
duplicated (questdata, MARGIN = 2)
#>    Q1    Q2    Q3    Q4 
#> FALSE FALSE FALSE FALSE
unique (questdata)
#>      Q1  Q2  Q3  Q4 
#> [1,] "b" "c" "a" "d"
#> [2,] "d" "d" "c" "a"
#> [3,] "a" "d" "c" "e"
#> [4,] "c" "b" "a" "e"
unique (questdata, MARGIN = 1)
#>      Q1  Q2  Q3  Q4 
#> [1,] "b" "c" "a" "d"
#> [2,] "d" "d" "c" "a"
#> [3,] "a" "d" "c" "e"
#> [4,] "c" "b" "a" "e"
unique (questdata, MARGIN = 2)
#>       Q1  Q2  Q3  Q4 
#>  [1,] "b" "c" "a" "d"
#>  [2,] "d" "d" "c" "a"
#>  [3,] "a" "d" "c" "e"
#>  [4,] "a" "d" "c" "e"
#>  [5,] "b" "c" "a" "d"
#>  [6,] "a" "d" "c" "e"
#>  [7,] "b" "c" "a" "d"
#>  [8,] "d" "d" "c" "a"
#>  [9,] "c" "b" "a" "e"
#> [10,] "b" "c" "a" "d"
```


(ii)	Examine Table \@ref(tab:MatrixFunc) and carefully describe the behaviour of the functions `duplicated()` and `unique()`.

(iii)	Write an R function, say `full2resp` to obtain the response pattern representation of questionnaire data like those given above. Test your function on `questdata`.

(iv)	Write an R function, say `resp2full` to obtain the full data set given its response pattern representation.  Test your function on the response pattern representation of the `questdata`.
</div>    

## Recursion

Functions in R can call themselves. This process is called *<span style="color:#FF9966">recursion</span>* and it is implemented in R programming by the function `Recall()`.

(a) As an example we will use recursion to calculate $x(x+1)(x+2)\dots(x+k)$ with $k$ a positive integer: 


``` r
recurs.example <- function (x, k) 
{ # Function to calculate x(x+1)(x+2).....(x+k)
  # where k is a positive integer.
     if (k < 0 ) 
      stop("k not allowed to be negative or non-integer")
    else if( k == 0) x
       else(x+k) * Recall(x,k-1)
   }
```

<div style="margin-left: 25px; margin-right: 20px;">
Investigate if `recurs.example()` works correctly.
</div>    

(b)	Explain how recursion works by studying the output of the following function for values of $r = 1, 2, 3, 4, 5, 6$:


``` r
Recursiontest <- function (r)
{ if (r <= 0) NULL
  else { cat("Write = ", r, "\n")
         Recall (r - 1)
         Recall (r - 2)
       }
}
Recursiontest(1)
#> Write =  1
#> NULL
Recursiontest(2)
#> Write =  2 
#> Write =  1
#> NULL
Recursiontest(3)
#> Write =  3 
#> Write =  2 
#> Write =  1 
#> Write =  1
#> NULL
Recursiontest(4)
#> Write =  4 
#> Write =  3 
#> Write =  2 
#> Write =  1 
#> Write =  1 
#> Write =  2 
#> Write =  1
#> NULL
Recursiontest(5)
#> Write =  5 
#> Write =  4 
#> Write =  3 
#> Write =  2 
#> Write =  1 
#> Write =  1 
#> Write =  2 
#> Write =  1 
#> Write =  3 
#> Write =  2 
#> Write =  1 
#> Write =  1
#> NULL
Recursiontest(6)
#> Write =  6 
#> Write =  5 
#> Write =  4 
#> Write =  3 
#> Write =  2 
#> Write =  1 
#> Write =  1 
#> Write =  2 
#> Write =  1 
#> Write =  3 
#> Write =  2 
#> Write =  1 
#> Write =  1 
#> Write =  4 
#> Write =  3 
#> Write =  2 
#> Write =  1 
#> Write =  1 
#> Write =  2 
#> Write =  1
#> NULL
```

(c)	Use recursion and the function `Recall()` to write an R function to calculate $x!$.

(d)	Use recursion to write an R function that generates a matrix  whose rows contain subsets of size $r$  of the first $n$ elements of the vector `v`.   Ignore the possibility of repeated values in `v` and give this vector the default value of `1:n`.

##	Environments in R

Study the following parts from the *R Language definition Manual*:  § 3.5 Scope of variables; Chapter 4:  *Functions*.

Consider an R function `xx(argument)`.  Write an R function to add a constant to the correct object (i.e. the object in the correct environment) that corresponds to `argument`.  In order to answer this question, you must determine in which environment `argument` exists and evaluation must take place in this environment.  Possible candidates to consider are the *<span style="color:#3399FF">parent frame</span>*, the *<span style="color:#3399FF">global environment</span>* and the search list.   Assume that only the first data basis on the search list is not read-only so that in cases where argument can be found anywhere in the search list it can be assigned to the first data basis. *Hint*:  Study how the following functions work: `assign()`, `deparse()`, `invisible()`, `exists()`, `substitute()`, `sys.parent()`. 

## “Computing on the language”

Read *R Language Definition Manual Chapter 6: Computing on the language*.

##	Writing user friendly applications: the package shiny

The `shiny` package in R allows one to create an interactive environment inside R. As an example, the code below generates data from a bivariate normal distribution and makes a scatter plot of the two variables. With shiny a sliding bar is added where the user can adjust the correlation between the two variables.

A shiny app consists of a user interface (`ui`) a `server` function and the `shinyApp` function that uses the `ui` object and the `server` function to build a Shiny app object. For the sliding bar, the function `sliderInput()` is used. Table \@ref(tab:InputElements) provides a list of different input elements.

The `server` function uses the `inputs` – the `cor.val` in this example – to produce an `output` – the scatter plot in this example – using a reactive expression – the `plot` command in this example. The `server` function and thus the reactive expression is called with every change in the `input`, i.e. the plot is executed with the updated `cor.val`. The `output` produced by die `server` function – `scatter` in this example – is plotted in the `mainPanel` with the function `plotOutput`.

Table: (\#tab:InputElements) Input elements for shiny apps.

| ------ | ------ | ------ | 
| `actionButton()`       |	`fileInput()`     | `sliderInput()`    | 
| `checkboxGroupInput()` |	`numericInput()`  | `submitButton()`   | 
| `checkboxInput()`      |	`passwordInput()` | `textAreaInput()`  | 
| `dateInput()`          |	`radioButtons()`  | `textInput()`      | 
| `dateRangeInput()`     |	`selectInput()`   | `varSelectInput()` |


``` r
library(shiny)

ui <- pageWithSidebar(
      headerPanel("Bivariate normal plot"),
      # App title

      sidebarPanel(
      # Sidebar panel for inputs

          sliderInput(inputId = "cor.val",
                      label = "Correlation",
                      min = -1,
                      max = 1,
                      value = 0,
                      step = 0.01
          )
      ),

      mainPanel(
      # Main panel for scatter plot

          textOutput("caption"),
          plotOutput("scatter")
      )
   )

server <- function(input, output) {
         require(MASS)
         sigma <- diag(2)

         output$caption <- renderText({ paste ("Bivariate normal data with 
                                correlation", input$cor.val)
                           })
         output$scatter <- renderPlot({  
                              sigma[1,2] <- sigma[2,1] <- input$cor.val
                              X <- mvrnorm(1000, mu=c(0,0), sigma)
                              plot(X,asp=1,col="red",pch=15)
                           })
      }

shinyApp(ui, server)
```

Adjust the shiny app above by adding three more input sources:

i.	The number of observations to be generated.

ii.	Selecting the mean vector for the bivariate normal from the following options

* $\mathbf{\mu}' = [0, 0]$
* $\mathbf{\mu}' = [10, 2]$
* $\mathbf{\mu}' = [-3, -3]$
* $\mathbf{\mu}' = [8, 207]$

iii.	Having a series of radio buttons to choose the colour for the observations in the plot.

## Exercise

::: {style="color: #80CC99;"}

(a)	Write an R function to determine which positive whole number elements $≤10^{10}$  of a given vector are prime and to return these primes.  Test this function with randomly generated vectors.

(b)	Repeat (a) using recursion.

(c)	Write a Shiny App that allows the user to choose between one of the data sets:`LifeCycleSavings` and `state.x77` as a data matrix $\mathbf{X}:n \times p$. The unweighted Minkowski metric for the pairwise distance between observation $i$ and observation $j$ is defined as $d_{ij} = \left( \sum_{k=1}^p{|x_{ik}-x_{jk}|^λ} \right)^{(1/λ)}$, $λ≥1$. Make provision for the user to choose the value of $\lambda$ to be used to calculate the pairwise distances between all the rows of the data matrix. Note that $λ=1$ is the Manhattan distance and $λ=2$ is the Euclidean distance. Use $λ=2$ as your default value.

:::

##	The function on.exit()

What does the function `on.exit()` do?

One use of the special argument `...` together with the `on.exit()`  function is to allow a user to make temporary changes to graphical parameters of a graphical display  within a function.  This can be done as follows:  


``` r
function(...)
 { oldpar <- par(...)
   on.exit(par(oldpar))  
   or on.exit(par(c(par(oldpar),par(mfrow = c(1,1)))))
   new plot instructions
   ..............................
  }
```

In the above it is assumed that only arguments of `par()` can be substituted when the function concerned is called. A further use of `on.exit()` is for temporarily changing *<span style="color:#FF9966">options</span>*.

## Error tracing

Any error that is generated during the execution of a function will record details of the calls that were being executed at the time.  These details can be shown by using the function `traceback()`.  The function `dump.frames()` gives more detailed information, but it must be used sparingly because it can create very large objects in the *<span style="color:#3399FF">workspace</span>*. The function `options (error = xx)` can be used to specify the action taken when an error occurs. The recommended option during program development is `options(error = recover)`.  This ensures that an error during an interactive session will call `recover()` from  the lowest relevant function call, usually the call that produced the error. You can then browse in this or any of the currently active calls to recover arbitrary information about the state of computation at the time of the error.  An alternative is to set `options(error = dump.frames)`.  This will save all the data in the calls that were active when an error occurred. Calling `debugger()` later on produce a similar result to `recover()`.  

The following is a summary of the most common error tracing facilities in R:

| ------ | ---------------- | 
| `print()`, `cat()` | The printing of key values within a function is often all that is needed.  |   
| `traceback()`      | Must be used together with `dump.frames()`.  |   
| `options(warn=2)`  | Changes warning to an error that causes a dump.  | 
| `options(error=)`  | Changes the function that is used for the dump action.  |   
| `last.dump()`      | The object in the *<span style="color:#3399FF">.RData</span>* that contains a list of calls to dump.  |   
| `debugger()`       | Function to inspect last.dump for an error.  |   
| `browser()`        | Function that can be used within a function to interrupt the latter’s execution so that variables within the local frame concerned can be inspected.  |   
| `trace()`          | Places tracing information before or within functions.  Can be used to place calls to the browser at given positions within a function.  |   
| `untrace()`        | 	Switches all or some of the functions of `trace()` off. |   

(a)	Study the *R Language Manual Definition Chapter 9: Debugging* for a summary of error tracing facilities in R . Note especially how the functions `print()`, `cat()`, `traceback()`, `browser()`, `trace()`, `untrace()`, `debug()`, `undebug()` and `options(warn=2 or error=)` work.

(b)	Study usage of: `options(error = dump.frames);  debugger()`

(c)	Study usage of: `options(error = dump.frames)`

(d)	Study usage of  the objects  `last.dump` and `.Traceback`.

## Error handling: The function `try()`

As an example of the need to be able to handle errors properly consider a simulation study involving a large number of repetitive calculations.


``` r
Example.8.18.a <- function (iter = 500)
{ select.sample <- function (x) 
  { temp <- rnorm (100, m = 50, s = 20)
    if (any (temp < 0)) stop("Negative numbers not allowed")
    mean(log(temp))                                                         }
  out <- lapply(1:iter, function(i) select.sample(i))
  out
}
```

With `iter` set to a large value, inevitably a call to `Example.8.18.a()` will result in an error message:


``` default
> Example.8.18.a()
Error in select.sample(i) : Negative numbers not allowed.
```

To see how `try()` can be used make the following change in `Example.8.18.a()`:


``` r
Example.8.18.b <- function (iter = 500)
{ select.sample <- function (x) 
  { temp <- rnorm (100, m = 50, s = 20)
    if (any (temp < 0)) stop("Negative numbers not allowed")
    mean(log(temp))                                                         }
  out <- lapply(1:iter, function(i) 
                        try(select.sample(i), silent = TRUE))
  out
}
```

A typical chunk of output from a call to `Example.8.18.b()` is


``` default
> Example.8.18.b(2)
[[1]]
[1] 3.804975
[[2]]
[1] "Error in select.sample(i) : Negative numbers not allowed\n"
attr(,"class")
[1] "try-error"
attr(,"condition")
<simpleError in select.sample(i): Negative numbers not allowed>
```

Notice that execution of `Example.8.18.b` was not halted prematurely. From the above output we can make some final changes to our example function:


``` r
Example.8.18.c <- function (iter = 500)
{ select.sample <- function (x) 
  { temp <- rnorm (100, m = 50, s = 20)
    if (any (temp < 0)) stop("Negative numbers not allowed")
    mean(log(temp))                                                         }
  out <- lapply(1:iter, function(i) 
                        try(select.sample(i), silent = TRUE))
  out <- lapply(out, function(x)
                     { if (is.null (attr (x,"condition"))) x <- x
                       else x <- attr(x, "condition")
                     })
  Error.report <- lapply(out, function(x) 
                              ifelse(!is.numeric(x), x, "No Error"))
  Numeric.results <- unlist(lapply(out, function(x)   
                                        ifelse (is.numeric(x), x, NA)))
  list (Error.report = Error.report, Numeric.results = Numeric.results) 
}
```

Study the output of a call to `Example.8.18.c` and comment on the merits of `try()` in this example.

