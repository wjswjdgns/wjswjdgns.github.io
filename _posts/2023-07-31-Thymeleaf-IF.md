---

title:  "타임리프 - 조건부 평가"
tag:

- Thymeleaf

---

타임리프의 조건식

`if`, `unless` (`if` 의 반대

### BasicController 추가

```java
@GetMapping("/condition")
    public String condition(Model model){
        addUsers(model);
        return "basic/condition";
    }

private void addUsers(Model model){
        List<User> list = new ArrayList<>();
        list.add(new User("userA", 10));
        list.add(new User("userB", 20));
        list.add(new User("userC", 30));

        model.addAttribute("users", list);
    }
```

`/resources/templates/basic/condition.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>if, unless</h1>
<table border="1">
    <tr>
        <th>count</th>
        <th>username</th>
        <th>age</th>
    </tr>
    <tr th:each="user, userStat : ${users}">
        <td th:text="${userStat.count}">1</td>
        <td th:text="${user.username}">username</td>
        <td>
            <span th:text="${user.age}">0</span>
            <span th:text="'미성년자'" th:if="${user.age lt 20}"></span>
            <span th:text="'미성년자'" th:unless="${user.age ge 20}"></span>
        </td>
    </tr>
</table>
<h1>switch</h1>
<table border="1">
    <tr>
        <th>count</th>
        <th>username</th>
        <th>age</th>
    </tr>
    <tr th:each="user, userStat : ${users}">
        <td th:text="${userStat.count}">1</td>
        <td th:text="${user.username}">username</td>
        <td th:switch="${user.age}">
            <span th:case="10">10살</span>
            <span th:case="20">20살</span>
            <span th:case="*">기타</span>
        </td>
    </tr>
</table>
</body>
</html>
```

### if, unless

타임리프는 해당 조건이 맞지 않으면 태그 자체를 렌더링하지 않는다.

만약 다음 조건이 `false` 인 경우 `<span> … </span>` 부분 자체가 렌더링 되지 않고 사라진다.

`<span th :text “ ‘미성년자’ “ th:if=”${user.age lt 20}”></span>`

### Switch

```html
<td th:switch="${user.age}">
	<span th:case="10">10살</span>
	<span th:case="20">20살</span>
	<span th:case="*">기타</span>
```

`*` 은 만족하는 조건이 없을 때 사용하는 디폴트이다.