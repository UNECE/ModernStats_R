---
title: Introduction to Programming
teaching: 15
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Learn basic concepts of programming

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What is programming?
- What is object oriented programming?
- How to document code?
- What is a directory?

::::::::::::::::::::::::::::::::::::::::::::::::::

## What is programming?

Programmers use programming languages to give instructions to their computers. In this course, we will learn how to use the open source language R to complete common tasks required in the field of official statistics. This includes the basics of R, data manipulation, and best practices.

There are a few reasons why programming with R is useful for official statistics. Data manipulation and analysis with R is:

- Time-saving: R can complete many computations on a large amount of data that would take a person a long time manually

- Reproducible: This code can be re-run with other data with small modifications and shared with others to be applied to other new purposes

- Transparent: When you've completed a script using best practices, you should be left with a clear list of instructions to complete the data analysis in the form of code. This avoids "black boxes" where an analyst is unsure what they've done to the data to get it to it's final form

## R is an object oriented programming language

Object oriented programming languages use *objects* as their main tools. These *objects* have classes, which describe their general properties. For example, in R you might work with *numeric* objects, which would contain numbers. You could also work with *characters*, which would be composed of text. We'll explore classes and data types thoroughly in Episode 3 (Data Types and Structures). We can assign "labels" to these objects, creating a *variable* and use them interchangeably. We assign objects with an assignment operator. In R, the most commonly used assignment operator is `<-`. Try reproducing the example below on your machine by entering the code into the console and hitting the "run" button.

```r
# Assign a number to a variable
number_flowers <- 8

# Print the variable's contents
print(number_flowers)
```

We can get the value stored within the variable by printing it.

```source
[1] 8
```

Assigning a new value to a variable breaks the connection with the old value; R forgets that number and applies the variable name to the new value.

When you assign a value to a variable, R only stores the value, not the calculation you used to create it. This is an important point if you're used to the way a spreadsheet program automatically updates linked cells. Let's look at an example.

```
# Reassign the variable
number_flowers <- 7
```

{: .language-r)

```output
[1] 7
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Variable Naming Conventions

Historically, R programmers have used a variety of conventions for naming variables. The `.` > character in R can be a valid part of a variable name; thus the above assignment could have
easily been `weight.kg <- 57.5`. This is often confusing to R newcomers who have programmed
in languages where `.` has a more significant meaning.
Today, most R programmers 1) start variable names with lower case letters, 2) separate words > in variable names with underscores, and 3) use only lowercase letters, underscores, and
numbers in variable names. The *Tidyverse Style Guide* includes
a [section](https://style.tidyverse.org/syntax.html) on this and other style considerations.


::::::::::::::::::::::::::::::::::::::::::::::::::

## Documenting Code

Notice that in the above examples, hashtags (`#`) are used before giving instructions that are intended for you rather than R. Hashtags produce *comments*, which are handy for leaving information about the code that will follow. Commenting as much code as possible is part of best practices. Always comment your code! You owe it to your colleagues who may see your code (not to mention your future coding self).

```-r
# Hashtags go before commented code, which is not run

# print("This code will not be run")
print("Always comment your code!")
```

```output
[1] "Always comment your code!"
```

## Directories

A directory is a location on your machine. Say you'd like to open a file that's located in a folder on your computer. We need to tell R where to look for the file if we expect to find it. Directories are usually listed by referencing nested folders separated by slashes. There are small differences due to operating system (OS), so refer to documentation specific to your OS when learning to work with folder structures.

For example: `/Users/Documents/Learning-R` points to a folder called "Learning-R" in a user's documents folder. Depending on your IDE (Integrated Development Environment) and setup, you can print your current directory, known as the *working directory*. R automatically reads and writes files from and to your current working directory.

```r
# print current working directory 
getwd()
```

```output
[1] "/Users/Documents/
```

Before beginning our lessons, please set your working directory to the folder that we created in the setup section with `setwd()`. For example, if your folder is named Learning-R:

```r
# change current working directory 
setwd("~/Documents/Learning-R")
```



:::::::::::::::::::::::::::::::::::::::: keypoints

- Programming makes our work faster, more reproducible, and more transparent.
- R is an object oriented programming language
- Document your code with comments
- A working directory is the active location on your computer where R can read and write files

::::::::::::::::::::::::::::::::::::::::::::::::::


