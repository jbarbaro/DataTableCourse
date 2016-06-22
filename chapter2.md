---
title       : Beyond the Basics with data.table
description : This chapter looks at other useful syntax in data.table beyond the basic i , j , by arguments
attachments :

--- type:NormalExercise lang:r xp:100 skills:1 key:eefb68970c
## Deleting columns

In some aspects of data cleaning you might find that some columns are not as useful or not necessary. In this case it might be best to delete the column in question.

Deleting columns in `data.table` is actually very straight forward once you understand the `i , j , b` syntax.

The process is similar to adding a new column in `data.table` but instead of assigning data to a column we instead set it to `NULL`.

For example:

`data.table[, column := NULL]`

*** =instructions
- Using the iris data set remove the column Sepal.Length, set the new data.table equal to a variable named `iris_one`
- Using the iris data set remove the column Sepal.Length and Petal.Length, set the new data.table equal to a variable named `iris_two`

*** =hint
- Deleting multiple columns at once is the similar to adding multiple columns at once


*** =pre_exercise_code
```{r}
library(data.table)
iris <- data.table(iris)

```

*** =sample_code
```{r}

# Remove the column Sepal.Length and assign it to a variable named iris_one

# Remove the column Sepal.Length and Petal.Legth and assign it to a variable named iris_one

```

*** =solution
```{r}
iris_one <- iris[, Sepal.Length:= NULL]
iris_two <- iris[, `:=` (Sepal.Length=NULL, Petal.Length=NULL)]
```

*** =sct
```{r}
test_object("iris_one")
test_object("iris_two")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:eefb68970c
## Using .N 

The `.N` attribute is a specific call that can be used only within `data.table` that corresponds to the number of rows within the `data.table`. This allows you to specify the number of rows without the use of the `nrow()` function.

This call is especially useful when working within the `i` argument of `data.table`.

For instance if you wanted to isolate the last row of your `data.table` 

`data.table[.N]`

Would do the trick. 

It can also be useful when filtering for something like: the first and last 10 observations.

`data.table[c(1:10, (.N-9):.N)]`


*** =instructions
- Filter the iris data set on the 1st, 10th and last row and assign the output to filter_one
- Filter the iris data set on the first 20 and last 20 rows and assign the output to filter_two

*** =hint


*** =pre_exercise_code
```{r}
library(data.table)
iris <- data.table(iris)
```

*** =sample_code
```{r}
# Filter the iris data set on the 1st, 10th and last row and assign the output to filter_one


# Filter the iris data set on the first 20 and last 20 rows and assign the output to filter_two

```

*** =solution
```{r}

filter_one <- iris[c(1,10,.N)]

filter_two <- iris[c(1:20,(.N-19:20))]

```

*** =sct
```{r}
test_object("filter_one")
test_object("filter_two")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:eefb68970c
## Introduction to Chaining 


*** =instructions
- 
*** =hint


*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}


```

*** =solution
```{r}

```

*** =sct
```{r}
```

--- type:NormalExercise lang:r xp:100 skills:1 key:eefb68970c
## Introduction to .SDcols


*** =instructions
- 
*** =hint


*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}


```

*** =solution
```{r}

```

*** =sct
```{r}
```