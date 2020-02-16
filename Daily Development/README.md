# 개발 일지
> 하루동안 개발 또는 코딩하면서 기록하고 싶은 부분


## 2020-02-07 개발 일지
- [IntelliJ Junit5 DisplayName 결과가 출력되지 않을 때](https://medium.com/@sorravitbunjongpean/fix-junit5-display-name-did-not-show-in-run-tab-intellij-a00c94f39679)


## 2020-02-05 개발 일지
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


## 2020-01-30 개발 일지
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


## 2020-01-29 개발 일지
### Spring Boot + Vue.js 게시판 만들기
- [구현 코드](https://github.com/CODEMCD/spring-boot-vuejs-web/tree/feature/article-vuejs)

#### Vue에서 데이터 통신하는 방법
- 부모 자신 관계의 컴포넌트에서 props ↔ event 통신
- 같은 레벨 관계의 컴포넌트에서 통신
- publish ↔ subscribe 관계의 컴포넌트에서 통신(EventBus, [https://blog.feruden.com/blog/vue-js-event-bus-event-bus-in-vue-js](https://blog.feruden.com/blog/vue-js-event-bus-event-bus-in-vue-js))
- VueX와 같은 전역 공간이 필요한 통신


## 2020-01-28 개발 일지
### Spring Boot + Vue.js 게시판 만들기
- [구현 코드](https://github.com/CODEMCD/spring-boot-vuejs-web/tree/feature/article-vuejs)
    - 게시글 관련 UI 개발했음
    - 최대한 컴포넌트를 나눠 재사용성과 가독성을 높이려고 노력함
    - Vuetify 쓰니 간단하게 UI 만들기 편한듯 함
    - 속성같은거 알아보고 위치 같은거 생각하느라 시간이 너무 빨리감