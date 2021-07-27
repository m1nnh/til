## [IntelliJ] Maven Spring MVC + MySQL + MyBatis

xml로 초기 환경 설정한 것은 지난번에 포스팅 하였다.

[IntelliJ Maven Spring MVC 환경설정](https://minhyeok-rithm.tistory.com/entry/Spring-Environment?category=851909)

### 의존성 주입(pom.xml)

```
    <!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-api -->
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter-api</artifactId>
      <version>5.7.0</version>
<!--      <exclusions>-->
<!--        <exclusion>-->
<!--          <groupId>org.hamcrest</groupId>-->
<!--          <artifactId>hamcrest-core</artifactId>-->
<!--        </exclusion>-->
<!--      </exclusions>-->
      <scope>test</scope>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.hamcrest/hamcrest-library -->
    <dependency>
      <groupId>org.hamcrest</groupId>
      <artifactId>hamcrest-library</artifactId>
      <version>2.2</version>
      <scope>test</scope>
    </dependency>

    <!-- https://mvnrepository.com/artifact/com.jayway.jsonpath/json-path -->
    <dependency>
      <groupId>com.jayway.jsonpath</groupId>
      <artifactId>json-path</artifactId>
      <version>2.6.0</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.18.20</version>
      <scope>provided</scope>
    </dependency>


    <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.7</version>
    </dependency>

    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>2.0.6</version>
    </dependency>

    <dependency>
      <groupId>commons-dbcp</groupId>
      <artifactId>commons-dbcp</artifactId>
      <version>1.4</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.3.7</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.25</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.12.3</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.datatype/jackson-datatype-jsr310 -->
    <dependency>
      <groupId>com.fasterxml.jackson.datatype</groupId>
      <artifactId>jackson-datatype-jsr310</artifactId>
      <version>2.12.3</version>
    </dependency>


    <!-- 추가 -->
    <!-- https://mvnrepository.com/artifact/ch.qos.logback/logback-classic -->
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.2.3</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
    </dependency>


    <!-- https://mvnrepository.com/artifact/org.mockito/mockito-junit-jupiter -->
    <dependency>
      <groupId>org.mockito</groupId>
      <artifactId>mockito-junit-jupiter</artifactId>
      <version>3.7.7</version>
      <scope>test</scope>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.hamcrest/hamcrest-library -->
    <dependency>
      <groupId>org.hamcrest</groupId>
      <artifactId>hamcrest-library</artifactId>
      <version>2.2</version>
      <scope>test</scope>
    </dependency>

    <!-- https://mvnrepository.com/artifact/com.jayway.jsonpath/json-path -->
    <dependency>
      <groupId>com.jayway.jsonpath</groupId>
      <artifactId>json-path</artifactId>
      <version>2.6.0</version>
    </dependency>

    <dependency>
      <groupId>org.hibernate.validator</groupId>
      <artifactId>hibernate-validator</artifactId>
      <version>6.0.10.Final</version>
    </dependency>

    <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger2</artifactId>
      <version>3.0.0</version>
    </dependency>

    <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger-ui</artifactId>
      <version>2.9.2</version>
    </dependency>

    <dependency>
      <groupId>org.glassfish</groupId>
      <artifactId>javax.el</artifactId>
      <version>3.0.0</version>
    </dependency>

    <dependency>
      <groupId>org.hamcrest</groupId>
      <artifactId>hamcrest-library</artifactId>
      <version>2.2</version>
      <scope>test</scope>
    </dependency>
```

### Config 파일

xml 파일 java 파일로 대체한 설정 파일

#### Web.xml -> WebAppInitializer

```
import org.springframework.web.context.WebApplicationContext;
import org.springframework.web.servlet.DispatcherServlet;
import org.springframework.web.servlet.FrameworkServlet;
import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

public class WebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[] { AppConfig.class };
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[] { WebConfig.class };
    }

    @Override
    protected String[] getServletMappings() {
        return new String[] { "/" };
    }

    @Override
    protected FrameworkServlet createDispatcherServlet (WebApplicationContext wac) {
        DispatcherServlet ds = new DispatcherServlet(wac);
        ds.setThrowExceptionIfNoHandlerFound(true);
        return ds;
    }
}
```

#### dispatcher-servlet.xml -> WebConfig

```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.ComponentScan.Filter;
import org.springframework.context.annotation.Configuration;
import org.springframework.validation.beanvalidation.MethodValidationPostProcessor;
import org.springframework.context.annotation.FilterType;
import org.springframework.context.annotation.Import;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RestControllerAdvice;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.ViewResolverRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
@EnableWebMvc
@ComponentScan(
        basePackages = {"[directory]"},
        useDefaultFilters = false,
        includeFilters = {
                @Filter(
                        type = FilterType.ANNOTATION,
                        classes = {
                                Controller.class, ControllerAdvice.class, RestController.class,
                                RestControllerAdvice.class
                        }
                )
        }
)
@Import(SwaggerConfig.class)
public class WebConfig implements WebMvcConfigurer {
    @Bean
    public MethodValidationPostProcessor methodValidationPostProcessor() {
        return new MethodValidationPostProcessor();
    }

    @Override
    public void configureViewResolvers(ViewResolverRegistry registry) {
        registry.jsp("/WEB-INF/views/", ".jsp");
    }

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("swagger-ui.html")
                .addResourceLocations("classpath:/META-INF/resources/");

        registry.addResourceHandler("/webjars/**")
                .addResourceLocations("classpath:/META-INF/resources/webjars/");
    }
}
```

#### applicationContext.xml -> AppConfig

```
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.ComponentScan.Filter;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;
import org.springframework.context.annotation.Import;
import org.springframework.stereotype.Repository;
import org.springframework.stereotype.Service;

@Configuration
@ComponentScan(
        basePackages = {"[directory]"},
        useDefaultFilters = false,
        includeFilters = {
                @Filter(
                        type = FilterType.ANNOTATION,
                        classes = { Service.class, Repository.class }
                )
        }
)
@Import(MyBatisConfig.class)
public class AppConfig {

}

```

#### MyBatisConfig

```
import javax.sql.DataSource;

import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.core.io.Resource;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.jdbc.datasource.DriverManagerDataSource;
import org.springframework.transaction.TransactionManager;


@Configuration
@PropertySource("classpath:application.properties")
@MapperScan(basePackages = { "[directory]" })
public class MyBatisConfig {
    private ApplicationContext applicationContext;

    @Value("${db.driverClassName}")
    private String driverClassName;
    @Value("${db.url}")
    private String url;
    @Value("${db.username}")
    private String username;
    @Value("${db.password}")
    private String password;

    public MyBatisConfig(ApplicationContext applicationContext) {this.applicationContext = applicationContext;}

    @Bean
    public SqlSessionFactory sqlSessionFactory() throws Exception{
        SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
        factoryBean.setDataSource(dataSource());
        Resource[] resources = applicationContext.getResources("classpath:/mybatis/mappers/*.xml");
        factoryBean.setMapperLocations(resources);
        factoryBean.setConfigLocation(applicationContext.getResource("classpath:/mybatis/settings.xml"));
        return factoryBean.getObject();
    }

    @Bean
    public DataSource dataSource()
    {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName(driverClassName);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }

    @Bean
    public TransactionManager transactionManager() {
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource());
        return transactionManager;
    }
}
```

#### recourse -> application.properties 추가

application.properties는 DB와 연결할 속성들을 추가하는 곳이다.

```
db.driverClassName={db.driverClassName}
db.url={db.url}
db.username={db.username}
db.password={db.password}
```

### Test

#### resource -> mybatis -> mappers -> TestMapper.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="[Mapper Directory]r">
    <select id="selectAll" resultType="Minh">
        /* actor.select */
        SELECT *
        FROM test
        WHERE id = 1;
    </select>
</mapper>
```

#### resource -> mybatis -> settings.xml

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
  <settings>
    <setting name="mapUnderscoreToCamelCase" value="true" />
  </settings>

  <typeAliases>
    <typeAlias type="[entity directory]" alias="[entity name]"/>

  </typeAliases>
</configuration>
```

#### [entity] Entity

```
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@AllArgsConstructor
@NoArgsConstructor
@Data
public class Minh {
    private long id;
    private long a;
    private long b;
}

```

#### Mapper

```
import [Entity Directory];
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface MinhMapper {
    Minh selectAll();
}
```

#### Repository

```
import [Mapper Directory];
import [Entity Directory];
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Repository;

@RequiredArgsConstructor
@Repository
public class MinhRepository {
    public final MinhMapper minhMapper;

    public Minh findAll() {return minhMapper.selectAll();}
}
```

#### Service

```
import [Entity Directory];
import [Repository Directory];
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;

@Service
@RequiredArgsConstructor
public class MinhService {
    private final MinhRepository minhRepository;

    public Minh findAll() {
        return minhRepository.findAll();
    }
}
```

#### Controller

```
import com.minh.capstone.common.response.ResponseMessage;
import [Entity Directory];
import [Service Directory];
import lombok.RequiredArgsConstructor;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequiredArgsConstructor
@Validated
public class MinhController {
    private final MinhService minhService;

    @GetMapping("/minh")
    public ResponseMessage<Minh> findAll() {
        return new ResponseMessage<>(minhService.findAll());
    }
}
```

### 결과

tomcat, artifacts 설정은 초기 설정 xml 설정에서 이미 했으므로 생략한다. MinhMapper.xml에 작성한 쿼리문을 볼 수 있을 것이다.

```
SELECT *
FROM test
WHERE id = 1;
```

<center><img src = "https://user-images.githubusercontent.com/78870076/127099675-4d021d68-c4be-4cc3-8f04-493646e18ae1.png"></center>

<center><img src="https://user-images.githubusercontent.com/78870076/127099743-1067f8a0-81ef-43a2-8a1c-f829e2d89dd9.png"></center>


