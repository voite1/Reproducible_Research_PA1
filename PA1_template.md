# Peer Assessment 1

The document is created to satisfy requirements for Peers Assessment 1, Reproducible Research class offered by Coursera. Today is Wed Jan 07 18:24:09 2015. 

### Loading and preprocessing the data

The code below downloads the data from the internet, unzips the file, and loads the content of the file in the variable. The script also converts the *date* filed to *Date*. The data is obtained from [Activity Monitoring Data](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip).

#### Load the data

Unzipping the data file and loading the *activity.csv* file into the variable called *data*.  The variable is used to hold the content of the entire file, including NA entries.


```r
unzip("activity.zip")
data <- read.csv("activity.csv")
```

#### Process/transform the data (if necessary) into a format suitable for your analysis

The date field is converted to Date using *as.Date* function to assure that any date-related calculations will be executed properly.


```r
data$date <- as.Date(data$date)
```

### What is mean total number of steps taken per day?

The script below displays the histogram, mean, and median for the steps taken each day. *ggplot2* library is used to construct the histogram using *qplot* function. Disregard the warning recelived during the load of the *ggplot2* library. I am using R 3.1.1, but the *ggplot2* library is compiled using R 3.1.2. This should not make any difference.

#### Make a histogram of the total number of steps taken each day

The histogram is created using *qplot* function. *tapply* function was used to sum up the number of steps per day. Additiona, all the NA entries were removed when building *steps*.


```r
steps <- tapply(data$steps, data$date, FUN=sum, na.rm=TRUE)
library(ggplot2)
qplot(steps, binwidth=800, xlab="Daily Steps")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 

#### Calculate and report the mean and median total number of steps taken per day

Calling *mean* and *median* functions on the *steps* dataset to obtain corresponding mean and median as required by the assignment.



```r
mean(steps)
```

```
## [1] 9354
```

```r
median(steps)
```

```
## [1] 10395
```

### What is average daily activity patter?

#### Make a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

The *aggregate* function is used to derive average of the steps per 5 minute interval. *ggplot2* library is used to creage time series plot.


```r
avgs <- aggregate(x=list(msteps=data$steps), by=list(interval=data$interval), 
        FUN=mean, na.rm=TRUE)

ggplot(avgs, aes(x=interval, y=msteps)) + geom_line() + xlab("Interval (5-min)") + 
       ylab("Steps (average)") 
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 

#### Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

Displaying the interval that contains the most daily steps: the interval number, and the number of steps. We display the whole row anlog with the row number form the *avgs*.


```r
avgs[which.max(avgs$msteps),]
```

```
##     interval msteps
## 104      835  206.2
```

### Imputing missing values

### Are there any differences in activity patters between weekdays and weekends?