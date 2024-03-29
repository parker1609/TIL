# 4강 Hello World 실행하기
- SQL 명령의 마지막에는 세비콜론(;)을 붙여야 실행된다.
  - 세미콜론없이 엔터를 누르면 실행되지 않고 명령을 계속 입력할 수 있는 상태로 있다.(엔터 전의 명령은 편집할 수 없다.)

### SELECT 명령 구문
- `SELECT * FROM 테이블명`
  - `SELECT`: 명령의 종류
  - `*`: 모든 열을 의미하는 메타문자.(애스터리스크라고 부름)
- `FROM`은 '구'라고 불리며, 이 외에도 여러 가지가 있다.
- 예를 들어 테이블 이름이 'simple21'이라 할 때 `SELECT * FROM sample21`은 데이터베이스에서 Hello World와 같다.

### 예약어와 데이터베이스 객체명
- `SELECT, *, FROM`과 같은 것을 **예약어** 라고 부른다.
- 데이터베이스에는 테이블 외에 다양한 데이터를 저장하거나 관리하는 '어떤 것'을 만들 수 있다. 이를 **데이터베이스 객체** 라 부른다.
  - 예를 들어 뷰(view)가 있다.
- 데이터베이스 객체는 이름을 붙여 관리한다.
  - 같은 이름으로 다른 데이터베이스 객체는 만들 수 없다.
  - 데이터베이스 객체명에는 예약어와 동일한 이름을 사용할 수 없다.
- 예약어와 데이터베이스 객체명은 대소문자를 구별하지 않는다.
  - SQL 명령과 달리 많은 데이터베이스 제품들은 데이터의 대소문자를 구별한다.
  - 설정에따라 구별하지 않을 수도 있다.

### Hello World를 실행한 결과 == 테이블
- SELECT 명령 실행 결과로 테이블이 나온다.

![SQL 첫걸음_그림2-10](https://user-images.githubusercontent.com/34755287/68540331-7c807e80-03d3-11ea-90b0-2ff190d3e501.jpeg)

- `no`과 같이 숫자만으로 구성된 데이터를 **수치형 데이터** 라고 한다.
  - 수치형 데이터는 오른쪽 정렬로 표시된다.
- 임의의 문자로 구성된 데이터를 **문자열형 데이터** 라고 한다.
  - 문자형은 왼쪽으로 정렬되어 표시된다.
- 날자와 시각을 나타내는 데이터를 **날짜시간형 데이터** 라고 하낟.
  - 날짜시간형 데이터는 왼쪽 정렬로 표시된다.

### 값이 없는 데이터 == NULL
- NULL은 특별한 데이터 값으로 아무것도 저장되어 있지 않은 상태를 의미한다.
