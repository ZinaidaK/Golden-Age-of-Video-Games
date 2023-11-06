### Our database contains two tables. We've limited each table to 400 rows for this project, but you can find the complete dataset with over 13,000 games on Kaggle.https://www.kaggle.com/datasets/holmjason2/videogamedata


![Screenshot 2023-11-05 195620](https://github.com/ZinaidaK/Golden-Age-of-Video-Games/assets/100050035/051edbf5-d400-43a0-bc6b-103f1267f0c5)


### 1. The ten best-selling video games

*--Select all information for the top ten best-selling games*
*-- Order the results from best-selling game down to tenth best-selling*

SELECT * 

FROM game_sales

ORDER BY games_sold DESC

LIMIT 10;

*10 rows affected.*

### 2. Missing review scores

*-- Join games_sales and reviews*

*-- Select a count of the number of games where both critic_score and user_score are null*

SELECT COUNT(g.game)

FROM game_sales AS g

LEFT JOIN reviews AS r

ON g.game = r.game

WHERE critic_score IS NULL AND user_score IS NULL


![Screenshot 2023-11-05 200505](https://github.com/ZinaidaK/Golden-Age-of-Video-Games/assets/100050035/c53b16ad-df6c-4a40-88d6-778d9a54094a)


### 3. Years that video game critics loved

*-- Select release year and average critic score for each year, rounded and aliased*

*-- Join the game_sales and reviews tables*

*-- Group by release year*

*-- Order the data from highest to lowest avg_critic_score and limit to 10 results*
  
SELECT g.year, ROUND(AVG(r.critic_score),2) AS avg_critic_score  

FROM game_sales AS g

INNER JOIN reviews AS r   

ON g.game = r.game

GROUP BY g.year   

ORDER BY avg_critic_score DESC 

LIMIT 10;

![Screenshot 2023-11-05 200846](https://github.com/ZinaidaK/Golden-Age-of-Video-Games/assets/100050035/58ea8f5e-e776-4fda-a5bc-47fdcb402df8)


### 4. Was 1982 really that great?
*-- Select release year and average critic score for each year, rounded and aliased*

  *-- Join the game_sales and reviews tables*
  
  *-- Group by release year*
  
  *-- Order the data from highest to lowest avg_critic_score and limit to 10 results* 
  
  
SELECT g.year, COUNT(g.game) AS num_games, ROUND(AVG(r.critic_score),2) AS avg_critic_score 

FROM game_sales AS g

INNER JOIN reviews AS r  

ON g.game = r.game

GROUP BY g.year       

HAVING COUNT(g.game) > 4

ORDER BY avg_critic_score DESC  

LIMIT 10;

10 rows affected.


### 5. Years that dropped off the critics' favorites list

That looks better! The num_games column convinces us that our new list of the critics' top games reflects years that had quite a few well-reviewed games rather than just one or two hits. But which years dropped off the list due to having four or fewer reviewed games? Let's identify them so that someday we can track down more game reviews for those years and determine whether they might rightfully be considered as excellent years for video game releases!

It's time to brush off your set theory skills. To get started, we've created tables with the results of our previous two queries:


![Screenshot 2023-11-05 202524](https://github.com/ZinaidaK/Golden-Age-of-Video-Games/assets/100050035/6d7fecf7-cb28-4422-8acb-f19ac9167247)


SELECT year, avg_critic_score 

FROM top_critic_years

EXCEPT

SELECT year, avg_critic_score

FROM top_critic_years_more_than_four_games

ORDER BY avg_critic_score

![Screenshot 2023-11-05 202704](https://github.com/ZinaidaK/Golden-Age-of-Video-Games/assets/100050035/08bd7187-7fe6-49de-a776-809075388ff7)

### 6. Years video game players loved

SELECT g.year, ROUND(AVG(r.user_score),2) AS avg_user_score, COUNT(g.game) AS num_games

FROM game_sales AS g

LEFT JOIN reviews AS r

ON g.game = r.game

GROUP BY g.year

HAVING COUNT(g.game) >4

ORDER BY avg_user_score DESC

LIMIT 10;

*10 rows affected.*

### 7. Years that both players and critics loved

SELECT year

FROM top_critic_years_more_than_four_games

INTERSECT

SELECT year

FROM top_user_years_more_than_four_games

*-- Select the year results that appear on both tables*

year

1998

2008

2002


### 8. Sales in the best video game years

SELECT year, SUM(games_sold) AS total_games_sold

FROM game_sales

WHERE year IN 

(

SELECT year

FROM top_critic_years_more_than_four_games

INTERSECT

SELECT year

FROM top_user_years_more_than_four_games

)

GROUP BY year

ORDER BY total_games_sold DESC;


*3 rows affected.*


![Screenshot 2023-11-05 203554](https://github.com/ZinaidaK/Golden-Age-of-Video-Games/assets/100050035/7b8f3310-af69-4021-97a2-02fb4144145d)
