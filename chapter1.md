---
title       : Introduction To data.table
description : This chapter provides a basic overview of `data.table` by introducing the user to the methodology and syntax used for data analytics
attachments :
  slides_link : https://s3.amazonaws.com/assets.datacamp.com/course/teach/slides_example.pdf

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:4279d28502
## Benefits of data.table

> "data.table inherits from data.frame. It offers fast subset, fast grouping, fast update, fast ordered joins and list columns in a short and flexible syntax, for faster development.
    <br>- Matt Dowle, data.table package maintainer   </br>                                                                                                          

The `data.table` package is a powerful tool for storing big data in R. You can think of it as a data.frame 2.0: combining aspects of `dplyr` with `data.table` and resulting in a faster and more efficient way of manipulating, storing and reading data. 

But instead of talking my word for it, let me prove it to you:

**Speed Test**

To show the speed of `data.table` we will use an example that compares `data.table` and `data.frame` when setting values in a loop:

m = matrix(1, nrow = 2e6L, ncol = 100L)
<br> DF = as.data.frame(m) </br>
<br> DT = as.data.table(m) </br>   

system.time(for (i in 1:1000) DF[i, 1] = i)
<br> speed = 15.856 seconds </br>

system.time(for (i in 1:1000) DT[i, V1 := i])
<br>speed = 0.279 seconds </br>

Looping through `data.table` is about 57x faster than `data.frame` ! This will get even faster when we introduce the `set` function with `data.table` 

**Efficiency Test**

To show the efficiency of `data.table` here is the same code needed in both `data.frame` and `data.table` to produce identical output.

setkey(DT,"V2")
<br> data.table = DT [ c( " A " , " C " ) , .( V4 = sum( V4 ) ) , by = .EACHI ]</br>

data.frame = DF %>% 
       <br> group_by( V2 ) %>% </br>
        <br> filter( V2 == " A " | V2 == " C ") %>% </br>
        <br> summarise( V4 = sum( V4 ) ) </br>

In `data.frame` the actions for data manipulation need to be explicitely called, `data.table` on the other hand is build for implicit manipulation within the `data.table` itself.


If these two examples were not enough for you then hopefully durring the duration of this course you begin to see the value of using data.table for big data manipulation and analytics.

In the next lecture we will begin by looking at `data.table` syntax. 

*** =instructions

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