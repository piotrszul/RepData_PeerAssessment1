# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data


```r
activity_data <- read.csv(unz("activity.zip", filename = "activity.csv"))
head(activity_data)
```

```
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
```

```r
summary(activity_data)
```

```
##      steps               date          interval   
##  Min.   :  0.0   2012-10-01:  288   Min.   :   0  
##  1st Qu.:  0.0   2012-10-02:  288   1st Qu.: 589  
##  Median :  0.0   2012-10-03:  288   Median :1178  
##  Mean   : 37.4   2012-10-04:  288   Mean   :1178  
##  3rd Qu.: 12.0   2012-10-05:  288   3rd Qu.:1766  
##  Max.   :806.0   2012-10-06:  288   Max.   :2355  
##  NA's   :2304    (Other)   :15840
```



## What is mean total number of steps taken per day?


```r
steps_by_day <- xtabs(steps ~ date, data = activity_data, na.action = na.omit)
head(steps_by_day)
```

```
## date
## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
##          0        126      11352      12116      13294      15420
```

```r
hist(steps_by_day, )
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 


Summary statistics:

* Mean of total steps per day: **9354.2295**
* Median of total steps per day: **1.0395 &times; 10<sup>4</sup>**


```r
mean(steps_by_day)
```

```
## [1] 9354
```

```r
median(steps_by_day)
```

```
## 2012-10-20 
##      10395
```



## What is the average daily activity pattern?


```r
avg_steps_per_interval <- aggregate(steps ~ interval, data = activity_data, 
    FUN = mean, na.action = na.omit)
head(avg_steps_per_interval)
```

```
##   interval   steps
## 1        0 1.71698
## 2        5 0.33962
## 3       10 0.13208
## 4       15 0.15094
## 5       20 0.07547
## 6       25 2.09434
```

```r
plot(avg_steps_per_interval, type = "l")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 


Interval **835** contains the maximum number of steps, on average across all the days in the dataset.

## Imputing missing values

Total numer is missing step values in the data set is: **2304**

Filling-in non avaliable step values by using the average value of for the interval across all days.


```r
merged_data <- merge(activity_data, avg_steps_per_interval, by = "interval")
complete_activity_data <- data.frame(steps = ifelse(is.na(merged_data$steps.x), 
    merged_data$steps.y, merged_data$steps.x), date = merged_data$date, interval = merged_data$interval)
head(completed_activity_data)
```

```
## Error: object 'completed_activity_data' not found
```




```r
complete_steps_by_day <- xtabs(steps ~ date, data = complete_activity_data, 
    na.action = na.omit)
head(complete_steps_by_day)
```

```
## date
## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
##      10766        126      11352      12116      13294      15420
```

```r
hist(complete_steps_by_day)
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6.png) 



Summary statistics:
* Mean of total steps per day: **1.0766 &times; 10<sup>4</sup>**
* Median of total steps per day: **1.0766 &times; 10<sup>4</sup>**


```r
mean(complete_steps_by_day)
```

```
## [1] 10766
```

```r
median(complete_steps_by_day)
```

```
## 2012-11-04 
##      10766
```


## Are there differences in activity patterns between weekdays and weekends?
