---
layout: post
title:  "JDBC Template"
date:   2021-03-06 12:18:33 +0100
categories: Spring Certification
---

The JDBC template is provided as a way to simplify your JDBC queries, so instead of having to do all that work of:

* opening connection
* executing a query
* extracting data from result set
* mapping or displaying
* cleaning up

Like the example below:

```java

        //STEP 1: Open a connection
      System.out.println("Connecting to database...");
      conn = DriverManager.getConnection(DB_URL,USER,PASS);

      //STEP 2: Execute a query
      System.out.println("Creating statement...");
      stmt = conn.createStatement();
      String sql;
      sql = "SELECT id, first, last, age FROM Employees";
      ResultSet rs = stmt.executeQuery(sql);

      //STEP 3: Extract data from result set
      while(rs.next()){
         //Retrieve by column name
         int id  = rs.getInt("id");
         int age = rs.getInt("age");
         String first = rs.getString("first");
         String last = rs.getString("last");

         //Display values
         System.out.print("ID: " + id);
         System.out.print(", Age: " + age);
         System.out.print(", First: " + first);
         System.out.println(", Last: " + last);
      }
      //STEP 4: Clean-up environment
      rs.close();
      stmt.close();
      conn.close();

```

You can do it way simpler with the JdbcTemplate. Check how better it is to do that.

```java

    public List<Employee> findEmployees() {
        return jdbcTemplate.query(
                "select employee_id, first_name, last_name, email salary from employee",
                this::map
        );
    }


  private Employee map(ResultSet resultSet, int index) {
        return new Employee(
                resultSet.getInt("employee_id"),
                resultSet.getString("first_name"),
                resultSet.getString("last_name"),
                resultSet.getString("email"),
                resultSet.getFloat("salary")
        );
    }

```

Way cleaning and easier.


## JDBC Template methods.


Jdbc support SQL in the following method each one with its respective purpose.

* query
* queryForList
* queryForObject
* queryForMap
* queryForRowSet
* execute
* update
* batchUpdate 