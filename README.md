
# Spring Security - Getting Started

+ Secure Spring MMVC Web Apps
+ Develop login pages(default and custom)
+ Define users and roles with simple authentication
+ Protect URLs based on role
+ Use JSP tags to hide/show content based on role
+ Store users, passwords and roles in DB (plain-text -> encrypted)


### Spring Security - Overview
**Spring Security Model**

+ Spring Security defines a framework for security
+ Implemented using Servlet filters in the background
+ Two mehods of security a Web app: declarative and programmatic

**Spring Security with Servlet Filters**

+ Servlet Filters are used to pre-process / post-process web requests
+ Servlet Filters can route web requests based on security logic
+ Spring provides a bulk of security functionality with servlet filters

**Spring Security Overview**

<img src="https://user-images.githubusercontent.com/80107049/190439619-74647311-ccbe-4913-8ff7-8c8c6e7920da.png" width="500" />



**Flowchart**

<img src="https://user-images.githubusercontent.com/80107049/190439685-13fa8475-235c-437b-b3d0-8cccdd4b17af.png" width="500" />

**Security Concepts**

+ Authentication
    + Check user id and password with credentials stored in app / db

+ Authorization
    + Check to see if user has an authorized role

**Declarative Security**

+ Define application's security constraints in configuration
    + All java config(@Configuration, no xml)
    + or Spring XML config

+ Provides separation of concerns between application code and security

**Programmatic Security**

+ Spring Security provides an API for custom application coding
+ Provides greater customization for specific app requirements

**Different Login Methods**

+ HTTP Basic Authentication

+ Default login form
    + Spring Security provides a default login form

+ Custom login form
    + your own look-and-feel, HTML + CSS

**Authentication and Authorization**

+ In-memory
+ JDBC
+ LDAP
+ Custom / Pluggable
+ others ...

###  Spring Security - All Java Configuration

**Java Configuration**
+ Instead of configuration Spring MVC app using XML
    + web.xml
    + spring-mvc-demo-servlet.xml

+ Configuration the Spring MVC app with Java code

**Development Process**

1. Add Maven dependencies for Spring MVC Web App
2. Create Spring App Configuration(@Configuration)
3. Create Spring Dispatcher Servlet Initializer
4. Develop our Spring controller
5. Develop our JSP view page

_Step 1:Add Maven dependencies for Spring MVC Web App_

<img src="https://user-images.githubusercontent.com/80107049/190439751-d3e6742b-dc47-4d49-9769-4290622bc361.png" width="500" />


File:pom.xml
```XML
<!-- Spring MVC support -->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-webmvc</artifactId>
  <version>...</version>
</dependency>

<!-- Servlet JSP and JSTL support-->
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>...</version>
</dependency>

...

```` 
+ Add Servlet, JSP and JSTL support


**Customize Maven Build**

+ Need to customize Maven build, Since we are not using web.xml
    + Must add maven WAR plug-in

```XML
<build>
  
  <pluginManagment>
    <plugins>
      
      <plugin>
        <!-- Add Maven coordnate (GAV) for: maven-war-plugin -->
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-war-plugin</artifactId>
        <version>3.2.0</version>
      </plugin>
      
    </plugins>
  </pluginManagment>
  
</build>
```

**XML config to Java config**

<img src="https://user-images.githubusercontent.com/80107049/190439864-67391cc1-198b-482b-9290-42ecdba5d902.png" width="500" />

**Enabling the MVC Java Config**

```JAVA
      @EnableWebMvc
```
+ Provides similar support to `<mvc:annotation.driven />` in XML
+ Adds conversion, formatting and validation support
+ Processing of @Controller classes and @RequestMapping etc ... methods


_Step 2:Create Spring App Configuration_

File:DemoAppConfig.java
```JAVA
@Configuration
@EnableWebMvc
@ComponentScan(basePackage="com.tilmeez.springsecurity.demo")
public class DemoAppConfig {
  
  // define a bean for viewResolver
  
  @Bean
  public ViewResolver viewResolver() {
    
    InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
    
    viewResolver.setPrefix("/WEB-INF/view/");
    viewResolver.setSuffix(".jsp");
    
    return viewResolver;
  }
}
```

**Web App Initializer**

+ Spring MVC provides support for web app initialization
+ Makes sure code is automatically detected
+ Code is used to initialize the servlet container

```Java
    AbstractAnnotationConfigDispatcherServletInitializer
```

+ TO DO list
    + Extend this abstract base class
    + Override required methods
    + Specify servlet mapping and location of app cinfig

_Step 3:Create Spring Dispatcher Servlet Initializer_

File:MySpringMvcDispatcherServletInitializer.java
```JAVA
import org.springframwork.web.servlet.support.AbstractAnnoatationConfigDispatcherServletInitializer;

public class MySpringMvcDispatcherServletInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
  
  @Override
  protected Class<?>[] getRootonfigClasses() {
    return null;
  }
  
  @Override
  protected Class<?>[] getServletConfigClasses() {
    return new Class[] { DemoAppConfig.class };
  }
  
  @Override
  protected String[] getServletMappings() {
    return new String[] { "/" };
  }
  
}
```
+ In method `getServletConfigClasses()` `getServletConfigClasses()` is config class form Step 2

+ `getServletConfigClasses()` method is same as web.xml
```XML
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/spring-mvc-crud-demo-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
```
+ `getServletMappings()` is

```XML
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```

_Step 4:Develop our Spring Controller_

File:DemoController.java
```JAVA
@Controller
public class DemoController {
  
  @GetMaping("/")
  public String showHome() {
    
    return "home";
  }
  
}
```
+ `return "home"` is view name as in `/WEB_INF/view/home.jsp`

_Step 5:Develop JSP view page_

File:/WEB-INF/view/home.jsp
```JSP
<html>
  
  <body>
    
    Welcome to the tilmeez company home page!
    
  </body>
  
</html>
```