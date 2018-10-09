# Regression Model to Predict Over/Under Outcomes for the Upcoming NFL Season

## Goal
Develop a model that can recommend to bettors whether teams are likely to go over or under the set line

## ETL 
- To train our regression model, we used a dataset with every NFL game since 1979, with features including betting lines, game outcomes, weather conditions, and more. 
- We needed to engineer a lot of new features to augment the data we started with. First, since our data did not include the results of any betting line (i.e. whether the home team pushed), we needed to add new columns to identify whether the over/under line was exceeded, pushed, or above the game's total score. To add more context to the difference in quality between teams, we went through the dataset and calculated each team's current record and point differential at the time of their game.
- Since 16 game seasons are an awfully small sample size, record can be misleading, so using the point differential we obtained, we calculated each team's Adjusted Pythagorean Expected Win Percentage at the time of each matchup, using Football Outsiders formula. This provided a more accurate look at the true strength of each team, using point differential to calculate what their expected win percentage should be. 
- Since total points scored in a game is less reliant on the relative strength of each team and more dependent solely on offensive and defensive performance, we calculated for each team their average points scored and allowed per game at the time of each matchup. 
- While we did not have injury data, which would have clued our model in on when a team's actual performace would be worse than its expected performance, we were able to calculate a rolling win percentage of each team's last four games, which provided a more accurate glimpse of how good or bad the team currently was, rather then their season-to-date performance. 

## Model and Feature Selection

To narrow down our features, we explored the correlations each had with the Over/Under line, as well as with each other so that we could limit multicolinearity. We ultimately decided on using 7 features: points per game, points allowed per game, temperature, wind, dome (binary 1 or 0), and season (to account for changes between eras). 

Correlation heatmap

<img src="https://github.com/slieb74/NFL-Betting-Data/blob/master/images/Screen%20Shot%202018-10-08%20at%204.05.44%20PM.png" width='350' height='350'>

Regression for Points per Game

<img src="https://github.com/slieb74/NFL-Betting-Data/blob/master/images/Screen%20Shot%202018-10-09%20at%2012.45.15%20PM.png" width='400' height='350'> 

For our model, we tested out linear, log-linear, and log-log regression models, settling on a linear regression which fit out data best. Due to the odd distribution of NFL scores (most scoring plays are either 3 or 7 points), we used the BoxCox Power Transformation on each of our variables to transform them into a more normal distribution. Our final regression model had an Adjusted R^2 of 0.697

Boxcox Transformation for Points per Game

<img src="https://github.com/slieb74/NFL-Betting-Data/blob/master/images/Screen%20Shot%202018-10-09%20at%2012.44.49%20PM.png" width='850' height='500'>

## Clustering the Data
In order to train a more accurate regression model, we experimented with clustering our data, classifying games as being one of four types: good offense vs. good defense, good offense vs. bad defense, bad offense vs. bad defense, and bad offense vs. good defense. Our hypothesis was that by clustering the games into these four categories, and regressing on each individually, we'd be able to predict Over/Under lines with even higher accuracy. However, after testing this on 4 separate regression models, we found that each were in fact less accurate than the overall regression model, since having a good offense far outweighed having a strong defense.

<img src="https://github.com/slieb74/NFL-Betting-Data/blob/master/images/Screen%20Shot%202018-10-09%20at%2012.45.36%20PM.png" width='400' height='400'>

## Week One Predictions
After running our regressions, we plugged in the data from Week 1 of the 2018 season and predicted for each game what the Over/Under line should be. If our line was higher than the actual line, we recommended better the over, and vice versa for the under. Using Naive Bayes, we then calculated the probability of a game going over or under, given our predictions and the past history of games with the same line.

<img src="https://github.com/slieb74/NFL-Betting-Data/blob/master/week1predictions.png" width='600' height='500'>

For Week 1, we correctly predicted 9 of 16 games. We also ran every prior game through our model to see how accurate it would be if we had bet on every single game since 1979 (Weeks 5-16). Our model gave good predictions 54.79% of the time, classified as when our model guided the bettor to a win or push.

<img src="https://github.com/slieb74/NFL-Betting-Data/blob/master/images/Screen%20Shot%202018-10-09%20at%201.01.10%20PM.png" width='300' height='300'>
