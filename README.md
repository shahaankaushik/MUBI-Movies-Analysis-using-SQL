# MUBI-Movies-Analysis-using-SQL
Data Analysis from the MUBI movies data using SQL queries


Q1 total movies and total ratings

SELECT 
  (SELECT COUNT(*) FROM movies) AS total_movies,
  (SELECT COUNT(*) FROM ratings) AS total_ratings;


![total movies and total ratings](images/images/mubi_ss/total%20movies%20and%20total%20ratings.png)



Q2 Top 10 Highest-Rated Movies (min 100 ratings)

SELECT m.movie_title, 
       ROUND(AVG(r.rating_score), 2) AS avg_rating, 
       COUNT(*) AS rating_count
FROM movies m
JOIN ratings r ON m.movie_id = r.movie_id
GROUP BY m.movie_id
HAVING COUNT(*) >= 100
ORDER BY avg_rating DESC
LIMIT 10;


![Top 10 Highest-Rated Movies (min 100 ratings)](images/images/mubi_ss/Top%2010%20Highest-Rated%20Movies%20(min%20100%20ratings).png)


Q3 Most-Watched Movies

SELECT m.movie_title,
       COUNT(*) AS watch_count
FROM movies m
JOIN ratings r ON m.movie_id = r.movie_id
GROUP BY m.movie_id
ORDER BY watch_count DESC
LIMIT 10;


![Most-Watched Movies](images/images/mubi_ss/Most-Watched%20Movies.png)


Q4 Movies with the Most 5-Star Ratings

SELECT m.movie_title, COUNT(*) AS five_star_count
FROM movies m
JOIN ratings r ON m.movie_id = r.movie_id
WHERE r.rating_score = 5
GROUP BY m.movie_id
ORDER BY five_star_count DESC
LIMIT 10;

![Movies with the Most 5 Star Ratings](images/images/mubi_ss/Movies%20with%20the%20Most%205%20Star%20Ratings.png)


Q5 Lowest Rated Movies (with at least 50 ratings)

SELECT m.movie_title, ROUND(AVG(r.rating_score), 2) AS avg_rating, COUNT(*) AS rating_count
FROM ratings r
JOIN movies m ON r.movie_id = m.movie_id
GROUP BY m.movie_id
HAVING COUNT(*) >= 50
ORDER BY avg_rating ASC
LIMIT 10;

![Top 10 Lowest-Rated Movies with at least 100 ratings](images/images/mubi_ss/Top%2010%20Lowest-Rated%20Movies%20with%20at%20least%20100%20ratings.png)


Q6 Most Active Users (Based on Number of Ratings)

SELECT u.user_id, COUNT(*) AS total_ratings
FROM ratings r
JOIN users u ON r.user_id = u.user_id
GROUP BY u.user_id
ORDER BY total_ratings DESC
LIMIT 10;

![Q6](images/images/mubi_ss/Monthly%20Active%20Users%20Trend%20Based%20on%20Ratings.png)



Q7 Newest vs. Oldest Movies in the Database

SELECT 
  (SELECT movie_title FROM movies ORDER BY movie_release_year DESC LIMIT 1) AS newest_movie,
  (SELECT movie_title FROM movies ORDER BY movie_release_year ASC LIMIT 1) AS oldest_movie;

![Newest vs. Oldest Movies in the Database](images/images/mubi_ss/Newest%20vs.%20Oldest%20Movies%20in%20the%20Database.png)


Q8 Distribution of Ratings (1-5)

SELECT rating_score, COUNT(*) AS total_count
FROM ratings
GROUP BY rating_score
ORDER BY rating_score;

![Distribution of Ratings (1-5)](images/images/mubi_ss/Distribution%20of%20Ratings%20(1-5).png)



Q9 Distribution of Movies by Decade(Post 1970)

SELECT 
  CASE 
    WHEN movie_release_year BETWEEN 1970 AND 1979 THEN '1970–1979'
    WHEN movie_release_year BETWEEN 1980 AND 1989 THEN '1980–1989'
    WHEN movie_release_year BETWEEN 1990 AND 1999 THEN '1990–1999'
    WHEN movie_release_year BETWEEN 2000 AND 2009 THEN '2000–2009'
    WHEN movie_release_year BETWEEN 2010 AND 2019 THEN '2010–2019'
    WHEN movie_release_year BETWEEN 2020 AND 2029 THEN '2020–2029'
    ELSE 'Unknown'
  END AS decade,
  COUNT(*) AS movies_count
FROM movies
WHERE movie_release_year >= 1970
GROUP BY decade
ORDER BY decade;

![Distribution of Movies by Decade(Post 1970)](images/images/mubi_ss/Distribution%20of%20Movies%20by%20Decade(Post%201970).png)


Q10 Top 10 Lowest-Rated Movies with at least 100 ratings:

SELECT 
    m.movie_title, 
    ROUND(AVG(r.rating_score), 2) AS avg_rating, 
    COUNT(r.rating_score) AS rating_count
FROM 
    ratings r
JOIN 
    movies m ON r.movie_id = m.movie_id
GROUP BY 
    r.movie_id
HAVING 
    COUNT(r.rating_score) >= 100
ORDER BY 
    avg_rating ASC
LIMIT 10;

![Q10 Output](images/Top 10 Lowest-Rated Movies with at least 100 ratings:.png)



Q11 User Engagement Funnel
Users who created lists vs rated movies

WITH list_users AS (
    SELECT DISTINCT user_id FROM user_lists
),
rating_users AS (
    SELECT DISTINCT user_id FROM ratings
)
SELECT 
    'Created Lists' AS activity, COUNT(*) FROM list_users
UNION ALL
SELECT 
    'Rated Movies' AS activity, COUNT(*) FROM rating_users;


![Users who created lists vs rated movies](images/images/mubi_ss/Users%20who%20created%20lists%20vs%20rated%20movies.png)


    

Q12 Ratings Distribution (Score Buckets)

SELECT 
    CASE 
        WHEN rating_score BETWEEN 0 AND 2 THEN '0-2'
        WHEN rating_score BETWEEN 2.1 AND 4 THEN '2.1-4'
        WHEN rating_score BETWEEN 4.1 AND 6 THEN '4.1-6'
        WHEN rating_score BETWEEN 6.1 AND 8 THEN '6.1-8'
        ELSE '8.1-10'
    END AS rating_bucket,
    COUNT(*) AS num_ratings
FROM 
    ratings
GROUP BY 
    rating_bucket
ORDER BY 
    rating_bucket;


![Ratings Distribution (Score Buckets)](images/images/mubi_ss/Ratings%20Distribution%20(Score%20Buckets).png)

    

Q13 Most Polarizing Movies (High Std Dev in Ratings)

WITH rating_stats AS (
    SELECT 
        r.movie_id,
        m.movie_title,
        COUNT(r.rating_score) AS num_ratings,
        AVG(r.rating_score) AS avg_rating,
        SUM(r.rating_score * r.rating_score) AS sum_sq
    FROM 
        ratings r
    JOIN 
        movies m ON r.movie_id = m.movie_id
    GROUP BY 
        r.movie_id
    HAVING 
        num_ratings >= 100
)
SELECT 
    movie_title,
    avg_rating,
    ROUND(
        SQRT((sum_sq - num_ratings * avg_rating * avg_rating) / num_ratings), 
        2
    ) AS stddev_rating,
    num_ratings
FROM 
    rating_stats
ORDER BY 
    stddev_rating DESC
LIMIT 10;


![Most Polarizing Movies (High Std Dev in Ratings)](images/images/mubi_ss/Most%20Polarizing%20Movies%20(High%20Std%20Dev%20in%20Ratings).png)



Q14 User Retention Proxy (Users who returned to rate after 30 days)

WITH user_activity AS (
  SELECT 
    user_id,
    MIN(DATE(rating_timestamp_utc)) AS first_rating,
    MAX(DATE(rating_timestamp_utc)) AS last_rating
  FROM 
    ratings
  GROUP BY user_id
)
SELECT 
    COUNT(*) AS users_retained,
    ROUND(100.0 * COUNT(*) / (SELECT COUNT(*) FROM user_activity), 2) AS retention_rate
FROM 
    user_activity
WHERE 
    JULIANDAY(last_rating) - JULIANDAY(first_rating) >= 30;


![User Retention Proxy (Users who returned to rate after 30 days)](images/images/mubi_ss/User%20Retention%20Proxy%20(Users%20who%20returned%20to%20rate%20after%2030%20days).png)




Q15 Detect Bot-like Behavior: Users Who Rated 100+ Movies in 1 Day

SELECT 
    r.user_id,
    COUNT(*) AS ratings_in_a_day,
    DATE(r.rating_timestamp_utc) AS rating_date
FROM 
    ratings r
GROUP BY 
    r.user_id, rating_date
HAVING 
    ratings_in_a_day >= 100
ORDER BY 
    ratings_in_a_day DESC;


![Detect Bot Users](images/images/mubi_ss/Detect%20Bot-like%20Behavior%20Users%20Who%20Rated%20100%2B%20Movies%20in%201%20Day.png)




Q16 Top Directors by Movie Count

SELECT 
    REPLACE(SUBSTR(director_url, INSTR(director_url, '/cast/') + 6), '-', ' ') AS director_name,
    COUNT(*) AS movie_count
FROM 
    movies
WHERE 
    director_url IS NOT NULL 
    AND director_url != ''
    AND director_url LIKE '%/cast/%'
    AND TRIM(SUBSTR(director_url, INSTR(director_url, '/cast/') + 6)) != 'unknown'
GROUP BY 
    director_name
ORDER BY 
    movie_count DESC
LIMIT 10;


![Top Directors by Movie Count](images/images/mubi_ss/Top%20Directors%20by%20Movie%20Count.png)
