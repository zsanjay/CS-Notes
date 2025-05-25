
**What’s type code?** Type code occurs when, instead of a separate data type, you have a set of numbers or strings that form a list of allowable values for some entity. Often these specific numbers and strings are given understandable names via constants, which is the reason for why such type code is encountered so much.

### Problem

A class has a field that contains type code. The values of this type aren’t used in operator conditions and don’t affect the behavior of the program.

```java
class Person {
	private int O;
	private int A;
	private int B;
	private int AB;
	private int bloodgroup;
}
```

### Solution

Create a new class and use its objects instead of the type code values.

```java
class Person {
	private BloodGroup bloodgroup;
}
```

```java
class BloodGroup {
	private BloodGroup O;
	private BloodGroup A;
	private BloodGroup B;
	private BloodGroup AB;
}
```

### Why Refactor

One of the most common reasons for type code is working with databases, when a database has fields in which some complex concept is coded with a number or string.

For example, you have the class `User` with the field `user_role`, which contains information about the access privileges of each user, whether administrator, editor, or ordinary user. So in this case, this information is coded in the field as `A`, `E`, and `U` respectively.

What are the shortcomings of this approach? The field setters often don’t check which value is sent, which can cause big problems when someone sends unintended or wrong values to these fields.

In addition, type verification is impossible for these fields. It’s possible to send any number or string to them, which won’t be type checked by your IDE and even allow your program to run (and crash later).

### Benefits

- We want to turn sets of primitive values—which is what coded types are—into full-fledged classes with all the benefits that object-oriented programming has to offer.

- By replacing type code with classes, we allow type hinting for values passed to methods and fields at the level of the programming language.

    For example, while the compiler previously didn’t see difference between your numeric constant and some arbitrary number when a value is passed to a method, now when data that doesn’t fit the indicated type class is passed, you’re warned of the error inside your IDE.

- Thus we make it possible to move code to the classes of the type. If you needed to perform complex manipulations with type values throughout the whole program, now this code can “live” inside one or multiple type classes.