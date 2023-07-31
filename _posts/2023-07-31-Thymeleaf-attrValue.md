---

title:  "타임리프 - 속성 값 설정"
tag:

- Thymeleaf

---

타임리프는 주로 HTML 태그에 `th:*` 속성을 지정하는 방식으로 동작한다. `th:*` 로 속성을 적용하면 기존 속성을 대체한다. 기존 속성이 없으면 새로 만든다.

**BasicController 추가**

```java
@GetMapping("/attribute")
  public String attribute() {
      return "basic/attribute";
  }
```

`/resources/templates/basic/attribute.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<h1>속성 설정</h1>
<input type="text" name="mock" th:name="userA" />
<h1>속성 추가</h1>
- th:attrappend = <input type="text" class="text" th:attrappend="class='
large'" /><br/>
- th:attrprepend = <input type="text" class="text" th:attrprepend="class='large'" /><br/>
- th:classappend = <input type="text" class="text" th:classappend="large" /><br/>
<h1>checked 처리</h1>
- checked o <input type="checkbox" name="active" th:checked="true" /><br/>
- checked x <input type="checkbox" name="active" th:checked="false" /><br/>
- checked=false <input type="checkbox" name="active" checked="false" /><br/>
</body>
</html>
```
<img width="413" alt="Untitled" src="https://github.com/wjswjdgns/wjswjdgns.github.io/assets/55444587/f92c569d-8c3b-4da1-8be6-f3c799fa0f46">


**속성 설정**

th:* 속성을 지정하면 타임리프는 기존 속성을 th:* 로 지정한 속성으로 대체한다. 기존 속성이 없다면 새로 만든다.

`<input type=”text” name=”mock” th:name=”userA” />`

→ 타임리프 렌더링 후 `<input type=”text” name=”userA” />`

**속성 추가**

`th:attrappend` : 속성 값의 뒤에 값을 추가한다.

`th:attrprepend` : 속성 값의 앞에 값을 추가한다. 

`th:classappend` : class 속성에 자연스럽게 추가한다.

**checked 처리**

HTML 에서는 `<input type=”checkbox” name=”activate” checked=”false” />` → 이 경우에도 checked 속성이 있기 때문에 checked 처리가 되어버린다.

<img width="158" alt="Untitled 1" src="https://github.com/wjswjdgns/wjswjdgns.github.io/assets/55444587/a7165cb3-1797-4455-8307-78cf69f690f2">


HTML에서 `checked` 속성은 `checked` 속성의 값과 상관없이 `checked` 라는 속성만 있어도 체크가 된다.

이런 부분이 `true`, `false` 값을 주로 사용하는 개발자 입장에서는 불편하다

**타임리프의 th:checked 는 값이 false 인 경우 checked 속성 자체를 제거한다.**

`<input type=”checkbox” name=”activate” th:checked=”false” />`

→ 타임리프 렌더링 후 `<input type=”checkbox” name=”active” />`
