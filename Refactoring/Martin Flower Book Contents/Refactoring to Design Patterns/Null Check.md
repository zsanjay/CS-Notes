### Avoid null checks with a dedicated class.

Null check is widespread everywhere because the programming languages allow it. It causes a multitude of `undefined` or `null` checks everywhere: in guard checks, in condition blocks, and verifications clauses. Instead, special objects could be created that implement the missing-event behavior, errors could be thrown and catched, and many duplications would be removed. Even an anecdote sometimes appears here and there on discussion forums that the inventor of the `null`reference, Tony Hoare (also known as the creator of the QuickSort algorithm), apologizes for its invention and calls it a _billion-dollar mistake_.

_Null Check_ is a special case of [Special Case](https://luzkan.github.io/smells/special-case) code smell

### Causation

The direct cause of null checking is the lack of a proper Null Object that might implement the object's behavior in case it's null. There is a strong opinion that `null` or `undefined` is a detrimentally bad idea in programming languages [[1](https://luzkan.github.io/smells/null-check#sources)].

### Problems

#### **Duplication** 

Usually, the null check reoccurs.

#### **Increased Complexity** 

Special cases must be made for an object that might be undefined.

#### **Bijection Violation** 

A `null`/`undefined` as a model is not in a one-to-one relationship with the domain. Moreover, there is no representation.

### Examples 

#### Smelly

```python
class BonusDamage(ABC):
    @abstractmethod
    def increase_damage(self, damage: float) -> float:
        """ Increases the output damage """


class Critical(BonusDamage):
    multiplier: float

    def increase_damage(self, damage: float):
        def additional_damage() -> float:
            return damage * self.multiplier * math.random(0, 2)

        return damage + additional_damage()


class Magical(BonusDamage):
    multiplier: float

    def increase_damage(self, damage: float):
        return damage * multiplier


bonus_damage: BonusDamage | None = perk.get_bonus_damage()

def example_of_doing_something_with_bonus_damage(bonus_damage: BonusDamage | None) -> ... | None:
    if not bonus_damage:
        return
```

#### Solution

```python
class BonusDamage(ABC):
    @abstractmethod
    def increase_damage(self, damage: float) -> float:
        """ Increases the output damage """


class Critical(BonusDamage):
    multiplier: float

    def increase_damage(self, damage: float):
        def additional_damage() -> float:
            return damage * self.multiplier * math.random(0, 2)

        return damage + additional_damage()


class Magical(BonusDamage):
    multiplier: float
    def increase_damage(self, damage: float):
        return damage * multiplier


class NullBonusDamage(BonusDamage):
    def increase_damage(self, damage: float):
        return damage


bonus_damage: BonusDamage = perk.get_bonus_damage()

def example_of_doing_something_with_bonus_damage(bonus_damage: BonusDamage) -> ...:
```

### Refactoring:

- Introduce Null Object
- Introduce `Maybe`/`Optional`



