---
layout: post
title:  "Understanding @Bean annotation"
date:   2021-02-19 12:18:33 +0100
categories: Spring Certification
---

The `@Bean` annotation is used to configura beans that normally not spring beans by default (classes already annotated with `@Component,` `@Service,` `@Repository etc`)


Take the example below and lets comment each bean creation;

```java

@Configuration
public class ApplicationConfig {

    // This is an example of property name injection, we have more than one bean of the same type but based on the name of the property spring knows
    // which one to inject.
    @Autowired
    private SpringBean3 springBean3C;

    //This is a normal bean declaration but you can see that we are injecting 2 beans in the method and also defining the initMethod attribute, which will basically
    // Tell spring which method to call
    @Bean(initMethod = "init")
    public SpringBean1 springBean1(SpringBean2 springBean2, SpringBean3 springBean3rd) {
        return new SpringBean1(springBean2, springBean3rd);
    }
    
    //Same as above but now declaring the destroy method that will be called;
    @Bean(destroyMethod = "destroy")
    public SpringBean2 springBean2() {
        return new SpringBean2();
    }


    //Here we are creting the bean and naming it as springBean3rd instead of using the default name which would be springBean3rd.
    // If we try to inject with the name springBean3A it won't work because you are overriding the default by springBean3rd
    @Bean(name = "springBean3rd")
    @Autowired
    public SpringBean3 springBean3A(MessageDigest messageDigest) {
        return new SpringBean3A(messageDigest);
    }


    //Here we are giving more than one name fo one bean (aliases), we can inject them using any of it
    @Bean(name = {"springBean4th", "springBeanNo4", "springBeanNoFour"})
    public SpringBean3 springBean3B() {
        return new SpringBean3B();
    }
    
    //Here it is a default declaration with the default name of the bean which will be springBean3c
    @Bean
    public SpringBean3 springBean3C() {
        return new SpringBean3C();
    }

    //Creating a bean from a tpp library.
    @Bean
    public MessageDigest messageDigest() {
        return DigestUtils.getSha384Digest();
    }


}


```