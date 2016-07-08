---
title       : Introduction to data.table
description : This chapter provides a basic overview of `data.table` by introducing the user to the methodology and syntax used for data analytics
attachments :
  slides_link : https://s3.amazonaws.com/assets.datacamp.com/course/teach/slides_example.pdf

--- type:VideoExercise lang:r xp:50 skills:1 key:24df2f6cd0
## Section One: Getting started with data.table

*** =video_link
//player.vimeo.com/video/154783078

--- type:NormalExercise lang:r xp:100 skills:1 key:eefb68970c
## Getting started Exercise

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
        <br> summarize( V4 = sum( V4 ) ) </br>

*With data table*

setkey(DT,"V2")
<br> data.table = DT [ c( " A " , " C " ) , .( V4 = sum( V4 ) ) , by = .EACHI ]</br>


In `data.frame` the actions for data manipulation need to be explicitly called, `data.table` on the other hand is built for implicit manipulation within the `data.table` itself.


If these two examples were not enough for you then hopefully during the duration of this course you begin to see the value of using data.table for big data manipulation and analytics.

You will begin this course by working through the power point presentation found in the `slides` tab to the right of the screen. This presentation will get you accustomed to the `data.table` syntax and show you some examples of simple data manipulation. 

*** =instructions
- Read through power point presentation
- Click Submit Answer to continue to next exercise 
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

m = matrix(1, nrow = 2e4L, ncol = 100L)
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

# With data table

setkey(DT,"V2")
data.table = DT [ c( "A" , "C" ) , .( V4 = sum( V4 ) ) , by = .EACHI ]


```

*** =solution
```{r}

```

*** =sct
```{r}
```

--- type:NormalExercise lang:r xp:100 skills:1 key:3d92e0348c
## Creating data.tables

Creating data.tables is almost identical to creating data.frames in R. 

If you are not familiar with the process, it can be broken down into two steps:

1. Create vectors of data you want to include in your data.table, make sure they are equal length as they will become the columns

2. Call the vectors into the function `data.table()` and assign the output to a variable

See the example below:

1.

A = 1 : 5
<br>B = LETTERS[ 1 : 5 ]</br>

2.

my_DT = data.table( A , B)

Alternatively you might have a data set that is being stored as a data.frame that you want to convert to a data.table. You can do this using the `as.data.table` function.

*** =instructions
- Create a new data.table named `my_DT` which has three columns. The first column named `Alpha` with data A:J, the second column named `Numeric` with data 1:10 and a third column named `small_alpha` with data a:j

*** =hint
- Create the vectors first before calling them into data.table()

*** =pre_exercise_code
```{r}
library(data.table)
iris = as.data.table(iris)
```

*** =sample_code
```{r}
# Create a vector named Alpha

# Create a vector named Numeric

# Create a vector named small_alpha

# Create a data.table named my_DT

```

*** =solution
```{r}

# Create a vector named Alpha

Alpha <-  LETTERS[1:10] 

# Create a vector named Numeric

Numeric <- 1:10

# Create a vector named small_alpha

small_alpha <- letters[1:10]

# Create a data.table named my_DT

my_DT <- data.table(Alpha,Numeric,small_alpha)

```

*** =sct
```{r}
test_object("Alpha")
test_object("Numeric")
test_object("small_alpha")
test_object("my_DT")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:af30f5ffe2
## Introduction the the Iris data set 

Before you move on to practicing the techniques described in the power point material, it is important to familiarize yourself with the data set we will be using for a majority of the exercises.

The iris data set is preloaded in R, and consists of 5 columns and 150 rows. The data set compares three different Species of flowers: Setosa, Versicolor and Virginica across four different variables: Sepal.Length, Sepal.Width, Petal.Length, Petal.Width.

It is a simple data set but useful for practice when first starting out in data.tables.

*** =instructions
- The iris data set has been preloaded into the R console. Before moving on view the data set in the console.
*** =hint
- type iris into the console

*** =pre_exercise_code
```{r}
iris <- iris
```

*** =sample_code
```{r}
# View iris data set

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

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:4279d28502
## Quiz 1: What is data.table

Which of the following statements best represents `data.table` as a data storage tool

*** =instructions
- A data storage tool which works exceptionally well with large data sets
- A data storage tool that allows for implicit data manipulation
- A data store tool that allows for faster and more efficient coding
- All of the above

*** =hint
- `data.table` has many positive qualities


*** =sct
```{r}
# The sct section defines the Submission Correctness Tests (SCTs) used to
# evaluate the student's response. All functions used here are defined in the 
# testwhat R package

msg_bad <- "That is not correct!"
msg_success <- "Exactly! data.table is all this and more!"

# Use test_mc() to grade multiple choice exercises. 
# Pass the correct option (Action, option 2 in the instructions) to correct.
# Pass the feedback messages, both positive and negative, to feedback_msgs in the appropriate order.
test_mc(correct = 4, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_success)) 
```

--- type:VideoExercise lang:r xp:50 skills:1 key:065c0ea84b
## Section Two: The i arrgument in data.table

*** =video_link
//player.vimeo.com/video/154783078

--- type:NormalExercise lang:r xp:100 skills:1 key:00e1a144dd
## The i Arrgument Exercise

Now that you have had a basic introduction to the data.table syntax and the iris data set, lets start applying some of this knowledge.

The i argument allows you to control filtering of the data.table whether it is by value(s) in a column or a specific row(s).

Let's begin with some basic exercise below

*** =instructions
- Using the iris data set filter the Species column for the flower type setosa, assign it to variable filter_one
- Filter the first five rows in the iris data set, assign it to variable filter_two
- Filter the first, third and fifth row in the iris data set, assign it to variable filter_three
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

# Filter the first five rows in the data set

# Filter the first, third and fifth row in the data set

# Filter the setosa species that have a petal length greater than 1.5

```

*** =solution
```{r}

# Filter the Species column for setosa

filter_one <- iris[Species == "setosa"]

# Filter the first five rows in the data set

filter_two <- iris[1:5]

# Filter the first, third and fifth row in the data set

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

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:846ed4ba74
## Quiz 2: Selecting Rows

Have a look at the code below. Chose the option that represents the output from this call.

`iris[ c( 3 , 1 , .N ) ]`
*** =instructions
- Return the third, first and last row from the `iris` data set in that order
- Return the first, third and last row from the `iris` data set in that order
- Return an error as `.N` is not a valid command
- Return an error as there is a missing comma to indicate you are operating the `i` argument

*** =hint
- Order does matter when selecting rows in `data.table`


*** =sct
```{r}
# The sct section defines the Submission Correctness Tests (SCTs) used to
# evaluate the student's response. All functions used here are defined in the 
# testwhat R package

msg_bad <- "That is not correct!"
msg_success <- "Exactly! The third, first and last column would be selected all in that order."

# Use test_mc() to grade multiple choice exercises. 
# Pass the correct option (Action, option 2 in the instructions) to correct.
# Pass the feedback messages, both positive and negative, to feedback_msgs in the appropriate order.
test_mc(correct = 1, feedback_msgs = c(msg_success, msg_bad, msg_bad, msg_bad)) 
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:6d2acd8c44
## Quiz 3: Filtering Rows

Which statement best describes the call shown below

`iris[ Sepal.Width > 2 | Petal.Length < 1 ]`

*** =instructions
- Filter the iris data set for rows where Sepal.Width is greater than 2 and Petal.Length is less than 1
- An error as the `|` character is not valid in the `i argument`
- Filter the iris data set for rows where Sepal.Width is greater than 2 or Petal.Length is less than 1
- None of the above

*** =hint
- Either condition can be true for the row to be displayed 

*** =sct
```{r}
# The sct section defines the Submission Correctness Tests (SCTs) used to
# evaluate the student's response. All functions used here are defined in the 
# testwhat R package

msg_bad <- "That is not correct!"
msg_success <- "Exactly! The call represents an or condtion filter."

# Use test_mc() to grade multiple choice exercises. 
# Pass the correct option (Action, option 2 in the instructions) to correct.
# Pass the feedback messages, both positive and negative, to feedback_msgs in the appropriate order.
test_mc(correct = 3, feedback_msgs = c(msg_bad, msg_bad, msg_success, msg_bad)) 
```

--- type:VideoExercise lang:r xp:50 skills:1 key:5fbb832313
## Section Three: The j arrgument in data.table

*** =video_link
//player.vimeo.com/video/154783078

--- type:NormalExercise lang:r xp:100 skills:1 key:bc99a682a0
## The j Arrgument Exercise

Now that you have had some time to practice filtering data.tables, let's look at using the j argument.

The j argument allows you add new columns, update existing columns, summarize data and call specific columns.

Let's begin with some basic exercise below

*** =instructions
- Create a new variable called j_one with data only from the column Sepal.Length
- Add a new column to the iris data.table named Petal.Width_2 which is Petal.Width multiplied by 2 and assign it to a new variable iris2
- Find the average Sepal width in the data.table and assign the value to `Avg_Sepal_Width`

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

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:99d4fc698c
## Quiz 4: Manipulating existing data

Which of the following code shown below would be used to find the sum of the column `Petal.Width` in the `iris` data set

*** =instructions
- iris[.( sum( Petal.Width ) ) ]
- iris[ , .( sum( Petal.Width ) ) ]
- iris[, sum( Petal.Width ) ]
- Both 2 and 3

*** =hint
- When only preforming one operation in j you don't need .()


*** =sct
```{r}
# The sct section defines the Submission Correctness Tests (SCTs) used to
# evaluate the student's response. All functions used here are defined in the 
# testwhat R package

msg_bad <- "That is not correct!"
msg_success <- "Exactly! When only preforming one operation in j it is not required to use .()"

# Use test_mc() to grade multiple choice exercises. 
# Pass the correct option (Action, option 2 in the instructions) to correct.
# Pass the feedback messages, both positive and negative, to feedback_msgs in the appropriate order.
test_mc(correct = 4, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_success)) 
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:2b00f7f041
## Quiz 5: Adding New Data

Which of the following code shown below would be used to add two new columns to the `iris` data set


*** =instructions
- iris[ ,.( New.Col1 = Petal.Length * 2, New.Col2 = Petal.Length / 2 ) ]
- iris[ ,.( New.Col1 := Petal.Length * 2, New.Col2 := Petal.Length / 2 ) ]
- iris[ , `:=` ( New.Col1 = Petal.Length * 2, New.Col2 = Petal.Length / 2 ) ]
- iris[ , `:=`.( New.Col1 = Petal.Length * 2, New.Col2 = Petal.Length / 2 ) ]

*** =hint
- special assignment notation is needed when creating new columns


*** =sct
```{r}
# The sct section defines the Submission Correctness Tests (SCTs) used to
# evaluate the student's response. All functions used here are defined in the 
# testwhat R package

msg_bad <- "That is not correct!"
msg_success <- "Exactly! Good Work!"

# Use test_mc() to grade multiple choice exercises. 
# Pass the correct option (Action, option 2 in the instructions) to correct.
# Pass the feedback messages, both positive and negative, to feedback_msgs in the appropriate order.
test_mc(correct = 3, feedback_msgs = c(msg_bad, msg_bad, msg_success, msg_bad)) 
```

--- type:VideoExercise lang:r xp:50 skills:1 key:078e8fb308
## Section Four: The by arrgument in data.table

*** =video_link
//player.vimeo.com/video/154783078


--- type:NormalExercise lang:r xp:100 skills:1 key:ae605fcdfa
## The by Arrgument Exercise

The by argument in data.table is incredibly useful because it allows you to summarize data by various variables.

Let's take a look at how we use by in data.table

*** =instructions
- Find the average Petal Length by Species and assign it to a variable named `avg_petal_length`
- Find the average Petal Length by Species and Petal Width and assign it to a variable named `avg_petal_length2`

*** =hint
- When calling multiple columns into the by argument be sure it is in the form by = .(columns)

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

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:8fe3688a44
## Quiz 6: What is by

Which of the following statements best represents the functionality of the `by` argument

*** =instructions
- Allows you to summarize data by a single column in the data set
- Allows you to summarize data by a column or multiple columns in the data set
- Allows you to call additional functions in parallel to the j argument
- Is a way of keeping a column with all it's original data in the output  

*** =hint
- Similar functionality to `group_by` in SQL and data.frame


*** =sct
```{r}
# The sct section defines the Submission Correctness Tests (SCTs) used to
# evaluate the student's response. All functions used here are defined in the 
# testwhat R package

msg_bad <- "That is not correct!"
msg_success <- "Exactly! It allows you to group data by multiple columns!"

# Use test_mc() to grade multiple choice exercises. 
# Pass the correct option (Action, option 2 in the instructions) to correct.
# Pass the feedback messages, both positive and negative, to feedback_msgs in the appropriate order.
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_success, msg_bad, msg_bad)) 
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:03e730697f
## Quiz 7: Using by

Which statement best describes the call shown below

`iris[ Sepal.Width > 2 , sum(Petal.Length), by = .( Species , Sepal.Length ) ]`
*** =instructions
- Sum values of Petal.Length, grouped by Species and Sepal.Length
- Sum values of Petal.Length only for instances where Sepal.Width is greater than 2, grouped by Sepal.Length then Species 
- An error since `.()` is not used in the j argument
- Sum values of Petal.Length only for instances where Sepal.Width is greater than 2, grouped by Species then Sepal.Length

*** =hint
- Similar functionality to `group_by` in SQL and data.frame


*** =sct
```{r}
# The sct section defines the Submission Correctness Tests (SCTs) used to
# evaluate the student's response. All functions used here are defined in the 
# testwhat R package

msg_bad <- "That is not correct!"
msg_success <- "Exactly! Good Work!"

# Use test_mc() to grade multiple choice exercises. 
# Pass the correct option (Action, option 2 in the instructions) to correct.
# Pass the feedback messages, both positive and negative, to feedback_msgs in the appropriate order.
test_mc(correct = 4, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_success)) 
```



