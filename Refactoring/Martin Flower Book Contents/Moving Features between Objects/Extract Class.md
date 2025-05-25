#### Create a new class to separate concerns.
### Problem

When one class does the work of two, awkwardness results.

![[20250321161731.png]]

### Solution

Instead, create a new class and place the fields and methods responsible for the relevant functionality in it.

![[20250321161804.png]]

### Why Refactor

Classes always start out clear and easy to understand. They do their job and mind their own business as it were, without butting into the work of other classes. But as the program expands, a method is added and then a field... and eventually, some classes are performing more responsibilities than ever envisioned.

### Benefits

- This refactoring method will help maintain adherence to the _Single Responsibility Principle_. The code of your classes will be more obvious and understandable.

- Single-responsibility classes are more reliable and tolerant of changes. For example, say that you have a class responsible for ten different things. When you change this class to make it better for one thing, you risk breaking it for the nine others.

### Drawbacks

- If you “overdo it” with this refactoring technique, you will have to resort to [Inline Class](https://refactoring.guru/inline-class).