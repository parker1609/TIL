## Javascript - JSON

```javascript
var person = {"height": 174, "job": "programmer"}
```

자바스크립트는 위와 같이 ```{}``` 를 선언한 부분을 객체로 만들어준다. 따라서, 위 예제는 ```height=174```이고, ```job=programmer```인 ```person``` 객체를 만든 것이다.

```javascript
var members = ["egoing", "k8805", "sorialgi"]
```

위는 ```[]```로 선언되어 있는데, 이는 자바스크립트에서 배열을 뜻한다.

이처럼 자바스크립트에서 언어 문법은 서버에서 사용하는 자바나 파이썬같은 다른 언어들과 다르다. 따라서, 프론트앤드에서 자바스크립트로 만든 배열과 같은 데이터를 **백앤드 서버에서 바로 사용하기 매우 불편하다.**

이러한 불편함을 해소하기 위해서 공통의 규칙을 만들었다. 이것이 바로 **JSON(JavaScript Object Notation)** 이다. JSON의 표현식은 사람과 기계가 이해하기 쉬우며, 데이터의 용량이 작다. 그래서 기존 XML을 대체하여 설정의 저장이나 데이터를 전송 등에 많이 사용된다.

JSON은 여러 프로그램에서 설정 파일로도 자주 사용한다. 아래는 서브라임텍스트의 설정파일의 일부이다.

```json
{
  "font_face": "Inconsolata",
  "font_size": 30,
  "ignored_packages":
  [
  ],
  "line_numbers": true
}
```

이 설정 JSON 파일을 자바스크립트에서 사용하면 바로 객체를 만들 수 있다.

```javascript
var info = {
  "font_face": "Inconsolata",
  "font_size": 30,
  "ignored_packages":
  [
  ],
  "line_numbers": true
}
```

위보다 자바스크립트에서는 문자열로 저장하는 것이 더 의미가 있다. 자바스크립트에서 줄바꿈을 위해 ```\```를 사용해야 한다.

```javascript
var info = '{\
  "font_face": "Inconsolata",\
  "font_size": 30,\
  "ignored_packages":\
  [\
  ],\
  "line_numbers": true\
}'
```

이를 ```info```를 콘솔창에 입력하면

```
"{ "font_face": "Inconsolata", "font_size": 30, "ignored_packages": [ ], "line_numbers": true }"
```

이를 JSON 형식의 데이터를 자바스크립트에서 객체로 만들어주려면

```javascript
var infoobj = JSON.parse(info);
```

```
Object { "font_face": "Inconsolata", "font_size": 30, "ignored_packages": Array[0], "line_numbers": true }
```

```javascript
var infostr = JSON.stringify(infoobj);
```

위는 자바스크립트 객체를 JSON 형식의 문자열로 만든다.
