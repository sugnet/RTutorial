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

| *<span style="color:#F7CE21">Operator</span>* | *<span style="color:#F7CE21">Function</span>* | *<span style="color:#F7CE21">Operator</span>* | *<span style="color:#F7CE21">Function</span>* |
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
  
Logical operators result in `TRUE`, `FALSE` or `NA`. Study the use of the logical operators in Table \@ref(tab:LogichOperators).  *<span style="color:#FF9966">Warning</span>*:  While it is perfectly legitimate to write


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

| *<span style="color:#F7CE21">Operator</span>* | *<span style="color:#F7CE21">Function</span>*  |
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

| *<span style="color:#F7CE21">Operator</span>* | *<span style="color:#F7CE21">What it does</span>*  |
| ------ | --------------- | ------ | --------------- |
| `>`   |	Greater than |




### Character vectors { #character }
  
  
