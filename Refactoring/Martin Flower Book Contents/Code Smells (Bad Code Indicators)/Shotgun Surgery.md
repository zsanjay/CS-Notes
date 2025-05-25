#### A single change requires modifications in multiple places.

Similar to [Divergent Change](https://luzkan.github.io/smells/divergent-change), but with a broader spectrum, the smell symptom of the _Shotgun Surgery_ code is detected by the unnecessary requirement of changing multiple different classes to introduce a single modification. Things like that can happen with the failure to use the correct design pattern for the given system. This expansion of functionality can lead to an easy miss (and thus introduce a bug) if these small changes are all over the place and they are hard to find. Most likely, [too many classes](https://luzkan.github.io/smells/oddball-solution) solve a simple problem.

Joshua Kerievsky noted this smell as _Solution Sprawl_ [[1](https://luzkan.github.io/smells/shotgun-surgery#sources)]. Monteiro stated that the tiny difference between these two comes from how they are sensed. In the [Divergent Change](https://luzkan.github.io/smells/divergent-change), one becomes aware of the smell while making the changes, and in the _Solution Sprawl_, one is aware by observing the issue. [[2](https://luzkan.github.io/smells/shotgun-surgery#sources)]

### Causation

Wake says it could have happened due to an "overzealous attempt to eliminate Divergent Change" [[3](https://luzkan.github.io/smells/shotgun-surgery#sources)]. A missing class could understand the entire responsibility and handle the existing cluster of changes by itself. That scenario could also happen with cascading relationships of classes [[4](https://luzkan.github.io/smells/shotgun-surgery#sources)].

### Problems

#### **Single Responsibility Principle Violation**

The codebase is non-cohesive.
#### **Duplicated Code**

The increased learning curve for new developers to effectively implement a change.

### Examples

#### Smelly

```python
class Minion:
    energy: int

    def attack(self):
        if self.energy < 20:
            animate('no-energy')
            skip_turn()
            return
        ...

    def cast_spell(self):
        if self.energy < 50:
            animate('no-energy')
            skip_turn()
            return
        ...

    def block(self):
        if self.energy < 10:
            animate('no-energy')
            skip_turn()
            return
        ...

    def move(self):
        if self.energy < 35:
            animate('no-energy')
            skip_turn()
            return
        ...
```


#### Solution

```python
class Minion:
    energy: int

    def attack(self):
        if not self.has_energy(20):
            return
        ...

    def cast_spell(self):
        if not self.has_energy(50):
            return
        ...

    def block(self):
        if not self.has_energy(10):
            return
        ...

    def move(self):
        if not self.has_energy(35):
            return
        ...

    def has_energy(self, energy_required: int) -> bool:
        if self.energy < energy_required:
            self.handle_no_energy()
            return False
        return True

    def handle_no_energy(self) -> None:
        animate('no-energy')
        skip_turn()
```

### Refactoring:

- Extract Method
- Combine Functions into Class
- Combine Functions into Transform
- Split Phase
- Move Method and Move Field
- Inline Function/Class


