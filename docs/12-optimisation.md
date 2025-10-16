# Introduction to Optimisation {#optimisation}

Two related topics are briefly discussed in this chapter. First, the focus is on solving the equation $f(x)=0$.  The following two methods are considered (a) the bisection or binary search method and (b) the Newton-Raphson method. In the last two sections some principles underlying optimization are considered by briefly examining the usage of the function `optim()` for general optimization and the packages `lpSolve` and `Rsolnp` for constrained optimization.

## The bisection method for solving $f(x)=0$

The bisection method is based on the *<span style="color:#FF9966">Mean Value Theorem</span>* (MVT):  Let $f(x)$ be a continuous function defined over the interval $x∈[a,b]$ with the property that $f(a)$ and $f(b)$ have opposite signs.  According to the MVT there exists a value $p$ in $(a,b)$ such that $f(p)=0$.  In order to keep arguments simple it is assumed that the root of the equation is unique in this interval. The bisection method consists of

(i)	repeatedly halving subintervals of $[a,b]$ and
(ii) to determine in each step which half contains $p$.

The process is initialized by setting $a_1=1$ and $b_1=b$.  Let $p_1$ represents the midpoint of $[a_1,b_1 ]$, i.e. $p_1 = a_1 + \frac{b_1 - a_1}{2} = \frac{a_1 + b_1}{2}$. Suppose $f(p_1 )=0$ then $p=p_1$ so that we have the root. Suppose $f(p_1) \neq 0$ then $f(p_1)$ has similar sign to either $f(a_1)$ or $f(b_1)$.  If $f(p_1)$ and $f(a_1)$ have similar signs then $p∈(p_1,b_1)$ and we set $a_2=p_1$ and $b_2=b_1$. If $f(p_1)$ and $f(a_1)$ have opposite signs then $p∈(a_1,p_1)$ and we set $a_2=a_1$ and $b_2=p_1$. The whole process is now repeated with the interval $[a_2,b_2]$.   This process is illustrated in Figure \@ref(fig:bisection) and coded in the R function, `Bisection()`, given below.

<div class="figure">
<img src="pics/bisection.jpg" alt="Principle underlying the bisection method for finding a root of f(x)=0." width="100%" />
<p class="caption">(\#fig:bisection)Principle underlying the bisection method for finding a root of f(x)=0.</p>
</div>


``` r
Bisection <- function (a, b, fun, eps=.Machine$double.eps^0.4,  maxiter=100) 
{ #  This function finds the root of continuous function   
  #  f(x) = 0 in the interval [a, b] where
  #  f(a) and f(b) have opposite signs.
  #  The function f() is given in the form of an R function in the argument fun
  
  Iter <- 1
  while (Iter <= maxiter)
    { fa <- fun(a)
      p <- a + (b-a)/2
      fp <- fun(p)
      if ((fp == 0) | ((b-a)/2 < eps))
       { p <- p
         break   }
      else
       { Iter <- Iter + 1
         if ( fa * fp > 0 )
           { a <- p
             fa <- fp   }
         else b <- p
       }
    }
  
  if (Iter >= maxiter)
    stop ("Process has not converged. Try increasing maxiter.\n")
  p
}
```

(a) Use the bisection method to solve the equation for any given $c$ and $n$

$$
c^2 + x^2 + \frac{2cx}{n-1} = n-2.
$$

<div style="margin-left: 25px; margin-right: 20px;">
The function must check whether $b>a$ and whether $f(a)$ and $f(b)$ have opposite signs.
</div>

(b)	Change the bisection function to provide for `fun` to accept several arguments. Demonstrate your function with the equation given in (a).

(c)	Consider the following data given by @DobsonBarnett2008


```{=html}
<div class="tabwid"><style>.cl-dbdff660{}.cl-dbd3e6fe{font-family:'Arial';font-size:11pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(102, 102, 102, 1.00);background-color:transparent;}.cl-dbd903b4{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:3pt;padding-top:3pt;padding-left:3pt;padding-right:3pt;line-height: 1;background-color:transparent;}.cl-dbd903be{margin:0;text-align:right;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:3pt;padding-top:3pt;padding-left:3pt;padding-right:3pt;line-height: 1;background-color:transparent;}.cl-dbd92b32{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0.75pt solid rgba(102, 102, 102, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-dbd92b3c{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-dbdff660'><thead><tr style="overflow-wrap:break-word;"><th class="cl-dbd92b32"><p class="cl-dbd903b4"><span class="cl-dbd3e6fe">Season</span></p></th><th class="cl-dbd92b32"><p class="cl-dbd903b4"><span class="cl-dbd3e6fe">Number of tropical cyclones</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">1</span></p></td><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">6</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">2</span></p></td><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">5</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">3</span></p></td><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">4</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">4</span></p></td><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">6</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">5</span></p></td><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">6</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">6</span></p></td><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">3</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">7</span></p></td><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">12</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">8</span></p></td><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">7</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">9</span></p></td><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">4</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">10</span></p></td><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">2</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">11</span></p></td><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">6</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">12</span></p></td><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">7</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">13</span></p></td><td class="cl-dbd92b3c"><p class="cl-dbd903be"><span class="cl-dbd3e6fe">4</span></p></td></tr></tbody></table></div>
```


<div style="margin-left: 25px; margin-right: 20px;">
Suppose the number of cyclones can be modelled by a Poisson distribution with parameter $\theta$.

(i) Write down the log-likelihood function for determining the maximum likelihood (m.l.) estimate of $\theta$.

(ii) 	Use the function written in (b) without changing it to obtain the m.l. estimate of $\theta$ as well as the maximum of the log-likelihood.

(iii)	Use the R function `deriv()` to write a function based on the bisection principle for finding the maximum of $f(x,θ)$ with respect to $\theta$. Demonstrate its usage.

</div>

(d)	Study the help file of the R function `uniroot()`. Answer questions (a) and (c) using `uniroot()`.

## The Newton-Raphson method

Consider the following: Let $f(x)$ be a function that is at least twice differentiable over the interval $[a,b]$. The value of $x=p$ such that $f(p)=0$ needs to be found. Let $p_m∈[a,b]$ be an approximate value of $p$ such that $f'(p_m) \neq 0$ and $|p-p_m|$ is “small”.  Consider the Taylor power series expansion of $f(x)$ about $p_m$:

$$
f(x) = f(p_m) + (x - p_m)f'(p_m) + \frac{(x - p_m)^2}{2}f''(\xi(x))
$$

where $\xi(x)$ lies between $x$ and $p_m$. Since $f(p)=0$, it follows from the previous equation that

$$
0 = f(p) = f(p_m) + (p - p_m)f'(p_m) + \frac{(p - p_m)^2}{2}f''(\xi(p))
$$

but $|p-p_m|$ “small” implies that $(p-p_m )^2$ is much smaller and therefore

$$
0 \approx f(p_m) + (p - p_m)f'(p_m)
$$

so that

$$
p \approx p_m - \frac{f(p_m)}{f'(p_m)}
$$

From this follows the Newton-Raphson algorithm.  It starts with an initial approximation  $p_0$ and generates a sequence $\{p_m \}$ where

$$
p_{m+1} \approx p_m - \frac{f(p_m)}{f'(p_m)}
$$

for $m≥0$. In Figure \@ref(fig:NewtonRaphson) it is illustrated how the approximations are found by consecutive tangent lines.

<div class="figure">
<img src="pics/bisection.jpg" alt="Principle underlying the Newton-Raphson algorithm." width="100%" />
<p class="caption">(\#fig:NewtonRaphson)Principle underlying the Newton-Raphson algorithm.</p>
</div>

(a)	When the Newton-Raphson algorithm is used for finding m.l. estimates the score statistic, $U$, and its derivative viz. $U'$ are needed. The statistic $U'$ can be approximated by $E(U') = -\mathfrak{I} = -var(U)$.  When $U'$ is substituted by $E(U')$ in the Newton-Raphson algorithm the method is known as the *<span style="color:#FF9966">(Fisher) scoring method</span>*.

(b)	Write an R function to implement
    (i)  the Newton-Raphson algorithm and 
    (ii)  the scoring method for finding numerically the m.l. estimate of the scale  
      parameter of the Weibull-distribution

$$
f(y_1, \dots, y_n) = \prod_{i=1}^{n}{\frac{\lambda y_i^{\lambda-1}}{\theta^\lambda}exp \big[-(\frac{y_i}{\theta})^\lambda \big]}
$$

<div style="margin-left: 45px; margin-right: 20px;">
where $y_i>0$. Experiment with various initial values and convergence criteria. The following function serves as an example:
</div>


``` r
NewtonRaphson <- function(fun1, fun2, initval, maxiter = 20, 
                          eps = .Machine$double.eps^0.4, ...)
{ # initval = initial approximation of root
  # fun1 = function for which root is sought
  # fun2 = derivative of fun1 or the expected value of the 
  #                   derivative of the score statistic
  
  count <- 1
  Iter <- count
  Outvec <- as.vector (initval)
  Estim <- initval - fun1 (initval,...) /fun2 (initval,...)
  while (abs (Estim - initval) > eps)
    { count <- count + 1
      Iter <- c(Iter, count) 
      initval <- Estim
      Estim <- initval - fun1 (initval,...)/fun2 (initval,...)
      Outvec <- c(Outvec , Estim)
      if (count > maxiter) 
      { warning("Max number iterations reached without convergence") 
        break
      }
    }
  data.frame (IterNo = Iter,  Estim = Outvec)
}
```

The Weibull distribution is often used to model the time to failure (i.e. the survival time) of organisms or components. The parameter $\lambda$ determines the shape while $\theta$ is a scale parameter. The following table (from @AndrewsHerzberg1985 Table 29.1) contains the lifetimes of a random sample of pressure vessels:



```{=html}
<div class="tabwid"><style>.cl-dc14406e{}.cl-dc06f1ca{font-family:'Arial';font-size:11pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(102, 102, 102, 1.00);background-color:transparent;}.cl-dc0c0674{margin:0;text-align:right;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:3pt;padding-top:3pt;padding-left:3pt;padding-right:3pt;line-height: 1;background-color:transparent;}.cl-dc0c2f82{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0.75pt solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-dc0c2f8c{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-dc0c2f96{width:0.75in;background-color:transparent;vertical-align: middle;border-bottom: 0.75pt solid rgba(51, 51, 51, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-dc14406e'><tbody><tr style="overflow-wrap:break-word;"><td class="cl-dc0c2f82"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">1051</span></p></td><td class="cl-dc0c2f82"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">4921</span></p></td><td class="cl-dc0c2f82"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">7886</span></p></td><td class="cl-dc0c2f82"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">10861</span></p></td><td class="cl-dc0c2f82"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">13520</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">1337</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">5445</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">8108</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">11026</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">13670</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">1389</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">5620</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">8546</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">11214</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">14110</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">1921</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">5817</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">8666</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">11362</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">14496</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">1942</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">5905</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">8831</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">11604</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">15395</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">2322</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">5956</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">9106</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">11608</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">16179</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">3629</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">6068</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">9711</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">11745</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">17092</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">4006</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">6121</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">9806</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">11762</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">17568</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">4012</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">6473</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">10205</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">11895</span></p></td><td class="cl-dc0c2f8c"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">17568</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-dc0c2f96"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">4063</span></p></td><td class="cl-dc0c2f96"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">7501</span></p></td><td class="cl-dc0c2f96"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">10396</span></p></td><td class="cl-dc0c2f96"><p class="cl-dc0c0674"><span class="cl-dc06f1ca">12044</span></p></td><td class="cl-dc0c2f96"><p class="cl-dc0c0674"><span class="cl-dc06f1ca"></span></p></td></tr></tbody></table></div>
```


(c)	Use your implementation of the Newton-Raphson method to obtain the m.l. estimate for $\lambda$ and given $\theta = 2$.

(d)	Change  your Newton-Raphson function  so that the derivative is automatically calculated i.e. the function has only the arguments `fun`, `initval`, `maxiter`, eps`.

