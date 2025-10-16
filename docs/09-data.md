# Reading data files into R, formatting and printing { #data }

## Reading Microsoft Excel files into R

The following three ways can be used to read an Excel file into R as an object:

(a)	The file can be stored as a *<span style="color:#FFB3B3">.txt</span>*  or *<span style="color:#FFB3B3">.csv</span>* file and then `read.table()`, `scan()` or `read.csv()` can be used to read the file into R.

(b)	Directly read the *<span style="color:#FFB3B3">.xlsx</span>* file into R with the `readxl` package. List the sheet names with `excel_sheets()`. Specify a worksheet by name or number with a command like `objectname <- read_excel(xlsx_example, sheet = "Sheet1")`.

(c)	The *<span style="color:#FFB3B3">.xlsx</span>* file can also be read into R with the `xlsx` package. The R functions `read.xlsx()` and `read.xlsx2()` can be used to read the contents of an Excel worksheet into an R data.frame. The difference between these two functions is that `read.xlsx()` preserves the data type. It tries to guess the class type of the variable corresponding to each column in the worksheet. Note that, the `read.xlsx()` function is slow for large data sets (worksheet with more than 100 000 cells). The `read.xlsx2()` function is faster on big files compared to `read.xlsx()` function. The commands have the following format: `objectname <- read.xlsx (file, sheetIndex, header = TRUE,             colClasses=NA)` and `objectname <- read.xlsx2 (file, sheetIndex, header = TRUE, colClasses="character")`.

(d)	Select the data in Excel (Data can also be selected in any other application such as Word or a text editor). Copy the selected range. In R: 
`objectname <- read.table (file = "clipboard")`. *Hint*: Be careful with empty cells in Excel: some preparation of the Excel file might be needed.

(e)	To avoid problems with end-of-file characters that can occur when using the method in (d), the package `clipr` can be used.


``` r
library (clipr)
objectname <- read_clip_tbl (header = TRUE, row.names = 1)
```

<div style="margin-left: 25px; margin-right: 20px;">
The functions `clear_clip()` and `write_clip()` can also be very useful.
</div>

## Reading other data files into R

The R package `foreign()` provides functions for reading data from other packages into R:


``` r
library(foreign)
objects(name="package:foreign")
#>  [1] "data.restore"  "lookup.xport"  "read.arff"    
#>  [4] "read.dbf"      "read.dta"      "read.epiinfo" 
#>  [7] "read.mtp"      "read.octave"   "read.S"       
#> [10] "read.spss"     "read.ssd"      "read.systat"  
#> [13] "read.xport"    "write.arff"    "write.dbf"    
#> [16] "write.dta"     "write.foreign"
```

Study the helpfiles of these functions for reading into R binary data, SAS XPORT format, Weka Attribute-Relation File Format, the Xbase family of database languages dBase, Clipper and FoxPro, Stata, Epi Info and EpiData files, Minitab portable worksheets, Octave text files, data.dump files that were produced in S version 3, SPSS save or export files, SAS data sets to be converted to ssd format^[This function requires SAS to be installed since it creates and run a SASA program that converts the data set to ssd format and uses `read.xport()` to obtain a dataframe.] and Systat files.

## Sending output to a file

The function `sink("filename")` can be used to divert output that normally appears in the console to a file.  The option `options (echo = TRUE)` ensures that the R instructions will also be included in the file. The instruction `sink()` makes output to appear in the console again.

How do the functions `write(x)` and `sink("filename")` differ?   Study the arguments of `write()` thoroughly.

## Writing R objects for transport

The R function `save(..., file = )` writes an external representation of R objects to the specified file. The names of the objects to be saved should appear either as symbols (or character strings) in `...` or as a character vector in list. These objects can be read back from the file using the function `load (file = )`. Study how these two functions work by consulting the help files.  The functions `save()` and `load()` are very useful for transporting R objects between computers. 

The functions `saveRDS (object = , file = )` and `object.name <- readRDS (file = )` write a single R object to a file, and restore it named `object.name`. Care has to be taken with the deprecated functions `dump()` and `source()`. If R objects were saved to a file using `dump()`, it should be restored to an R workspace with `source()`, not `load()`.

## The use of the file .Rhistory and the function `history()`

The file *<span style="color:#FFB3B3">.Rhistory</span>* is created in the same folder where the *<span style="color:#FFB3B3">.Rdata</span>* exists. It can be inspected with any text editor or with MS Word and as such provides an exact record of all activity in the R console (commands window).

Study the help file of the function  `history()`.

## Command re-editing

(a)	Use of the up and down arrows to recall previous commands. Delete, Backspace, Home and End keys for editing.

(b)	Note the use of the script window to execute entire functions or selected instructions only.

## Customized printing

The basic tool for customized printing is the function `cat()`.   This function can be used to output messages to the console or to a file.  Note the different arguments that are available for `cat()`:

(i)	By default output is display on the screen; for output to be directed to a file, use argument `file = "file name including path"`.

(ii)	By default output directed to a file replaces previous contents of the file; use argument `append = TRUE` to append new output to previous contents.

(iii)	Use `sep = "xx"` to automatically insert characters between the unnamed arguments to `cat()` in the output.

(iv)	To automatically insert new lines in the output use `fill = TRUE`.

(v)	The `labels =` argument allows insertion of a character string at the beginning of each output line. If labels is a vector its values are used cyclically.

Write today’s date as given by the function date() in the form `“The date today is:   Day of the week,  xx, month,  20xx.”` as an heading to a file. *Hint*: recall functions `cat()`, `match()`, `substring()`, `paste()`,  `replace()`.

##	Formatting numbers

(a)	Study how the functions `round()` and `signif()`  together with `cat()` can be used to set the number of decimals that are printed.

(b)	Study the use of `options(digits=xx)`.

(c)	Study how the function `format()` works. Note the use of `format()` together with `paste()` and `cat()`.

(d)	What does `print()` do?

(e)	Study the help file of `write.table()`.

(f)	The functions `prmatrix()` or `print()` can be used to output matrices to the console during execution of a function. This is very convenient for inspecting intermediate results.  Determine how the latter function differs from `cat()`.

(g)	Note the difference between the following statements:


``` r
colnames(state.x77)
#> [1] "Population" "Income"     "Illiteracy" "Life Exp"  
#> [5] "Murder"     "HS Grad"    "Frost"      "Area"
format(colnames(state.x77))
#> [1] "Population" "Income    " "Illiteracy" "Life Exp  "
#> [5] "Murder    " "HS Grad   " "Frost     " "Area      "
```

(h)	Study the following example carefully:


``` r
format.mns <- format (apply (state.x77, 2, mean))
format.names <- format (colnames (state.x77))
descrip.mns <- paste("Mean for variable", format.names, " = ", format.mns)
cat(descrip.mns, fill = max(nchar(descrip.mns)))
#> Mean for variable Population  =   4246.4200 
#> Mean for variable Income      =   4435.8000 
#> Mean for variable Illiteracy  =      1.1700 
#> Mean for variable Life Exp    =     70.8786 
#> Mean for variable Murder      =      7.3780 
#> Mean for variable HS Grad     =     53.1080 
#> Mean for variable Frost       =    104.4600 
#> Mean for variable Area        =  70735.8800
```

## Printing tables

Study the example below of how to represent the maximum and minimum value of the variables in the  state.x77 data set in a table with the names of the countries corresponding to the values.


``` r
mins <- apply(state.x77, 2, min)
maxs <- apply(state.x77, 2, max)
min.name <- character(ncol(state.x77))
min.name
#> [1] "" "" "" "" "" "" "" ""
for(i in 1:8) min.name[i] <- rownames(state.x77)[state.x77[,i] == mins[i]][1]
max.name <- character(8)
for(i in 1:8) max.name[i] <- rownames(state.x77)[state.x77 [,i] == maxs[i]][1]
my.table <- data.frame(mins, min.name, maxs, max.name)
dimnames(my.table) <- list(names(mins),c("Minimum", 
                                         "State with Min",
                                         "Maximum",
                                         "State with Max"))
colnames(my.table)[3] <- paste("     ", colnames(my.table)[3])
my.table
#>            Minimum State with Min       Maximum
#> Population  365.00         Alaska       21198.0
#> Income     3098.00    Mississippi        6315.0
#> Illiteracy    0.50           Iowa           2.8
#> Life Exp     67.96 South Carolina          73.6
#> Murder        1.40   North Dakota          15.1
#> HS Grad      37.80 South Carolina          67.3
#> Frost         0.00         Hawaii         188.0
#> Area       1049.00   Rhode Island      566432.0
#>            State with Max
#> Population     California
#> Income             Alaska
#> Illiteracy      Louisiana
#> Life Exp           Hawaii
#> Murder            Alabama
#> HS Grad              Utah
#> Frost              Nevada
#> Area               Alaska
```

An alternative version of the above table could be obtained with the following instructions:


``` r
cat (paste (format (    c  (" ", "Statistic", " ", names(mins))),
            format ( paste ("  ", c("  ", "Minimum", " ", format(mins)))),
            format (    c  ("State having", "Minimum", " ", min.name)),
            format (paste  ("       ", c(" ", "Maximum", " ", format(maxs)))),
            format (    c  ("State having","Maximum", " ", max.name))), 
              fill=TRUE)
#>                       State having                    State having 
#> Statistic     Minimum Minimum                Maximum  Maximum      
#>                                                                    
#> Population     365.00 Alaska                  21198.0 California   
#> Income        3098.00 Mississippi              6315.0 Alaska       
#> Illiteracy       0.50 Iowa                        2.8 Louisiana    
#> Life Exp        67.96 South Carolina             73.6 Hawaii       
#> Murder           1.40 North Dakota               15.1 Alabama      
#> HS Grad         37.80 South Carolina             67.3 Utah         
#> Frost            0.00 Hawaii                    188.0 Nevada       
#> Area          1049.00 Rhode Island           566432.0 Alaska
```

Make the necessary changes in the above lines of code to improve the column spacing.

## Communicating with the operating system

Study how the function `system()` works using the DOS instructions:  *“time”*,  *“date”* and *“dir”*.  *Hint*:  First study the help file of the R function `system()` and then the following instructions:


``` r
system (paste (Sys.getenv ("COMSPEC"), "/c", "time \t"),                      
         show.output.on.console = TRUE, invisible = TRUE)
system (paste (Sys.getenv ("COMSPEC"), "/c", "date \t"),                  
         show.output.on.console = TRUE, invisible = TRUE)
system (paste (Sys.getenv ("COMSPEC"), "/c", "dir c:\\"),                  
         show.output.on.console = TRUE, invisible = TRUE)
```

The R function `system()` can also be used together with Notepad  to create a text file during an R session:


``` r
system (paste (Sys.getenv ("COMSPEC"), "/c", 
               "notepad c:\\temp\\test.txt"),
        show.output.on.console = TRUE, invisible = TRUE)
```

(a)	Use `system()` to create a text file without terminating the R session. 
(b)	Use `system()` to write a function  `myfile.exists()` that checks if any specified file exists.

##	Exercise

::: {style="color: #80CC99;"}

1.	Construct tables displaying the values of all variables in the state.x77 data set separately for each region as defined in the R object `state.region`.

2.	Print a table from the state.x77 data set such that for each variable, an asterisk is placed after the maximum value for that variable. The numbers must line up correctly.

:::

## Tidyverse

*<span style="color:#FF9966">Tidyverse</span>* is a collection or *ecosystem* of R packages that use the same data structures for data manipulation and exploration. With the command `library (tidyverse)`, the core packages listed in Table \@ref(tab:TidyverseCore) will also be loaded. A selection of other packages from the tidyverse collection is given in Table \@ref(tab:TidyverseOther).

Table: (\#tab:TidyverseCore) Additional core tidyverse packages.

| *<span style="color:#CC99FF">Package</span>* | *<span style="color:#CC99FF">Purpose</span>*  |
| ------ | --------------- | 
| `dplyr`   |	Data manipulation |
| `tidyr`   |	Data tidying |
| `tibble`  |	Similar to data frames |
| `readr`   |	Data import |
| `ggplot2` |	Data visualisation (see Chapter 10) |
| `stringr` |	String manipulation |
| `forcats` |	Factor variable manipulation |
| `purr`    |	Functional programming |

Table: (\#tab:TidyverseOther) Selection of packages from tidyverse.

| *<span style="color:#CC99FF">Package</span>* | *<span style="color:#CC99FF">Purpose</span>*  |
| ------ | --------------- | 
| `hms`, `lubridate` |	Working with date/time vectors |
| `feather`          |	Sharing with Python and other languages |
| `haven`            |	Importing SPSS, SAS and Stata files |
| `httr`             |	Sharing with web interfaces |
| `jsonlite`         |	Java script (JSON) |
| `rvest`            |	Web scraping |
| `readxl`           |	Reading *<span style="color:#FFB3B3">.xls</span>* and *<span style="color:#FFB3B3">.xlsx</span>* files |
| `xml2`             |	XML |
| `modelr`           |	Modelling within a pipeline |
| `broom`            |	Turning models into tidy data |


### Tibbles

A *<span style="color:#FF9966">tibble</span>* is a new version of a dataframe. Tibbles have an enhanced `print()` method which makes them easier to use with large datasets containing complex objects. To create a tibble from the dataframe iris, we use the commands:


``` r
library ("tidyverse")
#> ── Attaching core tidyverse packages ──── tidyverse 2.0.0 ──
#> ✔ dplyr     1.1.4     ✔ readr     2.1.5
#> ✔ forcats   1.0.0     ✔ stringr   1.5.1
#> ✔ ggplot2   3.5.2     ✔ tibble    3.3.0
#> ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
#> ✔ purrr     1.1.0     
#> ── Conflicts ────────────────────── tidyverse_conflicts() ──
#> ✖ dplyr::filter() masks stats::filter()
#> ✖ dplyr::lag()    masks stats::lag()
#> ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
iris.tibble <- tibble(iris)
iris.tibble
#> # A tibble: 150 × 5
#>    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
#>           <dbl>       <dbl>        <dbl>       <dbl> <fct>  
#>  1          5.1         3.5          1.4         0.2 setosa 
#>  2          4.9         3            1.4         0.2 setosa 
#>  3          4.7         3.2          1.3         0.2 setosa 
#>  4          4.6         3.1          1.5         0.2 setosa 
#>  5          5           3.6          1.4         0.2 setosa 
#>  6          5.4         3.9          1.7         0.4 setosa 
#>  7          4.6         3.4          1.4         0.3 setosa 
#>  8          5           3.4          1.5         0.2 setosa 
#>  9          4.4         2.9          1.4         0.2 setosa 
#> 10          4.9         3.1          1.5         0.1 setosa 
#> # ℹ 140 more rows
```

Tibbles can also be formed from vectors automatically creating a column vector.


``` r
tibble(x = fruit)   # data set fruit in package stringr
#> # A tibble: 80 × 1
#>    x           
#>    <chr>       
#>  1 apple       
#>  2 apricot     
#>  3 avocado     
#>  4 banana      
#>  5 bell pepper 
#>  6 bilberry    
#>  7 blackberry  
#>  8 blackcurrant
#>  9 blood orange
#> 10 blueberry   
#> # ℹ 70 more rows
```

Matrices are also easily converted to tibbles.


``` r
X <- matrix (1:12,ncol=3)
tibble(X)
#> # A tibble: 4 × 1
#>   X[,1]  [,2]  [,3]
#>   <int> <int> <int>
#> 1     1     5     9
#> 2     2     6    10
#> 3     3     7    11
#> 4     4     8    12
```

Even lists can be converted to tibbles.


``` r
my.list <- list(a = 1:10, beta = exp(-3:3), 
                logic = c(TRUE,FALSE,FALSE,TRUE))

my.list
#> $a
#>  [1]  1  2  3  4  5  6  7  8  9 10
#> 
#> $beta
#> [1]  0.04978707  0.13533528  0.36787944  1.00000000
#> [5]  2.71828183  7.38905610 20.08553692
#> 
#> $logic
#> [1]  TRUE FALSE FALSE  TRUE
tibble (my.list)
#> # A tibble: 3 × 1
#>   my.list     
#>   <named list>
#> 1 <int [10]>  
#> 2 <dbl [7]>   
#> 3 <lgl [4]>
```

To create a tibble from scratch we can use the command:


``` r
my.dat <- tibble(x = 1:5, y = 1, z = y - x ^ 2)
my.dat
#> # A tibble: 5 × 3
#>       x     y     z
#>   <int> <dbl> <dbl>
#> 1     1     1     0
#> 2     2     1    -3
#> 3     3     1    -8
#> 4     4     1   -15
#> 5     5     1   -24
```

There are three major differences between tibbles and dataframes.

(a) As seen above, the print method for tibbles only shows the first 10 rows and uses fonts and colours for emphasis. It also only shows the columns that fit onto the screen and provides a summary of each column type. You can control the default print behaviour by setting options: `options(tibble.print_max = n, tibble.print_min = m)`. If there are more than $n$ rows, print only $m$ rows. Use `options(tibble.print_min = Inf)` to always show all rows and `options(tibble.width = Inf)` to always print all columns, regardless of the width of the screen.

(b) Tibbles are stricter with subsetting, always returning another tibble.


``` r
my.dat["y"]
#> # A tibble: 5 × 1
#>       y
#>   <dbl>
#> 1     1
#> 2     1
#> 3     1
#> 4     1
#> 5     1
```

<div style="margin-left: 25px; margin-right: 20px;">
To extract a column, there are three options:
</div>


``` r
my.dat$x
#> [1] 1 2 3 4 5
my.dat[["y"]]
#> [1] 1 1 1 1 1
my.dat[[3]]
#> [1]   0  -3  -8 -15 -24
```

<div style="margin-left: 25px; margin-right: 20px;">
Tibbles never do partial matching, and will return NULL with a warning if the column does not exist.
</div>

(c) Tibbles are also stricter with recycling, only allowing values of length one to be recycled. The first column with length different to one determines the number of rows in the tibble and conflicts will lead to an error. To create a tibble with zero rows, use the first row to have $0 \neq 1$ rows with the command


``` r
tibble(a = integer(), b = 1)
#> # A tibble: 0 × 2
#> # ℹ 2 variables: a <int>, b <dbl>
```

### Pipe operator

The pipe operator, `|>`, pipes an object forward into a function or call expression, something like `x |> f`, rather than $f(x)$.  A simple example to achieve the same result as the three commands with two intermediate objects, `car_data` and `cyl_means` created, would be a single call as shown below:


``` r
car_data <- mtcars[mtcars$hp > 100,]
cyl_means <- apply(car_data, 2, function(x, cyl) 
                                  { tapply(x, cyl, mean)
                                  }, cyl=car_data$cyl)
cyl_means
#>        mpg cyl     disp       hp     drat       wt     qsec
#> 4 25.90000   4 108.0500 111.0000 3.940000 2.146500 17.75000
#> 6 19.74286   6 183.3143 122.2857 3.585714 3.117143 17.97714
#> 8 15.10000   8 353.1000 209.2143 3.229286 3.999214 16.77214
#>          vs        am     gear     carb
#> 4 1.0000000 1.0000000 4.500000 2.000000
#> 6 0.5714286 0.4285714 3.857143 3.428571
#> 8 0.0000000 0.1428571 3.285714 3.500000
  
mtcars |>
  filter(hp > 100) |>
  group_by(cyl) |>
  summarise(across(everything(), mean))
#> # A tibble: 3 × 11
#>     cyl   mpg  disp    hp  drat    wt  qsec    vs    am
#>   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#> 1     4  25.9  108.  111   3.94  2.15  17.8 1     1    
#> 2     6  19.7  183.  122.  3.59  3.12  18.0 0.571 0.429
#> 3     8  15.1  353.  209.  3.23  4.00  16.8 0     0.143
#> # ℹ 2 more variables: gear <dbl>, carb <dbl>
```

The first pipe operator `%>%` was created in the package `magrittr`. This package is automatically loaded when `tidyverse` is attached. The following call with therefore have a similar outcome:


``` r
mtcars %>%
  filter(hp > 100) %>%
  group_by(cyl) %>%
  summarise(across(everything(), mean))
#> # A tibble: 3 × 11
#>     cyl   mpg  disp    hp  drat    wt  qsec    vs    am
#>   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#> 1     4  25.9  108.  111   3.94  2.15  17.8 1     1    
#> 2     6  19.7  183.  122.  3.59  3.12  18.0 0.571 0.429
#> 3     8  15.1  353.  209.  3.23  4.00  16.8 0     0.143
#> # ℹ 2 more variables: gear <dbl>, carb <dbl>
```

From R version 4.1.0 the pipe operator `|>` is directly built into R and can therefore be used at any time without having to attach another package.

The dataframe (or tibble) is piped forward to the function `filter()`, i.e. telling R that the variable `hp` belongs to `mtcars` and the sub-tibble with only `hp > 100` values, is piped forward to the `group_by()` function. 

### Tidy data

Tidy data is data where every column represents a single variable, every row is a single observation and in every cell is a single value. The terms ‘variable’ and ‘observation’ are important – a variable contains all values that measure the same feature across units; an observation contains all values on a single unit, across features. For creating a tidy data set there are five main types of operations:

####	Pivotting

The functions `pivot_longer()` and `pivot_wider()` are used to convert data into long or wide format respectively. Consider the long data set Rabbit in package `MASS`.


``` r
library (MASS)
#> 
#> Attaching package: 'MASS'
#> The following object is masked from 'package:dplyr':
#> 
#>     select
tibble (Rabbit)
#> # A tibble: 60 × 5
#>    BPchange   Dose Run   Treatment Animal
#>       <dbl>  <dbl> <fct> <fct>     <fct> 
#>  1     0.5    6.25 C1    Control   R1    
#>  2     4.5   12.5  C1    Control   R1    
#>  3    10     25    C1    Control   R1    
#>  4    26     50    C1    Control   R1    
#>  5    37    100    C1    Control   R1    
#>  6    32    200    C1    Control   R1    
#>  7     1      6.25 C2    Control   R2    
#>  8     1.25  12.5  C2    Control   R2    
#>  9     4     25    C2    Control   R2    
#> 10    12     50    C2    Control   R2    
#> # ℹ 50 more rows
```

The command below, pivots the tibble into a wide format.


``` r
rabbit <- Rabbit |> 
  pivot_wider(names_from = c(Animal, Treatment, Run), values_from = BPchange)
rabbit
#> # A tibble: 6 × 11
#>     Dose R1_Control_C1 R2_Control_C2 R3_Control_C3
#>    <dbl>         <dbl>         <dbl>         <dbl>
#> 1   6.25           0.5          1             0.75
#> 2  12.5            4.5          1.25          3   
#> 3  25             10            4             3   
#> 4  50             26           12            14   
#> 5 100             37           27            22   
#> 6 200             32           29            24   
#> # ℹ 7 more variables: R4_Control_C4 <dbl>,
#> #   R5_Control_C5 <dbl>, R1_MDL_M1 <dbl>, R2_MDL_M2 <dbl>,
#> #   R3_MDL_M3 <dbl>, R4_MDL_M4 <dbl>, R5_MDL_M5 <dbl>
```

For the converse, the command below pivots the wide tibble, `rabbit`, to long format. 


``` r
rabbit |> pivot_longer(cols = -Dose, names_to = "Treat.comb", 
                       values_to = "BPchange")
#> # A tibble: 60 × 3
#>     Dose Treat.comb    BPchange
#>    <dbl> <chr>            <dbl>
#>  1  6.25 R1_Control_C1     0.5 
#>  2  6.25 R2_Control_C2     1   
#>  3  6.25 R3_Control_C3     0.75
#>  4  6.25 R4_Control_C4     1.25
#>  5  6.25 R5_Control_C5     1.5 
#>  6  6.25 R1_MDL_M1         1.25
#>  7  6.25 R2_MDL_M2         1.4 
#>  8  6.25 R3_MDL_M3         0.75
#>  9  6.25 R4_MDL_M4         2.6 
#> 10  6.25 R5_MDL_M5         2.4 
#> # ℹ 50 more rows
```

Note that the column headings now form a single variable. To separate the combination of variables into different columns, we need the following command:
  

``` r
rabbit |> 
  pivot_longer(cols = -Dose,
               names_to = c("animal","treatment","run"),
               names_pattern ="(.*)_(.*)_(.*)",
               values_to = "BPchange")
#> # A tibble: 60 × 5
#>     Dose animal treatment run   BPchange
#>    <dbl> <chr>  <chr>     <chr>    <dbl>
#>  1  6.25 R1     Control   C1        0.5 
#>  2  6.25 R2     Control   C2        1   
#>  3  6.25 R3     Control   C3        0.75
#>  4  6.25 R4     Control   C4        1.25
#>  5  6.25 R5     Control   C5        1.5 
#>  6  6.25 R1     MDL       M1        1.25
#>  7  6.25 R2     MDL       M2        1.4 
#>  8  6.25 R3     MDL       M3        0.75
#>  9  6.25 R4     MDL       M4        2.6 
#> 10  6.25 R5     MDL       M5        2.4 
#> # ℹ 50 more rows
```

#### Rectangling {#rectangling}

Rectangling is used to place lists in clean data rectangular format. Consider the list below:
  

``` r
df <- tibble(
  character = c("Toothless", "Dory"),
  metadata = list(
    list(
      species = "dragon",
      color = "black",
      films = c(
        "How to Train Your Dragon",
        "How to Train Your Dragon 2",
        "How to Train Your Dragon: The Hidden World"
      )
    ),
    list(
      species = "blue tang",
      color = "blue",
      films = c("Finding Nemo", "Finding Dory")
    )
  )
)
df
#> # A tibble: 2 × 2
#>   character metadata        
#>   <chr>     <list>          
#> 1 Toothless <named list [3]>
#> 2 Dory      <named list [3]>
```

The following command places the two list items of metadata in a tibble with two rows, one for Toothless and one for Dory. Each of the three components – species, color and films – forms a column in the new tibble.


``` r
df |> unnest_auto(metadata)
#> Using `unnest_wider(metadata)`; elements have 3 names in common
#> # A tibble: 2 × 4
#>   character species   color films    
#>   <chr>     <chr>     <chr> <list>   
#> 1 Toothless dragon    black <chr [3]>
#> 2 Dory      blue tang blue  <chr [2]>
```

In addition to the function `unnest_auto()`, the functions `unnest_wider()` and `unnest_longer()` places the list components into columns or rows respectively. The `unnest_auto()` selects the most appropriate of `unnest_wider()` or `unnest_longer()`. In the first line of the output above, the `unnest_auto()` function states Using `'unnest_wider(metadata)'`, indicating that the wider application was used for this list.

The function `hoist()` can be used to reach down multiple layers. 


``` r
df |> hoist(metadata, "species", 
            first_film = list("films", 1L),                
            third_film = list("films", 3L))
#> # A tibble: 2 × 5
#>   character species   first_film     third_film metadata    
#>   <chr>     <chr>     <chr>          <chr>      <list>      
#> 1 Toothless dragon    How to Train … How to Tr… <named list>
#> 2 Dory      blue tang Finding Nemo   <NA>       <named list>
```

Note that `hoist()` also allows us to extract only certain components.

####	Nesting

In nesting, a tibble of lists are created. In the example below, we create a tibble with three rows – one for each species – and two columns where each element in the second column is a $50 \times 4$ matrix of the four variables measured on $50$ samples from that particular species.


``` r
iris |> nest(data = !Species)
#> # A tibble: 3 × 2
#>   Species    data             
#>   <fct>      <list>           
#> 1 setosa     <tibble [50 × 4]>
#> 2 versicolor <tibble [50 × 4]>
#> 3 virginica  <tibble [50 × 4]>
```

We can also create tibbles with three columns where the data is grouped by ‘Petal’ and ‘Sepal’ in the first instance and by ‘width’ and ‘length’ in the second.


``` r
iris |> nest(petal = starts_with("Petal"), sepal = starts_with("Sepal"))
#> # A tibble: 3 × 3
#>   Species    petal             sepal            
#>   <fct>      <list>            <list>           
#> 1 setosa     <tibble [50 × 2]> <tibble [50 × 2]>
#> 2 versicolor <tibble [50 × 2]> <tibble [50 × 2]>
#> 3 virginica  <tibble [50 × 2]> <tibble [50 × 2]>
iris |> nest(width = contains("Width"), length = contains("Length"))
#> # A tibble: 3 × 3
#>   Species    width             length           
#>   <fct>      <list>            <list>           
#> 1 setosa     <tibble [50 × 2]> <tibble [50 × 2]>
#> 2 versicolor <tibble [50 × 2]> <tibble [50 × 2]>
#> 3 virginica  <tibble [50 × 2]> <tibble [50 × 2]>
```

The function `unnest()` is similar to the functions discussed in \@ref(rectangling), and can be used to simultaneously `unlist` several column from a simple table containing lists.
  

``` r
df <- tibble(x = 1:3,
             y = list(NULL,
                      tibble(a = 1, b = 2),
                      tibble(a = 1:3, b = 3:1)))
df
#> # A tibble: 3 × 2
#>       x y               
#>   <int> <list>          
#> 1     1 <NULL>          
#> 2     2 <tibble [1 × 2]>
#> 3     3 <tibble [3 × 2]>
  
df |> unnest(y)
#> # A tibble: 4 × 3
#>       x     a     b
#>   <int> <dbl> <dbl>
#> 1     2     1     2
#> 2     3     1     3
#> 3     3     2     2
#> 4     3     3     1
df %>% unnest(y, keep_empty = TRUE)
#> # A tibble: 5 × 3
#>       x     a     b
#>   <int> <dbl> <dbl>
#> 1     1    NA    NA
#> 2     2     1     2
#> 3     3     1     3
#> 4     3     2     2
#> 5     3     3     1
  
df <- tibble(a = list(c("a", "b"), "c"),
             b = list(1:2, 3),
             c = c(11, 22))
df
#> # A tibble: 2 × 3
#>   a         b             c
#>   <list>    <list>    <dbl>
#> 1 <chr [2]> <int [2]>    11
#> 2 <chr [1]> <dbl [1]>    22
  
df |> unnest(c(a, b))
#> # A tibble: 3 × 3
#>   a         b     c
#>   <chr> <dbl> <dbl>
#> 1 a         1    11
#> 2 b         2    11
#> 3 c         3    22
df |> unnest(a) %>% unnest(b)
#> # A tibble: 5 × 3
#>   a         b     c
#>   <chr> <dbl> <dbl>
#> 1 a         1    11
#> 2 a         2    11
#> 3 b         1    11
#> 4 b         2    11
#> 5 c         3    22
```
  
#### Splitting and combining
  
We use the functions `separate()` and `extract()` for separating columns and `unite()` to combine columns into a single column. The function `separate()` divides the data, while `extract()` picks out a part of the data.


``` r
df <- data.frame(x = c(NA, "a.b", "a.d", "b.c"))
df  
#>      x
#> 1 <NA>
#> 2  a.b
#> 3  a.d
#> 4  b.c
  
df |> separate(x, c("A", "B"))
#>      A    B
#> 1 <NA> <NA>
#> 2    a    b
#> 3    a    d
#> 4    b    c
df |> separate(x, c(NA, "B"))
#>      B
#> 1 <NA>
#> 2    b
#> 3    d
#> 4    c
  
df |> extract(x, "A")
#>      A
#> 1 <NA>
#> 2    a
#> 3    a
#> 4    b
df |> extract(x, c("A", "B"),"([[:alnum:]]+).([[:alnum:]]+)")
#>      A    B
#> 1 <NA> <NA>
#> 2    a    b
#> 3    a    d
#> 4    b    c
  
df <- expand_grid(x = c("a", NA), y = c("b", NA))
df
#> # A tibble: 4 × 2
#>   x     y    
#>   <chr> <chr>
#> 1 a     b    
#> 2 a     <NA> 
#> 3 <NA>  b    
#> 4 <NA>  <NA>
  
  df |> unite("z", x:y, remove = FALSE)
#> # A tibble: 4 × 3
#>   z     x     y    
#>   <chr> <chr> <chr>
#> 1 a_b   a     b    
#> 2 a_NA  a     <NA> 
#> 3 NA_b  <NA>  b    
#> 4 NA_NA <NA>  <NA>
  df |> unite("z", x:y, na.rm = TRUE, remove = FALSE)
#> # A tibble: 4 × 3
#>   z     x     y    
#>   <chr> <chr> <chr>
#> 1 "a_b" a     b    
#> 2 "a"   a     <NA> 
#> 3 "b"   <NA>  b    
#> 4 ""    <NA>  <NA>
```
  
#### Dealing with missing values
  
The functions `complete()`, `drop_na()`, `fill()` and `replace_na()` are the most important for treatment of missing values.
  

``` r
df <- tibble(group = c(1:2, 1),
             item_id = c(1:2, 2),
             item_name = c("a", "b", "b"),
             value1 = 1:3,
             value2 = 4:6)
df
#> # A tibble: 3 × 5
#>   group item_id item_name value1 value2
#>   <dbl>   <dbl> <chr>      <int>  <int>
#> 1     1       1 a              1      4
#> 2     2       2 b              2      5
#> 3     1       2 b              3      6
  
df |> complete(group, nesting(item_id, item_name))
#> # A tibble: 4 × 5
#>   group item_id item_name value1 value2
#>   <dbl>   <dbl> <chr>      <int>  <int>
#> 1     1       1 a              1      4
#> 2     1       2 b              3      6
#> 3     2       1 a             NA     NA
#> 4     2       2 b              2      5
df |> complete(group, nesting(item_id, item_name), 
                 fill = list(value1 = 0))
#> # A tibble: 4 × 5
#>   group item_id item_name value1 value2
#>   <dbl>   <dbl> <chr>      <int>  <int>
#> 1     1       1 a              1      4
#> 2     1       2 b              3      6
#> 3     2       1 a              0     NA
#> 4     2       2 b              2      5
df <- tibble(x = c(1, 2, NA), y = c("a", NA, "b"))
df
#> # A tibble: 3 × 2
#>       x y    
#>   <dbl> <chr>
#> 1     1 a    
#> 2     2 <NA> 
#> 3    NA b
  
df |> replace_na(list(x = 0, y = "unknown"))
#> # A tibble: 3 × 2
#>       x y      
#>   <dbl> <chr>  
#> 1     1 a      
#> 2     2 unknown
#> 3     0 b
  
df |> drop_na()
#> # A tibble: 1 × 2
#>       x y    
#>   <dbl> <chr>
#> 1     1 a
df |> drop_na(x)
#> # A tibble: 2 × 2
#>       x y    
#>   <dbl> <chr>
#> 1     1 a    
#> 2     2 <NA>
```
  
### Package `dplyr` {#dplyr}

The main data manipulation functions is found in the package `dplyr`. The functions are referred to as “verbs”, since each performs a particular operation of data manipulation. The verbs are grouped in Table \@ref(tab:dplyr) according to operations on columns, rows or groups of rows.

Table: (\#tab:dplyr) Verbs for data manipulation in dplyr.

| *<span style="color:#CC99FF">Verb</span>* | *<span style="color:#CC99FF">Operates on</span>*  |
| ------ | --------------- | 
| `select()`   |	Columns |
| `rename()`   |	Columns |
| `mutate()`   |	Columns |
| `relocate()` |	Columns |
| `filter()`   |	Rows |
| `arrange()`  |	Rows |
| `slice()`    |	Rows |
| `group_by()` |	Rows |
| `summarise()`|	Group of rows |

The functioning of the verbs will be illustrated with `UScereal` in the package `MASS`.


``` r
library (MASS)
cereal <- tibble (UScereal)
cereal
#> # A tibble: 65 × 11
#>    mfr   calories protein   fat sodium fibre carbo sugars
#>    <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#>  1 N         212.   12.1   3.03   394. 30.3   15.2  18.2 
#>  2 K         212.   12.1   3.03   788. 27.3   21.2  15.2 
#>  3 K         100     8     0      280  28     16     0   
#>  4 G         147.    2.67  2.67   240   2     14    13.3 
#>  5 K         110     2     0      125   1     11    14   
#>  6 G         173.    4     2.67   280   2.67  24    10.7 
#>  7 R         134.    2.99  1.49   299.  5.97  22.4   8.96
#>  8 P         134.    4.48  0      313.  7.46  19.4   7.46
#>  9 Q         160     1.33  2.67   293.  0     16    16   
#> 10 G          88     4.8   1.6    232   1.6   13.6   0.8 
#> # ℹ 55 more rows
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
```

The function `select()` allows for extracting one or more columns from a data set. The columns can be names or referred to by index. Using the function `everything()` in conjunction with `select()` is useful to sort or reorder the columns of a data set.


``` r
dplyr::select (cereal, calories)        # select only column calories
#> # A tibble: 65 × 1
#>    calories
#>       <dbl>
#>  1     212.
#>  2     212.
#>  3     100 
#>  4     147.
#>  5     110 
#>  6     173.
#>  7     134.
#>  8     134.
#>  9     160 
#> 10      88 
#> # ℹ 55 more rows
dplyr::select (cereal, calories, fat)   # select two columns
#> # A tibble: 65 × 2
#>    calories   fat
#>       <dbl> <dbl>
#>  1     212.  3.03
#>  2     212.  3.03
#>  3     100   0   
#>  4     147.  2.67
#>  5     110   0   
#>  6     173.  2.67
#>  7     134.  1.49
#>  8     134.  0   
#>  9     160   2.67
#> 10      88   1.6 
#> # ℹ 55 more rows
dplyr::select (cereal, c(5,7:8))        # select by index
#> # A tibble: 65 × 3
#>    sodium carbo sugars
#>     <dbl> <dbl>  <dbl>
#>  1   394.  15.2  18.2 
#>  2   788.  21.2  15.2 
#>  3   280   16     0   
#>  4   240   14    13.3 
#>  5   125   11    14   
#>  6   280   24    10.7 
#>  7   299.  22.4   8.96
#>  8   313.  19.4   7.46
#>  9   293.  16    16   
#> 10   232   13.6   0.8 
#> # ℹ 55 more rows
dplyr::select (cereal, -c(1,9,11))      # select columns to exclude
#> # A tibble: 65 × 8
#>    calories protein   fat sodium fibre carbo sugars
#>       <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#>  1     212.   12.1   3.03   394. 30.3   15.2  18.2 
#>  2     212.   12.1   3.03   788. 27.3   21.2  15.2 
#>  3     100     8     0      280  28     16     0   
#>  4     147.    2.67  2.67   240   2     14    13.3 
#>  5     110     2     0      125   1     11    14   
#>  6     173.    4     2.67   280   2.67  24    10.7 
#>  7     134.    2.99  1.49   299.  5.97  22.4   8.96
#>  8     134.    4.48  0      313.  7.46  19.4   7.46
#>  9     160     1.33  2.67   293.  0     16    16   
#> 10      88     4.8   1.6    232   1.6   13.6   0.8 
#> # ℹ 55 more rows
#> # ℹ 1 more variable: potassium <dbl>
dplyr::select (cereal, calories, fibre, everything()) 
#> # A tibble: 65 × 11
#>    calories fibre mfr   protein   fat sodium carbo sugars
#>       <dbl> <dbl> <fct>   <dbl> <dbl>  <dbl> <dbl>  <dbl>
#>  1     212. 30.3  N       12.1   3.03   394.  15.2  18.2 
#>  2     212. 27.3  K       12.1   3.03   788.  21.2  15.2 
#>  3     100  28    K        8     0      280   16     0   
#>  4     147.  2    G        2.67  2.67   240   14    13.3 
#>  5     110   1    K        2     0      125   11    14   
#>  6     173.  2.67 G        4     2.67   280   24    10.7 
#>  7     134.  5.97 R        2.99  1.49   299.  22.4   8.96
#>  8     134.  7.46 P        4.48  0      313.  19.4   7.46
#>  9     160   0    Q        1.33  2.67   293.  16    16   
#> 10      88   1.6  G        4.8   1.6    232   13.6   0.8 
#> # ℹ 55 more rows
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
      # reorder with calories first, followed by fibre
```

The `rename()` function changes one of more column names. The companion function `rename_with()` can be used to apply a function to column headings, such as `tolower()` and `toupper()` to change the case of column headings.


``` r
rename (cereal, Manufacturer=mfr)
#> # A tibble: 65 × 11
#>    Manufacturer calories protein   fat sodium fibre carbo
#>    <fct>           <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>
#>  1 N                212.   12.1   3.03   394. 30.3   15.2
#>  2 K                212.   12.1   3.03   788. 27.3   21.2
#>  3 K                100     8     0      280  28     16  
#>  4 G                147.    2.67  2.67   240   2     14  
#>  5 K                110     2     0      125   1     11  
#>  6 G                173.    4     2.67   280   2.67  24  
#>  7 R                134.    2.99  1.49   299.  5.97  22.4
#>  8 P                134.    4.48  0      313.  7.46  19.4
#>  9 Q                160     1.33  2.67   293.  0     16  
#> 10 G                 88     4.8   1.6    232   1.6   13.6
#> # ℹ 55 more rows
#> # ℹ 4 more variables: sugars <dbl>, shelf <int>,
#> #   potassium <dbl>, vitamins <fct>
rename_with (cereal, toupper, starts_with("F"))
#> # A tibble: 65 × 11
#>    mfr   calories protein   FAT sodium FIBRE carbo sugars
#>    <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#>  1 N         212.   12.1   3.03   394. 30.3   15.2  18.2 
#>  2 K         212.   12.1   3.03   788. 27.3   21.2  15.2 
#>  3 K         100     8     0      280  28     16     0   
#>  4 G         147.    2.67  2.67   240   2     14    13.3 
#>  5 K         110     2     0      125   1     11    14   
#>  6 G         173.    4     2.67   280   2.67  24    10.7 
#>  7 R         134.    2.99  1.49   299.  5.97  22.4   8.96
#>  8 P         134.    4.48  0      313.  7.46  19.4   7.46
#>  9 Q         160     1.33  2.67   293.  0     16    16   
#> 10 G          88     4.8   1.6    232   1.6   13.6   0.8 
#> # ℹ 55 more rows
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
```


New variables can be added or created from existing columns with the function `mutate()`. The newly formed variables are immediately available for creating more variables. Variables can be removed by transforming them to `NULL` or using the `.keep` argument.


``` r
mutate (cereal, fat.vs.pr = fat/protein, mfr=NULL) |>
     dplyr::select (fat.vs.pr, everything())
#> # A tibble: 65 × 11
#>    fat.vs.pr calories protein   fat sodium fibre carbo
#>        <dbl>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>
#>  1     0.250     212.   12.1   3.03   394. 30.3   15.2
#>  2     0.250     212.   12.1   3.03   788. 27.3   21.2
#>  3     0         100     8     0      280  28     16  
#>  4     1         147.    2.67  2.67   240   2     14  
#>  5     0         110     2     0      125   1     11  
#>  6     0.667     173.    4     2.67   280   2.67  24  
#>  7     0.5       134.    2.99  1.49   299.  5.97  22.4
#>  8     0         134.    4.48  0      313.  7.46  19.4
#>  9     2.00      160     1.33  2.67   293.  0     16  
#> 10     0.333      88     4.8   1.6    232   1.6   13.6
#> # ℹ 55 more rows
#> # ℹ 4 more variables: sugars <dbl>, shelf <int>,
#> #   potassium <dbl>, vitamins <fct>
mutate (cereal, fat.vs.pr = fat/protein, 
                 comb.var = sodium + fat.vs.pr,
                 new.var=1:nrow(cereal), .keep="used")
#> # A tibble: 65 × 6
#>    protein   fat sodium fat.vs.pr comb.var new.var
#>      <dbl> <dbl>  <dbl>     <dbl>    <dbl>   <int>
#>  1   12.1   3.03   394.     0.250     394.       1
#>  2   12.1   3.03   788.     0.250     788.       2
#>  3    8     0      280      0         280        3
#>  4    2.67  2.67   240      1         241        4
#>  5    2     0      125      0         125        5
#>  6    4     2.67   280      0.667     281.       6
#>  7    2.99  1.49   299.     0.5       299.       7
#>  8    4.48  0      313.     0         313.       8
#>  9    1.33  2.67   293.     2.00      295.       9
#> 10    4.8   1.6    232      0.333     232.      10
#> # ℹ 55 more rows
```

Why is it useful to pipe the mutated tibble above to select? In comparison, `relocate()` makes it easy to move blocks of columns.


``` r
relocate (cereal, shelf)
#> # A tibble: 65 × 11
#>    shelf mfr   calories protein   fat sodium fibre carbo
#>    <int> <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>
#>  1     3 N         212.   12.1   3.03   394. 30.3   15.2
#>  2     3 K         212.   12.1   3.03   788. 27.3   21.2
#>  3     3 K         100     8     0      280  28     16  
#>  4     1 G         147.    2.67  2.67   240   2     14  
#>  5     2 K         110     2     0      125   1     11  
#>  6     3 G         173.    4     2.67   280   2.67  24  
#>  7     1 R         134.    2.99  1.49   299.  5.97  22.4
#>  8     3 P         134.    4.48  0      313.  7.46  19.4
#>  9     2 Q         160     1.33  2.67   293.  0     16  
#> 10     1 G          88     4.8   1.6    232   1.6   13.6
#> # ℹ 55 more rows
#> # ℹ 3 more variables: sugars <dbl>, potassium <dbl>,
#> #   vitamins <fct>
relocate (cereal, cal=calories, .before = fat)
#> # A tibble: 65 × 11
#>    mfr   protein   cal   fat sodium fibre carbo sugars shelf
#>    <fct>   <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl> <int>
#>  1 N       12.1   212.  3.03   394. 30.3   15.2  18.2      3
#>  2 K       12.1   212.  3.03   788. 27.3   21.2  15.2      3
#>  3 K        8     100   0      280  28     16     0        3
#>  4 G        2.67  147.  2.67   240   2     14    13.3      1
#>  5 K        2     110   0      125   1     11    14        2
#>  6 G        4     173.  2.67   280   2.67  24    10.7      3
#>  7 R        2.99  134.  1.49   299.  5.97  22.4   8.96     1
#>  8 P        4.48  134.  0      313.  7.46  19.4   7.46     3
#>  9 Q        1.33  160   2.67   293.  0     16    16        2
#> 10 G        4.8    88   1.6    232   1.6   13.6   0.8      1
#> # ℹ 55 more rows
#> # ℹ 2 more variables: potassium <dbl>, vitamins <fct>
relocate(cereal, where(is.factor), .after=last_col())
#> # A tibble: 65 × 11
#>    calories protein   fat sodium fibre carbo sugars shelf
#>       <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl> <int>
#>  1     212.   12.1   3.03   394. 30.3   15.2  18.2      3
#>  2     212.   12.1   3.03   788. 27.3   21.2  15.2      3
#>  3     100     8     0      280  28     16     0        3
#>  4     147.    2.67  2.67   240   2     14    13.3      1
#>  5     110     2     0      125   1     11    14        2
#>  6     173.    4     2.67   280   2.67  24    10.7      3
#>  7     134.    2.99  1.49   299.  5.97  22.4   8.96     1
#>  8     134.    4.48  0      313.  7.46  19.4   7.46     3
#>  9     160     1.33  2.67   293.  0     16    16        2
#> 10      88     4.8   1.6    232   1.6   13.6   0.8      1
#> # ℹ 55 more rows
#> # ℹ 3 more variables: potassium <dbl>, mfr <fct>,
#> #   vitamins <fct>
```

The `filter()` function select rows from a tibble, based on any operator that evaluates to a column of `TRUE` / `FALSE` values equal to the number of rows.


``` r
filter (cereal, fat<1)
#> # A tibble: 23 × 11
#>    mfr   calories protein   fat sodium fibre carbo sugars
#>    <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#>  1 K         100     8        0   280  28     16     0   
#>  2 K         110     2        0   125   1     11    14   
#>  3 P         134.    4.48     0   313.  7.46  19.4   7.46
#>  4 R         110     2        0   280   0     22     3   
#>  5 K         100     2        0   290   1     21     2   
#>  6 K         110     1        0    90   1     13    12   
#>  7 K         110     2        0   220   1     21     3   
#>  8 R         133.    2.67     0   253.  1.33  24     6.67
#>  9 K         147.    1.33     0   267.  1.33  18.7  14.7 
#> 10 K         125     3.75     0     0   3.75  17.5   8.75
#> # ℹ 13 more rows
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
filter (cereal, fat<1, mfr=="K")
#> # A tibble: 12 × 11
#>    mfr   calories protein   fat sodium fibre carbo sugars
#>    <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#>  1 K         100     8        0   280  28     16     0   
#>  2 K         110     2        0   125   1     11    14   
#>  3 K         100     2        0   290   1     21     2   
#>  4 K         110     1        0    90   1     13    12   
#>  5 K         110     2        0   220   1     21     3   
#>  6 K         147.    1.33     0   267.  1.33  18.7  14.7 
#>  7 K         125     3.75     0     0   3.75  17.5   8.75
#>  8 K         179.    4.48     0   358.  7.46  20.9  17.9 
#>  9 K         100     3        0   320   1     20     3   
#> 10 K         180     4        0     0   4     30    12   
#> 11 K         110     2        0   290   0     22     3   
#> 12 K         110     6        0   230   1     16     3   
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
filter (cereal, fat<1 | mfr=="K")
#> # A tibble: 32 × 11
#>    mfr   calories protein   fat sodium fibre carbo sugars
#>    <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#>  1 K         212.   12.1   3.03   788. 27.3   21.2  15.2 
#>  2 K         100     8     0      280  28     16     0   
#>  3 K         110     2     0      125   1     11    14   
#>  4 P         134.    4.48  0      313.  7.46  19.4   7.46
#>  5 R         110     2     0      280   0     22     3   
#>  6 K         100     2     0      290   1     21     2   
#>  7 K         110     1     0       90   1     13    12   
#>  8 K         220     6     6      280   8     20    14   
#>  9 K         110     2     0      220   1     21     3   
#> 10 R         133.    2.67  0      253.  1.33  24     6.67
#> # ℹ 22 more rows
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
filter(cereal, between(sugars, 10, 20))
#> # A tibble: 38 × 11
#>    mfr   calories protein   fat sodium fibre carbo sugars
#>    <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#>  1 N         212.   12.1   3.03   394. 30.3   15.2   18.2
#>  2 K         212.   12.1   3.03   788. 27.3   21.2   15.2
#>  3 G         147.    2.67  2.67   240   2     14     13.3
#>  4 K         110     2     0      125   1     11     14  
#>  5 G         173.    4     2.67   280   2.67  24     10.7
#>  6 Q         160     1.33  2.67   293.  0     16     16  
#>  7 G         160     1.33  4      280   0     17.3   12  
#>  8 G         220     6     4      280   4     26     14  
#>  9 G         110     1     1      180   0     12     13  
#> 10 K         110     1     0       90   1     13     12  
#> # ℹ 28 more rows
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
```

The verb `arrange()` refers to sorting the rows according to the values in one or more columns.


``` r
arrange (cereal, fibre)
#> # A tibble: 65 × 11
#>    mfr   calories protein   fat sodium fibre carbo sugars
#>    <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#>  1 Q        160     1.33  2.67   293.      0  16    16   
#>  2 G        160     1.33  4      280       0  17.3  12   
#>  3 G        110     1     1      180       0  12    13   
#>  4 R        110     2     0      280       0  22     3   
#>  5 G        110     1     1      180       0  12    13   
#>  6 P        147.    1.33  1.33   180       0  17.3  16   
#>  7 P        114.    2.27  0       51.1     0  12.5  17.0 
#>  8 G        147.    1.33  1.33   373.      0  20    12   
#>  9 P         82.7   0.752 0      135.      0  10.5   8.27
#> 10 G         73.3   1.33  0.667  173.      0  14     2   
#> # ℹ 55 more rows
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
arrange (cereal, -fibre)
#> # A tibble: 65 × 11
#>    mfr   calories protein   fat sodium fibre carbo sugars
#>    <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#>  1 N         212.   12.1   3.03   394. 30.3   15.2  18.2 
#>  2 K         100     8     0      280  28     16     0   
#>  3 K         212.   12.1   3.03   788. 27.3   21.2  15.2 
#>  4 P         440    12     0      680  12     68    12   
#>  5 P         364.    9.09  9.09   227.  9.09  39.4  12.1 
#>  6 P         179.    4.48  1.49   299.  8.96  16.4  20.9 
#>  7 K         220     6     6      280   8     20    14   
#>  8 P         134.    4.48  0      313.  7.46  19.4   7.46
#>  9 P         179.    4.48  2.99   239.  7.46  17.9  14.9 
#> 10 K         179.    4.48  0      358.  7.46  20.9  17.9 
#> # ℹ 55 more rows
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
arrange (cereal, fat, desc(mfr))
#> # A tibble: 65 × 11
#>    mfr   calories protein   fat sodium fibre carbo sugars
#>    <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#>  1 R        110     2         0  280    0     22     3   
#>  2 R        133.    2.67      0  253.   1.33  24     6.67
#>  3 R         97.3   0.885     0  212.   0     20.4   1.77
#>  4 Q         50     1         0    0    0     13     0   
#>  5 P        134.    4.48      0  313.   7.46  19.4   7.46
#>  6 P        114.    2.27      0   51.1  0     12.5  17.0 
#>  7 P        440    12         0  680   12     68    12   
#>  8 P         82.7   0.752     0  135.   0     10.5   8.27
#>  9 N        134.    4.48      0    0    5.97  28.4   0   
#> 10 N        134.    4.48      0    0    4.48  29.9   0   
#> # ℹ 55 more rows
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
```

The function `slice()` also allows for the selection of rows and works with a few helper functions: `slice_head()`, `slice_tail()`, `slice_sample()`, `slice_min()` and `slice_max()` to select the first few, last few, random sample, rows with lowest values or rows with highest values, respectively.


``` r
slice (cereal, 10:20)
#> # A tibble: 11 × 11
#>    mfr   calories protein   fat sodium fibre carbo sugars
#>    <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#>  1 G          88     4.8   1.6    232   1.6   13.6    0.8
#>  2 G         160     1.33  4      280   0     17.3   12  
#>  3 G         220     6     4      280   4     26     14  
#>  4 G         110     1     1      180   0     12     13  
#>  5 R         110     2     0      280   0     22      3  
#>  6 K         100     2     0      290   1     21      2  
#>  7 K         110     1     0       90   1     13     12  
#>  8 G         110     1     1      180   0     12     13  
#>  9 K         220     6     6      280   8     20     14  
#> 10 K         110     2     0      220   1     21      3  
#> 11 G         133.    2.67  1.33   187.  2.67  14.7   13.3
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
slice (cereal, -(10:20))
#> # A tibble: 54 × 11
#>    mfr   calories protein   fat sodium fibre carbo sugars
#>    <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#>  1 N         212.   12.1   3.03   394. 30.3   15.2  18.2 
#>  2 K         212.   12.1   3.03   788. 27.3   21.2  15.2 
#>  3 K         100     8     0      280  28     16     0   
#>  4 G         147.    2.67  2.67   240   2     14    13.3 
#>  5 K         110     2     0      125   1     11    14   
#>  6 G         173.    4     2.67   280   2.67  24    10.7 
#>  7 R         134.    2.99  1.49   299.  5.97  22.4   8.96
#>  8 P         134.    4.48  0      313.  7.46  19.4   7.46
#>  9 Q         160     1.33  2.67   293.  0     16    16   
#> 10 R         133.    2.67  0      253.  1.33  24     6.67
#> # ℹ 44 more rows
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
slice_tail (cereal, n=3)
#> # A tibble: 3 × 11
#>   mfr   calories protein   fat sodium fibre carbo sugars
#>   <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#> 1 R         149.    4.48  1.49   343.  4.48  25.4   4.48
#> 2 G         100     3     1      200   3     17     3   
#> 3 G         147.    2.67  1.33   267.  1.33  21.3  10.7 
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
slice_sample (cereal, n=8)
#> # A tibble: 8 × 11
#>   mfr   calories protein   fat sodium fibre carbo sugars
#>   <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#> 1 P         114.    3.41  1.14  159.   3.41  17.0   5.68
#> 2 K         147.    2.67  1.33   93.3  1.33  12    20   
#> 3 G         140     3     1     190    4     15    14   
#> 4 G         160     1.33  4     280    0     17.3  12   
#> 5 P         147.    1.33  1.33  180    0     17.3  16   
#> 6 P         364.    9.09  9.09  227.   9.09  39.4  12.1 
#> 7 G         100     3     1     200    3     17     3   
#> 8 K         125     3.75  0       0    3.75  17.5   8.75
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
slice_max (cereal, sodium, n=4)
#> # A tibble: 4 × 11
#>   mfr   calories protein   fat sodium fibre carbo sugars
#>   <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#> 1 K         212.   12.1   3.03   788.  27.3  21.2   15.2
#> 2 P         440    12     0      680   12    68     12  
#> 3 N         212.   12.1   3.03   394.  30.3  15.2   18.2
#> 4 G         147.    1.33  1.33   373.   0    20     12  
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
```

A grouped object can be formed with the `group_by()` function. At first glance, it appears similar to the ungrouped tibble, but grouping will prove useful further data manipulations.


``` r
cereal.mfr <- group_by(cereal, mfr)
cereal.mfr          # looks no different
#> # A tibble: 65 × 11
#> # Groups:   mfr [6]
#>    mfr   calories protein   fat sodium fibre carbo sugars
#>    <fct>    <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>  <dbl>
#>  1 N         212.   12.1   3.03   394. 30.3   15.2  18.2 
#>  2 K         212.   12.1   3.03   788. 27.3   21.2  15.2 
#>  3 K         100     8     0      280  28     16     0   
#>  4 G         147.    2.67  2.67   240   2     14    13.3 
#>  5 K         110     2     0      125   1     11    14   
#>  6 G         173.    4     2.67   280   2.67  24    10.7 
#>  7 R         134.    2.99  1.49   299.  5.97  22.4   8.96
#>  8 P         134.    4.48  0      313.  7.46  19.4   7.46
#>  9 Q         160     1.33  2.67   293.  0     16    16   
#> 10 G          88     4.8   1.6    232   1.6   13.6   0.8 
#> # ℹ 55 more rows
#> # ℹ 3 more variables: shelf <int>, potassium <dbl>,
#> #   vitamins <fct>
class(cereal)
#> [1] "tbl_df"     "tbl"        "data.frame"
class(cereal.mfr)   # but it is a grouped object
#> [1] "grouped_df" "tbl_df"     "tbl"        "data.frame"
```

The `summarise()` function allows for the computation of descriptive statistics. Operating on an ungrouped object, the overall statistic is computed, while the grouped object will provide the required statistics by group.


``` r
summarise(cereal.mfr, mean.cal = mean(calories), 
          median.carbo = median(carbo))
#> # A tibble: 6 × 3
#>   mfr   mean.cal median.carbo
#>   <fct>    <dbl>        <dbl>
#> 1 G         138.         15.7
#> 2 K         150.         20  
#> 3 N         160.         28.4
#> 4 P         195.         17.3
#> 5 Q         136.         16  
#> 6 R         125.         22.4
group_by(cereal, mfr, shelf) |> 
    summarise(mean.cal = mean(calories))
#> `summarise()` has grouped output by 'mfr'. You can override
#> using the `.groups` argument.
#> # A tibble: 15 × 3
#> # Groups:   mfr [6]
#>    mfr   shelf mean.cal
#>    <fct> <int>    <dbl>
#>  1 G         1    121. 
#>  2 G         2    117. 
#>  3 G         3    165. 
#>  4 K         1    117. 
#>  5 K         2    134. 
#>  6 K         3    174. 
#>  7 N         1    134. 
#>  8 N         3    212. 
#>  9 P         1     98.2
#> 10 P         2    147. 
#> 11 P         3    235. 
#> 12 Q         2    143. 
#> 13 Q         3    125  
#> 14 R         1    123. 
#> 15 R         3    133.
summarise(cereal, mean.cal = mean(calories), max.fat = max(fat), 
          median.carbo = median(carbo), sum.sugar = tibble(fivenum(sugars)))
#> Warning: Returning more (or less) than 1 row per `summarise()` group
#> was deprecated in dplyr 1.1.0.
#> ℹ Please use `reframe()` instead.
#> ℹ When switching from `summarise()` to `reframe()`,
#>   remember that `reframe()` always returns an ungrouped
#>   data frame and adjust accordingly.
#> Call `lifecycle::last_lifecycle_warnings()` to see where
#> this warning was generated.
#> # A tibble: 5 × 4
#>   mean.cal max.fat median.carbo sum.sugar$`fivenum(sugars)`
#>      <dbl>   <dbl>        <dbl>                       <dbl>
#> 1     149.    9.09         18.7                         0  
#> 2     149.    9.09         18.7                         4  
#> 3     149.    9.09         18.7                        12  
#> 4     149.    9.09         18.7                        14  
#> 5     149.    9.09         18.7                        20.9
```

Since the function `fivenum()` does not return a scalar value, but a vector, the output appears as a tibble above. Alternatively, the function `reframe()` can be used.


``` r
reframe(cereal, mean.cal = mean(calories), max.fat = max(fat), 
          median.carbo = median(carbo), sum.sugar = fivenum(sugars))
#> # A tibble: 5 × 4
#>   mean.cal max.fat median.carbo sum.sugar
#>      <dbl>   <dbl>        <dbl>     <dbl>
#> 1     149.    9.09         18.7       0  
#> 2     149.    9.09         18.7       4  
#> 3     149.    9.09         18.7      12  
#> 4     149.    9.09         18.7      14  
#> 5     149.    9.09         18.7      20.9
```

##	Exercise

::: {style="color: #80CC99;"}

1.	Use the `fish_encounters` in package `tidyr` to convert it into a wide format with fish IDs as the row variable and a column for each station. The entries in the cells should be '1' for a fish encounter and '0' otherwise.

2.	The `billboard` data set in package `tidyr` contains song rankings for billboard top 100 in the year 2000 with columns artist, track, date.enter and wk1 - w76 which contains the ranking of the song in each week after it entered the charts.

    (a)	Create a long data set listing the columns wk1 to w76 below each other in a single column called week and the associated rank position in a column called rank. Note that not all songs stayed on the charts for the entire 76 weeks. *Hint*, use `values_drop_na = TRUE`.

    (b)	Use the command `nest()` to create a tibble with one row for each artist-track combination and a rank.hist variable where each cell contains a tibble with 76 rows (one for each week) and a column for each of date.entered, week and rank.

3.	Another form of mutation, is to join together two separate data sets. Study the working of the functions `inner_join()`, `left_join()`, `right_join()` and `full_join()` together with the output of the commands:


``` r
band_members %>% inner_join(band_instruments)
band_members %>% left_join(band_instruments)
band_members %>% right_join(band_instruments)
band_members %>% full_join(band_instruments)
band_members %>% full_join(band_instruments2, 
                              by = c("name" = "artist"))
```

4.	Use `state.x77` in package `MASS` to create a tibble called `USA.states` with the names of the states in the first column. *Hint*: first convert the matrix to a dataframe to get neater column names.

    (a)	Add the column `state.region`, also from package `MASS`, to USA.states in the second position.
    
    (b)	Select only the columns State, Region, Population, Income, Illiteracy, Life Exp and Area, then use the pipe operator to reorder the columns such that Area appears between Region and Population.
    
    (c)	Add a column `Pop.Density` for the Population density in number per square miles. Note that the population values in `state.x77` represent 1000's of persons. This column should appear between Population and Income.
    
    (d)	In a single command, using the pipe operator, create a tibble called `USA.groups` where you:
    * select only states with an area < 500 000 square miles;
    *	order the rows according to decreasing population density;
    * group by Region
    
    (e)	Compute the mean income and median life expectancy per region.

:::

