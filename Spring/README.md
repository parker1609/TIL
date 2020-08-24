# Spring Framework 관련
## `@ModelAttribute` VS `@RequestBody` VS `@RequestParam`
- `@ModelAttribute`
    - 해당 필드의 setter 함수가 없으면 null값이 들어간다.
    - 파라미터를 바로 자바 객체로 매핑하는 형식
    - 대부분 Form 태그로 들어온 데이터를 매핑하는데 사용
- `@RequestBody`
    - POST 요청에만 쓸 수 있다.(GET 요청에서 쓰면 에러)
    - MessageConverter를 사용해서 객체 변환
    - setter 함수는 필요없지만, 기본 생성자가 필요(확인해볼 것)

학습 테스트를 통해 좀 더 분석해볼 것!