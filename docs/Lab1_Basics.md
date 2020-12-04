# (PART) Labs {.unnumbered}

# Basics

"8/27/2020 | Last Compiled: 2020-12-04"

## Reading and walkthrough video

Vokey & Allen, Chapters 1

Here is a walkthrough lecture on this lab (sorry the volume is a little low on this one):

<iframe width="560" height="315" src="https://www.youtube.com/embed/FuRD0HAIfBc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Overview

The labs for this course are designed to give students exposure to the free and open-source statistical programming language R. The assumption is that students may have zero prior experience with scripting, coding, or computer programming. Over the semester we will use R both as a tool for data-analysis, and as a tool to conduct demonstrations of statistical concepts. 

Before starting this lab, make sure you have followed the getting started instructions to install R, R-studio, create a Github.com account, download github Desktop, and make sure that you test the github pipeline. Throughout the semester you will be posting your assignments on github.com, and submitting links to your repositories on blackboard.

## Problem 1: Summing 1 to 100

In Chapter 1 Vokey & Allen tell the story of how a young Gauss very quickly summed up the numbers from 1 to 100. 

1. Use R to find the sum of the sequence of numbers from 1 to 100:


```r
sum(1:100)
```

```
## [1] 5050
```

As you can see, it is pretty simple and fast to solve Gauss's problem in R. Indeed, you could easily find other sums by changing the 1 or the 100


```r
sum(5:10)
```

```
## [1] 45
```

```r
sum(100:200)
```

```
## [1] 15150
```

At the same time, there are many details going on behind the scenes in R that we will dive into to understand how the sum function works. We will need to understand how to actually generate sequences of number so that we add them up. This will begin a discussion of variables in R (objects that store information, like sequences of numbers). And, we need to understand how take actions, like "summing" up a sequence of numbers. This involves understanding functions and other "active" operations we can do with R.

## R Basics Background

### Creating sequences of numbers in R

A sequence of numbers from a starting value to an ending value can be generated using the following syntax `x:y`, where x is the starting value, and y is the ending value.


```r
1:5
```

```
## [1] 1 2 3 4 5
```

```r
1:10
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

```r
5:-5
```

```
##  [1]  5  4  3  2  1  0 -1 -2 -3 -4 -5
```

### seq()

The above method generates sequences that increment by a value of 1. It is possible to create sequences that increment by any constant value using the `seq()` function. Look up "help" for any R function by typing `?name_of_function` into the console


```r
?seq
```

R comes pre-packaged with many functions like `seq()`, and you can write your own functions, and download libraries of functions that other people have written to extend the base functionality of R. We will look more closely at functions throughout the semester.

Briefly, a function will usually receive some kind of input, "do something", and then return some kind of output. In R, you use functions by writing the name of the function and parentheses `name()`. If the function takes inputs, then you define the inputs inside the parentheses `name(x=1)`. Functions can have multiple inputs, that are separated by commas. 

Let's take a look at using the `seq()` function to generare sequences of numbers.


```r
#lines beginning with # are comments and not run

#seq(from, to)
seq(from = 1, to = 5)
```

```
## [1] 1 2 3 4 5
```

```r
seq(1, 5)
```

```
## [1] 1 2 3 4 5
```

```r
#seq(from, to, by= )
seq(from = 1, to = 5, by = 2)
```

```
## [1] 1 3 5
```

```r
seq(1, 5, .5)
```

```
## [1] 1.0 1.5 2.0 2.5 3.0 3.5 4.0 4.5 5.0
```

```r
seq(1, 10, 2)
```

```
## [1] 1 3 5 7 9
```

```r
#seq(from, to, length.out= )
seq(from = 1, to = 2, length.out =5)
```

```
## [1] 1.00 1.25 1.50 1.75 2.00
```

```r
seq(5)
```

```
## [1] 1 2 3 4 5
```

## Problem 2: Summing any constant series

Now that we have seen the `sum()` and `seq()` functions, you should be able to use them to find the sum of any constant series.

For example, find the sum of the series 100 to 200, going up by five.


```r
sum( seq(100,200,5) )
```

```
## [1] 3150
```

It is also possible to write the analytic formula in R, and compare results, remember that:

$X_1 + X_2 + \ldots + X_n = (\frac{X_n-X_1}{c}+1)(\frac{X_1+X_n}{2})$

Where $X_1$ is the starting value, $X_n$ is the ending value and $c$ is the constant step value.

Here is an example writing this formula in R. We create variables with the names `X1`, `Xn`, and `step`, and assign (`<-`) them any value we want. Then we compute the formula. This should give the same value as the previous example, because it is the same sequence.


```r
X1 <- 100
Xn <- 200
step <- 5

(((Xn - X1)/step) + 1) * ((X1 + Xn)/2)
```

```
## [1] 3150
```

```r
( ( (Xn-X1)/step ) + 1 ) * ( (X1+Xn)/2 )
```

```
## [1] 3150
```


## Vectors

### The Gaussian trick

Remember Gauss added up the numbers from 1 to 100 by imaging two number lines:


```r
1:100
```

```
##   [1]   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17  18
##  [19]  19  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35  36
##  [37]  37  38  39  40  41  42  43  44  45  46  47  48  49  50  51  52  53  54
##  [55]  55  56  57  58  59  60  61  62  63  64  65  66  67  68  69  70  71  72
##  [73]  73  74  75  76  77  78  79  80  81  82  83  84  85  86  87  88  89  90
##  [91]  91  92  93  94  95  96  97  98  99 100
```

```r
100:1
```

```
##   [1] 100  99  98  97  96  95  94  93  92  91  90  89  88  87  86  85  84  83
##  [19]  82  81  80  79  78  77  76  75  74  73  72  71  70  69  68  67  66  65
##  [37]  64  63  62  61  60  59  58  57  56  55  54  53  52  51  50  49  48  47
##  [55]  46  45  44  43  42  41  40  39  38  37  36  35  34  33  32  31  30  29
##  [73]  28  27  26  25  24  23  22  21  20  19  18  17  16  15  14  13  12  11
##  [91]  10   9   8   7   6   5   4   3   2   1
```

He noted that the sum of the columns always added up to 101 (e.g., 1+100 = 101, 2+99 = 101, etc.). It is possible to demonstrate this in R because we can directly add both number lines:


```r
1:100 + 100:1
```

```
##   [1] 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101
##  [19] 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101
##  [37] 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101
##  [55] 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101
##  [73] 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101 101
##  [91] 101 101 101 101 101 101 101 101 101 101
```
Each time we created a sequence above we were creating something called a `numeric vector` in R. These are objects that can store multiple numbers. Vectors are one kind of variable in R. Vectors can have names, they can be saved, and they can be manipulated. Let's take a quick look at vectors:

`a` is the name of the new vector. `<-` is called the assignment operator. `1:5` creates a vector of length 5, containing the sequence of numbers 1 to 5. In plain language, we "assign" the object on the right (the 1:5), to the "name" in the left.


```r
#creates a variable a
a <- 1:5
```

When you create a variable like the above, it's name will appear in the global environment (top right environment tab). When the variable is created for the first time (by executing the code in the console), it becomes registered or saved in your computer's memory for the current R session. 

If you want to double-check that a variable exists in memory, enter its name into the console, and press enter:


```r
a
```

```
## [1] 1 2 3 4 5
```

You can clear a variable using `rm()`. And, you can clear the entire global environment using `Session > Clear Workspace...` from the RStudio menu.


```r
rm(a)
```

You can also check the `class` of a variable in R using the `class()` function. Let's create two vectors and check their classes. The `a` vector contains an integer class, and `b` vector contains a `numeric` class because there are decimal values.


```r
a <- 1:10
class(a)
```

```
## [1] "integer"
```

```r
b <- seq(1,2,.25)
class(b)
```

```
## [1] "numeric"
```

### c()

It is also possible to create vectors that contain characters, rather than numbers. To illustrate an example of a `character vector`, we introduce one more basic R function `c()`, which is short for "combine".


```r
?c
```

It could be helpful to consider a train car analogy when thinking about vectors. In the analogy, a vector is like a train. Trains are composed of connected train cars, and each car is a container that holds things like people, or resources. Trains can have any number of train cars, just like a vector can have any length. The cars in a train are the slots in a vector. To make a train you have to connect the train cars together. Similarly, in R, to make a vector you need to concatenate or connect the slots together. This is what the `c()` function is all about. It combines slots together to form a vector. It is a very flexible and useful function. Also, to avoid confusion, I will only use the letter `c` to refer to the `c()` function, and never as the name of a variable.

To use `c()`, we insert individual items separated by commas. To insert characters, we wrap each character with quotations


```r
letters <- c("a","b","c")
numbers <- c(1,2,3)
numbers_as_chars <- c("1","2","3")
words <- c("this","is","a","vector","of","strings")
```

In each of the above examples, we "combine" the elements inside each `c()` function to form a vector. This is like connecting train cars together. 

### length()

Just like a train has a specific number of cars, a vector has a specific number of slots. This is called the length of the vector. R has a `length()` function the reports the length of a vector.


```r
length(letters)
```

```
## [1] 3
```

```r
length(words)
```

```
## [1] 6
```

### More on combining

The `c()` function is very flexible because it can be used to combine all sorts of elements, even elements that are vectors, or elements that are existing variables.


```r
some_numbers <- c(1, 2, 3, 1:5)

some_characters <- c(letters, words)
```

Remember that vectors have different classes depending on what kind of elements are inside the vector. This is important, because in general R requires that **all of the elements** in a vector have the **same** class.


```r
class(c(1,2,3))
```

```
## [1] "numeric"
```

```r
class(c("A","B","C"))
```

```
## [1] "character"
```

```r
class(c(TRUE,FALSE,TRUE))
```

```
## [1] "logical"
```

It is possible to combine vectors that start with different classes, but R may give an error, or it will convert one class into another. To go back to the train car analogy, R doesn't like trains that have different kinds of cars...it wants the whole thing to be a passenger cars, or the whole thing to be tank cars. 


```r
# the numbers are converted to characters
c(1,2,3,"a","b","c")
```

```
## [1] "1" "2" "3" "a" "b" "c"
```

### Indexing a vector

Vector indexing is the process of being able inspect and change the individual elements of a vector. This is just like a train, where you might want to go an look at what is in cars 3 to 5, or unload car number 7 and put something else in it.

We us the square bracket `[]` notation to index a vector. Generally, the form is `variable_name[x]`. Where, x is another vector specifying the slots that you want to isolate.

Looking at individual elements or groups of elements using an index.


```r
a <- c(1,6,3,2,8,9)
a[1]
```

```
## [1] 1
```

```r
a[2]
```

```
## [1] 6
```

```r
a[1:3]
```

```
## [1] 1 6 3
```

```r
a[c(1,5)]
```

```
## [1] 1 8
```

It is also possible to assign new values to specific elements of a vector:


```r
# assign 100 to the first slot of a
a[1] <- 100
a
```

```
## [1] 100   6   3   2   8   9
```

```r
# assign a 1 to slots 5 to 6 of a
a[5:6] <- 1
a
```

```
## [1] 100   6   3   2   1   1
```

### Growing a vector

In many upcoming lab exercises you will it useful to use vectors to store information. Sometimes you know in advance how many slots you need for your vector, and other times you might not know, and instead you could decide to build your vector one slot at a time.

Below I begin with an empty (NULL) vector. I use the `c()` command, but I don't combine anything together. This like starting a train with no cars at all.


```r
a <- c()
a
```

```
## NULL
```

We can add a slot to this vector by combining a new element to the existing variable. Here we combine `a` with 1, and then assign the result back into `a`, replacing it's original NULL value.


```r
a <- c(a,1)
a
```

```
## [1] 1
```

If we keep doing this, we will keep adding 1s, to the end of `a`.


```r
a <- c(a,1)
a
```

```
## [1] 1 1
```

```r
a <- c(a,1) # c(1,1,1)
a
```

```
## [1] 1 1 1
```

```r
a <- c(a,1)
a
```

```
## [1] 1 1 1 1
```

```r
a <- c(a,1)
a
```

```
## [1] 1 1 1 1 1
```

Consider this alternative method for growing a vector:


```r
a <- c()
a
```

```
## NULL
```

```r
a[1] <- 1
a
```

```
## [1] 1
```

```r
a[2] <- 1
a
```

```
## [1] 1 1
```

```r
a[3] <- 1
a
```

```
## [1] 1 1 1
```

```r
a[10] <- 1
a
```

```
##  [1]  1  1  1 NA NA NA NA NA NA  1
```


## Problem 3: Writing a sum function in R

We have already used R to solve the problems in Chapter in 1 of Vokey & Allen (2018). We can create sequences of numbers, we can create custom vectors of numbers, and we can use the `sum()` function to find the sum. However, we haven't discussed **how** the `sum()` function actually works. How does R know how to find the sum?

This is an example of writing your own sum function in R. This example involves understanding `for` loops and writing custom functions, both of which are explained in the next sections.


```r
my_sum <- function(x) {
  sum <- 0
  for(i in x) sum <- sum + i
  return(sum)
}

my_sum(1:100)
```

```
## [1] 5050
```


## Algorithms

To understand how functions like `sum()` work in R, we need to understand the more general concept of an algorithm. I'll define algorithm as a recipe, or series of steps/actions that result in a particular outcome. With a scripting language like R, it is possible to define algorithms that are infallible. That is, given an input, they **always** apply all of the steps and arrive at the **same answer** demanded by the algorithm. 

When you sum up a series of numbers in your head, say the numbers 1 to 5, you are probably applying a simple algorithm that could be describe like this:

1) Take the first number and add it to the second (1+2 = 3)
2) Take the sum (3) and add it to the next number (3+3 = 6)
3) Repeat step 2 until there are no more numbers in the series
  - 6+4 = 10
  - 10+5 = 15
4) report the final sum (15)


```r
sum(1:5)
```

```
## [1] 15
```

Consider how this could look in R. This is one example of producing an algorithm in R. Everytime we run the below script, we always end up with a sum of 15, because the answer is demanded by the series of steps that I wrote down.


```r
a <- 1:5
a
```

```
## [1] 1 2 3 4 5
```

```r
the_sum <- a[1]+a[2]
the_sum
```

```
## [1] 3
```

```r
the_sum <- the_sum + a[3]
the_sum
```

```
## [1] 6
```

```r
the_sum <- the_sum + a[4]
the_sum
```

```
## [1] 10
```

```r
the_sum <- the_sum + a[5]
the_sum
```

```
## [1] 15
```
## Loops

The above example shows an algorithm written by hand in R. It would be tiresome to write out a sum for a sequence with many more numbers. Fortunately, there are ways to **automate** repetitive processes in R. A common method for repeating commands in R is to use a `for` loop.

Check R help for on Control Flow `?Control`.

`for(){}`
`for(loop control){do something each iteration}`

The basic syntax for loops is as follows:


```r
for(iterator in vector){
  #do something
}
```

Loop control is defined in between the parentheses. The name of the iterator is placed on the left of `in`(can be assigned any name you want, does not need to be declared in advance). During the execution of the loop, the iterator takes on the values inside the vector which is placed on the right side of `in`. Specifically, the following is happening.

Loop steps: 
  1. iterator <- vector[1]
  2. iterator <- vector[2]
  3. iterator <- vector[3]
  4. etc.
  
The loop will automatically stop once it reaches the last item in the vector. The loop can be stopped before that using the `break` command.


```r
# Make a loop do something 5 times
# i is the iterator
# 1:5 creates a vector with 5 numbers in it, 1, 2, 3, 4, 5
# the loop will run 5 times, because there are five things to assign to i
for(i in 1:5){
  print("hello")
}
```

```
## [1] "hello"
## [1] "hello"
## [1] "hello"
## [1] "hello"
## [1] "hello"
```


```r
# show the value of i each step of the loop
for(i in 1:5){
  print(i)
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
```


```r
# define the vector to loop over in advance
my_sequence <- 1:5
for(i in my_sequence){
  print(i)
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
```


```r
# Reminder that i becomes the next value in the vector
# your vector can have any order 
my_sequence <- c(1,5,2,3,4)
for(i in my_sequence){
  print(i)
}
```

```
## [1] 1
## [1] 5
## [1] 2
## [1] 3
## [1] 4
```


```r
# index vector does not need to be numbers
my_things <- c("A","B","C","D")
for(i in my_things){
  print(i)
}
```

```
## [1] "A"
## [1] "B"
## [1] "C"
## [1] "D"
```

### Breaking a loop

`break` stops a loop. Used with logical statements to define the conditions necessary to cause the break.


```r
for(i in 1:10){
  if(i <= 5){
    print(i)
  } else {
    break
  }
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
```

### While loops

While loops run until a logical condition is met. Here there is no iterator, just a logic statement that needs to be met. 

This one prints i while i is less than 6. As soon as i becomes "not less than 6", then the loop stops. Critically, inside the loop, the value of i increases each iteration. 


```r
i <- 1 # create an variable
while (i < 6) {
  print(i)
  i = i+1 #add one each step of the loop
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
```

### Repeat loops

Similar to while, but let's do things until a condition is met.


```r
i<-0
repeat{
  i<-i+1
  print(i)
  if(i==5){
    break
  }
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
```


### Examples

Braces are not needed on one line


```r
for(i in 1:5) print(i)
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
```

Using the value of the iterator to assign in values systematically to another variable.


```r
# put 1 into the first five positions of x
x <- c() # create empty vector
for(i in 1:5){
  x[i] <- 1  # assign 1 to the ith slot in x
}
x
```

```
## [1] 1 1 1 1 1
```

```r
# put the numbers 1-5 in the first 5 positions of x
x <-c()
for(i in 1:5){
  x[i] <- i
}
x
```

```
## [1] 1 2 3 4 5
```

Using a loop to add up numbers in a vector.


```r
a <- c(10,20,30,40,50) #some numbers

the_sum <- 0 # initialize a variable that will keep track of the sum

for (i in a) {
 the_sum <- the_sum + i  
}

the_sum
```

```
## [1] 150
```

```r
sum(a) #check against the sum function
```

```
## [1] 150
```

The above example shows the use of a `for` loop to compute the sum of the numbers in the vector `a`. This is an example of using a loop in an algorithm. At the end we checked out custom script against the `sum()` function, and we see that we arrived at the same answer.

A final task for this lab is take a look at R functions, and learn how to write our own basic functions like `sum()`. 

## Functions

Functions are re-useable algorithms. For example, rather re-writing all of the code necessary to compute a sum everytime we want to find a sum, we instead store the necessary code inside a named variable called `sum()`, then we "call" the function by writing its name and providing inputs.

It is fairly straightforward to write your own custom functions in R, and learning how to write your own functions is an excellent method to improve your understanding of R fundamentals.

### function syntax

This is the general syntax for writing functions:


```r
function_name <- function(input1,input2){
  #code here
  return(something)
}
```

### Example functions

This function has no input between the `()`. Whenever you run this function, it will simply return whatever is placed inside the `return` statement.


```r
# define the function
print_hello_world <- function(){
  return(print("hello world"))
}

# use the function
print_hello_world()
```

```
## [1] "hello world"
```

This function simply takes an input, and then returns the input without modifying it. 


```r
return_input <- function(input){
  return(input)
}

# the variable input is assigned a 1
# then we return(input), which will result in a 1
# because the function internally assigns 1 to the input
return_input(1)
```

```
## [1] 1
```

```r
a <- "something"
return_input(a)
```

```
## [1] "something"
```

This function takes an input, then creates an internal variable called temp and assigns input+1. Then the contents of temp is returned. Note there, is no checking of the input, so it will return an erro if you input a character (can't add one to a character in R)


```r
add_one <- function(input){
  temp <- input+1
  return(temp)
}
add_one(1)
```

```
## [1] 2
```

```r
add_one("a") #this will cause an error
```

```
## Error in input + 1: non-numeric argument to binary operator
```

This function adds some input checking. We only add one if the input is a numeric type. Otherwise, we use `stop()` to return an error message to the console


```r
add_one <- function(input){
  if(class(input) == "numeric"){
    temp <- input+1
    return(temp)
  } else {
    return(stop("input must be numeric"))
  }
}
add_one(1)
```

```
## [1] 2
```

```r
add_one("a")
```

```
## Error in add_one("a"): input must be numeric
```

A function with three inputs


```r
add_multiply <- function(input, x_plus, x_times){
  temp <- (input+x_plus)*x_times
  return(temp)
}

# input is 1
# x_plus <- 2
# x_times <- 3
# will return (1+2)*3 = 9
add_multiply(1,2,3)
```

```
## [1] 9
```


## Lab 1 Generalization Assignment

Follow the instructions below to complete the assignment for lab 1 and hand it in by the due date on blackboard. For the first lab I have taken the extra step of pretending I was student in this course, and completed the first lab myself. This next video shows how I would complete the lab if I was a student. It is important that you try to solve the problems on your own, but please use this video as a resource to help you if you get stuck.

<iframe width="560" height="315" src="https://www.youtube.com/embed/FLD3K-jLmjs" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Instructions

In general, labs will present a discussion of problems and issues with example code like above, and then students will be tasked with completing generalization assignments, showing that they can work with the concepts and tools independently. 

Your assignment instructions are the following:

1. Make a new R project (initialized as a git repository) called "StatsLab1'.
2. Create a new R Markdown document called "Lab1.Rmd"
3. Upload your StatsLab1 R project to Github.com using Github Desktop
4. Use Lab1.Rmd to show your work attempting to solve the following generalization problems. Commit your work regularly so that it appears on your Github repository.
5. Submit your github repository link for Lab 1 on blackboard.
6. There are six problems to solve, each worth 1 point.

Refer to the [getting started](https://crumplab.github.io/psyc7709Lab/articles/Getting_Started.html) videos for examples of creating a new R project and uploading it to Github. If you have problems with these steps and they have not been resolved in our first class, then please email me about them, or create an issue on the course github page <https://github.com/CrumpLab/psyc7709Lab/issues>

### Problems

1. Compute the sum of the sequence 100 to 1000, going up by a constant value of 100 (100,200,300,400,500,600,700,800,900,1000).
2. Compute the sum of these numbers (1,3,2,4,3,5,4,3,4,5,6,5,6,7,6,5,6,5,4,3,4,5)
3. Write a custom sequence generator function **using a for loop** that generates a sequence from a starting integer value to an ending integer value in steps of 1. Demonstrate that it can produce the sequence 1 to 10.
4. Write a custom function to implement the following general equation to find the sum of any constant series:

$X_1 + X_2 + \ldots + X_n = (\frac{X_n-X_1}{c}+1)(\frac{X_1+X_n}{2})$

Demonstrate that your function correctly produces the sum for the series below:


```r
seq(10,100,10)
```

```
##  [1]  10  20  30  40  50  60  70  80  90 100
```

5. Write a custom function that generates a constant series between any start and end values, with any constant, and finds the sum. Have your function output both the sequence and the sum. For this problem, feel free to use the existing `seq()` and `sum()` functions in your custom function. Demonstrate the function correctly prints out the above sequence (10 to 100 in steps of 10), and its sum.
6. Use the `sum()` and the `length()` functions to calculate the mean (average) of the numbers `x = c(1,2,3,4,5)`.




















