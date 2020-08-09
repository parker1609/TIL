# 20강 행 개수 구하기 - COUNT

```sql
COUNT (집합)
```

- SQL은 집합을 다루는 **집계함수**를 제공함.
- 집계함수는 인수로 집합을 지정

```sql
SELECT COUNT(*) FROM sample51;
```

- `COUNT(*)`에서 '*'는 모든 열을 나타냄
- 집계함수는 집합으로부터 하나의 값을 반환
    - 집계함수가 아닌 일반적인 함수는 하나의 행에 대해 하나의 값을 반환
    - 집계함수를 SELECT 구에 쓰면 WHERE 구의 유무와 관계없이 결괏값으로 하나의 행을 반환
- COUNT의 인수로 일반적으로 열명을 지정(그 열의 개수를 반환)
- **집계함수 안에 NULL 값이 있으면 이를 제외함.**
    - '*'는 NULL을 포함함.
- 중복되지 않는 데이터의 개수를 구하려면 SELECT 구 바로 다음에 **DISTINCT** 키워드를 추가

## 집계함수에서 DISTINCT
중복이 제거된 개수를 구하기 위해서는 다음과 같이 해야 한다.

```sql
SELECT COUNT(DISTINCT name) FROM sample51;
```

`SELECT DISTINCT COUNT(name)`는 COUNT가 먼저 계산되므로 중복이 그대로 포함되어 개수를 센다.
