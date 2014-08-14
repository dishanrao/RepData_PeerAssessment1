Reproducible Research: Peer Assessment 1

Loading and preprocessing the data


```r
        # copy the data into your working directory
        # Load provided data
        data=read.csv("activity.csv",stringsAsFactors = FALSE)
```

```
## Warning: cannot open file 'activity.csv': No such file or directory
```

```
## Error: cannot open the connection
```

```r
        # Change class for the date variable
        data[[2]]=as.Date(data[[2]])
```

```
## Error: object of type 'closure' is not subsettable
```

```r
        # Define some options for knitr
        knitr::opts_chunk$set(tidy=FALSE, fig.path='figures/')
```

The Data provided has three columns--the number of steps (steps), the date (date) and a label for the 5 minute interval (interval). Missing values in steps where coded as NA and dates were coded in the ???YYYY-MM-DD??? format.

What is mean total number of steps taken per day?


```r
        sum<- aggregate(steps~date, FUN=sum, data=data , na.rm=TRUE)
```

```
## Error: cannot coerce class '"function"' into a data.frame
```

```r
        m=mean(sum[[2]])
```

```
## Error: object of type 'builtin' is not subsettable
```

```r
        me=median(sum[[2]])
```

```
## Error: object of type 'builtin' is not subsettable
```

A mean of 10766.19 and a median of 10765 steps per day is seen.

histogram of total steps taken per day is given below :


```r
        plot(sum[[1]],sum[[2]],type="h",main="Number of steps taken daily", xlab= "Date",ylab="No. of steps taken")
```

```
## Error: object of type 'builtin' is not subsettable
```

What is the average daily activity pattern?

The 104th interval contains the maximum number of steps ie. 206.1698 , averaged across all days.

Plot of average number of steps taken during each interval, across the days is provided below: 


```r
        #Summarize the data by interval
        mean_interval<-aggregate(steps~interval, FUN="mean",data=data, na.rm=TRUE)
```

```
## Error: cannot coerce class '"function"' into a data.frame
```

```r
        #maximum value calculation
        x=mean_interval[mean_interval[[2]]==max(mean_interval[[2]]),]
```

```
## Error: object 'mean_interval' not found
```

```r
        plot(mean_interval[[1]],mean_interval[[2]],type="l",xlab="Interval", ylab = "Average steps per interval",main=" Average of steps taken for each interval")
```

```
## Error: object 'mean_interval' not found
```


Inputing missing values

completing the missing cases using the average number of steps from the corresponding interval (rounded towards zero to avoid using fractional steps). Instead of average steps per day as , it has more variation compared to average steps per interval.


```r
        ## unique(is.na(data[[3]]))
        ## unique(is.na(data[[2]]))
        ## by exe. above statements - resul = FALSE for both the cases
        ## while unique(is.na(data[[1]])) results in both TRUE and FALSE
        ## N.A only in the first column
        
        # Use previously calculated means
        mean_interval$imputed_steps <- floor(mean_interval$steps)
```

```
## Error: object 'mean_interval' not found
```

```r
        # Merge the replacement values
        mean_interval <- merge(data,mean_interval[,c('interval', 'imputed_steps')],
                          by='interval')
```

```
## Error: cannot coerce class '"function"' into a data.frame
```

```r
         for(i in 1:nrow(mean_interval))
         {
                 if(is.na(mean_interval[i,2]))
                 {
                         mean_interval[i,2]=mean_interval[i,4]
                 }
         }
```

```
## Error: object 'mean_interval' not found
```

```r
        # Remove unnecesary data
        mean_interval$imputed_steps <- NULL
```

```
## Error: object 'mean_interval' not found
```

```r
        sum2<- aggregate(steps~date, FUN="sum",data=mean_interval)
```

```
## Error: object 'mean_interval' not found
```

```r
        m2=mean(sum2[[2]])
```

```
## Error: object 'sum2' not found
```

```r
        me2=median(sum2[[2]])
```

```
## Error: object 'sum2' not found
```

percent change in mean  before and after putting the NA Values


```r
        ((m-m2)/m)*100
```

```
## Error: object 'm' not found
```

percent change in median before and after putting the NA Values


```r
        ((me-me2)/me)*100
```

```
## Error: object 'me' not found
```

histogram of the new data set 


```r
        plot(sum2[[1]],sum2[[2]],type="h",main="Number of steps taken daily (added values)", xlab= "Date" ,ylab="No. of steps taken")
```

```
## Error: object 'sum2' not found
```

weekdays and weekend comparision


```r
       ##diving the data into weekdays and weekend
        wk_days=data[weekdays(data[[2]])=="Sunday"|weekdays(data[[2]])=="Saturday",]
```

```
## Error: object of type 'closure' is not subsettable
```

```r
        wk_ends=data[weekdays(data[[2]])=="Monday"|weekdays(data[[2]])=="Tuesday"|weekdays(data[[2]])=="Wednesday"|weekdays(data[[2]])=="Thursday"|weekdays(data[[2]])=="Friday",]
```

```
## Error: object of type 'closure' is not subsettable
```

```r
        #summarise each dataframe- accd. to interval
        wd_mean<- aggregate(steps~interval, FUN="mean",data=wk_days)
```

```
## Error: object 'wk_days' not found
```

```r
        we_mean<- aggregate(steps~interval, FUN="mean",data=wk_ends)
```

```
## Error: object 'wk_ends' not found
```

```r
        par(mfrow=c(2,1))

        plot(wd_mean[[1]],wd_mean[[2]],xlab="Interval", ylab = "Average steps per interval",main=" Average of steps taken for each interval-weekdays",type="l")
```

```
## Error: object 'wd_mean' not found
```

```r
        plot(we_mean[[1]],we_mean[[2]],xlab="Interval", ylab = "Average steps per interval",main=" Average of steps taken for each interval-weekends",type="l")
```

```
## Error: object 'we_mean' not found
```

what we can clearly see for the above graph is that people have excericed less during the weekends.
