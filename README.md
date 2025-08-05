# MUBI-Movies-Analysis-using-SQL
Data Analysis from the MUBI movies data using SQL queries
## Q1 total movies and total ratings
```sql
SELECT 
  (SELECT COUNT(*) FROM movies) AS total_movies,
  (SELECT COUNT(*) FROM ratings) AS total_ratings;
```
![Q1 total movies and total ratings](images/images/mubi_ss/total%20movies%20and%20total%20ratings.png)

## Q2 Top 10 Highest-Rated Movies (min 100 ratings)
```sql
SELECT m.movie_title, 
       ROUND(AVG(r.rating_score), 2) AS avg_rating, 
       COUNT(*) AS rating_count
FROM movies m
JOIN ratings r ON m.movie_id = r.movie_id
GROUP BY m.movie_id
HAVING COUNT(*) >= 100
ORDER BY avg_rating DESC
LIMIT 10;
```
![Q2 Top 10 Highest-Rated Movies (min 100 ratings)](images/images/mubi_ss/Top%2010%20Highest-Rated%20Movies%20%28min%20100%20ratings%29.png)

## Q3 Most-Watched Movies
```sql
SELECT m.movie_title,
       COUNT(*) AS watch_count
FROM movies m
JOIN ratings r ON m.movie_id = r.movie_id
GROUP BY m.movie_id
ORDER BY watch_count DESC
LIMIT 10;
```
![Q3 Most-Watched Movies](images/images/mubi_ss/Most-Watched%20Movies.png)

## Q4 Movies with the Most 5-Star Ratings
```sql
SELECT m.movie_title, COUNT(*) AS five_star_count
FROM movies m
JOIN ratings r ON m.movie_id = r.movie_id
WHERE r.rating_score = 5
GROUP BY m.movie_id
ORDER BY five_star_count DESC
LIMIT 10;
```
![Q4 Movies with the Most 5-Star Ratings](images/images/mubi_ss/Movies%20with%20the%20Most%205%20Star%20Ratings.png)

## Q5 Lowest Rated Movies (with at least 50 ratings)
```sql
SELECT m.movie_title, ROUND(AVG(r.rating_score), 2) AS avg_rating, COUNT(*) AS rating_count
FROM ratings r
JOIN movies m ON r.movie_id = m.movie_id
GROUP BY m.movie_id
HAVING COUNT(*) >= 50
ORDER BY avg_rating ASC
LIMIT 10;
```
![Q5 Lowest Rated Movies (with at least 50 ratings)](images/images/mubi_ss/Top%2010%20Lowest-Rated%20Movies%20with%20at%20least%20100%20ratings.png)

## Q6 Distribution of Ratings (1-5)

```sql
SELECT rating_score, COUNT(*) AS total_count
FROM ratings
GROUP BY rating_score
ORDER BY rating_score;```
```

![Distribution of Ratings (1-5)](images/images/mubi_ss/Distribution%20of%20Ratings%20%281-5%29.png)

---

## Q7 Newest vs. Oldest Movies in the Database
```sql
SELECT 
  (SELECT movie_title FROM movies ORDER BY movie_release_year DESC LIMIT 1) AS newest_movie,
  (SELECT movie_title FROM movies ORDER BY movie_release_year ASC LIMIT 1) AS oldest_movie;
```
![Q7 Newest vs. Oldest Movies in the Database](images/images/mubi_ss/Newest%20vs.%20Oldest%20Movies%20in%20the%20Database.png)

## Q8 Users who created lists vs rated movies

```sql
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
```

![Users who created lists vs rated movies](images/images/mubi_ss/Users%20who%20created%20lists%20vs%20rated%20movies.png)

---


### ✅ Q9: Ratings Distribution (Score Buckets)

```sql
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
    rating_bucket;```

![Ratings Distribution (Score Buckets)](images/images/mubi_ss/Ratings%20Distribution%20%28Score%20Buckets%29.png)

---

### ✅ Q10: Top 10 Highest-Rated Movies (min 100 ratings)

```sql
SELECT m.movie_title, 
       ROUND(AVG(r.rating_score), 2) AS avg_rating, 
       COUNT(*) AS rating_count
FROM movies m
JOIN ratings r ON m.movie_id = r.movie_id
GROUP BY m.movie_id
HAVING COUNT(*) >= 100
ORDER BY avg_rating DESC
LIMIT 10;
```

![Top 10 Highest-Rated Movies (min 100 ratings)](images/images/mubi_ss/Top%2010%20Highest-Rated%20Movies%20%28min%20100%20ratings%29.png)

---

### ✅ Q11: Top 10 Lowest-Rated Movies with at least 100 ratings

```sql
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
```

![Top 10 Lowest-Rated Movies with at least 100 ratings](images/images/mubi_ss/Top%2010%20Lowest-Rated%20Movies%20with%20at%20least%20100%20ratings.png)

---

### ✅ Q12: Top Directors by Movie Count

```sql
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

```

![Top Directors by Movie Count](images/images/mubi_ss/Top%20Directors%20by%20Movie%20Count.png)

---

### ✅ Q13: total movies and total ratings

```sql
SELECT 
  (SELECT COUNT(*) FROM movies) AS total_movies,
  (SELECT COUNT(*) FROM ratings) AS total_ratings;```

![total movies and total ratings](images/images/mubi_ss/total%20movies%20and%20total%20ratings.png)

---

### ✅ Q14: User Retention Proxy (Users who returned to rate after 30 days)

```sql
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
```

![User Retention Proxy (Users who returned to rate after 30 days)](images/images/mubi_ss/User%20Retention%20Proxy%20%28Users%20who%20returned%20to%20rate%20after%2030%20days%29.png)

---
