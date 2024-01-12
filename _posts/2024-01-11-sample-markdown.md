---
layout: post
title: How can we evaluate and assign value to NFL tackles?
subtitle: My submission to the 2024 NFL Big Data Bowl 
thumbnail-img: /assets/img/tackle.jpg
gh-repo: BenjaminMillman/2024_BigDataBowl
gh-badge: [star, fork, follow]
tags: [NFL, Football, Statistics, ML]
comments: true
author: Ben Millman
---

{: .box-success}
This project was created as part of a Kaggle competition whose expiration has passed. All work was completed by myself using data from the 2022 season provided by the NFL in a Kaggle notebook running R. This was an awesome opportunity to learn and combine my passions as an athlete and data analytics student. I hope you enjoy it!

## Introduction
Football can be complex at times, but when it's all boiled down, there's 2 goals. The offense wants to move the ball forward, and the defense wants to stop them. Defenses do this through tackling, and that was the theme of this year's competition.  The task at the outset was general - create metrics that assign value to tackling. As someone who has played football my entire life, this seemed simple at first as we sort of intuitively assign value to tackles as we watch, but creating math-driven, objective assessments is much more difficult. I split my approach and focussed on two avenues. First, I wanted to calculate the probability of a player making a tackle as a function of their distance to the ball. Second, I wanted to predict the location of each tackle. With each of these I could generate further metrics. 

## Data Cleaning
Before I could start on any of that, I had to prepare the data. The NFL provided multiple tables including the following: a games table which contained basic information about each game and its final score, a players table which contained information about each player and their background, a plays table which detailed the state of the game before and after the play and contained information about the play itself, a tackles table which contained information about every tackle tackle that occurred, and lastly, 9 weeks of tracking information containing the football's and every player's positional information of every frame of every play. Each of these also contained various ID columns which could be used to join them. 

For each of these tables I made sure that columns read in as characters that needed to be factors were appropriately converted, and made sure all dates and times were read in correctly or converted to the same format. Next, I combined all of the tracking weeks into one large table. 

## Tackle Probability
The first thing I wanted to do now that the data was clean was calculate each player's tackle probability by distance. To start, I assigned each play a 1 if the player made a tackle and a 0 otherwise. So for every frame in a play where a player made the tackle, that player would have a 1. Then I calculated every player's distance to the ball on every frame of every play. With these two combined we have a Bernoulli distribution for each player where tackles varie as a function of distance. I then created a logistic regression to model this relationship where Y is the log of a tackle and the predictors are distance to the ball and the interaction between distance to the ball and a player's name. The resulting curves are pictured below. 

*Note: The positive slopes and flat lines are data issues and/or players with few opportunities. These were removed for future analysis.*
![Tackle Curves](https://raw.githubusercontent.com/BenjaminMillman/2024_BigDataBowl/main/Screen%20Shot%202024-01-08%20at%202.39.22%20PM.png)
This isn't super helpful, but it is good to see the results of the model. The players with the least steep slopes and highest Y intercept are the best by our metrics. 

### Tackle Range
My next goal was to quantify a player's tackle range. Using the above model I could predict each player's tackle probability at each whole number distance from the ball. Using this startegy, I first found the top players' tackle probability at 0 yards, this is pictured below. 
![Top Tacklers at 0](https://raw.githubusercontent.com/BenjaminMillman/2024_BigDataBowl/main/top15at0.jpg)
These represent the most consistent players in successfully tackling a player when they're close to them.
Next,  I defined tackle range as the distance at which a player is more likely to go on to make than miss a tackle. ie P(Tackle) > .5. Below are the top players by my tackle range metric. 
![Top Tackle Range](https://raw.githubusercontent.com/BenjaminMillman/2024_BigDataBowl/main/range_plot.jpg)
As shown above, once Geno Stone crosses 13 yards, he is more likely to make than miss the tackle. 

## Yards Saved
Next I wanted to find a way to quantify the value of tackle. This initially seems simple, but as with most of this project it was not. I started by creating a neural network to predict the locations of tackles. This involved a lot of data manipulation to create the correct structure. I then split the data into 70% training and 30% testing. This data contains complex chronological patterns and the best type of model to capture these is a Recurrent Neural Network. My model takes in the positional data of all players on each frame, and outputs a location for the tackle at the end of the play. Due to time and resource restrictions, I couldn't fully optimize the model, I did get it to a place that is acceptable. Below are the results of the model where blue is the actual location and red is the prediction. 
![Model Results](https://raw.githubusercontent.com/BenjaminMillman/2024_BigDataBowl/main/Screen%20Shot%202024-01-08%20at%202.57.46%20AM.png)

Then I used this model to estimate yards saved. To do this, I masked the player who made the tackle on each play and re-predicted the tackle locations; the distance between the new and old predictions is my estimate of yards saved. This approach does a good job of capturing the essence of yards saved, but it does also unfortunately capture the effect of the defense having one less player, this means my model most likely overestimates the value of a tackle. Below are an animation of one of the most valuable tackles, and the top players by average yards saved. 
![MVT](https://raw.githubusercontent.com/BenjaminMillman/2024_BigDataBowl/main/MVT2.gif)
![Top Yards Saved](https://raw.githubusercontent.com/BenjaminMillman/2024_BigDataBowl/main/top_yards_saved.jpg)

## Conclusion 
This project was a great opportunity to combine things I'm passionate about. I learned a lot and challenged myself by doing it all in R. If you'd like to see more click my [Kaggle Notebook](https://www.kaggle.com/code/bmailman/a-statistical-analysis-on-tackling-in-r) this has a more technical write up of the project. The linked github repo also contains all the code. Thanks for reading. I hope it was insightful!


