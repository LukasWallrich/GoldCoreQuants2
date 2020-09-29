# Getting started with R

## Install R on your computer

R is highly customizable, so that your life is a lot easier if you use your own copy rather than a shared copy on a university computer. To install it on your computer, follow these detailed instructions in [Section 1.4 of the R for Data Science book](https://r4ds.had.co.nz/introduction.html){target="_blank"} 

Then **start exploring** - using R is a skill that comes with practice, not with reading about it. Most example from these notes can be copy-pasted or typed into R. Do that whenever possible to see what you are able to do and where you still have questions.

## Some key concepts

If you understand the following concepts, using R will make a lot more sense. 

### R, RStudio and R packages

You can think about the different parts of R in terms of a computer or smartphone. In that analogy:

* **R is the processor and operating system** that carries out everything under the hood. It also contains many basic capabilities for everyday tasks

* **RStudio is the user interface.** The following parts are most relevant:
  + **Console** to type and run commands (usually to try things out or do things that need to be done once only)
  + **File editor** to develop production code (i.e. code needed to reproduce your analyses and create reports). Here you edit .Rmd files (or plain .R scripts, not used in this course)
  + **Environment** shows a list of all variables you have created. For dataframes, it shows the number of observations (rows) and variables (columns). Clicking on the name opens the data, clicking on the arrow shows some details
  + **History** is a searchable list of all commands you have run - helpful if you want to resuse something done a while back.

* **R packages are the apps**
  + Installed once, using `install.packages("name")`
  + Loaded in every session, using `library(name)`

For a more comprehensive introduction to R, you can watch this video:

<iframe src=" https://www.youtube.com/embed/kvaMhkQF0H4?rel=0&modestbranding=1&loop=1&playlist=kvaMhkQF0H4 " allowfullscreen width=80% height=350></iframe>


### Data and variables in R

Lines starting with a # are comments that are ignored by R. I use them below to explain parts of the code further.


```r
# <- saves the text "value" into the variable variableName
month <- "January" 

# c() combines several values into one vector that can then be saved as a variable
weekend <- c("Saturday", "Sunday") 
sleepHrs <- c(4, 9)

#Variables can then be combined into dataframes (sth like tables)
sleepData <- data.frame(day = weekend, hours = sleepHrs)

#Variables within dataframes are accessed using the $ operator
sum(sleepData$hours)
```

```
## [1] 13
```

**Variable classes** indicate what kind of data a variable contains. There are four main types 

* numeric, integer or double (i.e. numbers, the differences are not relevant to us)
* character, i.e. any text
* logical, i.e. TRUE/FALSE values
* and  factors, i.e. categorical variables, for example weekdays. 

The `class()` function shows the class of a single variable, the `str()` and `glimpse()` functions include the classes of all variables in a dataframe.

Usually, R gets the classes right by itself and converts as required. If you need to change variable classes, you can use the `as.numeric()`, `as.character()`, `as.logical()`, `as.factor()` etc. functions. One thing to note: the `data.frame()` function and many functions that import data automatically change text to factors, which is often what we want (for instance, it correctly did this for weekdays above). To prevent this conversion, add `stringsAsFactors = FALSE` as an argument to the function, or convert the columns later using the `as.character()` function.


```r
class(sleepData$hours)
str(sleepData)
```

```
## [1] "numeric"
## 'data.frame':	2 obs. of  2 variables:
##  $ day  : chr  "Saturday" "Sunday"
##  $ hours: num  4 9
```

### Functions in R

All work in R is based on calling functions that do something. A function is called by its name with () behind. If you just type the name, the function is printed out - you usually don't want this.


```r
#Some functions work without any additional arguments
timestamp()
#This shows the time this code segment was run - might be helpful if you create multiple reports

#Functions that work with given data are more helpful - that data is given as an 'argument'
print("Hello")
mean(c(1,2,3))

#Additionally, arguments can contain instructions to the function
mean(c(1,2,3, NA), na.rm = TRUE)
#NA is a missing value, na.rm tells the mean function to remove it before calculating the mean

#To save the results of a function into a variable, use the <- operator again
variableName <- mean(c(1,2,3, NA), na.rm = TRUE)
```

```
## ##------ Tue Sep 29 21:02:26 2020 ------##
## [1] "Hello"
## [1] 2
## [1] 2
```

To learn more about the arguments accepted by a specific function, use the `?` command to open the help page, e.g., by typing `?mean`. Arguments are matched by their name, as with `na.rm =` above; if no name is given, they are matched by their position in the function call. Usually, only very common arguments, typically the data, should be left unnamed.

### Key R packages

The most essential packages for this course are included in the `tidyverse` - a whole set of packages to make data manipulation and visualisation in R efficient and consistent. They include:

* `dplyr`: a package to efficiently manipulate data and create summary statistics
* `ggplot2`: a package to create all kinds of graphs
* `readr`: a package to load table data into R that is formatted as text

All these packages are loaded with the `library(tidyverse)` command. There are some additional packages in the tidyverse that need to be loaded separately, using `library(packagename)`. The most relevant for us are:

* `readxl` and `haven`: package to load data from Excel and statistical software
* `stringr`: a package to manipulate strings 
* `lubridate`: a package to convert, filter and analyse times and dates

#### Using the `pacman` package

As an alternative to `install.packages()` and `library()`, you can also use the `p_load` function in the `pacman` package. That function checks first whether the package is installed, installs it if needed, and then loads it. I am using it in the code here so that you easily copy-paste parts of it; in your own code, the choice is up to you. If you want to use `pacman`, just run `install.packages("pacman")` and then use `pacman::p_load(package1, package2)` to load your packages. 

#### Using a single function in a package

If you just need to use one function from a package, you do not need to load the whole package. You can also give R the whole address and use `package::function()`. I will only do that for `pacman::p_load()` since that is the only function from that package we will ever use in this course.

### RMarkdown

A key strength of R are **reproducible analyses.** For that, we don't want to type commands and just write down the results, but rather create scripts that can produce results again and again. That also makes changes much less painful, for instance if you've made a mistake at the start.

**RMarkdown (.Rmd)** files are such scripts that additionally allow to add text  and split the code into separate chunks. Within them, *grey chunks* are code, anything else is not executed. It is just formatted (e.g., with headings and highlights) when you create the report. Markup symbols allow for easy formatting of the text, as per the instructions [here](https://rmarkdown.rstudio.com/authoring_basics.html). (Most of that is not required in this course, a couple of key symbols such as # to indicate headings will be introduced later.)

Within *.Rmd* files, you need to **Insert** new code blocks for the major parts of the analysis. For that, use the insert button at the top of the File Editor window. When you want to test your code, you can also **Run** it there or by pressing *Ctrl+Enter* in the line you want to use (or after you have selected multiple lines to run). Finally, to create the report you need to **Knit** it, again using the button in the File Editor Without installing anything else, you can only knit to *HTML files*, but those can be opened in any internet browser, so are good to share with anyone.

### HELP! What to do when problems appear?

When **red text** appears in the console, don't panick. Some messages that are often entirely irrelevant are displayed in red, for example when packages are loaded. **Warnings** are more important, but they just alert you that something might be going wrong - read them, but then decide whether you need to worry. For instance, ggplot prints a warning when data is missing, which might or might not be a problem for you. Only **Errors** always need to be dealt with as they stop your commands from being executed and the .Rmd file from being knitted. Unfortunately, many R functions give rather cryptic error messages - if you are stuck, first check your code for typos, then Google the error message.

Many errors occur due to **common mistakes**, that you need to pay attention to:

* **Missing brackets** - make sure each `(` is matched by a `)` in the right place. When your cursor is at a closing bracket, R highlights the opening bracket it is matched with, which helps to check your syntax
* **Misspellings** - variable names and functions need to be spelled correctly, which includes capitalisation, otherwise you might inadvertently create duplicates or cause other issues. Some functions are available in both American and British spelling (e.g., `summarise()`and `summarize()` in dplyr), but this is not always the case so that it might be sensible to always stick to American spelling
* **Missing library() calls** to load packages - if a function is not found, run `library(packagename)`
* **Function masking** - many packages have functions of the same name, so that packages that are loaded later mask (i.e. "over-write")  the names of functions in packages loaded earlier (for example, dplyr's `filter()` function masks the `filter()` function in base R). Often, that is exactly what we want, but if a function does not do what you expect it to do, it's worth specifying the package it comes from to make sure you are not actually calling a different function that has masked it. You can do that by adding the package name and `::`, for example, by calling `dplyr::filter()` *Note*: If you only want to use a single function from a large package, this is an alternative to loading the full package.

As you progress in R, **do not try to remember everything** - instead, look things up as needed, in particular things like argument names:

* Use `?functionName` to get help in R
* Refer to the [RStudio Cheatsheets](https://rstudio.com/resources/cheatsheets/){target="_blank"} 
* Google your questions and pay particular attention to results from r-bloggers.com and stackoverflow.com

There is also a very active R user community that is willing to help - [stackoverflow.com](https://stackoverflow.com/){target="_blank"} is a good place to ask questions about R Code, while [stats.stackexchange.com](https://stats.stackexchange.com){target="_blank"} is the place to go with questions that are more generally about statistics. In both cases, it pays off to first google and then post questions that are as specific as possible.
