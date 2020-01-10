# Spring 프레임워크와 Spring Boot
## Spring Framework
스프링 프레임워크는 자바 애플리케이션을 개발하는데 필요한 많은 기능을 제공하는 프레임워크이다. 스프링 프레임워크에서 제공하는 대표적인 모듈은 다음과 같다.
- Spring JDBC
- Spring MVC
- Spring Security
- Spring AOP
- Spring ORM
- Spring Test

스프링에서는 모듈들을 **Dependency Injection** 으로 제공하며, 이 모듈을 사용하면 개발하는 시간을 크게 단축시킬 수 있다.


## Spring Boot
스프링 부트는 기본적으로 스프링 프레임워크를 확장한 것으로 **스프링을 사용하기 위해 필요한 기본 설정을 스프링 부트에서 제공함으로써 보일러플레이트를 제거하였다.**

> 보일러플레이트(Boilerplate)는 수정하지 않거나 최소한의 수정만을 거쳐 여러 곳에 필수적으로 사용되는 코드를 말한다.

스프링을 사용하기 위해서는 많은 설정이 필요하므로 개발을 시작하는데 오래걸렸다. 이러한 문제점을 해결하기 위해 스프링 부트가 만들어졌다. 스프링 부트를 사용하면 빠르고 효율적으로 **실행** 할 수 있는 독립적인 상용가능한 스프링 기반 애플리케이션을 만들 수 있다.

스프링 부트는 스프링과 달리 다음과 같은 특징을 가진다.
- 빌드와 설정을 간단하게 하기 위해 **starter** dependencies를 제공한다.
- 애플리케이션 배포의 복잡성을 줄이기 위해 **내장 서버** 를 제공한다.
- Auto Configuration 기능

### Starters
Starters는 프로젝트를 시작할 때 필요하거나, 프로젝트를 빠르고 안정적으로 실행시키기 위한 많은 의존 라이브러리를 포함하고 있다. 이 라이브러리는 의존관계전이(transitive dependencies)가 가능하므로, 라이브러리 내부에서 다른 라이브러리를 의존하고 있다면 해당 라이브러리까지 같이 받아온다.

- 자세한 starters 종류는 이 [링크](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#using-boot-starter)에서 확인할 수 있다.

### 편리한 의존성 설정 및 관리
Auto configuration 기능을 통해 기존의 스프링보다 의존성 설정과 관리가 편리해졌다. 예를들어 DB 설정을보자.

#### 스프링에서 DB 설정

```java
@EnabldeTransactionManagement
@Configuration
public class DatabaseConfig {
  @Bean
  public DataSource dataSource(
  @Value("${db.driver}") Class<Driver> driverClass,
  @Value("${db.url}") String url,
  @Value("${db.user}") String user,
  @Value("${db.password}") String password
  ) {
    return new SimpleDriverDataSource(
      BeanUtils.instantiateClass(driverClass),
      url,
      user,
      password);  
  }
}
```

#### 스프링 부트에서 DB 설정
1. 의존성 추가: `spring-boot-starter-jdbc`
2. 환경변수 추가: application.properties(또는 application.yml) 파일을 생성한 후 아래 내용 추가(H2 데이터베이스 설정)

```
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
```

### 내장서버
스프링부트는 기본적으로 톰캣을 내장 서버로 제공한다. 기존 스프링에서는 외부 톰캣을 사용해야했으므로 이를 미리 설치하고 설정해주는 등 복잡한 과정을 거쳤다. 하지만 스프링 부트에서는 톰캣이 내장되어 있으므로 바로 애플리케이션을 실행할 수 있다.
- 톰캣을 jetty 등으로 다른 WAS로 변경가능하다.
- 서버가 내장되어 있으므로 war가 아닌 jar파일로 배포할 수 있다.

> jar파일로 배포할 수 있다는 말은 아마도 아래와 같은 명령어로 jar 파일만 만들어 실행하면 서버가
> 실행되므로 이로만 배포가 가능하다는 말로 이해하였다.
>
> $ java -jar build/libs/web-service-0.0.1-SNAPSHOT.jar


## 참고자료
- https://www.baeldung.com/spring-vs-spring-boot
- https://github.com/ihoneymon/translate-spring-boot-reference
- https://jojoldu.tistory.com/43
- https://www.slideshare.net/sbcoba/2016-deep-dive-into-spring-boot-autoconfiguration-61584342
