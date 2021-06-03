## 스프링 웹 개발 기초

### 정적 컨텐츠
- 프로그램을 할 수는 없다.
- 소스코드를 그대로 화면에 보여준다.


`정적 컨텐츠 실행 이미지`

<img width="856" alt="스크린샷 2021-04-14 오전 9 41 04" src="https://user-images.githubusercontent.com/78870076/114637990-924f4d00-9d05-11eb-9a73-9867ad6a4075.png">


### MVC와 템플릿 엔진
- MVC : Model, View, Controller
- Controller 랑 View랑 나눈다.

`Controller`
```
@GetMapping("hello-mvc")
    public String helloMvc(@RequestParam(value = "name") String name, Model model) {
        model.addAttribute("name", name);
        return "hello-template";
    }
```

`View`

```
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```

<img width="833" alt="스크린샷 2021-04-14 오전 10 02 16" src="https://user-images.githubusercontent.com/78870076/114639329-83b66500-9d08-11eb-9c2d-469b17e03451.png">

검색창에 `localhost:8080/hello-mvc`를 치면 톰캣 서버에서 Controller를 찾는다. name이 스트링으로 넘겨 받아서 return한 hello-template 즉 뷰로 넘어간다. 뷰로 넘어가서 `${name}` 모델의 key 값을 치환한다.

### API

```
@GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name) {
        return "hello " + name;
    }
```

`ResponseBody`를 사용하면 뷰 리볼버를 사용하지 않고 창을 띄울 수 있다.

```
@GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name) {
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }

    static class Hello {
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
```

`API`를 궁극적으로 사용하는 이유는 바로 `객체`를 반환하기 위해서 사용한다. 여기서 `static class Hello`에 `Getter` `Setter`는 객체를 다른 곳에서 사용하기 위해 쓴다. 현재 String name이 class Hello 안에 private로 선언 되있기 때문에 이것을 다른 곳에서 사용하기 위해서 `Getter` `Setter`를 사용한다.

`ResponseBody`
- HTTP의 BODY에 문자 내용을 직접 반환
- 뷰 리볼버 대신에 `HttpMessageConverter`가 동작한다.
- 기본 문자처리는 `StringHttpMessageConverter`
- 기본 객체처리는 `MappingJackson2HttpMessageConverter`
- byte 처리 등등 기타 여러 `HttpMessageConverter`에 기본적으로 등록되어 있다.

## Reference

인프런 강의 : [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근기술](https://www.inflearn.com/course/스프링-입문-스프링부트)
