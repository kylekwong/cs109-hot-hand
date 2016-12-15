---
layout: default
---
# Hot Hand: Fact or Fallacy?
{: style="text-align: center"}

![curry](https://github.com/kylekwong/cs109-hot-hand/blob/master/visuals/curry.jpg?raw=true)

The Hot Hand theory is the widespread belief that a person who has experienced success with a seemingly random event has a greater chance of further success in additional attempts. In the context of basketball, this is the belief that as a player continuously makes successful, sequential shots (i.e. when they’re on a hot streak), their next shot has a higher chance of success than if they were not on a streak. Most studies in the past have shown that the Hot Hand theory is fallacious [1]; however, a recent study has provided some convincing evidence that the Hot Hand theory might not be so far-fetched after all [2]. Our goal in this project was to provide our own analysis of NBA data to determine if the Hot Hand theory was a fact or a fallacy. 

After a thorough analysis of various factors using logistic regression, we found that previous streak had no effect on shot probability, meaning that a player’s hotness does not actually improve their performance. We conclude that from our study, the Hot Hand appears to be a fallacy. We also explored the Clutch Factor, another belief in basketball where certain players improve their performance in important situations. Following a similar procedure with logistic regression, we found that how clutch a given shot is has no effect on shot probability, suggesting that none of the players we studied showed evidence of the Clutch Factor. Overall, our work disproved both the Hot Hand and Clutch Factor theories, showing us that public perception does not match our statistical findings.


## References
1. Gilovich, Thomas et al. “The Hot Hand in Basketball: On the Misperception of Random Sequences.” Cognitive Psychology, vol. 17, no. 3, 1985, pp. 295–314. 
2. Bocskocsky, Andrew et al. “The Hot Hand: A New Approach to an Old ‘Fallacy.’” MIT Sloan Analytics Conference, 28 Feb. 2014.
