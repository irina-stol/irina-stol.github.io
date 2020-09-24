---
layout: post
title: "Clustering power demand"
date: 2020-09-24
categories: rblogging
tags: ggplot2 R clustering 
---
This post will anayse electricity power demand across South Wales and perform a clustering analysis on the substations in the datasets. 

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

<table style="width:60%;">
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

<img src="/img/2020-09-24-clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-3-1.png" style="display: block; margin: auto;" />

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

![](/img/2020-09-24-clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-4-1.png)

    # Plot for Percentage of industrial and commercial customers 
    ggplot(Char, aes(x=Percentage_IC, fill=TRANSFORMER_TYPE))+
      geom_histogram(bins=25)+
      labs(title='Percentage of idustrial and commercial customers by transformer type')+
      theme(legend.position='bottom')

![](/img/2020-09-24-clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-5-1.png)

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

![](/img/2020-09-24-clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-6-1.png)

    ggplot(Char, aes(x=TOTAL_CUSTOMERS, fill=Transformer_RATING))+
      geom_histogram(bins=30)+
      labs(title='Total customers by transformer rating',
           x='Total customers')+
      theme(legend.position='bottom')

![](/img/2020-09-24-clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-7-1.png)

    ggplot(Char, aes(x=as.factor(LV_FEEDER_COUNT), fill=TRANSFORMER_TYPE))+
      geom_bar(stat='count')+
      labs(title='LV Feeder counts by transformer type',
           x='LV Feeder Count')+
      theme(legend.position='bottom')

![](/img/2020-09-24-clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-8-1.png)

    ggplot(Char, aes(x=as.factor(LV_FEEDER_COUNT), fill=Transformer_RATING))+
      geom_bar(stat='count')+
      labs(title='LV Feeder counts by transformer rating',x='LV Feeder Count')+
      theme(legend.position='bottom')

![](/img/2020-09-24-clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-9-1.png)

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

![](/img/2020-09-24-clustering-power-demand_files/figure-markdown_strict/unnamed-chunk-11-1.png)
4. Choose an appropriate number of clusters and label each substation
according t
