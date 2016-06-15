---
title       : Introduction To data.table
description : This chapter provides a basic overview of `data.table` by introducing the user to the methodology and syntax used for data analytics
attachments :
  slides_link : https://s3.amazonaws.com/assets.datacamp.com/course/teach/slides_example.pdf

--- type:NormalExercise lang:r xp:100 skills:1 key:eefb68970c
## Getting started with data.table

> "data.table inherits from data.frame. It offers fast subset, fast grouping, fast update, fast ordered joins and list columns in a short and flexible syntax, for faster development."
    <br>- Matt Dowle, data.table package maintainer   </br>                                                                                                          

The `data.table` package is a powerful tool for storing big data in R. You can think of it as a data.frame 2.0: combining aspects of `dplyr` with `data.frame`, resulting in a faster and more efficient way of manipulating, storing and reading data. 

But instead of taking my word for it, let me prove it to you:

**Speed Test**

To show the speed of `data.table` we will use an example that compares `data.table` and `data.frame` when setting values in a loop:

m = matrix(1, nrow = 2e6L, ncol = 100L)
<br> DF = as.data.frame(m) </br>
<br> DT = as.data.table(m) </br>   

*With data frame*

system.time(for (i in 1:1000) DF[i, 1] = i) 
<br> speed = 15.856 seconds </br>

*With data table*

system.time(for (i in 1:1000) DT[i, V1 := i])
<br>speed = 0.279 seconds </br>

Looping through `data.table` is about 57x faster than `data.frame` ! This will get even faster when we introduce the `set` function with `data.table` 

**Efficiency Test**

To show the efficiency of `data.table` here is the same code needed in both `data.frame` and `data.table` to produce identical output.

*With data frame*

data.frame = DF %>% 
       <br> group_by( V2 ) %>% </br>
        <br> filter( V2 == " A " | V2 == " C ") %>% </br>
        <br> summarise( V4 = sum( V4 ) ) </br>

*With data table*

setkey(DT,"V2")
<br> data.table = DT [ c( " A " , " C " ) , .( V4 = sum( V4 ) ) , by = .EACHI ]</br>


In `data.frame` the actions for data manipulation need to be explicitely called, `data.table` on the other hand is built for implicit manipulation within the `data.table` itself.


If these two examples were not enough for you then hopefully durring the duration of this course you begin to see the value of using data.table for big data manipulation and analytics.

You will begin this course by working through the powerpoint presentation found in the `slides` tab to the right of the screen. This presentation will get you accustomed to the `data.table` syntax and show you some examples of simple data manipulation. 

*** =instructions
- Read through powerpoint presentation
- Click Submitt Answer to continue to next excersise 
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

--- type:NormalExercise lang:r xp:100 skills:1 key:1beea71ace
## Introduction the the Iris dataset 

Before you move on to practicing the techniques described in the power point material, it is important to familiarize yourself with the dataset we will be using for a majority of the excersises.

The iris dataset is preloaded in R, and consists of 5 columns and 150 rows. The dataset compares three different Species of flowers: Setosa, Versicolor and Virginica across four different variables: Sepal.Length, Sepal.Width, Petal.Length, Petal.Width.

It is a simple dataset but useful for practice when first starting out in data.tables.

*** =instructions
- The iris dataset has been preloaded into the R consol. Before moving on view the dataset in the consol.
*** =hint
- type iris into the consol

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
# View iris dataset

```

*** =solution
```{r}
iris
```

*** =sct
```{r}
test_object("iris")
```

iris
--- type:NormalExercise lang:r xp:100 skills:1 key:fb61f7b52f
## Getting started with the i arrgument in data.table

Now that you have had a basic introduction to the data.table syntax, lets start applying some of this knowledge.

The i argument allows you to control filtering of the data.table whether it is by value(s) in a column or a specific row(s).










