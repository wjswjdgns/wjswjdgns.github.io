---

title:  "타임리프 - 리터럴"
tag:

- Thymeleaf

---

타임리프에서 문자열을 작성할 때는 언제나 `‘` 소따움표를  사용해야 한다. 하지만 예외적으로 생략을 할 수 있는데 그것은 문자열이 띄어쓰기 없이 이어져 있는 경우에 가능하다 이런 점으로 많이 오류가 많이 발생하게 된다.

**리터럴은 소스 코드 상에 고정된 값을 말하는 용어이다.**

예를 들어서 다음 코드에 “Hello”는 문자 리터럴, 10,20 는 숫자 리터럴이다. 

```java
String a = "Hello"
int a = 10 * 20
```

타임리프는 다음과 같은 리터럴이 있다.

- 문자 : `‘hello’`
- 숫자 : `10`
- Boolen : `true`, `false`
- null : `null`

타임리프에서 문자 리터럴은 항상 `‘` (작은 따옴표)로 감싸야 한다.

`<span th: text “ ‘hello ‘ “ >`

그런데 문자를 항상 `‘` 로 감싸는 것은 너무 귀찮은 일이다. 공백 없이 쭉 이어진다면 하나의 의미있는 토큰으로 인지해서 다음과 같은 작은 따옴표를 생략할 수 있다. 

`**<span th:text =”hello”>**`

**오류**

`<span th:text=”hello world!”></span>`

문자 리터럴은 원칙상 `‘` 로 감싸줘야 한다. 중간에 공백이 있어서 하나의 의미있는 토큰으로 인식되지 않는다. 

**수정**

`<span th:text” ‘hello world!’ “ > </span>`

이렇게 `‘` 로 감싸면 정상 동작한다.

**BasicController 추가**

```java
@GetMapping("/literal")
public String literal(Model model) {
 model.addAttribute("data", "Spring!");
 return "basic/literal";
}
```

**HTML**

```java
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
 <meta charset="UTF-8">
 <title>Title</title>
</head>
<body>
<h1>리터럴</h1>
<ul>
 <!--주의! 다음 주석을 풀면 예외가 발생함-->
<!-- <li>"hello world!" = <span th:text="hello world!"></span></li>-->
<li>'hello' + ' world!' = <span th:text="'hello' + ' world!'"></span></li>
 <li>'hello world!' = <span th:text="'hello world!'"></span></li>
 <li>'hello ' + ${data} = <span th:text="'hello ' + ${data}"></span></li>
 <li>리터럴 대체 |hello ${data}| = <span th:text="|hello ${data}|"></span></li>
</ul>
</body>
</html>
```

**문법정리**

- “ ’hello‘ +’ world’ ”
- “ ‘hello world!’ “
- “ ‘hello ‘ + ${data}”
- “ |hello ${data}|”

**리터럴 대체 (Literal substitutions)**

`<span th : text=”| hello ${data} | “>`

마지막의 리터럴 대체 문법을 사용하면 마치 템플릿을 사용하는 것처럼 편리하다