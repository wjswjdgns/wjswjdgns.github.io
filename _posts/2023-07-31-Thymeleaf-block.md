---

title:  "타임리프 - 블록"
tag:

- Thymeleaf

---

`<th:block>` 은 HTML 태그가 아닌 타임리프의 유일한 자체 태그이다

### BasicController 추가

```java
@GetMapping("/block")
public String block(Model model){
	addUsers(model);
	return "basic/block"; 
}
```

`/resources/templates/basic/block.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
	<meta charset="UTF-8">
	<title>Title</title>
</head>
<body>

<th:block th:each="user : ${users}">
	<div>
		사용자 이름1 <span th:text="${user.username}"></span>
		사용자 나이1 <span th:text="${user.age}"></span>
	</div>
	<div>
		요약 <span th:text="${user.username} + ' / ' + ${user.age}"></span>
	</div>
</th:block>

</body>
</html>
```

### 실행 결과

타임리프 특성상 HTML 태그안에 속성으로 기능을 정의해서 사용하는데, 위 예처럼 이렇게 사용하기 애매한 경우에 사용하면 된다. <th:block> 은 렌더링 시 제거 된다.

```html
<div>
사용자 이름1 <span>userA</span>
사용자 나이1 <span>10</span>
</div>
<div>
요약 <span>userA / 10</span>
</div>
<div>
사용자 이름1 <span>userB</span>
사용자 나이1 <span>20</span>
</div>
<div>
요약 <span>userB / 20</span>
</div>
<div>
사용자 이름1 <span>userC</span>
사용자 나이1 <span>30</span>
</div>
<div>
요약 <span>userC / 30</span>
</div>
```