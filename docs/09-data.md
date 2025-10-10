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

Study the helpfiles of these functions for reading into R binary data, SAS XPORT format, Weka Attribute-Relation File Format, the Xbase family of database languages dBase, Clipper and FoxPro, Stata, Epi Info and EpiData files, Minitab portable worksheets, Octave text files, data.dump files that were produced in S version 3, SPSS save or export files, SAS data sets to be converted to ssd format and Systat files.

### ssd format footnote

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







