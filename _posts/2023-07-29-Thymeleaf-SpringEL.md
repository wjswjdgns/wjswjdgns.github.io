---

title:  "타임리프 - SpringEL"
tag:

- Thymeleaf

---

**변수 표현식** : `${…}`

그리고 이 변수 표현식에는 스프링 EL 이라는 스프링이 제공하는 표현식을 사용할 수 있다. 

### SpringEL 다양한 표현식 사용

```java
// @Data로 했기 때문에 getter setter가 자동으로 적용되어 있다.
@Data
    static class User {
        private String username;
        private int age;

        public User(String username, int age) {
            this.username = username;
            this.age = age;
        }
    }

@GetMapping("/variable")
    public String variable(Model model){
        User userA = new User("userA", 10);
        User userB = new User("userB", 20);

        List<User> list = new ArrayList<>();
        list.add(userA);
        list.add(userB);

        Map<String, User> map = new HashMap<>();
        map.put("userA", userA);
        map.put("userB", userB);

        model.addAttribute("user", userA);
        model.addAttribute("users", list);
        model.addAttribute("userMap", map);

        return "basic/variable";
    }
```

```html
<h1>SpringEL 표현식</h1>
<ul>Object
    <li>${user.username} = <span th:text="${user.username}"></span></li>
    <li>${user['username']} = <span th:text="${user['username']}"></span></li>
    <li>${user.getUsername()} = <span th:text="${user.getUsername()}"></span></li>
</ul>
<ul>List
    <li>${users[0].username} = <span th:text="${users[0].username}"></span></li>
    <li>${users[0]['username']} = <span th:text="${users[0]['username']}"></span></li>
    <li>${users[0].getUsername()} = <span th:text="${users[0].getUsername()}"></span></li>
</ul>
<ul>Map
    <li>${userMap['userA'].username} = <span th:text="${userMap['userA'].username}"></span></li>
    <li>${userMap['userA']['username']} = <span th:text="${userMap['userA']['username']}"></span></li>
    <li>${userMap['userA'].getUsername()} = <span th:text="${userMap['userA'].getUsername()}"></span></li>
</ul>
```

**Object : 객체 접근**

- `user.username` : user의 username을 프로퍼티 접근
→ user.getUsername()
- `user[’username’]` 
→ user.getUsername()
- `user.getUsername()`
→ user의 getUsername()을 직접 호출

**List ( 인덱싱으로 값을 찾은 다음 프로퍼티 접근 )**

- `user[0].username` : List에서 첫번째 회원을 찾고 username 프로퍼티 접근
→ List.get(0).getUsername()
- `users[0][’username’]`
→ List.get(0).getUsername()
- `users[0].getUsername()` : List에서 첫 번째 회원을 찾고 메서드 직접 호출

**Map ( 키 값으로 Value 값을 찾은 다음 프로퍼티 접근 )**

- `userMap[’userA’] username` : Map에서 userA를 찾고, username 프로퍼티 접근 →
    
    `map.get(”userA”).getUsername()`
    
- `userMap[’userA’][’username’]`
`map.get(”userA”).getUsername()`
- `userMap[’userA’].getUsername()` : Map에서 UserA를 찾고 메서드 직접 호출

### 지역 변수 선언

`th : with` 를 사용하면 지역 변수를 선언해서 사용할 수 있다. 지역 변수는 선언한 태그 안에서만 사용할 수 있다.

`/resource/templates/basic/variable.html` 추가

```html
<h1>지역 변수 - (th:with)</h1>
<div th:with="first=${users[0]}">
 <p>처음 사람의 이름은 <span th:text="${first.username}"></span></p>
</div>
```