# Regression Model to Predict Over/Under Outcomes for the Upcoming NFL Season

## Goal
Develop a model that can recommend to bettors whether teams are likely to go over or under the set line

## ETL 
- To train our regression model, we used a dataset with every NFL game since 1979, with features including betting lines, game outcomes, weather conditions, and more. 
- We needed to engineer a lot of new features to augment the data we started with. First, since our data did not include the results of any betting line (i.e. whether the home team pushed), we needed to add new columns to identify whether the over/under line was exceeded, pushed, or above the game's total score. To add more context to the difference in quality between teams, we went through the dataset and calculated each team's current record and point differential at the time of their game.
- Since 16 game seasons are an awfully small sample size, record can be misleading, so using the point differential we obtained, we calculated each team's Adjusted Pythagorean Expected Win Percentage at the time of each matchup, using Football Outsiders formula. This provided a more accurate look at the true strength of each team, using point differential to calculate what their expected win percentage should be. 
- Since total points scored in a game is less reliant on the relative strength of each team and more dependent solely on offensive and defensive performance, we calculated for each team their average points scored and allowed per game at the time of each matchup. 
- While we did not have injury data, which would have clued our model in on when a team's actual performace would be worse than its expected performance, we were able to calculate a rolling win percentage of each team's last four games, which provided a more accurate glimpse of how good or bad the team currently was, rather then their season-to-date performance. 

## Feature Selection

To narrow down our features, we explored the correlations each had with the Over/Under line, as well as with each other so that we could limit multicolinearity.

## Model 


## Clustering the Data


## Week One Predictions


