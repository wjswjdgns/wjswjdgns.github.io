---

title:  "타임리프 - 펨플릿 레이아웃 01"
tag:

- Thymeleaf

---

 `<head>` 에 공통으로 사용하는 css, javascript 같은 정보들이 있는데 이러한 공통 정보들을 한 곳에 모아두고 공통으로 사용하지만 각 페이지마다 필요한 정보를 더 추가해서 사용하고 싶다면 아래처럼 사용한다.

```java
@GetMapping("/layout")
public String layout(){
	return "template/layout/layoutMain";
}
```

`/resources/templates/template/layout/base.html`

```html
<html xmlns:th="http://www.thymeleaf.org">
<head th:fragment="common_header(title,links)">
	<title th:replace="${title}">레이아웃 타이틀</title>
	
	<!-- 공통 -->
	<link rel="stylesheet" type="text/css" media="all" th:href="@{/css/awesomeapp.css}">
	<link rel="shortcut icon" th:href="@{/images/favicon.ico}">
	<script type="text/javascript" th:src="@{/sh/scripts/codebase.js}"></script>

	<!-- 추가 -->
	<th:block th:replace="${links}" />

</head>
```

`/resources/templates/template/layout/layoutMain.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head th:replace="template/layout/base :: common_header(~{::title},~{::link})">
	<title>메인 타이틀</title>
	<link rel="stylesheet" th:href="@{/css/bootstrap.min.css}">
	<link rel="stylesheet" th:href="@{/themes/smoothness/jquery-ui.css}">
</head>

<body>
	메인 컨텐츠
</body>

</html>
```

**결과**

```html
<!DOCTYPE html>
<html>
<head>
<title>메인 타이틀</title>
<!-- 공통 -->

<link rel="stylesheet" type="text/css" media="all" href="/css/awesomeapp.css">
<link rel="shortcut icon" href="/images/favicon.ico">
<script type="text/javascript" src="/sh/scripts/codebase.js"></script>

<!-- 추가 -->
<link rel="stylesheet" href="/css/bootstrap.min.css">
<link rel="stylesheet" href="/themes/smoothness/jquery-ui.css">

</head>
<body>
메인 컨텐츠
</body>
</html>
```

- `common_header(~{::title}, ~{::link})` 이 부분이 핵심이다.
    - `:: title` 은 현재 페이지의 title 태그들을 전달한다.
    - `:: link` 는 현재 페이지의 link 태그들을 전달한다.
    

**결과를 보자**

- 메인 타이틀이 전달한 부분으로 교체되었다.
- 공통 부분은 그대로 유지되고, 추가 부분에 전달한 `<link>` 들이 포함된 것을 확인 할 수 있다.

이 방식은 사실 앞서 배운 코드 조각을 조금 더 적극적으로 사용하는 방법이다. 쉽게 이야기해서 레이아웃 개념을 두고, 그 레이아웃에 필요한 코드 조각을 전달해서 완선하는 것으로 이해하면 된다 

### 템플릿 레이아웃 이해하기

- title, links 라는 변수로 지정 (fragment) → replace로 재배치
- title과 link를 담는다

```html
<html xmlns:th="http://www.thymeleaf.org">
**<head th:fragment="common_header(title,links)">**
	**<title th:replace="${title}">레이아웃 타이틀</title>**
	
	<!-- 공통 -->
	<link rel="stylesheet" type="text/css" media="all" th:href="@{/css/awesomeapp.css}">
	<link rel="shortcut icon" th:href="@{/images/favicon.ico}">
	<script type="text/javascript" th:src="@{/sh/scripts/codebase.js}"></script>

	<!-- 추가 -->
	**<th:block th:replace="${links}" />**

</head>

##########################################################################

<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
**<head th:replace="template/layout/base :: common_header(~{::title},~{::link})">
	<title>메인 타이틀</title>
	<link rel="stylesheet" th:href="@{/css/bootstrap.min.css}">
	<link rel="stylesheet" th:href="@{/themes/smoothness/jquery-ui.css}">
</head>**

<body>
	메인 컨텐츠
</body>

</html>
```

<img width="992" alt="Untitled" src="https://github.com/wjswjdgns/wjswjdgns.github.io/assets/55444587/c8f623ef-d5d4-468e-b29f-8eea399d85f0">

