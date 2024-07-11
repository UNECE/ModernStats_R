---
title: R Fundamentals
teaching: 60
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- read data into R
- perform basic data operations

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How do I use the Rstudio IDE?
- How do I read data into R?
- What is a data frame?
- How do I access subsets of a data frame?
- How do I calculate simple statistics like mean and median?
- What is plotting?
- How do I install and use packages?

::::::::::::::::::::::::::::::::::::::::::::::::::

## The Rstudio IDE

Rstudio (which will soon be known as Posit) is an IDE (Integrated Development Environment). Just as certain word processing software provide a handy squiggly line under a misspelled word, your IDE provides tools for helping you write good code. Rstudio has 4 panels by default. Utilize them and you're on your way to becoming the programming data scientist you always dreamed you'd be. [Check out this link from r-bloggers.com for an in depth tour.](https://www.r-bloggers.com/2018/08/a-tour-of-rstudio/)

## The Four Corners of Rstudio

Top Left: Your script editor. From the top left you can select the type of file within RStudio you wish to run. In the case of this tutorial, you can use the plus sign to select for a R script. This section contains files that you can edit and save for later.

Top Right: This is your environment. It tells you all of the objects and datafiles that are active within your working directory. It also tells you useful information such as the the type of file, for example numerical or character based.

Bottom Left: This is your console. Think of this as the interactive pane, where you can write practice lines without saving them.

Bottom Right: This is the management section. From here you can browse the files within your computer and manually select a working directory. Here is where the help finder will pop up when we use it later in this tutorial. Any plots that are produced will be created here.

![](https://i.redd.it/o6tq04zyozh11.png){alt='The 4 Corners of Rstudio. Photo courtsey of u/randy3k on the r/rstats subreddit'}

## Reading Data into R

The files you were asked to download in the setup section are in comma-separated values (CSV) format. Each row holds the observations and each column holds information per that observation.
This is what the first few rows look like.

```r
tmp <- read.csv("data/inflammation-01.csv", header = FALSE, nrows = 5)
write.table(tmp, quote = FALSE, sep = ",", row.names = FALSE, col.names = FALSE)
rm(tmp)
```

We want to:

- Load data into memory,
- Calculate the average value of inflammation per day across all patients, and
- Plot the results.

To do all that, we'll have to learn a little bit about programming.

## Getting started

Since we want to import the file called `inflammation-01.csv` into our R
environment, we need to be able to tell our computer where the file is.
To do this, we will create a "Project" with RStudio that contains the
data we want to work with.
The "Projects" interface in RStudio not only creates a working directory for
you, but also remembers its location (allowing you to quickly navigate to it).
The interface also (optionally) preserves custom settings and open files to
make it easier to resume work after a break.

### Create a new project

- Under the `File` menu in RStudio, click on `New project`, choose
  `New directory`, then `New project`
- Enter a name for this new folder (or "directory") and choose a convenient
  location for it. This will be your **working directory** for the rest of the
  day (e.g., `~/Desktop/r-novice-inflammation`).
- Click on `Create project`
- Create a new file where we will type our scripts. Go to File > New File > R
  script. Click the save icon on your toolbar and save your script as
  "`script.R`".
- Make sure you copy the data for the lesson into this folder, if they're
  not there already.

### Loading Data

Now that we are set up with an Rstudio project, we are sure that the
data and scripts we are using are all in our working directory.
The data files should be located in the directory `data`, inside the working
directory. Now we can load the data into R using `read.csv`:

```r
read.csv(file = "data/inflammation-01.csv", header = FALSE)
```

The expression `read.csv(...)` is a [function call](../learners/reference.md#function-call) that asks R to run the function `read.csv`.

`read.csv` has two [arguments](../learners/reference.md#argument): the name of the file we want to read, and whether the first line of the file contains names for the columns of data.
The filename needs to be a character string (or [string](../learners/reference.md#string) for short), so we put it in quotes. Assigning the second argument, `header`, to be `FALSE` indicates that the data file does not have column headers. We'll talk more about the value `FALSE`, and its converse `TRUE`, in lesson 04. In case of our `inflammation-01.csv` example, R auto-generates column names in the sequence `V1` (for "variable 1"), `V2`, and so on, until `V40`.

:::::::::::::::::::::::::::::::::::::::::  callout

## Other Options for Reading CSV Files

`read.csv` actually has many more arguments that you may find useful when
importing your own data in the future. You can learn more about these
options in this supplementary [lesson](11-supp-read-write-csv.Rmd).


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Loading Data with Headers

What happens if you forget to put `header = FALSE`? The default value is
`header = TRUE`, which you can check with `?read.csv` or `help(read.csv)`.
What do you expect will happen if you leave the default value of `header`?
Before you run any code, think about what will happen to the first few rows
of your data frame, and its overall size. Then run the following code and
see if your expectations agree:

```r
read.csv(file = "data/inflammation-01.csv")
```

:::::::::::::::  solution

## Solution

R will construct column headers from values in your first row of data,
resulting in `X0 X0.1 X1 X3 X1.1 X2 ...`.

Note that the character `X` is prepended: a standalone number would not be a valid variable
name. Because column headers are variables, the same naming rules apply.
Appending `.1`, `.2` etc. is necessary to avoid duplicate column headers.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Reading Different Decimal Point Formats

Depending on the country you live in, your standard can use the "dot" or the "comma" as
decimal mark. Also, different devices or software can generate data with different kinds of
decimal marks.
Take a look at `?read.csv` and write the code to load a file called `commadec.txt` that has
numeric values with commas as decimal mark, separated by semicolons.

:::::::::::::::  solution

## Solution

```output
read.csv(file = "data/commadec.txt", sep = ";", dec = ",")
```

or the built-in shortcut:

```output
read.csv2(file = "data/commadec.txt")
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Data Frames

Now that we know how to assign things to variables, let's re-run read.csv and save its result into a variable called 'dat':

```r
dat <- read.csv(file = "data/inflammation-01.csv", header = FALSE)
```

This statement doesn't produce any output because the assignment doesn't display anything. If we want to check if our data has been loaded, we can print the variable's value by typing the name of the variable `dat`. However, for large data sets it is convenient to use the function head to display only the first few rows of data.

```r
head(dat)
```

First, let's ask what type of object `dat` is:

```r
class(dat)
```

The output tells us that it's a data frame. Think of this structure as a spreadsheet in MS Excel that many of us are familiar with. Data frames are very useful for storing data and you will use them frequently when programming in R. A typical data frame of experimental data contains individual observations in rows and variables in columns.

We can see the shape, or [dimensions](../learners/reference.md#dimensions-of-an-array), of the data frame with the function `dim`:

```r
dim(dat)
```

This tells us that our data frame, `dat`, has `r nrow(dat)` rows and `r ncol(dat)` columns.

If we want to get a single value from the data frame, we can provide an [index](../learners/reference.md#index) in square brackets. The first number specifies the row and the second the column:

```r
# first value in dat, row 1, column 1
dat[1, 1]
# middle value in dat, row 30, column 20
dat[30, 20]
```

The first value in a data frame index is the row, the second value is the column.
If we want to select more than one row or column, we can use the function `c`,
which **c**ombines the values you give it into one vector or list.
For example, to pick columns 10 and 20 from rows 1, 3, and 5, we can do this:

```r
dat[c(1, 3, 5), c(10, 20)]
```

We frequently want to select contiguous rows or columns, such as the first ten rows, or columns 3 through 7. You can use `c` for this, but it's more convenient to use the `:` operator. This special function generates sequences of numbers:

```r
1:5
3:12
```

For example, we can select the first ten columns of values for the first four rows like this:

```r
dat[1:4, 1:10]
```

or the first ten columns of rows 5 to 10 like this:

```r
dat[5:10, 1:10]
```

If you want to select all rows or all columns, leave that index value empty.

```r
# All columns from row 5
dat[5, ]
# All rows from column 16-18
dat[, 16:18]
```

If you leave both index values empty (i.e., `dat[,]`), you get the entire data frame.

:::::::::::::::::::::::::::::::::::::::::  callout

## Addressing Columns by Name

Columns can also be addressed by name, with either the `$` operator (ie. `dat$V16`) or square > brackets (ie. `dat[, 'V16']`).
You can learn more about subsetting by column name in this supplementary
[lesson](10-supp-addressing-data.Rmd).


::::::::::::::::::::::::::::::::::::::::::::::::::

Now let's perform some common mathematical operations to learn more about our inflammation data. When analyzing data we often want to look at partial statistics, such as the maximum value per patient or the average value per day. One way to do this is to select the data we want to create a new temporary data frame, and then perform the calculation on this subset:

```r
# first row, all of the columns
patient_1 <- dat[1, ]
# max inflammation for patient 1
max(patient_1)
```

We don't actually need to store the row in a variable of its own.
Instead, we can combine the selection and the function call:

```r
# max inflammation for patient 2
max(dat[2, ])
```

R also has functions for other common calculations, e.g. finding the minimum, mean, median, and standard deviation of the data:

```r
# minimum inflammation on day 7
min(dat[, 7])
# mean inflammation on day 7
mean(dat[, 7])
# median inflammation on day 7
median(dat[, 7])
# standard deviation of inflammation on day 7
sd(dat[, 7])
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Forcing Conversion

The code above may give you an error in some R installations,
since R does not automatically convert a row from a `data.frame` to a vector.
(Confusingly, subsetted columns are automatically converted.)
If this happens, you can use the `as.numeric` command to convert the row of data to a numeric vector:

`patient_1 <- as.numeric(dat[1, ])`

`max(patient_1)`

You can also check the `class` of each object:

`class(dat[1, ])`

`class(as.numeric(dat[1, ]))`


::::::::::::::::::::::::::::::::::::::::::::::::::

R also has a function that summaries the previous common calculations:

```r
# Summarize function
summary(dat[, 1:4])
```

For every column in the data frame, the function "summary" calculates: the minimun value, the first quartile, the median, the mean, the third quartile and the max value, giving helpful details about the sample distribution.

### Plotting

The mathematician Richard Hamming once said, "The purpose of computing is insight, not numbers," and the best way to develop insight is often to visualize data.
Visualization deserves an entire lecture (or course) of its own, but we can explore a few of R's plotting features.

Let's take a look at the average inflammation over time.
Recall that we already calculated these values above using `apply(dat, 2, mean)` and saved them in the variable `avg_day_inflammation`.
Plotting the values is done with the function `plot`.

```r
plot(avg_day_inflammation)
```

Above, we gave the function `plot` a vector of numbers corresponding to the average inflammation per day across all patients.
`plot` created a scatter plot where the y-axis is the average inflammation level and the x-axis is the order, or index, of the values in the vector, which in this case correspond to the 40 days of treatment.
The result is roughly a linear rise and fall, which is suspicious: based on other studies, we expect a sharper rise and slower fall.
Let's have a look at two other statistics: the maximum and minimum inflammation per day.

```r
max_day_inflammation <- apply(dat, 2, max)
plot(max_day_inflammation)
```

```r
min_day_inflammation <- apply(dat, 2, min)
plot(min_day_inflammation)
```

The maximum value rises and falls perfectly smoothly, while the minimum seems to be a step function. Neither result seems particularly likely, so either there's a mistake in our calculations or something is wrong with our data.

:::::::::::::::::::::::::::::::::::::::  challenge

## Plotting Data

Create a plot showing the standard deviation of the inflammation data for each day across all patients.

:::::::::::::::  solution

## Solution

This is the body of the solution.

```r
sd_day_inflammation <- apply(dat, 2, sd)
plot(sd_day_inflammation)
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Installing Packages

Although R has many built in tools for calculations, most programmers use *packages* (also known as libraries) to add to their toolbox. Packages are bundles of functions that can be installed and loaded from [CRAN](https://cran.r-project.org/) or other library repositories.

How you download packages might be specific to your statistical organization, so consult with your instructor. Let's try installing `tidyverse`, a widely used package for data management.

```r
install.packages("dplyr")
```

Then, we'll need to load the package with `library()`

```r
library(dplyr)
```



:::::::::::::::::::::::::::::::::::::::: keypoints

- The Rstudio IDE gives you tools for programming
- Read in data with \`read.csv()
- Data frames are the most common data type used in R
- Index with square bracket notation to access specific parts of a dataframe
- R has built in functions for many common calculations and operations
- Use plots to visualize data
- Install packages from CRAN with \`install.packages()

::::::::::::::::::::::::::::::::::::::::::::::::::


