
### Problem

A class has features that are used only in certain cases.

```js
class JobItem {
	getTotalPrice(),
	getUnitPrice(),
	getEmployee()
}
```

### Solution

Create a subclass and use it in these cases.

```js
class LabourItem extends JobItem {
	getUnitPrice(),
	getEmployee()
}
```

### Why Refactor

Your main class has methods and fields for implementing a certain rare use case for the class. While the case is rare, the class is responsible for it and it would be wrong to move all the associated fields and methods to an entirely separate class. But they could be moved to a subclass, which is just what we’ll do with the help of this refactoring technique.

### Benefits

- Creates a subclass quickly and easily.
- You can create several separate subclasses if your main class is currently implementing more than one such special case.

### Drawbacks

- Despite its seeming simplicity, _Inheritance_ can lead to a dead end if you have to separate several different class hierarchies. If, for example, you had the class `Dogs` with different behavior depending on the size and fur of dogs, you could tease out two hierarchies:

    - by size: `Large`, `Medium` and `Small`
    - by fur: `Smooth` and `Shaggy`

    And everything would seem well, except that problems will crop up as soon as you need to create a dog that’s both `Large` and `Smooth`, since you can create an object from one class only. That said, you can avoid this problem by using _Compose_ instead of _Inherit_ (see the [Strategy](https://refactoring.guru/design-patterns/strategy)pattern). In other words, the `Dog` class will have two component fields, size and fur. You will plug in component objects from the necessary classes into these fields. So you can create a `Dog` that has `LargeSize` and `ShaggyFur`.


A simple example: in the Car class, you had the field `isElectricCar` and, depending on it, in the `refuel()` method the car is either fueled up with gas or charged with electricity. Post-refactoring, the `isElectricCar` field is removed and the `Car` and `ElectricCar` classes will have their own implementations of the `refuel()` method.
