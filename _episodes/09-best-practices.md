---
title: Best Practices for Writing R Code
teaching: 10
exercises: 0
source: Rmd
editor_options: 
  markdown: 
    wrap: sentence
---

::: objectives
-   Describe changes made to code for future reference.
-   Understand the importance about requirements and dependencies for your code.
-   Know when to use setwd().
-   To identify and segregate distinct components in your code using \# or #-.
-   Additional best practice recommendations.
:::

::: questions
-   How can I write R code that other people can understand and use?
:::


### Document the code

#### Heading
Starting the R script with a header to inform what it contains, who wrote it and when it was created.
The description should be short, clear and concise.
If it is part of a project, the name of the project should as well be cited on the header.

Use in your code `#` or `#-` to annotate and to set off sections of your code and to make finding specific parts of your code easier.

```{r heading}
################################################################################
# @Who - Sarah Supp, Tracy Teal, and Jon Borelli
# @Project - 2014_paper
# @Description - This file replicates the analyses and figures from our paper
################################################################################
```

#### Versioning
You may also think that your R script is subject to evolve over time.
So rather than creating several vintage copies (V1, V2,etc..) of it, a versioning system would be more appropriate.
"Git" is the most common one and allows you to precisely track-change a script.
The integration of "Git" with Rstudio enables you to view these changes in a dedicated window.
you could consider taking the following trainings: [Software Carpentry's `git-novice` lessons][swc-lesson-git].

#### Consistent style
Use a consistent style within your code.
For example, name all matrices something ending in `_mat`.
Consistency makes code easier to read and problems easier to spot.


### List the requirements and dependencies of the R-code

First write the list of packages needed to execute your R-code.

```{r loadpkgs, eval=FALSE}
################################################################################
# 1a - List of libraries one by one
################################################################################
library(ggplot2)
library(reshape)
library(vegan)

################################################################################
# 1b - Load the list at once and assign a TRUE if the library is already installed and
# a FALSE if not
################################################################################
lapply(list('ggplot2', 'reshape', 'vegan'), require, character.only = TRUE)
```


Second Keep your code in bite-sized chunks.
If a single function or loop gets too long, consider looking for ways to break it into smaller pieces.
if so you will source R-script that is not part of the main script but needed to run your code.


Don't repeat yourself--automate!
If you are repeating the same code over and over, use a loop or a function to repeat that code for you.
Needless repetition doesn't just waste time--it also increases the likelihood you'll make a costly mistake!

If you create one function, put it on the top of your code.
But if you have written many functions, put them all in their own ".R" file and then `source` those files.
`source` will call functions so that your code can make use of them.

```{r source, eval=FALSE}
################################################################################
# 2 - Call functions
################################################################################
source('myfunctions.R')
```

When writing a function, you may not forget to define the function: its purpose, its parameter and the result.

```{r functions, eval=FALSE}
################################################################################
# function 1 - My function is doing ....
################################################################################
#' @param df a dataframe
#' @param n number of observations 
#'
#' @return a dataframe
#' @export
#'  
myfun<-function(df,n) {
  
  return()
}
```

Third you should limit the "hard-coding" of the input and output files in your script.
If your code will read data from a file, define a variable at the beginning of your code that stores the path to that file. And call this variable when needed instead of hard-writing the path of your file in several instances. Like that if you mofify the path, you will have to update your code at one instance. 

```{r params, eval=FALSE}
################################################################################
# 3 - Define parameters
################################################################################
input_file <- "data/data.csv" 
output_file <- "data/results.csv"
```

Fourth it is worth considering what the working directory is.
If the working directory must change, it is best to do that at the beginning of the script.

```{r parameters, eval=FALSE}
################################################################################
# 4 - Define working environment
################################################################################
MAIN_DIR <- "C:/MyPoject/"

# or
setwd("C:/MyPoject/")
```


Or create a project and keep all of your source files for a [project in the same directory][rstudio-project], then use relative paths as necessary to access them. For example, use

```{r relpath, eval=FALSE}
dat <- read.csv(file = "files/dataset-2013-01.csv", header = TRUE)
```

rather than:

```{r abspath, eval=FALSE}
dat <- read.csv(file = "/Users/Karthik/Documents/sannic-project/files/dataset-2013-01.csv", header = TRUE)
```

#### Be careful when using `setwd()`

One should exercise caution when using `setwd()`.
Changing directories in a script file can limit reproducibility:

-   `setwd()` will return an error if the directory to which you're trying to change doesn't exist or if the user doesn't have the correct permissions to access that directory. This becomes a problem when sharing scripts between users who have organized their directories differently.
-   If/when your script terminates with an error, you might leave the user in a different directory than the one they started in, and if they then call the script again, this will cause further problems. If you must use `setwd()`, it is best to put it at the top of the script to avoid these problems. The following error message indicates that R has failed to set the working directory you specified:

```{=html}
<!-- -->
```
    Error in setwd("~/path/to/working/directory") : cannot change working directory

It is best practice to have the user running the script begin in a consistent directory on their machine and then use relative file paths from that directory to access files.


:::

### Other ideas
1. Don't save the workspace BUT start with a clean environment.
[Don't save a session history][dc-rstudio] (the default option in R, when it asks if you want an `RData` file).
Instead, start in a clean environment so that older objects don't remain in your environment any longer than they need to.
If that happens, it can lead to unexpected results.

2. R can run into memory issues.
It is a common problem to run out of memory after running R scripts for a long time.
To inspect the objects in your current R environment, you can list the objects, search current packages, and remove objects that are currently not in use.
A good practice when running long lines of computationally intensive code is to remove temporary objects after they have served their purpose.
However, sometimes, R will not clean up unused memory for a while after you delete objects.
You can force R to tidy up its memory by using `gc()`.

```{r gc_ex, eval=FALSE}
# Sample dataset of 1000 rows
interim_object <- data.frame(rep(1:100, 10),
                             rep(101:200, 10),
                             rep(201:300, 10))
object.size(interim_object) # Reports the memory size allocated to the object
rm("interim_object") # Removes only the object itself and not necessarily the memory allotted to it
gc() # Force R to release memory it is no longer using
ls() # Lists all the objects in your current workspace
rm(list = ls()) # If you want to delete all the objects in the workspace and start with a clean slate
```

3. Wherever possible, [keep track of `sessionInfo()` somewhere][sessionInfo] in your project folder.
Session information is invaluable because it captures all of the packages used in the current project.
If a newer version of a package changes the way a function behaves, you can always go back and reinstall the version that worked (Note: At least on CRAN, all older versions of packages are permanently archived).

4.  Collaborate.
Grab a buddy and practice "code review".
Review is used for preparing experiments and manuscripts; why not use it for code as well?
Our code is also a major scientific achievement and the product of lots of hard work!
Reviews are built into [GitHub's Pull request feature][gh-pr]


::: challenge
## Best Practice

1.  What other suggestions do you have for coding best practices?
2.  What are some specific ways we could restructure the code we worked on today to make it easier for a new user to read? Discuss with your neighbor.
3.  Make two new R scripts called `inflammation.R` and `inflammation_fxns.R`. Copy and paste code into each script so that `inflammation.R` "does stuff" and `inflammation_fxns.R` holds all of your functions. **Hint**: you will need to add `source` to one of the files.

::: solution
## Solution

```{r, engine="bash"}
cat inflammation.R
```

```{r, engine="bash"}
cat inflammation_fxns.R
```
:::
:::

::: keypoints
-   Start each program with a description of what it does.
-   Then load all required packages.
-   Consider what working directory you are in when sourcing a script.
-   Use comments to mark off sections of code.
-   Put function definitions at the top of your file, or in a separate file if there are many.
-   Name and style code consistently.
-   Break code into small, discrete pieces.
-   Factor out common operations rather than repeating them.
-   Keep all of the source files for a project in one directory and use relative paths to access them.
-   Keep track of the memory used by your program.
-   Always start with a clean environment instead of saving the workspace.
-   Keep track of session information in your project folder.
-   Have someone else review your code.
-   Use version control.
:::
