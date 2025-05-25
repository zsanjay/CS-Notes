
# BAD SMELLS IN CODE

### 1. Duplicated code

Same code structure in more than one place.

### 2. Long Method

The longer a procedure is the more difficult is to understand.

### 3. Large Classes

When a class is trying to do too much, duplicated code cannot be far behind.

### 4. Long Parameter List

The are hard to understand, inconsistent and difficult to use.

### 5. Divergent Change

When one class is commonly changed in different ways for different reasons.

### 6. Shotgun Surgery

When every time you make a kind of change, you have to make a lot of little changes to a lot of different classes.

### 7. Feature Envy

A method that seems more interested in a class other than the one it actually is in.

### 8. Data Clumps

Bunches of data(fields, parameters...) that hang around together.

### 9. Primitive Obsession

Using primitives types instead of small objects.

### 10. Switch Statements

The same switch statement scattered about a program in different places. Use polymorphism.

### 11. Parallel Inheritance Hierarchies

Every time you make a subclass of one class, you also have to make a subclass of another.

### 12. Lazy Class

A class that isn't doing enough to pay for itself should be eliminated.

### 13. Speculative Generality

All sorts of hooks and special cases to handle things that aren't required.

### 14. Temporary Field

An instance variable that is set only in certain circumstances.

### 15. Message Chain

When a client asks one object for another object, which the client then asks for yet another object...

### 16. Middle Man

When an object delegates much of its functionality.

### 17. Inappropriate Intimacy

When classes access to much to another classes.

### 18. Alternative Classes with Different Interfaces

Classes with methods that look to similar.

### 19. Incomplete Library Class

When we need extra features in libraries.

### 20. Data Class

Don't allow manipulation in Data Classes. Use encapsulation and immutability.

### 21. Refused Bequest

Subclasses that don't make uses of parents methods.

### 22. Comments

Not all comments but the ones that are there because the code is bad.

# COMPOSING METHODS

## 1. Extract Method

You have a code fragment that can be grouped together.

```java
	void printOwing(double amount) {
		printBanner();
		//print details
		System.out.println ("name:" + _name);
		System.out.println ("amount" + amount);
	}
```

to

```java
	void printOwing(double amount) {
		printBanner();
		printDetails(amount);
	}

	void printDetails (double amount) {
		System.out.println ("name:" + _name);
	System.out.println ("amount" + amount);
	}
```


## 2. Remove Assignments to Parameters

```js

	void printOwing(double previousAmount) {
		printBanner();
		double outstanding = getOutstanding(previousAmount * 1.2);
		printDetails(outstanding);
	}

	double getOutstanding(double initialValue) {
		// Temp Variable Result
		double result = initialValue;
		Enumeration e = _orders.elements();

		while (e.hasMoreElements()) {
			Order each = (Order) e.nextElement();
			result += each.getAmount();
		}
		return result;
	}
```

```js
	int discount (int inputVal, int quantity, int yearToDate) {
		if (inputVal > 50) inputVal -= 2;
```

to

```js
	int discount (int inputVal, int quantity, int yearToDate) {
		int result = inputVal;
		if (inputVal > 50) result -= 2;
```

**Motivation**

- You can change the internals of object is passed but do not point to another object.
- Use only the parameter to represent what has been passed.
## 3. Inline Methods 

A method's body is just as clear as its name.

```js
	int getRating() {
		return (moreThanFiveLateDeliveries()) ? 2 : 1;
	}

	boolean moreThanFiveLateDeliveries() {
		return _numberOfLateDeliveries > 5;
	}
```

```js
	int getRating() {
		return (_numberOfLateDeliveries > 5) ? 2 : 1;
	}
```


## 4. Inline Temp

You have a temp that is assigned to once with a simple expression, and the temp is getting in the way of other refactorings.

```js
	double basePrice = anOrder.basePrice();
	return (basePrice > 1000)
```
to
```js
	return (anOrder.basePrice() > 1000)
```

## 5. Replace Temp with Query

You are using a temporary variable to hold the result of an expression.

```js
	double basePrice = _quantity * _itemPrice;
	if (basePrice > 1000)
		return basePrice * 0.95;
	else
		return basePrice * 0.98;
```

to 

```js
	if (basePrice() > 1000)
		return basePrice() * 0.95;
	else
		return basePrice() * 0.98;
	...
	double basePrice() {
		return _quantity * _itemPrice;
	}
```

**Motivation**

- Replacing the temp with a query method, any method in the class can get at the information.
- Is a vital step before [1. Extract Method](https://github.com/zsanjay/Refactoring-Summary?tab=readme-ov-file#1-extract-method)

## 6. Introduce Explaining Variable

You have a complicated expression

```js
	if ( (platform.toUpperCase().indexOf("MAC") > -1) &&
		(browser.toUpperCase().indexOf("IE") > -1) &&
		wasInitialized() && resize > 0 )
	{
		// do something
	}
```

to

```js
	final boolean isMacOs = platform.toUpperCase().indexOf("MAC") >-1;
	final boolean isIEBrowser = browser.toUpperCase().indexOf("IE") >-1;
	final boolean wasResized = resize > 0;
	if (isMacOs && isIEBrowser && wasInitialized() && wasResized) {
		// do something
	}
```

**Motivation**

- When expressions are hard to read

## 7. Split Temporary Variable

You have a temporary variable assigned to more than once, but is not a loop variable nor a collecting temporary variable.

```js
	double temp = 2 * (_height + _width);
	System.out.println (temp);
	temp = _height * _width;
	System.out.println (temp);
```

to

```js
	final double perimeter = 2 * (_height + _width);
	System.out.println (perimeter);
	final double area = _height * _width;
	System.out.println (area);
```

**Motivation**

- Variables should not have more than one responsibility.
- Using a temp for two different things is very confusing for the reader.

## 8. Replace Method with Method Object

You have a long method that uses local variables in such a way that you cannot apply Extract Method.

```java
	class Order...
		double price() {
			double primaryBasePrice;
			double secondaryBasePrice;
			double tertiaryBasePrice;
			// long computation;
			...
		}
```

to 

```java

	class Order...
		double price(){
			return new PriceCalculator(this).compute()
		}
	}

	class PriceCalculato...
	compute(){
		double primaryBasePrice;
		double secondaryBasePrice;
		double tertiaryBasePrice;
		// long computation;
		return ...
	}

```

**Motivation**

- When a method has lot of local variables and applying decomposition is not possible.

```java
	Class Account
		int gamma (int inputVal, int quantity, int yearToDate) {
			int importantValue1 = (inputVal * quantity) + delta();
			int importantValue2 = (inputVal * yearToDate) + 100;
			if ((yearToDate - importantValue1) > 100)
			importantValue2 -= 20;
			int importantValue3 = importantValue2 * 7;
			// and so on.
			return importantValue3 - 2 * importantValue1;
		}
	}
```

to

```java

	class Gamma...
		private final Account _account;
		private int inputVal;
		private int quantity;
		private int yearToDate;
		private int importantValue1;
		private int importantValue2;
		private int importantValue3;

		Gamma (Account source, int inputValArg, int quantityArg, int yearToDateArg) {
			_account = source;
			inputVal = inputValArg;
			quantity = quantityArg;
			yearToDate = yearToDateArg;
		}

		int compute () {
			importantValue1 = (inputVal * quantity) + _account.delta();
			importantValue2 = (inputVal * yearToDate) + 100;
			if ((yearToDate - importantValue1) > 100)
			importantValue2 -= 20;
			int importantValue3 = importantValue2 * 7;
			// and so on.
			return importantValue3 - 2 * importantValue1;
		}

		int gamma (int inputVal, int quantity, int yearToDate) {
			return new Gamma(this, inputVal, quantity,yearToDate).compute();
	}
```

## 9. Substitute Algorithm

You want to replace an algorithm with one that is clearer.

```java
	String foundPerson(String[] people){
		for (int i = 0; i < people.length; i++) {
			if (people[i].equals ("Don")){
				return "Don";
			}
			if (people[i].equals ("John")){
				return "John";
			}
			if (people[i].equals ("Kent")){
				return "Kent";
			}
		}
		return "";
	}
```

to

```java
	String foundPerson(String[] people){
		List candidates = Arrays.asList(new String[] {"Don", "John","Kent"});
	for (int i = 0; i<people.length; i++)
		if (candidates.contains(people[i]))
			return people[i];
	return "";
	}
```

**Motivation**

- Break down something complex into simpler pieces.
- Makes easier apply changes to the algorithm.
- Substituting a large, complex algorithm is very difficult; making it simple can make the substitution tractable.

# Moving features between elements

## 10. Move method

A method is, or will be, using or used by more features of another class than the class on which it is defined.

_Create a new method with a similar body in the class it uses most. Either turn the old method into a simple delegation, or remove it altogether._

```java
	class Class1 {
		aMethod()
	}

	class Class2 {	}
```

to

```java
	class Class1 {	}

	class Class2 {
		aMethod()
	}
```

**Motivation**

When classes do to much work or when collaborate too much and are highly coupled.

## 11. Move field

A field is, or will be, used by another class more than the class on which it is defined. _Create a new field in the target class, and change all its users._

```java
	class Class1 {
		aField
	}

	class Class2 {	}
```

to
```java
	class Class1 {	}

	class Class2 {
		aField
	}
```

## 12. Extract Class

You have one class doing work that should be done by two. _Create a new class and move the relevant fields and methods from the old class into the new class._

```java
	class Person {
		name,
		officeAreaCode,
		officeNumber,
		getTelephoneNumber()
	}
```

to

```java
	class Person {
		name,
		getTelephoneNumber()
	}

	class TelephoneNumber {
		areaCode,
		number,
		getTelephoneNumber()
	}
```

**Motivation**

Classes grow. _Split them when_:

- subsets of methods seem to be together
- subsets of data usually change together or depend on each other

## 13. Inline Class

A class isn't doing very much. _Move all its features into another class and delete it._

```java
	class Person {
		name,
		getTelephoneNumber()
	}

	class TelephoneNumber {
		areaCode,
		number,
		getTelephoneNumber()
	}
```

to

```java
	class Person {
		name,
		officeAreaCode,
		officeNumber,
		getTelephoneNumber()
	}
```

**Motivation**

After refactoring normally there are a bunch of responsibilities moved out of the class, letting the class with little left.

## 14. Hide Delegate

A client is calling a delegate class of an object.  
_Create methods on the server to hide the delegate._

```java
	class ClientClass {
		//Dependencies
		Person person = new Person()
		Department department = new Department()
		person.doSomething()
		department.doSomething()
	}
```

to
```java
	class ClientClass {
		Person person = new Person()
		person.doSomething()
	}

	class Person{
		Department department = new Department()
		department.doSomething()
	}
```

Solution

```java
	class ClientClass{
		Server server = new Server()
		server.doSomething()
	}

	class Server{
		Delegate delegate = new Delegate()
		void doSomething(){
			delegate.doSomething()
		}
	}
	// The delegate is hidden to the client class.
	// Changes won't be propagated to the client they will only affect the server
	class Delegate{
		void doSomething(){...}
	}
```

**Motivation**

Key of encapsulation. Classes should know as less as possible of other classes.

`> manager = john.getDepartment().getManager();`

```java
	class Person {
		Department _department;
		public Department getDepartment() {
			return _department;
		}
		public void setDepartment(Department arg) {
			_department = arg;
			}
		}

	class Department {
		private String _chargeCode;
		private Person _manager;
		public Department (Person manager) {
			_manager = manager;
		}
		public Person getManager() {
			return _manager;
		}
		...
```

to 

`> manager = john.getManager();`

```java
	class Person {
		...
		public Person getManager() {
			return _department.getManager();
		}
	}
```

## 15. Remove Middle Man

A class is doing too much simple delegation. _Get the client to call the delegate directly._

```java
	class ClientClass {
		Person person = new Person()
		person.doSomething()
	}

	class Person{
		Department department = new Department()
		department.doSomething()
	}
```

to

```java
	class ClientClass {
		//Dependencies
		Person person = new Person()
		Department department = new Department()
		person.doSomething()
		department.doSomething()
	}
```

**Motivation**

When the "Middle man" (the server) does to much is time for the client to call the delegate directly.

## 16. Introduce Foreign Method

A server class you are using needs an additional method, but you can't modify the class.  
_Create a method in the client class with an instance of the server class as its first argument._

```java
Date newStart = new Date(previousEnd.getYear(),previousEnd.getMonth(),previousEnd.getDate()+1);
```

to

```java
	Date newStart = nextDay(previousEnd);

	private static Date nextDay(Date date){
		return new Date(date.getYear(),date.getMonth(),date.getDate()+1);
	}
```

**Motivation**

When there is a lack of a method in class that you use a lot and you can not change that class.

## 17. Introduce Local Extension

A server class you are using needs several additional methods, but you can't modify the class. _Create a new class that contains these extra methods. Make this extension class a subclass or a wrapper of the original._

```java
	class ClientClass(){

		Date date = new Date()
		nextDate = nextDay(date);

		private static Date nextDay(Date date){
			return new Date(date.getYear(),date.getMonth(),date.getDate()+1);
		}
	}
```

to

```java
	class ClientClass() {
		MfDate date = new MfDate()
		nextDate = nextDate(date)
	}
	class MfDate() extends Date {
		...
		private static Date nextDay(Date date){
			return new Date(date.getYear(),date.getMonth(),date.getDate()+1);
		}
	}
```

**Motivation**

When plenty of [16. Introduce Foreign Method](https://github.com/zsanjay/Refactoring-Summary?tab=readme-ov-file#16-introduce-foreign-method) need to be added to a class.

# ORGANIZING DATA

## 18. Self Encapsulate Field

You are accessing a field directly, but the coupling to the field is becoming awkward.  
_Create getting and setting methods for the field and use only those to access the field._

```java
private int _low, _high;
boolean includes (int arg) {
	return arg >= _low && arg <= _high;
}
```

to

```java
private int _low, _high;
boolean includes (int arg) {
	return arg >= getLow() && arg <= getHigh();
}
int getLow() {return _low;}
int getHigh() {return _high;}
```

**Motivation**  
Allows a subclass to override how to get that information with a method and that it supports more flexibility in managing the data, such as lazy initialization.

## 19. Replace Data Value with Object

You have a data item that needs additional data or behavior.  
_Turn the data item into an object_

```java
	class Order...{
		private String _customer;
		public Order (String customer) {
			_customer = customer;
		}
	}
```

to

```java
class Order...{
		public Order (String customer) {
			_customer = new Customer(customer);
		}
	}

	class Customer {
		public Customer (String name) {
			_name = name;
		}
	}
```

**Motivation**  
Simple data items aren't so simple anymore.

## 20.  Change Value to Reference

You have a class with many equal instances that you want to replace with a single object.  
_Turn the object into a reference object._

```java
	class Order...{
		public Order (String customer) {
			_customer = new Customer(customer);
		}
	}

	class Customer {
		public Customer (String name) {
			_name = name;
		}
	}
```

to

```java
	//Use Factory Method
	class Customer...
		static void loadCustomers() {
			new Customer ("Lemon Car Hire").store();
			new Customer ("Associated Coffee Machines").store();
			new Customer ("Bilston Gasworks").store();
		}
		private void store() {
			_instances.put(this.getName(), this);
		}
		public static Customer create (String name) {
			return (Customer) _instances.get(name);
		}
```

**Motivation**  
Reference objects are things like customer or account. Each object stands for one object in the real world, and you use the object identity to test whether they are equal.

## 21. Change Reference to Value

You have a reference object that is small, immutable, and awkward to manage.  
_Turn it into a value object_

```java
new Currency("USD").equals(new Currency("USD")) // returns false
```

to

```java
	// Add `equals` and `hashCode` to the Currency class 
	class Currency{ 
		...
		public boolean equals(Object arg) {
	 		if (! (arg instanceof Currency)) return false;
	 		Currency other = (Currency) arg;
			return (_code.equals(other._code));
		}

		public int hashCode() {
 			return _code.hashCode();
 		}
 	}

 	// Now you can do
	new Currency("USD").equals(new Currency("USD")) // now returns true
```

**Motivation**  
Working with the reference object becomes awkward. The reference object is immutable and small. Used in distributed or concurrent systems.

## 22. Replace Array with Object

You have an array in which certain elements mean different things.  
_Replace the array with an object that has a field for each element_

```java
String[] row = new String[3];
row [0] = "Liverpool";
row [1] = "15";
```

to

```java
	Performance row = new Performance();
	row.setName("Liverpool");
	row.setWins("15");
```


### References

https://github.com/zsanjay/Refactoring-Summary?tab=readme-ov-file#1-duplicated-code


