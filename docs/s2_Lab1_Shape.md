

# (PART) Semester 2 Labs {.unnumbered}

# Shaping Data

## Overview

Welcome back. This lab overviews practical aspects about the form or shape that data can take. We will use coding concepts in R that should be mostly familiar from last semester, and use this lab as an opportunity for a little bit of review. There are no readings from the textbook for this lab, but you may find the following links generally helpful:

1.  [dplyr](https://dplyr.tidyverse.org)
2.  [Data-transformation Chapter](https://r4ds.had.co.nz/transform.html)

Apologies that the videos are in two parts...I couldn't compete with a vacuum cleaner.

<iframe width="560" height="315" src="https://www.youtube.com/embed/WRk20lrFhWw" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/l9KG15RS_iY" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Background

The structure of research designs imply that produced data will particular properties, the data can be saved in different formats, data can arrive in different shapes and sizes, and it must be transformed into specific shapes in order to conduct specific analyses. Thus, the shape of data, and the shaping of data, are part and parcel of research, from the beginning to the end. We could spend a good deal of time discussing data organization and manipulation. Most of this conversation will unfold over the semester. In the remaining part of this background section I'm going to identify a few places where data organization is important. Each of these topics could easily be expanded to a whole chapter. For now, my goal is to alert you to them. We will cover some of these topics in more detail in this lab.

### Data-Creation

1.  How should you save your data?
2.  What kind of file formats are there for saving data?

Perhaps because the discipline of Psychology is so large and varied it has been slow to adopt any widespread standards for formatting data. Certainly, there are so many kinds of data that standards for one research project might not apply to another. Here are some considerations to keep in mind:

1.  The decisions you make about how to save your data will have consequences later for analysis. If you save in a format that is analysis-ready, then you won't have to transform the data later
2.  Save the data in a machine-readable and transformable format. Machine-readable means that you can input the data to a program language like R, and transformable means that you can reshape the data while preserving its inherent factor structure (more on this in this lab).
3.  There are numerous file formats. We will typically be reading in .txt (text files), .csv (comma separated value) files, and sometimes .xlsx (excel) or proprietary files.

### Reproducible analysis pipeline

1.  Inputting data to R
2.  Re-shaping it for analysis

Data-shaping is a practical part of all data-analysis in R. Using scripts to handle the data from the input to analysis allows a reproducible pipeline. A reproducible is helpful for you and others. When your analysis pipeline is not reproducible, you may not be able to fix mistakes that you make. For example, if you accidentally delete something, or move something around by hand, you may not have a record of having performed that operation, and if you forget about it, you may never be able to go back and fix errors. When you use a script for analysis you never "touch" the data by hand. Instead, all actions are taken by script. Even if your script makes a mistake, the mistake is at least identifiable and fixable. If someone else has the raw data, and your analysis script, then if they input the data to your script, they should output the same analysis that you reported. The nuts and bolts of the analysis script often include many transformations of the data: The raw data is inputted to R, it might be saved in different variables, pre-processed in various ways, and reformatted and sliced and diced to meet input requirements for R statistical analysis functions.

### Knowing your design and analysis

1.  Design and implied shape
2.  Simulated data and analysis

A major overarching goal for the end of our course is for you to understand how you could create your own statistical analyses customized exactly to nuances of your research designs. In other words, how to WYOR (Write Your Own Recipes) for statistics. In order to get there, it is important to recognize fundamental connections between your research design and the shape of data that will be collected in the design. In this lab, we will use R as a conceptual and practical tool to illustrate how simulated data can be created for particular designs. Once you have simulated data, you can test out your own planned analysis in advance of obtaining real data.

## Concept I: Transformability

This is a short concept section to illustrate the concept of transformability. The basic idea is that transformable data can be arranged into different shapes and back again without losing any information. As a practical matter, many functions depend on their inputs being formatted in a particular way. Thus, data transformation is often required to get the data into shape so that it can be inputted into some function.

### Long vs. wide data

As a general rule, most functions for statistical tests in R require that data are organized in long-format. I personally find this convenient because it means that:

1.  If can get my data into long format, then
2.  I can do just about anything I want to it using R functions for analysis and visualization

Let's look at an example of wide data. Imagine we have five people, and we have measured how many times they check their phone, in the morning, afternoon, and evening.


```r
wide_data <- data.frame(person = 1:5,
                        Morning = c(1,3,2,4,3),
                        Afternoon = c(3,4,5,4,7),
                        Evening = c(7,8,7,6,9))
knitr::kable(wide_data)
```



| person| Morning| Afternoon| Evening|
|------:|-------:|---------:|-------:|
|      1|       1|         3|       7|
|      2|       3|         4|       8|
|      3|       2|         5|       7|
|      4|       4|         4|       6|
|      5|       3|         7|       9|

If we have 5 people, and collect measures three times each, then we must have 5 x 3 total cells. The wide version of this data is a 5x3 matrix with 5 rows (different people), and 3 columns (morning, afternoon, evening). This matrix has 15 cells in it, so it is capable of representing all complete cases of the data. Wide-data is a perfectly fine way to represent data, and there is nothing inherently wrong with wide-data. However, as I mentioned before, many functions in R are written with the assumption that data is shaped as long-data.

Here is an example of the same thing in long-format:


```r
long_data <- data.frame(person = rep(1:5, each=3),
                        time_of_day = rep(c("Morning", "Afternoon", "Evening"),5),
                        counts = c(1,3,7,3,4,8,2,5,7,4,4,6,3,7,9))
knitr::kable(long_data)
```



| person|time_of_day | counts|
|------:|:-----------|------:|
|      1|Morning     |      1|
|      1|Afternoon   |      3|
|      1|Evening     |      7|
|      2|Morning     |      3|
|      2|Afternoon   |      4|
|      2|Evening     |      8|
|      3|Morning     |      2|
|      3|Afternoon   |      5|
|      3|Evening     |      7|
|      4|Morning     |      4|
|      4|Afternoon   |      4|
|      4|Evening     |      6|
|      5|Morning     |      3|
|      5|Afternoon   |      7|
|      5|Evening     |      9|

```r


person <- rep(1:5,3)
time_of_day <- rep(c("Morning", "Afternoon", "Evening"),each =5)
counts <- c(1,3,7,3,4,8,2,5,7,4,4,6,3,7,9)

test <- data.frame(person,time_of_day,counts)
```

As you can see, in long-data format, the data gets really long. The rule here is that each dependent measure (e.g., the counts of phone looking) is listed on a single row. There ar 5 x 3 = 15 total individual measures, so there must be 15 rows in the long-form representation.

### Converting Wide to long

Sometimes you might receive data in wide format and need to convert it to long format. This can be accomplished in R in multiple different ways. You can write custom code to do it, or you can try using various existing functions. If you Google "wide to long in R", you might come across a curious history of functions that have been developed to provide this function. One part of that history is that the functions keep getting re-written so that they are "more clear" about how they work. I'll admit that I have found these functions confusing before, and usually find myself messing around with them until they do the conversion I'm looking for. In any case, here is an example of pivoting from wide to long using `tidyr`


```r
library(tidyr)

pivot_longer(data = wide_data, 
             cols = !person,
             names_to = "time_of_day",
             values_to = "counts")
#> # A tibble: 15 × 3
#>    person time_of_day counts
#>     <int> <chr>        <dbl>
#>  1      1 Morning          1
#>  2      1 Afternoon        3
#>  3      1 Evening          7
#>  4      2 Morning          3
#>  5      2 Afternoon        4
#>  6      2 Evening          8
#>  7      3 Morning          2
#>  8      3 Afternoon        5
#>  9      3 Evening          7
#> 10      4 Morning          4
#> 11      4 Afternoon        4
#> 12      4 Evening          6
#> 13      5 Morning          3
#> 14      5 Afternoon        7
#> 15      5 Evening          9
```

### Custom to long

The grim reality is that data that is not made by you could take a huge number of different formats. And, once you get it into R, you may have wrangle it into shape before you can proceed to analyze it. In this example, I will show a strange data format, and write some custom code to wrangle it into long-format. This is just to illustrate the general idea that sometimes you may have to do custom data shaping.

Consider the following format. Each subject's phone checking count is a number separated by commas. The first number is always for morning, the second is for afternoon, and the third for evening. Individual subjects are separated by semi-colons. Thus, the first three numbers are for subject 1, and the next three are for subject 2, and so on. As you can see, all of the data from before is perfectly preserved, all on one line.


```r
the_data<-"1,3,7;3,4,8;2,5,7;4,4,6;3,7,9"
```

So, we have a custom format above, and now we need to get it into long format. Unfortunately, there are no tidy-verse functions for shaping weird custom data formats. So, someone has pull some tricks out of their hat.


```r
library(dplyr)


subjects <- unlist(strsplit(the_data, split = ";"))
subjects
#> [1] "1,3,7" "3,4,8" "2,5,7" "4,4,6" "3,7,9"
subjects <- strsplit(subjects,split=",")
subjects
#> [[1]]
#> [1] "1" "3" "7"
#> 
#> [[2]]
#> [1] "3" "4" "8"
#> 
#> [[3]]
#> [1] "2" "5" "7"
#> 
#> [[4]]
#> [1] "4" "4" "6"
#> 
#> [[5]]
#> [1] "3" "7" "9"

subjects <- t(data.frame(subjects))
subjects
#>                  [,1] [,2] [,3]
#> c..1....3....7.. "1"  "3"  "7" 
#> c..3....4....8.. "3"  "4"  "8" 
#> c..2....5....7.. "2"  "5"  "7" 
#> c..4....4....6.. "4"  "4"  "6" 
#> c..3....7....9.. "3"  "7"  "9"
colnames(subjects) <- c("Morning","Afternoon","Evening")
subjects
#>                  Morning Afternoon Evening
#> c..1....3....7.. "1"     "3"       "7"    
#> c..3....4....8.. "3"     "4"       "8"    
#> c..2....5....7.. "2"     "5"       "7"    
#> c..4....4....6.. "4"     "4"       "6"    
#> c..3....7....9.. "3"     "7"       "9"
row.names(subjects) <- 1:5
subjects <- as.data.frame(subjects) %>%
  mutate(person=1:5)

pivot_longer(data = subjects, 
             cols = 1:3,
             names_to = "time_of_day",
             values_to = "counts")
#> # A tibble: 15 × 3
#>    person time_of_day counts
#>     <int> <chr>       <chr> 
#>  1      1 Morning     1     
#>  2      1 Afternoon   3     
#>  3      1 Evening     7     
#>  4      2 Morning     3     
#>  5      2 Afternoon   4     
#>  6      2 Evening     8     
#>  7      3 Morning     2     
#>  8      3 Afternoon   5     
#>  9      3 Evening     7     
#> 10      4 Morning     4     
#> 11      4 Afternoon   4     
#> 12      4 Evening     6     
#> 13      5 Morning     3     
#> 14      5 Afternoon   7     
#> 15      5 Evening     9
```

<!--

Functions are shape dependent


```r

find_group_means_A <- function(x){
  colMeans(x)
}

find_group_means_B <- function(x,IV,DV){
  aggregate(DV~IV, data=x, mean)
}

wide_data <- matrix(1:20,ncol=2)
long_data <- data.frame(IV = rep(c("A","B"),each=10),
                        DV = 1:20)

find_group_means_A(wide_data)
#> [1]  5.5 15.5
find_group_means_B(long_data)
#>   IV   DV
#> 1  A  5.5
#> 2  B 15.5
```

-->

## Practical I: Simulated data for different designs

The purpose of this section is to focus on the process of creating data-structures in R that have the following properties:

1.  They appropriately represent the data necessary for a particular design
2.  They are formatted so that R functions for statistical tests can be performed on them

### One-sample t-test 

A one-sample t-test involves a vector of means. Here, a vector of means is created by sampling 10 values from a unit normal distribution.


```r
dv <- rnorm(10,0,1)
t.test(dv)
#> 
#> 	One Sample t-test
#> 
#> data:  dv
#> t = 4.2546, df = 9, p-value = 0.002128
#> alternative hypothesis: true mean is not equal to 0
#> 95 percent confidence interval:
#>  0.43278 1.41551
#> sample estimates:
#> mean of x 
#> 0.9241449
```

Consider a design with 50 participants. Each participant takes a TRUE/FALSE quiz with 10 questions. A researcher wants to apply a one-sample t-test to test whether the participants performed better than chance.

1.  Create example raw data that represents each subjects' answer to each question

    -   There 50 participants x 10 questions, so there must be 500 cells

    -   I sample 1s and 0s from a binomial to indicate correct vs incorrect one each question

2.  Create a summary vector of means suitable for the t.test function

3.  Run the t.test


```r
subject_data <- matrix( rbinom(50*10,1,.5), ncol=10, nrow=50)
subject_means <- rowMeans(subject_data)
t.test(subject_means, mu=.5)
#> 
#> 	One Sample t-test
#> 
#> data:  subject_means
#> t = -0.44361, df = 49, p-value = 0.6593
#> alternative hypothesis: true mean is not equal to 0.5
#> 95 percent confidence interval:
#>  0.4446992 0.5353008
#> sample estimates:
#> mean of x 
#>      0.49
```

### Paired sample t-test

Consider a design measuring fluctuations in weight as a function of weekday vs. weekend. Researchers have 25 people weigh themselves 5 times throughout the day on Wednesday, and 5 times throughout the day on Sunday. Create a data frame that represents this situation, and conduct a paired sample t-test.

To break this down, we will create a long data.frame with four columns: Subject number, Day, measurement number, weight. How many rows must their be? There are 25 people, 5 measurements per day, and two days of measurements. In long-format, there is only one measure per row. Therefore, there are 25 x 5 x 2 = 250 rows.

I repeat each number from 1 to 25, 10 times each.


```r
subject_number <- rep(1:25, each=10)
```

The day column has two levels, Wednesday vs. Sunday. Each level has to appear 5 times for each subject.


```r
#day <- rep(c("Wednesday","Sunday"), each = 5) # makes one subject
day <- rep(rep(c("Wednesday","Sunday"), each = 5), 25)
```

We need a variable to represent each of the five measurements that are taken per day. Let's call this measurement_number


```r
measurement_number <- rep(1:5, 2*25)
```

We need some pretend measurements. For now, let's just choose some random numbers from a normal distribution. We need 250 numbers.


```r
weights <- rnorm(250, 100, 25)
```

Next, let's combine all of these vectors into a data.frame


```r
weight_data <- data.frame(subject_number,
                          day,
                          measurement_number,
                          weights)
head(weight_data)
#>   subject_number       day measurement_number   weights
#> 1              1 Wednesday                  1  79.23932
#> 2              1 Wednesday                  2 114.84009
#> 3              1 Wednesday                  3  78.59676
#> 4              1 Wednesday                  4 150.13053
#> 5              1 Wednesday                  5 105.23875
#> 6              1    Sunday                  1 101.74751
```

Note, we could have defined everything inside a single data.frame


```r
weight_data <- data.frame(subject_number = rep(1:25, each=10),
                          day = rep(rep(c("Wednesday","Sunday"), each = 5), 25),
                          measurement_number = rep(1:5, 2*25),
                          weights = rnorm(250, 100, 25))
```

The data.frame `weight_data` now represents the complete shape of the data implied by the research design. However, this data is not yet ready for the `t.test` function. This is because the `t.test` function assumes that inputs will be means for each participant in each condition. So, the raw data must be summarized first. In other words, we must find the mean weight within each day that each participant was measured. We will continue to use the `dplyr` syntax to group and summarize data.


```r
subject_means <- weight_data %>%
  group_by(subject_number,day) %>%
  summarize(mean_weight = mean(weights), .groups = "drop")

head(subject_means)
#> # A tibble: 6 × 3
#>   subject_number day       mean_weight
#>            <int> <chr>           <dbl>
#> 1              1 Sunday           82.8
#> 2              1 Wednesday        88.1
#> 3              2 Sunday          101. 
#> 4              2 Wednesday       106. 
#> 5              3 Sunday           87.4
#> 6              3 Wednesday        92.2
```

Finally, we can run the `t.test`


```r
t.test(mean_weight~day, paired=TRUE, data=subject_means)
#> 
#> 	Paired t-test
#> 
#> data:  mean_weight by day
#> t = 0.076271, df = 24, p-value = 0.9398
#> alternative hypothesis: true difference in means is not equal to 0
#> 95 percent confidence interval:
#>  -6.621660  7.129843
#> sample estimates:
#> mean of the differences 
#>               0.2540919
```

### Independent sample t-test

A researcher gives 10 subjects a recall memory test. They all read 50 words for a later memory test. After a short break half of the participants are put in a noisy room, and the other half are put in a quiet room. They are all given a piece of paper with 50 lines and asked to write down as memory words as they can remember. The raw data is coded as 1s or 0s, with 1 representing a correctly recalled word and 0 represent an incorrectly recalled word. A researcher wants to do a t-test on the number of correctly recalled words in the noisy vs quiet room.

Note, I switch to using a [tibble](https://blog.rstudio.com/2016/03/24/tibble-1-0-0/) here instead of a data.frame. 


```r

subjects <- rep(1:10, each = 50)
room <- rep(c("Noisy","Quiet"), each = 50*5)
words <- rep(1:50, 10)
correct <- rbinom(500,1,.5)

recall_data <- tibble(subjects,
                      room,
                      words,
                      correct)

recall_data
#> # A tibble: 500 × 4
#>    subjects room  words correct
#>       <int> <chr> <int>   <int>
#>  1        1 Noisy     1       1
#>  2        1 Noisy     2       1
#>  3        1 Noisy     3       1
#>  4        1 Noisy     4       1
#>  5        1 Noisy     5       1
#>  6        1 Noisy     6       1
#>  7        1 Noisy     7       1
#>  8        1 Noisy     8       1
#>  9        1 Noisy     9       1
#> 10        1 Noisy    10       0
#> # … with 490 more rows

count_data <- recall_data %>%
  group_by(subjects,room) %>%
  summarize(number_correct = sum(correct), .groups="drop")

count_data
#> # A tibble: 10 × 3
#>    subjects room  number_correct
#>       <int> <chr>          <int>
#>  1        1 Noisy             25
#>  2        2 Noisy             24
#>  3        3 Noisy             20
#>  4        4 Noisy             25
#>  5        5 Noisy             19
#>  6        6 Quiet             24
#>  7        7 Quiet             26
#>  8        8 Quiet             29
#>  9        9 Quiet             27
#> 10       10 Quiet             31

t.test(number_correct~room, var.equal=TRUE, data=count_data)
#> 
#> 	Two Sample t-test
#> 
#> data:  number_correct by room
#> t = -2.7175, df = 8, p-value = 0.02635
#> alternative hypothesis: true difference in means between group Noisy and group Quiet is not equal to 0
#> 95 percent confidence interval:
#>  -8.8732154 -0.7267846
#> sample estimates:
#> mean in group Noisy mean in group Quiet 
#>                22.6                27.4
```

### Simple linear regression

100 people write down their height in centimeters, and the day of the month they were born. Conduct a linear regression to see if day of month explains variation in height.


```r

people <- tibble(height = rnorm(100, 90, 10),
                 day = sample(1:31, 100, replace=TRUE))

people
#> # A tibble: 100 × 2
#>    height   day
#>     <dbl> <int>
#>  1  112.      5
#>  2   91.9    20
#>  3  101.     31
#>  4   98.6    17
#>  5   77.5     8
#>  6   78.4     8
#>  7   85.7    14
#>  8   90.2     9
#>  9   63.2    30
#> 10   81.6    12
#> # … with 90 more rows

lm.out <- lm(height~day, data= people)
lm.out
#> 
#> Call:
#> lm(formula = height ~ day, data = people)
#> 
#> Coefficients:
#> (Intercept)          day  
#>    87.59461     -0.01262
summary(lm.out)
#> 
#> Call:
#> lm(formula = height ~ day, data = people)
#> 
#> Residuals:
#>      Min       1Q   Median       3Q      Max 
#> -26.7687  -6.4261  -0.1001   6.9333  24.4441 
#> 
#> Coefficients:
#>             Estimate Std. Error t value Pr(>|t|)    
#> (Intercept) 87.59461    2.16125  40.530   <2e-16 ***
#> day         -0.01262    0.11110  -0.114     0.91    
#> ---
#> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> Residual standard error: 9.906 on 98 degrees of freedom
#> Multiple R-squared:  0.0001316,	Adjusted R-squared:  -0.01007 
#> F-statistic: 0.0129 on 1 and 98 DF,  p-value: 0.9098
```

### One-way ANOVA

We haven't yet covered one-way ANOVA in this course. You may already be familiar with ANOVA. For now, we can think of it is a kind of extension of t-tests to deal with single-factor designs with more than one level. For example, consider extending the paired t-test sample above. In that example, 25 people weighed themselves 5 times throughout the day on Wednesday, and 5 times throughout the day on Sunday. The independent variable is day, and it has two levels (Wednesday vs Sunday). Why should we stop at Wednesday vs. Sunday? There are five other days of the week. Maybe each day has its own influence on weight.

Consider simulating data for a one-factor design with 7 levels, one for each day of the week. N=25, and each person measures themselves 5 times each day.


```r
weight_data <- tibble(subject_number = rep(1:25, each=5*7),
                          day = rep(rep(c("S","M","T","W","Th","F","Sa"),
                                      each = 5), 25),
                          measurement_number = rep(1:5, 7*25),
                          weights = rnorm(25*5*7, 100, 25))
```

As we will see in later labs on ANOVA, the analysis can be performed in one-line with the `aov` function


```r
subject_means <- weight_data %>%
  group_by(subject_number,day) %>%
  summarize(mean_weight = mean(weights), .groups="drop")

subject_means
#> # A tibble: 175 × 3
#>    subject_number day   mean_weight
#>             <int> <chr>       <dbl>
#>  1              1 F           102. 
#>  2              1 M           108. 
#>  3              1 S           107. 
#>  4              1 Sa          116. 
#>  5              1 T            97.7
#>  6              1 Th           95.5
#>  7              1 W            88.3
#>  8              2 F            85.0
#>  9              2 M            85.1
#> 10              2 S           108. 
#> # … with 165 more rows

aov.out <- aov(mean_weight ~ day, data = subject_means)
summary(aov.out)
#>              Df Sum Sq Mean Sq F value Pr(>F)
#> day           6    668   111.4   0.759  0.603
#> Residuals   168  24655   146.8
```

And, as we will also learn in class, ANOVA and Linear Regression are fundamentally the same analysis, so we could also use the `lm` function and treat the analysis as a regression.


```r
subject_means <- weight_data %>%
  group_by(subject_number,day) %>%
  summarize(mean_weight = mean(weights), .groups="drop")

subject_means
#> # A tibble: 175 × 3
#>    subject_number day   mean_weight
#>             <int> <chr>       <dbl>
#>  1              1 F           102. 
#>  2              1 M           108. 
#>  3              1 S           107. 
#>  4              1 Sa          116. 
#>  5              1 T            97.7
#>  6              1 Th           95.5
#>  7              1 W            88.3
#>  8              2 F            85.0
#>  9              2 M            85.1
#> 10              2 S           108. 
#> # … with 165 more rows

lm.out <- lm(mean_weight ~ day, data = subject_means)
summary(lm.out)
#> 
#> Call:
#> lm(formula = mean_weight ~ day, data = subject_means)
#> 
#> Residuals:
#>      Min       1Q   Median       3Q      Max 
#> -29.6835  -8.5188   0.1274   8.4696  29.0607 
#> 
#> Coefficients:
#>             Estimate Std. Error t value Pr(>|t|)    
#> (Intercept) 100.3022     2.4229  41.398   <2e-16 ***
#> dayM         -1.4732     3.4264  -0.430    0.668    
#> dayS          0.5147     3.4264   0.150    0.881    
#> daySa         2.0101     3.4264   0.587    0.558    
#> dayT          2.3145     3.4264   0.675    0.500    
#> dayTh        -3.4167     3.4264  -0.997    0.320    
#> dayW          1.9340     3.4264   0.564    0.573    
#> ---
#> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> Residual standard error: 12.11 on 168 degrees of freedom
#> Multiple R-squared:  0.0264,	Adjusted R-squared:  -0.008373 
#> F-statistic: 0.7592 on 6 and 168 DF,  p-value: 0.603
```

### Factorial ANOVA

The one-way ANOVA extends the t-test design in terms of the number of levels of a single factor. It is also possible to run experiments with multiple independent variables, that each have multiple levels.

For example, let's continue with the above design and make some minor changes so it becomes a factorial 7x2 design. A 7x2 design has two independent variables. The first one has 7 levels, the second one has 2 levels.

We modify the example include a time of day factor. For example, we will have people measure their weight two times in the morning, and two times in the evening. Thus, there will be two independent variables. Day (S, M, T, W, Th, F, Sa) and time of day (morning, evening). We will keep the number of subjects at 25.


```r
weight_data <- tibble(subject_number = rep(1:25, each=4*7),
                          day = rep(rep(c("S","M","T","W","Th","F","Sa"),
                                      each = 4), 25),
                          time_of_day = rep(c("Morning","Morning",
                                              "Evening","Evening"),7*25),
                          measurement_number = rep(rep(1:2, 2), 7*25),
                          weights = rnorm(25*4*7, 100, 25))
```


```r
subject_means <- weight_data %>%
  group_by(subject_number,day, time_of_day) %>%
  summarize(mean_weight = mean(weights), .groups="drop")

subject_means
#> # A tibble: 350 × 4
#>    subject_number day   time_of_day mean_weight
#>             <int> <chr> <chr>             <dbl>
#>  1              1 F     Evening            65.5
#>  2              1 F     Morning            90.9
#>  3              1 M     Evening            84.9
#>  4              1 M     Morning            84.5
#>  5              1 S     Evening           101. 
#>  6              1 S     Morning           104. 
#>  7              1 Sa    Evening           112. 
#>  8              1 Sa    Morning           116. 
#>  9              1 T     Evening            99.8
#> 10              1 T     Morning           117. 
#> # … with 340 more rows

aov.out <- aov(mean_weight ~ day*time_of_day, data = subject_means)
summary(aov.out)
#>                  Df Sum Sq Mean Sq F value Pr(>F)
#> day               6   2039   339.9   1.038  0.401
#> time_of_day       1    156   156.3   0.477  0.490
#> day:time_of_day   6   1443   240.6   0.734  0.622
#> Residuals       336 110072   327.6
```

Just like we can treat a one-way ANOVA as a regression, we can also treat a Factorial ANOVA as a multiple regression:


```r
subject_means <- weight_data %>%
  group_by(subject_number,day, time_of_day) %>%
  summarize(mean_weight = mean(weights), .groups="drop")

subject_means$day <-as.factor(subject_means$day)
subject_means$time_of_day <-as.factor(subject_means$time_of_day)

subject_means
#> # A tibble: 350 × 4
#>    subject_number day   time_of_day mean_weight
#>             <int> <fct> <fct>             <dbl>
#>  1              1 F     Evening            65.5
#>  2              1 F     Morning            90.9
#>  3              1 M     Evening            84.9
#>  4              1 M     Morning            84.5
#>  5              1 S     Evening           101. 
#>  6              1 S     Morning           104. 
#>  7              1 Sa    Evening           112. 
#>  8              1 Sa    Morning           116. 
#>  9              1 T     Evening            99.8
#> 10              1 T     Morning           117. 
#> # … with 340 more rows

lm.out <- lm(mean_weight ~ day*time_of_day, data = subject_means)
summary(lm.out)
#> 
#> Call:
#> lm(formula = mean_weight ~ day * time_of_day, data = subject_means)
#> 
#> Residuals:
#>     Min      1Q  Median      3Q     Max 
#> -51.947 -11.853  -0.171  11.607  54.663 
#> 
#> Coefficients:
#>                          Estimate Std. Error t value Pr(>|t|)    
#> (Intercept)               92.2268     3.6199  25.478   <2e-16 ***
#> dayM                      10.6690     5.1193   2.084   0.0379 *  
#> dayS                       5.9800     5.1193   1.168   0.2436    
#> daySa                      6.6601     5.1193   1.301   0.1942    
#> dayT                      10.8615     5.1193   2.122   0.0346 *  
#> dayTh                      3.8682     5.1193   0.756   0.4504    
#> dayW                       8.0789     5.1193   1.578   0.1155    
#> time_of_dayMorning         4.1781     5.1193   0.816   0.4150    
#> dayM:time_of_dayMorning   -9.6100     7.2398  -1.327   0.1853    
#> dayS:time_of_dayMorning    3.0051     7.2398   0.415   0.6783    
#> daySa:time_of_dayMorning  -3.0615     7.2398  -0.423   0.6727    
#> dayT:time_of_dayMorning   -6.0307     7.2398  -0.833   0.4054    
#> dayTh:time_of_dayMorning   0.7375     7.2398   0.102   0.9189    
#> dayW:time_of_dayMorning   -4.9324     7.2398  -0.681   0.4962    
#> ---
#> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> Residual standard error: 18.1 on 336 degrees of freedom
#> Multiple R-squared:  0.032,	Adjusted R-squared:  -0.00545 
#> F-statistic: 0.8545 on 13 and 336 DF,  p-value: 0.6019
anova(lm.out)
#> Analysis of Variance Table
#> 
#> Response: mean_weight
#>                  Df Sum Sq Mean Sq F value Pr(>F)
#> day               6   2039  339.90  1.0376 0.4006
#> time_of_day       1    156  156.27  0.4770 0.4903
#> day:time_of_day   6   1443  240.56  0.7343 0.6223
#> Residuals       336 110072  327.60
```

## Lab 1 Generalization Assignment

<iframe width="560" height="315" src="https://www.youtube.com/embed/pwxqxTlDHHo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

NOTE: For Spring 2022, following the video in the [semester long project tab](https://www.crumplab.com/rstatsmethods/articles/Stats2/semester_project.html) for setting up your new R project and github repo. You will be making an .Rmd, adding it to your vignettes folder, and then displaying your work on your pkgdown website.

### Instructions

Your assignment instructions are the following:

1.  Work inside the new R project that you created
2.  Create a new R Markdown document called "Lab1.Rmd", inside the vignettes folder
3.  Use Lab1.Rmd to show your work attempting to solve the following generalization problems. Commit your work regularly so that it appears on your Github repository.
4.  **For each problem, make a note about how much of the problem you believe you can solve independently without help**. For example, if you needed to watch the help video and are unable to solve the problem on your own without copying the answers, then your note would be 0. If you are confident you can complete the problem from scratch completely on your own, your note would be 100. It is OK to have all 0s or 100s anything in between.
5. When you have finished, compile your pkgdown website by running `pkgdown::build_site()` so that your lab work is displayed on your website.
6. Push everything to github, make sure your github repo is public, and that github pages is enabled. Make sure you can view your website. Submit the URL to your website on blackboard.

### Problem (6 points)

1. Download the `Lab1_data.xlsx` data file. This file contains fake data for a 2x3x2 repeated measures design, for 10 participants. The data is in wide format. Here is the link. 

<https://github.com/CrumpLab/rstatsmethods/raw/master/vignettes/Stats2/Lab1_data.xlsx>

Your task is to convert the data to long format, and store the long-format data in a data.frame or tibble. Print out some of the long-form data in your lab1.Rmd, to show that you did make the appropriate conversion. For extra fun, show two different ways to solve the problem. 

If you need to modify the excel by hand to help you solve the problem that is OK, just make a note of it in your lab work.



## References
