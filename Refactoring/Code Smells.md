## Bloaters

Bloaters are code, methods and classes that have increased to such gargantuan proportions that they’re hard to work with. Usually these smells don’t crop up right away, rather they accumulate over time as the program evolves (and especially when nobody makes an effort to eradicate them).

#### [Long Method](https://refactoring.guru/smells/long-method)

A method contains too many lines of code. Generally, any method longer than ten lines should make you start asking questions.

### Reasons for the Problem

Like the Hotel California, something is always being added to a method but nothing is ever taken out. Since it’s easier to write code than to read it, this “smell” remains unnoticed until the method turns into an ugly, oversized beast.

Mentally, it’s often harder to create a new method than to add to an existing one: “But it’s just two lines, there’s no use in creating a whole method just for that...” Which means that another line is added and then yet another, giving birth to a tangle of spaghetti code.

### Treatment

As a rule of thumb, if you feel the need to comment on something inside a method, you should take this code and put it in a new method. Even a single line can and should be split off into a separate method, if it requires explanations. And if the method has a descriptive name, nobody will need to look at the code to see what it does.

- To reduce the length of a method body, use [Extract Method](https://refactoring.guru/extract-method).

- If local variables and parameters interfere with extracting a method, use [Replace Temp with Query](https://refactoring.guru/replace-temp-with-query), [Introduce Parameter Object](https://refactoring.guru/introduce-parameter-object) or [Preserve Whole Object](https://refactoring.guru/preserve-whole-object).

- If none of the previous recipes help, try moving the entire method to a separate object via [Replace Method with Method Object](https://refactoring.guru/replace-method-with-method-object).

- Conditional operators and loops are a good clue that code can be moved to a separate method. For conditionals, use [Decompose Conditional](https://refactoring.guru/decompose-conditional). If loops are in the way, try [Extract Method](https://refactoring.guru/extract-method).

#### [Large Class](https://refactoring.guru/smells/large-class)

A class contains many fields/methods/lines of code.

### Reasons for the Problem

Classes usually start small. But over time, they get bloated as the program grows.

As is the case with long methods as well, programmers usually find it mentally less taxing to place a new feature in an existing class than to create a new class for the feature.

### Treatment

When a class is wearing too many (functional) hats, think about splitting it up:

- [Extract Class](https://refactoring.guru/extract-class) helps if part of the behavior of the large class can be spun off into a separate component.

- [Extract Subclass](https://refactoring.guru/extract-subclass) helps if part of the behavior of the large class can be implemented in different ways or is used in rare cases.

- [Extract Interface](https://refactoring.guru/extract-interface) helps if it’s necessary to have a list of the operations and behaviors that the client can use.

- If a large class is responsible for the graphical interface, you may try to move some of its data and behavior to a separate domain object. In doing so, it may be necessary to store copies of some data in two places and keep the data consistent. [Duplicate Observed Data](https://refactoring.guru/duplicate-observed-data)offers a way to do this.

### [Primitive Obsession](https://refactoring.guru/smells/primitive-obsession)

- Use of primitives instead of small objects for simple tasks (such as currency, ranges, special strings for phone numbers, etc.)
- Use of constants for coding information (such as a constant `USER_ADMIN_ROLE = 1` for referring to users with administrator rights.)
- Use of string constants as field names for use in data arrays.

### Reasons for the Problem

Like most other smells, primitive obsessions are born in moments of weakness. “Just a field for storing some data!” the programmer said. Creating a primitive field is so much easier than making a whole new class, right? And so it was done. Then another field was needed and added in the same way. Lo and behold, the class became huge and unwieldy.

### Treatment

- If you have a large variety of primitive fields, it may be possible to logically group some of them into their own class. Even better, move the behavior associated with this data into the class too. For this task, try [Replace Data Value with Object](https://refactoring.guru/replace-data-value-with-object).

- If the values of primitive fields are used in method parameters, go with [Introduce Parameter Object](https://refactoring.guru/introduce-parameter-object) or [Preserve Whole Object](https://refactoring.guru/preserve-whole-object).

- When complicated data is coded in variables, use [Replace Type Code with Class](https://refactoring.guru/replace-type-code-with-class), [Replace Type Code with Subclasses](https://refactoring.guru/replace-type-code-with-subclasses) or [Replace Type Code with State/Strategy](https://refactoring.guru/replace-type-code-with-state-strategy).

- If there are arrays among the variables, use [Replace Array with Object](https://refactoring.guru/replace-array-with-object).

### Payoff

- Code becomes more flexible thanks to use of objects instead of primitives.

- Better understandability and organization of code. Operations on particular data are in the same place, instead of being scattered. No more guessing about the reason for all these strange constants and why they’re in an array.

- Easier finding of duplicate code.

