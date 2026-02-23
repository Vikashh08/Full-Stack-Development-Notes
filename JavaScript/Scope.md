# Scope in JavaScript

Scope is one of the most fundamental and important concepts in JavaScript. It defines where variables and functions are accessible in your code.

Understanding scope is essential because it is the foundation for:
- **Hoisting**
- **Closures**
- `this` keyword
- **Execution Context**
- **Variable lifetime**
- **Memory management**

---

## What is Scope?
Scope is the region of code where a variable or function can be accessed.

In simple words: **Scope determines the visibility and accessibility of variables.**

### Real-Life Example
Imagine your house:
- **Your bedroom** → only you can access → *Private Scope*
- **Living room** → everyone in the house can access → *Broader Scope*
- **Public park** → everyone can access → *Global Scope*

JavaScript works the exact same way when handling variables!

---

## Types of Scope in JavaScript
JavaScript has 4 main types of scope:
1. **Global Scope**
2. **Function Scope**
3. **Block Scope**
4. **Lexical Scope**

---

### 1. Global Scope
A variable declared outside all functions and blocks belongs to the **global scope**.
It can be accessed and modified from anywhere in the program.

#### Example:
```javascript
let name = "Vikash";

function greet() {
    console.log(name);
}

greet();
console.log(name);
```

**Output:**
```text
Vikash
Vikash
```

**Explanation:**  
`name` is declared globally, so it is accessible everywhere, including inside the `greet` function.

#### Global Scope and Execution Context
Global variables are stored in the **Global Execution Context's Memory Phase**.

- **Memory phase:**
  - `name` → `undefined`
  - `greet` → function definition

- **Execution phase:**
  - `name` → `"Vikash"`

---

### 2. Function Scope
Variables declared with `var`, `let`, or `const` inside a function are only accessible inside that specific function. They cannot be accessed from outside.

#### Example:
```javascript
function test() {
    let age = 21;
    console.log(age);
}

test();
console.log(age);
```

**Output:**
```text
21
ReferenceError: age is not defined
```

**Explanation:**  
`age` only exists inside the `test` function. Trying to access it outside throws a ReferenceError.

#### Function Scope with `var`
```javascript
function demo() {
    var city = "Delhi";
    console.log(city);
}

demo();
console.log(city);
```

**Output:**
```text
Delhi
ReferenceError: city is not defined
```

> [!IMPORTANT]
> **Important Interview Point:**  
> `var` is **function scoped**, NOT **block scoped**.

#### Example (Why `var` isn't block scoped):
```javascript
function example() {
    if (true) {
        var x = 10;
    }
    console.log(x);
}

example();
```

**Output:**
```text
10
```
Because `var` ignores the `{}` block scope of the `if` statement, it remains accessible throughout the `example` function.

---

### 3. Block Scope
Block scope is created using curly braces `{ }` (like in `if` statements, `for` loops, etc.).

Variables declared with `let` and `const` are **block scoped**, meaning they won't exist outside their specific block.

#### Example:
```javascript
{
    let x = 10;
    const y = 20;

    console.log(x);
    console.log(y);
}

console.log(x);
console.log(y);
```

**Output:**
```text
10
20
ReferenceError: x is not defined
ReferenceError: y is not defined
```

**Explanation:**  
Because `x` and `y` were declared using `let` and `const`, they exist only inside that `{}` block.

#### `var` is NOT Block Scoped
```javascript
{
    var z = 30;
}
console.log(z);
```

**Output:**
```text
30
```
Because `var` completely ignores the block scope `{ }`.

#### Interview Summary Table

| Keyword | Scope Type |
|---------|------------|
| `var`   | Function scope |
| `let`   | Block scope |
| `const` | Block scope |

---

### 4. Lexical Scope (VERY IMPORTANT)
**Lexical scope** (also called Static Scope) means scope is determined by **where the variables and functions are written** in the source code. An inner function will always have access to its outer function's variables.

#### Example:
```javascript
let globalVar = "Global";

function outer() {
    let outerVar = "Outer";

    function inner() {
        console.log(globalVar);
        console.log(outerVar);
    }

    inner();
}

outer();
```

**Output:**
```text
Global
Outer
```

**Explanation:**  
The `inner` function can seamlessly access the variables of its parent function (`outer`) AND the global variable (`globalVar`) due to lexical scoping.

---

## Scope Chain
JavaScript uses a **Scope Chain** to find variables. If a variable is not found in the current scope, JavaScript looks up to the next outer scope, checking layer by layer until it reaches the global scope.

**Order of lookup:**
```text
Current Scope → Outer Scope → Global Scope → undefined (Throws ReferenceError)
```

#### Example:
```javascript
let a = 10;

function outer() {
    let b = 20;

    function inner() {
        let c = 30;

        console.log(a); // 10 (from global)
        console.log(b); // 20 (from outer)
        console.log(c); // 30 (from current inner)
    }

    inner();
}

outer();
```

#### Scope Chain Visual
```text
[inner scope] → [outer scope] → [global scope]
```

---

## Variable Shadowing
Variable shadowing happens when a variable in a local scope has the exact same name as a variable in an outer or global scope.

#### Example:
```javascript
let x = 10;

function test() {
    let x = 20; // Shadows the global x
    console.log(x);
}

test();
console.log(x);
```

**Output:**
```text
20
10
```
The local variable `x` "shadows" (overrides for that scope only) the global variable `x` while inside the `test` function.

#### Illegal Shadowing
If you declare a variable using `let` in an outer scope, re-declaring it with `var` in an inner block scope is called **illegal shadowing** and causes a `SyntaxError`.

```javascript
let x = 10;

// Block scope
{
    var x = 20; // SyntaxError: Identifier 'x' has already been declared
}
```
This causes an immediate error because `var` scopes globally/to the function and conflicts perfectly with the block-scoped `let`.

---

## Interview Questions on Scope

**Q1: What is scope?**  
**Answer:** Scope defines the region of the code where variables and functions can be accessed and used.

**Q2: What are the main types of scope?**  
**Answer:**
1. Global scope
2. Function scope
3. Block scope
4. Lexical scope

**Q3: What is the difference between `var`, `let`, and `const` in terms of scope?**  
**Answer:**
- `var` is **function scoped**.
- `let` is **block scoped**.
- `const` is **block scoped**.

**Q4: What is lexical scope?**  
**Answer:** Lexical scope means inner functions have access to the variables defined in their outer (parent) functions, based precisely on where they are written in the code.

**Q5: What is the scope chain?**  
**Answer:** The scope chain is the mechanism JavaScript uses to resolve variables by traversing up through nested scopes until it hits the global scope.

---

## Advanced Interview Example
**Predict output:**
```javascript
let x = 10;

function outer() {
    let x = 20;

    function inner() {
        console.log(x);
    }

    inner();
}

outer();
```

**Output:**
```text
20
```
**Explanation:**  
Because `inner` uses the closest available variable in the scope chain, which is `x = 20` from the `outer` function!

---

## Placement-Level Summary
- **Scope** determines variable accessibility.
- **Hierarchy:**
  ```text
  Block Scope → Function Scope → Global Scope
  ```
- **Lexical scope** perfectly determines how the **scope chain** behaves.

### Next Concept
> **Closures (MOST IMPORTANT JavaScript interview topic)**  
> *Note: Closures depend entirely on a strong understanding of Scope.*