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
---

## Data Cleaning and Exploratory Data Analysis
`clean_1 = df[['league', 'side', 'position', 'playername', 'teamname', 'gamelength', 'kills', 'deaths', 'dpm', 'damagetakenperminute', 'wpm', 'wardskilled', 'damagemitigatedperminute']].fillna(np.nan)`
I take all of the columns I need to continue my reseach, and fill all empty values with na to indicate that the value is missing.

`clean_t = clean_1[clean_1['position'] == 'team']`
This dataframe contains information for teams only. I would encounter problems if I included both players and teams because they share the same values for some columns, while other columns contain stats that vary significantly. To ensure a more precise analysis, I will focus solely on teams, which represent the overall stats for players on each team.

First let's look at the stats for dpm
**stat**
The median of stats is around 2500, but we can see there are lots of outliers on the right side of the graph. 
That tell us lots of teams are performing extremely well on how to dealt tons of damage to the enemy team. I wouldn't say the 
team has the higher damage dealt per minutes is more likely to finish the game early, but it is definitely one of the factors
that leads to shorter game.

Next, we will look the relationship between gamelength and wpm
**stat**
This tells us that the number of wards placed per minute has a positive correlation with the length of each game. 
The more wards a team places down, the more likely the game will last longer. This makes sense since vision is an important 
factor in League of Legends. If a team doesn't place wards frequently, it could indicate reckless play or facing a weak team,
which often leads to an early finish. On the other hand, if a team places wards often, it could suggest they are playing carefully, or they are facing an evenly matched opponent, which forces the team to play more patiently.

Let's look at one more scatter between kills and deaths
**stat**
It is pretty obviously that there isn't a strong correlation between  kills and deaths 


The last analysis we are interested in is the average number of deaths on blue side and red side in different leagues.
**stat**
By looking at the average number of deaths on both sides across all of the leagues, I can see that the red side tends to have a larger average number of deaths compared to the blue side. This is not surprising. In each competition, every team gets to choose whether they want the blue side or the red side. The majority of the time, teams choose the blue side due to geographic advantages, which explains why the blue side has fewer average deaths compared to the red side across the leagues.
---

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

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis

