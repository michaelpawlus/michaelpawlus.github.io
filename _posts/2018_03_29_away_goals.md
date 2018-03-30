---
title: "Away Goals Rule"
author: "Michael Pawlus"
layout: post
published: true
status: publish
draft: no
---
 
## What If Away Goals were always weighted at a Premium?
 
This is just a quick post that I have been meaning to do for a while on the away goals rule.
 
### What You Need To Know About The Away Goals Rule
 
There are some tournaments like the Champions League where teams play two legs: one home and one away. The winner is the team that scores the most goals, in aggregate, over the two legs. However, if at the end of the two legs the aggregate score is tied then the team that scored more away goals wins.
 
For example, if team A plays the first leg at home and that game ends 1-1 and then team A travels and plays as the away team and that game ends 2-2 then team A wins even though both games ended in a draw because team A scored 2 away goals while the other team only scored 1 away goal.
 
Basically, this rule places a small premium on goals scored away from home. I have always wondered: if we are going to say that away goals are more difficult then home goals and we want to say they are worth just a little more then why not apply this to league play as well as tournament games. The code below imagines just such a scenario for the 2016-2017 English Premier League season.
 
### R Code Included and Future Plans
 
Whether you care about this hypothetical set of outcomes or not, here is the R code included which may be of interest. There is a decent amount of `dplyr` to wrangle up the raw data and create summary tables. This is the first time that I used the `case_when` function which is great. This is also my first time creating a slope chart and I used the example from [Top 50 ggplot2 Visualizations](http://r-statistics.co/Top50-Ggplot2-Visualizations-MasterList-R-Code.html) to make it happen. 
 
This is just for one season. Next, I want to map this same script over as many past seasons as I can and wrap that all up in a Shiny app so anyone interested can select whichever season they want and see a chart similar to the one made below.
 
#### Load the Data and Libraries

{% highlight r %}
library(tidyverse)
library(scales)
library(knitr)
theme_set(theme_classic())
 
## read in the results from the 2016-17 EPL season
epl17 <- read_csv("http://www.football-data.co.uk/mmz4281/1617/E0.csv")
{% endhighlight %}



{% highlight text %}
## Parsed with column specification:
## cols(
##   .default = col_double(),
##   Div = col_character(),
##   Date = col_character(),
##   HomeTeam = col_character(),
##   AwayTeam = col_character(),
##   FTHG = col_integer(),
##   FTAG = col_integer(),
##   FTR = col_character(),
##   HTHG = col_integer(),
##   HTAG = col_integer(),
##   HTR = col_character(),
##   Referee = col_character(),
##   HS = col_integer(),
##   AS = col_integer(),
##   HST = col_integer(),
##   AST = col_integer(),
##   HF = col_integer(),
##   AF = col_integer(),
##   HC = col_integer(),
##   AC = col_integer(),
##   HY = col_integer()
##   # ... with 6 more columns
## )
{% endhighlight %}



{% highlight text %}
## See spec(...) for full column specifications.
{% endhighlight %}
 
#### Create the actual results from the 2016-2017 season
 
First, we will create a detail table for all home games by all teams.
 
From all the data, I `select` the home team, goals by the home team, goals by the away team and the result. `case_when` is used to tally up points: 3 if the home team won, 1 if the game ended in a draw and 0 if the home team lost.
 

{% highlight r %}
home_res <-  epl17 %>%
  select(HomeTeam, FTHG, FTAG, FTR) %>%
  rename(team = HomeTeam, GF = FTHG, GA = FTAG, result = FTR) %>%
  mutate(
    points = case_when(
      result == "H" ~ 3,
      result == "D" ~ 1,
      result == "A" ~ 0
    )
  )
{% endhighlight %}
 
 
Then, create a detail table for all away games by all teams in a similar manner:
 

{% highlight r %}
away_res <-  epl17 %>%
  select(AwayTeam, FTAG, FTHG, FTR) %>%
  rename(team = AwayTeam, GF = FTAG, GA = FTHG, result = FTR) %>%
  mutate(
    points = case_when(
      result == "A" ~ 3,
      result == "D" ~ 1,
      result == "H" ~ 0
    )
  )
{% endhighlight %}
 
Then we will stack these two tables and summarize to recreate the league table. `bind_rows` stacks the data. Goals for, goals against and points are just summed up and then the goal difference is calculated by subtracting goals against from goals for. Then, sort by points and goal differential descending. Add an ascending count for position and a descending count to use for plotting later just because it was easy.
 

{% highlight r %}
table <- bind_rows(
  home_res,
  away_res
) %>%
  group_by(team) %>%
  summarise(GF = sum(GF), GA = sum(GA), GD = GF-GA, points = sum(points)) %>%
  arrange(-points, -GD) %>%
  mutate(position = 1:20) %>%
  mutate(order = 20:1)
{% endhighlight %}
 
Now, we have a league table:
 

{% highlight r %}
kable(table)
{% endhighlight %}



|team           | GF| GA|  GD| points| position| order|
|:--------------|--:|--:|---:|------:|--------:|-----:|
|Chelsea        | 85| 33|  52|     93|        1|    20|
|Tottenham      | 86| 26|  60|     86|        2|    19|
|Man City       | 80| 39|  41|     78|        3|    18|
|Liverpool      | 78| 42|  36|     76|        4|    17|
|Arsenal        | 77| 44|  33|     75|        5|    16|
|Man United     | 54| 29|  25|     69|        6|    15|
|Everton        | 62| 44|  18|     61|        7|    14|
|Southampton    | 41| 48|  -7|     46|        8|    13|
|Bournemouth    | 55| 67| -12|     46|        9|    12|
|West Brom      | 43| 51|  -8|     45|       10|    11|
|West Ham       | 47| 64| -17|     45|       11|    10|
|Leicester      | 48| 63| -15|     44|       12|     9|
|Stoke          | 41| 56| -15|     44|       13|     8|
|Crystal Palace | 50| 63| -13|     41|       14|     7|
|Swansea        | 45| 70| -25|     41|       15|     6|
|Burnley        | 39| 55| -16|     40|       16|     5|
|Watford        | 40| 68| -28|     40|       17|     4|
|Hull           | 37| 80| -43|     34|       18|     3|
|Middlesbrough  | 27| 53| -26|     28|       19|     2|
|Sunderland     | 29| 69| -40|     24|       20|     1|
 
Next, we will do almost the same to create this alternate universe table where the away goals rule is enforced meaning that any games ending in a draw, other than a 0-0 draw, are won by the away team and result in 3 points for this team rather than a point a piece.
 
So, everything below is the same except that in this case if the result is a draw and there are goals then the home team, in this case, loses and gets 0 points.
 

{% highlight r %}
home_res_agr <-  epl17 %>%
  select(HomeTeam, FTHG, FTAG, FTR) %>%
  rename(team = HomeTeam, GF = FTHG, GA = FTAG, result = FTR) %>%
  mutate(
    points = case_when(
      result == "H" ~ 3,
      result == "D" & GF == 0 ~ 1,
      result == "D" & GF > 0 ~ 0,
      result == "A" ~ 0
    )
  )
{% endhighlight %}
 
Next we make the away team detail table and in this case if a game ends in a draw with goals then the away team wins and gets all three points.
 

{% highlight r %}
away_res_agr <-  epl17 %>%
  select(AwayTeam, FTAG, FTHG, FTR) %>%
  rename(team = AwayTeam, GF = FTAG, GA = FTHG, result = FTR) %>%
  mutate(
    points = case_when(
      result == "A" ~ 3,
      result == "D" & GF == 0 ~ 1,
      result == "D" & GF > 0 ~ 3,
      result == "H" ~ 0
    )
  )
{% endhighlight %}
 
Stacking and summarizing is done in the same way:
 

{% highlight r %}
table_agr <- bind_rows(
  home_res_agr,
  away_res_agr
) %>%
  group_by(team) %>%
  summarise(GF = sum(GF), GA = sum(GA), GD = GF-GA, points = sum(points)) %>%
  arrange(-points, -GD) %>%
  mutate(position = 1:20) %>%
  mutate(order = 20:1)
{% endhighlight %}
 
Now, we have an alternate league table in a universe where away goals are used as tiebreakers when applicable.
 

{% highlight r %}
kable(table_agr)
{% endhighlight %}



|team           | GF| GA|  GD| points| position| order|
|:--------------|--:|--:|---:|------:|--------:|-----:|
|Chelsea        | 85| 33|  52|     99|        1|    20|
|Tottenham      | 86| 26|  60|     92|        2|    19|
|Liverpool      | 78| 42|  36|     81|        3|    18|
|Man City       | 80| 39|  41|     77|        4|    17|
|Arsenal        | 77| 44|  33|     77|        5|    16|
|Man United     | 54| 29|  25|     66|        6|    15|
|Everton        | 62| 44|  18|     65|        7|    14|
|West Brom      | 43| 51|  -8|     56|        8|    13|
|Bournemouth    | 55| 67| -12|     53|        9|    12|
|West Ham       | 47| 64| -17|     50|       10|    11|
|Leicester      | 48| 63| -15|     48|       11|    10|
|Stoke          | 41| 56| -15|     48|       12|     9|
|Southampton    | 41| 48|  -7|     46|       13|     8|
|Crystal Palace | 50| 63| -13|     45|       14|     7|
|Swansea        | 45| 70| -25|     44|       15|     6|
|Watford        | 40| 68| -28|     41|       16|     5|
|Burnley        | 39| 55| -16|     39|       17|     4|
|Hull           | 37| 80| -43|     32|       18|     3|
|Middlesbrough  | 27| 53| -26|     31|       19|     2|
|Sunderland     | 29| 69| -40|     23|       20|     1|
 
The last bit of wrangling is to create a comparison table that we can use to make our slope chart. So here we get the team, order (for plotting), points and goal differential and then rename them so we can get the exact same columns from the alternate universe table. Then we assign these color tags to a column called class for reference when making our slope chart.
 

{% highlight r %}
comp_table <- table %>%
  select(team, order, points, GD) %>%
  rename(actual_order = order, actual_points = points, actual_gd = GD) %>%
  left_join(select(table_agr, team, order, points, GD), by = "team") %>%
  rename(adjusted_order = order, adjusted_points = points, adjusted_gd = GD) %>%
  mutate(
    class = case_when(
      (actual_order - adjusted_order) > 0 ~ "red",
      (actual_order - adjusted_order) < 0 ~ "green",
      (actual_order - adjusted_order) == 0 ~ "gray"
    )
  )
{% endhighlight %}
 
Finally, we plot everything. I more or less get what is happening here but honestly I am just sliding my data in where the example data used to be and then making a few aesthetic modifications. First, let's look at this thing and then we can break it down a bit.
 

{% highlight r %}
p <- ggplot(comp_table) + geom_segment(aes(x=1, xend=2, y=actual_order, yend=adjusted_order, col=class), size=.75, show.legend=F) + 
  geom_vline(xintercept=1, linetype="dashed", size=.1) + 
  geom_vline(xintercept=2, linetype="dashed", size=.1) +
  scale_color_manual(labels = c("Up", "Down", "Same"), 
                     values = c("green"="#00ba38", "red"="#f8766d", "gray"="#D3D3D3")) +  # color of lines
  labs(x="", y="", title = "Premier League Table with Away Goals Rule", subtitle = "Showing position, points and goal differential if league play included away goals rule") +  # Axis labels
  xlim(.5, 2.5) + 
  ylim(0,(1.1*(max(comp_table$actual_order, comp_table$adjusted_order)))) 
 
left_label <- paste(comp_table$team, comp_table$actual_points, comp_table$actual_gd,sep=", ")
right_label <- paste(comp_table$team, comp_table$adjusted_points, comp_table$adjusted_gd,sep=", ")
 
# Add texts
p <- p + geom_text(label=left_label, y=comp_table$actual_order, x=rep(1, NROW(comp_table)), hjust=1.1, size=3.5)
p <- p + geom_text(label=right_label, y=comp_table$adjusted_order, x=rep(2, NROW(comp_table)), hjust=-0.1, size=3.5)
p <- p + geom_text(label="Actual Results", x=1, y=1.1*(max(comp_table$actual_order, comp_table$adjusted_order)), hjust=1.2, size=5)  # title
p <- p + geom_text(label="Adjusted Results", x=2, y=1.1*(max(comp_table$actual_order, comp_table$adjusted_order)), hjust=-0.1, size=5)
 
p + theme(panel.background = element_blank(), 
          panel.grid = element_blank(),
          plot.title = element_text(hjust = 0.5),
          plot.subtitle = element_text(hjust = 0.5),
          axis.ticks = element_blank(),
          axis.text = element_blank(),
          axis.line = element_blank(),
          panel.border = element_blank())
{% endhighlight %}

![plot of chunk unnamed-chunk-11](/figures/unnamed-chunk-11-1.png)
 
So, here is quick rundown of more or less what is happening:
 
* `geom_segment` creates the line segments between two arbitrary x points and the y points for actual and adjusted positions.
 
* `geom_vline` creates the dotted lines at the arbitrary x points
 
* `scale_color_manual` creates the color key for the line segments
 
* labs and lims just add title text and set the size of the plot
 
* labels are the data to be included to identify each side of a segment
 
* `geom_text` adds some little headers above the labels
 
* finally theme is used to take out lots of stuff and give the plot a more minimal aesthetic
