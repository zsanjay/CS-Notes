### A method uses more data from another class than its own.

If a method inside a class manipulates more features (be it fields or methods) of another class more than from its own, then this method has a _Feature Envy_. In Object-Oriented Programming, developers should tie the functionality and behavior close to the data it uses. The instance of this smell indicates that the method is in the wrong place and is more tightly coupled to the other class than to the one where it is currently located. [[1](https://luzkan.github.io/smells/feature-envy#sources)]

This was the explanation based on Fowler's book from 1999. In his recent "book update", he rephrased the _class_ into _module_, generalizing the concept from a _zone_ perspective. Depending on the size of the system, the _Feature Envy_ code smell may apply accordingly.
### Causation

The root cause of this smell is misplaced responsibility.

### Problems

#### **Low Testability**

Difficult to create proper test or tests in separation. Mocking is required.

#### **Inability to Reuse**

Coupled objects have to be used together. This can cause lousy duplication issues if one tries to reuse applicable code by extracting and cutting off what he does not need.

#### **Bijection Problems**

### Example

#### Smelly

```python
@dataclass(frozen=True)
class ShoppingItem:
    name: str
    price: float
    tax: float


class Order:
    ...
    def get_bill_total(self, items: list[ShoppingItem]) -> float:
        return sum([item.price * item.tax for item in items])

    def get_receipt_string(self, items: list[ShoppingItem]) -> list[str]:
        return [f"{item.name}: {item.price * item.tax}$" for item in items]

    def create_receipt(self, items: list[ShoppingItem]) -> float:
        bill = self.get_bill_total(items)
        receipt = self.get_receipt_string(items).join('\n')
        return f"{receipt}\nBill {bill}"
```

#### Solution

```python
@dataclass(frozen=True)
class ShoppingItem:
    name: str
    price: float
    tax: float

    @property
    def taxed_price(self) -> float:
        return self.price * self.tax

    def get_receipt_string(self) -> str:
        return f"{self.name}: {self.price * self.tax}$"

class Order:
    ...
    def get_bill_total(items: list[ShoppingItem]) -> float:
        return sum([item.taxed_price for item in items])

    def get_receipt_string(items: list[ShoppingItem]) -> list[str]:
        return [item.get_receipt_string() for item in items]

    def create_receipt(items: list[ShoppingItem]) -> float:
        bill = self.get_bill_total(items)
        receipt = self.get_receipt_string(items).join('\n')
        return f"{receipt}\nBill: {bill}$"
```

### Refactoring:

- Move Method
- Move Field
- Extract Method


