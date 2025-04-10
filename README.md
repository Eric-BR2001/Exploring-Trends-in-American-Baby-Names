# Exploring-Trends-in-American-Baby-Names
What makes a name timeless or trendy? This project uses over a century of U.S. Social Security data to explore American baby name preferences.  The data manipulation skills you'll use in this project are broadly applicable: understanding changing tastes is a key competency for businesses as well as parents searching for a baby name!


# Baby Names Analysis Project

![Baby Names Visualization](baby_names.jpg)

## Project Overview
This project explores how American baby name tastes have changed from 1920 to 2020. The analysis examines:
- Names that have remained popular for over 100 years
- Comparisons between classic and trendy names
- Gender-specific naming trends
- The evolution of name popularity over time

## Dataset
The data comes from the United States Social Security Administration and includes first names given to more than 5,000 American babies in any year from 1920 through 2020.

### `baby_names` Table Structure
| Column       | Type    | Description                                      |
|--------------|---------|--------------------------------------------------|
| `year`       | int     | Year of record                                  |
| `first_name` | varchar | First name recorded                             |
| `sex`        | varchar | Sex of babies given the name (M/F)              |
| `num`        | int     | Number of babies of that sex given that name    |

## Key Analyses

### 1. Classic vs. Trendy Names
```sql
SELECT first_name, SUM(num),
    CASE WHEN COUNT(year) > 50 THEN 'Classic'
        ELSE 'Trendy' END AS popularity_type
FROM baby_names
GROUP BY first_name
ORDER BY first_name
LIMIT 5;
```

### 2. Top Male Names Ranking
```sql
SELECT
    RANK() OVER(ORDER BY SUM(num) DESC) AS name_rank,
    first_name, SUM(num)
FROM baby_names
WHERE sex = 'M'
GROUP BY first_name
ORDER BY name_rank
LIMIT 20;
```

### 3. Female Names Spanning the Century
```sql
SELECT a.first_name, (a.num + b.num) AS total_occurrences
FROM baby_names a
JOIN baby_names b
ON a.first_name = b.first_name
WHERE a.year = 1920 AND a.sex = 'F'
AND b.year = 2020 AND b.sex = 'F';
```

### Business Applications
The skills practiced in this analysis are broadly applicable to:

Trend analysis in consumer preferences

Market research

Product lifecycle management

Historical data pattern recognition