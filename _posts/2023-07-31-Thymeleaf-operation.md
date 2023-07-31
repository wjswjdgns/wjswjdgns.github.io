---

title:  "타임리프 - 연산"
tag:

- Thymeleaf

---

타임리프 연산은 자바와 크게 다르지 않다. HTML 안에서 사용하기 때문에 HTML 엔티티를 사용하는 부분만 주의하자

- **th : text = 를 통해서 연산이 진행된다.**

**BasicController 추가**

```java
@GetMapping("operation")
    public String operation(Model model){
        model.addAttribute("nullData", null);
        model.addAttribute("data", "Spring!");
        return "basic/operation";
    }
```

**HTML**

- 타임리프 안에서 단순히 보내는 것 없이 바로 연산이 가능하다
- (조건식) ? A : B
- ${data}? 데이터가 없습니다.
    - 데이터가 있으면 ${data} 출력
    - 없으면 “데이터가 없습니다 출력”
- ${data}? : _
    - 데이터가 있으면 ${data} 출력
    - 없으면 정적 데이터를 출력한다.

```java
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<ul>
  <li>산술 연산
    <ul>
      <li>10 + 2 = <span th:text="10 + 2"></span></li>
      <li>10 % 2 == 0 = <span th:text="10 % 2 == 0"></span></li>
    </ul>
  </li>
  <li>비교 연산
    <ul>
      <li>1 > 10 = <span th:text="1 &gt; 10"></span></li>
      <li>1 gt 10 = <span th:text="1 gt 10"></span></li>
      <li>1 >= 10 = <span th:text="1 >= 10"></span></li>
      <li>1 ge 10 = <span th:text="1 ge 10"></span></li>
      <li>1 == 10 = <span th:text="1 == 10"></span></li>
      <li>1 != 10 = <span th:text="1 != 10"></span></li>
    </ul>
  </li>
  <li>조건식
    <ul>
      <li>(10 % 2 == 0)? '짝수':'홀수' = <span th:text="(10 % 2 == 0)?'짝수':'홀수'"></span></li>
    </ul>
  </li>
  <li>Elvis 연산자
    <ul>
      <li>${data}?: '데이터가 없습니다.' = <span th:text="${data}?: '데이터가 없습니다.'"></span></li>
      <li>${nullData}?: '데이터가 없습니다.' = <span th:text="${nullData}?:'데이터가 없습니다.'"></span></li>
    </ul>
  </li>
  <li>No-Operation
    <ul>
      <li>${data}?: _ = <span th:text="${data}?: _">데이터가 없습니다.</span></li>
      <li>${nullData}?: _ = <span th:text="${nullData}?: _">데이터가 없습니다.</span></li>
    </ul>
  </li>
</ul>
</body>
</html>
```

- **비교연산** : HTML 엔티티를 사용해야 하는 부분을 조심하자
    - `>`  (gt), `<` (lt) `≥` (ge) `≤` (le), `!` (not), `==` (eq), `≠` (neq, ne)
- **조건식** : 자바의 조건식과 유사하다
- **Elvis 연산자** : 조건식의 편의 버전
- **No-Operation** : `_` 인 경우 마치 타임 리프가 실행되지 않는 것 처럼 동작한다. 이것을 잘 사용하면 HTML의 내용 그대로 활용할 수 있다. 마지막 예를 보면 데이터가 없습니다. 부분이 그대로 출력된다