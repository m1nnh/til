## 프로젝트 환경설정

### 사전 준비물
- Java 11 설치 (저는 JDK 8 사용중)
- IDE : IntelliJ 또는 Eclipse 설치
- [스프링 부트 스타터](https://start.spring.io)

### 스프링 부트 스타터 설정
- Project : Gradle Project
- Language : Java
- Spring Boot : SNAPSHOT, M?을 제외한 가장 최신 버전
- Project Metadata
    - Group : 기업명
    - Artifact : 빌드된 결과물
- Dependencies : 어떤 라이브러리를 가져와서 쓸 것인지
    - Spring web
    - Thymeleaf

만든 hello-spring을 IntelliJ로 열면 .gradle / .idea / gradle / src / build.grdle 폴더가 있다. 

- gradle : gradle 관련 파일 폴더
- src : source 파일 폴더
    - src 폴더에는 main과 test 폴더 두개가 있는데 요즘에 test case가 그만큼 중요하다는 것을 알려준다.
- build.gradle : 스프링 부트 스타터 설정들이 들어가있다.

만든 hello-spring을 실행하고 검색 창에 `localhost:8080`을 치면

![스크린샷 2021-04-13 오후 5 41 28](https://user-images.githubusercontent.com/78870076/114523789-96388c00-9c7f-11eb-82ac-20ea969d05ef.png)

다음과 같이 뜨면 성공한 것이다. 여기까지 완료하면 스프링 설정은 끝이다.

- `SpringApplication.run` : 자기가 띄우면서 톰캣이라는 웹서버를 내장 되어 있고 자동으로 창을 띄워준다.

### 라이브러리
스프링이 톰캣 웹 서버를 내장한 스프링 부트가 나오면서 기존에 사용 했던 방식에서 개발 친화적으로 변했다. 실행만 해도 웹서버가 뜨는 것처럼 라이브러리 하나 올려서 실행하면 된다.

- spring-boot-starter-web
    - spring-boot-start-tomcat : 톰캣 웹서버
    - spring-boot-webmvc : 스프링 웹 MVC
- spring-boot-starter-thymeleaf : 템플릿 엔진
- spring-boot-starter
    - spring-boot
        - spring-core
    - spring-boot-starter-logging
        - logback
        - slf4j
- spring-boot-starter-test
    - junit : 테스트 프레임워크
    - mockito : 목 라이브러리
    - assertj : 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
    - spring-test : 스프링 통합 테스트 지원

### View 환경설정

```
<!DOCTYPE HTML>
<html>

<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
Hello
<a href="/hello">hello</a>
</body>
</html>
```

아래 코드를 src 폴더에서 main 폴더에서 resource 폴더중 static 폴더에서 index.html 파일을 만들고 위의 코드를 작성한다. 그리고 서버를 중지했다가 다시 키고 `localhost:8080`에 들어간다.

<img width="418" alt="스크린샷 2021-04-13 오후 6 12 02" src="https://user-images.githubusercontent.com/78870076/114528214-c4b86600-9c83-11eb-99c5-e3be384af83c.png">

그러면 위와 같이 뜰 것이다. 뷰의 실행 방법은 static에 있는 index.html을 찾고 실행한다.

`Controller`
```
package hello.hellospring.Controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController {
    @GetMapping("hello")
    public String hello(Model model) {
        model.addAttribute("data", "hello");
        return "hello";
    }

}
```

`hello.html`
```
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
<p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
</body>
</html>
```


`localhost:8080/hello` 입력하면

![스크린샷 2021-04-13 오후 6 25 05](https://user-images.githubusercontent.com/78870076/114530142-a0f61f80-9c85-11eb-8bfc-86a593fbd5df.png)

다음과 같이 동적으로 창이 생성된다. 동작 환경을 살펴 보면

<img width="836" alt="스크린샷 2021-04-13 오후 6 23 48" src="https://user-images.githubusercontent.com/78870076/114530127-9e93c580-9c85-11eb-8be9-ecb23fe50339.png">

컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리볼버가 화면을 찾아서 처리한다. 
- resources:templates / +(Viewname) + .html

### 빌드하고 실행하기
커맨드 창을 열고 `hello-spring` 폴더로 들어가 `./gradlw build`를 입력하면 `build` 폴더가 생긴다. 그리고 `build/library`로 들어가 `java -jar 파일이름` 을 실행하면 실행이 된다.

## Reference

인프런 강의 [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근기술](https://www.inflearn.com/course/스프링-입문-스프링부트)