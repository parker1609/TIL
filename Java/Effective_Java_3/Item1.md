# Item1. 생성자 대신 정적 팩토리 메서드를 고려하라.
- 클래스는 생성자와는 별도로 정적 팩토리 메서드(static factory method)를 제공할 수 있다.
- 정적 팩토리 메서드는 디자인 패턴의 팩토리 메서드와 다르다.

## 정적 팩토리 메서드의 장점
- **이름을 가질 수 있다.**
    - 메서드의 이름을 잘 지으면 어떤 특성을 가진 객체인지 쉽게 묘사할 수 있다.
    - 생성자는 하나의 시그니처만에 종속한다. 하나의 시그니처로 여러 동작을 원하면 정적 팩토리 메서드를 쓸 수 있다.
- **호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.**
    - 불변 클래스와 같이 미리 만들어논 인스턴스를 재활용할 수 있다.(불필요한 객체 생성을 막음)
    - 인스턴스 통제
        - 싱글톤
        - 인스턴스화 불가
        - 인스턴스가 하나임을 보장(대표적으로 Enum은 인스턴스가 하나임을 보장함)
        - ...
- **반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.**
    - 반환할 객체의 클래스를 자유롭게 선택할 수 있다.
- **입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.**
    - 반환 타입의 하위 타입이기만 하면 어떤 클래스를 반환하든 상관없다.
- **정적 팩토리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.**
    - 이런 유연함은 서비스 제공자 프레임워크를 만드는 근간이다.(대표적인 서비스 제공자 프레임워크는 JDBC)
        - 제공자는 서비스의 구현체
    - 서비스 제공자 프레임워크는 3개의 핵심 컴포넌트로 이루어진다.
        - 서비스 인터페이스: 구현체의 동작 정의(JDBC `Connection`)
        - 제공자 등록 API: 제공자가 구현체를 등록할 때 사용(JDBC `DriverManager.registerDriver`)
        - **서비스 접근 API**: 클라이언트가 서비스의 인터페이스를 얻을 때(정적 팩토리 사용, JDBC `DriverManager.getConnection`)
        - 서비스 제공자 인터페이스: 팩토리 객체에 대한 설명(핵심 컴포넌트는 아님)
    - 서비스 제공자 프레임워크는 정적 팩토리 메서드뿐 아니라 여러 패턴이 있다.(DI, Bridge Pattern 등)

## 정적 팩토리 메서드의 단점
- **상속을 하려면 public 또는 protected 생성자가 필요하여 정적 팩토리 메서드만으로는 하위 클래스를 만들 수 없다.**
- **정적 팩토리 메서드는 개발자가 찾기 어렵다.**

## 정적 팩토리 메서드에서 자주 사용되는 명명 방식
- `from`: 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드
- `of`: 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드
- `valueOf`: `from`과 `of`의 더 자세한 버전
- `instance` or `getInstance`: 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지는 않는다.
- `create` or `newInstance`: 기능은 위와 같지만, 매번 새로운 인스턴스를 생성해 반환함을 보장해야한다.
- `getType`: `getInstance`와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩토리 메서드를 정의할 때 쓴다.(Type은 반환할 객체 타입)
- `newType`: `newInstance` + `getType`
- `type`: 간결한 버전