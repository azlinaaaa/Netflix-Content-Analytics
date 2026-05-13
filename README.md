# 🎬 Netflix Content Analytics & Business Intelligence Project

## 📌 Project Overview

This project focuses on building a complete SQL-based Business Intelligence solution using Netflix content data. The project simulates how to use SQL analytics to transform raw datasets into strategic business insights for decision-making.

The project demonstrates real-world SQL workflows including:

- Database Design
- Data Import & Validation
- Data Cleaning
- KPI Analytics
- Business Intelligence Reporting
- Advanced SQL Analytics
- Window Functions
- Stored Procedures
- Temporary Tables
- Views & Reporting Structures

The objective of this project is to showcase strong analytical, technical, and problem-solving capabilities

---

# 🏢 Business Problem

Streaming platforms such as Netflix continuously analyze massive amounts of content data to answer critical business questions:

- Which countries contribute the most content?
- What content type dominates the platform?
- Which directors produce the highest number of titles?
- How has content growth evolved over time?
- Which audience ratings dominate the platform?
- How can content be segmented strategically?

This project simulates how data teams support business stakeholders using SQL-driven analytics.

---

# 🎯 Project Objectives

- Build a structured relational database using MySQL
- Import and validate large datasets
- Perform data cleaning and quality checks
- Generate business KPIs using SQL
- Apply advanced SQL analytical concepts
- Create executive-level reporting outputs
- Simulate real-world Business Intelligence workflows

---

# 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| MySQL Workbench | Database Development |
| SQL | Data Querying & Analytics |
| CSV Dataset | Raw Data Source |
| GitHub | Project Repository |
| Window Functions | Advanced Ranking Analytics |
| Views & Procedures | Reporting Automation |

---

# 📂 Dataset Information

Dataset: Netflix Movies & TV Shows Dataset

Main attributes include:

- Show ID
- Title
- Type
- Director
- Cast
- Country
- Release Year
- Date Added
- Rating
- Duration
- Genre
- Description

---

# 🗄️ Database Creation

## Create Database

```sql
CREATE DATABASE netflix_db;
USE netflix_db;
```

---

## Create Main Table

```sql
CREATE TABLE IF NOT EXISTS netflix_titles (
    show_id VARCHAR(10) PRIMARY KEY,
    type VARCHAR(20),
    title VARCHAR(255),
    director TEXT,
    main_cast TEXT,
    country VARCHAR(255),
    date_added VARCHAR(50),
    release_year INT,
    rating VARCHAR(10),
    duration VARCHAR(50),
    listed_in TEXT,
    description TEXT
);
```

---

# 📥 Data Import Process

```sql
LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/NETFLIX.csv'
INTO TABLE netflix_titles
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\r\n'
IGNORE 1 ROWS;
```

### 📌 Business Importance

Efficient data ingestion is essential in enterprise environments where organizations process millions of records daily.

---

# ✔ Verify Imported Data

```sql
SELECT COUNT(*) AS total_records_imported
FROM netflix_titles;
```

## 📸 Output

![Total Record](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Total_Record.png)

---

## 📸 Record Verification

![Total Record Verification](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Total_Record2.png)

---

# 🧹 Data Cleaning & Validation

## Missing Value Analysis

```sql
SELECT
    SUM(CASE WHEN director = '' OR director IS NULL THEN 1 ELSE 0 END) AS missing_director,
    SUM(CASE WHEN country = '' OR country IS NULL THEN 1 ELSE 0 END) AS missing_country,
    SUM(CASE WHEN main_cast = '' OR main_cast IS NULL THEN 1 ELSE 0 END) AS missing_main_cast,
    SUM(CASE WHEN rating = '' OR rating IS NULL THEN 1 ELSE 0 END) AS missing_rating,
    SUM(CASE WHEN duration = '' OR duration IS NULL THEN 1 ELSE 0 END) AS missing_duration
FROM netflix_titles;
```

## 📸 Output

![Check Missing Value](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Check_Missing_Value.png)

---

# 🔍 Duplicate Record Detection

```sql
SELECT
    show_id,
    COUNT(*) AS duplicate_count
FROM netflix_titles
GROUP BY show_id
HAVING COUNT(*) > 1;
```

## 📸 Output

![Check Duplicate Record](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Check_Duplicate_Record.png)

### 📌 Business Impact

Data validation ensures reporting accuracy and prevents incorrect business decisions caused by duplicate or incomplete records.

---

# 👀 Preview Cleaned Dataset

```sql
SELECT *
FROM netflix_titles
LIMIT 10;
```

## 📸 Output

![Preview Clean Dataset](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Preview_Clean_Dataset.png)

---

# 📊 Business Intelligence & Analytics

# 1️⃣ Date Analytics

## Analyze yearly content growth

```sql
SELECT
    YEAR(STR_TO_DATE(date_added, '%M %d, %Y')) AS added_year,
    type,
    COUNT(*) AS total_content
FROM netflix_titles
WHERE date_added IS NOT NULL
GROUP BY added_year, type
ORDER BY added_year DESC;
```

## 📸 Output

![Date Analytics](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Date_Analytics.png)

### 📌 Business Insight

The analysis reveals rapid content expansion after 2015, reflecting Netflix’s aggressive global scaling strategy and increasing investment in original content production.

---

# 2️⃣ Stored Procedure

## Reusable Business Query

```sql
CREATE PROCEDURE GetContentByYear(IN selected_year INT)
BEGIN

    SELECT
        title,
        type,
        director,
        release_year
    FROM netflix_titles
    WHERE release_year = selected_year;

END;
```

## 📸 Output

![Stored Procedure](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Stored_Procedure.png)

### 📌 Business Impact

Stored procedures improve automation, reporting consistency, and query reusability in enterprise systems.

---

# 3️⃣ Top 10 Longer Duration Movies

```sql
SELECT
    title,
    duration
FROM netflix_titles
WHERE type = 'Movie'
ORDER BY CAST(REPLACE(duration, ' min', '') AS UNSIGNED) DESC
LIMIT 10;
```

## 📸 Output

![Top 10 Longer Duration](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Top10_Longer_Duration.png)

### 📌 Business Insight

Long-duration movies may influence audience engagement, watch-time metrics, and binge-watching behavior.

---

# 4️⃣ Business Overview Dashboard View

```sql
CREATE VIEW netflix_dashboard_summary AS
SELECT
    type,
    rating,
    release_year,
    country,
    COUNT(*) AS total_content
FROM netflix_titles
GROUP BY
    type,
    rating,
    release_year,
    country;
```

## 📸 Output

![Business Overview](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Business_Overview.png)

### 📌 Business Impact

Views simplify executive reporting and enable dashboard integration for decision-makers.

---

# 5️⃣ Percentage Analytics (Business KPI)

```sql
SELECT
    type,
    ROUND(
        COUNT(*) * 100.0 /
        (SELECT COUNT(*) FROM netflix_titles),
        2
    ) AS percentage
FROM netflix_titles
GROUP BY type;
```

## 📸 Output

![Percentage Analytics](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Percentage_Analytics.png)

### 📌 Business Insight

Movies dominate the platform content distribution, highlighting Netflix’s strong focus on movie-based audience engagement.

---

# 6️⃣ Top Countries by Content Production

```sql
WITH country_content AS (
    SELECT
        country,
        COUNT(*) AS total_content
    FROM netflix_titles
    WHERE country != 'Unknown'
    GROUP BY country
)

SELECT
    country,
    total_content
FROM country_content
WHERE total_content > 100
ORDER BY total_content DESC;
```

## 📸 Output

![Top Countries by Content Production](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Top_Countries_by_Content_Production.png)

### 📌 Business Insight

The United States and several major markets dominate content production, reflecting regional market leadership and content investment concentration.

---

# 7️⃣ Temporary Table for High Rated Content

```sql
CREATE TEMPORARY TABLE temp_tv_ma_content AS
SELECT
    title,
    type,
    rating,
    release_year
FROM netflix_titles
WHERE rating = 'TV-MA';
```

## 📸 Output

![Store High Rated Content](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Store_High_Rated_Content.png)

### 📌 Business Insight

TV-MA content represents a major audience segment, supporting mature-content targeting strategies.

---

# 8️⃣ Rank Movies by Release Year

```sql
SELECT
    title,
    release_year,
    ROW_NUMBER() OVER (
        ORDER BY release_year DESC
    ) AS row_num
FROM netflix_titles
WHERE type = 'Movie';
```

## 📸 Output

![Rank Movies by Release Year](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Rank_Movies_by_Release_Year.png)

### 📌 Business Impact

Window functions improve ranking analysis for trend monitoring and performance reporting.

---

# 9️⃣ Top Directors Ranking

```sql
SELECT
    director,
    COUNT(*) AS total_titles,
    DENSE_RANK() OVER (
        ORDER BY COUNT(*) DESC
    ) AS director_rank
FROM netflix_titles
WHERE director != 'Unknown'
GROUP BY director;
```

## 📸 Output

![Top Directors Ranking](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Top_Directors_Ranking.png)

### 📌 Business Insight

Identifying top-performing directors supports partnership evaluation and content acquisition strategies.

---

# 🔟 Content Released Above Average Year

```sql
SELECT
    title,
    release_year
FROM netflix_titles
WHERE release_year >
(
    SELECT AVG(release_year)
    FROM netflix_titles
)
ORDER BY release_year DESC;
```

## 📸 Output

![Content Released Above Average Year](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Content_Released_Above_Average_Year.png)

### 📌 Business Insight

Modern content dominates the catalog, reflecting recent production growth and evolving audience preferences.

---

# 1️⃣1️⃣ Content Era Classification

```sql
SELECT
    title,
    release_year,

    CASE
        WHEN release_year < 2000 THEN 'Classic Content'
        WHEN release_year BETWEEN 2000 AND 2015 THEN 'Modern Content'
        ELSE 'Recent Content'
    END AS content_segment

FROM netflix_titles;
```

## 📸 Output

![Content Era Classification](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Content_Era_Classification.png)

### 📌 Business Insight

Content segmentation improves recommendation systems and audience personalization strategies.

---

# 1️⃣2️⃣ Top 3 Countries by Total Content

```sql
WITH country_rankings AS (

    SELECT
        country,
        COUNT(*) AS total_content,

        DENSE_RANK() OVER (
            ORDER BY COUNT(*) DESC
        ) AS ranking

    FROM netflix_titles
    WHERE country != 'Unknown'
    GROUP BY country
)

SELECT *
FROM country_rankings
WHERE ranking <= 3;
```

## 📸 Output

![Top 3 Countries by Total Content](https://github.com/azlinaaaa/Netflix-Content-Analytics/blob/13aea9571ed159ce821dc29c5f1a1f5b5c4676a4/Code_Output/Top%203%20Countries%20by%20Total%20Content.png)

### 📌 Business Insight

Top-producing countries represent strategic markets with high content generation capacity and audience reach.

---

# 🧠 Advanced SQL Concepts Demonstrated

| SQL Concept | Purpose |
|---|---|
| Window Functions | Ranking & Analytics |
| CTE (WITH Clause) | Complex Query Simplification |
| Stored Procedures | Query Automation |
| Temporary Tables | Intermediate Data Storage |
| Views | Dashboard Reporting |
| Subqueries | Advanced Filtering |
| CASE Statements | Business Segmentation | 

---

# 📌 Final Conclusion

This project successfully demonstrates how SQL can be transformed from a querying language into a complete Business Intelligence solution capable of supporting strategic decision-making in business environments.

The project reflects real-world analytical workflows and showcases strong technical, analytical, and business intelligence capabilities.
