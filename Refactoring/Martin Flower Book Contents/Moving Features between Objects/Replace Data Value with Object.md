### Problem

A class (or group of classes) contains a data field. The field has its own behavior and associated data.

```java
class Order {
	private String customer;
}
```

### Solution

Create a new class, place the old field and its behavior in the class, and store the object of the class in the original class.

```java
class Order {
	private Customer customer;
}
```

```java
class Customer {
	private String name;
}
```


### Why Refactor

This refactoring is basically a special case of [Extract Class](https://refactoring.guru/extract-class). What makes it different is the cause of the refactoring.

In [Extract Class](https://refactoring.guru/extract-class), we have a single class that’s responsible for different things and we want to split up its responsibilities.

With replacement of a data value with an object, we have a primitive field (number, string, etc.) that’s no longer so simple due to growth of the program and now has associated data and behaviors. On the one hand, there’s nothing scary about these fields in and of themselves. However, this fields-and-behaviors family can be present in several classes simultaneously, creating duplicate code.

Therefore, for all this we create a new class and move both the field and the related data and behaviors to it.

### Benefits

- Improves relatedness inside classes. Data and the relevant behaviors are inside a single class.