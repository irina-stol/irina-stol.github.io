
---
layout: clustering post
title: "This is a test post for my R blog"
date: 2020-09-24
categories: rblogging
tags: test ggplot2
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
