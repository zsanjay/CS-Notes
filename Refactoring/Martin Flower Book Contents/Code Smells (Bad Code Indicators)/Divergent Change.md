### Multiple reasons to change a single class.

If adding a simple feature makes the developer change many seemingly unrelated methods inside a class, that indicates the _Divergent Change_ code smell. Simply put, the class has irrelevant methods in it [[1](https://luzkan.github.io/smells/divergent-change#sources)]. As an example, suppose that someone needs to modify class `A` due to a change in the database but then has to modify the same class `A` due to a change in the calculation formula [[2](https://luzkan.github.io/smells/divergent-change#sources)].

The difference between _Divergent Change_ and [_Shotgun Surgery_](https://luzkan.github.io/smells/shotgun-surgery) is that the _Divergent Change_ addresses the issue within a class, while the _Shotgun Surgery_ between classes.

### Causation

Over time, a class tries to do more and more things and has many responsibilities. The fact that the class already has two or more different types of decisions implemented (for example, finding an object and doing something with object [[3](https://luzkan.github.io/smells/divergent-change#sources)]) was overlooked and left unrefactored.

### Problems

#### **Single Responsibility Principle Violation**

The class has too much responsibility.

#### **Duplication**

### Example

#### Smelly

```python
class ReportModifier:
    def get_report(self, report_name):
        ...
        return report

    def modify_report(self, report, new_entry):
        ...
        return modified_report

    def run(self, report_name, new_entry):
        report = self.get_report(report_name)
        return self.modify_report(report, new_entry)


report_modifier = ReportModifier(...)
modified_report = report_modifier.run('raport.csv', 'Parsed')
```

#### Solution

```python
class ReportReader:
    def get_report(self, report_name):
        ...
        return report

class ReportModifier:
    def modify_report(self, report, new_entry):
        ...
        return modified_report

report_reader = ReportReader(...)
report = report_reader.get_report('raport.csv')

report_modifier = ReportModifier(...)
modified_report = report_modifier.modify_report(report, 'Parsed')
```

### Refactoring:

- Extract Superclass, Subclass, or new Class
- Extract Function
- Move Function




