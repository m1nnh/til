## [IntelliJ] Maven Spring MVC 초기 환경설정

### 프로젝트 만들기

원하는 JDK 설정 후 Next 클릭한다.

<center><img src = "https://user-images.githubusercontent.com/78870076/126158565-395ad89d-a629-4e82-bca0-edef1a4ff91d.png"></center>

다음 체크박스와 같이 `GroupId`와 `ArtifactId`를 자신이 원하는 이름으로 설정한다.

<center><img src = "https://user-images.githubusercontent.com/78870076/126158728-7c158d9f-1c87-444d-818a-f5c117fbe458.png"></center>

### 의존성 주입

자바에서 `Dependency Injection` 의존성 주입이란, 프로그래밍에서 구성요소간의 의존 관계가 소스코드 내부가 아닌 외부의 설정파일 등을 통해 정의하게 하는 디자인 패턴 중의 하나이다. 조금 더 쉽게 설명하면, 외부에서 의존 객체를 생성하여 넘겨주는 것을 의미한다.

<center><img src = "https://user-images.githubusercontent.com/78870076/126159048-b870ffca-833e-423e-a581-2e875c5a7581.png"></center>

예를 들어, A Class가 B Class를 의존할 때 B Object를 A가 직접 생성하지 않고 외부에서 생성하여 넘겨주면 의존성을 주입했다고 한다. 왼쪽은 A에서 B를 생성하는 일반적인 의존 형태이고, 오른쪽은 외부에서 의존 객체를 생성하고 주입하는 형태이다. 스프링에서는 외부에서 객체를 생성하고 넘겨주는 것을 컨테이너라고 부른다.

- pom.xml 추가

```
 <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.15.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.2.15.RELEASE</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
        </dependency>
</dependencies>
```

의존성을 주입하고 IntellJ 우측에 아래와 같은 표시를 눌러주자!!

<center><img src = "https://user-images.githubusercontent.com/78870076/126159591-cfbcb3e0-20a9-4888-889e-6f81443111f2.png"></center>

### Web.xml 생성

File -> Project Structure -> Modules `+`를 클릭한 후 `Web` 클릭!!

<center><img src = "https://user-images.githubusercontent.com/78870076/126159816-a49d6b27-e47a-42cb-909d-05a7c1c1139b.png"></center>


<center><img src = "https://user-images.githubusercontent.com/78870076/126159978-0ffba3e2-24ce-443c-bb8d-ee41b357c472.png"></center>

- 첫 번째 경로는 ../[Project name]/web/WEB-INF/web.xml
- 두 번째 경로는 ../[Project name]/web

첫 번째 것은 `web.xml` 파일을 생성하는 경로이고, 두 번째는 `Web` 디렉토리를 만드는 것이다. 만들면 아래 사진처럼 디렉토리가 만들어진 것을 확인할 수 있다.

<center><img src = "https://user-images.githubusercontent.com/78870076/126160238-e4e929cf-a44e-4a17-bd2e-fdffb9c9c7e5.png"></center>


- web.xml 추가

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

`web.xml`에서 `servlet`을 설정하는데, 서블릿에 `dispatcher-servlet`을 설정해두면 `Controller`을 찾아 사용자 요청 주소를 매핑하여 처리해준다. <span style = "background-color:#FAF4C0">프로젝트를 실행하면 Tomcat 서버가 최초로 구동되는데, 이 때 web.xml을 읽고 해당하는 설정을 준비한다.</span>

### dispatcher-servlet.xml, applicationContext.xml

web.xml 설정이 끝났으면 `WEB-INF` 디렉토리에 `dispatcher-servlet.xml`과 `applicationContext.xml` 파일을 `XML Configuration File -> Spring Config` 파일로 만들어준다.

- dispatcher-servlet.xml 추가

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <mvc:annotation-driven></mvc:annotation-driven> <!-- Annotation 활성화 -->
    <context:component-scan base-package="[Controller Dir]"></context:component-scan> <!-- Component 패키지 지정 -->

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
</beans>
```

위에 [Controller Dir]은 만약 src/java/Controller/HelloController.java 파일이 있다면, Controller을 적어주면 된다.

### index.jsp 생성

`WEB-INF` 디렉토리 안에 `views` 디렉토리를 생성하고 `JSP` 파일 형식으로 `index.jsp`를 생성한다.

<center><img src = "https://user-images.githubusercontent.com/78870076/126161581-f0df856e-32a6-43c7-99c0-dad4618a1bcb.png"></center>


현재까지 디렉토리 구조와, index.jsp 파일의 내용이다.

### Controller 

나는 src/java에 `com.spring.test` 디렉토리 안에 `HelloController.java`를 생성하였다.

```
package com.spring.test;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloController {

    public HelloController() {
    }

    @RequestMapping({"/"})
    public String hello() {
        return "index";
    }
}
```

### Artifacts 설정

Maven Project에서 사용되는 개념으로, 메이븐 프로젝트의 빌드 결과물로 나오는 개체를 artifact라고 이해하면 된다. 그래서 보통 빌드로 나오는 `JAR` 파일을 artifact라고 이해하면 편하다.

`File -> Project Structure -> Artifacts`

<center><img src = "https://user-images.githubusercontent.com/78870076/126162205-12a170ea-84bd-44b5-aa10-5541f93e2fab.png"></center>

아래 첫 번째 체크박스는 빌드의 결과물이 저장되는 디렉토리 경로와 이름이다. 나는 프로젝트 파일에 `target`이라는 폴더에 결과물을 뽑았다.

<center><img src = "https://user-images.githubusercontent.com/78870076/126162384-00d52736-493f-4fd4-b03b-b0d495a9a469.png"></center>

위에와 마찬가지로 `Archive`도 설정해주어야 한다.

<center><img src = "https://user-images.githubusercontent.com/78870076/126162797-248bdc09-2e33-4165-a4e4-21b81140e0ac.png"></center>

<center><img src = "https://user-images.githubusercontent.com/78870076/126162726-4039f195-e3a9-45a2-9b6d-809f7c43ef73.png"></center>

- exploded : 아카이브를 압축해제한 디렉토리 형태 구조
    - 압축 및 해제 과정이 불필요
    - 별도의 디렉토리에 원본 소스를 복사하여 만듦
    - 파일이 많은 경우 복사 시간이 오래걸릴 수 있다.
    - 원본 소스를 건드리지 않고 배포를 원하는 경우 적합하다.
    - 리모트 서버에 배포시 파일이 많은 경우 전송 시간이 오래걸릴 수 있다.
- archive : 아카이브(war, ear) 파일로 배포
    - 아카이브는 결국 WAS에 의해 압축이 풀린다.
    - 파일이 많을 경우 압축해제 시간이 오래걸릴 수 있다.
    - 리모트 서버에 배포시 한개의 파일만 전송하면 된다.
    - WAS에서 제공하는 업로드를 통한 배포기능 활용이 가능하다.

### Tomcat 설정

Tomcat을 설정하기 전에 Tomcat을 설치하여야 한다. Tomcat을 미리 설치 했다고 가정을 하겠다. IntelliJ 우측 상단에 빨간 체크박스를 클릭한다.

<center><img src = "https://user-images.githubusercontent.com/78870076/126163219-ae3bf21e-3f13-4648-a606-b5fe3f7d493c.png"></center>

+를 클릭후 아래 사진과 같이 `Tomcat -> Local` 선택

<center><img src = "https://user-images.githubusercontent.com/78870076/126163359-49ae4aa7-baee-4cd8-bab4-67b800eddb7c.png"></center>

첫 번째로 `Configure`를 클릭하여, 톰캣이 설치된 경로를 설정해준다. 두 번째로 포트 번호를 설정해준다. 포트 번호는 Default로 8080을 이용한다. 그리고 마지막 Fix 버튼을 클릭한다.

<center><img src = "https://user-images.githubusercontent.com/78870076/126163531-6f8e23db-4062-47a0-b919-970f9a40595c.png"></center>

아래와 같이 + 버튼을 누르고 `Artifact..`를 클릭하고, Archive나 Exploded 둘 중 아무거나 선택한다.

<center><img src = "https://user-images.githubusercontent.com/78870076/126163751-c50c4aeb-163a-44e0-995b-e718c5d19518.png"></center>

마지막으로 `Application Context`에 값을 `/`로 바꿔준다.

<center><img src = "https://user-images.githubusercontent.com/78870076/126164004-9c936a8b-c909-4b18-af19-3fec84f3b086.png"></center>


### 실행

이제 톰캣을 실행한다. 톰캣을 실행하면 아래 화면처럼 localhost:8080에 `index.jsp`에 설정한 페이지가 화면에 나올 것이다.

<center><img src = "https://user-images.githubusercontent.com/78870076/126164193-6813d941-e058-4a13-8cb2-05d8483d7703.png"></center>
