
Reproducible Research: Peer Assessment 1

Loading and preprocessing the data


```r
        # copy the data into your working directory
        # Load provided data
        data=read.csv("activity.csv",stringsAsFactors = FALSE)
        # Change class for the date variable
        data[[2]]=as.Date(data[[2]])
```

The Data provided has three columns--the number of steps (steps), the date (date) and a label for the 5 minute interval (interval). Missing values in steps where coded as NA and dates were coded in the ???YYYY-MM-DD??? format.

What is mean total number of steps taken per day?


```r
        sum<- aggregate(steps~date, FUN=sum, data=data , na.rm=TRUE)
        m=mean(sum[[2]])
        me=median(sum[[2]])
```

A mean of 10766.19 and a median of 10765 steps per day is seen.

histogram of total steps taken per day is given below :


```r
        plot(sum[[1]],sum[[2]],type="h",main="Number of steps taken daily", xlab= "Date",ylab="No. of steps taken")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 

What is the average daily activity pattern?

The 104th interval contains the maximum number of steps ie. 206.1698 , averaged across all days.

Plot of average number of steps taken during each interval, across the days is provided below: 


```r
        #Summarize the data by interval
        mean_interval<-aggregate(steps~interval, FUN="mean",data=data, na.rm=TRUE)
        #maximum value calculation
        x=mean_interval[mean_interval[[2]]==max(mean_interval[[2]]),]

        plot(mean_interval[[1]],mean_interval[[2]],type="l",xlab="Interval", ylab = "Average steps per interval",main=" Average of steps taken for each interval")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 


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

        # Merge the replacement values
        mean_interval <- merge(data,mean_interval[,c('interval', 'imputed_steps')],
                          by='interval')

     
         for(i in 1:nrow(mean_interval))
         {
                 if(is.na(mean_interval[i,2]))
                 {
                         mean_interval[i,2]=mean_interval[i,4]
                 }
         }
        
        # Remove unnecesary data
        mean_interval$imputed_steps <- NULL

        sum2<- aggregate(steps~date, FUN="sum",data=mean_interval)
        m2=mean(sum2[[2]])
        me2=median(sum2[[2]])
```

percent change in mean  before and after putting the NA Values


```r
        ((m-m2)/m)*100
```

```
## [1] 0.1525
```

percent change in median before and after putting the NA Values


```r
        ((me-me2)/me)*100
```

```
## [1] 1.152
```

histogram of the new data set 


```r
        plot(sum2[[1]],sum2[[2]],type="h",main="Number of steps taken daily (added values)", xlab= "Date" ,ylab="No. of steps taken")
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8.png) 

weekdays and weekend comparision


```r
       ##diving the data into weekdays and weekend
        wk_days=data[weekdays(data[[2]])=="Sunday"|weekdays(data[[2]])=="Saturday",]
        wk_ends=data[weekdays(data[[2]])=="Monday"|weekdays(data[[2]])=="Tuesday"|weekdays(data[[2]])=="Wednesday"|weekdays(data[[2]])=="Thursday"|weekdays(data[[2]])=="Friday",]
        
        #summarise each dataframe- accd. to interval
        wd_mean<- aggregate(steps~interval, FUN="mean",data=wk_days)
        we_mean<- aggregate(steps~interval, FUN="mean",data=wk_ends)
        
        par(mfrow=c(2,1))

        plot(wd_mean[[1]],wd_mean[[2]],xlab="Interval", ylab = "Average steps per interval",main=" Average of steps taken for each interval-weekdays",type="l")
        
        plot(we_mean[[1]],we_mean[[2]],xlab="Interval", ylab = "Average steps per interval",main=" Average of steps taken for each interval-weekends",type="l")
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 

what we can clearly see for the above graph is that people have excericed less during the weekends.
