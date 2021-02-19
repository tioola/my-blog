---
layout: post
title:  "Spring BeanFactoryPostProcessor and BeanPostProcessor"
date:   2021-01-09 12:18:33 +0100
categories: jekyll update
---

These two classes can be used to get into the spring bean definition registration and initialization.

In case we need to intercept the **beans definitions** we can use the `BeanFactoryPostProcessor` which is executed during the spring bean definitions (before the initialization).

On the other hand in case we need to intercept the **beans initialization** we can use the `BeanPostProcessor` which is executed _before_ and _after_ every bean initialization.


## BeanFactoryPostProcessor

We can take the example below for the `BeanFacotryPostProcessor`.

```java


public class CustomBeanFactoryPostProcessor implements BeanFactoryPostProcessor {

    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        System.out.println(getClass().getSimpleName() + "::postProcessorBeanFactory Listing Beans Start" );
        Arrays.stream(beanFactory.getBeanDefinitionNames()).forEach(System.out::println);
        System.out.println(getClass().getSimpleName() + "::postProcessBeanFactory Listing Beans End\n");
    }

}

```

And you can register it in your configuration class like below:

```java

@ComponentScan(basePackageClasses = ApplicationConfigWithBeanFactoryPostProcessor.class)
public class ApplicationConfigWithBeanFactoryPostProcessor {


    /**
     * bean declaration is done using a static method.
        The reason for this is so that the bean declaration is picked
     * up when the context is created, earlier than the configuration class annotated
     * with @Configuration, and so the property values are added to the Spring
     * environment and become available for injection in the said configuration class,
     * before this class is initialized.
     */
    @Bean
    public static BeanFactoryPostProcessor getPostProcess(){
        return new CustomBeanFactoryPostProcessor();
    }


}

```

When we create the spring-context using the test code below


```java

    @Test
    public void beanFactoryPostProcessorLogs() {

        //Execution this code you will see in the logs the definitions of each bean
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(ApplicationConfigWithBeanFactoryPostProcessor.class);


    }


```

We will get the following logs 

```text

CustomBeanFactoryPostProcessor::postProcessorBeanFactory Listing Beans Start
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.event.internalEventListenerProcessor
org.springframework.context.event.internalEventListenerFactory
applicationConfigWithBeanFactoryPostProcessor
computer
hardDisk
memory
processor
getPostProcess
CustomBeanFactoryPostProcessor::postProcessBeanFactory Listing Beans End

```

These logs if you look at the code reflect all the beans that we have in the application. In this case other than the spring beans we have: 

* computer
* hardDisk
* memory
* processor
* getPostProcess

as beans in our application


## BeanPostProcessor

`BeanPostProcessor` has the similar concept but it will be executed before each bean initialization. We can take as an example our class that implements it below:

```java


public class CustomBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println(String.format("%s::postProcessBeforeInitialization %s %s", getClass().getSimpleName(), bean.getClass().getSimpleName(), beanName));
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println(String.format("%s::postProcessAfterInitialization %s %s", getClass().getSimpleName(), bean.getClass().getSimpleName(), beanName));
        return bean;
    }
}

```


Similary registered by the configuration class below:


```java

@ComponentScan(basePackageClasses = ApplicationConfigWithBeanPostProcessor.class)
public class ApplicationConfigWithBeanPostProcessor {

    @Bean
    public static BeanPostProcessor getPostProcessor(){
        return new CustomBeanPostProcessor();
    }


}

```

After executing the context with the application config class above, you will have the following logs:

```text

postProcessorBeforeInitialization ApplicationConfigWithBeanPostProcessor
postProcessAfterInitialization ApplicationConfigWithBeanPostProcessor
postProcessorBeforeInitialization Processor
postProcessAfterInitialization Processor
postProcessorBeforeInitialization Memory
postProcessAfterInitialization Memory
postProcessorBeforeInitialization HardDisk
postProcessAfterInitialization HardDisk
postProcessorBeforeInitialization Computer
postProcessAfterInitialization Computer

```

With the logs above you can see that before the initialization and after the initialization of the bean the method is executed.