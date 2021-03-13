---
layout: post
title:  "@Transaction enablement"
date:   2021-03-06 12:18:33 +0100
categories: Spring Certification
---

When working with transaction in spring you have to understand some concepts required to make it work properly.

## Enabling Transaction Management

If you are working outside of the `spring boot`, directly with the spring context you have to enable the transaction management 
using the `@EnableTransactionManagement` annotation.

So having a configuration class like below would enable it:

```java
@Configuration
@EnableTransactionManagement
public class DataSourceConfiguration {}

```

Not only the enablement is necessary, you also need to provide a bean for the `PlatformTransactionManager`.

This bean is responsible for controlling your transaction and has multiple implementations, some methods present in these class are:

* `getTransaction`
* `commit`
* `rollback`

`PlatformTransacationManager` implementations are:

* `DataSourceTransactionManager`
* `JtaTransactionManager`
* `JpaTransactionManager`

One simple example of DataSourceConfiguration is:


```java
@Configuration
@EnableTransactionManagement
public class DataSourceConfiguration {

    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder()
                .generateUniqueName(true)
                .setScriptEncoding("UTF-8")
                .addScript("db-scripts.sql")
                .build();
    }

    @Bean
    @Autowired
    public PlatformTransactionManager platformTransactionManager(DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }
}

```

With this configuration you are enabled to use the `@Transactional` annotation and for this annotation you can set.

* Transaction Manager
* Propagation Type
* Isolation Level
* Timout for Transaction
* Read only flag
* Define exceptions to rollback
* Define exceptions to not rollback
