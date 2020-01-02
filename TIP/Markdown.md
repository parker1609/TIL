# Markdown

## 잊기 쉬운 Markdown 문법
### 1. 목차 만들기
#### 미리보기
---
## 목차
- [1. 개발 팁](#개발-팁)
- [2. Spring Framework](#스프링-프레임워크)

### 개발 팁
### 스프링 프레임워크
---

#### 문법

```
## 목차
- [1. 개발 팁](#개발-팁)
- [2. Spring Framework](#스프링-프레임워크)

### 개발 팁
### 스프링 프레임워크
```

- `[보여주고 싶은 목차 이름](#이동할-헤더-이름)`
- 주의할 점은 `(#이동할-헤더-이름)` 이 부분이다.
  - 띄어쓰기는 `-`로 구분한다.
  - 영어는 반드시 소문자로 작성한다.

### 2. 접기/펼치기 버튼(Expander control)
#### 미리보기
---
<details>
<summary>접기/펼치기 버튼</summary>
<div markdown="1">

|제목|내용|
|--|--|
|1|1|
|2|10|

</div>
</details>

---

#### 문법

```
<details>
<summary>접기/펼치기 버튼</summary>
<div markdown="1">

|제목|내용|
|--|--|
|1|1|
|2|10|

</div>
</details>
```

- `<div markdown="1">`는 Markdown임을 인식하는 코드이다.

### 3. 강조 구문
#### 미리보기
*기울임*

**굵게**

~~취소선~~

#### 문법
```
*기울임*
_기울임_
**굵게**
__굵게__
~~취소선~~
```


### 참고 자료
- [목차 만들기](https://png93.github.io/markdown-link/#%EA%B0%9C%EB%B0%9C%EC%9D%84-%ED%95%98%EA%B3%A0-%EC%8B%B6%EC%96%B4%EC%9A%94)
- [접기/펼치기 버튼](https://inasie.github.io/it%EC%9D%BC%EB%B0%98/%EB%A7%88%ED%81%AC%EB%8B%A4%EC%9A%B4-expander-control/)
- [강조 구문](http://guswnsxodlf.github.io/how-to-write-markdown)


## 유용한 마크다운관련 사이트
- [표 만들기](http://www.tablesgenerator.com/)
- [이모지](https://www.webfx.com/tools/emoji-cheat-sheet/)
