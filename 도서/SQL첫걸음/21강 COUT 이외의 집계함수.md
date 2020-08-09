# 21강 COUNT 이외의 집계함수

```sql
SUM( [ALL | DISTINCT] 집합)
AVG( [ALL | DISTINCT] 집합)
MIN( [ALL | DISTINCT] 집합)
MAX( [ALL | DISTINCT] 집합)
```

- SUM: 합계
    - 문자열형이나 날짜시간형의 집합에서는 사용불가
    - NULL은 무시하고 계산
- AVG: 평균
    - `SUM()/COUNT()`로도 평균을 계산할 수 있음
    - NULL은 무시되며 NULL을 0으로 처리하고 싶다면 따로 `CASE` 문과 같은 로직을 사용
- MIN/MAX
    - 문자열형과 날짜시간형에도 사용가능
    - NULL 무시