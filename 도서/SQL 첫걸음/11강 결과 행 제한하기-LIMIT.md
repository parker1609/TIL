# 11강 결과 행 제한하기 - LIMIT
- `SELECT` 명령에서는 결괏값으로 반환되는 행을 제한할 수 있다.

```
SELECT 열명 FROM 테이블명 LIMIT 행수 [OFFSET 시작행]
```

- `LIMIT`구는 표준 SQL은 아니다.
  - MySQL, PostgreSQL에서 사용할 수 있는 문법이다.
- `LIMIT`구는 `SELECT` 명령의 마지막에 지정하는 것으로 `WHERE`구나 `ORDER BY`구 뒤에 지정한다.

```
SELECT 열명 FROM 테이블명 WHERE 조건식 ORDER BY 열명 LIMIT 행수
```

- 행수는 최대 행수를 말한다.
  - 만약 `LIMIT 10`이면 최대 10개의 행이 클라이언트로 반환된다.
  - 테이블의 행의 개수가 1개라도 `LIMIT 10`을 지정하면 1개만 반환된다.
- `LIMIT 3`은 `WHERE no <= 3`으로도 처리할 수 있다.
  - `LIMIT`는 조건식과 정렬 마지막에 처리된다.

## 오프셋 지정
- 오프셋은 대량의 데이터를 하나의 페이지에서 표시하는 것보다 여러 페이지로 나눠서 가져올 때 편히 사용할 수 있다.
- `LIMIT`구에서 `OFFSET`은 생략 가능하며 기본값은 0이다.
- 오프셋의 위치는 컴퓨터의 배열이 0부터 시작하는 것으로 생각하고 사용해야 한다.

```
SELECT 열명 FROM 테이블명 LIMIT 행수 OFFSET 위치
```

- 예제

```
SELECT * FROM sample33 LIMIT 3 OFFSET 3;
```

| no |
|:-:|
| 4 |
| 5 |
| 6 |
