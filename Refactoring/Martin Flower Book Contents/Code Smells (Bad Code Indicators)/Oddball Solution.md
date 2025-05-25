### The same code appears in multiple places.

If a similar problem is solved differently in different parts of the project, it is an Oddball Solution. This code smell could also have been classified under [Duplicated Code](https://luzkan.github.io/smells/duplicated-code), although it is not exactly a one-to-one copy-paste - it is more subtle [[1](https://luzkan.github.io/smells/oddball-solution#sources)].

### Causation

This smell often occurs when there is some recognized method of calling a set of classes whose interfaces are not uniform.

### Problems

#### **Increased Complexity**

There should be one way to deal with the problem throughout the project. There is no reason to keep two ways of dealing with one problem - just use the better one.

#### **Duplication**

### Examples

#### Smelly

```python
class Instrument:
    ...

class USB2(Instrument):
    def __init__(self, ip, port):
        connection = socket.new_connection(f"{ip}:{port}")
        ...

    def ask(self, command):
        ...
        self.connection.query(command)


class USB3(Instrument):
    def __init__(self, address):
        connection = socket.new_connection(address)
        ...

    def read(self, command):
        ...
        self.connection.query(command)
```

#### Solution

```python
class SocketAdapter:
    def __init__(self, ip, port):
        connection = socket.new_connection(f"{ip}:{port}")
        ...

    def query(self, command):
        ...

class Instrument:
    connection: SocketAdapter
    ...

class USB2(Instrument):
    def __init__(self, ip, port):
        self.connection = SocketAdapter(ip, port)
        ...


class USB3(Instrument):
    def __init__(self, ip, port):
        self.connection = SocketAdapter(ip, port)
        ...
```

### Refactoring:

- Unify Interfaces with Adapter


