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

## Q6 Most Active Users (Based on Number of Ratings)
```sql
SELECT u.user_id, COUNT(*) AS total_ratings
FROM ratings r
JOIN users u ON r.user_id = u.user_id
GROUP BY u.user_id
ORDER BY total_ratings DESC
LIMIT 10;
```
![Q6 Most Active Users (Based on Number of Ratings)](images/images/mubi_ss/Monthly%20Active%20Users%20Trend%20Based%20on%20Ratings.png)

## Q7 Newest vs. Oldest Movies in the Database
```sql
SELECT 
  (SELECT movie_title FROM movies ORDER BY movie_release_year DESC LIMIT 1) AS newest_movie,
  (SELECT movie_title FROM movies ORDER BY movie_release_year ASC LIMIT 1) AS oldest_movie;
```
![Q7 Newest vs. Oldest Movies in the Database](images/images/mubi_ss/Newest%20vs.%20Oldest%20Movies%20in%20the%20Database.png)

## Q8 Distribution of Ratings (1-5)
```sql
SELECT rating_score, COUNT(*) AS total_count
FROM ratings
GROUP BY rating_score
ORDER BY rating_score;
```
![Q8 Distribution of Ratings (1-5)](images/images/mubi_ss/Distribution%20of%20Ratings%20%281-5%29.png)

---

### ✅ Q9: Ratings Distribution (Score Buckets)

```sql
-- Your SQL query for "Ratings Distribution (Score Buckets)" goes here
```

![Ratings Distribution (Score Buckets)](images/images/mubi_ss/Ratings%20Distribution%20%28Score%20Buckets%29.png)

---

### ✅ Q10: Top 10 Highest-Rated Movies (min 100 ratings)

```sql
-- Your SQL query for "Top 10 Highest-Rated Movies (min 100 ratings)" goes here
```

![Top 10 Highest-Rated Movies (min 100 ratings)](images/images/mubi_ss/Top%2010%20Highest-Rated%20Movies%20%28min%20100%20ratings%29.png)

---

### ✅ Q11: Top 10 Lowest-Rated Movies with at least 100 ratings

```sql
-- Your SQL query for "Top 10 Lowest-Rated Movies with at least 100 ratings" goes here
```

![Top 10 Lowest-Rated Movies with at least 100 ratings](images/images/mubi_ss/Top%2010%20Lowest-Rated%20Movies%20with%20at%20least%20100%20ratings.png)

---

### ✅ Q12: Top Directors by Movie Count

```sql
-- Your SQL query for "Top Directors by Movie Count" goes here
```

![Top Directors by Movie Count](images/images/mubi_ss/Top%20Directors%20by%20Movie%20Count.png)

---

### ✅ Q13: total movies and total ratings

```sql
-- Your SQL query for "total movies and total ratings" goes here
```

![total movies and total ratings](images/images/mubi_ss/total%20movies%20and%20total%20ratings.png)

---

### ✅ Q14: User Retention Proxy (Users who returned to rate after 30 days)

```sql
-- Your SQL query for "User Retention Proxy (Users who returned to rate after 30 days)" goes here
```

![User Retention Proxy (Users who returned to rate after 30 days)](images/images/mubi_ss/User%20Retention%20Proxy%20%28Users%20who%20returned%20to%20rate%20after%2030%20days%29.png)

---

### ✅ Q15: Users who created lists vs rated movies

```sql
-- Your SQL query for "Users who created lists vs rated movies" goes here
```

![Users who created lists vs rated movies](images/images/mubi_ss/Users%20who%20created%20lists%20vs%20rated%20movies.png)

---

### ✅ Q16: Distribution of Ratings (1-5)

```sql
-- Your SQL query for "Distribution of Ratings (1-5)" goes here
```

![Distribution of Ratings (1-5)](images/images/mubi_ss/Distribution%20of%20Ratings%20%281-5%29.png)

