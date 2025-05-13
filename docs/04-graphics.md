# Introducing traditional R graphics {#graphics}

A basic knowledge of R graphics is needed before directing attention to the art of writing programs (functions) in R. Therefore, in this chapter a brief overview is given of the basics of traditional R graphics. In a later chapter, after studying the principles of R programming, a second round of R graphics will follow.  

##	General

Study the graphical parameters by requesting


``` r
?par
```

In Figure \@ref(fig:figRegion) the main components of a graph window are illustrated. Study this figure in detail. The *<span style="color:#FF9966">Plot Region</span>* together with the*<span style="color:#FF9966">Margins</span>*  is called the *<span style="color:#FF9966">Figure Region</span>*.


<div class="figure">
<img src="pics/figMargins.jpg" alt="The main components of a graph window and the parameters for controlling their sizes.  The parameter mai is a numerical vector of the form c(bottom, left, top, right) specifying the margins in inches while the parameter mar has a similar form specifying the respective margins as the number of lines. The default of mar is c(5, 4, 4, 2) + 0.1." width="640" />
<p class="caption">(\#fig:figRegion)The main components of a graph window and the parameters for controlling their sizes.  The parameter mai is a numerical vector of the form c(bottom, left, top, right) specifying the margins in inches while the parameter mar has a similar form specifying the respective margins as the number of lines. The default of mar is c(5, 4, 4, 2) + 0.1.</p>
</div>


(a)	What is the difference between high-level and low-level plotting instructions?

(b)	Take note especially how the functions `windows()`, `win.graph()` or `x11()` are used as well as the different options available for these functions. 

(c)	The instruction `dev.new()` allows opening a new graph window in a platform-independent way.

(d)	In this chapter some high-level plotting instructions are studied. Each of these instructions results in a (new) graph window with a complete graph drawn.  The command `graphics.off()` deletes all open graphic devices.

(e)	Study the use of  `par()`,  `par(mfrow =)`  and  `par(mfcol =)`. Study the use of `par(new = TRUE)` to plot more than one figure on the same set of axes.

(f)	Study how the functions `graphics.off()` and `dev.off()`  work.

##	High-level plotting instructions

(a)	Construct a barplot of the illiteracy of the states according to the `areagrp` (as defined in section \@ref(areagrp)) in the `state.x77` dataframe. *Hint*: The function `tapply()` operates on a vector given as its first argument.  Its second argument groups the first argument into groups so that the function given in its third argument can be applied to each of these groups. Study the following command:


``` r
barplot (tapply (state.x77[, "Illiteracy"], areagrp, mean), 
         names=levels(areagrp), ylab = "Illiteracy", xlab = "Area of State", 
         main = "Barplot of Mean Illiteracy")
```

(b)	Construct, for the `state.x77` data set, box plots of illiteracy broken down by the income of the states. First use `cut()` to form three categories of state income:


``` r
state.income <- cut (state.x77[ , "Income"], c(0, 4000, 5000, Inf),
                     labels=c("$4000 or less", "$4001-$5000", "more than $5001"))
```

<div style="margin-left: 25px; margin-right: 20px;">
Then use `boxplot()` together with `split()` to produce the desired graph:
</div>


``` r
boxplot (split (state.x77[ , "Income"], state.income))
```

<div style="margin-left: 25px; margin-right: 20px;">
Add labels for the axes as well as a title for the figure.
</div>

