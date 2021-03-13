---
layout: post
title:  "Transaction Isolation Level"
date:   2021-03-06 12:18:33 +0100
categories: Spring Certification
---

We have 3 types of situations when we talk about transaction isolation level.

* Phantom Read
* Non-repeatable read
* Dirty Read

## Phantom Read

The phantom read happens when you don't have an isolation level which provides `Range Lock`.

One example is when you have 2 transactions, and you are reading the data with one and the other inserted new values.

Example of Phantom read:

**Transaction A** - Read a range of data

`SELECT * FROM Items it WHERE it.id between 10 and 50`

**Transaction B** - Write and commit new Item with id within the range.

`INSERT INTO Items .....`


**Transaction A** Read a range of data again:

`SELECT * FROM Items it WHERE it.id between 10 and 50`

Depending on the `isolation level` the data inserted in transaction be WILL be present int he second select even thought transaction A is not committed.

To avoid that you need an `isolation level` which provides a `range lock`


## Range lock

Non-repeatable read happens when you don't have an isolation level which provides `read-write locks`

One example is when you have 2 transactions, and you are reading the data with one transaction, and the other updates a value.

 
 
**Transaction A** - Read a range of data

`SELECT * FROM Items it WHERE it.id = 1`

**Transaction B** - Write and commit new Item with id within the range.

`UPDATE ITEM SET ..... WHERE it.id =1`


**Transaction A** Read a range of data again:

`SELECT * FROM Items it WHERE it.id = 1`


To avoid that you need an `isolation level` which provides a `read-write lock`

## Dirty read

The dirty read is like similar to the above but the item of the second transaction **does not need to be committed.**

**Transaction A** - Read a range of data

`SELECT * FROM Items it WHERE it.id = 1`

**Transaction B** - Write and **Not committed** new Item with id within the range.

`UPDATE ITEM SET ..... WHERE it.id =1`


**Transaction A** Read a range of data again:

`SELECT * FROM Items it WHERE it.id = 1`

to avoid that you need an `isolation level` which prevents at least `read uncommitted`




## Isolation Levels

The Isolation levels which can be used as strategy to avoid the situations above can be from highest to lowest level.

### Serializable

This is the highest isolation level

    *Read-Write Locks held until the end of transaction
    *Range locks held until end of transaction
    
### Repeatable Read

    * Read-Write Locks held until end of transaction
    
### Read Committed

    * Read Locks held until **end of select statement**
    * Write Locks held until end of transaction
    
### Read Uncommitted

    * Lowest Isolation level
    * Possible to see changes from other transaction uncommitted.


