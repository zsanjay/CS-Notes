
## 1. Replace Magic Number with Symbolic Constant

## **Problem**

Your code uses a number or a string that has a certain meaning to it.

## **Why Refactor ?**

The use of Magic Numbers and string literals makes it hard to understand the code's logic, and modifying them requires changes in multiple places. Refactoring the code would make it easier to read and understand, reduce the complexity of the code, and improve its maintainability. This “anti-pattern” makes it harder to understand the program and refactor the code

## **Benefits**

**1:** The symbolic constant can serve as live documentation of the meaning of its value.
  
**2.** It’s much easier to change the value of a constant than to search for this number throughout the entire codebase, without the risk of accidentally changing the same number used elsewhere for a different purpose

**3.** By using Symbolic Constants, the code can be reused in other parts of the application.

# **How to Refactor?**

**1:** Declare a constant and assign the value of the magic number to it.


```js
export function potentialEnergy(mass, height) {

	return mass * height * 9.81;

}
```


## Solution

```js

const GRAVITY = 9.81;

export function potentialEnergy(mass, height) {
	return mass * height * GRAVITY;
}
```


## 2.  Primitive Obession

## **Problem**

Primitive Obsession code smell means that a computer program is using simple data types like numbers or strings too much instead of creating new, more complex types. This can make the program harder to understand and change in the future.

## **Why Refactor ?**

The code uses the Primitive Obsession code smell because it is using primitive types (strings and numbers) to represent more complex concepts (currencies) instead of creating a dedicated class or data structure for them. Using primitive types in this way can lead to a number of issues. For example, if additional currencies were added to the function, new if statements and calculations would have to be added, making the code more complex and difficult to maintain

## **How ?**

A better approach would be to create a Currency class or data structure that encapsulates the currency and exchange rate information. This class could then be used in the calculateBill function, making it more flexible and easier to modify in the future.


```js
export const calculateBill = (amount, currency) => {

if (currency === 'USD') {

return "$" + amount * 0.84;

} else if (currency === 'EUR') {

return "€" + amount;

} else if (currency === 'GBP') {

return "£" + amount * 1.17;

} else {

return amount + " " + currency;

}

}
```

## Solution

Use Polymorphism

```js
  
export const calculateBill = (amount, currency) => {

return (BILL_CLASSES[currency] || new Bill()).getAmount(amount, currency);

}

  

class Bill {

  

getAmount(amount, currency) {

return `${amount} ${currency}`;

}

}

  

class BillInUSD extends Bill {

  

getAmount(amount, currency) {

return "$" + amount * 0.84;

}

}

  

class BillInEUR extends Bill {

  

getAmount(amount, currency) {

return "€" + amount;

}

}

  

class BillInGBP extends Bill {

  

getAmount(amount, currency) {

return "£" + amount * 1.17;

}

}

  

const BILL_CLASSES = {

USD: new BillInUSD(),

EUR: new BillInEUR(),

GBP: new BillInGBP(),

};
```

## Solution 2


```javascript
 const currencies = {
      USD: {
        symbol: "$",
        conversionRate: 0.84
      },
      EUR: {
        symbol: "€",
        conversionRate: 1
      },
      GBP: {
        symbol: "£",
        conversionRate: 1.17
      }
    };
    
    export const calculateBill = (amount, currencyCode) => {
      const currency = currencies[currencyCode];
      if (currency) {
        const convertedAmount = amount * currency.conversionRate;
        return currency.symbol + convertedAmount;
      } else {
        return amount + currencyCode;
      }
    };
```


## 3.  Replace Error Code with Exception


## **Problem**

A method returns a special value that indicates an error.

## **Why Refactor ?**

Returning error codes is an obsolete holdover from procedural programming. In modern programming, error handling is performed by special classe, which are `named exceptions`. If a problem occurs, you “throw” an error, which is then “caught” by one of the exception handlers. Special error-handling code, which is ignored in normal conditions, is activated to respond


## **Benefits**

**1:** Frees code from a large number of conditionals for checking various error codes. Exception handlers are a much more succinct way to differentiate normal execution paths from abnormal ones.

  
**2.** Exception classes can implement their own methods, thus containing part of the error handling functionality (such as for sending error messages).

  
**3.** Unlike exceptions, error codes can’t be used in a constructor, since a constructor must return only a new object.

  
# **How to Refactor?**

**1:** Find call to the method that returns error codes and, instead of checking for an error code, wrap it in try/catch blocks.

**2:** Inside the method, instead of returning an error code, throw an exception.

## **Criteria**

1)All the **test** should be passing  
2)**Exception** should be thrown instead of returning error-code.

```js
export const divide = (num1, num2) => {

if (num2 === 0) {

return -1 // error code indicating division by zero

}

return num1 / num2

}

const result = divide(10, 0)

if (result === -1) {

console.error("Error: Division by zero")

}
```

## Solution


```javascript
 export const divide = (num1, num2) => {
      if (num2 === 0) {
        throw new Error("Division by zero");
      }
      return num1 / num2;
    }
    
    try {
      const result = divide(10, 0);
      console.log(result);
    } catch (error) {
      console.error("Error: " + error.message);
    }
```


### References

https://www.refactornow.dev/problem/error-code