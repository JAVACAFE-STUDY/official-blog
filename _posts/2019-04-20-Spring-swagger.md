---
title : 간단히 스프링에 Swagger 적용해보기
date : 2019-04-20 17:00:00
categories:
- spring boot
tags:
- spring boot
- swagger
author : 이정근
---

간단히 스프링부트에 Swagger 적용해보기

원문은 https://dzone.com/articles/simplified-spring-swagger?utm_medium=feed&utm_source=feedpress.me&utm_campaign=Feed:%20dzone%2Fjava 입니다.

이번 예제에서 스프링부트에 Swagger를 적용한 REST 프로젝트를 구현해보고 annotation을 사용하여 유효검증하는 방법을 알아보겠다.

##### 요구사항:
- Java 8.x
- Maven 3.x
<br />
<br />
### 단계
Maven JAR 프로젝트를 생성하고 아래와 같이 pom.xml을 초기화한다.

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.4.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>eg</groupId>
    <artifactId>sample</artifactId>
    <version>0.1.0</version>
    <name>sample</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>com.jayway.jsonpath</groupId>
            <artifactId>json-path</artifactId>
        </dependency>

        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <!--버전을 넣지 않으면 동작하지 않는다. -->
            <version>2.9.2</version> 
        </dependency>

        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <!--버전을 넣지 않으면 동작하지 않는다. -->
            <version>2.9.2</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

Person 이라는 자바빈을 만든다.
``` java
package eg.sample;

import org.hibernate.validator.constraints.CreditCardNumber;

import javax.validation.constraints.*;
import javax.xml.bind.annotation.XmlAccessType;
import javax.xml.bind.annotation.XmlAccessorType;
import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement(name="person")
@XmlAccessorType(XmlAccessType.FIELD)
public class Person {
    private long id;
    private String firstName;

    @NotNull
    @NotBlank
    private String lastName;

    // 정규식으로 이메일의 유효성을 판단한다.
    @Pattern(regexp = ".+@.+\\..+", message = "Please provide a valid email address")
    private String email;

    // @Email annotation 으로 이메일의 유효성을 판단한다.
    @Email()
    private String email1;

    // @Min, @Max annotation 으로 최소, 최대값을 지정한다.
    @Min(18)
    @Max(30)
    private int age;

    // 신용카드번호의 유효성을 판단한다.
    @CreditCardNumber
    private String creditCardNumber;

    public String getCreditCardNumber(){
        return creditCardNumber;
    }

    public void setCreditCardNumber(String creditCardNumber){
        this.creditCardNumber = creditCardNumber;
    }

    public long getId(){
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }
    public String getEmail1() {
        return email1;
    }
    public void setEmail1(String email1) {
        this.email1 = email1;
    }

    @Size(min = 2)
    public String getFirstName(){
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }
    public String getLastName() {
        return lastName;
    }
    public void setLastName(String lastName) {
        this.lastName = lastName;
    }
    public String getEmail() {
        return email;
    }
    public void setEmail(String email) {
        this.email = email;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
```

컨트롤러를 만든다.
``` java
package eg.sample;

import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import javax.validation.Valid;

@RestController
public class PersonController {
    @RequestMapping(path = "/person", method= RequestMethod.POST)
    public Person person(@Valid @RequestBody Person person){
        return person;
    }
}
```

아래는 Swagger configuration 예제이다.
``` java
package eg.sample;

import com.google.common.base.Predicates;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket api(){
        return new Docket(DocumentationType.SWAGGER_2).select()
                .apis(Predicates.not(RequestHandlerSelectors.
                        basePackage("org.springframework.boot")))
                .paths(PathSelectors.any()).build();
    }
}

```

그리고 스프링부트 어플리케이션 클래스이다.
``` java
package eg.sample;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SampleApplication {

    public static void main(String[] args) {
        SpringApplication.run(SampleApplication.class, args);
    }

}

```

intelliJ 에서의 예제 프로젝트의 구조는 아래와 같다.  
![spring-swagger01](http://tech.javacafe.io/img/blog/20190420/spring-swagger01.png)

다음으로, `mvn clean package` 명령을 명령창이나 터미널에서 입력하여 jar파일을 생성한다.

그런 다음, `java -jar target/sample-0.1.0.jar`명령으로 jar을 실행한다.

이젠, Swagger UI를 확인해보자. http://localhost:8080/swagger-ui.html:

![spring-swagger02](http://tech.javacafe.io/img/blog/20190420/spring-swagger02.png)

`Try it out` 버튼을 클릭한 후 `Execute` 버튼을 클릭한다. 
그러면 유효성 에러가 회신된다. 

![spring-swagger03](http://tech.javacafe.io/img/blog/20190420/spring-swagger03.png)



좀 더 유효성 제약조건에 대한 정보를 swagger UI 에서 제공하고 싶다면 `spring-swagger-simplified` 라이브러리를 사용하면된다.

`spring-swagger-simplified` 종속성을 pom.xml 에 추가한다.
``` xml
<dependency>
    <groupId>org.bitbucket.tek-nik</groupId>
    <version>1.0.2</version>
    <artifactId>spring-swagger-simplified</artifactId>
</dependency>
```
그리고 메인어플리케이션 클래스를 수정한다.
``` java
package eg.sample;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
// add
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
// add
@ComponentScan(basePackages = {"org.bitbucket.tek.nik.simplifiedswagger", "eg.sample"})
public class SampleApplication {

    public static void main(String[] args) {
        SpringApplication.run(SampleApplication.class, args);
    }

}
```
만일 패키지이름을 `eg.sample` 이 아닌 다른 패키지명을 사용했다면 그에 맞게 `@componentScan` 을 수정해주면 된다.

아래를 보면 달라진점을 발견할 수 있다.  
![spring-swagger04](http://tech.javacafe.io/img/blog/20190420/spring-swagger04.png)

끝~!!!