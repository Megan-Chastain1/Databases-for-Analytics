# Exercise 02: World Database – Joins, Grouping, and Data Quality

- Name: Megan Chastain
- Course: Database for Analytics
- Module: 2
- Database Used: World Database (PostgreSQL)

---

## Instructions

- Answer each question below using SQL executed against the **World database**.
- All SQL commands **must be run by you**.
- For each SQL-based question:
  - Include the SQL command in a fenced code block
  - Include a **screenshot** showing the command and its results
- Store Screenshots in the `Screenshots/` folder and embed them below each answer.

---

## Question 1

When importing records from `worldPGSQL.sql`, **how many cities were imported**?

### Answer
4079

### Screenshot
_Show evidence of how you determined this (for example, a COUNT query)._

```sql
SELECT count(Name) From city;
```

![Q1 Screenshot](Screenshots/ex02_countcity.png)

---

## Question 2

Using the World database, write the SQL command to **display each country name along with the name of each language spoken in that country**.

### SQL

```sql
SELECT
	country.name,
    countrylanguage.language
FROM
    country
JOIN
    countrylanguage ON country.countrycode = countrylanguage.countrycode;
```

### Screenshot

![Q2 Screenshot](Screenshots/ex02_join1.png)

---

## Question 3

Using the World database, write the SQL command to **display each country name along with the name of each official language spoken in that country**.

### SQL

```sql
SELECT
	country.name as 'Country',
    countrylanguage.language as 'Official Language'
FROM
    country
JOIN
    countrylanguage ON country.countrycode = countrylanguage.countrycode
where
	countrylanguage.IsOfficial = 'T';
```

### Screenshot

![Q3 Screenshot](Screenshots/ex02_join2.png)

---

## Question 4

Consider the following two SQL statements:

```sql
SELECT *
FROM country, countrylanguage
WHERE country.code = countrylanguage.countrycode;
```

```sql
SELECT *
FROM country
LEFT OUTER JOIN countrylanguage
ON country.code = countrylanguage.countrycode;
```

**In your own words**, describe what data the **second query returns that the first query does not**.

### Answer
The second query will list all countries even if they don't have a language recorded.

---

## Question 5

Using the World database, write the SQL command to **list all different forms of government** found in the data.
Do **not** repeat any form of government more than once.

### SQL

```sql
SELECT
DISTINCT GovernmentForm
FROM country;
```

### Screenshot

![Q5 Screenshot](Screenshots/ex02_govt.png)

---

## Question 6

Using the World database, write the SQL command to **list all names of cities and countries in one column**.
Label the column **"City or Country Name"**.

### SQL

```sql
SELECT
Name as 'city or Country Name'
FROM country
Union
Select
Name
from city
```

### Screenshot

![Q6 Screenshot](Screenshots/ex02_join3.png)

---

## Question 7

Using the World database, write the SQL command to **list all countries by name**, along with the **number of languages spoken in each country**.
Be sure to **sort by country name**.

### SQL

```sql
SELECT country.Name, COUNT(countrylanguage.Language) AS NumberOfLanguages
FROM country
LEFT JOIN countrylanguage ON country.countryCode = countrylanguage.CountryCode
GROUP BY country.Name
ORDER BY country.Name;
```

### Screenshot

![Q7 Screenshot](Screenshots/ex02_join4.png)

---

## Question 8

Using the World database, write the SQL command to **list all languages**, along with the **number of countries where each language is spoken**.
Be sure to **sort by language name**.

### SQL

```sql
SELECT Language, count(countrycode) AS NumberOfCountries
FROM countrylanguage
GROUP BY Language
ORDER BY Language;
```

### Screenshot

![Q8 Screenshot](Screenshots/ex02_languagecount.png)

---

## Question 9

Using the World database, write the SQL command to **list countries that have more than two official languages**, along with the **number of official languages spoken**.

*Hint: There are 8 such countries in this dataset.*

### SQL

```sql
SELECT country.Name, COUNT(countrylanguage.Language) AS OfficialLanguagesCount
FROM country
JOIN countrylanguage ON country.countryCode = countrylanguage.CountryCode
WHERE countrylanguage.IsOfficial = 'T'
GROUP BY country.Name
HAVING COUNT(countrylanguage.Language) > 2
ORDER BY OfficialLanguagesCount DESC;
```

### Screenshot

![Q9 Screenshot](Screenshots/ex02_languagecount2.png)

---

## Question 10

Using the World database, write the SQL command to **find cities where the district value is missing**.

*Hint: Use `LIKE` and the dash (`-`) since some rows use that instead of actual data.*

### SQL

```sql
select
	*
From
	city
Where
	district is null or district ='';
```

### Screenshot

![Q10 Screenshot](Screenshots/ex02_misisng.png)

---

## Question 11

Using the World database, write the SQL command to **calculate the percentage of cities with missing district values**.

*Hint: The result should be approximately 0.4%.*

** I did not get 0.4%, I got 0.09% for my query.
### SQL

```sql
select
	sum(case when district
    is null or district = ''
    then 1
    else 0 end) *100/count(*)
    as missing
From
	city;
```

### Screenshot

![Q11 Screenshot](Screenshots/ex02_percentage2.png)
