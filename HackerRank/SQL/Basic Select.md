# SQL - Basic Select
- [링크](https://www.hackerrank.com/domains/sql?filters%5Bsubdomains%5D%5B%5D=select)

## CITY TABLE

| Field | Type |
|:-----------:|:------------:|
| ID | NUMBER |
| NAME | VARCHAR2(17) |
| COUNTRYCODE | VARCHAR2(3) |
| DISTRICT | VARCHAR2(20) |
| POPULATION | NUMBER |

#### 1. Query all columns for all American cities in CITY with populations larger than 100000. The CountryCode for America is USA.

```sql
SELECT * FROM CITY WHERE COUNTRYCODE = "USA" AND POPULATION > 100000;
```

#### 2. Query the names of all American cities in CITY with populations larger than 120000. The CountryCode for America is USA.

```sql
SELECT NAME FROM CITY WHERE COUNTRYCODE='USA' AND POPULATION > 120000;
```

#### 3. Query all columns (attributes) for every row in the CITY table.

```sql
SELECT * FROM CITY;
```


## STATION TABLE

| Field | Type |
|:------:|:------------:|
| ID | NUMBER |
| CITY | VARCHAR2(21) |
| STATE | VARCHAR2(2) |
| LAT_N | NUMBER |
| LONG_W | NUMBER |

where LAT_N is the northern latitude and LONG_W is the western longitude.

#### 1. Query a list of CITY and STATE from the STATION table.

```sql
SELECT CITY, STATE FROM STATION;
```

#### 2. Query a list of CITY names from STATION with even ID numbers only. You may print the results in any order, but must exclude duplicates from your answer.

```sql
SELECT DISTINCT CITY FROM STATION WHERE MOD(ID, 2) = 0;
```

#### 3. Let N be the number of CITY entries in STATION, and let N' be the number of distinct CITY names in STATION; query the value of N - N' from STATION. In other words, find the difference between the total number of CITY entries in the table and the number of distinct CITY entries in the table.

```sql
SELECT COUNT(CITY) - COUNT(DISTINCT CITY) FROM STATION;
```

#### 4. Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.

```sql
(SELECT CITY, LENGTH(CITY) FROM STATION ORDER BY LENGTH(CITY), CITY LIMIT 1) UNION 
(SELECT CITY, LENGTH(CITY) FROM STATION ORDER BY LENGTH(CITY) DESC, CITY ASC LIMIT 1)
```

#### 5. Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY FROM STATION WHERE CITY LIKE 'A%' OR CITY LIKE 'E%' OR CITY LIKE 'I%' OR CITY LIKE 'O%' OR CITY LIKE 'U%';
```

#### 6. Query the list of CITY names ending with vowels (a, e, i, o, u) from STATION. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY FROM STATION WHERE CITY LIKE '%A' OR CITY LIKE '%E' OR CITY LIKE '%I' OR CITY LIKE '%O' OR CITY LIKE '%U';
```

#### 7. Query the list of CITY names from STATION which have vowels (i.e., a, e, i, o, and u) as both their first and last characters. Your result cannot contain duplicates.

```sql
SELECT CITY FROM STATION WHERE CITY REGEXP '^(a|e|i|o|u).*(a|e|i|o|u)$'
```

#### 8. Query the list of CITY names from STATION that do not start with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY FROM STATION WHERE CITY REGEXP '^[^a|e|i|o|u].*'
```

#### 9. Query the list of CITY names from STATION that do not end with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY FROM STATION WHERE CITY REGEXP '.*[^a|e|i|o|u]$';
```

#### 10. Query the list of CITY names from STATION that either do not start with vowels or do not end with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY FROM STATION WHERE CITY REGEXP '^[^aeiou]|[^aeiou]$'
```

#### 11. Query the list of CITY names from STATION that do not start with vowels and do not end with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY FROM STATION WHERE CITY REGEXP '(^[^a|e|i|o|u].*[^a|e|i|o|u]$)'
```


## STUDENTS TABLE

| Field | Type |
|:-----:|:-------:|
| ID | Integer |
| NAME | String |
| MARKS | Integer |

#### 1. Query the Name of any student in STUDENTS who scored higher than 75 Marks. Order your output by the last three characters of each name. If two or more students both have names ending in the same last three characters (i.e.: Bobby, Robby, etc.), secondary sort them by ascending ID.

```sql
SELECT NAME FROM STUDENTS WHERE MARKS > 75 ORDER BY RIGHT(NAME, 3), ID;
```


## EMPLOYEE TABLE

| Field | Type |
|:-----------:|:-------:|
| EMPLOYEE_ID | Integer |
| NAME | String |
| MONTHS | Integer |
| SALARY | Integer |

#### 1. Write a query that prints a list of employee names (i.e.: the name attribute) from the Employee table in alphabetical order.

```sql
SELECT NAME FROM EMPLOYEE ORDER BY NAME;
```

#### 2. Write a query that prints a list of employee names (i.e.: the name attribute) for employees in Employee having a salary greater than $2000 per month who have been employees for less than 10 months. Sort your result by ascending employee_id.

```sql
SELECT NAME FROM EMPLOYEE WHERE SALARY > 2000 AND MONTHS < 10 ORDER BY EMPLOYEE_ID;
```