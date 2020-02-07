## GROUP BY
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