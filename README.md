# Movie Rentals Analysis SQL Project

## Project Overview

**Project Title**: Movie Rentals Analysis

**Level**: Intermediate

This project showcases my SQL skills in exploring and analysing data. It involves using a movie rental database, performing Exploratory Data Analysis(EDA), and executing specific queries to analyse the data.

## Objectives

- **Exploratory Data Analysis (EDA):** Perform basic EDA to understand the structure and distribution of the data.
- **Business Analysis:** Use SQL queries to answer real-world business questions and extract actionable insights.


## Technologies Used
- SQL (PostgreSQL)
- pgAdmin
- Git & GitHub for version control

## Data Analysis and Findings

### 1.Create a list of all the different (distinct) replacement costs of the films.

```sql
SELECT DISTINCT(REPLACEMENT_COST) 
FROM FILM 
ORDER BY REPLACEMENT_COST
```

### 2.Write a query that gives an overview of how many films have replacements costs in the following cost ranges
low: 9.99 - 19.99
medium: 20.00 - 24.99
high: 25.00 - 29.99

```sql
SELECT COUNT(CASE
WHEN REPLACEMENT_COST>=9.99 AND REPLACEMENT_COST<=19.99 THEN 1
END) AS LOW,
COUNT(CASE WHEN REPLACEMENT_COST>=20.00 AND REPLACEMENT_COST<=24.99 THEN 1
END) AS MEDIUM,
COUNT(CASE WHEN REPLACEMENT_COST>=25.00 AND REPLACEMENT_COST<=29.99 THEN 1
END) AS HIGH
FROM FILM 
```

### 3. Create a list of the film titles including their title, length, and category name ordered descendingly by length.

```sql
SELECT F.TITLE,F.LENGTH,C.NAME
FROM FILM F INNER JOIN FILM_CATEGORY FC ON F.FILM_ID=FC.FILM_ID
			INNER JOIN CATEGORY C ON C.CATEGORY_ID=FC.CATEGORY_ID
WHERE C.NAME='Drama' OR C.NAME='Sports' 
ORDER BY F.LENGTH DESC
```

### 4. Create an overview of how many movies (titles) are in each category (name).
```sql
SELECT COUNT(F.TITLE) AS FILM_COUNT,C.NAME
FROM FILM F INNER JOIN FILM_CATEGORY FC ON F.FILM_ID=FC.FILM_ID
			INNER JOIN CATEGORY C ON C.CATEGORY_ID=FC.CATEGORY_ID
GROUP BY(C.NAME)
ORDER BY COUNT(F.TITLE) DESC
```

### 5. Create an overview of the actors' first and last names and in how many movies they appear.
```sql
SELECT A.FIRST_NAME,A.LAST_NAME,COUNT(F.FILM_ID)
FROM FILM F INNER JOIN FILM_ACTOR FA ON F.FILM_ID=FA.FILM_ID
INNER JOIN ACTOR A ON A.ACTOR_ID=FA.ACTOR_ID
GROUP BY A.FIRST_NAME,A.LAST_NAME
ORDER BY COUNT(F.FILM_ID) DESC
```

### 6.Create an overview of the addresses that are not associated to any customer.
```sql
SELECT address
FROM ADDRESS A LEFT JOIN CUSTOMER C ON C.ADDRESS_ID=A.ADDRESS_ID
WHERE C.CUSTOMER_ID IS NULL
```

### 7. Create an overview of the sales  to determine the city where most sales occur.
```sql
SELECT CT.CITY,SUM(P.AMOUNT)
FROM ADDRESS A INNER JOIN CITY CT ON A.CITY_ID=CT.CITY_ID
INNER JOIN CUSTOMER C ON A.ADDRESS_ID=C.ADDRESS_ID
INNER JOIN PAYMENT P ON P.CUSTOMER_ID=C.CUSTOMER_ID
GROUP BY CT.CITY
ORDER BY SUM(P.AMOUNT) DESC
LIMIT 1
```

### 8.Create an overview of the revenue (sum of amount) grouped by a column in the format "country, city".
```sql
SELECT CT.CITY,CN.COUNTRY,SUM(P.AMOUNT)
FROM ADDRESS A INNER JOIN CITY CT ON A.CITY_ID=CT.CITY_ID
INNER JOIN COUNTRY CN ON CN.COUNTRY_ID=CT.COUNTRY_ID
INNER JOIN CUSTOMER C ON A.ADDRESS_ID=C.ADDRESS_ID
INNER JOIN PAYMENT P ON P.CUSTOMER_ID=C.CUSTOMER_ID
GROUP BY (CT.CITY,CN.COUNTRY)
ORDER BY SUM(P.AMOUNT) 
```

### 9.Create a query that shows average daily revenue of all Sundays.
```sql
SELECT SUM(SUM_OF_SUNDAYS),COUNT(SUM_OF_SUNDAYS),ROUND(SUM(SUM_OF_SUNDAYS)/COUNT(SUM_OF_SUNDAYS),2) AS AVERAGE FROM 
(SELECT SUM(AMOUNT) AS SUM_OF_SUNDAYS
FROM PAYMENT
where EXTRACT(dow FROM PAYMENT_DATE)=0
GROUP BY DATE (PAYMENT_DATE))

```


### 10. Create a list of movies - with their length and their replacement cost - that are longer than the average length in each replacement cost group.
```sql
SELECT TITLE,A.LENGTH FROM
(SELECT TITLE,REPLACEMENT_COST,LENGTH FROM FILM) A INNER JOIN
(SELECT REPLACEMENT_COST,AVG(LENGTH) AS AVG_LENGTH
FROM FILM
GROUP BY REPLACEMENT_COST)B ON A.REPLACEMENT_COST=B.REPLACEMENT_COST
WHERE A.LENGTH>AVG_LENGTH
ORDER BY A.LENGTH 
LIMIT 2
```

## Conclusion

This project demonstrates how to perform structured SQL analysis on a real-world style movie rental database. From basic EDA to business insights, it emphasises the practical use of SQL joins, aggregations, filtering, and subqueries. 







