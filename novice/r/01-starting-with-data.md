

# Analyzing Patient Data

We are studying inflammation in patients who have been given a new treatment for arthritis,
and need to analyze the first dozen data sets. 
The data sets are stored in .csv each row holds information for a single patient, 
and the columns represent successive days. 
The first few rows of our first file look like this:

	0,0,1,3,1,2,4,7,8,3,3,3,10,5,7,4,7,7,12,18,6,13,11,11,7,7,4,6,8,8,4,4,5,7,3,4,2,3,0,0
	0,1,2,1,2,1,3,2,2,6,10,11,5,9,4,4,7,16,8,6,18,4,12,5,12,7,11,5,11,3,3,5,4,4,5,5,1,1,0,1
	0,1,1,3,3,2,6,2,5,9,5,7,4,5,4,15,5,11,9,10,19,14,12,17,7,12,11,7,4,2,10,5,4,2,2,3,2,2,1,1
	0,0,2,0,4,2,2,1,6,7,10,7,9,13,8,8,15,10,10,7,17,4,4,7,6,15,6,4,9,11,3,5,6,3,3,4,2,3,2,1
	0,1,1,3,3,1,3,5,2,4,4,7,6,5,3,10,8,10,6,17,9,14,9,7,13,9,12,6,7,7,9,6,3,2,2,4,2,0,1,1

### We want to:

* load that data into memory,
* calculate the average inflammation per day across all patients, and
* plot the result.
To do all that, we'll have to learn a little bit about programming.

### Objectives
* Explain what a package is, and what packages are used for.
* Load an R package and use the things it contains.
* Read tabular data from a file into a program.
* Assign values to variables.
* Learn about data types.
* Select individual values and subsections from data.
* Perform operations on arrays of data.
* Display simple graphs.

## Loading Data

Words are useful, but what's more useful are the sentences and stories we use them to build. 
Similarly, while a lot of powerful tools are built into languages like R, even more lives in the packages they are used to build.
Importing a package is like getting a piece of lab equipment out of a storage locker and setting it up on the bench. 
Once it's done, we can ask the package to do things for us.

To load our inflammation data, we need to locate our data.
We will use `setwd()` and `read.csv()`. These are built-in functions in R. Let's check out the help screen.

* download the inflammation file
* put it in your working directory for these exercises

Change the current working directory to the location of the CSV file, e.g.


```r
setwd("~/Copy/SoftwareCarpentry/2014-05-12-oicr-toronto/novice/r")  #Change to the correct path.
# Can use Ctrl+Shift+K, but be sure to copy the command to a script.
```


then load the data into R

```r
read.csv("data/inflammation-01.csv", header = FALSE)
```


The expression `read.csv()` is a function call that asks R to run the function `read.csv()` that belongs to base R. 

`read.csv()` has many arguments including the name of the file we want to read, and the delimiter that separates values on a line. 

When we are finished typing and press `Control+Enter` on Windows/Linux or `Cmd + Return` on Mac, the console runs our command. 
Since we haven't told it to do anything else with the function's output, the console displays it.
In this case, that output is the data we just loaded. 

Our call to `read.csv()` read the file, but didn't save the data as an object. 
To do that, we need to assign the data frame to a variable. 
A variable is just a name for a value, such as `x`, `current_temperature`, or `subject_id`. 
We can create a new variable simply by assigning a value to it using `<-`


```r
weight_kg <- 55
```


Once a variable has a value, we can print it:


```r
weight_kg
```


and do arithmetic with it:


```r
2.2 * weight_kg
```


We can also change an object's value by assigning it a new value:


```r
weight_kg <- 57.5
```


If we imagine the variable as a sticky note with a name written on it, 
assignment is like putting the sticky note on a particular value:

This means that assigning a value to one object does not change the values of other variables. 
For example, let's store the subject's weight in pounds in a variable:


```r
weight_lb <- 2.2 * weight_kg
weight_kg
weight_lb
```


and then change `weight_kg`:


```r
weight_kg <- 100
weight_kg
weight_lb
```


__Updating a Variable__

Since `weight_lb` doesn't "remember" where its value came from, it isn't automatically updated when `weight_kg` changes. 
This is different from the way spreadsheets work.

__Challenges__

Draw diagrams showing what variables refer to what values after each statement in the following program:


```r
mass <- 47.5
age <- 122
mass <- mass * 2
age <- age - 20
```


We can also add to variable that are vectors, and update them by making them longer.


```r
weights <- 100
weights <- c(weights, 80)
```


What happens here is that we take the original vector weights, and we are adding the second item to the end of the first one. 
We can do this over and over again to build a vector or a dataset. 
As we program, this may be useful to autoupdate results that we are collecting or calculating.

Now that we know how to assign things to variables, let's re-run `read.csv` and save its result:


```r
dat <- read.csv("data/inflammation-01.csv", header = FALSE)
```


This statement doesn't produce any output because assignment doesn't display anything. If we want to check that our data has been loaded, we can print the variable's value:


```r
dat
```


__BREAK__
* Make sure everyone has imported the data
* How many rows and columns are there?
* What kind of data type is it?


## Manipulating Data

Now that our data is in memory, we can start doing things with it. 
First, let's ask what type of thing `dat` *is*:


```r
class(dat)
str(dat)
```


The output tells us that data currently is a data frame in R.

This is similar to a spreadsheet in MS Excel.

#### Useful data frame functions

* `head()` - shown first 6 rows
* `tail()` - show last 6 rows
* `dim()` - returns the dimensions
* `nrow()` - number of rows
* `ncol()` - number of columns
* `str()` - structure of each column
* `names()` - shows the `names` attribute for a data frame, which gives the column names.

`str` output tells us the dimensions and the data types (int is integer) of each column.

We can see what its shape is like this:


```r
dim(dat)
nrow(dat)
ncol(dat)
```


This tells us that data has 60 rows and 40 columns.

### Indexing

If we want to get a single value from the data frame, we must provide an row and column indices for the value we want in square brackets:


```r
dat[1, 1]
dat[30, 20]
```


R indexes starting at 1. Programming languages like Fortran, MATLAB, and R start counting at 1, because that's what human beings have done for thousands of years. 
Languages in the C family (including C++, Java, Perl, and Python) count from 0 because that's simpler for computers to do. 

An index like `[30, 20]` selects a single element of a data frame, but we can select whole sections as well. 
For example, we can select the first ten days (columns) of values for the first four patients (rows) like this:


```r
dat[1:4, 1:10]
```


The notation `1:4` means, "Start at index 1 and go to index 4." 
We don't start slices at 0:


```r
dat[5:10, 0:10]
```


and we don't have to take all the values in the slice, we can use `c()` to select certain values or groups of values:


```r
dat[c(1:10, 20:30), c(1:10, 20:30)]
```


Here we have taken rows and columns 1 through 10 and 20 through 30.


```r
dat[seq(1, 12, 3), seq(1, 20, 3)]
```


Here we have used the built-in function `seq()` to take regularly spaced rows and columns.
For example, we have taken rows 1, 4, 7, and 10, and columns 1, 4, 7, 10, 13, 16, and 19. 
(Again, we always include the lower bound, but stop when we reach or cross the upper bound.).
Remember, `1:10` is shorthand for `seq(from = 1, to = 10, by = 1)`.

If we want to know the average inflammation for all patients on all days, we cannot directly take the mean of a data frame. But we can take it from a matrix.

Lets convert our data frame to a matrix, but give it a new name:


```r
mat <- data.matrix(dat)
```


And then take the mean of all the values:


```r
mean(mat)
```


There are lots of useful built-in commands that we can use in R:


```r
max(mat)  #maximum
min(mat)  #minimum
sd(mat)  #standard deviation
```


When analyzing data, though, we often want to look at partial statistics, such as the maximum value per patient or the average value per day. 


```r
max(dat[1, ])
```


__EXERCISES__

1. If `dat` holds our data frame of patient data, what does `dat[3:3, 4:4]` produce? 
What about `dat[3:3, 4:1]`? Explain the results to the person sitting next to you.


## Functions - Operations Across Axes

What if we need the maximum inflammation for all patients, or the average for each day? 
As the diagram below shows, we want to perform the operation across an axis:

<!-- FIXME: needs I presume the rBlocks code here to produce the figure? -->

To support this, in R we can use the `apply()` function:


```r
help(apply)  #or ?apply
```


`apply()` allows us to repeat a function on all of the rows (`MARGIN = 1`), columns (`2`), or both(`1:2`) of a matrix (or higher dimensions of an array).

If each row is a patient, and we want to know each patient's average inflammation, we will need to iterate our method across all of the rows. 
	

```r
avg_inflammation <- apply(dat, 2, mean)
```


Some operations, such as the column-wise means have more efficient alternatives. For example `rowMeans()` and `colMeans()`.

```r
colMeans(dat)
```


### Challenge  
1. Find the maximum and minimum values for inflammation at each day (rows are patients, and columns are days).
2. Save these values to a varible.
3. What is the length of your new variable?




We can also create a vector of our study days (the number of columns in data)


```r
day <- 1:40
# or
day <- 1:ncol(dat)
```


## Plotting  
The mathematician Richard Hamming once said, "The purpose of computing is insight, not numbers," and the best way to develop insight is often to visualize data. Visualization deserves an entire lecture (or course) of its own, but we can explore a few features of R's base plotting package and **ggplot2** here. 

Lets use the average inflammation data that we saved and plot it over the study time. 


```r
plot(day, avg_inflammation)
```

![plot of chunk unnamed-chunk-30](figure/unnamed-chunk-30.png) 


The result is roughly a linear rise and fall, which is suspicious: based on other studies, we expect a sharper rise and slower fall. Let's have a look at two other statistics:


```r
plot(day, max_inflammation)
```

![plot of chunk unnamed-chunk-31](figure/unnamed-chunk-311.png) 

```r
plot(day, min_inflammation)
```

![plot of chunk unnamed-chunk-31](figure/unnamed-chunk-312.png) 


The maximum value rises and falls perfectly smoothly, while the minimum seems to be a step function. 
Neither result seems particularly likely, so either there's a mistake in our calculations or something is wrong with our data.

##ggplot2

Now that we have all this summary information, we can put it back together into a data frame for plotting with ggplot2.


```r
d.summary <- data.frame(day, avg_inflammation, min_inflammation, max_inflammation)
```



```r
library(ggplot2)
ggplot(d.summary, aes(day, avg_inflammation)) + geom_point()
```

![plot of chunk unnamed-chunk-33](figure/unnamed-chunk-33.png) 


__EXERCISES__

1. Create a plot showing the standard deviation of the inflammation data 
for each day across all patients.

2. Create a ggplot that shows the data as a line. As a histogram. etc.

## Key Points

* Import a package into your workspace using `library("pkgname")`.
* The key data types in R?
* Use `variable <- value` to assign a value to a variable in order to record it in memory.
* Objects are created on demand whenever a value is assigned to them.
* Use `print(something)` to display the value of something.
* The expression `dim()` gives the dimensions of a data frame or matrix.
* Use `object[x, y]` to select a single element from an array.
* Object indices start at 1.
* Use `low:high` to specify a slice that includes the indices from `low` to `high`.
* Use `#` to add comments to programs.
* Use `mean()`, `max()`, `min()` and `sd()` to calculate simple statistics.
* Update vectors using `append()`
<!-- * Write a simple for loop -->
* Use base R and the **ggplot2** package for creating simple visualizations.

## Next Steps

Our work so far has convinced us that something's wrong with our first data file. We would like to check the other 11 the same way, but typing in the same commands repeatedly is tedious and error-prone. Since computers don't get bored (that we know of), we should create a way to do a complete analysis with a single command, and then figure out how to repeat that step once for each file. These operations are the subjects of the next two lessons.
