---
layout: post
title:  "Ways of creating your Spring Context"
date:   2021-02-16 12:18:33 +0100
categories: jekyll update
---

There are 3 ways of creating your spring context.

* Non Web Applications
* Web Applications
* Spring Boot

For each way you can configure in different formats

## Non Web Applications

For the non web applications we can use

* AnnotationConfigApplicationContext
* ClassPathXmlApplicationContext
* FileSystemXmlApplicationContext


### AnnotationConfigApplicationContext

This is the simplest way of creating your Non Web Spring Context you can do it through

* @ComponentScan
* @Configuration
* string with package path

#### @ComponentScan

```java
@ComponentScan
public class ConfigurationComponentScan {
}

```

And then execute the context with the code 

```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(
                ConfigurationComponentScan.class
        );
```

Other than that you can directly pass the configuration class with the `beans` that should be present in the `spring context` 

#### @Configuration

```java

@Configuration
public class ConfigurationStatic {
    @Bean
    public BeanOne getBeanOne() {
        return new BeanOne();
    }

    @Bean
    public BeanTwo getBeanTwo() {
        return new BeanTwo();
    }
}
```

And the context creation.

```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(
                ConfigurationComponentScan.class
        );
```

#### String Path

This way you can pass the package of the beans to be scanned. 

```java

AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(
                "com.spring.professional.beans"
        );

```

### ClassPathXmlApplicationContext

This is how you to create using the beans.xml file. In this case you have use the following implementation which takes the `beans.xml` from classpath.

```java
     ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("/beans.xml");
```

### FileSystemXmlApplicationContext

Similar to the example above, but with the difference that you will take the `beans.xml` from the file system. 

```java
    FileSystemXmlApplicationContext context = new FileSystemXmlApplicationContext(beansXmlLocationOnFilesystem);
```


## Web Applications

In case you want your spring-context to be applied to a web application in a web container you have 3 options

* web.xml configuration 
* XmlWebApplicationContext
* AnnotationConfigWebApplicationContext


### web.xml

In the `web.xml` example you have to configure you web.xml like below:

```xml

<web-app xmlns="http://java.sun.com/xml/ns/javaee" version="2.5">
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>WEB-INF/beans.xml</param-value>
    </context-param>

    <servlet>
        <servlet-name>spring-web-app</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value></param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>spring-web-app</servlet-name>
        <url-pattern>/*</url-pattern>
    </servlet-mapping>

</web-app>

```

### XmlWebApplicationContext and AnnotationConfigWebInitializer.

In case you don't want to use the `web.xml` to use the dispatcher you can use  the WebApplicationInitializer or the AnnotationConfigWebInitializer to configure the dispatcher like the examples below


Java information

 ```java

public class XmlWebApplicationInitializer implements WebApplicationInitializer {
    @Override
    public void onStartup(ServletContext container) throws ServletException {
        XmlWebApplicationContext context = new XmlWebApplicationContext();
        context.setConfigLocation("/WEB-INF/beans.xml");

        ServletRegistration.Dynamic dispatcher = container
                .addServlet("dispatcher", new DispatcherServlet(context));

        dispatcher.setLoadOnStartup(1);
        dispatcher.addMapping("/");
    }
}

```

 **OR**
 
```java


public class AnnotationConfigWebApplicationInitializer implements WebApplicationInitializer {
    @Override
    public void onStartup(ServletContext container) throws ServletException {
        AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();

        context.setServletContext(container);
        context.register(ConfigurationComponentScan.class);
        context.refresh();

        ServletRegistration.Dynamic dispatcher = container
                .addServlet("dispatcher", new DispatcherServlet(context));

        dispatcher.setLoadOnStartup(1);
        dispatcher.addMapping("/");
    }
}

```


## Spring Boot

And in case of the spring boot we have two options

### Spring Boot Cli

In the case of `Spring boot cli` you have to annotate one classe with the annotation `@SpringBootApplication` which is an annotation that already has other
default annotations to configure your` spring context`  as you can see below.

```java

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {

```

So to startup a SpringBootApplication with cli the only thing that you have to do is annotate your main class and launch the run like below (note that you need to import the `spring-boot-starter-web`)

```java

@SpringBootApplication
public class SpringBootConsoleApplication implements CommandLineRunner {

    public static void main(String[] args) {
        SpringApplication.run(SpringBootConsoleApplication.class, args);
    }

    @Override
    public void run(String... args) throws Exception {
        System.out.println("hello");
    }
}

```


### Spring Boot Web

In case of the`SpringBoot web` you will initialize an embedded tomcat container therefore you need to import the `spring-boot-starter-web` in your classpath and simply run the code below

```java

@SpringBootApplication
public class SpringBootWebApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringBootWebApplication.class, args);
    }
}

```