# League of Legends: Role Importance
<p>This is a homework assignment for EECS 398 at the University of Michigan.<p>
<p>By: Meghna Mahesh (meghnam@umich.edu)</p>

<h2>Introduction</h2>

<p>League of Legends (LoL) is a multiplayer online battle arena video game created in 2009 by Riot Games. The dataset that I will be using contains League of Legends data from over 10,000 matches in Spring 2022 and was developed by Oracle's Elixir. It has 150,180 rows and 161 columns. There are 12 rows for each match: one for each of the 5 players on both teams and 2 containing summary data for the two teams. Each row contains information about several key gameplay statistics, and careful analysis of these statistics can provide significant insight into successful LoL strategies.</p>

<p>In League of Legends, two teams of five players each face each other in player-versus-player combat. Each team is made up of five roles: Top Lane, Jungle, Mid Lane, Bot Lane (ADC), and Support. The question I am exploring is: <b>how important is each role to a team's success?</b> My analysis will help players and coaches optimize team composition and play styles to improve their chances of victory.</p>

<p></p>

<p>Here are some of the relevant columns in more detail:</p>
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
<li><code>minionkills</code>: The column contains the total number of minions (non-player characters) a player or team killed during the game.</li>
<li><code>monsterkills</code>: The column contains the total number of monsters a player or team killed during the game.</li>
<li><code>wardskilled</code>: The column contains the total number of enemy wards a player or team destroyed during the game.</li>
<li><code>playername</code>: This column contains the in-game name of the player, which is a unique label used to distinguish between individual participants.</li>
<li><code>teamname</code>: This column contains the name of the team, which is a unique label used to distinguish between different teams.</li>
<li><code>league</code>: This column identifies the professional league that the match took place in.</li>
</ul>

<h2>Data Cleaning and Exploratory Data Analysis</h2>

<h3>Data Cleaning</h3>
<p>Because my central question is about individual player roles, I first removed all of the rows with team data from my data frame so as not to have issues later on in my analysis. These were the rows where the 'position' column contained 'team'. This left me with only individual player data, where each role was one of the following five roles: 'top' (Top Lane), 'jng' (Jungle), 'mid' (Mid Lane), 'bot' (Bot Lane or ADC), 'sup' (Support).</p>

<p>Next, I determined that all of the columns in the dataset with 'first' in the title should have been of type 'bool' but were not. For example, the 'firstblood' column contains 1 if the player achieved the first kill in a game and 0 if they did not. This should have been a column of type 'bool', but it was a numerical column instead. To fix this, I created a list of all of the columns with 'first' in the title and cast all of these columns to type 'bool'.</p>

<p>These were the only two steps I took to clean my data.</p>

<p>Here are the first 5 rows of the cleaned DataFrame:</p>
<p></p>

| gameid                | playername   |
|:----------------------|:-------------|
| ESPORTSTMNT01_2690210 | Soboro       |
| ESPORTSTMNT01_2690210 | Raptor       |
| ESPORTSTMNT01_2690210 | Feisty       |
| ESPORTSTMNT01_2690210 | Gamin        |
| ESPORTSTMNT01_2690210 | Loopy        |

<h3>Bivariate Analysis</h3>
<iframe
  src="assets/avg-kills-deaths-by-role.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>