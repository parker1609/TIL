# API

## Link 생성

```
POST /links HTTP/1.1
```

## Link 조회
- 전체 Link 조회

```
GET /links HTTP/1.1
```

## Link 수정
- Link 내부 Tag 수정

```
UPDATE /links/{linkId} HTTP/1.1
```

## Link 삭제

```
DELETE /links/{linkId} HTTP/1.1
```

## Tag 조회

- 전체 조회

```
GET /tags HTTP/1.1
```

- 특정 Tag 조회

```
GET /tags/{tagname} HTTP/1.1
```

