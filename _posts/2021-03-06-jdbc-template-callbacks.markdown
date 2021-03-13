---
layout: post
title:  "JDBC Template Callbacks"
date:   2021-03-06 12:18:33 +0100
categories: Spring Certification
---

For the certification it is important to know the callbacks that we have in JdbcTemplate

## Query Callbacks

* RowMapper
* RowCallBackHandler
* ResultSetExtract


### RowMapper

RowMapper Should extract values from current row in a stateless way


```java

    public List<Employee> findAll() {
        return jdbcTemplate.query(
                "select employee_id, first_name, last_name, email, salary from employee",
                new RowMapper<Employee>() {
                    @Override
                    public Employee mapRow(ResultSet resultSet, int rowNum) throws SQLException {
                        return new Employee(
                                resultSet.getInt("employee_id"),
                                resultSet.getString("first_name"),
                                resultSet.getString("last_name"),
                                resultSet.getString("email"),
                                resultSet.getFloat("salary")
                        );
                    }
                }
        );
    }


```

### RowCallbackHandler

Should map current row, usually in a stateful way so it can accumulate data in  some object.


```java

    private static class AvgSalaryRowCallbackHandler implements RowCallbackHandler {
        private float wageSum = 0;
        private int count = 0;

        @Override
        public void processRow(ResultSet rs) throws SQLException {
            wageSum += rs.getFloat("salary");
            ++count;
        }

        public float getAverageSalary() {
            return wageSum / (float) count;
        }
    }

    public float findAverageSalaryRowByRow() {
        AvgSalaryRowCallbackHandler avgSalaryRowCallbackHandler = new AvgSalaryRowCallbackHandler();

        jdbcTemplate.query(
                "select salary from employee",
                averageSalaryRowCallbackHandler
        );

        return averageSalaryRowCallbackHandler.getAverageSalary();
    }

```

### ResultSetExtractor

In this one you have to call the resultSet.next() to iterate through it unlike the example above.

```java

private static class AvgSalaryResultSetExtractor implements ResultSetExtractor<Float> {
        @Override
        public Float extractData(ResultSet rs) throws SQLException, DataAccessException {
            float wageSum = 0;
            int count = 0;

            while (rs.next()) {
                wageSum += rs.getFloat("salary");
                ++count;
            }

            return wageSum / (float) count;
        }
    }


```

## Other call backs.

For the certification you only need to remember that the CallBacks below exists. but not the details of it.

* PreparedStatementCreator - creates a `PrepareStatement` based on the connection provided by jdbc template.
* PreparedStatementSetter - set values on the `PreparedStatement` parementers
* CallableStatementCreator - creates a `CallableStatement`
* PreparedStatementCallback - this is used **internally** 
* CallableStatementCallback - this is used **internally** 
