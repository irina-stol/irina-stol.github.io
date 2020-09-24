---
layout: Clustering power demand 
title: "This is a test post for my R blog"
date: 2020-09-24
categories: rblogging
tags: ggplot2 R clustering 
---


Clustering power data
=====================

Initial data analysis tasks
---------------------------

1.  Summarise the data in the Characteristics.csv dataset, and plot the
    distributions for the percentage of industrial and commercial
    customers, transformer ratings and pole or ground monitored
    substations.

<!-- -->

    # Load the 'Charecteristics' data
    Char <-read.csv('data/Characteristics (1).csv', stringsAsFactors = FALSE)
    # Change transformer type to factor 
    Char$TRANSFORMER_TYPE <- as.factor(Char$TRANSFORMER_TYPE)
    # Change transformer rating to factor 
    Char$Transformer_RATING <- as.factor(Char$Transformer_RATING)

    # Summary table
    pander(summary(Char))

<table style="width:97%;">
<caption>Table continues below</caption>
<colgroup>
<col style="width: 27%" />
<col style="width: 44%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">SUBSTATION_NUMBER</th>
<th style="text-align: center;">TRANSFORMER_TYPE</th>
<th style="text-align: center;">TOTAL_CUSTOMERS</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">Min. :511016</td>
<td style="text-align: center;">Grd Mtd Dist. Substation :706</td>
<td style="text-align: center;">Min. : 0.0</td>
</tr>
<tr class="even">
<td style="text-align: center;">1st Qu.:521516</td>
<td style="text-align: center;">Pole Mtd Dist. Substation:242</td>
<td style="text-align: center;">1st Qu.: 3.0</td>
</tr>
<tr class="odd">
<td style="text-align: center;">Median :532653</td>
<td style="text-align: center;">NA</td>
<td style="text-align: center;">Median : 67.5</td>
</tr>
<tr class="even">
<td style="text-align: center;">Mean :534344</td>
<td style="text-align: center;">NA</td>
<td style="text-align: center;">Mean :104.3</td>
</tr>
<tr class="odd">
<td style="text-align: center;">3rd Qu.:552386</td>
<td style="text-align: center;">NA</td>
<td style="text-align: center;">3rd Qu.:179.2</td>
</tr>
<tr class="even">
<td style="text-align: center;">Max. :564512</td>
<td style="text-align: center;">NA</td>
<td style="text-align: center;">Max. :569.0</td>
</tr>
<tr class="odd">
<td style="text-align: center;">NA</td>
<td style="text-align: center;">NA</td>
<td style="text-align: center;">NA</td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 27%" />
<col style="width: 23%" />
<col style="width: 23%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">Transformer_RATING</th>
<th style="text-align: center;">Percentage_IC</th>
<th style="text-align: center;">LV_FEEDER_COUNT</th>
<th style="text-align: center;">GRID_REFERENCE</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">500 :262</td>
<td style="text-align: center;">Min. :0.00000</td>
<td style="text-align: center;">Min. : 0.000</td>
<td style="text-align: center;">Length:948</td>
</tr>
<tr class="even">
<td style="text-align: center;">300 :130</td>
<td style="text-align: center;">1st Qu.:0.01048</td>
<td style="text-align: center;">1st Qu.: 1.000</td>
<td style="text-align: center;">Class :character</td>
</tr>
<tr class="odd">
<td style="text-align: center;">315 :105</td>
<td style="text-align: center;">Median :0.17849</td>
<td style="text-align: center;">Median : 3.000</td>
<td style="text-align: center;">Mode :character</td>
</tr>
<tr class="even">
<td style="text-align: center;">800 : 84</td>
<td style="text-align: center;">Mean :0.37982</td>
<td style="text-align: center;">Mean : 2.762</td>
<td style="text-align: center;">NA</td>
</tr>
<tr class="odd">
<td style="text-align: center;">1000 : 75</td>
<td style="text-align: center;">3rd Qu.:0.90271</td>
<td style="text-align: center;">3rd Qu.: 4.000</td>
<td style="text-align: center;">NA</td>
</tr>
<tr class="even">
<td style="text-align: center;">16 : 72</td>
<td style="text-align: center;">Max. :1.00000</td>
<td style="text-align: center;">Max. :16.000</td>
<td style="text-align: center;">NA</td>
</tr>
<tr class="odd">
<td style="text-align: center;">(Other):220</td>
<td style="text-align: center;">NA</td>
<td style="text-align: center;">NA</td>
<td style="text-align: center;">NA</td>
</tr>
</tbody>
</table>

    # Plot for Transformer type 
    ggplot(Char, aes(x=TRANSFORMER_TYPE, fill=TRANSFORMER_TYPE))+
      geom_bar(stat='count')+
      labs(title='Transformer type distribution',x='Transformer type')+
      theme(legend.position='none')

<img src="/clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-3-1.png" style="display: block; margin: auto;" />

    # Proportion table 
    pander(prop.table(table(Char$TRANSFORMER_TYPE)))

<table style="width:76%;">
<colgroup>
<col style="width: 37%" />
<col style="width: 38%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">Grd Mtd Dist. Substation</th>
<th style="text-align: center;">Pole Mtd Dist. Substation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">0.7447</td>
<td style="text-align: center;">0.2553</td>
</tr>
</tbody>
</table>

    # Plot for Transformer rating 
    ggplot(Char,aes(x=Transformer_RATING, fill=TRANSFORMER_TYPE))+
      geom_bar(stat='count')+
      labs(title='Transformer rating by transformer type',x='Transformer rating')+
      theme(legend.position='bottom')

![](/clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-4-1.png)

    # Plot for Percentage of industrial and commercial customers 
    ggplot(Char, aes(x=Percentage_IC, fill=TRANSFORMER_TYPE))+
      geom_histogram(bins=25)+
      labs(title='Percentage of idustrial and commercial customers by transformer type')+
      theme(legend.position='bottom')

![](/clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-5-1.png)

1.  Using this and other analyses you think appropriate, describe the
    relationships between the different substation characteristics
    (transformer type, number of customers, rating, percentage of I&C
    customers and number of feeders).

<!-- -->

    ggplot(Char, aes(x=TOTAL_CUSTOMERS, fill=TRANSFORMER_TYPE))+
      geom_histogram(bins=30)+
      labs(title='Total customers by transformer type',
           x='Total customers')+
      theme(legend.position='bottom')

![](/clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-6-1.png)

    ggplot(Char, aes(x=TOTAL_CUSTOMERS, fill=Transformer_RATING))+
      geom_histogram(bins=30)+
      labs(title='Total customers by transformer rating',
           x='Total customers')+
      theme(legend.position='bottom')

![](/clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-7-1.png)

    ggplot(Char, aes(x=as.factor(LV_FEEDER_COUNT), fill=TRANSFORMER_TYPE))+
      geom_bar(stat='count')+
      labs(title='LV Feeder counts by transformer type',
           x='LV Feeder Count')+
      theme(legend.position='bottom')

![](/clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-8-1.png)

    ggplot(Char, aes(x=as.factor(LV_FEEDER_COUNT), fill=Transformer_RATING))+
      geom_bar(stat='count')+
      labs(title='LV Feeder counts by transformer rating',x='LV Feeder Count')+
      theme(legend.position='bottom')

![](/clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-9-1.png)

**Transformer Type**

-   Predominantly ‘Grid Mounted’ Station which make up ~75% and ~25%
    ‘Pole Mounted’
-   Grid Mounted stations have higher rating transformer types compared
    to Pole Mounted
-   The distribution of industrial and commercial customers for
    transformer type indicated that there are either a very low number
    of non-domestic customers using ‘Pole mounted’ or a very high
    number, and very limited numbers in between.
-   There are more customers that receive their power from ‘Grid
    mounted’ stations.
-   Most Stations with Low Voltage Feeder equal to 1, are ‘Pole mounted’
    stations.

**Number of Customers**

-   There is a significant amount of stations that do not serve
    ‘domestic’ customers or serve a significantly low number of
    customers.
-   Stations that serve very low number of customers or none are
    predominantly rated lower (4-50) than those that serve many
    customers.

**Transformer Rating**

-   Most popular rating is ‘500’ serving all groups of ‘total
    customers’.
-   Very low count for transformer ratings 4, 4.5, 380 and 750.

**Percentage of I&C**

-   Percentage of I&C users is concentrated at both extremes, near 0 and
    1, where there are over 250 stations that indicate there is no I&C
    customers at all, and at the other extreme that there are over 200
    stations that are fully I&C customers.

**Number of Feeders**

-   Most stations (about 300) have 1 Low Voltage Feeder, followed by 4
    Low Voltage feeders as the next most common configuration.
-   Stations with 1 Low Voltage Feeder have more stations with low
    transformer ratings (5-315), and stations with higher number of
    feeders have a higher transformer rating.

Initial clustering tasks
------------------------

Using the scaled daily measurements from the Autumn dataset perform
hierarchical clustering for the daily average demand:

1.  Using your preferred choice of a dissimilarity function, create a
    distance matrix for these data and produce a dendrogram.

<!-- -->

    # Load in data 
    get(load("data/Autumn_2012.RData"))

    # Select subset with scaled measurments 
    Autumn <- Autumn_2012[,1:146]
    # Transform Date fromat  
    Autumn$Date <- dates(Autumn_2012[,2], 
                         origin = c(month = 1,day = 1,year = 1970))

    # Calculate means for every minute interval grouped by Station 
    Aut_sample <- Autumn %>% 
                  select(-c(Date)) %>% 
                  group_by(Station) %>% 
                  summarise_all(funs(mean))

    set.seed(1234)
    # Calculate distance matrix for 144 scaled time intervals 
    distance <- dist(as.matrix(Aut_sample[,-1]))

    # Perform hierarchical clustering
    cluster <-hclust(distance)
    # Plot the dendogram 
    plot(cluster,hang=-1)

![](/clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-11-1.png)
4. Choose an appropriate number of clusters and label each substation
according to its cluster membership.

    # Cut dendogram for 7 clusters 
    clusters <- cutree(cluster,7)

    # Attach cluster membership to each Station
    result <- cbind(Aut_sample, Class=factor(unname(clusters))) %>% select(Station,Class)
    head(result)

      Station Class
    1  511029     1
    2  511030     2
    3  511033     2
    4  511034     2
    5  511035     2
    6  511079     2

1.  For each of your clusters, plot the daily average demand for 1) All
    days, 2) Weekdays, 3) Saturdays and 4) Sundays.

<!-- -->

    # Merge Autumn data set and cluster memberships for each Station
    res <- merge(Autumn,result,by='Station')
    # Chnage Date type 
    res$Date <- as.Date(res$Date)
    # Create weekdays as factors 
    res$weekday <- as.factor(weekdays(res$Date))
    ##levels(res$weekday) <- check levels 
    # Rename levels 
    levels(res$weekday) <- c("Weekday","Weekday","Saturday","Sunday",
                              "Weekday", "Weekday","Weekday")

    # Create data frame for all days 
    all_days <- res %>% 
            gather(key='minute',value,-Station,-Date,-Class,-weekday) %>%
            group_by(minute,Station,Class,Date,weekday) %>% 
            summarise(avg = mean(value))

    ## `summarise()` regrouping output by 'minute', 'Station', 'Class', 'Date' (override with `.groups` argument)

    all_days$minute <- as.numeric(all_days$minute)
    all_days <- all_days %>% group_by(minute,Class) %>% summarise(avg=mean(avg))

    ## `summarise()` regrouping output by 'minute' (override with `.groups` argument)

    # Create data frame for weekdays 
    Weekday <- res %>% filter(weekday=='Weekday') %>%
            gather(key='minute',value,-Station,-Date,-Class,-weekday) %>%
            group_by(minute,Station,Class,Date,weekday) %>% 
            summarise(avg = mean(value))

    ## `summarise()` regrouping output by 'minute', 'Station', 'Class', 'Date' (override with `.groups` argument)

    Weekday$minute <- as.numeric(Weekday$minute)
    Weekday <- Weekday %>% group_by(minute,Class) %>% summarise(avg=mean(avg))

    ## `summarise()` regrouping output by 'minute' (override with `.groups` argument)

    # Create data frame for saturdays 
    Saturday <- res %>% filter(weekday=='Saturday') %>%
            gather(key='minute',value,-Station,-Date,-Class,-weekday) %>%
            group_by(minute,Station,Class,Date,weekday) %>% 
            summarise(avg = mean(value))

    ## `summarise()` regrouping output by 'minute', 'Station', 'Class', 'Date' (override with `.groups` argument)

    Saturday$minute <- as.numeric(Saturday$minute)
    Saturday <- Saturday %>% group_by(minute,Class) %>% summarise(avg=mean(avg))

    ## `summarise()` regrouping output by 'minute' (override with `.groups` argument)

    # Create data frame for sundays 
    Sunday <- res %>% filter(weekday=='Sunday') %>%
      gather(key='minute',value,-Station,-Date,-Class,-weekday) %>%
      group_by(minute,Station,Class,Date,weekday) %>% 
      summarise(avg = mean(value))

    ## `summarise()` regrouping output by 'minute', 'Station', 'Class', 'Date' (override with `.groups` argument)

    Sunday$minute <- as.numeric(Sunday$minute)
    Sunday <- Sunday %>% group_by(minute,Class) %>% summarise(avg=mean(avg))

    ## `summarise()` regrouping output by 'minute' (override with `.groups` argument)

    # Plot avergae daily demand facetted by cluster, for days of week
    ggplot()+
      geom_line(data=all_days,aes(x=minute,y=avg,col='all days'))+
      geom_line(data=Weekday,aes(x=minute,y=avg,col='weekdays'))+
      geom_line(data=Saturday,aes(x=minute,y=avg,col='saturdays'))+
      geom_line(data=Sunday,aes(x=minute,y=avg,col='sundays'))+
      scale_x_continuous(breaks=c(1,24,48,72,96,120,144),
                         labels=c("00:00","04:00","08:00","12:00",
                                  "16:00","20:00","23:50")) +
      theme(legend.title = element_blank(),
            legend.position='bottom')+
      labs(title='Scaled daily average demand by cluster for days of week',
           x='Minute interval', y='Power')+
      facet_wrap(~Class)

![](/clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-14-1.png)

1.  Produce summaries of the variables in Characteristics.csv for each
    of your clusters.

<!-- -->

    # Select cluster membership for each station
    df1 <- res %>% select(Station,Class)
    # Rename column 1 in dataset 'Char'
    colnames(Char)[1] <- "Station"
    # merge cluster membership to 'Char' dataset by Station ID
    df2 <- merge(result,Char,by='Station')

    # Plot of transformet type by cluster membership
    ggplot(df2, aes(x=TRANSFORMER_TYPE, fill=Class))+
      geom_bar(stat='count')+
      labs(title='Transformer type by cluster membership',
           x='Transformer type')

<img src="/clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-15-1.png" style="display: block; margin: auto;" />

    for(i in sort(unique(df2$Class))){
    df2 %>% filter(Class==i) %>% summary() %>% print() 
    }

        Station       Class                    TRANSFORMER_TYPE TOTAL_CUSTOMERS 
     Min.   :511029   1:148   Grd Mtd Dist. Substation :117     Min.   :  0.00  
     1st Qu.:513475   2:  0   Pole Mtd Dist. Substation: 31     1st Qu.: 58.75  
     Median :532661   3:  0                                     Median :111.00  
     Mean   :534999   4:  0                                     Mean   :116.51  
     3rd Qu.:552278   5:  0                                     3rd Qu.:159.25  
     Max.   :563737   6:  0                                     Max.   :301.00  
                      7:  0                                                     
     Transformer_RATING Percentage_IC     LV_FEEDER_COUNT GRID_REFERENCE    
     500    :32         Min.   :0.00000   Min.   :0.000   Length:148        
     300    :29         1st Qu.:0.00000   1st Qu.:2.000   Class :character  
     315    :28         Median :0.03889   Median :3.000   Mode  :character  
     100    :16         Mean   :0.12196   Mean   :2.986                     
     200    :12         3rd Qu.:0.14073   3rd Qu.:4.000                     
     800    :11         Max.   :1.00000   Max.   :6.000                     
     (Other):20                                                             
        Station       Class                    TRANSFORMER_TYPE TOTAL_CUSTOMERS
     Min.   :511030   1:  0   Grd Mtd Dist. Substation :209     Min.   :  0.0  
     1st Qu.:513178   2:211   Pole Mtd Dist. Substation:  2     1st Qu.: 40.5  
     Median :532699   3:  0                                     Median :158.0  
     Mean   :533887   4:  0                                     Mean   :156.6  
     3rd Qu.:552824   5:  0                                     3rd Qu.:237.5  
     Max.   :564285   6:  0                                     Max.   :485.0  
                      7:  0                                                    
     Transformer_RATING Percentage_IC     LV_FEEDER_COUNT GRID_REFERENCE    
     500    :85         Min.   :0.00000   Min.   : 0.00   Length:211        
     300    :46         1st Qu.:0.08735   1st Qu.: 3.00   Class :character  
     800    :26         Median :0.28333   Median : 4.00   Mode  :character  
     315    :21         Mean   :0.42762   Mean   : 3.55                     
     1000   :21         3rd Qu.:0.84033   3rd Qu.: 5.00                     
     200    : 5         Max.   :1.00000   Max.   :10.00                     
     (Other): 7                                                             
        Station       Class                   TRANSFORMER_TYPE TOTAL_CUSTOMERS
     Min.   :511150   1: 0   Grd Mtd Dist. Substation :24      Min.   : 0.00  
     1st Qu.:512811   2: 0   Pole Mtd Dist. Substation: 4      1st Qu.: 2.75  
     Median :532664   3:28                                     Median :15.50  
     Mean   :533616   4: 0                                     Mean   :21.39  
     3rd Qu.:552015   5: 0                                     3rd Qu.:26.50  
     Max.   :564224   6: 0                                     Max.   :94.00  
                      7: 0                                                    
     Transformer_RATING Percentage_IC    LV_FEEDER_COUNT GRID_REFERENCE    
     500    :12         Min.   :0.0000   Min.   :0.000   Length:28         
     800    : 4         1st Qu.:0.9192   1st Qu.:1.000   Class :character  
     200    : 3         Median :0.9968   Median :2.000   Mode  :character  
     1000   : 3         Mean   :0.8944   Mean   :2.607                     
     300    : 2         3rd Qu.:1.0000   3rd Qu.:3.250                     
     315    : 2         Max.   :1.0000   Max.   :8.000                     
     (Other): 2                                                            
        Station       Class                  TRANSFORMER_TYPE TOTAL_CUSTOMERS 
     Min.   :511191   1:0   Grd Mtd Dist. Substation :6       Min.   : 0.000  
     1st Qu.:513673   2:0   Pole Mtd Dist. Substation:3       1st Qu.: 0.000  
     Median :532645   3:0                                     Median : 3.000  
     Mean   :531323   4:9                                     Mean   : 4.556  
     3rd Qu.:552052   5:0                                     3rd Qu.: 8.000  
     Max.   :552946   6:0                                     Max.   :15.000  
                      7:0                                                     
     Transformer_RATING Percentage_IC    LV_FEEDER_COUNT GRID_REFERENCE    
     1000   :3          Min.   :0.9566   Min.   :1.000   Length:9          
     100    :2          1st Qu.:1.0000   1st Qu.:1.000   Class :character  
     315    :2          Median :1.0000   Median :1.000   Mode  :character  
     300    :1          Mean   :0.9904   Mean   :2.222                     
     500    :1          3rd Qu.:1.0000   3rd Qu.:4.000                     
     0      :0          Max.   :1.0000   Max.   :5.000                     
     (Other):0                                                             
        Station       Class                  TRANSFORMER_TYPE TOTAL_CUSTOMERS
     Min.   :512438   1:0   Grd Mtd Dist. Substation :3       Min.   : 0.00  
     1st Qu.:512440   2:0   Pole Mtd Dist. Substation:1       1st Qu.: 0.00  
     Median :513008   3:0                                     Median : 1.50  
     Mean   :517806   4:0                                     Mean   :16.25  
     3rd Qu.:518374   5:4                                     3rd Qu.:17.75  
     Max.   :532770   6:0                                     Max.   :62.00  
                      7:0                                                    
     Transformer_RATING Percentage_IC     LV_FEEDER_COUNT GRID_REFERENCE    
     1000   :2          Min.   :0.00000   Min.   :0.00    Length:4          
     15     :1          1st Qu.:0.02551   1st Qu.:0.75    Class :character  
     315    :1          Median :0.05390   Median :1.00    Mode  :character  
     0      :0          Mean   :0.27695   Mean   :0.75                      
     4      :0          3rd Qu.:0.30534   3rd Qu.:1.00                      
     4.5    :0          Max.   :1.00000   Max.   :1.00                      
     (Other):0                                                              
        Station       Class                  TRANSFORMER_TYPE TOTAL_CUSTOMERS
     Min.   :513050   1:0   Grd Mtd Dist. Substation :4       Min.   :0.0    
     1st Qu.:513404   2:0   Pole Mtd Dist. Substation:1       1st Qu.:0.0    
     Median :513582   3:0                                     Median :1.0    
     Mean   :526709   4:0                                     Mean   :2.4    
     3rd Qu.:531459   5:0                                     3rd Qu.:5.0    
     Max.   :562052   6:5                                     Max.   :6.0    
                      7:0                                                    
     Transformer_RATING Percentage_IC    LV_FEEDER_COUNT GRID_REFERENCE    
     500    :4          Min.   :0.8781   Min.   :0.0     Length:5          
     50     :1          1st Qu.:1.0000   1st Qu.:0.0     Class :character  
     0      :0          Median :1.0000   Median :1.0     Mode  :character  
     4      :0          Mean   :0.9756   Mean   :0.8                       
     4.5    :0          3rd Qu.:1.0000   3rd Qu.:1.0                       
     5      :0          Max.   :1.0000   Max.   :2.0                       
     (Other):0                                                             
        Station       Class                  TRANSFORMER_TYPE TOTAL_CUSTOMERS
     Min.   :531057   1:0   Grd Mtd Dist. Substation :0       Min.   :0.0    
     1st Qu.:531645   2:0   Pole Mtd Dist. Substation:3       1st Qu.:0.5    
     Median :532232   3:0                                     Median :1.0    
     Mean   :531841   4:0                                     Mean   :1.0    
     3rd Qu.:532234   5:0                                     3rd Qu.:1.5    
     Max.   :532235   6:0                                     Max.   :2.0    
                      7:3                                                    
     Transformer_RATING Percentage_IC    LV_FEEDER_COUNT  GRID_REFERENCE    
     25     :1          Min.   :0.0000   Min.   :0.0000   Length:3          
     50     :1          1st Qu.:0.5000   1st Qu.:0.5000   Class :character  
     100    :1          Median :1.0000   Median :1.0000   Mode  :character  
     0      :0          Mean   :0.6667   Mean   :0.6667                     
     4      :0          3rd Qu.:1.0000   3rd Qu.:1.0000                     
     4.5    :0          Max.   :1.0000   Max.   :1.0000                     
     (Other):0                                                              

1.  Describe your clusters based on the information in
    Characteristics.csv and choose names for them. Describe the patterns
    of their power demands for each cluster.

**Cluster 1 – Domestic** Covers mainly domestic substations, as the ‘I&C
customers’ is at an average of 12%, with flat and average power demands
at daytime, then peaking in the evenings, and dropping low during night
hours.

**Cluster 2 – Domestic and Commercial**

Cluster 2 serves a mixture of domestic and commercial customers, mostly
by ‘Grid mounted’ stations. During daytime the power is relatively flat,
similar to Cluster 3, and then slightly peaks during evening/late hours
due to domestic customers.

**Cluster 3 – Commercial**

Commercial dominated stations, with ‘I&C customers’ at a mean of 89%,
where the demand is high and flat during day hours, and lower overnight,
with the exceptions of weekends, where some commercial customers may be
busier (clubs, bars).

**Cluster 4 – Industrial**

Very high I&C percentage – 94%, indicating an industrial profile, with a
load shape that is constantly high throughout the day and night.

**Cluster 5 – Economy 7**

Low mean I&C customer percentage (28%), indicating a domestic profile,
where power demand peaks at night hours. Such pattern can be observed by
‘Economy 7’ customers, who have lower electricity rates during night
hours.

**Cluster 6 – Large Commercial**

Similar to cluster 3, but with much higher proportion, since the day
power peak is more defined and power load is much lower at night. As
well as a higher mean ‘I&C customers’ percentage of 97%.

**Cluster 7– Street lighting**

Power load is high and constant at night hours, and very low during day
hours. These patterns can be found in substations serving motorways and
street lighting.

Allocating new substations
==========================

1.  For each substation, on the same plot, plot the daily average demand
    for 1) All days, 2) Weekdays, 3) Saturdays and 4) Sundays (one plot
    per new substation).

<!-- -->

    New_sub <- read_csv('data/NewSubstations.csv')

    Parsed with column specification:
    cols(
      .default = col_double(),
      Date = col_date(format = "")
    )

    See spec(...) for full column specifications.

    New_sub$X1 <- NULL
    New_sub$weekday <- as.factor(weekdays(New_sub$Date))
    levels(New_sub$weekday) <- c("Weekday","Weekday","Saturday","Sunday",
                               "Weekday", "Weekday","Weekday")

    all_days <- New_sub %>% 
                gather(key='minute',value,-Substation,-Date,-weekday) %>%
                group_by(minute,Substation,Date,weekday) %>% 
                summarise(avg = mean(value))

    `summarise()` regrouping output by 'minute', 'Substation', 'Date' (override with `.groups` argument)

    all_days$minute <- as.numeric(all_days$minute)
    all_days        <- all_days %>% group_by(minute,Substation) %>% 
                       summarise(avg=mean(avg))

    `summarise()` regrouping output by 'minute' (override with `.groups` argument)

    Weekday <- New_sub %>% filter(weekday=='Weekday')%>%
                gather(key='minute',value,-Substation,-Date,-weekday) %>%
                group_by(minute,Substation,Date,weekday) %>% 
                summarise(avg = mean(value))

    `summarise()` regrouping output by 'minute', 'Substation', 'Date' (override with `.groups` argument)

    Weekday$minute <- as.numeric(Weekday$minute)
    Weekday        <- Weekday %>% group_by(minute,Substation) %>% 
                      summarise(avg=mean(avg))

    `summarise()` regrouping output by 'minute' (override with `.groups` argument)

    Saturday <- New_sub %>% filter(weekday=='Saturday')%>%
               gather(key='minute',value,-Substation,-Date,-weekday) %>%
               group_by(minute,Substation,Date,weekday) %>% 
               summarise(avg = mean(value))

    `summarise()` regrouping output by 'minute', 'Substation', 'Date' (override with `.groups` argument)

    Saturday$minute <- as.numeric(Saturday$minute)
    Saturday        <- Saturday %>% group_by(minute,Substation) %>% 
                       summarise(avg=mean(avg))

    `summarise()` regrouping output by 'minute' (override with `.groups` argument)

    Sunday <- New_sub %>% filter(weekday=='Sunday')%>%
                gather(key='minute',value,-Substation,-Date,-weekday) %>%
                group_by(minute,Substation,Date,weekday) %>% 
                summarise(avg = mean(value))

    `summarise()` regrouping output by 'minute', 'Substation', 'Date' (override with `.groups` argument)

    Sunday$minute <- as.numeric(Sunday$minute)
    Sunday        <- Sunday %>% group_by(minute,Substation) %>% 
                    summarise(avg=mean(avg))

    `summarise()` regrouping output by 'minute' (override with `.groups` argument)

    ggplot()+
      geom_line(data=all_days,aes(x=minute,y=avg,col='all_days'))+
      geom_line(data=Weekday,aes(x=minute,y=avg,col='weekdays'))+
      geom_line(data=Saturday,aes(x=minute,y=avg,col='saturday'))+
      geom_line(data=Sunday,aes(x=minute,y=avg,col='sunday'))+
      labs(title='Daily average power by substation',
           x='Minute interval',y='Power KW')+
      theme(legend.title = element_blank(),
            legend.position='bottom')+
      facet_wrap(~Substation)

![](/clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-17-1.png)

1.  Using k-means (or other version, i.e. based on medians), allocate
    these new substations to one of your clusters.

<!-- -->

    # Create a Daily Max column 
    New_sub[,'DailyMax'] <- apply(New_sub[,3:146],1,max)
    # Scale the real power by dividing each value by 'Daily Max'
    New_sub_s <- New_sub[3:146]/New_sub$DailyMax
    # Add Substation column to scaled power inetervals
    New_sub_s<- cbind(Substation=New_sub$Substation,New_sub_s)

    # Summarise means for every interval by substations
    New_sub_s <- New_sub_s %>% 
                 group_by(Substation) %>%
                 summarise_all("mean")

    # Create function to claculate distances of scaled values to centers of clusters
    clusters <- function(x, centers) {
      # compute squared euclidean distance from each sample to each cluster center
      tmp <- sapply(seq_len(nrow(x)),
                    function(i) apply(centers, 1,
                                      function(v) sum((x[i, ]-v)^2)))
      max.col(-t(tmp))  # find index of min distance
    }


    # Find the cluster centres for Autumn dataset 
    centers <- res %>% select(-c(weekday,Station,Date)) 
    centers1 <- aggregate(. ~Class, centers, mean)

    # Use function to calculate distances 
    cl <- clusters(New_sub_s[,-1], centers1[,-1])
    # Assing cluster membership to new substations 
    res1 <- cbind(New_sub_s, Class=factor(unname(cl)))

    # Produce table of new substations and their cluster memebership 
    out <- res1 %>% select(Substation,Class)
    kable(out)

<table>
<thead>
<tr class="header">
<th style="text-align: right;">Substation</th>
<th style="text-align: left;">Class</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: right;">511079</td>
<td style="text-align: left;">2</td>
</tr>
<tr class="even">
<td style="text-align: right;">512457</td>
<td style="text-align: left;">3</td>
</tr>
<tr class="odd">
<td style="text-align: right;">532697</td>
<td style="text-align: left;">2</td>
</tr>
<tr class="even">
<td style="text-align: right;">552863</td>
<td style="text-align: left;">2</td>
</tr>
<tr class="odd">
<td style="text-align: right;">563729</td>
<td style="text-align: left;">2</td>
</tr>
</tbody>
</table>

1.  Based on your summaries and plots, is the cluster allocation as you
    expected?

Cluster allocation is as expected, substations that have been clustered
into cluster 2, follow a pattern that is closer to the domestic power
demand, with the difference that they have variable power magnitudes.
Substation 512457 has been assigned to cluster 3, the ‘Commercial’
cluster which closely aligns with the power demand profile in this
cluster, but on a smaller power scale.

Exploring differences between seasons
=====================================

The aim of this report is to perform analysis on power data recordings
at substations for the five-following season: Autumn, Winter, Spring,
Summer, High Summer.

The objectives of the report include

• Identifying differences in power demand between seasons •
Investigating cluster groupings of substations between seasons

First part of the analysis will be done by performing clustering by
k-means, for the Autumn data set, then attaching the cluster membership
to the substation IDs. On the assumption that the cluster memberships
are to be constant throughout the five seasons, the cluster memberships
can be mapped onto the other seasons for the same substations. This way
the clusters can be compared with seasonal patterns. To determine the
number of clusters, the changes in ‘the intra-cluster variation’ can be
looked at by plotting these on a graph for several clusters (in this
case a maximum of 20). Where the curve indicates a ‘point of the elbow
or a hockey stick’ that is where there is no additional information gain
from including more clusters. Figure 1 shows a plot of
‘intra-cluster-variation’ for 20 clusters. Here it is identified to use
seven clusters for the initial analysis.

    Welcome! Want to learn more? See two factoextra-related books at https://goo.gl/ve3WBa

![](/clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-19-1.png)

Performing k-means clustering with seven clusters on the Autumn dataset,
and then assigning these clusters to substations IDs, it is then
possible to assign these exact same cluster memberships to the other
four seasons. Figure 2 shows the seven clusters and the scaled power
demand for the five seasons. Figure 3 shows these clusters against the
real power demand in KW.

![](/clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-21-1.png)

![](/clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-23-1.png)

Looking at the plots the clusters can be described in the following way:

**Cluster 1 – Industrial** Serves predominantly industrial customers.
Load shape is constantly high throughout the day and night, and the
seasonal variation is hardly noticeable, except in winter the demand is
slightly higher.

**Cluster 2 – Domestic ** Mostly serves domestic customers, where the
load is low and flat during daytime, and peaks during evening hours. A
noticeable increase in power demand in the winter season, and a slight
peak in the autumn, mostly due to rising use of heating.

**Cluster 3 – Economy 7** Stations that serve domestic customers who are
on an Economy 7 plan. This cluster describes a load pattern where
customers benefit from a lower electricity rate during the night hours,
hence the peak during 00:00-08:00. This peak increases by a great amount
in wintertime, when heating is used. And then slightly less so in
Springtime (possible during the transition phase of season changes,
where it still might have been cold in the beginning of spring).

**Cluster 4 – Street/Motorway lightin** Power load is high and constant
at night hours, and very low during day hours. These patterns can be
found in substations serving motorways and street lighting. From the
graphs it is observed that during seasons with less day light hours
(winter and autumn) the power demand is at full utilisation for longer
periods of time, since it is darker for longer. In summer and spring,
the load profile becomes wider, and the streetlights can be on for less
hours.

**Cluster 5 – Commercial** Commercial dominated stations, where the
demand is high and flat during day hours, and lower overnight. Seasonal
trends are as expected, with a slight peak in demand during winter
months, and lower during summer months.

**Cluster 6 – Domestic & Commercial** Cluster 6 serves a mixture of
domestic and commercial customers. During daytime the power is
relatively flat, similar to cluster 5, and then slightly peaks during
evening/late hours due to domestic customers, similar to cluster 2. Here
again the anticipated seasonal changes are seen, with demand peaking
during winter.

**Cluster 7 – Rural Domestic**  
Cluster 7 looks to have stations that serve mostly a mixture of domestic
and commercial customers in rural areas, where the demand is much lower
compared to the other clusters. The load profile is relatively flat
through the day hours (commercial users) and peaks slightly around 18:00
(domestic users). There is another peak during midnight, perhaps due to
heating demands in rural reals. This peak is amplified when comparing
the non-scaled power measurements, where in winter the demand is much
higher.

In the second part of the analysis it is assumed that the cluster
memberships are not constant in relation to clusters produced by the
Autumn dataset. By calculating the distances of data points for the
remaining four seasons to the cluster centres of the Autumn clusters, it
can be seen how many stations have remained in the same cluster thorough
out the seasons and how many have changed cluster memberships. Table 1
showcases the five seasons, and their respective cluster memberships in
the columns. The last column ‘n’ counts how many substations have stayed
throughout the cluster membership configuration in each row. For
example, viewing row one, 64 stations have stayed in cluster 6 through
all five seasons. Viewing row four, it is seen that 43 stations started
off in cluster 2 for Autumn and Spring and later changed to cluster 6
for the reaming seasons. This is expected as cluster 2 and 6 are very
similar in their domestic load profile. There are many rows below the
fourth one, that have stations changing from cluster 2 to 6 and
vise-versa. Overall it is observed that most stations do not change
their cluster membership, hence Figure 2 with the seasonal plots can be
used as a good estimate for seasonal power demand changes.

<table>
<caption>Clusters by Season</caption>
<thead>
<tr class="header">
<th style="text-align: right;">Autumn</th>
<th style="text-align: right;">Winter</th>
<th style="text-align: right;">Spring</th>
<th style="text-align: right;">Summer</th>
<th style="text-align: right;">HSummer</th>
<th style="text-align: right;">n</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: right;">6</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">64</td>
</tr>
<tr class="even">
<td style="text-align: right;">2</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">52</td>
</tr>
<tr class="odd">
<td style="text-align: right;">5</td>
<td style="text-align: right;">5</td>
<td style="text-align: right;">5</td>
<td style="text-align: right;">5</td>
<td style="text-align: right;">5</td>
<td style="text-align: right;">47</td>
</tr>
<tr class="even">
<td style="text-align: right;">2</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">43</td>
</tr>
<tr class="odd">
<td style="text-align: right;">1</td>
<td style="text-align: right;">1</td>
<td style="text-align: right;">1</td>
<td style="text-align: right;">1</td>
<td style="text-align: right;">1</td>
<td style="text-align: right;">26</td>
</tr>
<tr class="even">
<td style="text-align: right;">2</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">21</td>
</tr>
<tr class="odd">
<td style="text-align: right;">3</td>
<td style="text-align: right;">3</td>
<td style="text-align: right;">3</td>
<td style="text-align: right;">3</td>
<td style="text-align: right;">3</td>
<td style="text-align: right;">11</td>
</tr>
<tr class="even">
<td style="text-align: right;">2</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">9</td>
</tr>
<tr class="odd">
<td style="text-align: right;">6</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">9</td>
</tr>
<tr class="even">
<td style="text-align: right;">2</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">2</td>
<td style="text-align: right;">7</td>
</tr>
<tr class="odd">
<td style="text-align: right;">2</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">6</td>
<td style="text-align: right;">7</td>
</tr>
<tr class="even">
<td style="text-align: right;">7</td>
<td style="text-align: right;">7</td>
<td style="text-align: right;">7</td>
<td style="text-align: right;">7</td>
<td style="text-align: right;">7</td>
<td style="text-align: right;">7</td>
</tr>
</tbody>
</table>

Table 2 lists cluster memberships and the count of substation in each
cluster for every season. As in the previous table it is observed that
stations in cluster 2 and 6 fall and rise in their counts throughout the
seasons, as stations change membership between these 2 clusters due to
their similar load profile. Cluster 1 has on average similar number of
stations for all seasons, except high summer where a few stations
(commercial) may be operating for longer hours, making their load shape
similar to that of an industrial profile.

    ## New names:
    ## * n -> n...2
    ## * n -> n...4
    ## * n -> n...6
    ## * n -> n...8
    ## * n -> n...10

<table>
<caption>Station Count by Season</caption>
<thead>
<tr class="header">
<th style="text-align: right;">cluster</th>
<th style="text-align: right;">Autumn</th>
<th style="text-align: right;">Winter</th>
<th style="text-align: right;">Spring</th>
<th style="text-align: right;">Summer</th>
<th style="text-align: right;">HSummer</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: right;">1</td>
<td style="text-align: right;">30</td>
<td style="text-align: right;">36</td>
<td style="text-align: right;">38</td>
<td style="text-align: right;">38</td>
<td style="text-align: right;">47</td>
</tr>
<tr class="even">
<td style="text-align: right;">2</td>
<td style="text-align: right;">163</td>
<td style="text-align: right;">160</td>
<td style="text-align: right;">90</td>
<td style="text-align: right;">78</td>
<td style="text-align: right;">77</td>
</tr>
<tr class="odd">
<td style="text-align: right;">3</td>
<td style="text-align: right;">21</td>
<td style="text-align: right;">26</td>
<td style="text-align: right;">33</td>
<td style="text-align: right;">19</td>
<td style="text-align: right;">18</td>
</tr>
<tr class="even">
<td style="text-align: right;">4</td>
<td style="text-align: right;">3</td>
<td style="text-align: right;">3</td>
<td style="text-align: right;">3</td>
<td style="text-align: right;">3</td>
<td style="text-align: right;">3</td>
</tr>
<tr class="odd">
<td style="text-align: right;">5</td>
<td style="text-align: right;">66</td>
<td style="text-align: right;">56</td>
<td style="text-align: right;">66</td>
<td style="text-align: right;">66</td>
<td style="text-align: right;">55</td>
</tr>
<tr class="even">
<td style="text-align: right;">6</td>
<td style="text-align: right;">96</td>
<td style="text-align: right;">101</td>
<td style="text-align: right;">148</td>
<td style="text-align: right;">170</td>
<td style="text-align: right;">176</td>
</tr>
<tr class="odd">
<td style="text-align: right;">7</td>
<td style="text-align: right;">11</td>
<td style="text-align: right;">8</td>
<td style="text-align: right;">12</td>
<td style="text-align: right;">16</td>
<td style="text-align: right;">14</td>
</tr>
</tbody>
</table>

Overall, with the exception of interchanging stations between clusters 2
and 6, on average the cluster memberships are observed to be stable with
changes in seasons. Performing a clustering analysis based on one of the
seasons initially, and then mapping the cluster memberships of each
station, to the remaining seasons, produces seasonal demand patterns,
that are on average accurate to rely on. And an increase in power demand
during winter months is observed in all clusters.

APPENDIX R CODE
===============

    library(tidyverse)
    library(dplyr)
    library(ggplot2)
    library(chron)
    library(knitr)
    library(factoextra)
    library(cluster)

    # Load power demand data for all seasons 
    Aut  <- get(load('Autumn_2012.RData'))
    Wint <- get(load('Winter_2012.RData'))
    Spri <- get(load('Spring_2013.RData'))
    Summ <- get(load('Summer_2012.RData'))
    HSumm <- get(load('HighSummer_2012.RData'))

    # Change date format 
    Aut$Date <- dates(Aut[,2], origin = c(month = 1,day = 1,year = 1970))
    Wint$Date <- dates(Wint[,2], origin = c(month = 1,day = 1,year = 1970))
    Spri$Date <- dates(Spri[,2], origin = c(month = 1,day = 1,year = 1970))
    Summ$Date <- dates(Summ[,2], origin = c(month = 1,day = 1,year = 1970))
    HSumm$Date <- dates(HSumm[,2], origin = c(month = 1,day = 1,year = 1970))

    # Calculate means for every minute interval in Autumn dataset 
    Aut1 <-  Aut[,1:146] %>% select(-c(Date)) %>% group_by(Station) %>% summarise_all(funs(mean))

    # --- Assuming clusters are the same as in Autumn
    set.seed(1234)

    # Perform k=means clustering on scaled Autumn measurments with 7 clusters 
    kmeans_Aut <- kmeans(Aut1[,-1], centers=7,nstart=25)
    # Output plot of wws vs nnumber of clusters
    fviz_nbclust(Aut1, kmeans, method = "wss", k.max=20)

    #---- sacled plot 

    # Merge cluster membership to station ID for every season 
    Autumn <- merge(Aut,res,by='Station')
    Winter <- merge(Wint,res,by='Station')
    Spring <- merge(Spri,res,by='Station')
    Summer <- merge(Summ,res,by='Station')
    HSummer <- merge(HSumm,res,by='Station')

    # Remove non_scaled measurements 
    Autumn[,147:291] <- NULL 
    # Create subset for the mean of minute intervals 
    Aut_avg <-  Autumn %>% gather(key='minute',value,-Date,-Class,-Station)%>%
      group_by(minute,Class)%>%
      summarise(avg=mean(value))
    Aut_avg$minute <- as.numeric(Aut_avg$minute)

    Winter[,147:291] <- NULL
    Wint_avg <-  Winter %>% gather(key='minute',value,-Date,-Class,-Station)%>%
      group_by(minute,Class)%>%
      summarise(avg=mean(value))
    Wint_avg$minute <- as.numeric(Wint_avg$minute)

    Spring[,147:291] <- NULL
    Spri_avg <-  Spring %>% gather(key='minute',value,-Date,-Class,-Station)%>%
      group_by(minute,Class)%>%
      summarise(avg=mean(value))
    Spri_avg$minute <- as.numeric(Spri_avg$minute)

    Summer[,147:291] <- NULL
    Summ_avg <-  Summer %>% gather(key='minute',value,-Date,-Class,-Station)%>%
      group_by(minute,Class)%>%
      summarise(avg=mean(value))
    Summ_avg$minute <- as.numeric(Summ_avg$minute)

    HSummer[,147:291] <- NULL
    HSumm_avg <-  HSummer %>% gather(key='minute',value,-Date,-Class,-Station)%>%
      group_by(minute,Class)%>%
      summarise(avg=mean(value))
    HSumm_avg$minute <- as.numeric(HSumm_avg$minute)

    # Produce plot of scaled minute interval measutments for every season facetted by cluster 
    scaled <- ggplot(Aut_avg,aes(x=minute,y=avg))+
      geom_line(mapping=aes(colour='autumn'))+
      geom_line(Wint_avg,mapping=aes(colour='winter'))+
      geom_line(Spri_avg,mapping=aes(colour='spring'))+
      geom_line(Summ_avg,mapping=aes(colour='summer'))+
      geom_line(HSumm_avg,mapping=aes(colour='high summer'))+
      labs(title='Scaled demand by cluster for each season',
           x='Time',
           y='Power')+
      scale_x_continuous(breaks=c(1,24,48,72,96,120,144),
                         labels=c("00:00","04:00","08:00","12:00",
                                  "16:00","20:00","23:50")) +
      theme(legend.title = element_blank(),legend.position='bottom')+
      facet_wrap(~Class)

    scaled

    # --- Non-scaled 

    # Reassing all seasons with cluster memberships 
    Autumn <- merge(Aut,res,by='Station')
    Winter <- merge(Wint,res,by='Station')
    Spring <- merge(Spri,res,by='Station')
    Summer <- merge(Summ,res,by='Station')
    HSummer <- merge(HSumm,res,by='Station')

    # Remove scaled measurements 
    Autumn[,3:147] <- NULL
    # Create subset for the mean of minute intervals 
    Aut_avg <-  Autumn %>% gather(key='minute',value,-Date,-Class,-Station)%>%
      group_by(minute,Class)%>%
      summarise(avg=mean(value))
    Aut_avg$minute <- as.numeric(Aut_avg$minute)


    Winter[,3:147] <- NULL
    Wint_avg <-  Winter %>% gather(key='minute',value,-Date,-Class,-Station)%>%
      group_by(minute,Class)%>%
      summarise(avg=mean(value))
    Wint_avg$minute <- as.numeric(Wint_avg$minute)

    Spring[,3:147] <- NULL
    Spri_avg <-  Spring %>% gather(key='minute',value,-Date,-Class,-Station)%>%
      group_by(minute,Class)%>%
      summarise(avg=mean(value))
    Spri_avg$minute <- as.numeric(Spri_avg$minute)

    Summer[,3:147] <- NULL
    Summ_avg <-  Summer %>% gather(key='minute',value,-Date,-Class,-Station)%>%
      group_by(minute,Class)%>%
      summarise(avg=mean(value))
    Summ_avg$minute <- as.numeric(Summ_avg$minute)

    HSummer[,3:147] <- NULL
    HSumm_avg <-  HSummer %>% gather(key='minute',value,-Date,-Class,-Station)%>%
      group_by(minute,Class)%>%
      summarise(avg=mean(value))
    HSumm_avg$minute <- as.numeric(HSumm_avg$minute)

    # Produce plot of non scaled minute interval measurements for all seasons facetted by cluster 
    non_scaled <- ggplot(Aut_avg,aes(x=minute,y=avg))+
                  geom_line(mapping=aes(colour='autumn'))+
                  geom_line(Wint_avg,mapping=aes(colour='winter'))+
                  geom_line(Spri_avg,mapping=aes(colour='spring'))+
                  geom_line(Summ_avg,mapping=aes(colour='summer'))+
                  geom_line(HSumm_avg,mapping=aes(colour='high summer'))+
                  labs(title='Real demand by cluster for each season',
                       x='Time',
                       y='Power KW')+
                  scale_x_continuous(breaks=c(1,24,48,72,96,120,144),
                                     labels=c("00:00","04:00","08:00","12:00",
                                              "16:00","20:00","23:50")) +
                  theme(legend.title = element_blank(),legend.position='bottom')+
                  facet_wrap(~Class)

    non_scaled

    # ----  Part two, if clusters change by season

    # Bind the cluster memberships to the Autumn dataset
    res <- cbind(Aut1, Class=factor(unname(kmeans_Aut$cluster))) 

    # Create function to calculate distances of new data to the cluster centers
    clusters <- function(x, centers) {
      # compute squared euclidean distance from each sample to each cluster center
      tmp <- sapply(seq_len(nrow(x)),
                    function(i) apply(centers, 1,
                                      function(v) sum((x[i, ]-v)^2)))
      max.col(-t(tmp))  # find index of min distance
    }


    # Find the cluster centres for clusters in Autumn data
    centers <- kmeans_Aut$centers

    # Calculate means for each substation by minute interval for each season
    Wint_s  <- Wint[1:146]
    Wint_s  <- Wint_s %>% group_by(Station) %>% summarise_all('mean')
    Spri_s  <- Spri[1:146]
    Spri_s  <- Spri_s %>% group_by(Station) %>% summarise_all('mean')
    Summ_s  <- Summ[1:146]
    Summ_s  <- Summ_s %>% group_by(Station) %>% summarise_all('mean')
    HSumm_s <- HSumm[1:146]
    HSumm_s <- HSumm_s %>% group_by(Station) %>% summarise_all('mean')


    ## Note, below code is computationally heavy, please allow some time

    # Using 'clusters' function, compute clusters for new data in every season 
    cl   <- clusters(Wint_s[,3:146], centers)
    res1 <- cbind(Wint_s, Class=factor(unname(cl)))
    c2   <- clusters(Spri_s[,3:146], centers)
    res2 <- cbind(Spri_s, Class=factor(unname(c2)))
    c3   <- clusters(Summ_s[,3:146], centers)
    res3 <- cbind(Summ_s, Class=factor(unname(c3)))
    c4   <- clusters(HSumm_s[,3:146], centers)
    res4 <- cbind(HSumm_s, Class=factor(unname(c4)))

    # Select Station and Class(cluster membership) for each season
    Aut_stat   <- res %>% select(Station,Class)
    Wint_stat  <- res1 %>% select(Station,Class)
    Spri_stat  <- res2 %>% select(Station,Class)
    Summ_stat  <- res3 %>% select(Station,Class)
    HSumm_stat <- res4 %>% select(Station,Class)

    # Rename 'Class' column to respective season name 
    colnames(Aut_stat)[2] <- 'Autumn'
    colnames(Wint_stat)[2] <- 'Winter'
    colnames(Spri_stat)[2] <- 'Spring'
    colnames(Summ_stat)[2] <- 'Summer'
    colnames(HSumm_stat)[2] <- 'HSummer'

    # Merge all seasons into one dataframe 
    df <- merge(Aut_stat,Wint_stat, by='Station')
    df <- merge(df,Spri_stat,by='Station')
    df <- merge(df,Summ_stat,by='Station')
    df <- merge(df,HSumm_stat,by='Station')

    # Write to csv (to not perform above computationaly high operations)
    write.csv(df,file='cluster memberships.csv')
    # Read in the above data 
    df <- read.csv('cluster memberships.csv')
    # Remove first column
    df$X <- NULL

    # Create dataframe that compares cluster membership between seasons 
    ddf <- df %>% group_by(Autumn,Winter,Spring,Summer,HSummer) %>% count()
    kable(head(ddf[order(ddf$n, decreasing=TRUE),],12))

    # Subset cluster counts for each season
    cnt1 <- df %>% group_by(Autumn) %>% count() 
    cnt2 <- df %>% group_by(Winter) %>% count() 
    cnt3 <- df %>% group_by(Spring) %>% count() 
    cnt4 <- df %>% group_by(Summer) %>% count() 
    cnt5 <- df %>% group_by(HSummer) %>% count() 

    # Combine above subsets
    count <- cbind(cnt1,cnt2,cnt3,cnt4,cnt5)
    # Select every other column(duplicates)
    odd_ind <- seq(3,10,2)
    # rRemove duplicate columns 
    count[,odd_ind] <- NULL
    # Rename columns 
    colnames(count) <- c('cluster','Autumn','Winter','Spring','Summer','HSummer')
    # Output table with station counts per cluster for every season 
    kable(count)
