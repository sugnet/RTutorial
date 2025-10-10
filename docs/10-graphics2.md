# R graphics: Round II {#graphics2}

R offers several different types of graphics: *<span style="color:#FF9966">grid</span>* graphics is contained in package `grid`; the package `lattice` contains *<span style="color:#FF9966">trellis</span>* graphics; the package `ggplot2` introduces *<span style="color:#FF9966">ggplot</span>* graphics to be implemented by the function `ggplot()`.  In this chapter, further aspects of what is known as *traditional R graphics* are studied before moving on to ggplot graphics.

## Graphics parameters

(a)	Study the help file of `par()`. Execute `par()` to obtain a list of all the current values of the graphical parameters. 

(b)	How is `par()` used to obtain the current setting of a specific graphics parameter e.g. the parameter `fin`?


``` r
par("fin")
#> [1] 6.999999 4.999999
```

(c)	How is `par()` used to change a graphics parameter e.g. `mfrow`?

(d)	How do you reset the changed values to their original values? Note the `no.readonly` argument of `par()`. *Hint*:  Study the following instructions and there effects carefully:


``` r
par('col')
#> [1] "black"
```

<div style="margin-left: 25px; margin-right: 20px;">
The current colour for graphics is "black".
</div>


``` r
temp <- par(col = "blue")
```

<div style="margin-left: 25px; margin-right: 20px;">
Change colour for graphics to "blue".
</div>


``` r
temp
#> $col
#> [1] "black"
```

<div style="margin-left: 25px; margin-right: 20px;">
Temp is a list of parameter(s) **BEFORE** change was made.
</div>


``` r
par('col')
#> [1] "black"
```

<div style="margin-left: 25px; margin-right: 20px;">
Shows that the colour for graphics was indeed changed to "blue". 
</div>

(e)	It is sometimes useful to use `par (ask = TRUE)` to instruct R to ask you whether an existing graph should be replaced by a new one.

(f)	Draw a histogram of variable Ozone in the data set `airquality` where each class interval is randomly represented by a different colour. What happened to the `NA` values?

## Layout of graphics

(a)	Review Figure \@ref(fig:figRegion).  Note the parameters that are discussed there.

(b)	Multiple figures on one page:  How do the graphical parameters `mfg` and `mfrow` or `mfcol` differ?  What are represented in the R data sets `ldeaths`, `mdeaths` and `fdeaths`? Use  `mfg` and `mfrow`  to obtain Figure \@ref(fig:fmdeaths). *Hint*: The graphics parameters `mfg` and `mfrow` are used together.

<div class="figure">
<img src="10-graphics2_files/figure-html/fmdeaths-1.png" alt="Plots of the fdeaths and mdeaths data sets" width="100%" />
<p class="caption">(\#fig:fmdeaths)Plots of the fdeaths and mdeaths data sets</p>
</div>


``` r
par (mfrow = c(3, 2), mfg = c(1, 1))
```

<div style="margin-left: 25px; margin-right: 20px;">
The `mfrow` setting reserves three rows and two columns for graphics to be filled row-wise. The `mfg` setting specifies that the next graph will be placed in the position defined by row one column one. Once this graph has been constructed the instruction
</div>


``` r
par (mfg = c(1, 2))
```

<div style="margin-left: 25px; margin-right: 20px;">
will result in the next graph to appear in the position defined by row one and column two. Next we need the instruction
</div>


``` r
par (mfrow = c(3, 1), mfg = c(2, 1))
```
  
<div style="margin-left: 25px; margin-right: 20px;">
requesting a graph window having three rows and one column with the next graph to appear at position row two (only one column in row two).
</div>

(c)	Note how the meaning of the margins changes when more than one figure is drawn on a page to make provision for an *<span style="color:#FF9966">outer margin</span>* surrounding all figures in addition to the *<span style="color:#FF9966">margin</span>* surrounding each separate figure.

(d)	Study how the functions `split.screen()`, `screen()` and `close.screen()` work as explained in the help facility.

(e)	Study the usage of the function `layout()` in detail for more complicated arrangements of the graph window. An example of its usage is deferred until later in the chapter.

## Low-level plotting commands

*	The functions in Table \@ref(tab:lowlevelPlotFuncs) are used to edit existing graphs.  
*	Study these functions carefully.
*	Study how the right mouse button is used with R graphs.
*	Most plotting tasks require some combination of high-level and low-level plotting commands.

Table: (\#tab:lowlevelPlotFuncs) Low-level plotting functions.

| *<span style="color:#CC99FF">Function</span>* | *<span style="color:#CC99FF">Description</span>*  |
| ------ | --------------- | 
| `abline()`   |	Add regression lines to a plot; Also for adding a vertical and horizontal lines to a plot |
| `arrows()`   |	Draw arrow on plot |
| `axis()`     |	Add custom axis to plot |
| `box()`      |	Draw box around plot |
| `chull()`    |	Compute a convex hull |
| `jitter()`   |	Add a small amount of noise |
| `legend()`   |	Add a legend to a plot |
| `lines()`    |	Add lines to a plot |
| `mtext()`    |	Write text in margins |
| `points()`   |	Add points to a plot |
| `polygon()`  |	Draw and shade polygons |
| `rug()`      |	Add data-based marks to an axis |
| `segments()` |	Draw disconnected line segments |
| `symbols()`  |	Draw symbols on a plot |
| `text()`     |	Add text to a plot |
| `title()`    |	Add titles or axis labels to a plot |
	
## Using the plotting commands

### Multiple lines or groups of points on the same graph {#matplot}

Study how the function  `matplot()` works. Note the functions `matlines()` and `matpoints()`. Study and execute the following example:


``` r
my.func <- function () 
{ times <- matrix(0,100,3)
  for(i in 1:100)
    {  n <- i * 10000
       s1 <- 1:n
       s2 <- sample(n)
       s3 <- rnorm(n)
       times[i,1] <- system.time(sort(s1))[1]
       times[i,2] <- system.time(sort(s2))[1]
       times[i,3] <- system.time(sort(s3))[1]
     }
  matplot(x = (1:100)*10000, y= times, type = "l", lty = 1:3,
          col = c("black", "green", "red"), xlab = "Length",  
          ylab = "Time in seconds", main = "Time for sorting")  
}
my.func()
```

<div class="figure">
<img src="10-graphics2_files/figure-html/matplotExample-1.png" alt="Three methods of performing sort." width="100%" />
<p class="caption">(\#fig:matplotExample)Three methods of performing sort.</p>
</div>

###	Multiple lines or groups of points on the same graph but the lines (points) are not all the same length (number)

What technique must be followed?  First study the `Cars93` data set in package `MASS`; then study and execute the code below. Experiment with different values of `spar`.


``` r
my.func <- function (spar = 0.9)
{ require (MASS)                             # What is the effect of require()?
  oldstate <- par (no.readonly = TRUE)       # Describe object 'oldstate'
  on.exit (par (oldstate))                   # Of what use is on.exit()?

  cargrp <- Cars93[ , "Type"]
  price <- Cars93[ , "Price"]
  mpg.city <- Cars93[ , "MPG.city"]
  mpg.highway <- Cars93[ , "MPG.highway"]
  plot(price, mpg.city, type = "n", ylim = c(0, max(mpg.city)), 
       main = "Fuel Consumption vs Price for City Drive", xlab = "Price", 
       ylab = "Miles per Gallon in City")
  jj <- 0
  for(i in levels(cargrp))
    {  jj <- jj+1
       lines (smooth.spline (price[cargrp==i], mpg.city[cargrp==i], spar=spar),
              lty = jj, col = jj, lwd=2)
    }
  plot(price, mpg.highway, type = "n", ylim = c(0, max(mpg.highway)), 
       main = "Fuel Consumption vs Price for Highway Drive", xlab = "Price", 
       ylab = "Miles per Gallon on Highway")
  jj <- 0
  for(i in levels(cargrp))
    {  jj <- jj+1
       lines (smooth.spline (price[cargrp==i], mpg.highway[cargrp==i], 
                             spar = spar),
              lty = jj, col = jj, lwd = 2)
    }
}
my.func ()
#> Loading required package: MASS
```

<div class="figure">
<img src="10-graphics2_files/figure-html/diffLengths-1.png" alt="Plotting multiple lines of different lenghts" width="100%" />
<p class="caption">(\#fig:diffLengths-1)Plotting multiple lines of different lenghts</p>
</div><div class="figure">
<img src="10-graphics2_files/figure-html/diffLengths-2.png" alt="Plotting multiple lines of different lenghts" width="100%" />
<p class="caption">(\#fig:diffLengths-2)Plotting multiple lines of different lenghts</p>
</div>

(a)	Explain the output generated by the above function call.

(b)	What technique can also be followed in the case of point diagrams?

### Adding legends to a graph

(a)	Study how the function `legend()` and the graphical parameter `usr` work.  Study the code used to obtain Figure \@ref(fig:cargrps).  Revise the `locator()` function.

(b)	Use the facts that one USA gallon of liquid is equal to $0.83267$ UK (imperial) gallon of liquid and one mile is equal to $1.6093$ kilometres to obtain a figure similar to Figure 10.4.1 but with a kilometres per litre scale on the right-hand side that corresponds to the miles per gallon (USA) on highway scale on the left-hand side.



``` r
my.func <- function()
{ require (MASS)  
  oldstate <- par (no.readonly = TRUE) 
  on.exit (par (oldstate)) 

  cargrp <- Cars93[ , "Type"]
  price <- Cars93[ , "Price"]
  mpg.city <- Cars93[ , "MPG.city"]
  plot(price, mpg.city, type = "n", ylim = c(0, max(mpg.city)), 
       main = "Fuel Consumption vs Price for City Drive", xlab = "Price", 
       ylab = "Miles per Gallon in City")
  char <- substring (as.character (cargrp), 1, 2)
  text (x = price, y = mpg.city, labels = char, pos = 1, cex = 0.75)
  labs <- paste (substring (levels (cargrp), 1, 2), levels(cargrp), sep=":  ")
  legend(x = 40, y = 42, legend = labs)
}
my.func ()
```

<div class="figure">
<img src="10-graphics2_files/figure-html/cargrps-1.png" alt="Illustrating adding a legend to a plot." width="100%" />
<p class="caption">(\#fig:cargrps)Illustrating adding a legend to a plot.</p>
</div>


### Multiple plots with identical axes {#multAxes}

How can various graphs with identical axes be obtained?  Show how this can be done by graphing the sorting time for the three procedures considered in \@ref(matplot) above in three separate plots in the same graph window.

### Providing a single legend for multiple plots

Suppose there were two sorting methods for each of the three situations described in\@ref(matplot) and \@ref(multAxes) above. How can the three graphs be provided with a single legend without the legend appearing in one of the graphs?  Explain in detail.

### Changing the plotting character: common plotting characters in R

Note the use of graphical parameters `pch` and `mkh`. What plotting characters are available?  Study the help file of `par()` and `points()`. Study the plotting characters displayed in Figure \@ref(fig:pch) and the code used to produce the figure. How can plotting characters be made to appear in legends?


``` r
plot (x = rep(1:10, 2), y = rep (c(1,2), c(10,10)), pch = 0:19, cex = 2, 
      pty = "p", ylim = c(0,3), xlab = "", ylab = "", xaxt = "n", yaxt = "n")
```

<div class="figure">
<img src="10-graphics2_files/figure-html/pch-1.png" alt="Some common plotting characters available in R." width="100%" />
<p class="caption">(\#fig:pch)Some common plotting characters available in R.</p>
</div>

###	Changing the colour in plots

The graphical parameter `col` allows the user to specify the colour(s) in number format as given in Figure \@ref(fig:col). The full list of named colours can be obtained with the command `colors()` in the Console.

Alternatively, the colour can be specified by hue, saturation and value with `hsv (h = , s = , v = )`, hue, chroma and luminance with `hcl (h = , c = , l = )` or red, green and blue with `rgb (red = , green = , blue = )`. The `rgb()` function has an argument `maxColorValue` with default value `1` which indicates the range known as the gamma-compressed values. Typically, the red, green and blue values range between 0 and 255 or video display or 8-bit graphics. To select a specific shade of light blue, the following command can be used:


``` r
plot (ldeaths, col = rgb (red = 167, green = 227, blue = 227, 
                          maxColorValue = 255))
```

<div class="figure">
<img src="10-graphics2_files/figure-html/colExample-1.png" alt="Colour selection with rgb()." width="100%" />
<p class="caption">(\#fig:colExample)Colour selection with rgb().</p>
</div>

The output of the `rgb()` function is in the hexidemical colour number format, e.g. "#A7E3E3". The function `col2rgb()` accepts a colour name, hexadecimal colour number format or colour number and provides the red, green and blue values in the 0 to 255 range.

<div class="figure">
<img src="10-graphics2_files/figure-html/col-1.png" alt="The default colour palette available in R." width="100%" />
<p class="caption">(\#fig:col)The default colour palette available in R.</p>
</div>
 
A sequence of $n$ colours can be generated with the function `colorRampPalette()`.  As an example, the colour vector used for plotting in Figure \@ref(fig:colRamp) were generated with the call `colorRampPalette (c ("red", "green", "white", "gold"))(20)`. Study how the following instructions generate colour sequences: `rainbow()`, `heat.colors()`, `terrain.colors()`, `topo.colors()`, `cm.colors()`.

<div class="figure">
<img src="10-graphics2_files/figure-html/colRamp-1.png" alt="User specified colour sequence with colorRampPalette()." width="100%" />
<p class="caption">(\#fig:colRamp)User specified colour sequence with colorRampPalette().</p>
</div>
 
###	Logarithmic axes

The `log()` function and the `log` argument of the `plot()` function are useful in this regard. The `log` argument of the `plot()` function can be specified as `log="x"`; or `log="y"`; or `log="xy"` depending on whether the x-axis, the y-axis, or both axes should be plotted logarithmically.

### Graphs with character strings as the ‘scale’ on the axis

Figure \@ref(fig:catAxis) illustrates how user defined character strings can appear as calibrations on an axis. Furthermore, this figure illustrates several techniques to fine-tune plots. Study the code resulting in Figure \@ref(fig:catAxis) in detail.



``` r
my.func <- function()
{ old.state <- par(no.readonly = TRUE)
  on.exit (par (old.state))
  area <- state.x77[, "Area"]
  income <- state.x77[, "Income"]
  area.grp <- cut(area, c(0, quantile (area, c(1/3, 2/3, 1))),
                  labels = c("Small", "Medium", "Large"))
  income.grp <- cut(income, c(0, quantile (income, c(1/2, 1))),
                    labels = c("Below Median", "Above Median"))
  mns <- tapply(state.x77[, "Illiteracy"], list(area.grp, income.grp), mean)
  par(mfrow = c(1, 2))
  plot(c(0.8, 3.2), range(mns), type = "n", xaxt = "n", xlab = "Area Group", 
       ylab ="Mean Illiteracy", sub = "Function plot() used")
  axis(side = 1, at = 1:3, labels = levels(area.grp))
  lines(1:3, mns[, 1])
  lines(1:3, mns[, 2], lty = 2)
  par(usr = c(0, 1, 0, 1))
  legend(0.56, 0.96, lty = c(1,2), legend = levels(income.grp), cex= 0.5)
  text(0.63, 0.98, adj = 0, "Income Group", cex = 0.5)
	interaction.plot(area.grp, income.grp, state.x77[,"Illiteracy"], 
	                 xlab = "Area Group", ylab = "Mean Illiteracy", 
	                 sub = "Interaction.plot used", lty = 1:2, xtick = TRUE, 
	                 legend = FALSE)
  par(mfrow = c(1,1))
  par(new = T)
  plot(1:10, 1:10, type="n", xlab="", ylab="",axes = FALSE)
  title(main = "Illiteracy vs Size for States grouped by Income")
}
my.func ()
```

<div class="figure">
<img src="10-graphics2_files/figure-html/catAxis-1.png" alt="Figures with character strings as axis calibrations and other enhancements to plots." width="100%" />
<p class="caption">(\#fig:catAxis)Figures with character strings as axis calibrations and other enhancements to plots.</p>
</div>
 
###	Customizing bar charts and histograms

(a) How can every bar in a bar chart be represented in a different colour and be given separate headings? 

(b)	How can only a line graph without any colours be obtained?  

(c)	How can a probability density function be superimposed on a histogram?  

(d)	How can bar charts be provided with user-defined axes?  

Use the `Cars93` data set to answer the above four questions by constructing a figure similar to the one shown in Figure \@ref(fig:BarHist). Note: In the Mean MPG plot not all car types are used. If a factor variable is subsetted the original levels will be kept although some of them might not occurr. Hence it might be necessary to create a new factor variable with only the levels that are needed by using `factor()`.

<div class="figure">
<img src="10-graphics2_files/figure-html/BarHist-1.png" alt="Enhanced bar charts and histograms." width="100%" />
<p class="caption">(\#fig:BarHist)Enhanced bar charts and histograms.</p>
</div>


###	Three-dimensional graphical displays

(a) Study how the function `persp()` works.

(b) Work through the example code that creates Figure \@ref(fig:persp).  Apart from the arrow that points to the maximum, different colours must be used to highlight the different aspects of the graph.

(c)	Provide horizontally and vertically rotated views of the 3D plot.


``` r
my.func <- function () 
{ x <- seq(-10, 10, length= 30)
  y <- x
  ff <- function(x,y) { r <- sqrt(x^2+y^2); 10 * sin(r)/r }
  z <- outer(x, y, ff)
  z[is.na(z)] <- 1
  op <- par(bg = "white")
#  persp(x, y, z, theta = 30, phi = 30, expand = 0.5, col = "lightblue")
  res <- persp(x, y, z, theta = 30, phi = 30, expand = 0.5, col = "lightblue", 
               ltheta = 120, shade = 0.75, ticktype = "detailed", xlab = "X", 
               ylab = "Y", zlab = "Z" ) 
  print (round(res, 3))
  
  #--- Add to existing persp plot : ---
  #--- Function trans3d() -------------
 	trans3d <- function(x,y,z, pmat) 
 	{
 	  tr <- cbind(x,y,z,1) %*% pmat
    list(x = tr[,1]/tr[,4], y = tr[,2]/tr[,4])
  }
  # ----------------------------------
  z1 <- ff(1e-10, 1e-10)
  transfrm <- trans3d (c(0,-2.5), c(0,5), c(z1,z1), res)
  arrows(transfrm$x[1], transfrm$y[1], transfrm$x[2], transfrm$y[2], 
         length = 0.1, code = 1)
  text(transfrm$x[2], transfrm$y[2]+0.02, "Maximum occurs here")
  return(z1)
}
my.func()
```

<div class="figure">
<img src="10-graphics2_files/figure-html/persp-1.png" alt="Annotated 3D perspective plot." width="100%" />
<p class="caption">(\#fig:persp)Annotated 3D perspective plot.</p>
</div>

```
#>       [,1]   [,2]   [,3]   [,4]
#> [1,] 0.087 -0.025  0.043 -0.043
#> [2,] 0.050  0.043 -0.075  0.075
#> [3,] 0.000  0.074  0.042 -0.042
#> [4,] 0.000 -0.273 -2.890  3.890
#> [1] 10
```
 

###	Diagrams

Use R to draw a simple flow diagram.  The diagram must contain at least one rectangle, one square, one circle and one triangle.  Furthermore, there must be straight and curved lines as well as text describing the different elements.  *Hint*:  Study how the functions `arrows()`, `lines()`, `text()` and `symbols()` work as discussed in their respective help facilities.

###	Annotating graphics with special symbols

Construct a graph of a $normal(0, 1)$ density function. Give as a title to the plot the expression "Density of a normal random variable with $\mu = 0$ and $\sigma^2 = 1$."  *Hint*: Consult the help file of `plotmath()`. Within the plot draw an arrow to the density and label it $\frac{1}{\sqrt{2 \pi}} e^{-\frac{1}{2}x^2}$.

	Quantile plots
Consider the histogram of weight in Figure 10.4.6. Does this variable follow a normal distribution? A normal quantile plot, shows the observations vs the corresponding quantiles of a standard normal distribution. If the observations correspond to a normal distribution, this will approximately form a straight line. Use the qqline() function to add a straing line to the plot.
> qqnorm (Cars93$Weight)
> qqline (Cars93$Weight)

In a similar manner, quantile-quantile plots for other probability distributions can be constructed with the function qqplot().
> qqplot (qexp(seq(from=0,to=1,len=200), rate=4), 
          rexp(200, rate=4))
> qqline(y, distribution=function(p) qexp(p, rate=4))

	Estimating a density
The histograms in Figure 10.6.1 show 200 observations generated, 100 from a normal(9,2^2) and 100 from a normal(13,1) distribution. Histograms are very sensitive to the choice of the number of bins and the starting values of the bins. The wider bins do not show any evidence of a bimodal distribution. Using the smaller bins, the location of the bins can suggest either a bimodal or trimodal distribution. 

set.seed (38472)
x1 <- rnorm(100, 9, 2)
x2 <- rnorm(100, 13, 1)
x <- c(x1, x2)
par(mfcol=c(2,3))
out1 <- hist(x,right=F)
out2 <- hist(x,breaks=20,right=F)
b1 <- out1$breaks
b11 <- c(b1[1]-0.5,b1+0.5)
b2 <- out2$breaks
b21 <- c(b2[1]-0.5,b2+0.5)
hist(x,breaks=b11,right=F)
hist(x,breaks=b21,right=F)
b12 <- c(b1[1]-0.2,b1+0.2)
b22 <- c(b2[1]-0.2,b2+0.2)
hist(x,breaks=b12,right=F)
hist(x,breaks=b22,right=F)
 
Figure 10.6.1: Histograms with different bin sizes and bin locations of the same normal mixture data set.

One possible solution to the bin selection problem for histograms is the Average Shifted Histogram (ASH). First we define a density histogram. Since we aim to estimate the density (which integrates to one) a density histogram is normalised such that the area in the histogram is equal to one.
Consider a set of bins B_k=[b_k ┤,├ b_(k+1) ) with fixed bin width λ=b_(k+1)-b_k  ∀ k, then the density histogram is defined as f ̂=1/Nλ ∑_(i=1)^N▒〖I_[b_k,b_(k+1) )  (x_i)〗  for x∈B_k. Consider a collection of m histograms  f ̂_1,f ̂_2,…,f ̂_m each with bin width h, but with respective bin origins b_01=0,b_02=h/m,b_03=2h/m,…,b_0m=(m-1)h/m. The average shifted histogram is defined as f ̂_ASH=1/m ∑_(i=1)^m▒f ̂_i .

ASH <- function (x, b0=1, bk=15, h=0.5, m=5) # h=lambda
{
  Bvec <- as.vector((bk-b0)/h+2,"list")
  fhat <- matrix(nrow=m,ncol=(bk-b0)/h+1) 
  for (i in 1:m)
    { Bvec[[i]] <- seq(from=b0+(i-1)*h/m, to=bk+h+(i-1)*h/m, by=h)
      fhat[i,] <- hist(x,breaks=Bvec[[i]],right=T,plot=F)$density
    }
  fhat.ASH <- apply(fhat,2,mean)
  x.vec <- seq(from=b0,to=bk+h,length=length(fhat.ASH))
  plot (x.vec, fhat.ASH, type="l")
}
ASH(x, m=20, h=1, b0=-2, bk=18)

 
Figure 10.6.2: Average shifted histogram of normal mixture data.

The ASH is given in Figure 10.6.2. A more sophisticated method for estimating a density with a kernel density estimate. The density histograms is replaced by a smooth kernel function, leading to a smoother estimate. The R function density() provides a variety of kernels. Using the default kernel, a Gaussian distribution, the kernel density estimate is given in Figure 10.6.3.

> plot(density(x), type="l")

Experiment with different kernel function and different choices of bandwidth (argument bw) for controlling the amount of smoothing.

 
Figure 10.6.3: Guassian kernel density estimate of the normal mixture data.





