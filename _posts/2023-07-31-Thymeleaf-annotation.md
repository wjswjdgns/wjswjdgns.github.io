---

title:  "타임리프 - 주석"
tag:

- Thymeleaf

---

### BasicController

```java
@GetMapping("/comments")
    public String comments(Model model) {
        model.addAttribute("data", "Spring!");
        return "basic/comments";
    }

    private void addUsers(Model model){
        List<User> list = new ArrayList<>();
        list.add(new User("userA", 10));
        list.add(new User("userB", 20));
        list.add(new User("userC", 30));

        model.addAttribute("users", list);
    }
```

`/resources/templates/basic/comments.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>예시</h1>
<span th:text="${data}">html data</span>
<h1>1. 표준 HTML 주석</h1>
<!--
<span th:text="${data}">html data</span>
-->
<h1>2. 타임리프 파서 주석</h1>
<!--/* [[${data}]] */-->
<!--/*-->
<span th:text="${data}">html data</span>
<!--*/-->
<h1>3. 타임리프 프로토타입 주석</h1>
<!--/*/
<span th:text="${data}">html data</span>
/*/-->
</body>
</html>
```

1. **표준 HTML 주석**
    - 자바스크립트 표준 HTML 주석은 타임리프가 렌더링하지 않고, 그대로 남겨둔다.
    
2. **타임리프 파서 주석**
    - 타임리프 파서 주석은 타임리프의 진짜 주석이다. 렌더링에서 주석 부분을 제거한다
    - 태그 자체가 날아간다
    
3. **타임리프 프로토타입 주석**
    - 타임리프 프로토타입은 약간 특이한데, HTML 주석에 약간의 구문을 더했다.
    HTML 파일을 웹 브라우저에서 그대로 열어보면 HTML 주석이기 때문에 이 부분이 웹 브라우저가 렌더링하지 않는다.
    
    타임리프 렌더링을 거치면 이 부분이 정상 렌더링 된다.
    **쉽게 이야기해서 HTML 파일을 그대로 열어부면 주석처리가 되지만 타임리프를 렌더링 한 경우에만 보이는 기능**