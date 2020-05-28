# 버그 관련

## JPA
### mappedBy
주인이 되는 변수 이름을 대소문자 구분해서 정확히 적어주어야 한다.

- 주인

```java
@ManyToOne
@JoinColumn(name = "LINK_ID")
private Link link;
```

- 단순히 매핑만 하는 필드

```java
@OneToMany(mappedBy = "link")
private List<LinkTag> tags = new ArrayList<>();
```


## Test Code
### MockMvc를 사용한 controller 테스트 중 POST 검증
서비스 객체를 목킹하여 반환값만을 취하려는데, `given()` 절에서 실제 객체를 쓰면 `.willReturn`에서 계속 null 값이 반환된다. `any()`를 사용하니, 정상적으로 `.willReturn` 값이 반환되었다.

```java
given(linkService.save(any())).willReturn(linkResponseDto);
```