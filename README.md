# Microsoft_Movie_Analysis
Analysis of which films are doing the best in box office  

# Movie Analysis

## Introduction
Microsoft has decided to create a new movie studio, and has tasked us  with exploring what types of films are currently doing the best at the box office. We will analyze the market based on a few different metrics and present microsoft with our findings as well as offer insights on the current market.

## Objectives
We will analyze the market from different angles:
- First, we will analyze the numbers, and which genres are making the largest gross profit.
- Next, we will take a look at which genre is making the greatest percent profit.
- Then, we will identify if the length of movie has any affect on its ratings.
- We will then move on the see overall ratings by genre
- Lastly, we will offer overall recommendations.

<div>
<img src="images/movie.jpg", width = 600, height = 300/>
</div>

Photo by <a href="https://unsplash.com/photos/q8P8YoR6erg">GR stocks</a> on <a href="/@grstocks">Unsplash</a>

### Data Understanding
The data we are using is from two sources. Firstly, we are using a sql dataset from imdb which contains non-monetary information for over 70,000 movies. The next dataset is a much smaller csv file containing budgeting information regarding many movies.

### Business Understanding

We will be analyzing success based on gross earnings as well as ratings.

The overall business questions we are trying to answer are:

1. What is the influence of budget on overall profit?
2. What are the more profitable genres?
3. What is the distribution of movie runtimes, and how does runtime affect ratings?
4. What is the relationship between ratings and genre?

## 1. Loading the data
We begin by importing all the necessary packages to do the research.

import sqlite3
import math
import pandas as pd
import datetime as dt
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
from pandasql import sqldf
pysqldf = lambda q: sqldf(q, globals())

### IMDB statistics

We then load the required database from IMDB. This contains basic non-monetary information about different movies.

### Movie Budget

Next, we load a much smaller dataframe which contains csv file with budgeting information.

## 2. Joining the databases

To see the budgeting data by genre we need to join the 2 databases together.

A problem may arise if we join the databases by title alone, as there may be a few movies with the same name, that will incorrectly join with the other database. To avoid this issue we will join the databases by 2 different columns. Since the odds of two movies being released in the same year with the same name are very low, we will also join by release year. 

First, we need to create new column in the budget dataframe that contains just the year that the movie was released without the full date.

The join will be much smoother if the columns we are joining on have the same name. To accomplish this, we change the column name in the imdb dataframe, from `original_title` to `movie`.

Finally, we can perform the join using both the `movie` column and the `start_year` column, to create a new dataframe named `money_genre_df`.

The new dataframe contains far fewer values then both of the original dataframes. However, this is not an issue as the new dataframes is a random sample of the original dataframes with no particular bias as to how the movies made it into the new dataframe.

Although, we assumed that there were no movies with the same name released in the same year, we should still double check to make sure there are no duplicate values.

## 3. Editing the data

To start analyzing the monetary data we need the `production_budget`, `domestic_gross`, and `worldwide_gross` columns. Upon taking a closer look we realize that these rows are strings instead of the integer values that we want.

### Fixing the values

To change the values from string to integer:

1. We need to strip the value of the `$` sign
2. We need remove all the commas from within the strings
3. Finally, we can convert the values from string to integers

## 4. Data Analysis

Besides for the raw amount of money that each movie grossed it is useful to know what percent profit the movie gained over how much they put in. We accomplish this by creating a new column `gross_vs_budget` which divides the `worldwide_gross` by the `production_budget`.

Additionally, we don't want to include movies that had a very small budget as they can skew our data. As such, we will only include movies with a budget of atleast 1 million dollars.

### Monetary analysis
First, we will compare the relationship between each movies gross profit and the budget, by calculating the correlation coefficient as well as plotting out the points. 

The correlation between the 2 values is above .7, this means there is a strong positive correlation btwn gross profit and production.<!-- https://journals.lww.com/anesthesia-analgesia/fulltext/2018/05000/correlation_coefficients__appropriate_use_and.50.aspx
 -->. The scatterplot illustrates this point as we see the best fit line as an increasing slope. This is useful information as it shows that the more money that is spent on the film the greater the profit. 
 
Although, this was helpful, it will be more useful to figure out the relationship between the budget and percent gain. This value will show us from a business perspective whether one should invest a lot into a movie in hopes of exponentially multiplying their investment.

Since the correlation coefficient is below .1 there is negligible correlation between these factors. This shows that from an investors perspective, if interested in multiplying your investment, the size of your budget will not play a major role. 

We also see some of the movies, with smaller budgets multiplied their investment substantially more times then the movies with the higher budgets. On the other hand we see that the lower budget movies had more times where the movie ended up with a loss on their investment.

When we look at the higher budget movies we find that their are virtually no movies that multiplied their investment by more then 10 times their investment. However, there are also much fewer movies that had a loss on their investment.

What comes out is that, lower budget movies are higher risk and higher reward, as there is a greater chance of making a tremendous profit, but also a greater chance of losing money. While, making a movie with a higher budget is a much safer investment in that the profit will probably be a small multiple of the amount that one invests in the budget.

### Monetary Analysis by genre

Now lets break up all the films into genres to see how profit differs between genres. 

We will begin by checking out all the values in the `genre` column.

Clearly it will not be helpful to do analysis on each of the 220 indivual genres. Additionally, there are some categories with only one movie with that specific genre. As a result, we cannot learn anything from it, as it is much to small of a sample size. 

Due to this issue, we will run a sql query which breaks the genres up into five main genre dataframes: `Action`, `Comedy`, `Drama`, `Horror`, and `Romance`. We will then use those to create new dataframes which contain just the gross profit for each genre.

We will then run a check to make sure these loaded properly.

Now we will create a histogram of gross profit for each genre. To accomplish this more efficiently we will create three different lists. The first will contain a text version of each genre, this will used to label the x-axis. The second will contain each of the five new dataframes containing the gross profit. The third will contain five different colors, which will be used to different between the histograms. Finally, we can create a for loop to plot the five different histograms. Additionally, each histogram will contain a vertical line at the mean of each dataset.

Interestingly, all the histograms roughly have the same shape, this shape seems to indicate that profit falls into the pareto distribution.<!-- https://www.youtube.com/watch?v=EAynHZE-lK4 --> Meaning that the minority of movies account for the vast majority of the profit. Additionally, it appears that the genres of action and comedy are the most profitable, while drama, horror, and comedy are less profitable.

However, these results may be a function of how much on average each genre is spending on their movies. To investigate this, lets find the average gross vs profit for each genre.

As we predicted the budget for action movies is much greater on average then for other genres as such the average profit multiple for action movies is the lowest of all the genres. Another significant finding from our data is that although horror movies average gross was standard compared to the other genres, their average budget was far lower then the other genres. As such there profit multiple is atleast double all the other values. This demonstrates that when looking for the best investment, it pays to create a lower budget horror movie.


## 5. Findings

Overall, we found some very significant results from our monetary analysis of the movie dataframes. 

1. We found that for the most part, the money one puts into a movie the greater their profit will be. However, this isn't exponential, meaning that on the percent gain on your budget will roughly be the same whether you make a more or less expensive movie. 
2. We found that in a sense the lower budget movies are a riskier investment, as there is more opportunity to have a greater percent gain. At the same time there is greater opportunity for loss. While the higher budget movies had a more stable average percent gain.
3. We found that the distribution of gross profit is subject to the pareto principle.
4. If one is looking to make the largest gross amount of money the best investment on average is to create a high budget action movie. However, if one is looking for the best investment on average, or if stuck with a lower budget, the best option is to create a horror movie as this genre had over double the percent gain on ones investment.
