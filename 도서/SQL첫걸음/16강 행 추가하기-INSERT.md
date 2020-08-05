# 16강 행 추가하기 - INSERT

```sql
INSERT INTO 테이블명 VALUES (값1, 값2, ...)
```

- INSERT 명령어는 테이블에 행 단위로 데이터를 추가
- VALUES 를 지정할 때 해당 테이블의 열의 데이터 형식에 맞게 지정해야 함.

## 값을 저장할 열 지정하기

```sql
INSERT INTO 테이블명 (열1, 열2, ...) VALUES (값1, 값2, ...)
```

- 별도의 값이 지정되지 않은 열은 기본값이 들어감.
    기본값이 지정되지 않은 열은 NULL이 들어감.

## NOT NULL 제약
- NOT NULL 제약과 같이 INSERT 문으로 데이터를 추가할 때 해당 테이블의 제약을 준수해야함.

## DEFAULT
- `DESC` 명령으로 열 구성을 보면 Default 항목을 볼 수 있음
- VALUES에 값을 명시하지 않거나 `DEFAULT` 를 명시하면 해당 열의 기본값이 들어감.