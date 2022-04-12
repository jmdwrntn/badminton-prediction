# Badminton Predictions

## Description

A supervised machine learning approach is proposed that uses in-game statistics to predict the outcome of badminton matches, which has hitherto offered little in this regard when compared to other sports. Methods are defined to extract eight features of interest from the publicly available standard format match results. Logistic regression and K-nearest neighbour algorithms are applied to these features from 14,722 professional level matches played between 2018 and 2021, which results in between 76 and 80% match outcome prediction accuracy.

## Feature selection

When considering feature selection and creation, the first step was to identify what features were required to attempt to answer the problem statement, and the second to determine whether these features could be extracted from the existing features.

Many top-level players and coaches profess the importance of starting strong or building a cushion of points between ‘you’ and your opponent as the key to winning games and the overall match. These factors are interesting to consider, as they directly relate to physically scoring points and trying to win a match, though they are equally as important to mentality.

The features of interest concern starting a game strong, reaching the mid-game interval first, and achieving and maintaining a positive point difference throughout each game. By extracting these features, we can analyse whether this methodology to winning a match with psychology and mentality is evidenced in data. The desired features are:

* Consecutive points scored
* Point difference at the end of a game
* Point difference at the mid-game interval

During feature creation the decision was made to ‘forget’ about team two and treat each record as a match only from team one’s perspective. As each record contains details about a single match but contextually contains two points of view of the match, it was challenging to compare features to the target feature (match outcome), as features were having to be compared against each other for each team.

By only focusing on one team, each record can now be directly compared to the target feature of a win or a loss, rather than which team won. This enabled the analysis to measure the new features directly to see whether, for example, a team scored enough consecutive points to win that game.

### Consecutive points
Fortunately, consecutive points are already in the dataset as an integer value for each team in each game. As the analysis is now only concerning team one, this feature now contains team one’s most consecutive points for each game.

### Point difference at the end of a game
The final score for each game is a string value in the format 21-19 for each game. Two new features were created for each game, with the final score for each team, by splitting the string value by the hyphen. To find the relative point difference, a new feature was created for each game by dividing the losing team’s points by the winning team’s points.

### Point difference at the mid-game interval
The dataset contains a feature for each game which contains a list of string values in the same format as above that records each point scored in the match. This feature therefore contains the score at the mid- game interval and which team got there first. A function was defined to iterate over the feature and return the string representation of the score at the first instance of 11 appearing. This was then split into interval scores for each team in each game and the lower points were divided by the higher points to find the relative point difference at the mid-game interval, in the same manner as above.

### Dimensionality reduction
Following the creation of the desired features, the dataset contained a significant number of redundant columns. While some would provide value in future expansion of the model, such as discipline and player names, for the purpose of this approach all original features were removed from the dataset, resulting in nine total features. One feature that was created was also dropped, which was the point difference at the end of game three. This feature seemed too directly related to winning, as at the point this feature is realised the match is over. While these does exist to a degree for game two, there are still around a third of all matches being played reaching a third game, so it made sense to keep point difference at the end of game two as a feature.

### Data normalisation

Due to the dynamics of a badminton match and tournaments, the newly created features contained a deal of variance in their ranges. In the early rounds of a tournament, higher ranked players can beat lower ranked players by significant margins, which may produce very high point differences. Consequently, the decision was made to normalise the data as if algorithms that utilise distances were used this would improve the performance and accuracy. The scikit-learn library minmaxscaler was utilised for normalisation. This tool analyses each feature in the dataset, identifies the min and max values, and scales against these so every value in the feature is between 0 and 1.

