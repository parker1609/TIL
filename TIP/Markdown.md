# Markdown

## Markdown 문법
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

### 4. 링크
#### 4.1. 일반 링크
#### 미리보기
---

[GITHUB](https://github.com/)

---

#### 문법

```
[GITHUB](https://github.com/)
```

#### 4.2. 새창으로 링크
#### 미리보기
---

[GITHUB](https://github.com/){: target="_blank" }

---

#### 문법

```
[GITHUB](https://github.com/){: target="_blank" }
```

#### 4.3 새창으로 버튼 링크
#### 미리보기
---

[GITHUB](https://github.com/){: .btn.btn-default target="_blank" }

---

#### 문법

```
[GITHUB](https://github.com/){: .btn.btn-default target="_blank" }
```

#### 4.4 자동 링크
#### 미리보기
---

<https://github.com/>

---

#### 문법

```
<https://github.com/>
```

## 이미지 중앙 정렬하기

마크다운에서 이미지를 처리하는 방법은 두 가지 방법이 있다.

```
![이미지 이름](이미지 링크)
```

```
<img src="이미지 링크">
```

이미지 링크는 URI나 directory path이다.

이미지 중앙 정렬을 쉽게 하려면 `<img>` 태그를 사용하는 것이다. 첫 번째 방법을 사용하려면 따로 CSS 파일을 만들어야 한다. 다행히 첫 번째 이미지 링크를 가지고 두 번째에 복사해도 똑같이 동작한다.

```
<center>
    <img src="이미지 링크">
</center>
```

이미지를 중앙 정렬하는 방법은 위와 같다.
- Github에서는 이 방법이 안먹힌다. 여기서는 CSS 파일을 사용해야 할듯


## 유용한 마크다운관련 사이트
- [표 만들기](http://www.tablesgenerator.com/)
- [이모지](https://www.webfx.com/tools/emoji-cheat-sheet/)
