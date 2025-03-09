
https://www.interviewbit.com/react-mcq/

https://github.com/sudheerj/reactjs-interview-questions

https://github.com/sudheerj/javascript-interview-questions

https://www.frontendlead.com/trivia-questions

https://frontendlead.com/handbook/css-knowledge-and-best-practices

https://frontendlead.com/handbook/html-knowledge-and-best-practices

https://frontendlead.com/handbook/javascript-fundamentals

Important - https://medium.com/@phamtuanchip/top-30-interview-questions-and-answers-for-senior-web-developers-with-react-js-210190a6f847

https://blog.mettl.com/reactjs-interview-questions/

React Lifecycle

https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/

https://www.youtube.com/watch?v=m_mtV4YaI8c

### **What is `<Switch>` in React Router DOM?**

`<Switch>` is a component from **React Router (v5 and earlier)** that **renders the first matching `<Route>`** from a list of routes. It ensures that **only one route is rendered at a time**, even if multiple routes match the URL.

⚠️ **Note:** `<Switch>` has been replaced with `<Routes>` in React Router v6.

---

## **Usage of `<Switch>` in React Router v5**


```jsx
import React from "react";
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";
import Home from "./Home";
import About from "./About";
import NotFound from "./NotFound";

function App() {
  return (
    <Router>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
        <Route component={NotFound} /> {/* Catches all unmatched routes */}
      </Switch>
    </Router>
  );
}

export default App;
```

### **How `<Switch>` Works**

1. Checks routes from top to bottom.
2. Renders the **first** `<Route>` that matches the URL.
3. Stops checking further routes (prevents multiple renders).

### **Why Use `<Switch>`?**

✅ Prevents multiple routes from rendering simultaneously.  
✅ Improves performance by stopping after the first match.  
✅ Allows a **catch-all 404 page** for unmatched routes.

---

## **React Router v6: Replacing `<Switch>` with `<Routes>`**

In **React Router v6**, `<Switch>` was replaced with `<Routes>`, and `<Route>` now requires an element.

### ✅ **Equivalent Code in React Router v6**

```jsx
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Home from "./Home";
import About from "./About";
import NotFound from "./NotFound";

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="*" element={<NotFound />} /> {/* 404 route */}
      </Routes>
    </Router>
  );
}

export default App;
```

### **Key Differences:**

|Feature|React Router v5 (`<Switch>`)|React Router v6 (`<Routes>`)|
|---|---|---|
|Component name|`<Switch>`|`<Routes>`|
|Route matching|Stops at first match|Stops at first match|
|Route syntax|`component={}` prop|`element={}` prop required|

---

## **Should You Use `<Switch>`?**

- If you're using **React Router v5**, `<Switch>` is useful.
- If you're on **React Router v6+, use `<Routes>`** instead.


### **What is Cross-Site Scripting (XSS)?**

Cross-Site Scripting (XSS) is a **security vulnerability** that allows attackers to inject **malicious scripts** into web applications. These scripts are executed in a user's browser, potentially leading to data theft, session hijacking, or defacement.

---

## **Types of XSS Attacks**

### 1️⃣ **Stored XSS (Persistent XSS)**

- Malicious script is **permanently stored** on the server (e.g., in a database).
- Affects all users who load the compromised page.

**Example:**

```html
<script>
	alert('You are hacked!');
</script>
```

- If this script is stored in a **comments section** and executed when another user views it, it becomes a **Stored XSS attack**.

---

### 2️⃣ **Reflected XSS**

- Malicious script is **injected via a URL or request parameter** and executed immediately when the victim clicks on the link.
- Typically used in **phishing attacks**.

**Example:**

```html
<a href="http://example.com/search?query=<script>alert('Hacked')</script>">
Click Here</a>
```

- If the server **does not sanitize input**, this script is executed when a user clicks the link.

---

### 3️⃣ **DOM-Based XSS**

- The malicious script is injected into the **client-side JavaScript** and executed in the browser.
- No request to the server is needed.

**Example:**

```js
const userInput = window.location.hash.substring(1);
document.body.innerHTML = userInput; // XSS if input contains <script>
```

- If a user visits:  
    `http://example.com#<script>alert('XSS')</script>`  
    the script executes in their browser.

---

## **How to Prevent XSS Attacks**

### ✅ **1. Sanitize and Escape User Input**

- Use libraries like **DOMPurify** to remove malicious HTML.

```js
import DOMPurify from 'dompurify';
const safeHTML = DOMPurify.sanitize(userInput);
```

- Escape HTML before rendering:

```js
function escapeHTML(str) {
  return str.replace(/</g, "&lt;").replace(/>/g, "&gt;");
}
```

---

### ✅ **2. Use Content Security Policy (CSP)**

- A CSP restricts the execution of inline scripts.
- Example CSP header:

```pgsql
Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted.com
```

---

### ✅ **3. Validate and Sanitize Server-Side Input**

- Use libraries like **express-validator** for **Node.js**.

```js
app.post('/comment', (req, res) => {
  const sanitizedComment = sanitize(req.body.comment);
  saveToDatabase(sanitizedComment);
});
```

---

### ✅ **4. Avoid `innerHTML` and Use `textContent`**

- **Bad Practice** (Vulnerable to XSS):

```js
document.getElementById("output").innerHTML = userInput;
```

- **Good Practice** (Safe from XSS):

```js
document.getElementById("output").textContent = userInput;
```

---

### ✅ **5. Use HTTP-Only Cookies for Session Tokens**

- Prevents attackers from accessing cookies via JavaScript.

```http
Set-Cookie: sessionToken=abc123; HttpOnly; Secure
```

---

## **Conclusion**

XSS is a **critical security vulnerability** that can compromise user data and site integrity. To protect against XSS: ✅ **Sanitize input**  
✅ **Use CSP headers**  
✅ **Escape output**  
✅ **Avoid `innerHTML`**  
✅ **Use HTTP-Only cookies**

### Spread vs Rest

The **spread operator** (`...`) and the **rest operator** (`...`) look the same, but they are used in different contexts to serve different purposes. Here's how they differ:

### 1. Spread Operator (`...`):

The **spread operator** is used to **unpack** elements from an array or object. It is used when you want to expand or spread out the elements of an iterable (like an array or object) into individual items.

#### Common uses of the spread operator:

- **Arrays**: Spread an array into another array.
- **Objects**: Spread the properties of one object into another.
- **Function calls**: Spread arguments into a function call.

**Examples:**

**Array example:**

```js
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // Spread arr1 into arr2
console.log(arr2); // [1, 2, 3, 4, 5]
```

**Object example:**

```js
const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 }; // Spread obj1 into obj2
console.log(obj2); // { a: 1, b: 2, c: 3 }
```

**Function example:**

```js
const nums = [1, 2, 3];
const sum = (a, b, c) => a + b + c;
console.log(sum(...nums)); // 6
```

### 2. Rest Operator (`...`):

The **rest operator** is used to **collect** multiple elements into a single array or object. It is typically used in function parameters or destructuring assignments to collect remaining items.

#### Common uses of the rest operator:

- **Function parameters**: Collect arguments into an array.
- **Array destructuring**: Collect the remaining elements in an array.
- **Object destructuring**: Collect the remaining properties in an object.

**Examples:**

**Function parameters example:**

```js
const sum = (...numbers) => {
  return numbers.reduce((acc, num) => acc + num, 0);
};
console.log(sum(1, 2, 3, 4)); // 10
```

**Array destructuring example:**

```js
const arr = [1, 2, 3, 4];
const [first, second, ...rest] = arr;
console.log(first); // 1
console.log(second); // 2
console.log(rest); // [3, 4]
```

**Object destructuring example:**

```js
const obj = { a: 1, b: 2, c: 3, d: 4 };
const { a, b, ...others } = obj;
console.log(a); // 1
console.log(others); // { c: 3, d: 4 }
```

### Key Differences:

- The **spread operator** is used to _expand_ or _spread_ elements (arrays, objects) into separate values.
- The **rest operator** is used to _collect_ multiple values into an array or object.

### Summary:

- **Spread**: Expands elements.
- **Rest**: Collects elements.
