<h3>Author: Meghna Mahesh (meghnam@umich.edu)</h3>

<h2>Introduction</h2>

League of Legends (LoL) is a multiplayer online battle arena video game created in 2009 by Riot Games. In League of Legends, two teams of five players each face each other in player-versus-player combat. Each team is made up of five roles: Top Lane, Jungle, Mid Lane, Bot Lane (ADC), and Support. The question I am exploring is: <b>how important is each role to a team's success?</b> My analysis will help players and coaches optimize team composition and play styles to improve their chances of victory.

The dataset that I will be using contains League of Legends data from over 10,000 matches in Spring 2022 and was developed by Oracle's Elixir, with 150,180 rows and 161 columns. There are 12 rows for each match: one for each of the 5 players on both teams and 2 containing summary data for the two teams. Each row contains information about several key gameplay statistics, and the dataset as a whole can provide significant insight into successful LoL strategies.

The columns relevant to my analysis are described below in greater detail:
<ul>
<li><code>gameid</code>: This column contains a unique identifier used to distinguish between matches in the dataset.</li>
<li><code>result</code>: This is a binary column which indicates the outcome of the match for the team. It contains a 1 if the team won the match and a 0 if the team did not.</li>
<li><code>position</code>: This column specifies what role an individual player plays on their team. There are 5 possible individual player roles: 'top' (Top Lane), 'jng' (Jungle), 'mid' (Mid Lane), 'bot' (Bot Lane or ADC), 'sup' (Support). This column contains 'team' for the team rows.</li>
<li><code>kills</code>: This column contains the total number of kills the individual player or team achieved during the match. </li>
<li><code>deaths</code>: This column contains the total number of deaths of the individual player or team during the match.</li>
<li><code>assists</code>: This column contains the total number of assists of the individual player or team during the match.</li>
<li><code>damageshare</code>: This column contains the the proportion of the team's total damage that was dealt by a specific player. It is empty for rows with team data.</li>
<li><code>dpm</code>: 'dpm' stands for damage per minute. This column contains the amount of damage a player or team dealt per minute during a game.</li>
<li><code>damagetakenperminute</code>: This column contains the amount of damage a player or team received per minute during a game.</li>
<li><code>totalgold</code>: This column contains the total amount of gold a player or team accumulated during a game</li>
<li><code>minionkills</code>: This column contains the total number of minions (non-player characters) a player or team killed during the game.</li>
<li><code>monsterkills</code>: This column contains the total number of monsters a player or team killed during the game.</li>
<li><code>wardskilled</code>: This column contains the total number of enemy wards a player or team destroyed during the game.</li>
<li><code>playername</code>: This column contains the in-game name of the player, which is a unique label used to distinguish between individual participants.</li>
<li><code>teamname</code>: This column contains the name of the team, which is a unique label used to distinguish between different teams.</li>
<li><code>league</code>: This column identifies the professional league that the match took place in.</li>
</ul>

<h2>Data Cleaning and Exploratory Data Analysis</h2>

<h4>In this section, I narrowed the focus of my central question to the following: <b>Which role “carries” (does the best) in their team more often: ADCs (Bot lanes) or Mid laners?</b></h4>

<h3>Data Cleaning</h3>
Because my central question is about individual player roles, I first removed all of the rows with overall team data from my data frame. These were the rows where the 'position' column contained 'team'. This left me with only individual player data, where each role was one of the following five roles: 'top' (Top Lane), 'jng' (Jungle), 'mid' (Mid Lane), 'bot' (Bot Lane or ADC), 'sup' (Support). This helped ensure that I didn't have any issues with my analysis later when I was focusing on individual player data.

Next, I determined that all of the columns in the dataset with 'first' in the title should have been of type 'bool' but were not. For example, the 'firstblood' column contains 1 if the player achieved the first kill in a game and 0 if they did not. This should have been a column of type 'bool', but it was a numerical column instead. To fix this, I created a list of all of the columns with 'first' in the title and cast all of these columns to type 'bool'. While I didn't end up using any of these columns, this cleaning step could be very beneficial for future analysis.

These were the only two steps I took to clean my data.

Here are the first 5 rows of the cleaned DataFrame:

|    | gameid                | teamname          |   result | playername   | position   |   kills |   deaths |   assists |   totalgold |   minionkills |   monsterkills |   wardskilled |     dpm |   damageshare |   damagetakenperminute |
|---:|:----------------------|:------------------|---------:|:-------------|:-----------|--------:|---------:|----------:|------------:|--------------:|---------------:|--------------:|--------:|--------------:|-----------------------:|
|  0 | ESPORTSTMNT01_2690210 | BRION Challengers |        0 | Soboro       | top        |       2 |        3 |         2 |       10934 |           220 |             11 |             6 | 552.294 |     0.278784  |               1072.4   |
|  1 | ESPORTSTMNT01_2690210 | BRION Challengers |        0 | Raptor       | jng        |       2 |        5 |         6 |        9138 |            33 |            115 |            18 | 412.084 |     0.208009  |                944.273 |
|  2 | ESPORTSTMNT01_2690210 | BRION Challengers |        0 | Feisty       | mid        |       2 |        2 |         3 |        9715 |           177 |             16 |             7 | 499.405 |     0.252086  |                581.646 |
|  3 | ESPORTSTMNT01_2690210 | BRION Challengers |        0 | Gamin        | bot        |       2 |        4 |         2 |       10605 |           208 |             18 |             6 | 389.002 |     0.196358  |                463.853 |
|  4 | ESPORTSTMNT01_2690210 | BRION Challengers |        0 | Loopy        | sup        |       1 |        5 |         6 |        6678 |            42 |              0 |            14 | 128.301 |     0.0647631 |                475.026 |

There are too many columns to successfully display, so I chose to only include essential identification columns and the columns relevant to my later analysis.

<h3>Univariate Analysis</h3>
<iframe
  src="assets/damage-share-per-player.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

This histogram shows that the distribution of the damage share column is bimodal, as there are two distinct peaks. This indicates that damage share may vary based on other factors, such as role.

<h3>Bivariate Analysis</h3>

<iframe
  src="assets/kills-by-role.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

This side-by-side box plot shows the distribution of kills for each role (ADCs and Mid laners) and indicates that ADCs generally have more kills than Mid laners. This suggests that ADCs provide more offensive power to their teams than Mid laners do.

<iframe
  src="assets/avg-kills-deaths-by-role.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

This side-by-side bar chart shows the average number of kills and deaths per match for each role (ADCs and Mid laners). It indicates that ADCs generally have more kills and slightly less deaths than Mid laners. This suggests that ADCs contribute more to their team's success than Mid laners do.

<h3>Interesting Aggregates</h3>

| Position       |   Average Damage Share per Match |   Average Number of Deaths per Match |   Average Number of Kills per Match |
|:---------------|---------------------------------:|-------------------------------------:|------------------------------------:|
| Bot Lane (ADC) |                         0.266759 |                              2.54938 |                             4.26121 |
| Mid Lane       |                         0.260919 |                              2.65733 |                             3.50212 |

This table shows the average damage share, the average number of deaths, and the average number of kills per match for each role (ADCs and Mid laners). It indicates that ADCs generally have  more kills and slightly less deaths than Mid laners, with approximately the same damage share per match, implying that they "carry" their team more often.

<h4>Conclusion</h4>
The figures above seem to indicate that ADCs are marginally more important to a team's success than Mid laners. They have more kills per match and die slightly less per match, indicating that they "carry" their team more often. However, more analysis should be done on other features to make a stronger conclusion.

<h3>Imputation</h3>

I imputed values in 6 columns: 'minionkills', 'monsterkills', 'wardskilled', 'damageshare', 'dpm', and 'damagetakenperminute'. These were the only columns with NaN values that I was planning on using in my later analysis. To impute each missing value, I replaced it with the mean of the corresponding statistic for that player across their other games.

For example, if one row with playername 'Soboro' had a missing 'minionkills' value, I filled in this value with the mean 'minionkills' value for 'Soboro' across Soboro's other games. I felt that this would successfully fill in missing values while accurately representing a player's ability.

<h4>Distributions of Imputed Variables before Imputation</h4>

Below, I have plotted the distributions of the 6 columns I imputed values for before imputation.

<iframe
  src="assets/minion-kills-before.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
<iframe
  src="assets/monster-kills-before.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
<iframe
  src="assets/wards-killed-before.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
<iframe
  src="assets/damage-share-before.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
<iframe
  src="assets/dpm-before.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
<iframe
  src="assets/damage-taken-per-minute-before.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

<h4>Distributions of Imputed Variables after Imputation</h4>

Below, I have plotted the distributions of the 6 columns I imputed values for after imputation.

<iframe
  src="assets/minion-kills-after.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
<iframe
  src="assets/monster-kills-after.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
<iframe
  src="assets/wards-killed-after.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
<iframe
  src="assets/damage-share-after.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
<iframe
  src="assets/dpm-after.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
<iframe
  src="assets/damage-taken-per-minute-after.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

<h2>Framing a Prediction Problem</h2>

My prediction problem is as follows: <b>Predict which role (Top Lane, Jungle, Mid Lane, Bot Lane (ADC), Support) a player played given their post-game data.</b> 

This is a classification problem, and because there are 5 distinct roles, I am performing multiclass classification. The response variable is the 'position' column. I chose this response variable because I wanted to see if there were significant enough differences in key gameplay statistics between roles to predict a player's position solely from their game statistics. Statistical differences between roles could inform team composition strategy. I am using accuracy as a measure of success instead of other suitable metrics such as f1-score because the dataset is balanced, with the same number of players for each role, and accuracy performs well with balanced datasets.

I am using the following features for my prediction model: 'kills', 'deaths', 'assists', 'totalgold', 'minionkills', 'monsterkills', 'wardskilled', 'dpm', 'damageshare', 'damagetakenperminute'. I chose these features because they are all common metrics of player success in League of Legends. All of these features are post-game statistics, so they will be available at the time of prediction.

<h2>Baseline Model</h2>
My baseline prediction model uses a KNeighborsClassifier on the features 'kills' and 'deaths', which are both quantiative features. I chose to use these two features because they are direct measures of player success, and I hypothesized that they would vary based on importance of role. I used no categorical features, so I did not need to perform any encodings.

My model's accuracy is about 0.29. In my opinion, this is not a very successful model, as it mispredicts more often than it predicts correctly. This is likely because only using the 'kills' and 'deaths' columns does not give the model enough information to make successful predictions.

<h2>Final Model</h2>

In addition to 'kills' and 'deaths', I used 'assists', 'totalgold', 'minionkills', 'monsterkills', 'wardskilled', 'dpm', 'damageshare', and 'damagetakenperminute' for my final prediction model. I chose these features because they collectively measure the amount of damage that a player causes, the amount of gold they earn, and the amount of damage that they sustain, all of which are the most important factors contributing to player success. I once again used a KNeighborsClassifier.

To engineer new features, I used StandardScaler on all of the columns, as they are all quantitative. This is especially important for a KNeighborsClassifier, as it relies on the distance between points to make a prediction. Without standardization, the features with larger scales could be weighted too heavily by the model. Standardizing the features with StandardScaler eliminates this concern, thus improving model performance.

In addition, I added a new feature to the data called 'kda' using a Function Transformer. KDA (the Kill-Death-Assist ratio) uses the following equation: (Kills + Assists) / Deaths. While it's a common metric of success in League of Legends, it wasn't in the existing dataset. I added it as part of the pipeline to give the model another success metric to based its predictions off of.

As I mentioned above, I used a KNeighborsClassifier again, but this time, I searched for the best number of neighbors using hyperparameters, from 1 to 19 inclusive. I did this with GridSearchCV, and using 19 neighbors for the KNeighborsClassifier performed the best.

This prediction model had an accuracy of 0.78, which was significantly better than the baseline model's accuracy of 0.29. This means that it correctly predicted a player's role based on their post-game statistics nearly 80% of the time, making it a successful model, in my opinion.

The success of the model indicates that there is difference in performance across roles. By identifying which roles generally perform better (and are therefore more important to the team), players and coaches can optimize team composition to maximize success.