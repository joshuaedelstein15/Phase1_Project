# Microsoft_Movie_Analysis
Analysis of which films are doing the best in box office  

## Introduction
Microsoft has decided to create a new movie studio, and has tasked us  with exploring what types of films are currently doing the best at the box office. We will analyze the market based on a few different metrics and present microsoft with our findings as well as offer insights on the current market.

## Objectives
We will analyze the market from different angles:
- First, we will examine the correlation between different metrics.
- Next, we will analyze the numbers, and which genres are making the most money.
- Then, we will take a look at which genre is making the greatest percent profit.
- We will then identify if the length of movie has any affect on its ratings.
- Next, we will move on the see overall ratings by genre.
- Then, we will determine if there is a relationship between ratings and profit.
- Finally, we will offer overall recommendations.

<div>
<img src="Images/movie.jpg", width = 600, height = 300/>
</div>

Photo by <a href="https://unsplash.com/photos/q8P8YoR6erg">GR stocks</a> on <a href="/@grstocks">Unsplash</a>

### Data Understanding
The data we are using is from two sources. Firstly, we are using a sql dataset from IMDB which contains non-monetary information for over 70,000 movies. The next dataset is a much smaller csv file containing budgeting information regarding just under 6000 movies.

### Business Understanding

We will be analyzing success based on gross earnings, production budget, and ratings.

The overall business questions we are trying to answer are:

1. What is the influence of budget on overall profit?
2. What are the more profitable genres?
3. What is the distribution of movie runtimes, and how does runtime affect ratings?
4. What is the relationship between ratings and genre?
5. What is the relationship between profit and ratings?

## 1. Loading the data
We begin by importing all the necessary packages to do the research.

### IMDB statistics

We then load the required database from IMDB. This contains basic non-monetary information about different movies.

### Movie Budget

Next, we load a much smaller dataframe which contains a csv file with budgeting information.

## 2. Joining the databases

To analyze the budgeting data by genre we need to join the 2 databases together.

A problem may arise if we join the databases by title alone, as there may be a few movies with the same name that will incorrectly join with the other database. To avoid this issue we will join the databases by 2 different columns. Since the odds of two movies being released in the same year with the same name are very low, we will also join by release year. 

First, we need to create new column in the budget dataframe that contains just the year that the movie was released without the full date.

The join will be much smoother if the columns we are joining on have the same name. To accomplish this, we change the column name in the imdb dataframe, from `original_title` to `movie`.

Finally, we can perform the join using both the `movie` column and the `start_year` column, to create a new dataframe called `money_genre_df`.

The new dataframe contains far fewer values then both of the original dataframes. However, this is not an issue as the new dataframes is a random sample of the original dataframes with no particular bias as to how the movies made it into the new dataframe.

Although, we assumed that there were no movies with the same name released in the same year, we should still double check to make sure there are no duplicate values.

## 3. Editing the data

In order to analyze the monetary data we will need to use the `production_budget`, `domestic_gross`, and `worldwide_gross` columns. Upon taking a closer look we realize that these columns are strings instead of the integer values that we want.

### Fixing the values

To change the values from string to integer:

1. We need to strip the value of the `$` sign
2. We need remove all the commas from within the strings
3. Finally, we can convert the values from string to integers

## 4. Data Analysis

Besides for the raw amount of money that each movie grossed it is useful to know what percent profit the movie gained in comparison to how much they put in. We accomplish this by creating a new column `gross_vs_budget` which divides the `worldwide_gross` by the `production_budget`. This will show us the profit multiple/ how many times greater a movies profit was then its budget.

Additionally, we don't want to include movies that had a very small budget as they can skew our data. As such, we will only include movies with a budget of atleast 1 million dollars.

### Monetary analysis
First, we will compare the relationship between each movies gross profit and the budget. We will do this by calculating the correlation coefficient as well as plotting the relationship on a scatter plot.

The correlation between the 2 values is above .7, this means there is a strong positive correlation between gross profit and production.<!-- https://journals.lww.com/anesthesia-analgesia/fulltext/2018/05000/correlation_coefficients__appropriate_use_and.50.aspx
 -->. The scatterplot illustrates this point as we see the best fit line as an increasing slope. This is useful information as it shows that the more money that is spent on the film the greater the profit. 
 
Although, this was helpful, it will be more useful to figure out the relationship between the budget and percent gain. This value will show us, from a business perspective, whether one should invest a lot into a movie in hopes of exponentially multiplying their investment.

Since the correlation coefficient is below .1 there is negligible correlation between these factors. This shows that from an investors perspective, if interested in multiplying your investment, the size of your budget will not play a major role. 

We also see some of the movies, with smaller budgets multiplied their investment substantially more times than the movies with the higher budgets. On the other hand we see that the lower budget movies had more times where the movie ended up with a loss on their investment.

When we look at the higher budget movies we find that there are virtually no movies that multiplied their investment by more than 10. However, there are also much fewer movies that had a loss on their investment.

What comes out, is that lower budget movies are higher risk and higher reward. As there is a greater chance of making a tremendous profit, but also a greater chance of losing money. While, making a movie with a higher budget is a much safer investment. In that the profit will probably be a small multiple of the amount that one invests in the budget.

### Monetary Analysis by genre

Now let's break up all the films into genres to see how profit differs between genres. 

We will begin by checking out all the values in the `genre` column.

Clearly it will not be helpful to do analysis on each of the 220 individual genres. Additionally, there are some categories with only one movie with that specific genre. As a result, we cannot learn anything from it, as it is much too small of a sample size. 

Due to this issue, we will run a sql query which breaks the genres up into five main genre dataframes: `Action`, `Comedy`, `Drama`, `Horror`, and `Romance`. We will then use those to create new dataframes which contain just the gross profit for each genre.

We should now run a check to make sure these loaded properly.

Now we will create a histogram of gross profit for each genre. To accomplish this more efficiently we will create three different lists. The first will contain a text version of each genre, this will used to label the x-axis. The second will contain each of the five new dataframes containing the gross profit. The third will contain five different colors, which will be used to differentiate between the histograms. Finally, we can create a for loop to plot the five different histograms. Additionally, each histogram will contain a vertical line at the mean of each dataset.

Interestingly, all the histograms roughly have the same shape, this shape seems to indicate that profit falls into the Pareto distribution.<!-- https://www.youtube.com/watch?v=EAynHZE-lK4 --> Meaning that the minority of movies account for the vast majority of the profit. Additionally, it appears that the genres of action and comedy are the most profitable, while drama, horror, and romance are less profitable.

However, these results may be a function of how much on average each genre is spending on their movies. To investigate this, let's find the average gross vs profit for each genre.

As we predicted the budget for action movies is much greater on average than for other genres. As such the average profit multiple for action movies is the lowest of all the genres. Another significant finding from our data is that although horror movies average gross was standard compared to the other genres, their average budget was far lower than the other genres. As such their profit multiple is at least double all the other values. This demonstrates that when looking for the best investment, it pays to create a lower budget horror movie.

### Non-monetary Analysis
We will now move on and look into non monetary statistics. We begin by loading basic info about the imdb_db

We see that both `runtime_minutes` and `genre` are missing some values. We will have to clean this data before we are able to pull useful information from it.

### Data Cleaning
Let's begin by taking all the movies that are missing their genre and replacing the NaN value with "Absent." This is the most logical way to solve this issue as it won't affect any of the other genres. 
We will then run a check to make sure the values were indeed changed.

In terms of the values in the `runtime_minutes` column, it wouldn't make sense to create new values for each row randomly selected based on the current distribution of data, as this would create false information for each specific movie. Instead we will drop all of the rows that contain NaN values. Although this seems like a lot of rows we are still left with around 90% of our original data. 
Again we will run a check to make sure it ran properly.

Additionally, there may be some outlier values that were have a large impact on the mean as well as other aggregate functions. As such lets only include data that is within two standard deviations of the mean. Then we will run a check to make sure the new maximum and minimum seem to be realistic numbers.

### Data Analysis
Now, let's plot the distribution of the runtimes to see if there is any useful information.

The data seems to be very normal, as the median and mean are only 2 minutes apart. Overall nothing seems to stand out here.

Now let's check if there is any correlation between the length of the movie and its overall rating, and graph the distribution on a scatter plot.

With a value of -.02 the relationship is basically non existant. This is illustrated in the graph, which looks similar to a rain cloud in that there seems to be no recognizable place to put a best fit line

We will move on and break all the movies up by our five main genres. Then we will select only the average rating column from each row.

Now we will plot average rating by genre. Similar to the last time we plotted 5 histograms, we will create a for loop for more efficient code. We will use the `colors` and `txt_genres` list that we created last time. Additionally we will need to create a new list called `genre_rating` which contains the rating dataframes broken up by genre.

All the histograms have a relatively normal distribution with a slight skew to the left. This shows that most movies are above the mean but there are some values on the low end that pull the mean down and make it lower. 

We also see that in terms of ratings, drama is the highest rated genre, followed by romance. This makes sense as people are looking for feel-good movies and these two genres accomplish that the best. The next ranked genres are comedy and action, these genres are more of a raw form of entertainment, if people want to laugh or see something cool. Finally, we have horror with the lowest average ranking. Although, people choose to go to horror movies, they don't walk out feeling like they had a real positive experience. The films are made to give people a quick scare without walking away with anything real.

The last thing we should take a look at is the relationship between ratings and gross, to see if there is any correlation between these two. If there is, then it would show us that if one aims to make a movie with higher ratings than a large profit will come along with that.

We see that there is a weak relationship, meaning that when making a movie one would have to decide which factor matters more to them.

## 5. Summary and Recommendations

Overall, we found some very significant results from our monetary analysis of the movie dataframes. 

1. We found that for the most part, the more money one puts into a movie the greater their profit will be. However, this isn't exponential, meaning that the profit multiple on your budget will roughly be the same whether you make a more or less expensive movie. Therefore, if stuck with a lower budget the average percent gain on investment will roughly be the same as if one invested more money into the budget.
2. We found that in a sense the lower budget movies are a riskier investment, as there is more opportunity to have a greater percent gain. At the same time there is greater opportunity for loss. While the higher budget movies had a more stable average percent gain. If one is looking for a riskier investment with high upside they should make a lower budget movie. While if they want a more guaranteed gain on investment it pays to create a higher budget movie. 
3. We found that the distribution of gross profit is subject to the Pareto principle. Meaning that a vast minority of movies are making most of the industries profit. 
4. If one is looking to make the largest gross amount of money the best investment on average is to create a high budget action movie. However, if one is looking for the best investment on average, or if stuck with a lower budget, the best option is to create a horror movie as this genre had over double the percent gain on ones investment.

We then moved on, and looked at non-monetary metrics for success, and found some interesting results.

1. We found that there is basically no correlation between ratings and runtime. Therefore, when making a film, one shouldn't focus on the length of the movie but rather the quality.
2. We found that on average drama and romance are the highest rated genres, followed by comedy and action, with horror in last. As such, if one wants to create a highly rated film, they should make a feel-good drama/ romantic movie. 
3. Lastly, we found that there is very little correlation between gross profit and ratings. This is a meaningful finding, as it teaches us that when focused on making a movie, one has to choose what their metric for success is before making the movie. If they want to make a large profit they will have to focus on very different things then if they were trying to make a movie with high ratings.