# 17장 삭제하기 - DELETE

```sql
DELETE FROM 테이블명 WHERE 조건식
```

- DELETE 명령어는 테이블을 행 단위로 삭제

```sql
DELETE FROM 테이블명
```

- WHERE 조건 없이 DELETE 명령을 실행하면 테이블 자체가 삭제됨.
- MySQL에서는 ORDER BY 구와 LIMIT 구를 사용해 삭제할 행을 지정할 수 있음
    - 다른 RDBMS는 ORDER BY 구를 사용할 수 없음. 삭제 순서는 중요하지 않기 때문.