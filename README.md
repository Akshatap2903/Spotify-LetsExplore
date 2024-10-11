# Spotify Advanced SQL Project with Query Optimization
![Project Logo](https://github.com/user-attachments/assets/167a8eca-448c-4c6e-9998-ff7869a884c4)

## Overview
This project involves analyzing a Spotify dataset with various attributes about tracks, albums, and artists using **SQL**. It covers an end-to-end process of normalizing a denormalized dataset, performing SQL queries of varying complexity (easy, medium, and advanced), and optimizing query performance. The primary goals of the project are to practice advanced SQL skills and generate valuable insights from the dataset.

```sql
-- create table
DROP TABLE IF EXISTS spotify;
CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes BIGINT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);
```
## Project Steps

### 1. Data Exploration
Before diving into SQL, it’s important to understand the dataset thoroughly. The dataset contains attributes such as:
- `Artist`: The performer of the track.
- `Track`: The name of the song.
- `Album`: The album to which the track belongs.
- `Album_type`: The type of album (e.g., single or album).
- Various metrics such as `danceability`, `energy`, `loudness`, `tempo`, and more.

### 2. Querying the Data
After the data is inserted, various SQL queries can be written to explore and analyze the data. Queries are categorized into **easy**, **medium**, and **advanced** levels to help progressively develop SQL proficiency.

#### Easy Queries
- Simple data retrieval, filtering, and basic aggregations.
  
#### Medium Queries
- More complex queries involving grouping, aggregation functions, and joins.
  
#### Advanced Queries
- Nested subqueries, window functions, CTEs, and performance optimization.

### 3. Query Optimization
In advanced stages, the focus shifts to improving query performance. Some optimization strategies include:
- **Indexing**: Adding indexes on frequently queried columns.
- **Query Execution Plan**: Using `EXPLAIN ANALYZE` to review and refine query performance.
  
---

## 15 Practice Questions

### Easy Level

1. Retrieve the names of all tracks that have more than 1 billion streams. 
```sql
SELECT*FROM spotify WHERE streams>100000000
```
2. List all albums along with their respective artists.
```sql
SELECT album,artist FROM spotify
```
3. Get the total number of comments for tracks where `licensed = TRUE`.
```sql
SELECT COUNT(comments) as tot_comments FROM spotify
WHERE licensed='true'
```
4. Find all tracks that belong to the album type `single`.
```sql
SELECT* FROMrom spotify
WHERE type='single'
```
5. Count the total number of tracks by each artist.
```sql
SELECT artist,COUNT(tracks) as tot_tracks
FROM spotify
GROUP BY artist
ORDER BY artist
```
### Medium Level
1. Calculate the average danceability of tracks in each album.
```sql
SELECT album, AVG(danceability) AS avg_danceability
FROM spotify
GROUP BY album;
```
2. Find the top 5 tracks with the highest energy values.
```sql
SELECT track, artist, views, likes
FROM spotify
WHERE official_video = TRUE;
```
3. List all tracks along with their views and likes where `official_video = TRUE`.
```sql
SELECT track, artist, views, likes
FROM spotify
WHERE official_video = TRUE;
```
4. For each album, calculate the total views of all associated tracks.
```sql
SELECT album, SUM(views) AS total_views
FROM spotify
GROUP BY album;

```
5. Retrieve the track names that have been streamed on Spotify more than YouTube.
```sql
SELECT track, artist
FROM spotify
WHERE stream > views;

```
### Advanced Level
1. **Find the top 3 most-viewed tracks for each artist using window functions.**
```sql
SELECT artist, track, views
FROM (
    SELECT artist, track, views,
           ROW_NUMBER() OVER (PARTITION BY artist ORDER BY views DESC) AS rank
    FROM spotify
) ranked_tracks
WHERE rank <= 3;
```
2. **Write a query to find tracks where the liveness score is above the average.**
```sql
SELECT track, artist, liveness
FROM spotify
WHERE liveness > (SELECT AVG(liveness) FROM spotify);
```
3. **Use a `WITH` clause to calculate the difference between the highest and lowest energy values for tracks in each album.**
```sql
WITH cte
AS
(SELECT 
	album,
	MAX(energy) as highest_energy,
	MIN(energy) as lowest_energery
FROM spotify
GROUP BY 1
)
SELECT 
	album,
	highest_energy - lowest_energery as energy_diff
FROM cte
ORDER BY 2 DESC
```
   
5. Find tracks where the energy-to-liveness ratio is greater than 1.2.
```sql
SELECT track, artist, energy, liveness, (energy / liveness) AS energy_to_liveness_ratio
FROM spotify
WHERE (energy / liveness) > 1.2;
```
6. Calculate the cumulative sum of likes for tracks ordered by the number of views, using window functions.
```sql
SELECT track, artist, views, likes,
       SUM(likes) OVER (ORDER BY views) AS cumulative_likes
FROM spotify
ORDER BY views;
```

Here’s an updated section for my **Spotify Advanced SQL Project** README, focusing on the query optimization task I performed. I added specific screenshots after applying optimization techniques.

---

## Query Optimization Technique 

To improve query performance, I carried out the following optimization process:

- **Initial Query Performance Analysis Using `EXPLAIN`**
    - I began by analyzing the performance of a query using the `EXPLAIN` function.
    - The query retrieved tracks based on the `artist` column, and the performance metrics were as follows:
        - Execution time (E.T.): **7 ms**
        - Planning time (P.T.): **0.17 ms**
    - Below is the **screenshot** of the `EXPLAIN` result before optimization:
    - ![Screenshot 2024-10-12 004148](https://github.com/user-attachments/assets/943737f6-9e66-4351-ac33-88e79add0c86)

- **Index Creation on the `artist` Column**
    - To optimize the query performance, I created an index on the `artist` column. This ensures faster retrieval of rows where the artist is queried.
    - **SQL command** for creating the index:
      ```sql
      CREATE INDEX idx_artist ON spotify_tracks(artist);
      ```

- **Performance Analysis After Index Creation**
    - After creating the index, we ran the same query again and observed significant improvements in performance:
        - Execution time (E.T.): **0.153 ms**
        - Planning time (P.T.): **0.152 ms**
    - Below is the **screenshot** of the `EXPLAIN` result after index creation:
![Screenshot 2024-10-12 004308](https://github.com/user-attachments/assets/3c7c3e2b-4168-41a8-8486-ab058489b7b5)

  

This optimization shows how indexing can drastically reduce query time, improving the overall performance of our database operations in the Spotify project.
---

## Technology Stack
- **Database**: PostgreSQL
- **SQL Queries**: DDL, DML, Aggregations, Joins, Subqueries, Window Functions
- **Tools**: pgAdmin 4 (or any SQL editor), PostgreSQL

---
## Future Enhancements
- **More Data Analysis**: Add additional metrics like user engagement and popularity trends based on views and likes.
- **Integration with Streaming APIs**: Connect the dataset with real-time data from Spotify or YouTube APIs for more dynamic analysis.
- **Performance Optimization**: Experiment with further optimization techniques such as query caching and partitioning to handle large datasets more efficiently.
- **Visualization**: Implement data visualization tools like Tableau or Power BI to visually represent trends from the data.
## Contact
If you have any questions or suggestions, feel free to contact me:
- **Email**: akshatp108@gmail.com
- **LinkedIn**: https://www.linkedin.com/in/akshata-pawar-29march/
## Acknowledgements
- [PostgreSQL](https://www.postgresql.org) for the database management system.
- [pgAdmin](https://www.pgadmin.org/) for providing a great interface for database management.
- The open-source community for providing useful tools and resources.

