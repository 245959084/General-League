# General-League

by JingJun Ni (jin010@ucsd.edu)

## Introduction
League of Legends was created in 2009 and has become one of the most influential games worldwide due to its unique mechanics and the creative strategies it allows. League of Legends is a 5v5 game, where the goal is to destroy the enemy team's Nexus (the final tower) located on the opposite side of the map. There are three main paths leading to the Nexus: the top lane, the middle lane, and the bottom lane. Each lane has towers designed to protect the Nexus, and players must destroy these towers in order to reach the enemy's Nexus.

You are not alone in this task; in addition to your four teammates, you also have minions, which are AI-controlled units that assist in pushing down towers and controlling the lanes. Minions begin spawning at 1 minute and 15 seconds into the game and respawn every 30 seconds thereafter. They march down the top, middle, and bottom lanes to support your efforts.

Top: Typically focuses on pushing down the top lane towers.
Jungle: Roams the map, fighting neutral monsters in the jungle to gather resources and provide assistance to teammates.
Mid: Primarily pushes the middle lane, but often helps teammates during team fights.
Bot: Focuses on pushing the bottom lane towers and usually deals the majority of the team's damage.
Support: Typically accompanies the bot player, helping them during the game by providing utility and protection.
Among the players of League of Legends, there are a group of highly skilled individuals, and there are special leagues where 
these talented players can showcase their abilities. These leagues are divided by region, with each team consisting of at 
least five players. Each league attracts a large audience who supports their favorite teams.

As a viewer of these leagues, I often wonder: **what factors influence the length of a game (the total time played in one match)**?
To find the answer, I visited 2024 League of Legends pro leagues stats, because the playing style, teams strategies changed year over year.
I will just be investing recently stats in 2024.

There are 116064 rows in the dataframe
We will be looking at these columns for the research:
- league: Gives us which league are they playing in
- side: this tells us whether the team is on the blue side or the red side
- position: This tells us what kind of role each player is choosing
- playername: tell us the name of each player
- teamname: teams' name, we might have interest to do some test between teams
- gamelength: the main factor, time per game
- kills: how many enemy each player, team kills
- deaths: how many time each player, team died
- dpm: player or team's damage per minutes
- damagetakenperminute: how much damage a player or a team has taken per minute
- wpm: the number of wards(visions) placed down by a player or a team per minute
- wardskilled: the total number of wards(visions) killed by a player or a team in the whole game
- damagemitigatedperminute: the damge a team or a player reduce with character he played.

Above, will be everything we will be investing through this research


## Data Cleaning and Exploratory Data Analysis
`clean_1 = df[['league', 'side', 'position', 'playername', 'teamname', 'gamelength', 'kills', 'deaths', 'dpm', 'damagetakenperminute', 'wpm', 'wardskilled', 'damagemitigatedperminute']].fillna(np.nan)`

I take all of the columns I need to continue my reseach, and fill all empty values with na to indicate that the value is missing.

`clean_t = clean_1[clean_1['position'] == 'team']`

This dataframe contains information for teams only. I would encounter problems if I included both players and teams because they share the same values for some columns, while other columns contain stats that vary significantly. To ensure a more precise analysis, I will focus solely on teams, which represent the overall stats for players on each team.

First let's look at the stats for dpm
<iframe src="Assets/uni_dpm.html" width=800 height=600 frameBorder=0></iframe>
The median of stats is around 2500, with a mean little bit high than that. That mean the graph is a bit skewed to the right.
I wouldn't say the team has the higher damage dealt per minutes is more likely to finish the game early, but it is definitely one of the factors that leads to shorter game.

Next, we will look the relationship between gamelength and wpm
<iframe src="Assets/bi_game_wpm.html" width=800 height=600 frameBorder=0></iframe>
This tells us that the number of wards placed per minute has a positive correlation with the length of each game. 
The more wards a team places down, the more likely the game will last longer. This makes sense since vision is an important 
factor in League of Legends. If a team doesn't place wards frequently, it could indicate reckless play or facing a weak team,
which often leads to an early finish. On the other hand, if a team places wards often, it could suggest they are playing carefully, or they are facing an evenly matched opponent, which forces the team to play more patiently.

Let's look at one more scatter between kills and deaths
<iframe src="Assets/bi_kills_dea.html" width=800 height=600 frameBorder=0></iframe>
It is pretty obviously that there isn't a strong correlation between kills and deaths. By looking at the graph, it tells us
teams' deaths varies even if the teams get the same number of kills.


The last analysis we are interested in is the average number of deaths on blue side and red side in different leagues.
**stat**
By looking at the average number of deaths on both sides across all of the leagues, I can see that the red side tends to have a larger average number of deaths compared to the blue side. This is not surprising. In each competition, every team gets to choose whether they want the blue side or the red side. The majority of the time, teams choose the blue side due to geographic advantages, which explains why the blue side has fewer average deaths compared to the red side across the leagues.


## Assessment of Missingness
There are two columns in my cleaned dataframe that have missing values, they are playername and damagemitigatedperminute.
The dataframe contains 19344 rows, and there are 19344 missing values for playername column, and there are 2782 missing values
for damageitigatedperminute column. The missingness for playername is not NMAR, is missing by Design.#Why? When I was at my first step for cleaning data, the position column contains values like: Top, Jng, Mid, Bot, Sup and team, playername
#exist for rows such that the value stored in position column is not team. So, when I move on to my second steps, All of the values in playername are taken away when I drop all of the rows that doesn't have team in their position column. That is why playername is MD because it is dependent on the values in position.

damageitigatedperminute, this column has fewer missing values. But the missingess is still not NMAR. 
Look through the other columns, there isn't a significant relationship between damageitigatedperminute and numerical columns. 
While I was looking through the columns, I found out that there is deterministic relationship between league column and 
damageitigatedperminute column. I found out damageitigatedperminute has a missing value if
the value in league column is either LPL, LDL, MSI, Dcup, or WLDs. That tell us damageitigatedperminute is missing dependent on leagueWhich is MAR or the missingness could be done by purpose, which makes it MD.

Keep moving on with missingness, we want to see if missing values in damageitigatedperminute is dependent on other 
columns, let's use **death** columns. 

<iframe src="Assets/missing.html" width=800 height=600 frameBorder=0></iframe>
Looking at the distribution of damageitigatedperminute not missing data seems to roughly skewed to the right,
while the distribution of damageitigatedperminute is missing data seems rougly normal.

Permutation test
Null Hypothesis: The average 'deaths' when 'damage reduced per minute' is not missing is equal to the average 'deaths' when 'damage reduced per minute' is missing.

Alternative Hypothesis: The average 'deaths' when 'damage reduced per minute' is not missing is not equal to the average 'deaths' when 'damage reduced per minute' is missing

Test Statistic: absolute(mean of death(damage reduced per minute is not missing) - mean of death(damage reduced per minute is missing))

The significant level is 0.05

After the testing, the P_value(about 0.0) I get is lower than the significant level, this tells us that we reject the null hypothesis, which mean the mean of death stats vary when damageitigatedperminute is missing or not missing. This also support that
the missingness in damageitigatedperminute is likely to be MAR dependent on deaths
<iframe src="Assets/empirical_dea.html" width=800 height=600 frameBorder=0></iframe>



I also want to see if the missingness in damagemitigatedperminute is dependent on side column.
<iframe src="Assets/side_bar.html" width=800 height=600 frameBorder=0></iframe>

Permutation test
Null Hypothesis: The distribution of 'side' will not be different between non-missing 'reduce damage per minute' data and missing'reduce damage per minute' data.

Alternative Hypothesis: The distribution of 'side' will be different between non-missing 'reduce damage per minute' data and missing'reduce damage per minute' data.

Test Statistic: total variation distance between missing data and non-missing data base on side

The significant level is 0.05

The p_value(about 0.979) I get is significantly larger than our significiant level, so we fail to reject the null hypothesis, telling us the missingness in damagemitigatedperminute is not dependent on side column, this not MAR. But at the same time, we couldn't say the data is MCAR. Because there could be other columns that cause the value to miss.



## Hypothesis Testing
The central idea for this research deals with time played per game, and there are lots of factors we can use to play
with testing. At the same time, I am a fan of pro leagues. I watch LCK, LPL, MSI, World Cup quite often. So my question is
which league tends to have longer game length in 2024?

Before I perform the testings, I personally think LCK has longer game length compare other leagues because LCK have stronger players, and stronger teams. For example, SK Telecom, is a legendary team in League of Legends, this team has won 3 world championship, regarded as the number 1 team in the world by fans of League of Legends. This team is in LCK league, and they renamed to T1, and won two more world championship recently. Another factor is that LCK's team really like to use strategies to win the game, and they like to play safe. Their playing style are not as risky as playing style in other leagues, that is why I think LCK might have the longer gamelength in 2024.

The first test league vs gamelength, I will be choosing two leagues among all of the league. wLDs(World Series) and MSI(Middle Season Invitational) are consider international competitions, let's start by comparing these two leagues. Now we have two samples, I will be performing permutation to see if two leagues have same amount of gamelength per game.

Null hypothesis: The average gamelength in the world is equal to the average gamelength in the MSI.

Alternative hypothesis: The average gamelength in the world is longer comare to the average gamelength in the MSI
Teams tend to player more carefully in Worlds.

Test statistic: The mean of gamelength for Worlds - The mean of gamelength of MSI

Significant level is set to be 0.05
<iframe src="Assets/world_msi.html" width=800 height=600 frameBorder=0></iframe>

The p value I get is around 0.0001, we reject the null hypothesis, saying that the average gamelength for worlds is not likely to be the same as the average gamelength for MSI, and it tends show that the average gamelength for the worlds is higher than the average gamelength for MSI.Connecting this result to the real world, it actually make sense, since who ever wins the world gains a world championship, it is everyone's goal. So most of people will choose to play at a slower pase to seek for chance.


The second test is also league vs gamelength, but this time, we have LPL vs LCK. The two leagues are consider two of the most 
competitive leagues, and most of the time, the teams from these two Leagues are fighting in the World's final. I will use permuation to compare and contrast the gamelength between these two leagues.

Null hypothesis: The average gamelength in the LCk is equal to the average gamelength in the LPL.
Alternative hypothesis: The average gamelength in the LCK is lower comare to the average gamelength in the LPL.

Test statistic: The mean of gamelength for LCK - The mean of gamelength of LPL.

Significant level is set to be 0.05

The p value I get is around 0.19, we fail to reject the null hypothesis because the P_value we receive is high than the significant level. Saying that two leagues are likely to have the same average gamelength. This is actually little bit surprising to me, because I thought LPL is going to have lower average gamelength, because LPL players really like to have team fights, which could lead to early finish. But at the same time, it make sense beacuse both leagues are competitive.


## Framing a Prediction Problem
After discovering the missingness in the dataframe and the some relationship between columns and gamelength, **we are going to predict the gamelength of each game**. The ultimate goal of this project is to predict the gamelength.

Why are we interested in predict the gamelength? 
As the corona virus become less effective, people now spend fewer time at home, either because of their works or outdoor activities. Since people now are less willing to spend too much time on playing or watching the game than before, 
we want to if there are any factor that can reduce the gamelength. Obviously, we cannot just unplug the internet to minimize the gamelength(Your teammate are not going to be happy), so, let's try to find those factors in the game. Since we are prediciting a numerical column, we will be using **regression**. The response variable we will be using is "gamelength". Since we are using regression to predict numerical value, we will use R^2 to see how well our model predict the gamelength.
We don't use F1-score, since we are prediciting a numerical result. R^2 or RMSE both work, in this research, we will use R^2 to see how our model correlates with the actual gamelength.

Now, choosing out features. In previous tests, we found out there is some kind of relationship between league and gamelength. But there are too many leagues(unique) in league feature, we will choose the four most viewed leagues in our model, which is World, MSI,LCK, and LPL. Next, I have kills and deaths features in my model since they are consider as important factors in the game, and the two do not have a strong correlation. I will have wpm(wards placed down per minute) feature in model since it shows a roughly positive correlation with gamelength. Other than those features, I will have three features in my model: dpm(damage dealt per minute),damagetakenperminute, wardskilled. Even though I didn't test with them in the early test, they are all consider important factors thataffect the game. I like to mention that having higher damage dealt per time doesn't neccessary mean you will have more kill. Havinghigher damagetakeperminute doesn't neccessary mean your character will more likely to die.

**Features: league, kills, deaths, wpm, dpm, damagetakenperminute, wardskilled.**
**Prediciting: gamelength.**


## Baseline Model
This model includes one hot encoding, applying natural log transformations to the data, and the create a linear regression model.
The features are:
- league: Nominal categorical variable that contains four unique values: MSI, WLDs, LCK, LPL, each stands for a league for pro-players.
- kills: Discrete numerical variable that contains total number of kills for each team.
- deaths: Discrete numerical variable that contains total number of deaths for each team's characters.
- wpm: Continuous numerical variable, stands for wards(visions) placed down per minute for each team.
- gamelength: Discrete numerical variable that contains the length of a game for each time.
- dpm: Continuous numerical variable, stands for each team's damage dealt per minute.
- damagetakenperminute: Continuous numerical variable, stands for each team's damage taken per minute.
- wardskilled: Discrete numerical variable, stands for each team's total number of wards killed per game.

Before the transformation, I splited my data into training set and testing set, training set contains 80 percent of data.

Performed One Hot Ecoding for the only categorical value, which splited league to world, msi, lpl, lck, four columns that contains either 0 or 1. I transformed wpm with natural log to wpm to make it less important compares to other variables.
Then I fit the transform data into the linear regression model and find data's R^2.

#The R^2, which is the Coefficient of determination between the actual gamelength and predicted gamelength is about 
**0.65771616** for seen data(training) and **0.6669366** unseen data(testing). This is a decent model for prediciting the gamelength. Knowing that fact that gamelength is a challenging value to predict, since it can be influenced by mood of players on both side, the R^2 tells me that the model perform a decent prediction on gamelength. Let's see if we can find a better prediction.

## Final Model
In my decision for final model, I used both Linear Regression and Decision Tree Regressor, the result of Decision Tree Regressor varies by **max_depth(1 to 5)**, which is **one of hyperparameters** I have in my training, I want to find a max_depth such that it minimizes the bias and variance that can give me a low risk which gives me a high R^2. **Another hyperparameter that I have is transformation**, I have two transformations that have a little difference. This hyperparameter
are mainly being find the difference between changes to features, both transformations are also being applied to
#Decision Tree Regressor and linear regression. So, in total, I have (5+1)*2 = 12 models to test. The additional one comes from linear regression, so max_depth is only part of first hyperparameter, the whole first hyperparameter should be **linear regression plus decision tree regressor with max_depth from 1 to 5**.


In the **first transformataion**, I added 'wardskilled' to the FunctionTransformer(np.log) to make it less important compares rest of features, even though vision is a factor we need to think about when prediciting the gamelength, I personally, its importance is not as significant as the other variables, so I applied log to wardskilled, I had wpm in the log transform for the same reasoning. Then, I put in rest of the **columns('kills', 'deaths', 'dpm','damagetakenperminute')** into FunctionTransformer(root) to weaken their effects on the prediction, though the weaken effect is not as strong as the np.log. There is a reason for this, due to my experience on watching the pro-games, I have never seen a game that is shorter than 15 minutes. Knowing the fact the we can see 30 kills, 20 deaths in a 25 minutes or a 40 minutes game, I weaken the importance of kills and deaths. I put into dpm and damagetakenperminute in complete out of my curiosity, to see if these
two factors are important or not when predicting the gamelength.

In the **second transformation**, most of features are the same, except that I now transform **'wardskilled' and 'wpm' with the z-score**, instead of apply np.log. This is because my friend who play jungle(a role in the game) tells that visions much important than I thought. The more competitive the match is(even match), the more wards(vision) he will likely purchase in order to win the fight(because most player need visions to plan the next move). Transforming them doesn't change the importance of two variables, it is transformed to just add some transformation on top of them. 
The next difference is that **I didn't transformation dpm and damagetakenperminute with root** this time
because bigger dpm and damamgetakenperminute might tell us that players encounter lots of team fights, and team fights happen often in the late game, when characters are full built(6 max items), they will be to create more damages and taken more damages. That is why I passthrough them. But at the same time, I want to see if this idea is correct by comparing this transformation with the first transform. So two transformations are being applied to all linear regression and decision tree regressor.By making those less important variables less weighted, I believe it makes my prediction better.

The method I used to select hyperparameters and my overall model is **cross_val_score, 5-fold cross-validation**. Each fold, the method take 1/5 portion of transformed data, and finds its R^2. We have 5 folds per model, and we will decide which model has the best prediction by finding the model that has the highest average of 5 R^2. In my test, model that does the best in prediction is **linear regression with the second transformation**. This final model does a better prediction in both training set and testing set compare to baseline model, with **0.6603470680419258** for training set and **0.6740973819160796** for testing set. Though by the R^2 improved by around 0.005 to 0.01 for both training and testing set. The little change is due to the fact that gamelength is extreme hard to predict by just looking at the stats in the game, it also depends on the mood of players, if players on both side have equivalent skills andstrong wills to win the game, the game is more likely to longer. Unlucky, we don't have stats like that in the data. 



## Fairness Analysis
In the last part of this research, I want to see if the R^2 is the same for two different groups.

Group X will be named as offense group, this group contains teams that have more than 13 kills per game.
Group Y will be named as defense group, this group contains teams that have lower than or equal to 13 kills per game
Since my final model is regression model, the evaluation metric of this analysis will be R^2

Permutation
Null hypothesis: The regression's R^2 is the same for both offense group and defense group, and any differences are due to chance.
Alternative Hypothesis: The regression's R^2 is higher for offense group.

Test statistic: Difference in R^2 (offense minus defense).

The siginificant level is 0.05

The p value for the test is 0.377, which higher than the significant level. I failed to reject the null hypothesis. It seems like the model has achieve R^2 parity.
<iframe src="Assets/r2_parity.html" width=800 height=600 frameBorder=0></iframe>