---
title       : Introduction to data.table
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
library(data.table)
library(dplyr)

DF <- data.frame(V1 = c("a","b","b","c","a") , V2 = c("A","B","C","A","C") , V3 = 1:5 , V4 = 6:10)
DT <- data.table(V1 = c("a","b","b","c","a") , V2 = c("A","B","C","A","C") , V3 = 1:5 , V4 = 6:10)
```

*** =sample_code
```{r}
### Speed Test

m = matrix(1, nrow = 2e6L, ncol = 100L)
DF = as.data.frame(m) 
DT = as.data.table(m)

# With data frame

system.time(for (i in 1:1000) DF[i, 1] = i) 
## speed = 15.856 seconds 

# With data table

system.time(for (i in 1:1000) DT[i, V1 := i])
## speed = 0.279 seconds 

### Efficiency Test

# With data frame

data.frame = DF %>% 
       group_by( V2 ) %>% 
       filter( V2 =="A" | V2 =="C") %>% 
       summarise( V4 = sum( V4 ) ) 

With data table

setkey(DT,"V2")
data.table = DT [ c( "A" , "C" ) , .( V4 = sum( V4 ) ) , by = .EACHI ]


```

*** =solution
```{r}

```

*** =sct
```{r}
```

--- type:NormalExercise lang:r xp:100 skills:1 key:eefb68970c
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
iris <- iris
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


--- type:NormalExercise lang:r xp:100 skills:1 key:eefb68970c
## The i arrgument in data.table

Now that you have had a basic introduction to the data.table syntax and the iris dataset, lets start applying some of this knowledge.

The i argument allows you to control filtering of the data.table whether it is by value(s) in a column or a specific row(s).

Let's begin with some basic excersices below

*** =instructions
- Using the iris dataset filter the Species column for the flower type setosa, assign it to variable filter_one
- Filter the first five rows in the iris dataset, assign it to variable filter_two
- Filter the first, thrid and fifth row in the iris dataset, assign it to variable filter_three
- Filter all the setosa species that have a petal length greater than 1.5, assign it to variable filter_four

*** =hint
- When filtering a column for a value make sure you are using "==" eg. [Species == "versicolor"] 
- You can filter rows the same as you would in data.frame 
- Use the "c" function to concatenate the rows you want to filter 
- You can filter with multiple conditions using "&" for and conditions and "|" for or condition 

*** =pre_exercise_code
```{r}
library(data.table)
iris = as.data.table(iris)
```

*** =sample_code
```{r}
# Filter the Species column for setosa

# Filter the first five rows in the dataset

# Filter the first, third and fifth row in the dataset

# Filter the setosa species that have a petal length greater than 1.5

```

*** =solution
```{r}

# Filter the Species column for setosa

filter_one <- iris[Species == "setosa"]

# Filter the first five rows in the dataset

filter_two <- iris[1:5]

# Filter the first, third and fifth row in the dataset

filter_three <- iris[c(1,2,5)]

# Filter the setosa species that have a petal length greater than 1.5

filter_four <- iris[Species == "setosa" & Petal.Length >1.5]


```

*** =sct
```{r}
test_object("filter_one")
test_object("filter_two")
test_object("filter_three")
test_object("filter_four")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:eefb68970c
## The j arrgument in data.table

Now that you have had some time to practice filtering data.tables, let's look at using the j argument.

The j argument allows you add new columns, update existing columns, summarize data and call specific columns.

Let's begin with some basic excersices below

*** =instructions
- Create a new variable called j_one with data only from the column Sepal.Length
- Add a new column to the iris data.table named Petal.Width_2 which is Petal.Width multiplied by 2 and assign it to a new variable iris2
- Find the average Sepal width in the data.table and assign the value to Avg_Sepal_Width

*** =hint
- Use .() form to call individual columns out of a data.table
- To add or manipulate columns use :=
- You can use functions by calling them within the .() form

*** =pre_exercise_code
```{r}
library(data.table)
iris = as.data.table(iris)
```

*** =sample_code
```{r}
# Create a new variable j_one with data from Sepal.Length

# Add a new column to iris named Petal.Width_2 which is Petal.Width multiplied by 2

# Find the average Sepal width and assign the value to Avg_Sepal_Width

```

*** =solution
```{r}

# Create a new variable j_one with data from Sepal.Length

j_one <- iris[,.(Sepal.Length)]

# Add a new column to iris named Petal.Width_2 which is Petal.Width multiplied by 2 and assign it to a new variable iris2

iris2 <- iris[,Petal.Width_2 := Petal.Width *2]

# Find the average Sepal width and assign the value to Avg_Sepal_Width

Avg_Sepal_Width <- iris[,.(mean(Sepal.Width))]

```

*** =sct
```{r}
test_object("j_one")
test_object("iris2")
test_object("Avg_Sepal_Width")

```

--- type:NormalExercise lang:r xp:100 skills:1 key:eefb68970c
## The by arrgument in data.table

The by arrgument in data.table is incredibly useful because it allows you to summarize data by various variables.

Let's take a look at how we use by in data.table

*** =instructions
- Find the average Petal Length by Species and assign it to a variable named avg_petal_length
- Find the average Petal Length by Species and Petal Width and assign it to a variable named avg_petal_length2

*** =hint
- When calling muliple columns into the by argument be sure it is in the form by = .(columns)

*** =pre_exercise_code
```{r}
library(data.table)
iris = as.data.table(iris)
```

*** =sample_code
```{r}
# Find the average Petal Length by Species and assign it to a variable named avg_petal_length
# Find the average Petal Length by Species and Petal Width and assign it to a variable named avg_petal_length2

```

*** =solution
```{r}

# Find the average Petal Length by Species and assign it to a variable named avg_petal_length

avg_petal_length <- iris[,.(mean(Petal.Length)), by = Species]

# Find the average Petal Length by Species and Petal Width and assign it to a variable named avg_petal_length2

avg_petal_length2 <- iris[,.(mean(Petal.Length)), by = .(Species,Petal.Width)]

```

*** =sct
```{r}
test_object("avg_petal_length")
test_object("avg_petal_length2")


```


--- type:NormalExercise lang:r xp:100 skills:1 key:eefb68970c
## Creating data.tables

Now that you have had some time to practice filtering data.tables, let's look at using the j argument.

The j argument allows you add new columns, update existing columns, summarize data and call specific columns.

Let's begin with some basic excersices below

*** =instructions
- Create a new variable called j_one with data only from the column Sepal.Length
- Add a new column to the iris data.table named Petal.Width_2 which is Petal.Width multiplied by 2 and assign it to a new variable iris2
- Find the average Sepal width in the data.table and assign the value to Avg_Sepal_Width

*** =hint
- Use .() form to call individual columns out of a data.table
- To add or manipulate columns use :=
- You can use functions by calling them within the .() form

*** =pre_exercise_code
```{r}
library(data.table)
iris = as.data.table(iris)
```

*** =sample_code
```{r}
# Create a new variable j_one with data from Sepal.Length

# Add a new column to iris named Petal.Width_2 which is Petal.Width multiplied by 2

# Find the average Sepal width and assign the value to Avg_Sepal_Width

```

*** =solution
```{r}

# Create a new variable j_one with data from Sepal.Length

j_one <- iris[,.(Sepal.Length)]

# Add a new column to iris named Petal.Width_2 which is Petal.Width multiplied by 2 and assign it to a new variable iris2

iris2 <- iris[,Petal.Width_2 := Petal.Width *2]

# Find the average Sepal width and assign the value to Avg_Sepal_Width

Avg_Sepal_Width <- iris[,.(mean(Sepal.Width))]

```

*** =sct
```{r}
test_object("j_one")
test_object("iris2")
test_object("Avg_Sepal_Width")

```




