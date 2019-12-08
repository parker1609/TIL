# Level1 피드백 모음

#### 매직넘버는 상수로 치환한다.
- 매직넘버란? 코드 안에 작성된 구체적인 숫자 혹은 문자열을 말한다.
- 매직넘버의 단점
  - 의미를 이해하기 어렵다.
  - 복수의 장소에서 사용된다면 변경하기 비효율적이다.

#### 지역 변수는 그것을 가장 먼저 사용하는 곳 근처에 선언한다.
#### `getter()` 메서드는 클래스 하단에 위치한다.
#### 자바의 제어자(modifier) 표기 순서

```
public protected private abstract default static final transient volatile synchronized native strictfp
```

#### 변수 이름에 컬렉션 종류를 사용하지 않는다.
#### get/set은 `getter()`/`setter()` 역할을 하는 메서드에만 쓰는 것이 좋다.
#### `setter()`는 반환하는 값이 없다.
#### 생성자 다음에 정적 메서드가 위치하는 것이 좋다.
#### Google Java Style Guide를 보면 중괄호는 본문이 비어 있거나 하나의 문장만 포함하는 경우에도 사용한다
> Braces are used with if, else, for, do and while statements, even when the body is empty or contains only a single statement.

#### 메서드 순서
- 생성자
- static method
- 구현 코드(중요도 순, private이 아래로)
- toString
- equals/hashCode
- getter

#### `toString()` 사용
각 객체마다 중요한 정보를 나타낼 수 있는 `toString()` 메서드를 구현해야한다. 하지만, 단순히 입출력을 위한 `toString()`은 좋지않다. 예를 들어, 자동차 게임 미션에서 자동차의 이동을 표현하지 위해 `toString()`에서 "-"를 출력하는 것은 콘솔 입출력에만 의존하는 것이므로 지양해야한다.

#### 자바 Collection 사용

```java
public Foo(List<Integer> numbers) {
  this.numbers = new ArrayList<>(numbers);
}
```

- 한 객체의 생성자에서 `List<>`와 같은 컬렉션을 전달받을 때 새로운 컬렉션을 생성하여 대입해주어야 한다.
  - 새로 생성하지 않으면, 외부의 컬렉션과 같은 **주소를 공유하기** 때문에 외부에서 변경할 위험이 크다.

#### 컬렉션을 외부로 노출할 때는 불변으로 만들어줘야 한다.
- 콜렉션의 경우 `final` 키워드를 사용한다고 해도 참조만 변경이 불가능하고 콜렉션 자체는 변경이 가능하다. 그러므로 `Collections.unmodifableList()` 와 같은 메서드를 사용하여 불변으로 만들자.(List의 경우)

#### 재귀함수
- 자바는 꼬리재귀 최적화가 되어있지 않으므로 StackOverFlow가 발생할 확률이 크다.
- 위의 이유가 아니더라도 재귀를 악의적으로 계속 부를수있는 위험도 생각해봐야한다.

#### 2차원 배열
리뷰어 강현구님 의견
```
2차 배열 형태로 하려면 List<List<>>로 하거나 두 개의 일급 컬렉션을 만드는 것도 방법일거에요.
```

#### NullPointerException
- Null을 반환하는 것은 매우 위험하다.
- 참고 링크: <https://www.jpstory.net/2014/02/09/avoid-nullpointerexception/>

#### 특별한 이유가 있지 않는 경우 primitive type으로 구현한다.

#### `Arrays.asList()` 특징
크루 슬로스 정리 참고
- `Arrays.asList()`는 java.Arrays.ArrayList 클래스를 리턴하므로, `ArrayList<>`를 가지는 java.util.ArrayList 클래스와 다르다.
- `Arrays.asLsit()`는 내부에서 배열로 이루어져있으며, 이의 주소값을 리턴하므로 이를 변경할 수없다. 즉, 삽입, 삭제가 불가능하다. 만약 이를 수행하면 `UnsupportedOperationException` 에러가 발생한다.
  - 이를 해결하려면, `List<String> list = new ArrayList(Arrays.asList(array));` 이와 같이 새로운 리스트를 만들어야 한다.

#### 자바 스트림
- 베스트 프랙티스 참고 링크: <https://marcin-chwedczuk.github.io/java-streams-best-practices>

#### Exception 처리
- 참고 링크: <https://www.slipp.net/questions/350>, <http://www.nextree.co.kr/p3239/>

#### Matcher의 `find()`와 `matches()`의 차이점
- `find()`
  - 대상 문자열에서 해당 패턴이 일부라도 존재하면 true를 반환하고, 그 위치로 이동한다.
  - 대상 문자열에서 여러 개의 패턴이 매칭되는 경우 반복 실행이 가능하다.
- `matches()`
  - 대상 문자열 전체가 해당 패턴과 일치할 경우에만 true를 반환한다.
  - `String.matches()`도 존재하는데, 이는 정규식을 다시 컴파일하기 때문에 `Matcher.matches()`보다 성능상 좋지한다.(`Matcher.matches()`는 미리 컴파일한 정규식을 사용한다, [참고 링크](https://stackoverflow.com/questions/2469244/whats-the-difference-between-string-matches-and-matcher-matches/2469275))
- `String`을 패턴 매칭하기 위해 `Pattern`으로 바꾸는 것은 비싼 행위이다. 미리 `Pattern`으로 만들어서 사용하는 것이 좋다.

#### `printStackTrace()`는 안티패턴이다.
- 리플렉션 기반으로 추적하므로 성능에 대한 이슈가 존재한다.
  - Reflection: 객체를 통해 클래스의 정보를 분석해 내는 프로그램 기법
- **실무에서는 절대 사용하지 않는다.**

#### `final` 키워드
자바에서 `final` 키워드는 불변으로 만들어주는 것이 아니라 **재할당을 금지** 시킨다. 그러므로 만약 `List`나 `Map`과 같은 자료구조를 `final`로 선언하더라고 추가 및 삭제는 가능하다. 단지 재할당만을 막아준다. 이를 해결하기 위해 자바는 **일급 컬렉션** 이나 **래퍼 클래스** 등의 방법을 이용한다.

#### 얕은 복사와 깊은 복사
자바에서 컬렉션을 깊은 복사한다고 하면 가장 쉽게 생각하여 구현한다면 아래와 같다.

```java
copy = new ArrayList<>(origin);
```

이렇게 하면 새로운 리스트를 만들어서 원본을 복사하였기 때문에 원본에는 전혀 영향을 미치지 않을꺼라 생각한다. 하지만 자바에서는 객체 리스트를 만들때 C++에서 ```Object** objArr;```와 같이 만든다. 즉, 포인터 배열을 만들어 각 배열의 인자들은 실제 객체를 가리킨다. 즉 위의 자바코드와 같이 복사를 하면 **포인터 배열만 원본과 다른 새로운 배열을 만들고, 실제 가리키는 객체는 원본과 같은 객체를 가리킨다.**

이를 해결하여 진짜 깊은 복사를 하려면, 실제 가리키는 객체까지 원본과 똑같은 데이터로 새로 생성해주어야 한다.

- 참고 링크: <https://isooo.github.io/java/2019/01/11/shallowCopy-vs-deepCopy.html>

#### 자바 `static`
- 참고 링크: <https://12bme.tistory.com/94>

#### NullPointerException이 발생하는 경우
- null object의 인스턴스 메서드를 호출할 때
- null 필드를 수정/접근하려고 할 때
- 빈 배열 객체에 길이 속성을 가져올 때
- 배열의 null인 슬롯에 수정/접근하려고 할 때
- Throwable 값처럼 null을 던질 때

#### 객체는 단일 책임 원칙을 준수해야 한다.
- 단일 책임 원칙: 하나의 객체는 하나의 책임(일)을 가진다.

#### 정적(static) 메서드는 최대한 사용하지 않는다.
- 정적 메서드는 객체 지향이 아니므로, 이를 사용할 때는 신중해야한다.
- 참고 링크: <http://codingnuri.com/seven-virtues-of-good-object>
- 상태를 가지지 않으면 정적 메서드로 하는 것도 좋은 방법이다.
  - 여기서는 정적 메서드라고 했지만, 상태가 없다면 정적 클래스로 모든 메서드를 정적 메서드로 하는 것을 말한다.

#### Util 클래스는 모든 코드에 범용적이어야 한다.
- Util 클래스에서 특정 도메인을 호출하는 것은 좋지 않다.
- Util 클래스는 최대한 만들지 않는 것이 좋다. 전역 변수처럼 변질될 우려가 있다.
- 객체 지향 프로그래밍에서 유틸리티 클래스를 사용할 때 생각해볼 점
  - 참고 링크: <http://www.mimul.com/pebble/default/2016/01/06/1452060559741.html>

#### 리턴값이 `void`인 메소드는 지양한다.
- 객체에 메시지를 전달하고 리턴받는 형태가 테스트하기 용이하다.

#### 일급 컬렉션 적용
- 참고 링크: <https://jojoldu.tistory.com/412>
- 일급 컬렉션의 장점 4가지(자세한 사항은 위 링크 참고)
  - 비지니스에 종속적인 자료구조
  - Collection의 불변성 보장
  - 상태와 행위를 한 곳에서 관리
  - 이름있는 컬렉션

#### 자바 enum 활용하기
- 참고 링크: <http://woowabros.github.io/tools/2017/07/10/java-enum-uses.html>

#### 정적 팩토리 메서드(Static factory method)
- 장점
  - 정적 팩토리 메서드를 사용하면 일반 생성자를 사용하는 것보다 이름을 설정할 수 있으므로 가독성이 좋다.
  - 호출할 때마다 새로운 객체를 생성할 필요가 없다.
  - 하위 자료형 객체를 반환할 수 있다.
  - 형인자 자료형(parameterized type) 객체를 만들 때 편하다.
- 단점
  - 정적 팩토리 메서드만 있는 클래스라면, 생성자가 없으므로 하위 클래스를 만들지 못한다.
  - 정적 팩토리 메서드는 다른 정적 메서드와 구분하기 어렵다.(문서만으로 확인하기 어려움)

#### 빈약한 도메인 모델
- 도메인이 가져야할 로직은 반드시 도메인 모델 객체 안에 있어야한다. 이를 도메인을 컨트롤하는 서비스 계층에 있는 것은 안티패턴이다.
- 참고 링크: <https://m.blog.naver.com/PostView.nhn?blogId=muchine98&logNo=220304821784&proxyReferer=https%3A%2F%2Fwww.google.com%2F>

#### String, StringBuffer, StringBuilder의 차이점과 장단점
- 참고 링크: <https://www.slipp.net/questions/271>

#### 부동소수점 정확히 계산하기
- 참고 링크: <https://isooo.github.io/java/2019/01/17/BigDecimal.html>

#### `Comarable`과 `Comparator`의 차이점
- 참고 링크: <https://isooo.github.io/java/2019/01/21/Compare.html>

#### MVC 패턴
- Model 내부에는 View 로직이 있으면 안된다.
  - Model 내부에 콘솔을 담당하는 View 로직이 있다면, 이를 웹 페이지 View 로직으로 변경하려면 model 내부를 변경해야 한다. 이는 MVC 패턴을 하는 이유가 없어진다.(Model, View, Controller는 서로 독립적이다.)
- Controller는 상태를 가지지 않는다.

```java
// 자동차 게임 컨트롤러 예시
public class RacingGameApplication {
    public static void main(String[] args) {
        String carNames = InputView.getCarNames();
        int tryNo = InputView.getTryNo();

        RacingGame racingGame = new RacingGame(carNames, tryNo);
        RacingResult result = null;
        while(racingGame.isEnd()) {
            result = racingGame.race();
            ResultView.printResult(result);
        }
        ResultView.printWinners(result); // RacingResult에서 우승자를 구해 출력
    }
}
```

```java
// 사다리 게임 컨트롤러 예시
public static void main(String[] args) throws Exception {
    Players players = InputView.createPlayers();
    Rewards rewards = InputView.createRewards();

    Ladder ladder = LadderFactory.createLadder(players.countOfPeople(), InputView.getHeight());
    OutputView.printLadder(players, ladder, rewards);

    MatchingResult matchingResult = ladder.play();
    LadderResult result = matchingResult.map(players, rewards);

    OutputView.printResult(result);
}
/*
MatchingResult는 Map<Integer, Integer>에 대한 일급콜렉션 객체이다.
Map의 key는 시작 위치, values는 사다리를 실행한 결과 위치이다.

LadderResult는 위치 값만 가지고 있는 MatchingResult의 값을 이름과 결과로 매핑한 결과를 가지는 일급콜렉션 객체이다.
*/
```

#### enum을 사용하여 if문 제거하기
- 전략 패턴 사용하기
- 패턴이 아닌 객체 지향적 설계 방식으로도 if문을 제거할 수 있다.
- 내 코드: <https://github.com/CODEMCD/Java-simple-calculator>

#### 랜덤값을 테스트하기 좋은 방법은 DI 패턴을 사용하는 것이다.
- 자동차 경주 게임 랜덤값 테스트하기 코드: <https://github.com/CODEMCD/java-racingcar-DI>

#### DTO vs VO
- DTO(Data Transfer Object): 비즈니스 로직을 가지는 것이 아닌 서로 다른 영역간의 데이터 이동을 위한 용도로 사용한다.
  - 다른 영역 사이에서 여러 데이터가 필요할 때 여러 번의 호출을 하는 것은 매우 비효율적이다. 이 때, DTO를 사용해서 데이터를 묶어서 한번에 보낼 수도 있다.
  - 외부로부터 데이터 안정성을 확보할 수도 있다. 예를 들어, 한 객체의 데이터 일부를 DTO로 만들면 이 DTO를 변경하더라도 기존의 객체에는 영향을 끼치지 않는다.
  - View에서 Model을 사용할 때 상태를 변경하지 않는다면 도메인 모델 객체를 그대로 사용해도 된다. 하지만 상태를 변경하는 로직이 있는 객체는 DTO로 생성하는 것이 좋다.
  - 참고 링크: <https://martinfowler.com/eaaCatalog/dataTransferObject.html>
- VO(Value Object): 비즈니스 로직을 가지고 있으며, 이 객체는 불변이다.
  - VO 패턴은 불변이므로 깊은 복사에 대해 고민할 필요가 없다.
  - 참고 링크: <https://www.martinfowler.com/eaaCatalog/valueObject.html>
- DTO와 VO는 서로 다르다는 것을 인식하자.

#### double 자료형에서 0으로 나누는 오류
- 양수 0으로 나눈 경우: Double.POSITIVE_INFINITY
- 음수 0으로 나눈 경우: Double.NEGATIVE_INFINITY
- 0을 0으로 나눈 경우: Double.NaN

#### 자바 HashMap 효과적으로 사용하기
- 참고 링크: <http://tech.javacafe.io/2018/12/03/HashMap/>

#### 주석
리뷰어 제이슨님 의견

```
프로그래밍을 하다 보면 다음과 같은 느낌이 들어 코드를 삭제하지 않고 주석 처리하는 경우가 있는데요.
- 이 코드가 지금은 사용하지 않지만 언젠가 사용할 수 있을 것 같다.
- 이 코드를 내가 정말 힘들게 구현해서 삭제하기 아깝다.
- 분명 사용하지 않는 코드로 파악되지만 내가 구현한 코드가 아니라 함부로 삭제하기 힘들다.
과거에는 Subversion과 Git 같은 형상관리 툴을 사용하지 않았고, 코드를 참고할만한 곳이 많지 않았기
때문에 주석을 통해 유지하는 것이 의미가 있었을 거예요. 하지만 최근 개발 환경은 형상관리 툴을 사용하는 것이
일반적이 되어 이전에 구현한 코드를 손쉽게 원복 할 수 있게 되었어요. 오히려 지금은 코드를 얼마나 깔끔하고
가독성이 좋은 코드로 유지하는지가 더 중요하지 않을까요? 현시점에 굳이 필요 없다고 생각하는 코드가 있다면
아까운 마음을 버리고 가차 없이 삭제해보세요.
```

#### DDD(Domain Driven Development)
- 도메인 계층에서는 예외를 던지고 프리젠테이션 계층에서 모아서 처리하는 것이 좋다.
  - 도메인 계층에서는 ```try-catch```로 예외를 처리하기보다는 예외를 던지자.
- 패키지 구조
  - 참고 링크: <https://www.slipp.net/questions/36>

#### IDE 기능을 충분히 활용하자.
- IDE가 추천해주는 방법이 있으면 사용해보자.


## 테스트
#### private 메서드 테스트
- 참고 링크: <https://www.slipp.net/questions/253>

리뷰어 화투님 의견
```
private 메서드는 공개된 메서드랑 같이 테스트가 될거에요!
만약 private 메서드 내에 역할이 너무 많고 예외상황이 많아서 꼭 테스트해야한다면
내부 private 메서드가 아닌 별도 클래스로 분리해서 사용하는 방안도 생각해보시면 좋을 것 같습니다. :)
```

#### List 컬렉션 내부에 데이터가 있는지 검사할 때
- `assertThat(list).containsExactly()`를 사용할 수 있다.
  - `Exactly`를 포함한 키워드는 순서도 보장한다.

#### JUnit을 사용하면 메서드마다 "Test"라는 단어를 빼는 것이 중복을 피하는 것이다.
#### 하나의 테스트메서드에서는 하나의 `assert()`만을 사용하자.

#### 테스트 메서드 이름
- `메소드명_테스트하려는 상태_기대하는 동작` 와 같은 형태를 추천한다.
- `@DisplayName()` 어노테이션으로 설명을 자세히 해주는 것도 좋다.

#### 테스트하기 좋은 코드
- 참고 링크: <http://jwchung.github.io/testing-oh-my?fbclid=IwAR1YiYxTZYzmirVBsRqYs-uUXramUoO4E5pX0n9sdZxIWWmdtkd-A8ZtA20>

#### private 메서드에 대한 단위 테스트가 필요하다고 느낄 때가 리팩터링이 필요할 때이다.
