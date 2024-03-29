# 개발 일지
> 하루동안 개발 또는 코딩하면서 기록하고 싶은 부분

## 2020-02-27
### My Archive
- [개발 일지](https://www.notion.so/5936d5d4c1774debbe3029f9eab895a0?v=b8b15b30e1664e1da9f6e19bd3d40d74)

### My Archive 데이터
```json
{
	"uri": "https://velog.io/@codemcd/Sync-VS-Async-Blocking-VS-Non-Blocking-sak6d01fhx",
	"title": "Sync VS Async, Blocking VS Non-Blocking",
	"tags": [ "Sync", "Async", "Blocking", "Non-Blocking" ],
	"type": "글"
}

{
	"uri": "https://velog.io/@codemcd/Spring-Boot-JPA-IntelliJ%EB%A1%9C-%EC%9B%B9-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-1-xdk56qmpre",
	"title": "[Spring Boot + JPA로 웹 애플리케이션 구현] 1. 프로젝트 생성",
	"tags": [ "Java", "JPA", "Spring Boot" ],
	"type": "글"
}

{
	"uri": "https://velog.io/@codemcd/Spring-Boot-JPA%EB%A1%9C-%EC%9B%B9-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EA%B5%AC%ED%98%84-1.-ATDD%EB%A1%9C-%EA%B2%8C%EC%8B%9C%EA%B8%80-CRUD-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-93k5hn8dh3",
	"title": "[Spring Boot + JPA로 웹 애플리케이션 구현] 2. ATDD로 게시글 CRUD 구현하기",
	"tags": [ "Java", "JPA", "Spring Boot" ],
	"type": "글"
}
```

## 2020-02-25
### CORS
- <https://velog.io/@wlsdud2194/cors>
- 스프링 부트에서 CORS 해결하기
    - <https://siyoon210.tistory.com/154>
    - <https://stackoverflow.com/questions/36809528/spring-boot-cors-filter-cors-preflight-channel-did-not-succeed>


## 2020-02-24
### Npm Run
`npm run serve` 다른 포트로 수행하고 싶을 때 vue cli 3.x 버전이 깔려있다면 간단하게

`npm run serve -- --port 9000` 명령어를 사용하면 된다.

- [https://stackoverflow.com/questions/47219819/how-to-change-port-number-in-vue-cli-project](https://stackoverflow.com/questions/47219819/how-to-change-port-number-in-vue-cli-project)
    - 두 번째 답변 참고

## 2020-02-20
### 스프링부터와 AWS로 혼자 구현하는 웹 서비스 책 따라하기
#### Lombok 사용시 주의 사항
`@RequiredArgsConstructor`를 사용해 생성자 주입으로 의존성을 주입 받기 위해선 `private final`로 변수를 선언해야 한다.

#### Gradle 버전 변경 오류

```
$ gradlew wrapper --gradle-version 4.10.2
zsh: command not found: gradlew
```

gradle 버전을 변경하려고 위 명령어를 수행하니 자꾸 명령어를 찾을 수 없다는 메시지가 발생했다. `brew search gradle`을 해봤을 때 gradle이 있길래 설치가 되어있는 줄 알았는데 아니었나보다.

```
$ brew install gradle
```

위 명령어를 수행해서 gradle을 설치해주고 다시 `gradlew wrapper ...`을 해보니 똑같은 메시지가 떴다.

```
$ gradle wrapper --gradle-version 4.10.2
```

위 명령어를 수행하니 정상적으로 버전이 변경되었다.
- `gradle/wrapper/gradle-wrapper.properties` 파일의 버전을 확인하면 된다.


## 2020-02-18
### My Archive 만들기
Link와 Tag JPA 관계를 Link에서 OneToMany 단방향(JoinColumn)으로 구현했는데, Tag를 이용한 조회 구현해보면서 좀 더 관계 설정에 대해 생각해봐야겠다. OneToMany에서는 양방향이 가장 최선의 방법이라고 하지만 Tag가 워낙 변경 가능성이 거의 없고, 단지 Link에서 가지고 있는 데이터에 불과하다는 생각에서 코드를 구현하는데 굳이 연관관계를 주어야 할까라는 생각을 했다. JoinColumn을 사용하면 중간에 테이블은 만들어지지 않지만, 관계를 생성할 때 Link가 가진 Tag 개수만큼 UPDATE문이 발생한다. 하지만 아직 이부분은 감수할 수 있다고 생각하지만, 데이터베이스에서 어떻게 저장되는지 모르겠다. 하나의 Tags는 여러 Link가 가질 수 있는데, 현재 Tag 테이블은 (id, name, link_id)이다. 따라서 하나의 Tag는 하나의 Link만을 가지는데, 아직 이 부분은 데이터베이스를 확인해봐야겠다. Tag로의 조회는 데이터베이스가 아닌 자바 코드에서 Link가 Tag의 정보를 다 가지고 있으므로 할 수 있지만 뭔가 찜찜하다.
- [JPA 일대다 단방향 매핑 잘못 사용하면 벌어지는 일](https://homoefficio.github.io/2019/04/28/JPA-%EC%9D%BC%EB%8C%80%EB%8B%A4-%EB%8B%A8%EB%B0%A9%ED%96%A5-%EB%A7%A4%ED%95%91-%EC%9E%98%EB%AA%BB-%EC%82%AC%EC%9A%A9%ED%95%98%EB%A9%B4-%EB%B2%8C%EC%96%B4%EC%A7%80%EB%8A%94-%EC%9D%BC/)
- <https://vladmihalcea.com/the-best-way-to-map-a-onetomany-association-with-jpa-and-hibernate/>

DTO를 만들 때 Controller의 `@RequestBody`로 매핑하려면 리플렉션을 쓰기 때문에 기본 생성자가 필요하다고 알고 있다. 그런데 없어도 동작긴한다.(아직 이유는 모름) 그런데 테스트에서 `expectBody()`에서 해당 DTO 클래스를 매핑해서 가져올 때 내부가 리스트를 가지고 있는 경우라면 바로 기본 생성자가 없다는 오류가 뜬다. 물론 기본적으로 위처럼 리플랙션때문에 기본생성자를 만들어놓으면 문제가 없지만, 이전에 기본생성자가 없어도 동작해서 만들지 않았더니 문제가 발생했다.


## 2020-02-17
### Slug 란?
슬러그(Slug)란?
슬러그(Slug)란 원래 신문이나 잡지 등에서 제목을 쓸 때, 중요한 의미를 포함하는 단어만을 이용해 제목을 작성하는 방법을 말합니다. 조사나 전치사 등을 빼고 핵심 의미를 담고 있는 단어를 조합해서 긴 제목을 간단 명료하게 표현하는 것이죠. 많은 기사를 다루어야 하는 신문 등에서 편집하는 방법인 것이죠.

워드프레스에서 사용하는 슬러그(slug)도 이런 방법에서 유래한것으로 보입니다. 특히 페이지나 포스트의 제목을 쓰면, 워드프레스에서는 이 슬러그(slug)를 자동을 생성해줍니다. 띄어쓰기는 하이픈(-)으로 대체하고, 쉼표나 마침표 등 기호를 자동으로 없애줍니다.

특히 이 Slug는 검색엔진최적화(Search Engine Optimization)에 유용하게 사용되어집니다.페이지의 주소(url)이 의미를 담고 있는 키워드를 사용하면, 검색엔진에서 더 빨리 페이지를찾아주고 정확한 검색결과를 뿌려주는데 도움이됩니다.

아래는 워드프레스 Codex의 설명입니다.
Slug
A slug is a few words that describe a post or a page. Slugs are usually a URL friendly version of the post title (which has been automatically generated by WordPress), but a slug can be anything you like. Slugs are meant to be used with permalinks as they help describe what the content at the URL is.

Example post permalink: http://wordpress.org/development/2006/06/wordpress-203/

The slug for that post is "wordpress-203".

슬러그는 페이지나 포스트를 설명하는 몇개 단어의 집합입니다.  슬러그는 페이지나 포스트의 제목을 URL 친화적으로 만든 것이죠. 워드프레스에서는 이 슬러그가 자동으로 생성됩니다. 하지만 여러분이 원하시는 어떤 것으로도 사용이 가능합니다. 슬러그는 컨텐츠의 고유주소로 사용이 되어, 컨텐츠의 주소가 어떤 내용인지를 쉽게 이해할 수 있도록 합니다.

- <http://bingles1600.blogspot.com/2012/04/slug.html>

### draw.io 클래스 다이어그램 그리기
- 클래스 UML에서 필드 추가하려면 기존 필드 복사 및 붙여넣기 후 복사된 필드를 클래스로 드래그하면 해당 필드로 추가된다.
    - 따로 기능은 찾지 못했음.


## 2020-02-07
- [IntelliJ Junit5 DisplayName 결과가 출력되지 않을 때](https://medium.com/@sorravitbunjongpean/fix-junit5-display-name-did-not-show-in-run-tab-intellij-a00c94f39679)


## 2020-02-05
- [Java + Gradle 프로젝트 생성](https://jojoldu.tistory.com/138)
- [JUnit5 + Gradle 의존성 추가](https://itbellstone.tistory.com/106)

### JPA show_sql 설정 했을 때 콘솔에 두 번 출력되는 이유
- 로깅을 설정하면 system.out과 로깅 이렇게 두 번 출력된다.
- 일반적으로 테스트나 프로그램을 실행할 때 한 번만 출력하게 하려면 로깅 레벨을 디버그로 설정한다.
- <https://kwonnam.pe.kr/wiki/java/hibernate/log>

```
logging.level.org.hibernate.type=debug
```

### IntelliJ + Spring Boot 패키지 이름
패키지 이름이 `java.xxx`로 시작하면 프로젝트가 동작하지 않는다.. 찾아보니 java로 시작하는 패키지는 금지되어 있다고 한다. 에러 모습은 아래와 같다.

- 테스트 에러
<center>
    <img src='../images/package-name-bug-1.png'>
</center>

- 프로덕션 에러

<center>
    <img src='../images/package-name-bug-2.png'>
</center>

- Reference
    - <https://stackoverflow.com/questions/5490555/prohibited-package-name-java>
    - <https://androidhuman.tistory.com/46>


## 2020-01-30
### Spring Boot + Vue.js 게시판 만들기
Vue.js를 사용하여 어떤 컨벤션이 좋은지, 어떤 구조가 좋은지 고민했었는데, 좋은 예제 프로젝트를 찾았다. Vue에서 추천하는 컨밴션을 지키고 좋은 예제를 많이 만드는 오픈소스 그룹이었다.
- [코드 링크](https://github.com/gothinkster/vue-realworld-example-app)

이를 분석해서 프론트엔드 부분을 빠르고 좋은 방향으로 구현해야겠다. 게시글 UI가 완성되면 배포를 먼저 해볼 예정이다.

프론트엔드를 구현하면서 SPA에 대해 고민해보았다. 내가 구현하고 있는 것이 정말 SPA인가에 대해 생각했는데, SPA로 발전하게 된 과정을 잘 설명해주는 링크를 보고 조금이나마 이해할 수 있었다.
- <https://poiemaweb.com/js-spa>


### 나만의 음악 플레이 리스트 만들기
엔티티의 관계를 설계하고 그림을 보기 좋게 그려주는 툴을 찾고 있는데 몇 가지 정리해보고 써봐야겠다.
- MySQL Workbench
- <https://aquerytool.com/>
    - OKKY 회원분이 직접 만든 프로그램([링크](https://okky.kr/article/312693?note=1182243))
- [무료 및 저렴한 ERD 툴 정리](https://gomcine.tistory.com/entry/ERD-%EB%8B%A4%EC%9D%B4%EC%96%B4%EA%B7%B8%EB%9E%A8-%ED%88%B4-%EC%A2%85%EB%A5%98%EC%99%80-%EC%84%A4%EC%B9%98-%EA%B2%BD%EB%A1%9C-%EC%A0%95%EB%A6%AC)


## 2020-01-29
### Spring Boot + Vue.js 게시판 만들기
- [구현 코드](https://github.com/CODEMCD/spring-boot-vuejs-web/tree/feature/article-vuejs)

#### Vue에서 데이터 통신하는 방법
- 부모 자신 관계의 컴포넌트에서 props ↔ event 통신
- 같은 레벨 관계의 컴포넌트에서 통신
- publish ↔ subscribe 관계의 컴포넌트에서 통신(EventBus, [https://blog.feruden.com/blog/vue-js-event-bus-event-bus-in-vue-js](https://blog.feruden.com/blog/vue-js-event-bus-event-bus-in-vue-js))
- VueX와 같은 전역 공간이 필요한 통신


## 2020-01-28
### Spring Boot + Vue.js 게시판 만들기
- [구현 코드](https://github.com/CODEMCD/spring-boot-vuejs-web/tree/feature/article-vuejs)
    - 게시글 관련 UI 개발했음
    - 최대한 컴포넌트를 나눠 재사용성과 가독성을 높이려고 노력함
    - Vuetify 쓰니 간단하게 UI 만들기 편한듯 함
    - 속성같은거 알아보고 위치 같은거 생각하느라 시간이 너무 빨리감