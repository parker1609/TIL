## GROUP BY
### 입양 시각 구하기(2)
- [문제 링크](https://programmers.co.kr/learn/courses/30/lessons/59413#qna)

```sql
SELECT T1.HOUR AS HOUR, IFNULL(T2.COUNT, 0) AS COUNT
FROM
    (SELECT 0 AS HOUR UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9 UNION SELECT 10 UNION SELECT 11 UNION SELECT 12 UNION SELECT 13 UNION SELECT 14 UNION SELECT 15 UNION SELECT 16 UNION SELECT 17 UNION SELECT 18 UNION SELECT 19 UNION SELECT 20 UNION SELECT 21 UNION SELECT 22 UNION SELECT 23) T1
LEFT OUTER JOIN
    (
        SELECT HOUR(ao.DATETIME) AS HOUR, COUNT(*) AS COUNT
        FROM ANIMAL_OUTS ao
        GROUP BY HOUR
        ORDER BY HOUR
    ) T2
    ON T1.HOUR = T2.HOUR
;
```


### 입양 시각 구하기(1)
- [문제 링크](https://programmers.co.kr/learn/courses/30/lessons/59412)

```sql
SELECT HOUR(ao.DATETIME) as 'HOUR', COUNT(ao.ANIMAL_ID) as 'COUNT'
FROM ANIMAL_OUTS ao
GROUP BY HOUR(ao.DATETIME)
HAVING HOUR BETWEEN 9 AND 19
ORDER BY HOUR(ao.DATETIME)
;
```


### 동명 동물 수 찾기
- [문제 링크](https://programmers.co.kr/learn/courses/30/lessons/59041)

```sql
SELECT ai.NAME, COUNT(ai.NAME) AS COUNT
FROM ANIMAL_INS ai
WHERE ai.NAME IS NOT NULL
GROUP BY ai.NAME
HAVING COUNT > 1
ORDER BY ai.NAME
;
```


### 고양이와 개는 몇 마리 있을까
- [문제 링크](https://programmers.co.kr/learn/courses/30/lessons/59040)

```sql
SELECT ai.ANIMAL_TYPE, count(ai.ANIMAL_TYPE) AS count
FROM ANIMAL_INS ai
WHERE ai.ANIMAL_TYPE = 'Cat' OR ai.ANIMAL_TYPE = 'Dog'
GROUP BY ai.ANIMAL_TYPE
;
```

## JOIN
### 보호소에서 중성화한 동물
- [문제 링크](https://programmers.co.kr/learn/courses/30/lessons/59045)

```sql
SELECT ai.ANIMAL_ID, ai.ANIMAL_TYPE, ai.NAME
FROM ANIMAL_INS ai
INNER JOIN ANIMAL_OUTS ao ON ai.ANIMAL_ID = ao.ANIMAL_ID
WHERE ai.SEX_UPON_INTAKE LIKE 'Intact%' AND (ao.SEX_UPON_OUTCOME LIKE 'Spayed%' OR ao.SEX_UPON_OUTCOME LIKE 'Neutered%')
;
```


### 오랜 기간 보호한 동물(1)
- [문제 링크](https://programmers.co.kr/learn/courses/30/lessons/59044)

```sql
SELECT ai.NAME, ai.DATETIME
FROM ANIMAL_INS ai
LEFT OUTER JOIN ANIMAL_OUTS ao ON ai.ANIMAL_ID = ao.ANIMAL_ID
WHERE ao.ANIMAL_ID IS NULL
ORDER BY ai.DATETIME ASC
LIMIT 3
;
```


### 있었는데요 없었습니다.
- [문제 링크](https://programmers.co.kr/learn/courses/30/lessons/59043)

```sql
SELECT ai.ANIMAL_ID, ai.NAME
FROM ANIMAL_INS ai
INNER JOIN ANIMAL_OUTS ao ON ai.ANIMAL_ID = ao.ANIMAL_ID
WHERE ai.DATETIME > ao.DATETIME
ORDER BY ai.DATETIME
;
```


### 없어진 기록 찾기
- [문제 링크](https://programmers.co.kr/learn/courses/30/lessons/59042)

#### LEFT OUTER JOIN

```sql
SELECT ao.ANIMAL_ID, ao.NAME
FROM ANIMAL_OUTS ao
LEFT OUTER JOIN ANIMAL_INS ai ON ao.ANIMAL_ID = ai.ANIMAL_ID
WHERE ai.ANIMAL_ID IS NULL
ORDER BY ao.ANIMAL_ID
;
```

#### 서브 쿼리

```sql
SELECT ao.ANIMAL_ID, ao.NAME
FROM ANIMAL_OUTS ao
WHERE ao.ANIMAL_ID NOT IN (
    SELECT ai.ANIMAL_ID
    FROM ANIMAL_INS ai
)
ORDER BY ao.ANIMAL_ID
;
```