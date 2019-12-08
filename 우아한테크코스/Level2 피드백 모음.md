# Level 2 피드백 모음

#### gradle 빌드에서 `implementation`과 `complie(api)`의 차이점
- 참고 링크: <https://tjandroid.blogspot.com/2018/11/api-implementation.html>
- Gradle 3.0 부터는 의존 라이브러리 수정 시 재빌드가 필요한 라이브러리를 선택적으로 할 수 있도록 compile 대신 api와 implementation으로 나눠 필요없는 경우 재빌드하지 않도록 하였다.
- Gradle 버전 호환을 위해서 compile은 api와 동일한 동작을 한다.
- api와 implementation의 차이점
  - api: 의존 라이브러리 수정시 본 모듈을 의존하고 있는 모듈들 또한 재빌드한다.
    - `A(api) <- B <- C` 의 경우 C가 A를 접근할 수 있다.
    - A수정 시 B, C 모두 재빌드
  - implementation: 의존 라이브러리 수정시 본 모듈까지만 재빌드한다.
    - `A(implementation) <- B <- C`의 경우 C가 A에 접근할 수 없다.
    - A 수정 시 B만 재빌드
- 그외 의존성 옵션들
  - compileOnly
  - runtimeOnly

#### `@Autowired`는 Field Injection보다 Constructor Injection을 추천함
- [참고 링크](https://zorba91.tistory.com/entry/Spring%ED%95%84%EB%93%9C-%EC%A3%BC%EC%9E%85Field-Injection-%EB%8C%80%EC%8B%A0-%EC%83%9D%EC%84%B1%EC%9E%90-%EC%A3%BC%EC%9E%85Constructor-Injection%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)

#### `@RestController` 사용
- `@RestController`는 `@Controller`와 `@ResponseBody`를 합친 것으로, 흔히 RestFull API 웹 서비스를 구현할 때 사용한다.

#### `setter` 메서드는 도메인의 핵심 개념이나 의도를 코드에서 사라지게 한다.
- `setter()`와 같은 기능이라도 메서드 이름을 의도에 맞게 변경해야 한다.
- 예를 들어, `update()`로 자주 사용함

#### 컨트롤러 네임 컨벤션
- 참고 링크: <https://www.slipp.net/questions/79>

#### DTO은 어디까지 노출해야 하는가?
- 참고 링크: <https://www.slipp.net/questions/93>

#### 여러 쓰레드에서 접근할 수 있는 부분은 동기화해야 한다.
- 참고 링크: <http://redutan.github.io/2016/03/13/effective-java2-chapter10>

#### 스프링부트 프로퍼티 우선순위
- 참고 링크: <https://engkimbs.tistory.com/765>

#### REST 설계시 주의할 점
- 슬래시 구분자(`/`)는 계층 관계를 나타내는데 사용한다.
- URI 마지막 문자로 슬래시(`/`)를 포함하지 않는다.
- 하이픈(`-`)은 URI 가독성을 높이는데 사용한다.
- 밑줄(`_`)은 URI에 사용하지 않는다.
- URI 경로에는 소문자가 적합하다.
- 파일 확장자는 URI에 포함하지 않는다.

#### `@ModelAttribut` 애노테이션은 붙이지 않아도 자동으로 인식한다.
#### `Optional` 생각해보기

```java
public Article findById(int articleId) {
        return articles.stream()
                .filter(article -> article.matchId(articleId))
                .findAny()
                .orElseThrow(() -> new ArticleNotFoundException("존재하지 않는 게시물입니다."))
                ;
}
```

- 리뷰어 화투님 의견

```
객체를 조회할 때 Optional에서 바로 예외를 던지기보다는, Optional을 리턴 타입으로 제공해서 유연하게 사용하도록
해보면 어떨까요? 사용하는 도메인 레이어 측에서 orElseThrow로 예외를 던질지, 혹은 null에 대한 추가적인 처리를
할 지 선택할 수 있겠네요. :)
```

#### Constructor Injection는 `@Autowired`를 생략해도 된다.

#### REST API
- 참고 링크: <https://meetup.toast.com/posts/92>

#### 웹 인수 테스트(acceptance test)
- 리뷰어 강현수님 의견

```
지금 컨트롤러에서 한 테스트는 인수 테스트(acceptance test)로 볼 수 있습니다. 인수 테스트는 실제 운영 환경에서
사용할 준비가 되었는지 확인하는 테스트입니다. 즉, 개발자가 개입하지 않고 개발된 api나 화면을 직접 실행해보면서 문제가
없는지 점검할 수 있어야합니다.

repository 빈을 생성해서 값을 주입하게 되면 개발자가 운영 환경에 개입한게 되겠죠. 개발한 결과물이 정상 동작하는지
확인하려면 webTestClient를 사용해서 테스트해보세요. 그리고 인수 테스트가 아닌 상황에서 의존성이 있는 객체들을
테스트할 때는 mock을 사용하면 됩니다. 요건 차차 배워가실거에요.
```

#### 서비스 메서드 이름 컨벤션
- save보단 create, post가 좋다.(Repository와 이름을 다르게 하는 것을 추천)
- 서비스는 딱히 컨벤션이 없음

#### `import.sql`을 사용하면 스프링에서 자동으로 쿼리를 시작함(JPA)
#### DTO에서 도메인에서 만들거나 서비스에서 만드는 것은 취향차이
- 서비스 패키지안에 dto가 들어갈수 있다.

#### 레이어드 아키텍처에 맞게, 위는 아래를 알수있고, 아래는 위를 절대 알면 안됨
- 서포트는 독립적인 레이어

#### 서비스 테스트와 컨트롤러 테스트는 다를 수 있다.
- 컨트롤러는 단위테스트를 잘 하지 않음

#### H2 데이터베이스 설정
- 참고 링크: https://github.com/raycon/til/blob/master/framework/spring-boot-H2-database.md
- 참고 세팅

```
# application.properties
# H2 Setting
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.url=jdbc:h2:~/test;AUTO_SERVER=TRUE
spring.datasource.username=sa
spring.datasource.password=
spring.h2.console.enabled=true
spring.h2.console.path=/console
```

#### mysql 데이터베이스 설정
- 타임존 오류 해결

```
spring.datasource.url=jdbc:mysql://localhost:3306/blog?useUnicode=true&characterEncoding=utf8&allowPublicKeyRetrieval=true&useSSL=false&serverTimezone=UTC
```

- className은 현재 없어도 됨

```
spring.jpa.hibernate.ddl-auto=create
```

- create와 update의 차이
  - create를 사용해야 ```import.sql```이 동작한다.
  - create는 서버를 재시작할 때마다 테이블을 재생성한다.(데이터를 유지할 수 없음)
  - update는 서버를 재시작할 때마다 테이블이 없으면 생성하고, 있으면 수정한다.

#### 자바에서 비밀번호 해쉬화하기
- 참고 링크: <https://pjh3749.tistory.com/258>

#### Windows에서 포트 종료시키기
- 참고 링크: <https://stackoverflow.com/questions/39632667/how-to-kill-the-process-currently-using-a-port-on-localhost-in-windows>

```
netstat -ano | findstr :yourPortNumber
taskkill /PID typeyourPIDhere /F
```

- 띄어쓰기에 유의할 것

#### Dto에서도 유효성 검사를 해도 된다.
- 리뷰어 화투님 의견

> 비즈니스 레이어까지 가지 않아도 확인할 수 있는, 기본적인 nullable에 대한 체크, 혹은 간단한 패턴
> 체크 등은 DTO에서도 해주시는게 좋습니다.
> 그렇기 때문에 스프링에 JSR-303이란 스펙까지 반영이 되었구요!
> 도메인에서 하는 검증은 값검증 뿐만 아니라 조금 더 비즈니스 도메인에 가까운 비즈니스 검증도 같이
> 이루어진다고 보시면 될 것 같아요. :)

- JSR-303 참고 링크: <http://wonwoo.ml/index.php/post/1082>

#### 리다이렉트시 model 데이터 보내기
- `RedirectAttributes` 클래스 사용

#### Article 필드를 객체로 감싸서 DTO대신 VO로 사용할 수 있다.
#### 테스트에 port를 설정해두는 것은 다른 환경에서 테스트 할 때 적절치 않을 수 있다.
#### 도메인은 다른 환경에서도 돌아간다는 마인드로 구현해야한다.(DDD)
#### Article과 Comment 사이에 양방향
- 좀 더 객체 지향적
- 쿼리 조회 한번으로 가능(디비 부화를 줄임)
- 반면에 사이드 이펙트가 날 확률이 큼
#### if 조건문이 길어지면 일단 메서드로 빼서 리펙토링할 생각을 하자
#### 클래스 다이어그램 만들기 - 인텔리제이 기능 활용 가능

#### DTO 클래스에서는 필수 값, 값의 형식, 범위 등을 검사한다.
- DTO와 Repository가 연관관계가 있는 것은 어색하다.
- 유효성 검사는 해당 레이어에 속하는 검증인지를 분리하는 것이 좋다.
- 게시글 제목 중복은 허용하도록 변경
- 유저 이메일 중복은 DTO에서 하는 것이 아니라 서비스에서 하는 것이 좋겠다는 생각이 든다. 왜냐하면 이메일 중복은 레포지토리를 사용하는데 이는 서비스에서 주입받아서 사용하고 있다. 그러므로 레포지토리가 사용하는 레이어에서 유효성 검사를 하는 것이 옳다고 생각이 들고, DTO와 레포지토리는 전혀 상관이 없는 것으로 여기에 의존관계가 생기는 것이 어색하다.

#### Service에서 다른 entity를 사용할 때 repository보다 service에 의존한다.
- entity를 담당하는 service가 있다면 이를 사용하는 것이 의존관계가 쓸데없이 증가한다.
- Facade 패턴으로 서비스를 추상화하자.
  - 현재는 한 메서드에서 여러 레포지토리를 부르는 것이 1개있고, 레포지토리를 호출하는 개수도 2개로 매우 제한적이다. 그래서 아직까지는 facade패턴을 적용할 필요성이 느껴지지는 않는다.

#### Controller Naming Convention
- 참고 링크: <https://www.slipp.net/questions/79>

#### DTO와 ENTITY 사이 변환
- `ModelMapper` 클래스를 사용하는 것은 비효율적이다.
  - `setter()` 메서드가 있어야 한다.
    - ModelMapper 빈 설정으로 `setter()` 대신 리플렉션을 사용할 수도 있다.
  - 내포 클래스가 있을 때 정확히 변환되는 것을 보장할 수 없다.
    - 특히, List를 변환 시 정상적으로 동작하지 않을 수도 있다.
- `Assembler`를 사용하여 변환하는 것으로 변경
  - DTO와 ENTITY 사이 의존 관계가 필요 없다.
- sprint boot에서 여러 개의 구현체가 있는 인터페이스 ```@Autowired```하기
  - 참고 링크: <https://stackoverflow.com/questions/51766013/spring-boot-autowiring-an-interface-with-multiple-implementations>
