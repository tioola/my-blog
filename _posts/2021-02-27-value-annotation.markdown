---
layout: post
title:  "@Value annotation"
date:   2021-02-27 12:18:33 +0100
categories: Spring Certification
---

The @Value annotation is used when you want to inject literal values and for that you can use it with

* Simple value.
* Property reference (`$`).
* SpEL String (`#`).


Also note that @Value can be used at:

* Field 
* Constructor Parameter
* Method (_In this case all fields will have injected the same value_)
* Method parameter (_Injection will not be performed if @Value or @Autowired are not present in method level_)
* Annotation type


For that lets take some examples


```java

public class MyBean {  

    //Basic example o literal value injection
    @Value("Diogo")
    private String firstName;

    //This is a basic SpEL example
    @Value("#{'diogo'.toUpperCase()}")    
    private String upperCaseFirstName;

    //You can also use SPeL to do computations like
    @Value("#{1000 * 0.2}")
    private float myCalculation;

    //In the case below we are using the property reference which you get from the property file defined in the 
    //@PropertySource
    @Value("${app.employee.id}")
    private String employeeId;

    //You can also inject a list of values like the example below: (note that you must declare a bean to make list injection works)
    @Value("${app.employees}")
    private List<String> employeeNames;
    

    @Autowired //Remember that to inject with @value in parameter level you need the @autowired 
    public String concatenateName(@Value("firstName") firstName, @Value("lastName") String lastName) {
    return firstName + lastName;
}

}

```


#### List injection

The example above show the example of a List injection. To make it work you have to declare the bean DefaultConversionService


```java

@Configuration
@ComponentScan
@PropertySource("application.properties")
public class ApplicationConfiguration {
    @Bean
    public ConversionService conversionService() {
        return new DefaultConversionService();
    }
}


```