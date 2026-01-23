# Statistical modelling with R {#modelling}

This chapter is an introduction to statistical modelling with R. There are a substantial number of dedicated R modelling packages available. In this chapter only the basics of statistical modelling with R are considered. However, a thorough understanding of these principles is an invaluable aid in mastering any of the available R packages on statistical modelling and related topics.

## Introduction

Some of the aims of statistical modelling are: assessing the relative importance of a set of variables in relation to another variable(s); predicting the value of a dependent variable from those of a set of predictor variables; defining a statistical relationship between the dependent and the independent variables.

Here the following generic functions are considered.

Table: (\#tab:ModelFuncs) Generic modelling functions in R.

| *<span style="color:#F7CE21">Function</span>* | *<span style="color:#F7CE21">Model</span>*  | *<span style="color:#F7CE21">Package</span>* |
| ------ | --------------- | ------ |
| `lm()`   |	Linear models | `stats` |
| `aov()`   |	Analysis of variance | `stats` |
| `glm()`   |	Generalized linear models | `stats` |
| `gam()`   |	Generalized additive models| `gam`, `mgcv` |
| `loess()`   |	Local regression models | `stats` |
| `rpart()`   |	Tree-based models | `rpart` |
| `nls()`   |	Nonlinear models | `stats` |

## Data for statistical models

(a) Note the difference between *<span style="color:#FF9966">factors</span>* (categorical or classification variables), *<span style="color:#FF9966">regressors</span>* (continuous or *<span style="color:#FF9966">numeric</span>* variables) and frequency data.

(b) Why must the data be in the form of a dataframe when statistical modelling is carried out with R?

(c)	How are the functions `factor()`, `is.factor()` and `is.ordered()` used to set or determine the class attributes of variables that must behave as factors in further analyses? Note especially the usage of argument `'ordered = '` of function `factor()`.

## Expressing a statistical model in R

(a)	What role does the tilde-operator (`~`) play in statistical models in R?

(b)	Study how the operators in Table \@ref(tab:ModelOperators) work.  These operators have a different meaning when they appear in a statistical model.  

Table: (\#tab:ModelOperators)  Operators used in model formulae after the `~`.

| *<span style="color:#F7CE21">Operator</span>* | *<span style="color:#F7CE21">Algebraic meaning</span>*  | *<span style="color:#F7CE21">Meaning in formula after `~`</span>* |
| ------ | --------------- | --------------- |
| `+`    |	Addition       | Add a term |
| `-`    |	Subtraction    | Remove or exclude a term |
| `*`    |  Multiplication | 	Main effects and interactions |
| `/`    |	Division       | Main effect and nesting |
| `:`    |	Sequence       | Interaction |
| `^`    |	Exponentiation | Limit depth of interactions |
| `%in%` |	Matching       | Nesting |

(c)	In order to ensure that the operators in Table \@ref(tab:ModelOperators) have their usual meaning when appearing at the right-hand side of `~` in a statistical model use the `I()` function e.g. `I(a+b)`.

(d)	Note how *<span style="color:#FF9966">regression terms</span>*, *<span style="color:#FF9966">main effects</span>*, *<span style="color:#FF9966">interaction effects</span>*, *<span style="color:#FF9966">higher-order interaction effect</span>*, *<span style="color:#FF9966">nested effects</span>* and *<span style="color:#FF9966">covariate terms</span>* are specified.

(e)	The following summary illustrates the correspondence between the *<span style="color:#FF9966">model notation</span>* of R and the *<span style="color:#FF9966">algebraic notation</span>* of models for factors `a` and `b` with numeric (continuous) variable `x`:

| Model notation  | Algebraic notation  | 
| --------------- | --------------- | 
| `y~a+x`     |	$y = \mu + \alpha_a + \beta x + \epsilon$      | 
| `y~a+b+a:b` |	$y = \mu + \alpha_a + \beta_b + \gamma_{ab} + \epsilon$  | 
| `y~a+a:x`   |	$y = \mu + \alpha_a + \beta_a x + \epsilon$  | 
| `y~a*x`     |	$y = \mu + \alpha_a + \beta x + \gamma_a x + \epsilon$   | 
| `y~-1+a/x`  |	$y = \alpha_a + \beta_a x + \epsilon$    | 

(f)	Every numeric variable on the right-hand side of `~` generates one coefficient to be estimated in a fitted model; each *<span style="color:#FF9966">level</span>* of a factor variable generates one coefficient to be estimated in a fitted model.  What is an *<span style="color:#FF9966">estimable function</span>*?

(g)	Note how transformation of variables can be achieved and how model formulae are stored as R objects of class *<span style="color:#FF9966">formula</span>*.

## Common arguments to R modelling functions

(a)	The *<span style="color:#FF9966">formula</span>* argument.  Study how this argument and the period operator work.  If a dataframe is specified as an argument to the modelling function a period on the right-hand side of the `~` indicates that the additive effects of all the variables in the dataframe (except those used at the left-hand side of the `~`) are included in the model specification.

(b)	The *<span style="color:#FF9966">data</span>* argument.  Study how this argument works.  Note that this argument will accept any R expression that evaluates to a dataframe. Important: if a dataframe is specified in the data argument (i) variables may be referred to by their names; (ii) the dataframe is searched first for a name before the global environment and the rest of the search path; (iii) any other existing object can also be used as a variable. 

(c)	The *<span style="color:#FF9966">subset</span>* argument.  Note how statistical models can be fitted to subsets of data.

(d)	The *<span style="color:#FF9966">weights</span>* argument. Study how this argument works especially when iterative procedures are used.

(e)	The *<span style="color:#FF9966">na.action</span>* argument.  This argument controls how missing values are handled.

(f)	The *<span style="color:#FF9966">control</span>* argument.  What is the purpose of this argument?

## Using the statistical modelling objects

*Note*: each of the modelling functions returns an object whose class attribute has been set to the name of the function e.g. the value of `lm()` is set to be of class `lm`. A set of generic functions are available to provide access to the information contained in them.

To see what is available for a particular modelling function use `methods (class = function)` e.g.


``` r
methods (class = lm)
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

Table: (\#tab:ModelObjectFuncs)  Functions for Model Objects.

| *<span style="color:#F7CE21">Function</span>* | *<span style="color:#F7CE21">Description</span>*  |
| ------ | --------------- | 
| `add1()`        |	Add all possible single terms in a model |
| `anova()`       |	Return a sequential analysis of variance table or a comparison of two hierarchical models | 
| `coef()` or `coefficients()` |	Extract estimated coefficients |
| `deviance()`    |	Return the deviance |
| `df.residual()` |	Return the residual degrees of freedom |
| `drop1()`       |	Drop all possible single terms in a model |
| `fitted()` or `fitted.values() ` |	Extract fitted values |
| `formula()`     |	Extract the formula on which the model is based |
| `influence()`   |	Extract several types of influence measures |
| `plot()`        |	Construct relevant plots |
| `predict()`     |	Calculate predictions (means) optionally with standard errors |
| `print()`       |	Extract information about the analysis |
| `resid()` or `residuals()`  |	Extract residuals |
| `step()`        |	Stepwise selection of a model using a AIC criterion |
| `summary()`     |	Extract information about the analysis |
| `update()`      |	Modify and/or refit models |

*Note*:  Do not confuse function `anova()` which operates on an `lm` object with function `aov()` for fitting an analysis of variance model.

*Recall*: Asterisked  functions are non-visible.  Such functions can be accessed using for example


``` r
getAnywhere(add1.lm)
```

(a)	Note the use of `print()`, `summary()` and `plot()` to retrieve information about model objects.

(b)	Study the functions that are used to retrieve information about model objects or to modify them as summed up in Table \@ref(tab:ModelObjectFuncs).

## Usage of the function `with()`

Study the help file of function `with()` and take note specifically of its usage when calling `lm()`. 

## Linear regression and anova {#lmAnova}

(a) Consider the `Cars93` data set available in package `MASS`.  Use the `lm()` function to perform a regression analysis of `MPG.city` as a function of  a constant term,  `Length`, `(Rev.per.mile)`$^{–1}$, `Weight`, `RPM`  and `(Horsepower)`${–1}$.  Note how the function `update()` works. Illustrate the use of the functions `drop1()` and `add1()`:


``` r
library (MASS)
lm.city <- lm (MPG.city ~ 1+ Length + I(1/Rev.per.mile)+ Weight + RPM +
                 I(1/Horsepower), data=Cars93)
summary (lm.city)
#> 
#> Call:
#> lm(formula = MPG.city ~ 1 + Length + I(1/Rev.per.mile) + Weight + 
#>     RPM + I(1/Horsepower), data = Cars93)
#> 
#> Residuals:
#>     Min      1Q  Median      3Q     Max 
#> -5.2344 -1.4863 -0.1570  0.8975 14.0030 
#> 
#> Coefficients:
#>                     Estimate Std. Error t value Pr(>|t|)
#> (Intercept)       -2.913e+00  1.076e+01  -0.271 0.787288
#> Length             4.115e-02  3.393e-02   1.213 0.228518
#> I(1/Rev.per.mile)  7.439e+03  4.760e+03   1.563 0.121750
#> Weight            -3.345e-03  1.221e-03  -2.740 0.007448
#> RPM                2.750e-03  7.812e-04   3.521 0.000688
#> I(1/Horsepower)    1.287e+03  2.379e+02   5.410 5.48e-07
#>                      
#> (Intercept)          
#> Length               
#> I(1/Rev.per.mile)    
#> Weight            ** 
#> RPM               ***
#> I(1/Horsepower)   ***
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> Residual standard error: 2.677 on 87 degrees of freedom
#> Multiple R-squared:  0.7855,	Adjusted R-squared:  0.7732 
#> F-statistic: 63.72 on 5 and 87 DF,  p-value: < 2.2e-16
# update() is used here to restrict the model fitted in lm.city to cars with 
# front wheel drives
lm.city.front <- update (lm.city, subset = DriveTrain=="Front")
summary (lm.city.front)
#> 
#> Call:
#> lm(formula = MPG.city ~ 1 + Length + I(1/Rev.per.mile) + Weight + 
#>     RPM + I(1/Horsepower), data = Cars93, subset = DriveTrain == 
#>     "Front")
#> 
#> Residuals:
#>     Min      1Q  Median      3Q     Max 
#> -5.6598 -1.4476  0.0102  0.9468 13.7023 
#> 
#> Coefficients:
#>                     Estimate Std. Error t value Pr(>|t|)
#> (Intercept)        1.599e+00  1.418e+01   0.113 0.910571
#> Length             2.711e-02  4.875e-02   0.556 0.580093
#> I(1/Rev.per.mile)  7.895e+03  6.518e+03   1.211 0.230462
#> Weight            -3.808e-03  1.707e-03  -2.230 0.029406
#> RPM                2.672e-03  1.016e-03   2.630 0.010784
#> I(1/Horsepower)    1.246e+03  3.070e+02   4.058 0.000143
#>                      
#> (Intercept)          
#> Length               
#> I(1/Rev.per.mile)    
#> Weight            *  
#> RPM               *  
#> I(1/Horsepower)   ***
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> Residual standard error: 2.93 on 61 degrees of freedom
#> Multiple R-squared:  0.7656,	Adjusted R-squared:  0.7464 
#> F-statistic: 39.85 on 5 and 61 DF,  p-value: < 2.2e-16

# drop1 (lm object, scope) shows the effect of dropping the Length term from 
# the fitted model
drop1 (lm.city, . ~ Length)
#> Single term deletions
#> 
#> Model:
#> MPG.city ~ 1 + Length + I(1/Rev.per.mile) + Weight + RPM + I(1/Horsepower)
#>        Df Sum of Sq    RSS    AIC
#> <none>              623.27 188.92
#> Length  1    10.536 633.80 188.48

# Shows the effect of dropping each term in turn from the fitted model 
drop1 (lm.city)
#> Single term deletions
#> 
#> Model:
#> MPG.city ~ 1 + Length + I(1/Rev.per.mile) + Weight + RPM + I(1/Horsepower)
#>                   Df Sum of Sq    RSS    AIC
#> <none>                         623.27 188.92
#> Length             1    10.536 633.80 188.48
#> I(1/Rev.per.mile)  1    17.495 640.76 189.50
#> Weight             1    53.796 677.06 194.62
#> RPM                1    88.801 712.07 199.31
#> I(1/Horsepower)    1   209.659 832.93 213.89

lm.city.const <- lm (MPG.city ~ 1, data=Cars93)
anova (lm.city.const)
#> Analysis of Variance Table
#> 
#> Response: MPG.city
#>           Df Sum Sq Mean Sq F value Pr(>F)
#> Residuals 92 2905.6  31.582

# add1 (lm object, scope) shows the effect of adding the terms in scope 
# individually in turn to the model in lm object 
add1 (lm.city.const, . ~ Length + I(1/Rev.per.mile) + Weight + RPM + 
       I(1/Horsepower))
#> Single term additions
#> 
#> Model:
#> MPG.city ~ 1
#>                   Df Sum of Sq     RSS    AIC
#> <none>                         2905.57 322.09
#> Length             1   1289.71 1615.86 269.52
#> I(1/Rev.per.mile)  1   1090.68 1814.89 280.32
#> Weight             1   2065.52  840.05 208.68
#> RPM                1    382.96 2522.61 310.94
#> I(1/Horsepower)    1   1927.30  978.27 222.85
```

(b)	Is the object `a <- as.data.frame(matA)` identical with the object  `a1 <- data.frame(matA)`?

::: {style="color: #80CC99;"}

(c)	Once again, consider the `Cars93` data set: Consider cars manufactured by Buick, Chevrolet, Oldsmobile and Pontiac.  Use the median of `Weight` to create two car weight groups.

    * Construct an interaction plot to study the interaction between `Manufacturer` and `Weight` with respect to the highway fuel consumption. Interpret the graph. 
    * Use the function `aov()` to perform a two-way anova with highway fuel consumption as dependent variable  and  as independent (explanatory) variables weight-group and manufacturer.
    * What do you conclude about the main effects and the interaction effects?
    * Repeat the above using lm().
    
(d)	Study the help file of the `whiteside` data set available in package `MASS`.  Plot the gas consumption as a function of the temperature. Use different plotting characters and colours for the two levels of the factor variable `Insul`.  Add the regression lines of gas consumption on the temperature for the two levels of `Insul` to the graph . What do you conclude? Now test for the parallelism of the two regression lines using `lm()` with terms for `Insul`, `Temp` and `Insul:Temp`. Discuss the results of the analysis.

:::

##	Regression diagnostics

(a)	Study the help file of `lm.influence()` for information on its return values.

(b)	Standardized residuals as well as jackknife residuals where $stdres = \frac{\hat{e}_i}{s\sqrt{1-h_{ii}}}$ and $studres = \frac{y_i-\hat{y}_{(i)}}{\sqrt{var(y_i-\hat{y}_{(i)})}}$ can be obtained with the help of the following functions: `stdres()` and `studres()` available in package `MASS`.  Study the help files of these functions.

(c)	Study how `predict()` and `fitted()` work.

(d)	Stepwise model selection: study the use of `step()`.

(e)	What is returned by the function `dfbetas()`?

(f) Discuss the output of


``` r
step (lm (MPG.highway ~ Length + Width + Weight + EngineSize + RPM + 
            Rev.per.mile, data = Cars93))
#> Start:  AIC=210.55
#> MPG.highway ~ Length + Width + Weight + EngineSize + RPM + Rev.per.mile
#> 
#>                Df Sum of Sq     RSS    AIC
#> - RPM           1      1.58  771.32 208.74
#> - EngineSize    1     10.36  780.10 209.79
#> - Width         1     13.91  783.64 210.22
#> <none>                       769.74 210.55
#> - Rev.per.mile  1     17.66  787.39 210.66
#> - Length        1     50.29  820.02 214.44
#> - Weight        1    687.78 1457.52 267.93
#> 
#> Step:  AIC=208.74
#> MPG.highway ~ Length + Width + Weight + EngineSize + Rev.per.mile
#> 
#>                Df Sum of Sq     RSS    AIC
#> - EngineSize    1      9.19  780.50 207.84
#> - Width         1     12.56  783.87 208.24
#> <none>                       771.32 208.74
#> - Rev.per.mile  1     18.36  789.67 208.93
#> - Length        1     50.39  821.71 212.63
#> - Weight        1    699.81 1471.13 266.79
#> 
#> Step:  AIC=207.84
#> MPG.highway ~ Length + Width + Weight + Rev.per.mile
#> 
#>                Df Sum of Sq     RSS    AIC
#> - Rev.per.mile  1     10.79  791.29 207.12
#> <none>                       780.50 207.84
#> - Width         1     21.50  802.00 208.37
#> - Length        1     56.14  836.64 212.30
#> - Weight        1    710.64 1491.14 266.05
#> 
#> Step:  AIC=207.12
#> MPG.highway ~ Length + Width + Weight
#> 
#>          Df Sum of Sq     RSS    AIC
#> - Width   1     13.71  805.00 206.72
#> <none>                 791.29 207.12
#> - Length  1     52.31  843.61 211.07
#> - Weight  1    749.41 1540.70 267.09
#> 
#> Step:  AIC=206.72
#> MPG.highway ~ Length + Weight
#> 
#>          Df Sum of Sq     RSS    AIC
#> <none>                 805.00 206.72
#> - Length  1     91.62  896.62 214.74
#> - Weight  1   1039.48 1844.48 281.82
#> 
#> Call:
#> lm(formula = MPG.highway ~ Length + Weight, data = Cars93)
#> 
#> Coefficients:
#> (Intercept)       Length       Weight  
#>   37.521781     0.115526    -0.009633
```

(g) Calculate and interpret the `dfbetas` associated with the object `lm.city.front` created in Section \@ref(lmAnova).

(h)	Continuing (f): obtain a graph with the standardized residuals on the y-axis and the fitted y-values on the x-axis and interpret the graph.

(i)	Continuing (f): obtain a histogram of the studentized residuals. Interpret.

(j)	Consider the object `lm.city` created in section  \@ref(lmAnova). Verify the assumption of normally distributed residuals with the function `qqnorm()`. See section  \@ref(qqplot).

## Non-parametric regression

(a)	Inspect the help file of the `stackloss` data set.

(b)	Execute the following R code:


``` r
stack.data <- data.frame (stackloss)
names (stack.data) <- c("AirFlow", "WaterTemp", "AcidConc", "Loss")
```

(c)	Describe briefly what is understood by a Local regression model using the help file of the function `loess()`.

(d)	Execute the following R code:


``` r
stack.loess <- loess(Loss ~ AirFlow * WaterTemp, data = stack.data)
summary(stack.loess)
#> Call:
#> loess(formula = Loss ~ AirFlow * WaterTemp, data = stack.data)
#> 
#> Number of Observations: 21 
#> Equivalent Number of Parameters: 8.86 
#> Residual Standard Error: 2.986 
#> Trace of smoother matrix: 10.53  (exact)
#> 
#> Control settings:
#>   span     :  0.75 
#>   degree   :  2 
#>   family   :  gaussian
#>   surface  :  interpolate	  cell = 0.2
#>   normalize:  TRUE
#>  parametric:  FALSE FALSE
#> drop.square:  FALSE FALSE
names(stack.loess)
#>  [1] "n"         "fitted"    "residuals" "enp"      
#>  [5] "s"         "one.delta" "two.delta" "trace.hat"
#>  [9] "divisor"   "robust"    "pars"      "kd"       
#> [13] "call"      "terms"     "xnames"    "x"        
#> [17] "y"         "weights"

stack.loess$x
#>    AirFlow WaterTemp
#> 1       80        27
#> 2       80        27
#> 3       75        25
#> 4       62        24
#> 5       62        22
#> 6       62        23
#> 7       62        24
#> 8       62        24
#> 9       58        23
#> 10      58        18
#> 11      58        18
#> 12      58        17
#> 13      58        18
#> 14      58        19
#> 15      50        18
#> 16      50        18
#> 17      50        19
#> 18      50        19
#> 19      50        20
#> 20      56        20
#> 21      70        20
coplot (stack.loess$y ~ stack.loess$x[,"AirFlow"] | stack.loess$x[,"WaterTemp"],
       panel = points)
```

<img src="11-modelling_files/figure-html/stackloess-1.png" width="672" />

``` r
coplot (stack.loess$y ~ stack.loess$x[,"WaterTemp"] | stack.loess$x[,"AirFlow"],
        panel=points)
```

<img src="11-modelling_files/figure-html/stackloess-2.png" width="672" />

Comment on the results.

(e)	Alternatively, splines can be used to fit a nonparametric regression model. The package `splines` provides for fitting B-splines with the `bs()` function and natural cubic splines with the `ns()` function.

(f)	Study the help files of the `bs()` and `ns()` functions.

(g)	Inspect the help file of the `ethanol` data set available in package `lattice`.

    (i) Plot `NOx` versus `E`. 

    (ii) Use `lm()` and fit a straight line to the data. Add this line to the plot made above. Why is it necessary to order the observations?
    

``` r
y <- ethanol$NOx [order (ethanol$E)]
x <- ethanol$E [order (ethanol$E)]
yhat <- fitted (lm (y ~ x))
lines (x, yhat, col = "blue")
```

<div style="margin-left: 25px; margin-right: 20px;">
(iii) Use `lm()` and fit a third degree polynomial to the data. Also add this line to the plot in (i).
</div>


``` r
yhat <- fitted (lm (y ~ x + I(x^2) + I(x^3)))
lines (x, yhat, col = "magenta")
```

<div style="margin-left: 25px; margin-right: 20px;">
(iv) Use `loess()` with a default `span = 0.75` to fit a local regression function to the data and add this line to the plot.
</div>


``` r
yhat <- fitted (loess (y ~ x))
lines (x, yhat, col = "green")
```

<div style="margin-left: 25px; margin-right: 20px;">
(v) Use `bs()` to fit a B-spline with default `degree = 3` and `knots = NULL`.  Notice that a third degree polynomial is fitted, similar to (iii).
</div>


``` r
yhat <- fitted (lm (y ~ bs(x, degree=3)))
lines (x, yhat, col = "gold")
```

<div style="margin-left: 25px; margin-right: 20px;">
(vi) Use `bs()` to fit a cubic B-spline with 7 degrees of freedom.
</div>


``` r
yhat <- fitted (lm (y ~ bs(x, df=7)))
lines (x, yhat, col="red")
```

<div style="margin-left: 25px; margin-right: 20px;">
(vii) Compare the number of knots for the two fits in (v) and (vi).
</div>


``` r
attr (bs(x), "knots")
attr(bs(x), "Boundary.knots")
attr(bs(x, df=7), "knots")
attr(bs(x, df=7), "Boundary.knots")
```

<div style="margin-left: 25px; margin-right: 20px;">
(viii) Experiment with different values of the span for the `loess()` function and the `df` argument of the `bs()` function. 
</div>

(h)	When producing a coplot (refer to section \@ref(coplot)) it is helpful to draw lines or curves in each of the dependency panels, to make the relationship easier to appreciate. Use the panel argument of `coplot()` to draw  in each panel

    * a straight line representing the linear regression between the variables;
    * a curve representing the polynomial regression; 
    * a spline using `smooth.spline()`.


<div style="margin-left: 25px; margin-right: 20px;">
*Hint*:  Graphical arguments are passed to `coplot()` by writing a suitable panel function as ‘value’ for argument panel of  `coplot()`. Study the examples of panel functions below:
</div>


``` r
panel.straight.line <- function(x, y, col = par("col"), bg = NA, 
                                pch = par("pch"), cex = 1, 
                                col.smooth = "red")
{ datmat <- cbind (x, y)
  datmat <- datmat [order (datmat[, 1]),  ]
  points (datmat[, 1], datmat[, 2], pch = pch, col = col, bg = bg, 
          cex = cex)
  lines (datmat[, 1], fitted (lm (datmat[, 2] ~ datmat[, 1])), 
         col = col.smooth)
}

panel.poly <- function(x, y, col = par("col"), bg = NA, 
                       pch = par("pch"), cex = 1, 
                       col.smooth = "red")
{ datmat <- cbind(x, y)
  datmat <- datmat [order (datmat[, 1]),  ]
  points (datmat[, 1], datmat[, 2], pch = pch, col = col, 
          bg = bg, cex = cex)
  lines (datmat[, 1], fitted (lm (datmat[, 2] ~ 
                                        poly (datmat[, 1], 2))), 
         col = col.smooth)
}
               
panel.smooth <- function (x, y, col = par("col"), bg = NA, 
                          pch = par("pch"), cex = 1, 
                          col.smooth = "red", span = 2/3, 
                          iter = 3, ...) 
{ points(x, y, pch = pch, col = col, bg = bg, cex = cex)
  ok <- is.finite (x) & is.finite (y)
  if (any (ok)) 
  lines (stats::lowess(x[ok], y[ok], f = span, iter = iter), 
         col = col.smooth, ...)
}
```

<div style="margin-left: 25px; margin-right: 20px;">
An example
</div>


``` r
coplot (Illiteracy ~ Life.Exp | Income * Area, data = state, 
        given = list (co.intervals (state$Income, 4), 
                      co.intervals (state$Area, 4)), 
        panel = panel.straight.line)
coplot (Illiteracy ~ Life.Exp | Income * Area, data = state, 
        given = list (co.intervals (state$Income, 4), 
                      co.intervals (state$Area, 4)), 
        panel = panel.poly)
coplot (Illiteracy ~ Life.Exp | Income * Area, data = state, 
        given = list (co.intervals (state$Income, 4), 
                      co.intervals (state$Area, 4)), 
        panel = panel.smooth)
```

## The function glm()

Consider the following dataframe `treatments`.


```{=html}
<div class="tabwid"><style>.cl-bc9f6fac{}.cl-bc99725a{font-family:'Arial';font-size:11pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(102, 102, 102, 1.00);background-color:transparent;}.cl-bc9c0150{margin:0;text-align:right;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:3pt;padding-top:3pt;padding-left:3pt;padding-right:3pt;line-height: 1;background-color:transparent;}.cl-bc9c1244{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0.75pt solid rgba(102, 102, 102, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-bc9c124e{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-bc9c124f{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0.75pt solid rgba(51, 51, 51, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-bc9f6fac'><thead><tr style="overflow-wrap:break-word;"><th class="cl-bc9c1244"><p class="cl-bc9c0150"><span class="cl-bc99725a">TreatA</span></p></th><th class="cl-bc9c1244"><p class="cl-bc9c0150"><span class="cl-bc99725a">TreatB</span></p></th><th class="cl-bc9c1244"><p class="cl-bc9c0150"><span class="cl-bc99725a">Counts</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">1</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">1</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">18</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">1</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">2</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">17</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">1</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">3</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">15</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">2</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">1</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">20</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">2</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">2</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">10</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">2</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">3</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">20</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">3</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">1</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">3</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">2</span></p></td><td class="cl-bc9c124e"><p class="cl-bc9c0150"><span class="cl-bc99725a">13</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bc9c124f"><p class="cl-bc9c0150"><span class="cl-bc99725a">3</span></p></td><td class="cl-bc9c124f"><p class="cl-bc9c0150"><span class="cl-bc99725a">3</span></p></td><td class="cl-bc9c124f"><p class="cl-bc9c0150"><span class="cl-bc99725a">12</span></p></td></tr></tbody></table></div>
```



## The function gam()

(a)	Describe briefly what is understood by a *<span style="color:#FF9966">generalized additive model</span>*.


``` r
library (gam) 
stack.gam <- gam::gam (Loss ~ s(AirFlow) + s(WaterTemp) + s(AcidConc),
                       control = gam.control (bf.maxit = 50), data = stack.data)
summary(stack.gam)
#> 
#> Call: gam::gam(formula = Loss ~ s(AirFlow) + s(WaterTemp) + s(AcidConc), 
#>     data = stack.data, control = gam.control(bf.maxit = 50))
#> Deviance Residuals:
#>     Min      1Q  Median      3Q     Max 
#> -3.0831 -1.6186  0.2447  0.8839  3.9253 
#> 
#> (Dispersion Parameter for gaussian family taken to be 8.4463)
#> 
#>     Null Deviance: 2069.238 on 20 degrees of freedom
#> Residual Deviance: 67.5711 on 8.0001 degrees of freedom
#> AIC: 112.1371 
#> 
#> Number of Local Scoring Iterations: NA 
#> 
#> Anova for Parametric Effects
#>                  Df  Sum Sq Mean Sq  F value    Pr(>F)    
#> s(AirFlow)   1.0000 1435.95 1435.95 170.0095 1.136e-06 ***
#> s(WaterTemp) 1.0000  272.77  272.77  32.2944 0.0004635 ***
#> s(AcidConc)  1.0000    8.23    8.23   0.9742 0.3525476    
#> Residuals    8.0001   67.57    8.45                       
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> Anova for Nonparametric Effects
#>              Npar Df Npar F   Pr(F)  
#> (Intercept)                          
#> s(AirFlow)         3 1.0050 0.43916  
#> s(WaterTemp)       3 3.3321 0.07703 .
#> s(AcidConc)        3 0.9953 0.44293  
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
par(mfrow=c(3,1))
plot(stack.gam)
```

<img src="11-modelling_files/figure-html/gam-1.png" width="672" />

<div style="margin-left: 25px; margin-right: 20px;">
Comment on the results.
</div>

(b)	Return to the `ethanol` data set available in package `lattice`.

    (i)	Use `gam()` in package `mgcv` and fit a B-spline to the data.  The following code can be used for that:  
    

``` r
detach ('package:gam')
library (mgcv)
library (lattice)
library (splines)

out <- mgcv::gam (NOx ~ bs(E, df = 7), data = ethanol)
# str(out)    # inspect output components

# data must be sorted according to E values
data.temp <- data.frame (ethanol [,c(1,3)], fitted = fitted(out))
data.ordered <- with (data.temp, data.temp [order(E),])
with (data.ordered, plot (x = E, y = NOx, pch = 15, col = 'blue'))
with (data.ordered, lines (x = E, y = fitted, col = 'red', lwd = 2))
```

<img src="11-modelling_files/figure-html/mgcv_gam-1.png" width="672" />

<div style="margin-left: 45px; margin-right: 20px;">
The domain of the predictor is divided by knots and a B-spline generates a set of piecewise polynomials restricted to be continuous at the knots. Execute the following command:
</div>


``` r
plot (range (ethanol$E), c(0,1), type = "n")
apply (bs (ethanol$E, df = 7), 2, function(y, x)
                              lines (x[order(x)], y[order(x)]), x = ethanol$E)
#> NULL
knot.vec <- attr (bs (ethanol$E [order (ethanol$E)], df = 7), "knots")
for (i in knot.vec) lines (rep(i,2), c(0,1), lty = 2)
```

<div class="figure">
<img src="11-modelling_files/figure-html/knots-1.png" alt="Cubic B-spline basis functions with 4 interior knots." width="100%" />
<p class="caption">(\#fig:knots)Cubic B-spline basis functions with 4 interior knots.</p>
</div>

<div style="margin-left: 45px; margin-right: 20px;">
A set of $7$ basis functions were generated, each a degree $3$ polynomial in an interval with 
$7 - 3 = 4$ interior knots. Experiment with specifying either a different degree for the polynomials, or different numbers of knots. The specific knots can also be specified, e.g. `bs (ethanol$E, knots = c (0.7, 0.9, 1.1))`.
</div>

<div style="margin-left: 25px; margin-right: 20px;">
(ii)	It has been shown that B-splines can exhibit erratic behavior beyond the boundary knots. Repeat the fits and plots above, replacing the B-splines with natural cubic splines with the function `ns()`. Natural cubic splines constrain the basis functions to be linear beyond the boundary knots. As a result, a natural cubic spline with $K$ knots is represented by $K$ basis functions.

(iii)	Selecting the position and number of knots can be subjective. Smoothing splines solves the knot selection problem by making each observation a knot and controlling the overfitting with a single penalty parameter. Repeat the fits and plots in (i), replacing the B-splines with smoothing splines with the function `s()`, similar to the fit in (a).
</div>

## The function rpart()

(a)	Describe briefly what is understood by *<span style="color:#FF9966">Regression and Classification Trees</span>* using the help file of the function `rpart()` in package `rpart`.

(b)	Study the help file of the `kyphosis` data set available in package `MASS`.

(c)	Execute the following R code:


``` r
library (rpart)
fit <- rpart(Kyphosis ~ Age + Number + Start, data=kyphosis)
plot(fit)
text(fit, use.n = TRUE, xpd = NA)
```

<img src="11-modelling_files/figure-html/rpart-1.png" width="672" />

Comment on the results.

(d)	The package `partykit()` provides a toolkit for recursive partitioning. A more informative tree diagram compared to (c) can be obtained with the code:


``` r
library (partykit)
fit2 <- as.party(fit)
plot(fit2)
```

<img src="11-modelling_files/figure-html/partykit-1.png" width="672" />

## Nonlinear regression and the function nls()

The asymptotic regression model has the form: 

$$
y = \alpha + \beta e^{\gamma x}.
$$

When $\alpha > 0$, $\beta < 0$ and $\gamma < 0$ the above model becomes Mistcherlich’s model of the law of diminishing returns.  According to the law of diminishing returns the yield, $y$, initially increases quickly with increasing values of $x$, but then the yield slows down and finally levels off just below the value of $\alpha$. The parameters $\alpha > 0$, $\beta < 0$ and $\gamma < 0$ have the following meaning:

<div style="margin-left: 25px; margin-right: 20px;">
* $\alpha$ represents the upper asymptote for yield

* $\beta$ is the difference between the value of $y$ when $x = 0$ and the upper asymptote
</div>

The asymptotic regression model may be reparameterized such that $\beta$ is replaced by $\delta - \alpha$ where $\delta$ represents the yield when $x = 0$. A special case of the asymptotic regression model occurs when $\delta = 0$:

$$
y = \alpha - \alpha e^{\gamma x} = \alpha (1 - e^{\gamma x}).
$$

The asymptotic regression model represents a nonlinear regression model and in general no closed form solution exists for estimating its parameters. The R function `nls()` can be used to estimate the parameters of the model. When function `nls()` is employed starting values for the iterative estimating procedure are necessary.

### Choosing starting values

(1)	A starting value for \alpha can be found by looking at a scatterplot of $y$ as a function of $x$ and then chooses as starting value just larger than the largest value of $y$.

(2) \beta is the difference between the value of $y$ when $x = 0$ and the upper asymptote. A reasonable starting value is the minimum value of $y - \alpha$.

(3) \gamma can be roughly initially estimated by the negative of the slope between two "well separated" points on the plot.

(a)	Study the help file of `nls()`.  Notice carefully how `nls()` is called.

::: {style="color: #80CC99;"}

(b)	The data below is the `Yield` of a product after a chemical treatment of different time duration. Three replicates were made at each treatment time. 


```{=html}
<div class="tabwid"><style>.cl-bed57e4c{}.cl-becfbf66{font-family:'Arial';font-size:11pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(102, 102, 102, 1.00);background-color:transparent;}.cl-bed1e32c{margin:0;text-align:right;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:3pt;padding-top:3pt;padding-left:3pt;padding-right:3pt;line-height: 1;background-color:transparent;}.cl-bed1f240{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0.75pt solid rgba(102, 102, 102, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-bed1f24a{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-bed1f24b{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0.75pt solid rgba(51, 51, 51, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-bed57e4c'><thead><tr style="overflow-wrap:break-word;"><th class="cl-bed1f240"><p class="cl-bed1e32c"><span class="cl-becfbf66">Time (Hrs)</span></p></th><th class="cl-bed1f240"><p class="cl-bed1e32c"><span class="cl-becfbf66">Rep1</span></p></th><th class="cl-bed1f240"><p class="cl-bed1e32c"><span class="cl-becfbf66">Rep2</span></p></th><th class="cl-bed1f240"><p class="cl-bed1e32c"><span class="cl-becfbf66">Rep3</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.0000</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.0000</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.0000</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">3</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.0846</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.0556</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.0501</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">6</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.1072</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.0604</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.0545</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">12</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.1255</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.0590</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.0705</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">18</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.1671</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.0799</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.0687</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">24</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.1988</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.0958</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.0655</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">48</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.2927</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.1739</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.1075</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">72</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.3713</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.1910</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.1418</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">96</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.4773</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.2669</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.1725</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">168</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.6158</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.3584</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.3224</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">336</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.7297</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.4339</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.3322</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">504</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.8083</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.4816</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.3309</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">672</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.7019</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.4497</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.3798</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">840</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.6038</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.4851</span></p></td><td class="cl-bed1f24a"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.3757</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-bed1f24b"><p class="cl-bed1e32c"><span class="cl-becfbf66">1,008</span></p></td><td class="cl-bed1f24b"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.7386</span></p></td><td class="cl-bed1f24b"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.5332</span></p></td><td class="cl-bed1f24b"><p class="cl-bed1e32c"><span class="cl-becfbf66">0.3798</span></p></td></tr></tbody></table></div>
```



<div style="margin-left: 25px; margin-right: 20px;">
Plot the data.
</div>

(c) The above plot suggests a relationship of the form:

$$
y = \alpha - \alpha e^{\gamma x} = \alpha (1 - e^{\gamma x}).
$$

<div style="margin-left: 25px; margin-right: 20px;">
Explain the term: *<span style="color:#FF9966">nonlinear regression</span>*. Use `nls()` to estimate the parameters $\alpha$ and $\gamma$ in the above model. What effect does the changing of the start values have on the result of `nls()`? Experiment with different start values for the parameters. Add the estimated line to the plot in (b) and interpret the graph.
</div>

:::

## Detailed example: Analysis of Variance and Covariance

### One-way ANOVA models {#AnovaOneWay}

About 90 percent of the ozone in the atmosphere is contained in the stratosphere. Although only a small proportion of atmospheric ozone is found in the troposphere this ozone plays an important role in various climatic phenomena. Research has shown the highest amounts of ozone over the Arctic are found during the spring months of March and April, while the Antarctic has its lowest amounts of ozone during its autumn months of September and October, Atmospheric carbon dioxide not only plays an important role in global warming but its concentration is related to height above sea level and ocean temperature. In order to study relationships between ozone levels, CO_2 concentration, height above sea level, ocean and seasonality a small-scale simulation study has been undertaken. A very simple model was constructed for simulating the data called `climate.sim`. This simple simulation procedure was performed as described below. Here we show how the `climate.sim` data were subjected to various, ANOVA, regression and ANCOVA procedures.


``` r
library (tidyverse)
set.seed (7343)
ocean <- factor (c (rep ("N-Atlantic", 20), rep ("S-Atlantic", 20), 
                    rep ("Indian", 20)))
season <- factor (rep (rep (c ("summer", "autumn", "winter", "spring"), 
                            each = 5), 3))
height <- runif (60, 100, 750)
climate.sim <- tibble (ocean, season, height)
CO2 <- 350 + (as.numeric (ocean) - 2) + (as.numeric (season) - 2.5) * 1.8 + 
       (as.numeric (ocean) - 2) * (as.numeric (season) - 2.5) * 2 -
       0.012 * (height - 390) + rnorm (60, 0, 2)
ozone <- 15 + (as.numeric (ocean) - 2) + 0.02 * (height - 390) + 
         rnorm (60, 0, 0.4)
climate.sim <- mutate (climate.sim, CO2, ozone)
```

The researcher would like to know whether the CO2 levels are the same in the three oceanic regions.  Analysis of variance decomposes the total variance in the dependent variable into a between-groups variance and a within-groups variance (or error variance).  The underlying model can be written as

$$
Y_{ij} = \mu + \alpha_i + \epsilon_{ij}
$$

where $Y_{ij}$ represents the value of the numeric dependent variable for observation $j$ coming from population $i$; $\mu$ is an effect common to all observations; $\alpha_i$ represents an effect found only in population $i$ and $\epsilon_{ij}$ is an effect (e.g. measurement error) that is specific to only observation ($ij$). It is assumed that the $\epsilon_{ij}$ is normally distributed with a mean of zero and a constant variance $\sigma^2$  (i.e. the variance is the same in each of the populations).  The statistical models to be tested is

$$
H_0: \alpha_1 = \alpha_2 = \dots = \alpha_k
$$

where $k$ denotes the number of different populations, versus

$$
H_A: \text{At least one of the } \alpha \text{s differs from the rest}.
$$

In general $\mu, \alpha_1, \dots, \alpha_k$ cannot all be estimated uniquely.  In order for a statistical hypothesis to be *<span style="color:#FF9966">testable</span>* it must be expressed in terms of *<span style="color:#FF9966">estimable functions</span>* of the parameters.  A statistical hypothesis can often be expressed in different equivalent forms. The null hypothesis above, for example, is equivalent to the following hypotheses:

$$
H_0': \mu+\alpha_1 = \mu+\alpha_2 = \dots = \mu+\alpha_k
$$

$$
H_0'': \alpha_1 - \alpha_k = \alpha_2 - \alpha_k = \dots = \alpha_{k-1} - \alpha_k = 0 
$$

The latter two hypotheses are both in terms of  $k - 1$ linearly independent estimable functions and hence are testable. The default behaviour of R is to estimate $\mu$ (the intercept) using the data, take (estimate) one of the $\alpha$s as zero and then estimate the remainder of the $\alpha$s as deviations from the estimated intercept using the data. Notice, that in doing so the given null hypothesis above can still be tested.

(a)	Use `str(climate.sim)` to inspect the structure of the R object `climate.sim`. Note that the factor season has 4 levels, with the first two levels “autumn” and “spring”. R automatically order the levels alphabetically. In this case, we want the seasons to follow in the order we observe them, especially when making plots, such as `boxplot (CO2 ~ season, data = climate.sim)`.

<div style="margin-left: 25px; margin-right: 20px;">
To change the order of the levels, the R function `relevel()` allows putting a specific level first. Sequentially calling this function, allows one to place the levels in any required order. As part of the tidyverse ecosystem, the package `forcats` is loaded and the following calls allows the user to change the order of the levels in a single call.
</div>


``` r
climate.sim$season <- fct_relevel (climate.sim$season, 
                                   c("summer", "autumn", "winter", "spring"))
str(climate.sim)
#> tibble [60 × 5] (S3: tbl_df/tbl/data.frame)
#>  $ ocean : Factor w/ 3 levels "Indian","N-Atlantic",..: 2 2 2 2 2 2 2 2 2 2 ...
#>  $ season: Factor w/ 4 levels "summer","autumn",..: 1 1 1 1 1 2 2 2 2 2 ...
#>  $ height: num [1:60] 534 477 633 256 267 ...
#>  $ CO2   : num [1:60] 350 348 347 352 355 ...
#>  $ ozone : num [1:60] 17.5 17.1 20.4 12.1 11.9 ...
```

(b)	Use `with()` and `tapply()` to obtain the mean and the standard deviation of CO2 for the different seasons.

(c)	Test if the effects of the four seasons on CO2 are the same by executing the R command `lm (CO2 ~ season, data = climate.sim)` and using the commands `summary()` and `anova()`.

(d)	Use `coefficients()` and interpret the estimates referring to (b).

(e)	Obtain the residuals and plot them against the fitted values. Use different colours for the different seasons. Interpret the plot.

### Two-way ANOVA models: main effects and interaction effects

Inspection of the `climate.sim` data by requesting `str(climate.sim)` reveals the presence of a second factor variable ocean.

We are interested in CO2 as a function of the two factor variables season and ocean.  In addition to the *<span style="color:#FF9966">main effects</span>* of season and ocean, respectively, we also have to consider *<span style="color:#FF9966">interaction effects</span>*.  

*<span style="color:#FF9966">Interaction can be defined as the result where the effect of one factor depends on the level of another factor.</span>* 

In order to provide for a second factor together with interaction effects the two-way model is given by

$$
Y_{ij} = \mu + \alpha_i + \beta_j + (\alpha\beta)_{ij} + \epsilon_{ij}
$$

In this model $\alpha$ represents the main effect of season and $\beta$ the main effect of ocean while the interaction effect is represented by $(\alpha\beta)_{ij}$.  The first hypothesis to be tested is the null hypothesis that there exist no interaction effects or that the interaction effects are negligible. Symbolically this is the hypothesis

$$
H_0: (\alpha\beta)_{11} = (\alpha\beta)_{12} = \dots = (\alpha\beta)_{km}
$$

where $k$ denotes the number of levels in the first factor variable and $m$ the number of levels in the second factor variable. In order to test for interaction we must have $r > 1$ repetitions (replicates) of each $\alpha_i\beta_j$ combination. The value of $r$ in our example can be found using the following code:


``` r
with(climate.sim, table(season, ocean))
#>         ocean
#> season   Indian N-Atlantic S-Atlantic
#>   summer      5          5          5
#>   autumn      5          5          5
#>   winter      5          5          5
#>   spring      5          5          5
```

From the table above it is clear that all treatment combinations occur exactly the same number of times. Had there been some missing values in the data, the correct solution to the problem is to estimate the missing values by the mean values of the available data in the affected cells. These values are then substituted in place of the missing values to form a complete data set. The ANOVA is then performed on the complete data set but the degrees of freedom for error is reduced with the number of missing values.

The presence of interaction can be investigated with an interaction plot using the following instruction


``` r
with(climate.sim, interaction.plot (ocean, season, response = CO2)) 
```

<div class="figure">
<img src="11-modelling_files/figure-html/interactionSeasonOcean-1.png" alt="Interaction plot of factors season and ocean associates with the dependent variable CO2" width="100%" />
<p class="caption">(\#fig:interactionSeasonOcean)Interaction plot of factors season and ocean associates with the dependent variable CO2</p>
</div>

What can be deduced from Figure \@ref(fig:interactionSeasonOcean)? Explain in detail.

The formal ANOVA can be performed with the instruction


``` r
two.way.anova <- lm(CO2 ~ season + ocean + season:ocean, data=climate.sim)
```

An analysis-of-variance table can be constructed with the instruction:

``` r
anova(two.way.anova)
#> Analysis of Variance Table
#> 
#> Response: CO2
#>              Df Sum Sq Mean Sq F value   Pr(>F)   
#> season        3 146.93  48.977  4.9041 0.004718 **
#> ocean         2  44.20  22.099  2.2128 0.120437   
#> season:ocean  6 170.41  28.401  2.8439 0.018870 * 
#> Residuals    48 479.37   9.987                    
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

It is clear from the above table that the null hypothesis of no interaction is to be rejected. This means that it would not make sense to test the main effects of season and ocean. On the other hand, if no significant interaction would have been found then the F-values associated with the two main effects could have been investigated to conclude if none, one or both of the main effects are significant.

(a) Repeat the above analysis using in turn each of the other numeric variables as the response variable.

(b) Plot the residuals versus the fitted values and interpret the graphs. What can be concluded regarding the assumptions of the two-way ANOVA model?
 
#### Types of ANOVA tables

Suppose we want to fit an ANOVA model for the effect of `Origin` and `AirBags` on `Max.Price` in the `Cars93` data set in package `MASS`. Notice the difference in the two ANOVA tables below.


``` r
anova (lm (Max.Price ~ AirBags + Origin, data = Cars93))
#> Analysis of Variance Table
#> 
#> Response: Max.Price
#>           Df Sum Sq Mean Sq F value    Pr(>F)    
#> AirBags    2 3462.5 1731.26 20.6885 4.182e-08 ***
#> Origin     1  283.5  283.51  3.3879   0.06901 .  
#> Residuals 89 7447.7   83.68                      
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
anova (lm (Max.Price ~ Origin + AirBags, data = Cars93))
#> Analysis of Variance Table
#> 
#> Response: Max.Price
#>           Df Sum Sq Mean Sq F value    Pr(>F)    
#> Origin     1  160.5  160.46  1.9175    0.1696    
#> AirBags    2 3585.6 1792.78 21.4237 2.539e-08 ***
#> Residuals 89 7447.7   83.68                      
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

R computes sums of squares for each term, based on the assumption that all terms listed above that particular term in the ANOVA table are already included in the model. The effect is that the order in which terms are specified (specifically for unbalanced data) influences the results.

The R package `car` provides options to specify how sums of squares are computed:

* **Type I**: Sums of squares are computed based on the assumption that all terms specified before the particular term is included in the model.

* **Type II**: Sums of squares are computed according to the principle of marginality, testing each term after all others, except ignoring the term's higher-order interactions.

* **Type III**: Sums of squares are computed testing each term in the model after all the others are included in the model. The computation of these sums of squares violate marginality.


``` r
library (car)
anova (lm (Max.Price ~ AirBags * Origin, data = Cars93))   # type = "I"
#> Analysis of Variance Table
#> 
#> Response: Max.Price
#>                Df Sum Sq Mean Sq F value    Pr(>F)    
#> AirBags         2 3462.5 1731.26 21.4712 2.636e-08 ***
#> Origin          1  283.5  283.51  3.5161   0.06413 .  
#> AirBags:Origin  2  432.8  216.38  2.6835   0.07398 .  
#> Residuals      87 7015.0   80.63                      
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
Anova (lm (Max.Price ~ AirBags * Origin, data = Cars93), type = "II")
#> Anova Table (Type II tests)
#> 
#> Response: Max.Price
#>                Sum Sq Df F value    Pr(>F)    
#> AirBags        3585.6  2 22.2342 1.586e-08 ***
#> Origin          283.5  1  3.5161   0.06413 .  
#> AirBags:Origin  432.8  2  2.6835   0.07398 .  
#> Residuals      7015.0 87                      
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
Anova (lm (Max.Price ~ AirBags * Origin, data = Cars93), type = "III")
#> Anova Table (Type III tests)
#> 
#> Response: Max.Price
#>                Sum Sq Df F value    Pr(>F)    
#> (Intercept)    6405.3  1 79.4394 6.773e-14 ***
#> AirBags         855.9  2  5.3078  0.006684 ** 
#> Origin          574.5  1  7.1253  0.009067 ** 
#> AirBags:Origin  432.8  2  2.6835  0.073977 .  
#> Residuals      7015.0 87                      
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

If we focus on the type II tests, we notice that the sums of squares for interaction (432.8) corresponds to the sums of squares for the last term in the type I table. The sums of squares for `Origin` (283.5) corresponds to the sums of squares value when specifying `Max.Price ~ AirBags + Origin` and the sums of squares for `AirBags` (3585.6) corresponds to the sums of squares when specifying `Max.Price ~ Origin + AirBags`.

In the ANOVA table with type III tests, the interaction sums of squares agrees with the other two tables, but because the marginality principle is violated, the sums of squares for the main effects are different. Care should be taken when testing hypotheses with type III sums of squares.

### One-way ANCOVA models

A scatterplot of CO2 versus height suggests the possibility of a linear relationship between these two numeric variables. Check the above statement.

This means that the ANOVA model is not the correct model to use for the `climate.sim` data. Instead we should consider a one-way ANCOVA model with height as a *<span style="color:#FF9966">concomitant variable</span>*:

$$
Y_{ij} = \mu + \alpha_i + \beta_i x_{ij} + \epsilon_{ij}
$$

where $i = 1, 2, \dots, k$; $j = 1, 2, \dots, n_i$.

In this model $x_{ij}$ represents a continuous (or numeric) variable that is measured together with the dependent continuous variable $Y_{ij}$. If variable $x$ has a significant relationship with the dependent (response) variable the values of the latter should be adjusted before performing a one-way ANOVA. The researcher must first ascertain whether the regression coefficients (i.e. the slopes of the regression lines) of the different groups are the same. Therefore, <span style="color:#FF9966">Step I</span> of a covariance analysis investigates

$$
H_{01}: \beta_1 = \beta_2 = \dots = \beta_k
$$

and if this is not the case, then the analysis proceeds by (i) estimating the regression parameters $\beta_1, \beta_2, \dots, \beta_k$ for every group/treatment/level individually and (ii) by investigating the differences between the $y$-means of the $k$ groups at specific values of $x$.  On the other hand, if Step I shows that it can be assumed that $\beta_1 = \beta_2 = \dots = \beta_k = (\beta)$ then the $\beta_i$ in the model is replaced by the common $\beta$ and <span style="color:#FF9966">Step II</span> is carried out. Step II then consists of determining whether there is a significant regression relationship, i.e. the null hypothesis

$$
H_{02}: \beta = 0
$$

is tested. If it can be assumed that $\beta = 0$, then an ordinary analysis of variance can be carried out. If $\beta \neq 0$, <span style="color:#FF9966">Step III</span> of ANCOVA is executed which first consists of  adjusting the $y_{ij}$-observations for their regression relationship with the $\{x_{ij} \}$. An analysis of variance is then carried out on the adjusted $\{y_{ij} \}$ in order to ascertain whether the levels of the experimental factor have similar effects or not.

As an example we will again make use of the data of the `climate.sim` data set constructed above.

(a)	Obtain a scatterplot of CO2  as a function of height. Use different colours for the three oceans.  What can be deduced from the scatterplot?

(b)	Carry out a one-way ANOVA of CO2 measured in the three different ocean locations. 

(c)	 Perform Step I of a one-way ANCOVA using the commands:


``` r
one.way.ancova.I <- lm (CO2 ~ ocean + height + ocean:height, 
                        data = climate.sim)
Anova(one.way.ancova.I, type = "II")
#> Anova Table (Type II tests)
#> 
#> Response: CO2
#>              Sum Sq Df F value    Pr(>F)    
#> ocean         42.94  2  1.8288 0.1704134    
#> height       156.47  1 13.3290 0.0005907 ***
#> ocean:height   6.31  2  0.2689 0.7652300    
#> Residuals    633.93 54                      
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

coefficients(one.way.ancova.I)
#>            (Intercept)        oceanN-Atlantic 
#>          352.451112469            0.822464321 
#>        oceanS-Atlantic                 height 
#>            0.155359967           -0.010909055 
#> oceanN-Atlantic:height oceanS-Atlantic:height 
#>            0.002428426            0.004214614
```

<div style="margin-left: 25px; margin-right: 20px;">
*Interpretation of the ANOVA table*

First look at interaction effect in the anova table. It provides information on whether the hypothesis $\beta_1 = \beta_2 = \beta_3$ should be rejected. Since the conclusion is that all three β’s are the same (differences are negligent) then (and only then) inspect the line for the concomitant variable height to decide if the common slope $\beta = 0$. This is Step II.  If it can be concluded that the common slope is zero then the ANOVA of section \@ref(AnovaOneWay) is performed. 

In Step II  it is concluded that the common slope is not equal to zero so we perform Step III by calling,  for example,  
</div>


``` r
one.way.ancova.III <- lm (CO2 ~ ocean + height, data = climate.sim)
Anova (one.way.ancova.III, type = "II")
#> Anova Table (Type II tests)
#> 
#> Response: CO2
#>           Sum Sq Df F value    Pr(>F)    
#> ocean      42.94  2  1.8778 0.1624241    
#> height    156.47  1 13.6864 0.0004941 ***
#> Residuals 640.24 56                      
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

<div style="margin-left: 25px; margin-right: 20px;">
*Note that if you are not using type II sums of squares, but the default `anova()` function in the latter call the numeric concomitant variable MUST PRECEDE the categorical variable on the right-hand side of the tilde because Step III is only carried out on condition that Step II has indicated a significant relationship between the concomitant variable and the response variable.* The line corresponding to the factor variable ocean in the analysis of variance table is then inspected to conclude whether the effects associated with the respective levels of the factor variable are significantly different.

(i) In the example above we conclude that the three β’s in the ANCOVA model are the same and that the hypothesis $\beta = 0$ is rejected. In Step III we conclude that $\alpha_1 = \alpha_2 = \alpha_3$. Obtain the estimated regression equation using the following instructions:
</div>


``` r
one.way.ancova.final <- lm (CO2 ~ height, data = climate.sim)
coefficients (one.way.ancova.final)
#>   (Intercept)        height 
#> 352.723551558  -0.008562195
```

<div style="margin-left: 25px; margin-right: 20px;">
(ii) Add the regression line to the scatterplot constructed in (a). See Figure \@ref(fig:CO2oceanHeight).
</div>

<div class="figure">
<img src="11-modelling_files/figure-html/CO2oceanHeight-1.png" alt="Scatterplot of CO2 vs height with a single regression line." width="100%" />
<p class="caption">(\#fig:CO2oceanHeight)Scatterplot of CO2 vs height with a single regression line.</p>
</div>

<div style="margin-left: 25px; margin-right: 20px;">
(iii)	Inspection of Figure \@ref(fig:CO2oceanHeight) shows a negative relationship between CO2 and height but there is no real difference between the observations from the three different oceanic locations.
</div>

(d)	Perform Step I of a one-way ANCOVA of CO2 for the different seasons with height as concomitant variable.


``` r
one.way.ancova.I <- lm (CO2 ~ season + height + season:height, 
                        data = climate.sim)
Anova(one.way.ancova.I, type = "II")
#> Anova Table (Type II tests)
#> 
#> Response: CO2
#>               Sum Sq Df F value    Pr(>F)    
#> season        184.54  3  7.4808 0.0002966 ***
#> height        195.34  1 23.7559 1.069e-05 ***
#> season:height  71.04  3  2.8799 0.0446297 *  
#> Residuals     427.59 52                      
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

coefficients (one.way.ancova.I)
#>         (Intercept)        seasonautumn        seasonwinter 
#>       357.383726918        -9.397968637        -3.706636570 
#>        seasonspring              height seasonautumn:height 
#>        -3.832972292        -0.018192410         0.015544922 
#> seasonwinter:height seasonspring:height 
#>         0.012766975         0.006214125
```

<div style="margin-left: 25px; margin-right: 20px;">
*Interpretation of the ANOVA table*

First look at the line for interaction. This line provides information on whether the hypothesis $\beta_1 = \beta_2 = \beta_3 = \beta_3$ should be rejected (as is the case here. Why?)  Therefore we cannot proceed to Step II.  

(i)	In the example above we conclude that the three β’s in the ANCOVA model are not the same. Obtain the estimated regression equations for each of the seasons using the following instructions:
</div>


``` r
one.way.ancova.sep.reglns <- lm (CO2 ~ -1 + season/height, data = climate.sim)
coefficients (one.way.ancova.sep.reglns)
#>        seasonsummer        seasonautumn        seasonwinter 
#>       357.383726918       347.985758281       353.677090348 
#>        seasonspring seasonsummer:height seasonautumn:height 
#>       353.550754626        -0.018192410        -0.002647488 
#> seasonwinter:height seasonspring:height 
#>        -0.005425435        -0.011978285
```

<div style="margin-left: 25px; margin-right: 20px;">
(ii) Add the regression lines to the scatterplot of CO2 vs height. See Figure \@ref(fig:CO2seasonHeight).
</div>

<div class="figure">
<img src="11-modelling_files/figure-html/CO2seasonHeight-1.png" alt="Scatterplot of CO2 vs height with a separate regression line for each of the seasons." width="100%" />
<p class="caption">(\#fig:CO2seasonHeight)Scatterplot of CO2 vs height with a separate regression line for each of the seasons.</p>
</div>

<div style="margin-left: 25px; margin-right: 20px;">
(iii)	Inspection of Figure \@ref(fig:CO2seasonHeight) shows that the height has a much smaller effect in autumn and winter than in spring and summer. 

It is clear from Figure \@ref(fig:CO2_season_height) that the outcome of a test of the hypothesis that $\alpha_1 = \alpha_2 = \alpha_3 = \alpha_4$ depends on the value of height where the test is performed. At height = 600 the test that $\alpha_1 = \alpha_2 = \alpha_3 = \alpha_4$ is performed as follows:

First notice that the Residual sum of squares in the ANOVA table above is $RSSE=427.59$ with $52$ degrees of freedom. At height = 600 execute:
</div>


``` r
Anova (lm (CO2 ~ season:(I(height-600)) , data = climate.sim), type = "II")
#> Anova Table (Type II tests)
#> 
#> Response: CO2
#>                        Sum Sq Df F value    Pr(>F)    
#> season:I(height - 600) 318.53  4  8.3841 2.353e-05 ***
#> Residuals              522.39 55                      
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

<div style="margin-left: 25px; margin-right: 20px;">
Now calculate

$$
F_0 = \frac{\frac{522.39 - 427.59}{55-52}}{\frac{427.59}{52}} = 3.8429
$$

and the corresponding p-value using the instruction 
</div>


``` r
1 - pf (3.8429, 3, 52)
#> [1] 0.01468944
```

<div style="margin-left: 25px; margin-right: 20px;">
At height = 400
</div>


``` r
Anova (lm (CO2 ~ season:(I(height-400)) , data = climate.sim), type = "II")
#> Anova Table (Type II tests)
#> 
#> Response: CO2
#>                        Sum Sq Df F value    Pr(>F)    
#> season:I(height - 400) 245.34  4  5.6641 0.0006846 ***
#> Residuals              595.57 55                      
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

<div style="margin-left: 25px; margin-right: 20px;">
Now 

$$
F_0 = \frac{\frac{595.57 - 427.59}{55-52}}{\frac{427.59}{52}} = 6.80945
$$

and
</div>


``` r
1 - pf (6.80945, 3, 52)
#> [1] 0.0005874415
```

<div style="margin-left: 25px; margin-right: 20px;">
What conclusions can be drawn from the tests at heights of 600 and 400 respectively when using a 1% significance level? Conjecture on the conclusions following a multiple testing procedure after a significant anova result. 
</div>

(e)	Perform Step I of a one-way ANCOVA of ozone for the three oceanic locations with height as concomitant variable.


``` r
one.way.ancova.I <- lm (ozone ~ ocean + height + ocean:height, 
                        data = climate.sim)
Anova(one.way.ancova.I, type = "II")
#> Anova Table (Type II tests)
#> 
#> Response: ozone
#>              Sum Sq Df   F value Pr(>F)    
#> ocean         40.28  2  127.6912 <2e-16 ***
#> height       878.66  1 5571.0063 <2e-16 ***
#> ocean:height   0.52  2    1.6603 0.1996    
#> Residuals      8.52 54                     
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

coefficients(one.way.ancova.I)
#>            (Intercept)        oceanN-Atlantic 
#>           5.999989e+00           9.596618e-01 
#>        oceanS-Atlantic                 height 
#>           2.387163e+00           2.054280e-02 
#> oceanN-Atlantic:height oceanS-Atlantic:height 
#>           8.989381e-05          -9.691870e-04
```

<div style="margin-left: 25px; margin-right: 20px;">
*Interpretation of the ANOVA table*

First look at the line for interaction. We conclude that all three β’s are the same (differences are negligible) and then (and only then) inspect the line for continuous variable height to decide if the common slope β=0. This is Step II.  Since it is concluded that the common slope is not zero we proceed to Step III. 
</div>


``` r
one.way.ancova.III <- lm (ozone ~ ocean + height, data = climate.sim)
Anova(one.way.ancova.III, type = "II")
#> Anova Table (Type II tests)
#> 
#> Response: ozone
#>           Sum Sq Df F value    Pr(>F)    
#> ocean      40.28  2  124.75 < 2.2e-16 ***
#> height    878.66  1 5442.66 < 2.2e-16 ***
#> Residuals   9.04 56                      
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

coefficients(one.way.ancova.I)
#>            (Intercept)        oceanN-Atlantic 
#>           5.999989e+00           9.596618e-01 
#>        oceanS-Atlantic                 height 
#>           2.387163e+00           2.054280e-02 
#> oceanN-Atlantic:height oceanS-Atlantic:height 
#>           8.989381e-05          -9.691870e-04
```

<div style="margin-left: 25px; margin-right: 20px;">
The line corresponding to the factor variable ocean in the analysis of variance table is inspected to conclude if the effects associated with the respective levels of the factor variable are significantly different.

(i)	In the example above we conclude that the three β’s in the ANCOVA model are the same and that the hypothesis β=0 is rejected. In Step III we conclude that the hypothesis $\alpha_1 = \alpha_2 = \alpha_3$ is also rejected. Obtain the estimated regression equations using the following instructions:
</div>


``` r
one.way.ancova.reglns <- lm (ozone ~ ocean + height, data = climate.sim)
coefficients(one.way.ancova.reglns)
#>     (Intercept) oceanN-Atlantic oceanS-Atlantic 
#>      6.13127497      0.99327355      2.00694355 
#>          height 
#>      0.02020925
```

<div style="margin-left: 25px; margin-right: 20px;">
(ii)	Obtain a scatterplot of ozone as a function of height. Use different colours for the three oceans. Add a separate regression line for each of the oceans (see Figure \@ref(fig:ozoneOceanHeight)).

(iii)	Inspection of Figure \@ref(fig:ozoneOceanHeight) shows three parallel regression lines, since $\beta_1 = \beta_2 = \beta_3$, with different intercepts. The hypothesis that $\alpha_1 = \alpha_2 = \alpha_3$ is rejected irrespective of the height above sea level.
</div>

<div class="figure">
<img src="11-modelling_files/figure-html/ozoneOceanHeight-1.png" alt="Scatterplot of ozone vs height with a separate parallel regression lines for each of the different oceanic locations." width="100%" />
<p class="caption">(\#fig:ozoneOceanHeight)Scatterplot of ozone vs height with a separate parallel regression lines for each of the different oceanic locations.</p>
</div>

The steps of a one-way analysis of covariance is illustrated in the diagram in Figure \@ref(fig:ANCOVA).
 
<div class="figure">
<img src="pics/ANCOVA.jpg" alt="Diagrammatic process flow for a one-way analysis of covariance." width="100%" />
<p class="caption">(\#fig:ANCOVA)Diagrammatic process flow for a one-way analysis of covariance.</p>
</div>

### Maize Exercise

::: {style="color: #80CC99;"}

A researcher investigates if three maize varieties give the same yield. The following data are obtained according to the requirements of a linear model: 


```{=html}
<div class="tabwid"><style>.cl-c23b6a42{}.cl-c2363cb6{font-family:'Arial';font-size:11pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(102, 102, 102, 1.00);background-color:transparent;}.cl-c2388fde{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:3pt;padding-top:3pt;padding-left:3pt;padding-right:3pt;line-height: 1;background-color:transparent;}.cl-c2388fe8{margin:0;text-align:right;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:3pt;padding-top:3pt;padding-left:3pt;padding-right:3pt;line-height: 1;background-color:transparent;}.cl-c238a1f4{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-c238a1f5{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0.75pt solid rgba(102, 102, 102, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-c238a1fe{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-c238a208{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0.75pt solid rgba(51, 51, 51, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-c23b6a42'><thead><tr style="overflow-wrap:break-word;"><th  colspan="6"class="cl-c238a1f4"><p class="cl-c2388fde"><span class="cl-c2363cb6">VARIETY</span></p></th></tr><tr style="overflow-wrap:break-word;"><th  colspan="2"class="cl-c238a1f4"><p class="cl-c2388fde"><span class="cl-c2363cb6">KB1</span></p></th><th  colspan="2"class="cl-c238a1f4"><p class="cl-c2388fde"><span class="cl-c2363cb6">KB2</span></p></th><th  colspan="2"class="cl-c238a1f4"><p class="cl-c2388fde"><span class="cl-c2363cb6">KB3</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-c238a1f5"><p class="cl-c2388fde"><span class="cl-c2363cb6">Yield per hectare</span></p></th><th class="cl-c238a1f5"><p class="cl-c2388fde"><span class="cl-c2363cb6">Rainfall</span></p></th><th class="cl-c238a1f5"><p class="cl-c2388fde"><span class="cl-c2363cb6">Yield per hectare</span></p></th><th class="cl-c238a1f5"><p class="cl-c2388fde"><span class="cl-c2363cb6">Rainfall</span></p></th><th class="cl-c238a1f5"><p class="cl-c2388fde"><span class="cl-c2363cb6">Yield per hectare</span></p></th><th class="cl-c238a1f5"><p class="cl-c2388fde"><span class="cl-c2363cb6">Rainfall</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">17.50</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">95.90</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">37.50</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">114.10</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">18.75</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">98.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">45.00</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">116.20</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">42.15</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">119.28</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">13.75</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">117.60</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">38.75</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">116.20</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">48.75</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">121.94</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">52.50</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">105.28</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">33.75</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">93.10</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">32.50</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">97.02</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">17.50</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">94.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">18.75</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">81.34</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">53.75</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">102.90</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">18.75</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">92.40</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">21.25</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">115.36</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">38.75</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">91.70</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">52.50</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">110.74</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6"></span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6"></span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">22.50</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">102.76</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">32.50</span></p></td><td class="cl-c238a1fe"><p class="cl-c2388fe8"><span class="cl-c2363cb6">105.70</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-c238a208"><p class="cl-c2388fe8"><span class="cl-c2363cb6"></span></p></td><td class="cl-c238a208"><p class="cl-c2388fe8"><span class="cl-c2363cb6"></span></p></td><td class="cl-c238a208"><p class="cl-c2388fe8"><span class="cl-c2363cb6">17.50</span></p></td><td class="cl-c238a208"><p class="cl-c2388fe8"><span class="cl-c2363cb6">78.54</span></p></td><td class="cl-c238a208"><p class="cl-c2388fe8"><span class="cl-c2363cb6"></span></p></td><td class="cl-c238a208"><p class="cl-c2388fe8"><span class="cl-c2363cb6"></span></p></td></tr></tbody></table></div>
```


(a)	Plot the given data as a single scatterplot that distinguishes between the three varieties.	
(b) What hypotheses and models are of importance when considering a covariance analysis of the given data?

(c) Perform a detailed one-way ANCOVA.  

(d) Obtain estimates for the regression coefficients in the regression equations.

$$
Y_{ij} = \mu + \beta x_{ij} + \epsilon_{ij}, \hspace{1cm} i = 1, 2, \dots, k; j = 1, 2, \dots, n_i
$$

$$
Y_{ij} = \mu + \beta_i x_{ij} + \epsilon_{ij}, \hspace{1cm} i = 1, 2, \dots, k; j = 1, 2, \dots, n_i
$$

(e) Interpret the results of the analysis of covariance using the output of `lm()`.

:::

### Heart rate Exercise

::: {style="color: #80CC99;"}

@MillikenJohnson2002 discuss an investigation by a sport physiologist of the effect of three exercise programs (EX1, EX2 and EX3) on heart rate (HR) of males between 28 and 35 years of age.  The experiment consists of the random allocation of a subject to one of the programs. The subject then had to follow the exercise program for 8 weeks.  On finishing the program each subject had to complete a 6 minutes run and then had his heart rate (EHR) measured. As a measure of their fitness before entering the experiment each subject had also his resting heart rate (AHR) determined prior to the experiment. The following data were obtained:


```{=html}
<div class="tabwid"><style>.cl-c2522b92{}.cl-c24ca2da{font-family:'Arial';font-size:11pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(102, 102, 102, 1.00);background-color:transparent;}.cl-c24ec380{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:3pt;padding-top:3pt;padding-left:3pt;padding-right:3pt;line-height: 1;background-color:transparent;}.cl-c24ec38a{margin:0;text-align:right;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:3pt;padding-top:3pt;padding-left:3pt;padding-right:3pt;line-height: 1;background-color:transparent;}.cl-c24ed51e{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-c24ed51f{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0.75pt solid rgba(102, 102, 102, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-c24ed528{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-c24ed529{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0.75pt solid rgba(51, 51, 51, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-c2522b92'><thead><tr style="overflow-wrap:break-word;"><th  colspan="6"class="cl-c24ed51e"><p class="cl-c24ec380"><span class="cl-c24ca2da">EXERCISE PROGRAM</span></p></th></tr><tr style="overflow-wrap:break-word;"><th  colspan="2"class="cl-c24ed51e"><p class="cl-c24ec380"><span class="cl-c24ca2da">EX1</span></p></th><th  colspan="2"class="cl-c24ed51e"><p class="cl-c24ec380"><span class="cl-c24ca2da">EX2</span></p></th><th  colspan="2"class="cl-c24ed51e"><p class="cl-c24ec380"><span class="cl-c24ca2da">EX3</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-c24ed51f"><p class="cl-c24ec380"><span class="cl-c24ca2da">HER</span></p></th><th class="cl-c24ed51f"><p class="cl-c24ec380"><span class="cl-c24ca2da">AHR</span></p></th><th class="cl-c24ed51f"><p class="cl-c24ec380"><span class="cl-c24ca2da">HER</span></p></th><th class="cl-c24ed51f"><p class="cl-c24ec380"><span class="cl-c24ca2da">AHR</span></p></th><th class="cl-c24ed51f"><p class="cl-c24ec380"><span class="cl-c24ca2da">HER</span></p></th><th class="cl-c24ed51f"><p class="cl-c24ec380"><span class="cl-c24ca2da">AHR</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">118</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">56</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">148</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">60</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">153</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">56</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">138</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">59</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">159</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">62</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">150</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">58</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">142</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">62</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">162</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">65</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">158</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">61</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">147</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">68</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">157</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">66</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">152</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">64</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">160</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">71</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">169</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">73</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">160</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">72</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">166</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">76</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">164</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">75</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">154</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">75</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">165</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">83</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">179</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">84</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">155</span></p></td><td class="cl-c24ed528"><p class="cl-c24ec38a"><span class="cl-c24ca2da">82</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-c24ed529"><p class="cl-c24ec38a"><span class="cl-c24ca2da">171</span></p></td><td class="cl-c24ed529"><p class="cl-c24ec38a"><span class="cl-c24ca2da">87</span></p></td><td class="cl-c24ed529"><p class="cl-c24ec38a"><span class="cl-c24ca2da">177</span></p></td><td class="cl-c24ed529"><p class="cl-c24ec38a"><span class="cl-c24ca2da">88</span></p></td><td class="cl-c24ed529"><p class="cl-c24ec38a"><span class="cl-c24ca2da">164</span></p></td><td class="cl-c24ed529"><p class="cl-c24ec38a"><span class="cl-c24ca2da">86</span></p></td></tr></tbody></table></div>
```


Analyze the data and discuss the results. 

:::
