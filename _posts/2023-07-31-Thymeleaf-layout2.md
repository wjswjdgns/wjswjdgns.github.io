---

title:  "타임리프 - 템플릿 레이아웃2"
tag:

- Thymeleaf

---

### 템플릿 레이아웃 확장

앞서 이야기한 개념을 `<head>` 정도만 적용하는게 아니라 `<html>` 전체에 적용할 수 있다.

```java
@GetMapping("/layoutExtend")
public String layoutExtends(){
	return "template/layoutExtend/layoutExtendMain";
}
```

`/resources/templates/template/layoutExtend/layoutFile.html`

```html
<!DOCTYPE html>
<html th:fragment="layout (title, content)" xmlns:th="http://
www.thymeleaf.org">
<head>
	<title th:replace="${title}">레이아웃 타이틀</title>
</head>
<body>
<h1>레이아웃 H1</h1>
<div th:replace="${content}">
	<p>레이아웃 컨텐츠</p>
</div>
<footer>
	레이아웃 푸터
</footer>
</body>
</html>
```

`/resources/templates/template/layoutExtend/layoutExtendMain.html`

```html
<!DOCTYPE html>
<html th:replace="~{template/layoutExtend/layoutFile :: layout(~{::title}, ~{::section})}" xmlns:th="http://www.thymeleaf.org">
<head>
	<title>메인 페이지 타이틀</title>
</head>
<body>
<section>
	<p>메인 페이지 컨텐츠</p>
	<div>메인 페이지 포함 내용</div>
</section>
</body>
</html>
```

**생성 결과**

```html
<!DOCTYPE html>
<html>
<head>
<title>메인 페이지 타이틀</title>
</head>
<body>
<h1>레이아웃 H1</h1>

<section>
	<p>메인 페이지 컨텐츠</p>
	<div>메인 페이지 포함 내용</div>
</section>

<footer>레이아웃 푸터</footer>

</body>
</html>
```

`layoutFile.html` 을 보면 기본 레이아웃을 가지고 있는데, `<html>` 에 `th:fragment` 속성이 정의되어 있다. 이 레이아웃 파일을 기본으로 하고 여기에 필요한 내용을 전달해서 부분부분 변경하는 것으로 이해하면 된다.

`layoutExtendMain.html` 는 현재 페이지인데, `<html>` 자체를 `th : replace` 를 사용해서 변경하는 것을 확인 할 수 있다. 결국 `layoutFile.html` 에 필요한 내용을 전달하면서 `<html>` 자체를 `layoutFile.html`로 변경한다.

**TIP**

공통된 것을 처리하기 위한다면 해당 방법을 사용하면 되며.. 페이지 개수가 적을 경우 부분적으로 바꾸는 것을 사용하는 것도 좋은 방법이다.

<img width="1159" alt="Untitled" src="https://github.com/wjswjdgns/wjswjdgns.github.io/assets/55444587/65d281b7-7a39-4838-b221-9179316dbdaf">

결국 점점 확대해서 사용하는 것일 뿐 근본적으로 같은 원리로 사용하고 있다는 것을 확인 할 수 있다.
