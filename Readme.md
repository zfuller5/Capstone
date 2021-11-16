# DraftKings NFL Predictor
##### -Using Data Modeling to predict NFL player performance

### Description
---
DraftKings (DK) is a widely popular gaming website for daily fantasy sports. Fantasy football traditionally is a season long game where groups of 8 to 14 people form a league of members who then draft their respected team before the season starts. Teams consist of Quarterbacks (QB), Running Back (RB), Wide Receiver (WR), Tight End (TE), Defense (DEF), and Kicker (K) and several bench spots to store players. Throughout the season each league member with navigate injured players, bye weeks, and performance to start/play the best possible lineup given the choice of players in their lineup, their bench, and the free agent pool. With daily fantasy football, you are only creating a lineup of these players for one week's games, so the commitment is for only week or every week. It also allows people to create a lineup using all players in the league and not just players on your team which is a limitation of season long fantasy sports.

DraftKings started out competing with other sights namely FanDuel. However, to start the 2020 NFL season DraftKings inked a deal with ESPN to be their co-exclusive daily fantasy sports provider. This has tilted the favor in their hands as to which sight is more popular and widely used. One of DraftKings most popular game is the Millionaire Maker which awards a prize of one million dollars to the winner. This contest covers only the NFL games which start at 12pm CST and afternoon games starting in the 3pm CST hour. The game is capped at around 200k lineups to which a single user can submit up to 150 lineups at \\$20 per lineup. Each user has \\$50,000 to spend on the following positions QB, RB, RB, WR, WR, WR, TE, FLEX (any RB, WR, or TE), and DST (defense and special teams). Each player and defense has a salary associated with them valued by season long performance. Salaries are updated each week based on weekly trends.

The goal of this project is to predict which players will perform the best in week 10 of the NFL season and draft a lineup that would win the Millionaire Maker contest. I will explore several regression modeling techniques to predict a DK point score for each player position. The players with this highest predicted DK points will be used for the lineup while considering player salaries. 


### What was the starting point?
---
The amount of NFL data that is available is astonishing. The NFL generated \\$12 billion in revenue last year between TV contracts, merchandising, and local revenue, while the American sports betting industry is said to generate \\$150 billion annually. This sheds some light on why there are seemingly endless amounts of data and websites touting their predictive powers. To shield this project from too much influence, I stuck with Sports Reference's available data even signing up for their advance data package. I also collected and pulled in DK's weekly salary and results data.

I started out analyzing the null model which would be the season long DK point averages that team defenses allow to each respective position. If we were to simply create a lineup off the null model, we would explore the top five points allowed per position. Since the competition only uses the afternoon games, we automatically lose Thursday night, Sunday morning (occasional games in London), Sunday night, and Monday night games. This is typically 3 matchups (6 teams) a week that we will not be able to use players on. There will also be teams on a bye which can be up to 6 teams on bye for a total of up to 12 teams that will not be considered. For week 10, we see 4 teams on bye plus the 6 teams already excluded. Therefore, we have only 22 teams to consider players from. The other thing to note is point differential between teams. For QBs the top 3-5 team defenses giving up the most points all average in the 22-point range and are only 4 points off from the top. However, for RB we see a greater than 9-point difference between the top team and the 2nd team. For TE the top 5 only differ by less than 3 points. Lastly, for WR we see a large drop from 1st to 2nd but similar range from 2nd to 5th. Again, the idea here with the null model is that you draft the player with the best matchup, so RB against NYJ and WR against TEN are the must plays. There are more options with QB and TE since the range is tighter, though playing QB against WAS would satisfy this logic.

|QB|RB|TE|WR|
|---|---|---|---|
|WAS - 26.8|NYJ - 40.6|PHI - 19.6|TEN - 48.0| 
|KAN - 24.0|DET - 31.2|BAL - 19.2|WAS - 43.8|
|IND - 22.9|SFO - 31.0|LAC - 17.1|MIA - 43.7|
|DAL - 22.6|CIN - 29.3|LAR - 17.1|IND - 43.1|
|MIA - 22.1|PHI - 29.3|IND - 16.9|MIN - 42.6|

As an experienced season long fantasy football player and recent DK player, I know first-hand that using the null model does not consistently produce the highest player output. In fact, it rarely does, which makes these kinds of competitions hard to predict, challenging, and lots of fun. There are many factors that go into week in and week out parity. The best teams in the league are still liable to have a bad game, the worst teams in the league are bound to upset a successful team, and everything in between. The reality is that despite all the data available, it is nearly impossible to predict a top player waking up on the wrong side of the bed and having a bad game when a great matchup existed and was expected. 

The other data sets that were considered were various offensive and defensive standard and advanced data. For offensive, I looked at offensive stats encompassing yardage and efficiency gaining yardage by passing and rushing; red zone efficiency by category (pass, rushing, receiving); conversion efficiency on 3rd and 4th downs in addition to red zone; drive efficiency based on plays, scoring and time of possession; and scoring metrics by type of scoring. I also looked at defensive stats encompassing the same metrics. Lastly, I considered game by game metrics by team, position, and player. Out of 52 datasets in my library 40 were used for analysis and 12 were used for week 10 predictions. Of the 52 datasets only 36 were used for EDA and data cleaning, while only 21 were actually used for modeling.


### What did the data show?
---
All data was cleaned in a similar manner by implementing common feature names for allowing datasets to be merged and combined. Null values were replaced with zeros as empty values are equivalent to zero statistics. Special characters were removed or converted into meaningful values. The three most important features added were converting an unnamed column to 'opp_home' which indicates were the game was played, creating a column 'slot' which indicated which time slot a game was played, and by dissecting the 'result' column (presented as W 41-14 for example) into team W/L, team score, and opponent score. As I began modeling the game data, I found that my results were nearly perfect. I was able to accurately predict the amount of passing points and rushing points scored for a given matchup. Then, I realized that to predict the amount of points a player will score in week 10 I will only be able to use features for the week 10 matchups that are known before the games are played. This severely limits my feature set to the teams playing in the game, the location of the game, and time slot the game is played. Other conditions such as win/loss record going into the game, team coming off a bye week, team going into a bye week, time zone difference (west coast to east coast travel), and weather are a few features that could be used. As such, the data is only set up for teams playing and the location of the game on the positional data sets. I will have the points scored per position as my target variable, but I will only be able to use teams playing and location to predict positional score for week 10's matchups. This limited feature set severely crippled my model performance. 

I explored Linear Regression, Ridge and LASSO regularization, K Nearest Neighbors, Random Forest, and AdaBoost while tuning most of these models. The models scored quite badly for all models and all player positions. Unfortunately, there was just not enough flexibility in the data available.


### How close did we actually get?
---
Running predictions against the actual results for week 10 was probably the most satisfying part of the project. As bad as the R2 scores were, the predictions didn't seem to match up with how bad the models scored. The predictions for the most part predicted average results. I would have anticipated not being able to predict anything of note, but instead I can see some trends. I put together tables for each position that show the best 2 models per position. The structure of the tables is defined below:

Models Used: RidgeCV (cross-val Ridge), KNNR GS (KNN cross-val and grid searched), Random Forest, RndFor GS (Random Forest cross-val and grid searched),
Pred Opp: This is the predicted top 5 Defenses to play against for the respective position and the associated predicted DK fantasy points.
Actual Opp: This is the actual rank of the predicted defense to play against and actual DK fantasy points.



||||QB|||
|---|---|---|---|---|---|
|Null||RidgeCV||RndFor GS||
|Pred Opp|Actual Opp|Pred Opp|Actual Opp|Pred Opp|Actual Opp|
|WAS - 26.8|13th - 15.0|BAL - 18.5|8th - 18.9|BAL - 22.5|8th - 18.9| 
|KAN - 24.0|7th - 19.2|LAC - 17.4|10th - 18.5|MIN - 19.0|15th - 13.0|
|IND - 22.9|18th - 10.8|MIA - 17.0|11th - 16.4|LAC - 18.8|10th - 18.5|
|DAL - 22.6|31st - 2.7|MIN - 16.7|15th - 13.0|MIA - 17.7|11th - 16.4|
|MIA - 22.1|10th - 16.4|SEA - 16.6|18th - 11.5|NYJ - 17.6|3rd - 24.9|

Let's look at the Null model first. In short its predictions are average at best predicting as high as 7th but as low as 31st. For the ML models we can see that the predicted versus actual point total is fairly close for the top 5 predictions for Ridge, but not very close for Random Forest (RFR). What is also interesting is that the Ridge model did make predictions in ranking order meaning the process was off but it did linearly predict the order of actual results. Despite predicting two outcomes in the top 10, the RFR was not very good at predicting the DK points. Note: the data was adjusted to sum up the DK points of the two Miami QBs that played against BAL. Two played due to the starting QB leaving the game due to injury. Lastly, the top QB (KAN) for week 10 scored 39.2 DK points against the 27th ranked Defense (LVR) for QBs. That is to say the null model was expecting LVR to place 27th, but instead placed 1st.



||||RB|||
|---|---|---|---|---|---|
|Null||KNNR GS||Random Forest||
|Pred Opp|Actual Opp|Pred Opp|Actual Opp|Pred Opp|Actual Opp|
|NYJ - 40.6|13th - 19.0|MIN - 11.5|19th - 14.9|IND - 16.7|14th - 18.4| 
|DET - 31.2|11th - 20.3|CAR - 11.1|18th - 15.4|TEN - 15.0|9th - 20.8|
|SFO - 31.0|No data|ATL - 11.1|8th - 21.8|JAX - 14.1|3rd - 27.6|
|CIN - 29.3|No data|DET - 11.0|11th - 20.3|GNB - 13.1|39th - 5.9|
|PHI - 29.3|24th - 10.3|JAX - 10.7|3rd - 27.6|DET - 11.9|11th - 20.3|

Let's look at the Null model first. We do not have results for two of the five predictions due to not having data at the time of this post (Monday night) and a team on a bye week. It should be noted that some teams utilize multiple running backs, so comparing predicted point total for the null model should be considered all RBs against a defense. In this case we have (19 + 11.9 + 8.7 = 39.6) when consider the 3 utilized RBs against NYJ. This is mostly uncommon but it would prove the Null model correct as an RB group. For the ML models we see three predictions were outside of the top 10. We can see that the predicted versus actual points for each model was not very close. Unlike the Null model this predicted data is player data not team data. Also, the order of predictions did not even rank in a linear order as with the QB predictions. Nevertheless, both models did predict 2 of the top 10 finishers in its top 5 or you could say even 3 of the top 11 finishers in its top 5. Lastly, the top RB (KAN) for week 10 scored 32.4 DK points against the 17th ranked Defense (LVR) for RBs. That is to say the null model was expecting LVR to place 17th, but instead placed 1st.



||||TE|||
|---|---|---|---|---|---|
|Null||KNNR GS||Random Forest||
|Pred Opp|Actual Opp|Pred Opp|Actual Opp|Pred Opp|Actual Opp|
|PHI - 19.6|8th - 10.9|JAX - 7.7|20th - 6.1|ATL - 18.8|27th - 3.5| 
|BAL - 19.2|19th - 6.4|DAL - 7.5|10th - 10.0|CLE - 11.0|2nd - 19.7|
|LAC - 17.1|4th - 16.1|LVR - 7.2|1st - 22.9|TEN - 10.6|12th - 8.2|
|LAR - 17.1|No data|PHI - 7.1|8th - 10.9|MIA - 10.5|3rd - 18.3|
|IND - 16.9|6th - 13.7|WAS - 6.9|13th - 7.6|DAL - 10.3|10th - 10.0|

Let's look at the Null model first. We see that we do not have data for one prediction due to not having data at the time of this post (Monday night). It should also be noted that most teams typically utilize just one pass catching TE, but DEN had two TEs that scored 10.9 and 10.7 against the top Null model prediction (PHI). This combined DK point total would have been good for 2nd place among TEs. For the ML models we see the KNN model predict 3 of the top 10 TE performers in its top 5 rankings and accurately predict 1st place in its top 5. No other model across the player positions predicted the top scorer. The RFR model did well in predicting 2nd and 3rd place finishers in its top 5 and predicted 3 of the top 10 finishers in its top 5. The outlier for RFR being that DAL against ATL saw an uncharacteristic drop in targets to the TE position. Lastly, the top TE (KAN) for week 10 scored 22.9 DK points against the 11th ranked Defense (LVR) for TEs. That is to say the null model was expecting LVR to place 11th, but instead placed 1st. This was much closer than the other predictions for the Null model.



||||WR|||
|---|---|---|---|---|---|
|Null||KNNR GS||Random Forest||
|Pred Opp|Actual Opp|Pred Opp|Actual Opp|Pred Opp|Actual Opp|
|TEN - 48.0|18th - 14.4|DAL - 9.3|63rd - 4.2|MIN - 12.1|8th - 17.8| 
|WAS - 43.8|20th - 14.2|TEN - 9.1|18th - 14.4|LAC - 11.9|4th - 25.9|
|MIA - 43.7|21st - 14.0|MIA - 9.0|21st - 14.0|BAL - 11.7|17th - 14.6|
|IND - 43.1|22nd - 13.9|GNB - 9.0|49th - 5.6|PIT - 11.5|36th - 10.1|
|MIN - 42.6|8th - 17.8|LAC - 8.7|4th - 25.9|GNB - 10.9|49th - 5.6|

Let's look at the Null model first. We see that only one of its top 10 projections make the list, while the rest of the rankings fall in order. Predicting outcomes in order is a positive observation despite predicting 8th best below the others. For the Null model it should be noted that this is harder to compare as teams utilize 3 WRs commonly and this Null model prediction considers the sum average of all WR against the defense. For the ML models we see that the predictions for ranking are terrible until you spot predicting the 4th best WR at 5th in the predictions. The scoring difference between actual and predicted is way off as well, which falls in line with the terrible R2 scores for this position. The RFR model was way better at predicting WR outcomes predicting 2 of the top 10 and showing some linearity with its other rankings. Still, the predicted scores are far apart from actual as these two models performed the worst among the positions. Lastly, the top WR (BUF) for week 10 scored 33.2 DK points against the 27th ranked Defense (NYJ) for WRs. That is to say the null model was expecting NYJ to place 27th, but instead placed 1st. This WR scored more actual points than the defense allows to an entire team of WR for this matchup. This was as poor as a prediction as for the QB position above.


### Final Thoughts
---
The project started out with grand ideas for squeezing every ounce of information from the data collected in order to form a robust and accurate predictive model. Instead, ambition gave way to practicality and running a stripped-down feature set. For any mild success in predictions, you could also attribute that to the randomness and unpredictability of the NFL as a whole. In addition to the challenges faced and described, there are also unseen and unobservable impacts to this project that cannot be modeled. In a practical sense we cannot predict if a player will leave a game with an injury and therefore stop accumulating stats. We cannot know if a player woke up on the wrong side of the bed that morning or was distracted by family issues. Those things can play a large role and are not quantifiable.

My next steps as I continue to improve my models will be to explore implementing popular predictive metrics like 538's EOL rating which is a custom measurement of strength of matchup. I will also consider using implement my own observations about a team's play around bye weeks and how a team responds the week after experiencing a bad loss. Overall, I have created a foundation to build upon and will continue to learn and add techniques in an attempt to make as accurate of predictions as possible and maybe one day win the Millionaire Maker.


### Dictionaries
---
**Code Dictionary:**

|File Name|Folder|Description|
|---|---|---|
|EDA|code|Notebook reading in all data and cleaning for modeling|
|DEF_plots|code|Notebook showing plots of position stats versus defense| 
|game_model|code|Notebook modeling team data over weeks 1 through 9 and applying predictions|
|qb_model|code|Notebook modeling QB data over weeks 1 through 9 and applying predictions to week 10's matchups|
|rb_model|code|Notebook modeling RB data over weeks 1 through 9 and applying predictions to week 10's matchups|
|te_model|code|Notebook modeling QB data over weeks 1 through 9 and applying predictions to week 10's matchups|
|wr_model|code|Notebook modeling QB data over weeks 1 through 9 and applying predictions to week 10's matchups|


**Dataset Dictionary:**

|File Name|data|Description|
|---|---|---|
|DEF_QB|data|Defensive stats allowed by QB for weeks 1-9|
|DEF_RB|data|Defensive stats allowed by RB for weeks 1-9|
|DEF_TE|data|Defensive stats allowed by TE for weeks 1-9|
|DEF_WR|data|Defensive stats allowed by WR for weeks 1-9|
|team_game|data|Offensive game stats for weeks 1-9|
|QB_season_9|data|QB game stats for weeks 1-9|
|RB_season_9|data|RB game stats for weeks 1-9|
|TE_season_9|data|TE game stats for weeks 1-9|
|WR_season_9|data|WR game stats for weeks 1-9|
|week_10_QB|data|Week 10 projections for QB including season long stats and matchup stats|
|week_10_RB|data|Week 10 projections for RB including season long stats and matchup stats|
|week_10_TE|data|Week 10 projections for TE including season long stats and matchup stats|
|week_10_WR|data|Week 10 projections for WR including season long stats and matchup stats|
|week_10_QB_preds|data|Modified dataset for running week 10 QB predictions against|
|week_10_RB_preds|data|Modified dataset for running week 10 RB predictions against|
|week_10_TE_preds|data|Modified dataset for running week 10 TE predictions against|
|week_10_WR_preds|data|Modified dataset for running week 10 WR predictions against|
|week_10_QB_results|data|Actual week 10 QB performance|
|week_10_RB_results|data|Actual week 10 RB performance|
|week_10_TE_results|data|Actual week 10 TE performance|
|week_10_WR_results|data|Actual week 10 WR performance|
