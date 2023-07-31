---

title:  "타임리프 - 반복"
tag:

- Thymeleaf

---

타임리프에서 반복은 `th:each` 를 사용한다. 추가로 반복에서 사용할 수 있는 여러 상태 값을 지원한다.

**BasicController 추가**

```java
@GetMapping("/each")
    public String each(Model model) {
        addUsers(model);
        return "basic/each";
    }
    
    private void addUsers(Model model){
        List<User> list = new ArrayList<>(); 
        list.add(new User("userA", 10));
        list.add(new User("userB", 20));
        list.add(new User("userC", 30));
        
        model.addAttribute("users", list);
    }
```

`/resources/templates/basic/each.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Title</title>
</head>
<body>
<h1>기본 테이블</h1>
<table border="1">
<tr>
	<th>username</th>
	<th>age</th>
</tr>
<tr th:each="user : ${users}">
	<td th:text="${user.username}">username</td>
	<td th:text="${user.age}">0</td>
</tr>
</table>
<h1>반복 상태 유지</h1>

<table border="1">
<tr>
	<th>count</th>
	<th>username</th>
	<th>age</th>
	<th>etc</th>
</tr>
<tr th:each="user, userStat : ${users}">
	<td th:text="${userStat.count}">username</td>
	<td th:text="${user.username}">username</td>
	<td th:text="${user.age}">0</td>
<td>
		index = <span th:text="${userStat.index}"></span>
		count = <span th:text="${userStat.count}"></span>
		size = <span th:text="${userStat.size}"></span>
		even? = <span th:text="${userStat.even}"></span>
		odd? = <span th:text="${userStat.odd}"></span>
		first? = <span th:text="${userStat.first}"></span>
		last? = <span th:text="${userStat.last}"></span>
	current = <span th:text="${userStat.current}"></span>
	</td>
	</tr>
	</table>
	</body>
	</html>
```

### 반복 기능

`<tr th:each=”user : ${users} “>`

- 반복시 오른쪽 컬렉션 `(${users})` 의 값을 하나씩 꺼내서 왼쪽 변수 ( user )에 담아서 태그를 반복 실행합니다.
- th : each 는 List 뿐만 아니라 배열 `java.util.Iterable` , `java.util.Enumeration` 을 구현한 모든 객체를 반복에 사용할 수 있습니다. `Map` 도 사용할 수 있는데 이 경우 변수에 담기는 값은 `Map.Entry` 입니다.

### 반복 상태 유지

`<tr th:each=”user, userStat : ${users}” >`

반복의 두번째 파라미터를 설정해서 반복의 상태를 확인 할 수 있습니다.

두번째 파라미터는 생략 가능한데, 생략하면 지정한 변수명 `(user)` + `Stat` 가 됩니다. 

여기서는 user + Stat = userStat 이므로 생략 가능합니다.

### 반복 상태 유지 기능

- `index` : 0부터 시작하는 값
- `count` : 1부터 시작하는 값
- `size` : 전체 사이즈
- `even`, `odd` : 홀수, 짝수 여부 (`boolean`)
- `frist`, `last` : 처음, 마지막 여부 (`boolean`)
- `current` : 현재 객체