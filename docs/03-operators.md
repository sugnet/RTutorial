# R operators and functions {#operators}

After completing Chapters 1 and 2 it is assumed that the following are now familiar:

*	How to communicate with R;
*	How to manage workspaces;
*	How to perform simple tasks using R.

In this chapter we take a closer look at the behaviour of some of the most common 

*	R operators
*	R functions.

##	Arithmetic operators

(a)	Study the use of the operators in Table \@ref(tab:ArithOperators).

Table: (\#tab:ArithOperators) Arithmetic operators.

| *<span style="color:#CC99FF">Operator</span>* | *<span style="color:#CC99FF">Function</span>* | *<span style="color:#CC99FF">Operator</span>* | *<span style="color:#CC99FF">Function</span>* |
| ------ | --------------- | ------ | --------------- |
| `+`   |	Addition              | `^`   | Exponentiation |
| `-`   |	Subtraction           | `%/%` | Integer divide |
| `*`   |	Multiplication        | `%%`  | Modulus        |
| `/`   |	Division              | `:`   | Sequence       |
| `%*%` |	Matrix multiplication | `-`   | Uniry minus    |

<div style="margin-left: 25px; margin-right: 20px;">
Note that the arithmetic operators are also functions.  That this is so follows by studying the following examples:
</div>


``` r
3+7
#> [1] 10
```

``` r
"+"(3,7)
#> [1] 10
```

``` r
17 %% 3
#> [1] 2
```

``` r
"%%"(17,3)
#> [1] 2
```


(b)	Rules for operator expressions with vector arguments.

<div style="margin-left: 25px; margin-right: 20px;">
  Study the results of the following R instructions.
</div>
  

``` r
cars [,2] * 12 * 25.4 / 1000
#>  [1]  0.6096  3.0480  1.2192  6.7056  4.8768  3.0480  5.4864
#>  [8]  7.9248 10.3632  5.1816  8.5344  4.2672  6.0960  7.3152
#> [15]  8.5344  7.9248 10.3632 10.3632 14.0208  7.9248 10.9728
#> [22] 18.2880 24.3840  6.0960  7.9248 16.4592  9.7536 12.1920
#> [29]  9.7536 12.1920 15.2400 12.8016 17.0688 23.1648 25.6032
#> [36] 10.9728 14.0208 20.7264  9.7536 14.6304 15.8496 17.0688
#> [43] 19.5072 20.1168 16.4592 21.3360 28.0416 28.3464 36.5760
#> [50] 25.9080
```

``` r
7%/%3
#> [1] 2
```

``` r
7%%3
#> [1] 1
```

``` r
matrix(1,nrow=4,ncol=4) * matrix(3,nrow=4,ncol=4)
#>      [,1] [,2] [,3] [,4]
#> [1,]    3    3    3    3
#> [2,]    3    3    3    3
#> [3,]    3    3    3    3
#> [4,]    3    3    3    3
```

``` r
matrix(1,nrow=4,ncol=4) %*% matrix(3,nrow=4,ncol=4)
#>      [,1] [,2] [,3] [,4]
#> [1,]   12   12   12   12
#> [2,]   12   12   12   12
#> [3,]   12   12   12   12
#> [4,]   12   12   12   12
```

<div style="margin-left: 25px; margin-right: 20px;">
  Explain the following instructions and output from R:
</div>
  

``` r
1:12 + 1:3
#>  [1]  2  4  6  5  7  9  8 10 12 11 13 15
```

``` r
1:10 + 1:2
#>  [1]  2  4  4  6  6  8  8 10 10 12
```

``` r
1:10 + 1:3
#> Warning in 1:10 + 1:3: longer object length is not a
#> multiple of shorter object length
#>  [1]  2  4  6  5  7  9  8 10 12 11
```

<div style="margin-left: 25px; margin-right: 20px;">
In the above examples it is illustrated that R uses *<span style="color:#FF9966">vectorized arithmetic</span>* i.e. it operates on vectors as wholes. Sometimes the *<span style="color:#FF9966">recycling principle</span>* is applied with or without a warning.  It is a good R programming habit to make use of vectorizing calculations where possible. The effect of the recycling principle must be kept in mind since it might lead to unwanted results.
</div>

(c)	Missing values, infinity and “not a number”.

<div style="margin-left: 25px; margin-right: 20px;">
A missing value in R is denoted by NA. The result of a computation involving NAs is always NA e.g.
</div>
  


``` r
mean(c(1,3,NA,12,5))
#> [1] NA
```

``` r
0/0
#> [1] NaN
```

``` r
5/0
#> [1] Inf
```

``` r
-5/0
#> [1] -Inf
```

``` r
5/(-0)
#> [1] -Inf
```

<div style="margin-left: 25px; margin-right: 20px;">
The result of a computation that cannot be represented as a number e.g. `0/0` is denoted by `NaN`.
Note: some computational results are differently reported by R as the corresponding algebraic equivalents, `5/0` in R is given by `Inf` while algebraically it is undefined.
</div>

(d)	Scientific notation

<div style="margin-left: 25px; margin-right: 20px;">
R uses decimal notation as well as scientific notation for arithmetic calculations. Scientific notation is not to be confused with $exp()$.
</div>
  

``` r
60000000
#> [1] 6e+07
```

``` r
1/6000000
#> [1] 1.666667e-07
```

``` r
exp(15)
#> [1] 3269017
```

``` r
exp(-15)
#> [1] 3.059023e-07
```

(e)	How are numbers represented in a computer’s memory? What are the implications of this?
  
<div style="margin-left: 25px; margin-right: 20px;">
Computers use ON/OFF (or 1/0) switches for encoding information. A single switch is called a *<span style="color:#FF9966">bit</span>* and a group of eight bits is called a *<span style="color:#FF9966">byte</span>*. A single integer is represented exactly in a computer by a fixed number of bytes i.e. 32 or 64 bits. There are several schemes according to which integers are represented by bits in a computer. This representation in a computer takes place at a level where R has no control over it but R stores information about the computing environment in an object `.Machine`. The element `.Machine$integer.max` returns the largest integer that can be represented in the computer on which R is running e.g.
</div>
  

``` r
.Machine$integer.max
#> [1] 2147483647
```

<div style="margin-left: 25px; margin-right: 20px;">
Although the above method of representing integers by strings of bits provides a very efficient way of storing integers in a computer R usually treats integers similar to real numbers by using *<span style="color:#FF9966">floating point representation</span>*.  In binary floating point notation a number x is written as a sequence of zeros and ones (the *<span style="color:#FF9966">mantissa</span>*) times two with an exponent say $m$: $x=b_0 b_1 b_2…×2^m$ where $b_0=1$ except when $x=0$.

In practice there is only a limited number of $b$’s available and the exponent is also limited therefore, in general, not all real numbers can be represented exactly in a computer  –  they can at most be approximated. The smallest number $x$ such that  $1 + x$ can be distinguished from $1$ in a computer is called *<span style="color:#FF9966">machine epsilon</span>*. In R this can be obtained from `.Machine$double.eps` e.g.
</div>
  

``` r
.Machine$double.eps
#> [1] 2.220446e-16
```

<div style="margin-left: 25px; margin-right: 20px;">
Although floating point representation allows computation with very small (in magnitude) and very large numbers the above limitations can lead to *<span style="color:#FF9966">underflow</span>* or *<span style="color:#FF9966">overflow</span>* which can have disastrous consequences in practice. Writing good code in R must take the above seriously into account.
</div>
  
##	Logical operators
  
Logical operators result in `TRUE`, `FALSE` or `NA`. Study the use of the logical operators in Table \@ref(tab:LogicOperators).  *<span style="color:#FF9966">Warning</span>*:  While it is perfectly legitimate to write


``` r
x[x == -1] <- 0
x[x == 1] <- 0 
```

it is incorrect to specify


``` r
x[x == NA] <- 0
x[x = = NaN] <- 0 
```

The correct code in the latter case is


``` r
x[is.na(x)] <- 0
x[is.nan(x)] <- 0
```

What are the consequences of the above code? Also take note of the functions `any()` and `all()`.  These two functions are useful when combining logical objects. Give the necessary instructions to carry out the following tasks:

::: {style="color: #80CC99;"}
(a)	Check if any of the states in the `state.x77` data set have populations with an illiteracy rate that is not larger than $1.6$ and a Murder rate of more than $10.0$. 
(b)	Check if there is at least one state with income greater than $\$5000$ and life expectancy less than $70.0$ years.
(c)	Check if all states with an income of more than $\$5000$ has an illiteracy of below $2.0$.
:::

<div style="margin-left: 25px; margin-right: 20px;">
What is meant by a control logical operator?
</div>

Table: (\#tab:LogicOperators) Logical operators.

| *<span style="color:#CC99FF">Operator</span>* | *<span style="color:#CC99FF">Function</span>*  |
| ------ | --------------- | 
| `>`   |	Greater than |
| `<`   |	Less than |
| `<=`  |	Less than or equal to  |
| `>=`  |	Greater than or equal to  |
| `==`  |	Equality  |
| `&`   | Elementwise and |
| `|`   | Elementwise or |
| `&&`  | Control and |
| `||`  | Control or |
| `!`   | Unary not |
| `!=`  | Not equal to |

(d)	Carry out the instructions:


``` r
mata <- matrix(1:4, ncol = 2)
matb <- matrix(c(10, 20, 30, 40), ncol = 2)
mata
#>      [,1] [,2]
#> [1,]    1    3
#> [2,]    2    4
```

``` r
matb
#>      [,1] [,2]
#> [1,]   10   30
#> [2,]   20   40
```

``` r
mata>1 & matb>1
#>       [,1] [,2]
#> [1,] FALSE TRUE
#> [2,]  TRUE TRUE
```

``` r
mata>1 | matb>1
#>      [,1] [,2]
#> [1,] TRUE TRUE
#> [2,] TRUE TRUE
```

``` r
mata>1 && matb>1
#> Error in mata > 1 && matb > 1: 'length = 4' in coercion to 'logical(1)'
```

``` r
mata>1 || matb>1
#> Error in mata > 1 || matb > 1: 'length = 4' in coercion to 'logical(1)'
```

<div style="margin-left: 25px; margin-right: 20px;">
Comment on the above.
</div>

(e)	What is the result of `sum(c(TRUE, !FALSE, FALSE, TRUE, TRUE))`?
(f)	What is the result of `sum(c(TRUE, !FALSE, FALSE, NA, TRUE))` ? 

<div style="margin-left: 25px; margin-right: 20px;">
Explain
</div>

##	The operators `<-`, `<<-` and `~`

Before considering the use of these operators answer the following:

(a)	What will happen to an object `aa` in the working directory if within a function the following assignment is made `aa <- 20`?
(b)	Now, study the help file of `<<-` and then answer (a) if the operator `<-` has been replaced with the operator `<<-`. *<span style="color:#FF9966">Warning</span>*: use `<<-` very carefully.

(c)	The tilde operator is used in modelling functions, e.g. `lm (length ~ age)`.

##	Operator precedence

Study the precedence rules as summarized in Table 3.4.1. The rules followed are shown in Table \@ref(tab:Precedence) from top to bottom and left to right. Note the use of 

*	parentheses `( )` for function arguments and changing  precedence, 
*	braces `{ }` for demarcating blocks of instructions
*	and brackets `[ ]` for subscripting.

The correct way of extracting the fifth element of a sequence like 1:20 is


``` r
(1:20)[5]
#> [1] 5
```

Table: (\#tab:Precedence) Precedence rules.

| *<span style="color:#CC99FF">Operator</span>* | *<span style="color:#CC99FF">What it does</span>*  |
| ------ | --------------- |
| `$`                | List and dataframe subscripting |
| `[]`, `[[]]`       | Vector and matrix subscripting; list subscripting |
| `^`                | Exponentiation |
| `%*%`, `%/%`, `%%` | Matrix multiplication; integer divide; modulus |
| `*`, `/`           | Multiplication and division |
| `+`, `-`           | Addition and subtraction |
| `<`, `>`, `<=`, `>=`, `==`, `!=` | Logical comparisons |
| `!`                              | Unary not |
| `&`, `|`, `&&`, `||`             | Logical and; logical or; control and; control or |
| `<-`, `<<-`        | Assignment |

Explain the result of the following R instructions:


``` r
20 / 4 * 12 ^2 - 6 + 1
#> [1] 715
```

``` r
(20 / 4) * (12 ^2) + (-6 + 14)
#> [1] 728
```

``` r
20 / 4 * 12 ^(2 - 6 + 14)
#> [1] 309586821120
```

``` r
20 / 4 * (12 ^2 - 6 + 14)
#> [1] 760
```

##	Some mathematical functions

###	General mathematical functions

`abs()`,  `exp()`,  `log(x, base = exp(1))`,  `log10()`,  `gamma()`,  `sign()`,   `sqrt()`

###	Trigonometric functions
See Table \@ref(tab:TrigFunc).

Table: (\#tab:TrigFunc) Trigonometric functions.

| *<span style="color:#CC99FF">Operator</span>* | *<span style="color:#CC99FF">Function</span>*  | | *<span style="color:#CC99FF">Operator</span>* | *<span style="color:#CC99FF">Function</span>* |
| ------ | --------------- |------ | --------------- |
| `cos()`  | cosine             | `acos()`  | arc cosine  |
| `sin()`  | sine               | `asin()`  | arc sine    |
| `tan()`  | tangent            | `atan()`  | arc tangent |
| `cosh()` | hyperbolic cosine  | `acosh()` | arc hyperbolic cosine  |
| `sinh()` | hyperbolic sine    | `asinh()` | arc hyperbolic sine    |
| `tanh()` | hyperbolic tangent | `atanh()` | arc hyperbolic tangent |

###	Complex numbers
`Arg()`,  `Conj()`, `Mod()`, `Re()`, `Im()`

###	Functions for rounding and truncating
`round()`,  `ceiling()`,  `floor()`,  `trunc()`

Study the help files of the above functions. Check all arguments.

###	Functions for matrices
Study Table \@ref(tab:MatrixFunc) in detail.

Two other functions that play an important role in matrix calculations are the functions `rbind()` and `cbind()` for concatenating matrices row-wise or column-wise. Also revise the functions  `matrix()`, `dim()`,  `dimnames()`, `colnames()`, `rownames()` as well as `scan()` and `read.table()`.

Table: (\#tab:MatrixFunc) Functions for matrices.

| *<span style="color:#CC99FF">Function</span>* | *<span style="color:#CC99FF">What it does</span>*  | 
| ------ | --------------- |
| `chol()`  | Cholesky decomposition       | 
| `crossprod()`  | Matrix crossproduct     | 
| `diag()`  | Create identity matrix, diagonal matrix or extract diagonal elements depending on its argument  | 
| `eigen()`  | Finding eigenvectors and eigenvalues  | 
| `kronecker()`  | Computing the kronecker product of two matrices  | 
| `outer()`  | Outer product of two vectors  | 
| `scale()`  | Centring and scaling a data matrix  | 
| `solve()`  | Finding the inverse of a nonsingular matrix | 
| `svd()`  | Singular value decomposition of a rectangular matrix | 
| `qr()`  | QR orthogonalization | 
| `t()`  | Transpose of a matrix | 

(a) The function `chol()` performs a Cholesky decomposition of the square, symmetric, positive definite matrix $\mathbf{A}=\mathbf{U}'\mathbf{U}$ where $\mathbf{U}$ is an upper triangular matrix.

(b)	The function `crossprod (A, B)` returns the matrix $\mathbf{A'B}$.

(c)	The function `diag(arg)` performs various actions depending on its argument: if `arg` is a positive integer `diag(arg)` returns an identity matrix of the given size; if `arg` is a vector `diag(arg)` returns a diagonal matrix with diagonal elements the respective elements of the given vector; if `arg` is a matrix then `diag(arg)` returns a vector containing the diagonal elements of the given matrix.

(d)	What is the difference between `diag(A)` and `diag(diag(A))` where `A` is a square matrix?

(e)	The function `eigen()` operates on a square matrix and returns a list with named elements `values` and `vectors` containing respectively, the eigenvalues and eigenvectors. Study the help file of `eigen()` carefully.

(f)	The function `kronecker()` returns the Kronecker product $\mathbf{A} \otimes \mathbf{B}$ of matrices $\mathbf{A}$ and $\mathbf{B}$.

(g)	The function `outer (x, y, f)` operates on two vectors $x:n\times 1$ and $y:p\times 1$ to return a matrix of size $n \times p$ with $ij$th element the result of applying the function `f` on `x[i]` and `y[j]`. The default for `f` is `*`.

(h)	The function `scale()` has three arguments: a matrix as first argument; a second argument `center` and a third argument `scale`. If `center = FALSE`, no centring of the columns of the matrix argument is performed, if set to `TRUE` (the default), the mean value of each column is subtracted from the respective columns, if given a vector of values these values are subtracted from the respective columns. If `scale = FALSE`, no scaling of the columns of the matrix argument is performed, if set to `TRUE` (the default) each column is divided by its standard deviation, if given a vector of values then each column is divided by the corresponding value.

(i)	The function `solve (A, b)` is used for solving the equation $\mathbf{Ax=b}$ for $\mathbf{x}$, where $\mathbf{b}$ can be either a vector or a matrix with $\mathbf{A}$ being a square matrix. If argument `b` is missing it is taken to be the identity matrix so that the inverse of argument `A` is returned.

(j)	The function `svd()` returns the singular value decomposition of its matrix argument $\mathbf{A=UDV}'$. It returns a list with three components: `u` the orthogonal or orthonormal matrix $\mathbf{U}$; `d` the vector containing the ordered singular values of the rectangular matrix $\mathbf{A}$; `v` the orthogonal or orthonormal matrix $\mathbf{V}.

(k)	The function `qr()` performs a QR decomposition of any arbitrary matrix $\mathbf{M=QR}$ with $\mathbf{Q}$ and orthogonal matrix and $\mathbf{R}$ an upper triangular matrix. Study the help file of `qr()` for full details and usages of the function. Note that the matrices $\mathbf{Q}$ and $\mathbf{R}$ can be obtained directly by calling `qr.Q(qr())` and `qr.R(qr())`, respectively.

::: {style="color: #80CC99;"}

(l)	What is the meaning of each of the following instructions?

<div style="margin-left: 25px; margin-right: 20px;">
`rbind(a,b)`; `rbind(1,x)`; `rbind(a = 1:5,b = 10:14,c=20:24)`; `cbind( a= 1:5, b=10:14, c=20:24)`
</div>

(m)	Write a function to calculate the determinant of a square matrix. Name this function `det.own()` in order to distinguish it from the built in R function `det()`.

(n)	When the user is satisfied with a function, it is often necessary to have it available for all R projects. It is useful to assign all such functions to the same data base or folder. Use the function `assign (x, object, pos = , envir = )` to store the function `det.own()` in your own R functions folder. The argument `x` in `assign()` is a character string for assigning a name to the object. The function `remove (list of objects names, pos = , envir = )` can be used to remove objects from your own or any other database. *Hint*: First create a file and then use `attach()` to add it to the R search path.


``` r
save(file= " C:\\MyFunctions").  
```

<div style="margin-left: 25px; margin-right: 20px;">
Study how `save()` works.
</div>


``` r
attach("C:\\MyFunctions", pos=2). 
```

<div style="margin-left: 25px; margin-right: 20px;">
Study how `attach()` works.
</div>


``` r
assign("det.own", det.own, pos=2). 
```

<div style="margin-left: 25px; margin-right: 20px;">
Study how `assign()` works.
</div>


``` r
save(list=objects(2), file = "C:\\MyFunctions")
```

<div style="margin-left: 25px; margin-right: 20px;">
Explain the use of the argument `list=objects(2)`. To summarize: The construction `NAME <- object` is a simple way to assign an object to a name. This form of assignment always takes place in the global environment (the workspace).  Assignment can also be performed using the functions `save()` and `assign()` as illustrated above. The latter form of assignment is more complicated but the assignment is not restricted to the global environment.
</div>

(o)	The result of the function `gamma(x)` is $(x-1)!$ if $x$ is a non-negative whole number. Now write a function `fact()` to calculate $x!$. This function must make provision for $0!$ as well as for a negative number or a fraction that is read in by mistake. *Hint*: First study the usage of the if statement by requesting help `?Control`, recall Table \@ref(tab:HelpQueries). Store this function in your folder of R functions. How will you go about to make `fact()` and `det.own()` available for any R project?

(p)	The function `lgamma(x)` returns the logarithms of $\Gamma(x)$. Write a function to calculate the value of $f(n) = \frac{\Gamma(\frac{n-1}{2})}{\Gamma(\frac{1}{2})\Gamma(\frac{n-2}{2})}$. Calculate the value of $f(n)$ for $n = -10, 10, 100, 500, 1000$.

:::

###	Sorting functions
Note the use of the functions `sort()`, `order()` and `rank()`. First construct `MatX`  using the functions `scan()` and  `matrix()`.  Explain in detail what `order()` does by sorting all the columns of `MatX`  according to the values in the first column of the matrix.

$$
MatX = \begin{bmatrix}
         4 & 80 & 12\\
         5 & 70 & 70\\
         6 & 30 & 19\\
         2 & 40 & 80\\
         4 & 90 & 40\\
         1 & 60 & 50\\
         7 & 10 & 20\\
         3 & 30 & 200
       \end{bmatrix}
$$

### Some functions for data manipulation

Study the functions in Table \@ref(tab:DataManipulation).

Table: (\#tab:DataManipulation) Functions for data manipulation.

| *<span style="color:#CC99FF">Function</span>* | *<span style="color:#CC99FF">What it does</span>*  | 
| ------ | --------------- |
| `append()`     | Combine vectors; more flexibility than `c()`  | 
| `c()`          | Create vectors  | 
| `duplicated()` | Extract duplicated values  | 
| `match()`      | Match values in pairs of vectors  | 
| `pmatch()`     | Partial matching  | 
| `replace()`    | Replace specified values in vectors  | 
| `unique()`     | Extract unique values  | 

::: {style="color: #80CC99;"}

(a)	Insert the vector (101, 102, 103, 104, 105) into the vector (10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20) after its fifth element by utilising the argument `after` of the function `append()`.

(b)	The function `replace()` requires three arguments `x`, `list` and `vals`. The values in `x` with indices given in `list` is replaced by the successive values in `vals` making use of the recycling principle if needed. Explain this by replacing in the vector (10, 2, 7, 20, 5, 8, 9, 20, 9, 1,1 15), the values 10, 20 and 15 with zeros.

(c)	Find the unique values in the vector (10, 2, 7, 20, 5, 8, 9, 20, 9, 1, 15).

(d)	Find the duplicated values in the vector (10, 2, 7, 20, 5, 8, 9, 20, 9, 1, 15, 20, 20, 15).

(e)	Explain the usage of `match()` by considering the difference between


``` r
match (c(10,2,7,20,5,8,9,20,9,1,15), c(10,20,15))
#>  [1]  1 NA NA  2 NA NA NA  2 NA NA  3
```

``` r
match (c(10,20,15), c(10,2,7,20,5,8,9,20,9,1,15))
#> [1]  1  4 11
```

(f)	Illustrate the difference between `match()` and `pmatch()` by considering the names of the days of the week.

:::

###	Basic statistical functions

Study the functions in detail in  Table \@ref(tab:StatFunc).

Table: (\#tab:StatFunc) Basic statistical functions.

| *<span style="color:#CC99FF">Function</span>* | *<span style="color:#CC99FF">What it does</span>*  | *<span style="color:#CC99FF">Comments</span>*  | 
| ------ | --------------- | --------------- |
| `cor()`      | Correlation  | One or two arguments |
| `cumsum()`   | Cumulative sum of elements of a vector |   |
| `mean()`     | Arithmetic mean | Optional argument `trim =`  |
| `median()`   | Median | Accepts variable number of arguments  |
| `min()`      | Minimum value | Accepts variable number of arguments  |
| `max()`      | Maximum value | Accepts variable number of arguments  |
| `prod()`     | Product of elements of a vector | Accepts variable number of arguments  |
| `cumprod()`  | Cumulative product of elements of a vector |    |
| `quantile()` | Returns specified quantiles |    |
| `range()`    | Minimum and maximum of a vector |  Accepts variable number of arguments  |
| `sample()`   | Random sample |  With or without replacement  |
| `sum()`      | Arithmetic sum |  Also used for counting  |
| `var()`      | Variance and covariance; uses n-1 as denominator |  Accepts vectors or matrices  |
| `sd()`       | Standard deviation; uses n-1 as denominator |  Accept a vector as argument  |

Note also the functions `pmax()` and `pmin()`.

::: {style="color: #80CC99;"}

(a)	Find the average Life Expectancy of the states in the `state.x77` data set.
(b)	Find the 5% trimmed mean for Illiteracy of the states in the `state.x77` data set.
(c)	Find the correlation between the Illiteracy and the `Income` of the states in the `state.x77` data set.
(d)	Find the covariance matrix of all the variables in the `state.x77` data set.
(e)	Find the range for Murder in the `state.x77` data set.
(f)	Obtain the details of a random sample of 10 states in the `state.x77` data set.
(g)	Obtain two independent random permutations of the numbers $1, 2, \dots, 10$.
(h)	Write a function for computing the coefficient of kurtosis for a random sample. Test your function on the Frost variable in the `state.x77` data set.
(i)	Write a function for computing the coefficient of skewness for a random sample. Test your function on the Murder variable in the `state.x77` data set.
(j)	Write a function to compute the harmonic mean of a numeric vector. Test your function on the Life Expectancy of the states in the `state.x77` data set. Compare your answer to your answer in (a).

:::

###	Probability distributions in R

First, execute the R-instruction


``` r
help.search("distribution")
```

to obtain a list of available statistical distributions in R.  Each distribution has an identifying name preceded by one of the letters *<span style="color:#FF9966">d</span>*, *<span style="color:#FF9966">p</span>*, *<span style="color:#FF9966">q</span>* or *<span style="color:#FF9966">r</span>*.  In the case of an F-distribution, for example, the identifier is just the letter `f` and for a normal distribution the identifier is `norm`.  Preceding the distribution’s identifier by one of the letters `d`, `p`, `q` or `r` returns a density value, a probability, a quantile or a random sample for the specified distribution (probability density function or probability mass function). See Figure \@ref(fig:Fdist) for an explanation. 


<div class="figure">
<img src="pics/F-distribution.png" alt="Meaning of the letters d, p and q when preceding an R distribution identifier." width="100%" />
<p class="caption">(\#fig:Fdist)Meaning of the letters d, p and q when preceding an R distribution identifier.</p>
</div>

### Functions for categorical variables { #areagrp }

Apart from being *<span style="color:#FF9966">numeric</span>* or *<span style="color:#FF9966">logical</span>*, data in R can also be *<span style="color:#FF9966">categorical</span>* (*<span style="color:#FF9966">factor</span>* in R) or character strings. Study in detail the functions operating on factor data in Table \@ref(tab:CatFunc).

(a)	Use `cut()` to create an object `areagrp` to divide the `state.x77` data set into three groups representing the states with area within the intervals $(0, 10 000]$,$(10 000, 100 000]$ and $(100 000, Inf]$, respectively. *Hint*: First study the arguments of `cut()`.

(b)	Repeat (a) with argument `labels = ??` to specify each state as being *Small*, *Medium* or *Large* with respect to its area.

(c)	Use `unclass()` to obtain the numeric codes associated with each level of `areagrp`.

(d)	Repeat (a) to obtain `areagrp2` containing five equally spaced categories.

(e)	Repeat (a) to obtain `areagrp3` containing five groups with each containing $20\%$ of the data.

(f)	Use `cut()` to create an object `illitgrp` to divide the `state.x77` data set into five groups representing the states with illiteracy within the interval $[0, 0.50)$, $[0.50, 1.00)$, $[1.00, 1.50)$, $[1.50, 2.00)$ and $[2.00, 5.00)$, respectively.

(g)	Obtain a two-way table of the `state.x77` data set according to `areagrp` and `illitgrp`.

Table: (\#tab:CatFunc) Basic functions for categorical variables.

| *<span style="color:#CC99FF">Function</span>* | *<span style="color:#CC99FF">What it does</span>*  |
| ------ | --------------- |
| `cut()`      | Creates categories out of a continuous variable |
| `factor()`   | Encodes a vector as a **_nominal_** categorical variable  |
| `ordered()`  | Encodes a vector as a **_ordinal_** categorical variable when argument ordered is set to TRUE  |
| `levels()`   | Displays or sets the levels of  a factor variable  |
| `pretty()`   | Creates convenient break points for a categorical variable  |
| `split()`    | Breaks up an array according to the value of a categorical variable  |
| `table()`    | Counts the number of observations cross-classified by categories  |
| `unclass()` | Returns the numeric codes for representing the levels of a factor variable  |


### Functions for character manipulation { #character }

Study the functions in Table \@ref(tab:CharFunc) in detail.

Table: (\#tab:CharFunc) Basic functions for character manipulation.

| *<span style="color:#CC99FF">Function</span>* | *<span style="color:#CC99FF">What it does</span>*  |
| ------ | --------------- |
| `abbreviate()` | Generates abbreviations of character values |
| `cat()`        | Display,messages and/or values on screen or send to file  |
| `grep()`       | Search for patterns in characters |
| `nchar()`      | Number of characters in a string |
| `paste()`      | Combine values into character strings |
| `strsplit()`   | Split the elements of a character vector $\times$ into substrings |
| `substring()`  | Extracts parts of character strings |

(a)	What is the returned value of `grep ("ia", state.name)`?

(b)	Discuss the usage of `grep ("ia", state.name)`.

(c)	Discuss the output of `objects (pos = grep("stats", search()))`.

(d)	Use `paste()` to create variable names: var1, var2, …, var100.

(e)	Repeat (d) to create variable names: var_1, var_2, …, var_100.

(f)	Discuss the output of:


``` default
substring (paste (letters, collapse = ""),  
             1:nchar (paste (letters, collapse="")), 
             1:nchar (paste (letters, collapse="")))
```

(g)	From the Help menu, select Manuals (in PDF) and open the Introduction to R document. Obtain a copy of the first two paragraphs of the Preface on page 1 of this book in the R commands window. Use this copy to calculate the number of words as well as the total number of characters (including spaces between words) in the passage.

<div style="margin-left: 25px; margin-right: 20px;">
We are going to use several of the functions in Table \@ref(tab:CharFunc) to perform this task in steps. Proceed as follows in R after copying the relevant passage to the clipboard:


``` r
TextPar <- scan(file = "clipboard", what = "")
```
To obtain  a vector containing each of the words as a separate element.


``` r
TextPar <- paste (TextPar, collapse = " ")
```
To convert `TextPar` into a vector containing one element consisting of all the words concatenated and separated by spaces into a single character string. Add the correct line breaks ("\\n") in `TextPar` using e.g. `fix()`.


``` r
TextPar <- strsplit(x = TextPar, split = '\n')
```


``` default
mode(TextPar)
[1] "list"

mode(unlist(TextPar))
[1] "character" 
```


``` r
TextPar <- unlist(TextPar)
```
To change `TextPar` into a character vector.


``` r
nchar(TextPar)
length(TextPar)
```

</div>

##	Differentiation and integration

###	Symbolic differentiation

Study the help files of `D()` and `deriv()`.

###	Integration

Study the help file of `integrate()`.

### Exercise

::: {style="color: #80CC99;"}

(1)	It is known from elementary statistics that approximately 68% of data from a normal distribution with a mean of zero and a standard deviation of unity will have an absolute value less than unity. Use the `sum()` and `rnorm()` functions to find the proportion of $n$ random $normal (0, 1)$ variables whose absolute value is less than $1.0$. Repeat with different values for $n$ to investigate how widely the results vary.

(2)	Define: conditional inverse and generalized (Moore-Penrose) inverse for  matrix $\mathbf{X}: p \times q$ and make provision for $p = q$,  $p > q$ and $p < q$.  First, show how the svd of $\mathbf{X}$ can be used to obtain a conditional inverse, $\mathbf{X}^c$ for $\mathbf{X}$. Now use the above information to write an R function for calculating $\mathbf{X}^c$ for any given $\mathbf{X}$. The function must provide a test to check if the calculated conditional inverse is indeed a conditional inverse. Illustrate the usage of your function. 

(3)	Give the necessary instructions to:
	  (i) read into R an external text data file consisting of $10$ sample observations with each consisting of one character variable and two numerical variables.
	  (ii) read into R a large external text data file consisting of $50$ numerical variables but unknown number of records. Each record in this data file takes up 5 lines.  The variables in the R object must have the names X1, ..., X50.
	  
(4)	Discuss the meaning of the following R instructions:
    (i) `y <- x[!is.na(x)]`
    (ii) `z <- (x + y)[!is.na(x) & x >0]`
    (iii) `a <- x[-(1:5)]`
    (iv) `x[is.na(x)] <- 0`

:::
