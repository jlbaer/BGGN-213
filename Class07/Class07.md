Class07
================
Jason Baer
10/23/2019

revisit our functions from last day
===================================

``` r
source("http://tinyurl.com/rescale-R")
```

lets try our rescale() function
===============================

``` r
rescale(1:10)
```

    ##  [1] 0.0000000 0.1111111 0.2222222 0.3333333 0.4444444 0.5555556 0.6666667
    ##  [8] 0.7777778 0.8888889 1.0000000

``` r
rescale( c(3, 10, NA, 7) )
```

    ## [1] 0.0000000 1.0000000        NA 0.5714286

``` r
x <- c(1, 2, NA, 3, NA)
y<- c(NA, 3, NA, 3, 4)

is.na(x)
```

    ## [1] FALSE FALSE  TRUE FALSE  TRUE

``` r
is.na(y)
```

    ## [1]  TRUE FALSE  TRUE FALSE FALSE

``` r
which(is.na(x))
```

    ## [1] 3 5

Working snippet of code

``` r
is.na(x) & is.na(y)
```

    ## [1] FALSE FALSE  TRUE FALSE FALSE

``` r
both_na <- function(x , y) {
  sum(is.na(x) & is.na(y))
}
```

``` r
both_na(x, y)
```

    ## [1] 1

R recycles vector values, just like it does for colors!! it takes the shorter value and makes it as long as the longer value by RECYCLING
=========================================================================================================================================

Check for NA elements in both input vectors and don't allow re-cycling
----------------------------------------------------------------------

``` r
both_na2 <- function(x , y) {
  ## Check for NA elements in both input vectors and don't allow re-cycling
  if (length(x) != length(y)) {
   stop("Input values should be the same length")
 }
   sum(is.na(x) & is.na(y))
}
```

``` r
function(x, y) {
  ## Print some info on where NA's are as well as the number of them 
  if(length(x) != length(y)) {
    stop("Input x and y should be vectors of the same length", call.=FALSE)
  }
  na.in.both <- ( is.na(x) & is.na(y) )
  na.number  <- sum(na.in.both)
  na.which   <- which(na.in.both)

  message("Found ", na.number, " NA's at position(s):", 
          paste(na.which, collapse=", ") ) 
  
  return( list(number=na.number, which=na.which) )
}
```

    ## function(x, y) {
    ##   ## Print some info on where NA's are as well as the number of them 
    ##   if(length(x) != length(y)) {
    ##     stop("Input x and y should be vectors of the same length", call.=FALSE)
    ##   }
    ##   na.in.both <- ( is.na(x) & is.na(y) )
    ##   na.number  <- sum(na.in.both)
    ##   na.which   <- which(na.in.both)
    ## 
    ##   message("Found ", na.number, " NA's at position(s):", 
    ##           paste(na.which, collapse=", ") ) 
    ##   
    ##   return( list(number=na.number, which=na.which) )
    ## }

``` r
s1 <- c(100, 100, 100, 100, 100, 100, 100, 90)
s2 <- c(100, NA, 90, 90, 90, 90, 97, 80)

final_grade <- function(x) {
  x[is.na(x)] <- 0 #convert all NAs to 0
  mean(x[-which.min(x)], na.rm = TRUE) #takes the mean of all of a vector, subtracting the lowest value
}
```

``` r
final_grade(s2)
```

    ## [1] 91

``` r
all_grades <- read.csv("http://tinyurl.com/gradeinput")
rownames(all_grades) <- all_grades[,1]

apply(all_grades[,-1], 1, final_grade)
```

    ##  student-1  student-2  student-3  student-4  student-5  student-6 
    ##      91.75      82.50      84.25      84.25      88.25      89.00 
    ##  student-7  student-8  student-9 student-10 student-11 student-12 
    ##      94.00      93.75      87.75      79.00      86.00      91.75 
    ## student-13 student-14 student-15 student-16 student-17 student-18 
    ##      92.25      87.75      78.75      89.50      88.00      94.50 
    ## student-19 student-20 
    ##      82.75      82.75
