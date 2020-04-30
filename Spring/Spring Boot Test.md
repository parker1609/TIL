# Spring Boot Test

## 기본 사항
- `@RunWith(SpringRunner.class)` or `@ExtendWith(SpringExtension.class)` 가 없으면 선언된 모든 어노테이션을 무시한다.(반드시 선언해야함)
- 애플리케이션이 커질수록 모든 빈을 올리는 테스트는 매우 느려지기 때문에 테스트로서 가치가 떨어진다.

## `MockMVC` VS `WebTestClient`
- `MockMVC`는 서블릿을 Mock으로 띄운다.
    - 실제 서버에 서블릿 필터가 적용된다면, Mock 서블릿에는 적용이 안될 수 있다.
- `WebTestClient`는 비동기로 체이닝이 가능하여 더 쉽게 사용할 수 있다.

## 의문사항
- `WebTestClient`로 컨트롤러 단위 테스트(서버 안올림)를 하는데, post 요청에서는 응답 HTTP 데이터가 이상하다. 응답 코드만 보이고 나머지 데이터는 없다...get 요청은 정상적으로 가능하다. 구글링해봐도 같은 문제가 있다는 것만 있고 딱히 해결방법은 찾지 못했다.