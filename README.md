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

Business Problems and Solutions
1. Count the Number of Movies vs TV Shows
Objective: Understand the balance of content types on the platform.

SQL
SELECT type, COUNT(*) 
FROM netflix
GROUP BY type;
2. List All Movies Released in a Specific Year (2020)
Objective: Filter content for targeted year-over-year analysis.

SQL
SELECT * FROM netflix
WHERE release_year = 2020 AND type = 'Movie';
3. Top 5 Countries with the Most Content
Objective: Identify the primary geographical markets for Netflix content.
(Note: Using UNNEST and STRING_TO_ARRAY to handle multiple countries listed in a single row).

SQL
SELECT 
    UNNEST(STRING_TO_ARRAY(country, ',')) AS new_country,
    COUNT(show_id) AS content 
FROM netflix
GROUP BY 1
ORDER BY content DESC
LIMIT 5;
4. Identify the Longest Movie
Objective: Find the maximum runtime among movies.

SQL
SELECT * FROM netflix
WHERE type = 'Movie' 
AND duration = (SELECT MAX(duration) FROM netflix);
5. TV Shows Produced in India
Objective: Filter content by a specific regional market.

SQL
SELECT title, country FROM netflix
WHERE type = 'TV Show' AND country = 'India';
6. Content Directed by 'Kirsten Johnson'
Objective: Retrieve the portfolio of a specific director.

SQL
SELECT * FROM netflix 
WHERE director ILIKE '%Kirsten Johnson%';
7. Documentaries Category
Objective: Perform a keyword search for specific genres.

SQL
SELECT * FROM netflix
WHERE type = 'Movie' AND listed_in ILIKE '%Documentaries%';
8. Content Distribution by Rating
Objective: Understand the target audience demographics based on ratings.

SQL
SELECT rating, COUNT(*) AS content 
FROM netflix
GROUP BY rating 
ORDER BY content DESC;
9. Content Added in September
Objective: Analyze seasonal upload patterns.

SQL
SELECT title, date_added FROM netflix
WHERE date_added ILIKE '%September%';
10. Prolific Directors (>10 Titles)
Objective: Identify key creators with a significant volume of work.

SQL
SELECT director, COUNT(*) AS Total_content 
FROM netflix
WHERE director IS NOT NULL
GROUP BY 1
HAVING COUNT(*) >= 10
ORDER BY 2 DESC;
11. Content Age Categorization
Objective: Classify content into "Old" and "New" based on the release year.

SQL
SELECT title,
CASE 
    WHEN release_year < 2010 THEN 'Old'
    ELSE 'New'
END AS category 
FROM netflix;
12. Peak Year for Content Additions
Objective: Identify the year Netflix expanded its library most aggressively.

SQL
SELECT TRIM(SPLIT_PART(date_added, ',', 2)) AS year_added, 
       COUNT(*) AS total_content_added 
FROM netflix
WHERE date_added IS NOT NULL
GROUP BY 1 
ORDER BY 2 DESC
LIMIT 1;
13. Average Release Year per Content Type
Objective: Compare the "freshness" or vintage nature of movies vs. shows.

SQL
SELECT type, ROUND(AVG(release_year), 2) AS avg_release_year
FROM netflix
GROUP BY 1;
14. Top 10 Indian Actors
Objective: Identify the most frequently appearing actors in Indian productions.

SQL
SELECT UNNEST(STRING_TO_ARRAY(casts, ',')) AS Actors, 
       COUNT(*) AS Projects 
FROM netflix
WHERE country = 'India'
GROUP BY 1 
ORDER BY 2 DESC
LIMIT 10;
Findings and Conclusion
Content Dominance: The library is heavily weighted toward Movies, though TV Shows represent a significant and growing portion of the platform.

Regional Leaders: The United States and India emerge as the top content producers, reflecting Netflix's strategy of balancing Hollywood global appeal with strong regional (Bollywood) presence.

Release Trends: The average release year for both movies and shows is quite recent (post-2010), indicating that Netflix prioritizes newer content to keep subscribers engaged.

Director Influence: A small group of directors has contributed significantly to the platform, suggesting recurring partnerships between Netflix and specific creators.

Audience Targeting: The majority of content is rated for "Mature" audiences (TV-MA), followed by "Teen" audiences (TV-14), showing a focus on adult-oriented storytelling.
