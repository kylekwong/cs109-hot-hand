---
layout: default
---
# Hot Hand Analysis
{: style="text-align: center"}

## How the Hot Hand would Look
A statistical model of the Hot Hand might look something like 

![Logistic Function](https://github.com/kylekwong/cs109-hot-hand/blob/master/visuals/positive%20logit%20model.png?raw=true)

Depicted above is a logistic regression classifier that predicts a binary outcome (1 or 0 or in the case of the Hot Hand, shot made or missed) given some predictor. In the case of the Hot Hand, that predictor (the variable along the x axis) is the shooter’s previous streak. If the logistic regression predicts that the probability of making a shot increases as the player’s streak increases, this indicates the player has the Hot Hand.

## Methods
We extract publicly available shot data from the 2013-14 NBA season containing information on the game, player, and other factors in which the shot occurred. From this data, we are able to construct a previous streak metric which will serve as our primary predictor in our analysis. The previous streak (`PreviousStreak` and `previous_streak`, used interchangeably) metric is equal to the number of shots made in a row (or missed in a row) prior to the shot in question. For consecutive misses, the value of the previous streak variable is negative.

We then fit a logistic regression model:

![model equation](https://github.com/kylekwong/cs109-hot-hand/blob/master/visuals/equation.png?raw=true)

where X<sub>s</sub> represents a vector of shot information including shot distance, closest defender’s distance, shooter’s overall field goal percentage, shot clock, final margin of the game, amongst other predictors. β<sub>1</sub> represents the Hot Hand effects, i.e. the increase in probability of making a shot for each addition to a player’s streak.

## Results

### NBA-Wide

#### Single Predictor Model
The baseline logistic regression model on the entire dataset returns a very small coefficient and one that is insignificant with a large standard error as you can see in the below output.

![Logit Model with 1 Predictor](https://github.com/kylekwong/cs109-hot-hand/blob/master/visuals/SpecificLogitModel.JPG?raw=true)

The above figures demonstrate how difficult it is to build a useful model with only the `previous_streak` variable. The image displays the logit model and how it would predict whether a shot was made or not depending on the current value of `previous_streak`. If `previous_streak` had a strong correlation with whether the shot was made or not, then the line would have a more dramatic slope. If anything, the line has a negative slope contrary to our expectations about the Hot Hand effect. This model is only able to correctly classify **54.6%** of a test set of shots, which is barely better than flipping a coin. 

{: style="text-align: center"}
![Streak Distribution](https://github.com/kylekwong/cs109-hot-hand/blob/master/website%20reports/streak_distributions.png?raw=true)

The histogram figures displays the distribution of the `previous_streak` variable depending on whether the shot was missed or made. The distributions appear to be almost identical whether the shot is missed or made with the quantity of shots missed being greater than the amount made at about each value of `previous_streak`.

If `previous_streak` were more likely to be associated with a made shot, then we would see the made distribution more shifted to the right and the missed distribution more shifted to the left in the above figure, but this is obviously not the case and is what is driving the above results in the baseline model.

#### A more robust model
A model based solely on `previous_streak` doesn't take into account a variety of factors that may affect the shot, such as difficulty of the shot or the quality of defense against the shooter. In order to improve our model, we wanted to control for these factors by including them as predictors in our logistic regression model. 

**Improving the Classification Rate**
{: style="text-align: center"}

![Logit Model as number of predictors increases](https://github.com/kylekwong/cs109-hot-hand/blob/master/visuals/class%20rate%20versus%20predictors.png?raw=true)

As we add more predictors, the classification rate improves with the biggest jump due to the addition of the shot distance variable. The predictors we decided to use are listed along the x-axis of the figure above. 

Adding in more predictors improves both the model's accuracy as well as the significance of the `previous_streak` variable (regression results displayed below). This model implies that for an increase in streak of one unit, this only affects the probability of making the next shot by about or less than 1%. Furthermore, this effect on probability is a decrease, i.e. the exact opposite of what we expect to see if the Hot Hand were true. If anything, a shooter on a streak becomes less likely to make their next shot. The p-value of the `previous_streak` coefficient is also  < 0.05, showing us that this slight negative effect is significant. These results remain similar if we restrict the `previous_streak` variable to just positive values. The regression results show us `fg_percent` is the greatest predictor of shot probability, which is expected. Overall these regression results imply that from a league-wide perspective the Hot Hand theory is a fallacy.

![robust_model](https://github.com/kylekwong/cs109-hot-hand/blob/master/website%20reports/robust_logit_results.png?raw=true)


### Player by Player
We next wanted to move our analysis from a league-wide perspective to an individual player analysis to see if there are any individuals that may display a Hot Hand. We chose to analyze players who take many shots and have a high distance variance. We did this to avoid picking players who only go for layups or players who only take three-point shots, and we chose to pick players who take more shots because players with more shots have a higher chance of being on a streak. 

![Shots Taken vs. Distance Variance](https://github.com/kylekwong/cs109-hot-hand/blob/master/visuals/STDvsShots.JPG?raw=true)

In order to generate this player score, we calculated two statistics for each player; their average number of shots per game and the standard deviation of their shot distance. We then scaled these values so that they equaled a representative value between 0 and 1. Finally, we added the two scaled values to combine them into one score to represent each player. In the top right corner of the graph above, we can see plots of the players that we will examine (i.e. the players with the highest values in both categories).

![Top 20 Players](https://github.com/kylekwong/cs109-hot-hand/blob/master/visuals/Top20Players%20Model%20Values.JPG?raw=true)

The top 20 players are listed above. We fit our robust Hot Hand model to each and list the model parameters and performance.

**The Hot Hand effect varies for our top 20 players.** If we fit models for individual players, the Hot Hand effect varies in both magnitude and sign. There is about an equal distribution of negative and positive estimated coefficients which resonates with our finding that the streak metric is generally an estimated zero when the model is run on the whole dataset. These are visualized in the graph below.

**Important Note**: As we evaluate the model on a large number of players, we have to keep in mind the issue of Multiple Comparisons. This idea says that as we're comparing p-values across a large number of independent samples, there is a high chance of at least one having a p-value < 0.05 due to probability distribution alone and not necessarily due to significance. We have to adjust what our p-value cutoff for significance is. In this case, multiple comparisons across 20 samples says our proper p-value will be 0.05/20 = 0.00256.

Unfortunately, most of our `previous_streak` coefficient estimates have p-values greater than .05 or .1 indicating that our estimates are not significant. <!-- Interestingly enough though is that the only player to have a significant estimate is Steph Curry (likely because he takes a large amount of shots). Steph's Hot Hand effect is estimated to be a negative coefficient though that implies as Steph makes an additional shot in a streak, his probability of making the next shot decreases by about 8% (which is pretty significant in magnitude). The player with the largest positive Hot Hand effect is Derrick Rose whose probability of making the next shot increases by about 5% for each additional shot he makes in a streak. --> We find nobody significant below the 0.00256 threshold. 

![20 Players Analysis](https://github.com/kylekwong/cs109-hot-hand/blob/master/website%20reports/player_coefficients.png?raw=true)

These results show us that none of our estimates are significant. With these findings, we don't find any support for the Hot Hand theory among our Top 20 players, further indicating it is a fallacy. In the future, it may be useful to have data on players from multiple seasons and with additional observations, we may gain greater statistical precision.


#### Testing Binary Heat
In order to further test the Hot Hand theory, we decided to run our regression using binary-encoded variables for streak instead of the `previous_streak` variable. We believed that this would examine the possibility that when a player is hot, it doesn't matter "how hot" he is (i.e. how long his streak is), but rather simply if he is hot or not. 

Three different binary variables were tested. "Hot 2" was an indicator of whether or not that player's previous streak was greater than or equal to 2. "Hot 3" was an indicator of whether or not the previous streak was greater than or equal to 3, and "Hot 4" was an indicator of whether or not the previous streak was greater than or equal to 4.

![Binary Heat Table](https://github.com/kylekwong/cs109-hot-hand/blob/master/visuals/binary_heat.jpg?raw=true)

The results of this regression can be found in the table above. The columns for each variable name contain the coefficient calculated by our regression, and the column next to each variable (labeled p-Value#) is the corresponding p-value to the coefficient in the column to its left. Once again, due to multiple comparisons and a lower threshold for statistical significance, none of the players were found to have a statistically significant coefficient on any of the binary streak methods, further proving that the hot-hand is indeed dead.

![Binary Bars](https://github.com/kylekwong/cs109-hot-hand/blob/master/website%20reports/binarybars.png?raw=true)


## Conclusions
Despite fitting individual logistic regression models on our Top 20 players, and testing for both binary and non-binary streaks, we found that `previous_streak` lacked any strong predictive value. No models found `previous_streak` to be a significant factor in a shot’s probability. It appears therefore that the Hot Hand theory exists for neither individuals nor the league as a whole, suggesting that the Hot Hand is indeed a fallacy.
