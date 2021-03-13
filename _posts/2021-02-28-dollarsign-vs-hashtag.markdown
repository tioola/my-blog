---
layout: post
title:  "$ vs #"
date:   2021-02-28 12:18:33 +0100
categories: Spring Certification
---

The main difference between `$` and # is that:

### `$`

`$` is a compact way to access the Environment properties.
But on the other hand you can't execute a SpEL expression like you can do with `#`.

One example is 

```java 

    @Value("${app.item.default.code}")
    private String defaultItemCode;

```

### `#`

On the other hand # is used to evaluate expressions but you can also use it to pull information from the Environment altought it
is not as compact as the $.

You can take as an example

```java

@Value("#{environment['app.item.default.code']}")
private String defaultItemCode;


```

### Conclusion

Use `$` when you just need to access an Environment property and use # when you also need to execute a SpEL expression.