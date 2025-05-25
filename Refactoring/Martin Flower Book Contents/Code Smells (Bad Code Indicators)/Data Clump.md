#### Groups of variables that always appear together.

Data Clumps refer to a situation in which a few variables are passed around many times in the codebase instead of being packed into a separate object. Think of it as having to hold different groceries in a grocery store by hand instead of putting them into a basket or at least a handy cardboard box - this is just not convenient. Any set of data items that are permanently or almost always used together, but are not organized jointly, should be packed into a class. An example could be the `RGB` values held separately rather than in an `RGB` object.
### Causation

Developers often believe that a pair of variables is unworthy of creating a separate instance for them that could aggregate them under a common abstraction [[1](https://luzkan.github.io/smells/data-clump#sources)].

### Problems

#### **Hidden Abstraction**

Variables grouped into objects of their own increase the readability of the code, thus making the concept clearer.

#### **More Complex APIs** 

Components Interfaces complexity increases with the number of accepted data.

### Example

#### Smelly

```python
def colorize(red: int, green: int, blue: int):
```

#### Solution

```python
@dataclass(frozen=True)
class RGB:
    red: int
    green: int
    blue: int


def colorize(rgb: RGB):
```

### Refactoring:

- Extract Class (and additionally, maybe there is the possibility for Move Method)
- Introduce Parameter Object.


