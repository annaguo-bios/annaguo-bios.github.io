---
layout: post
title: Make R package
date: 2024-03-25 09:56:00-0400
description: detailed implementation see GitHub repo test.package
tags: code
categories: code
giscus_comments: true
related_posts: false
toc:
  sidebar: left
---

> This is more like a summary note of the two tutorials listed in the references section.

- [Step 0: Packages required](#step-0-packages-required)
- [Step 1: Create your package
  directory](#step-1-create-your-package-directory)
- [Step 2: Create a function](#step-2-create-a-function)
- [Step 3: Add package dependencies](#step-3-add-package-dependencies)
- [Step 4: Add documentation](#step-4-add-documentation)
- [Step 5: Add vignettes](#step-5-add-vignettes)
- [Step 5 (optional): Add a github readme
  file](#step-5-optional-add-a-github-readme-file)
- [Step 6: Install the package](#step-6-install-the-package)
- [Step 7: Test the package](#step-7-test-the-package)
- [References](#references)

This is a test for making R packages.

### Step 0: Packages required

``` r
library("devtools")
library("roxygen2")
```

### Step 1: Create your package directory

Travel to the parent folder where you want to create your package via `setwd`. Then
run `create` to create a R project:

``` r
setwd("path-to-parent_directory")
create("test.package")
```

### Step 2: Create a function

``` r
# A function for cat lovers
# Original code from https://hilaryparker.com/2014/04/29/writing-an-r-package-from-scratch/

cat_function <- function(love=TRUE){
  if(love==TRUE){
    print("I love cats!")
    print(combn(c('lover','love','cat'), 2, paste, collapse = " "))
  }
  else {
    print("I am not a cool person.")
    print(combn(c('not','cool'), 2, paste, collapse = " "))
  }
}
```

### Step 3: Add package dependencies

Given that `cat_function()` uses the `combn()` function, we need to add
the `itertools` package as a dependency. This can be achieved by add the
following line to the `DESCRIPTION` file:

``` r
Imports:
    itertools
```

### Step 4: Add documentation

To document the function, add the following commented lines on top of
the `cat_function()`.

``` r
#' A Cat Function
#'
#' This function allows you to express your love of cats.
#' @param love Do you love cats? Defaults to TRUE.
#' @keywords cats
#' @return A message and combination of words list
#' @importFrom itertools combn
#' @export
#' @examples
#' cat_function()
#' 
cat_function <- function(love=TRUE){
  if(love==TRUE){
    print("I love cats!")
    print(combn(c('lover','love','cat'), 2, paste, collapse = " "))
  }
  else {
    print("I am not a cool person.")
    print(combn(c('not','cool'), 2, paste, collapse = " "))
  }
}
```

Once the documentation is added, run `devtools::document()` to generate
the documentation files `cat_function.Rd`.

### Step 5: Add vignettes

``` r
usethis::use_vignette("introduction")
```

### Step 5 (optional): Add a github readme file

Create a README.Rmd file in the package directory such that there will
be a readme frontpage on github. An example yaml for the README.Rmd file
is as follows:

``` r
---
title: "README"
author: "Anna Guo"
date: "`r Sys.Date()`"
output: 
  github_document:
    toc: true
    number_sections: false
---
```

The code below sets global options for the README.Rmd file.

``` r
library(knitr)
opts_chunk$set(warning = FALSE, message = FALSE, eval=F)
```

### Step 6: Install the package

``` r
devtools::install() # install the package
devtools::build() # build the package as an zip file
```

OR make the package folder a git repository and install the package
using the following code:

``` bash
git init # initialize a local git repository
git add . # add all files to the repository
git commit -m "First commit" # commit the changes

# create an empty repository on GitHub and copy the ssh address
# run this line of code to connect local git repo the remote GitHub repo
git remote add origin git@github.com:<github_username>/<github_repo_name>.git # make change here!!!!!
git push origin main # push local main to origin
```

Once the above is done. The package can be download via

``` r
devtools::install_github("<github_username>/<github_repo_name>")
```

### Step 7: Test the package

This is a short tutorial on how to use the `test.package` package.

``` r
library(test.package)
cat_function(love=T)
```

    ## [1] "I love cats!"
    ## [1] "lover love" "lover cat"  "love cat"

### References

- [Writing an R package from
  scratch](https://hilaryparker.com/2014/04/29/writing-an-r-package-from-scratch/)
- [Making your first R
  package](https://tinyheero.github.io/jekyll/update/2015/07/26/making-your-first-R-package.html)
