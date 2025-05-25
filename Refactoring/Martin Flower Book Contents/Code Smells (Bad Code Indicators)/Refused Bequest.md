
Whenever a subclass inherits from a parent but only uses a subset of the implemented parent methods, that is called _Refused Bequest_. This behavior can happen both implicitly and explicitly:

- Implicitly, when the inherited routine doesn not work
- Explicitly, if an error is thrown instead of supporting the method

Whenever a child class is created, it should fully support all the data and methods that it inherits [1]. Fowler says that this smell is not that strong, though, and admits that he sometimes reuses only a bit of behavior, but it can cause confusion and problems [2].

However, there is one crucial thing. Both Fowler and Mäntylä are pointing out a strong case of this Code Smell whenever a subclass is reusing behavior but does not want to support the superclass interface [3] [1] [4].

### Causation

This could happen due to bad development decisions when part of the needed functionality is already implemented in another class, but in general, the parent class is about something different from whatever the developer would like to implement in the new class. This inconsistency most likely indicates that the inheritance does not make sense, and the subclass is not an example of its parent. [5]

### Problems

#### **Liskov Substitution Principle Violation**

The _Liskov Substitution Principle_ describes that the relationship of subtypes and supertypes should ensure that any property proved about supertype object also holds for its subtype object.

#### **Hard to Test**

Each subclass might have different capabilities up and down the class hierarchies.

### Example

#### Smelly

```python
class Minion(ABC):
    @abstractmethod
    def attack(self):
        """ """

    @abstractmethod
    def move(self):
        """ """

class Tower(Minion):
    def attack(self):
        ...

    def move(self):
        raise NotImplementedException
```

### Refactoring:

- Extract Subclass
- Push Down Field
- Push Down Method
- Replace Inheritance with Delegation
- Replace Superclass/Subclass with Delegate