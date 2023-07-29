---

title:  "타임리프 - Text, UTEXT"
tag:

- Thymeleaf

---

타임리프는 기본적으로 HTML 태그의 속성에 기능을 정의해서 동작한다. HTML 콘텐츠 ( content) 에 데이터를 출력할 때는 다음과 같이 `th : text` 를 사용하면 된다. `<span th : text =”${data}”>`

HTML 태그의 속성이 아니라 HTML 콘텐츠 영역 안에서 직접 데이터를 출력하고 싶으면 다음과 같이 [[…]] 를 사용하면 된다. 

`컨텐츠 안에서 직접 출력하기 **[[ ${data} ]]**`

**TIP**

- 컨텐츠 안에서 직접 출력할 경우에는 Javascript 에 적용할 때 사용하면 유용

### Escape

HTML 문서는 `<` `>` 같은 특수 문자를 기반으로 정의된다. 따라서 뷰 템플릿으로 HTML 화면을 생성할 때는 출력하는 데이터에 이러한 특수 문자가 있는 것을 주의해서 사용해야 한다.

→ 컨트롤러에서 다음과 같이 View 에 보냈다고 한다면

```java
@GetMapping("text-unescaped")
    public String textUnescaped(Model model){
        model.addAttribute("data", "Hello <b>Spring!</b>");
        return "basic/text-unescaped";
    }
```

이를 아래의 경우에 각각 다르게 결과가 노출이 된다. 

- th : text = Hello <b> Spring! </b>
- th:utext = Helllo **Spring!**

**주의!**

실제 서비스를 개발하다 보면 escape를 사용하지 않아서 HTML이 정상 렌더링 되지 않는 수 많은 문제가 발생한다. escape를 기본으로 하고, 꼭 필요할 때만 unescape 를 사용하자.