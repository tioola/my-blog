---
layout: post
title:  "Spring Data Source Creation"
date:   2021-03-06 12:18:33 +0100
categories: Spring Certification
---

The `javax.sql.DataSource` is the java default interface which represents a SQL DataSource.

`javax.sql.DataSource` can be created in different ways depending on the environment, because of that we can separate the creation in
3 categories:

* Standalone
* Spring Boot
* Application Server


## Standalone

In the standalone the DataSource it will be configured in the @Configuration class like the simple example below:

### DataSource creation

```java


@Configuration
public class DataSourceConfiguration {

    @Bean
    public DataSource dataSource() {
        BasicDataSource basicDataSource = new BasicDataSource();
        basicDataSource.setDriverClassName("org.hsqldb.jdbcDriver");
        basicDataSource.setUrl("jdbc:hsqldb:mem:localhost");
        return basicDataSource;
    }
}

```

### DataSource Initialization

We also have the DataSourceInitializer which can be used to Initialize your database, creating data or executing your scripts. 
This is mostly common in test and therefore should be used to prepare your database.

Check the example below:

```java

@Configuration
public class DataSourceTestDataConfiguration {

    @Value("classpath:/db-schema.sql")
    private Resource schemaScript;
    @Value("classpath:/db-test-data.sql")
    private Resource dataScript;

    @Bean
    public DataSourceInitializer dataSourceInitializer(final DataSource dataSource) {
        final DataSourceInitializer dataSourceInitializer = new DataSourceInitializer();
        dataSourceInitializer.setDataSource(dataSource);
        dataSourceInitializer.setDatabasePopulator(databasePopulator());
        return dataSourceInitializer;
    }

    private DatabasePopulator databasePopulator() {
        final ResourceDatabasePopulator databasePopulator = new ResourceDatabasePopulator();
        databasePopulator.addScript(schemaScript);
        databasePopulator.addScript(dataScript);
        return databasePopulator;
    }
}
``` 


## SpringBoot


In Spring boot is even easier since we only need to configure in the `resources/application.properties` the datasource information like the example below:

### DataSource creation

```properties
spring.datasource.url=jdbc:hsqldb:mem:localhost
spring.datasource.driver-class-name=org.hsqldb.jdbcDriver
```

You should also remember to import the dependency in your pom.xml or build.gradle so it can work correctly like below:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency>
```

### DataSource Initialization

Compared to the standalone with DataSource Initialization the only thing that you need to do is to put your `.sql` files in the `resources` folder and spring will execute it.

`resources/db-schema.sql`
`resource/data.sql`


## Application Server

In the application server you can you the JNDI lookup to get your DataSource like the example below

```java

@Configuration
public class DataSourceConfiguration {

    @Bean
    public DataSource dataSource() {
        return new JndiDataSourceLookup()
                .getDataSource("jdbc/Database");
    }
}


```
