# SQL - Basic Join

## CITY, COUNTRY
- CITY TABLE

| Field | Type |
|-------------|--------------|
| ID | NUMBER |
| NAME | VARCHAR2(17) |
| COUNTRYCODE | VARCHAR2(3) |
| DISTRICT | VARCHAR(20) |
| POPULATION | NUMBER |

- COUNTRY TABLE

| Field | Type |
|-------------|--------------|
| CODE | VARCHAR2(3) |
| NAME | VARCHAR2(44) |
| CONTINENT | VARCHAR2(13) |
| REGION | VARCHAR(25) |
| SURFACEAREA | NUMBER |
| ... | ... |

### Asian Population
Given the CITY and COUNTRY tables, query the sum of the populations of all cities where the CONTINENT is 'Asia'.

Note: CITY.CountryCode and COUNTRY.Code are matching key columns.

```sql
SELECT SUM(city.POPULATION)
FROM CITY city
INNER JOIN COUNTRY country ON city.COUNTRYCODE = country.CODE
WHERE COUNTRY.CONTINENT = 'Asia';
```

### African Cities
Given the CITY and COUNTRY tables, query the names of all cities where the CONTINENT is 'Africa'.

Note: CITY.CountryCode and COUNTRY.Code are matching key columns.

```sql
SELECT city.name
FROM CITY city
INNER JOIN COUNTRY country ON city.COUNTRYCODE = country.CODE
WHERE country.CONTINENT = 'Africa'
```

### Average Population of Each Continent
Given the CITY and COUNTRY tables, query the names of all the continents (COUNTRY.Continent) and their respective average city populations (CITY.Population) rounded down to the nearest integer.

Note: CITY.CountryCode and COUNTRY.Code are matching key columns.

```sql
SELECT country.CONTINENT, TRUNCATE(IFNULL(AVG(city.POPULATION), 0), 0)
FROM COUNTRY country
INNER JOIN CITY city ON country.CODE = city.COUNTRYCODE
GROUP BY country.CONTINENT
```

### The Report
- [문제 링크](https://www.hackerrank.com/challenges/the-report/problem)

```sql
SELECT (CASE g.grade >= 8 WHEN TRUE THEN s.name ELSE null END), g.grade, s.marks
FROM students s
INNER JOIN grades g ON s.marks BETWEEN min_mark AND max_mark
ORDER BY g.grade DESC, s.name, s.marks
```