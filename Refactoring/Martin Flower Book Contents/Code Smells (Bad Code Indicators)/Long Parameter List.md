
  
This is another code smell at the same abstraction level as [Long Method](https://luzkan.github.io/smells/long-method) which usually occurs when three, four, or more parameters are given as input for a single method. Basically, the longer the parameter list, the harder it is to understand.

### Causation 

In an attempt to generalize a routine with multiple variations, a developer could have passed too many parameters at one point. Another causation could be due to ignorance of the object relationship between other objects, and thus, instead, calling in all the entities via parameters [[1](https://luzkan.github.io/smells/long-parameter-list#sources)].

### Problems: 

#### **Hard to Use** 

Usage of a method with many parameters requires more knowledge to use it.

#### **Increased Complexity**

The input value is highly inconsistent, which creates too much variety in what might happen throughout the execution.

#### [**Single Responsibility Principle Violation**](https://luzkan.github.io/smells/long-parameter-list#single-responsibility-principle-violation)

When there are too many parameters, most likely, the method tries to do too many things or has too many reasons to change.

### Example 

#### Smelly 

```python
def foo(author: str, commit_id: str, files: List[str], sha_id: str, time: str):
    ...

author, commit_id, files, sha_id, time = get_last_commit()
foo(author, commit_id, files, sha_id, time)
```

#### Solution

```python
@dataclass(frozen=True)
class Commit:
    author: str
    commit_id: str
    files: List[str]
    sha_id: str
    time: str

    def foo(self):
        ...

commit = Commit(**get_last_commit())
commit.foo()
```

### Refactoring:

- Replace Parameter with Query
- Preserve the Whole Object
- Introduce Parameter Object
- Remove Flag Argument
- Combine Methods into Class

