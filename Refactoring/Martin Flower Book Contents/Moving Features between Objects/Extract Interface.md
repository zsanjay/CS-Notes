
### Problem

Multiple clients are using the same part of a class interface. Another case: part of the interface in two classes is the same.

```java
interface Employee {
	getRate(),
	hasSpecialSkill(),
	getName(),
	getDepartment()
}
```

### Solution

Move this identical portion to its own interface.

```java
interface Billable {
	getRate(),
	hasSpecialSkill()
}
```

```java
interface Employee extends Billable {
	getRate(),
	hasSpecialSkill(),
	getName(),
	getDepartment()
}
```


### Why Refactor

1. Interfaces are very apropos when classes play special roles in different situations. Use [Extract Interface](https://refactoring.guru/extract-interface) to explicitly indicate which role.

2. Another convenient case arises when you need to describe the operations that a class performs on its server. If it’s planned to eventually allow use of servers of multiple types, all servers must implement the interface.

### Good to Know

There’s a certain resemblance between [Extract Superclass](https://refactoring.guru/extract-superclass) and [Extract Interface](https://refactoring.guru/extract-interface).

Extracting an interface allows isolating only common interfaces, not common code. In other words, if classes contain [Duplicate Code](https://refactoring.guru/smells/duplicate-code), extracting the interface won’t help you to deduplicate.


