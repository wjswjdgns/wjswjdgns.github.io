---

title:  "타임리프 - 기본객체들"
tag:

- Thymeleaf

---

타임리프는 기본 객체들을 제공한다.

- `${#request}` - 스프링 부트 3.0 부터 제공하지 않는다.
- `${#response}` - 스프링 부트 3.0 부터 제공하지 않는다.
- `${#session}` - 스프링 부트 3.0부터 제공하지 않는다.
- `${#servletContext}` - 스프링 부트 3.0부터 제공하지 않는다.
- `${#locale}`

**주의! - 스프링 부트 3.0** 스프링 부트 3.0 부터는 `${#request}` , `${#response}` , `${#session}` , `${#servletContext}`를 지원하지 않는다. 만약 사용하게 되면 다음과 같은 오류가 발생한다

```java
Caused by: java.lang.IllegalArgumentException: The
'request','session','servletContext' and 'response' expression utility objects
are no longer available by default for template expressions and their use is
not recommended. In cases where they are really needed, they should be manually
added as context variables.

```

스프링 부트 3.0이라면 직접 `model` 에 해당 객체를 추가해서 사용해야 한다. 메뉴얼 하단에 스프링 부트 3.0에서 사용할 수 있는 예시를 적어두었다

그런데 `#request` 는 `HttpServletRequest` 객체가 그대로 제공되기 때문에 데이터를 조회하려면`request.getParameter("data")` 처럼 불편하게 접근해야 한다.

이런 점을 해결하기 위해 편의 객체도 제공한다.

- HTTP 요청 파라미터 접근: param
    - 예) ${param.paramData}
- HTTP 세션 접근: session
    - 예) ${session.sessionData}
- 스프링 빈 접근: @
    - 예) ${@helloBean.hello('Spring!')}

**주소**

[http://localhost:8080/basic/basic-objects**?paramData=HelloParam**](http://localhost:8080/basic/basic-objects?paramData=HelloParam)

```java
@GetMapping("/basic-objects")
    public String basicObjects(Model model, HttpServletRequest request,
                               HttpServletResponse response, HttpSession session) {
        session.setAttribute("sessionData", "Hello Session");
        model.addAttribute("request", request);
        model.addAttribute("response", response);
        model.addAttribute("servletContext", request.getServletContext());
        return "basic/basic-objects";
    }

// SpringBean 등록
@Component("helloBean")
    static class HelloBean{
        public String hello(String data){
            return "Hello " + data;
        }
    }
```

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<h1>식 기본 객체 (Expression Basic Objects)</h1>
<ul>
  <li>request = <span th:text="${request}"></span></li>
  <li>response = <span th:text="${response}"></span></li>
  <li>session = <span th:text="${session}"></span></li>
  <li>servletContext = <span th:text="${servletContext}"></span></li>
  <li>locale = <span th:text="${#locale}"></span></li>
</ul>
<h1>편의 객체</h1>
<ul>
  <li>Request Parameter = <span th:text="${param.paramData}"></span></li>
  <li>session = <span th:text="${session.sessionData}"></span></li>
  <li>spring bean = <span th:text="${@helloBean.hello('Spring!')}"></span></
  li>
</ul>
</body>
</html>
```

# 결과 값

## 식 기본 객체 (Expression Basic Objects)

- request = org.apache.catalina.connector.RequestFacade@5d65a449
- response = org.apache.catalina.connector.ResponseFacade@2a531415
- session = org.thymeleaf.context.WebEngineContext$SessionAttributeMap@550bee70
- servletContext = org.apache.catalina.core.ApplicationContextFacade@7b0af66b
- locale = ko_KR

## 편의 객체

- Request Parameter = HelloParam
- session = Hello Session
- spring bean = Hello Spring!