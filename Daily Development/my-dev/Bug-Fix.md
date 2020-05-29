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

### OneToMany 관계 데이터 가져오기
Lik에서 Tag의 정보를 가지고 있는 LinkTag 테이블을 OneToMany로 가지고 있다. 이 때 Link 전체를 조회하는 기능을 단순히 `findAll()`을 사용하면 에러가 발생한다. 왜냐하면 OneToMany는 기존적으로 Fetch 전략이 Lazy이므로 실제로 `get..()`과 같은 참조가 발생할 때 데이터베이스에서 가져오게 된다. 그런데 stream을 사용하는 도중에 collect로 리스트로 만들어주는 도중에 데이터가 존재하지 않는다는 예외가 발생했다.

이를 해결하기 위해 Link 전체 조회는 메인 화면에서 Link의 모든 데이터를 가져오는게 맞다고 판단하여 fetch join을 사용해서 한 번에 데이터를 모두 가져오도록 변경했다.

fetch join에서 문제점이 데이터베이스 테이블 특성상 OneToMany에서 여러 개의 데이터가 있으면 실제 parent 역시 중복해서 테이블에 저장하는 문제가 발생한다. 이는 `DISTINCT` 키워드를 쿼리에 추가하여 해결했다.

- [JPA N+1 문제 및 해결방안](https://jojoldu.tistory.com/165)
- [[Querydsl] OneToMany 관계에서 fetchjoin 시 데이터 중복 문제](https://blog.leocat.kr/notes/2020/01/13/querydsl-duplication-problem-on-fetchjoin-with-onetomany)

### 엔티티 삭제 설정
- [JPA - 영속성 전이(Cascade)와 고아 객체(Orphan)](https://coding-start.tistory.com/83)


## Test Code
### MockMvc를 사용한 controller 테스트 중 POST 검증
서비스 객체를 목킹하여 반환값만을 취하려는데, `given()` 절에서 실제 객체를 쓰면 `.willReturn`에서 계속 null 값이 반환된다. `any()`를 사용하니, 정상적으로 `.willReturn` 값이 반환되었다.

```java
given(linkService.save(any())).willReturn(linkResponseDto);
```