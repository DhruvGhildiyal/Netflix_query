# Netflix Movies and TV Shows Data Analysis using SQL

Netflix Movies and TV Shows Data Analysis using SQL
Project Overview
This project involves a comprehensive analysis of Netflix's movies and TV shows data using PostgreSQL. The goal is to extract valuable insights regarding content trends, directorial dominance, geographical distribution, and category breakdowns. This analysis provides a deep dive into the Netflix library to understand content evolution over the years.

Objectives
Content Distribution: Quantify the split between Movies and TV Shows.

Growth Trends: Identify the years and months with the highest content additions.

Geographical Insights: Determine which countries produce the most content.

Top Talent: Identify prolific directors and actors, specifically within the Indian market.

Content Categorization: Segment data based on release years and genres.

## 14 Business Problems & Solutions

### 1. Count the Number of Movies vs TV Shows

```sql
SELECT 
    type,
    COUNT(*) 
FROM netflix
GROUP BY 1;
```
Objective: Understand the distribution of content types available on the platform.

---

### 2. List All Movies Released in a Specific Year (e.g., 2020)

```sql
SELECT * 
FROM netflix
WHERE release_year = 2020 
  AND type = 'Movie';
```
Objective: Retrieve all movies released in a specific year to analyze content freshness.

---

### 3. Find the Top 5 Countries with the Most Content on Netflix

```sql
SELECT 
    UNNEST(STRING_TO_ARRAY(country, ',')) as new_country,
    COUNT(show_id) as total_content 
FROM netflix
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```
Objective: Identify the top 5 countries contributing the most content to the library.

---

### 4. Identify the Longest Movie

```sql
SELECT * 
FROM netflix
WHERE type = 'Movie' 
  AND duration = (SELECT MAX(duration) FROM netflix);
```
Objective: Find the single movie with the longest runtime across the entire dataset.

---

### 5. List all "TV Shows" Produced in "India"

```sql
SELECT 
    title, 
    country 
FROM netflix
WHERE type = 'TV Show' 
  AND country = 'India';
```
Objective: Filter content specifically produced in the Indian market.

---

### 6. Find all Content Directed by 'Kirsten Johnson'

```sql
SELECT * 
FROM netflix 
WHERE director ILIKE '%Kirsten Johnson%';
```
Objective: Retrieve the full portfolio of a specific director.

---

### 7. List all Movies Categorized as 'Documentaries'

```sql
SELECT * 
FROM netflix
WHERE type = 'Movie' 
  AND listed_in ILIKE '%Documentaries%';
```
Objective: Filter the dataset for documentary genre.

---

### 8. Total Number of Content Items for Each Rating

```sql
SELECT 
    rating, 
    COUNT(*) as content 
FROM netflix
GROUP BY 1 
ORDER BY 2 DESC;
```
Objective: Identify the most common content ratings.

---

### 9. List All Shows Added in the Month of 'September'

```sql
SELECT 
    title, 
    date_added 
FROM netflix
WHERE date_added ILIKE '%September%';
```
Objective: Analyze the timing of content additions.

---

### 10. Identify Directors with More Than 10 Movies or Shows

```sql
SELECT 
    director,
    COUNT(*) as total_content 
FROM netflix
WHERE director IS NOT NULL
GROUP BY 1
HAVING COUNT(*) >= 10
ORDER BY 2 DESC;
```
Objective: Highlight prolific directors.

---

### 11. Categorize Content into 'Old' and 'New'

```sql
SELECT 
    title,
    CASE 
        WHEN release_year <= 2010 THEN 'Old'
        ELSE 'New'
    END as category 
FROM netflix;
```
Objective: Group content based on release year.

---

### 12. Find the Year with the Most Content Added

```sql
SELECT 
    TRIM(SPLIT_PART(date_added, ',', 2)) as year_added, 
    COUNT(*) as total_content_added 
FROM netflix
WHERE date_added IS NOT NULL
GROUP BY 1 
ORDER BY 2 DESC
LIMIT 1;
```
Objective: Identify peak content addition year.

---

### 13. Average Release Year for Each Content Type

```sql
SELECT 
    type, 
    ROUND(AVG(release_year), 2) as avg_release_year
FROM netflix
GROUP BY 1;
```
Objective: Calculate average release year by type.

---

### 14. Top 10 Actors with Most Content in India

```sql
SELECT 
    UNNEST(STRING_TO_ARRAY(casts, ',')) as actors, 
    COUNT(*) as projects 
FROM netflix
WHERE country = 'India'
GROUP BY 1 
ORDER BY 2 DESC
LIMIT 10;
```
Objective: Identify the most prominent actors in the Indian film and television industry on Netflix.

---

###Findings and Conclusion

Content Dominance: The library is heavily weighted toward Movies, though TV Shows represent a significant and growing portion of the platform.

Regional Leaders: The United States and India emerge as the top content producers, reflecting Netflix's strategy of balancing Hollywood global appeal with strong regional (Bollywood) presence.

Release Trends: The average release year for both movies and shows is quite recent (post-2010), indicating that Netflix prioritizes newer content to keep subscribers engaged.

Director Influence: A small group of directors has contributed significantly to the platform, suggesting recurring partnerships between Netflix and specific creators.

Audience Targeting: The majority of content is rated for "Mature" audiences (TV-MA), followed by "Teen" audiences (TV-14), showing a focus on adult-oriented storytelling.
