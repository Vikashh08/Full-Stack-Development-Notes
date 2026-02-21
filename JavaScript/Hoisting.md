#  Hoisting in JavaScript: The Ultimate Interview Guide

**Hoisting** is one of the most frequently asked topics in JavaScript interviews. It can seem confusing at first, but once you understand how JavaScript executes code under the hood, it becomes incredibly simple.

## Simple Explanation (The "What")
Imagine you are packing for a trip. Before you put anything inside your suitcase, you first make a **list** of all the items you need to pack (shirts, shoes, toothbrush). 

JavaScript does exactly this! Before it runs a single line of your code, it scans the file and makes a "list" of all your variables and functions. It conceptually moves these declarations to the **top** of their scope so they are ready to use. This behavior is called **Hoisting**.

---

## The Core Concept (The "Why")

In interviews, saying *"Declarations are moved to the top"* is a basic answer. The **advanced, placement-level answer** involves explaining the **Execution Context**.

Whenever you run JavaScript, it creates an **Execution Context**. This context works in two phases:

### Phase 1: Memory Creation Phase 🧠
JavaScript scans the code line-by-line before executing it. It allocates memory for:
*   **Variables (`var`)**: Stored in memory and initialized with a special placeholder: `undefined`.
*   **Variables (`let` & `const`)**: Stored in memory but **not initialized**. They stay in a restricted state.
*   **Functions (Declarations)**: The **entire function code** is copied and stored directly into memory.

### Phase 2: Code Execution Phase ⚙️
JavaScript goes through the code again line by line, assigns the actual values to variables, and executes the functions.

---

## 1. Variable Hoisting Explained

### `var` Hoisting (Initialized with `undefined`)
Because `var` is initialized with `undefined` during the memory phase, accessing it before its declaration won't throw an error; it just prints `undefined`.

```javascript
console.log(studentName); // Output: undefined
var studentName = "Vikash";
console.log(studentName); // Output: Vikash
```

**How the JS Engine sees it:**
```javascript
var studentName; // memory allocated -> undefined
console.log(studentName);
studentName = "Vikash"; // actual assignment during execution
```

### `let` and `const` (The Temporal Dead Zone)
Are `let` and `const` hoisted? **Yes, they are!** 
But unlike `var`, they are *not* given the `undefined` placeholder. If you try to use them before they are declared, JavaScript throws a `ReferenceError`.

This restricted period is called the **Temporal Dead Zone (TDZ)**.

```javascript
// TDZ for 'age' starts here
console.log(age); // ❌ ReferenceError: Cannot access 'age' before initialization
let age = 25;     // TDZ ends here
```

> **Interview Tip:** The TDZ exists to catch errors. It forces developers to declare variables before using them, which is a good coding practice.

---

## 2. Function Hoisting Explained

###  Function Declarations (Complete Hoisting)
Function declarations are fully hoisted. This means you can call the function before you even write it in your code!

```javascript
sayHello(); // Output: "Hello string!"

function sayHello() {
    console.log("Hello string!");
}
```

###  Function Expressions & Arrow Functions (Behave like Variables)
If you assign a function or an arrow function to a variable (`var`, `let`, or `const`), it follows **variable hoisting rules**, not function hoisting rules.

```javascript
greet(); // ❌ TypeError: greet is not a function

var greet = function() {
    console.log("Hi there!");
};
```
*Why a TypeError?* Because `greet` is declared with `var`, it is hoisted as `undefined`. You are basically trying to call `undefined()`, which causes the error!

---

##  The Clash: Function vs Variable Priority

What if a variable and a function have the exact same name?
*   During the **Memory Phase**, Functions win.
*   During the **Execution Phase**, Variable assignments win.

```javascript
console.log(magic); // Output: [Function: magic]

var magic = 100;

function magic() {
    console.log("I am a function");
}

console.log(magic); // Output: 100
```
*Because the function is stored first, the `var magic` declaration is ignored. However, during execution, `magic` is reassigned the value `100`.*

---

##  Top Interview Questions on Hoisting

###  Level 1: The Basics

**Q1: What is hoisting in JavaScript?**
**Simple Answer:** It is JavaScript's default behavior of moving variable and function declarations to the top of their scope before code execution.
**Advanced Answer:** During the Memory Creation Phase of the Execution Context, JavaScript allocates memory for variables and functions before executing the code line-by-line.

**Q2: Are `let` and `const` hoisted?**
**Answer:** Yes, they are hoisted! However, they are not initialized with `undefined` like `var` is. They remain in the Temporal Dead Zone (TDZ) until their actual declaration line is executed.

**Q3: What is the Temporal Dead Zone (TDZ)?**
**Answer:** It is the period between entering a block scope and the actual execution of a variable declaration (`let` or `const`). Accessing the variable during this zone results in a `ReferenceError`.

### Level 2: Tricky Snippets

**Q4: Predict the Output**
```javascript
var x = 20;

function test() {
    console.log(x);
    var x = 40;
}
test();
```
**Answer:** `undefined`.
**Explanation:** Inside the `test()` function, a new local execution context is created. The local `var x` is hoisted to the top of the function and initialized with `undefined`. The `console.log` sees this local `x` (which is `undefined`) instead of the global `x` (`20`). This is called **variable shadowing**.

**Q5: Predict the Output**
```javascript
foo();

var foo = function() {
    console.log("Hello!");
};
```
**Answer:** `TypeError: foo is not a function`.
**Explanation:** `foo` is declared using `var`, so it is hoisted and initialized with `undefined`. Calling `foo()` before assignment is like calling `undefined()`.

### Level 3: Advanced

**Q6: Predict the Output**
```javascript
function login() {
    if (true) {
        var user = "Vikash";
        let role = "Admin";
    }
    console.log(user); 
    console.log(role); 
}
login();
```
**Answer:** 
- `Vikash`
- `ReferenceError: role is not defined`

**Explanation:** 
- `var` is **function-scoped**. It hoists to the top of `login()` and ignores the `if` block, making it accessible at the end of the function. 
- `let` is **block-scoped**. It is confined completely inside the `{}` of the `if` statement and cannot be accessed outside it.

---

##  Quick Summary Table (Memorize This!)

| Keyword | Hoisted? | Initialized in Memory Phase | Scope | Accessible Before Declaration? |
| :--- | :---: | :--- | :--- | :--- |
| **`var`** | ✅ Yes | `undefined` | Function/Global | ✅ Yes (As `undefined`) |
| **`let`** | ✅ Yes | *None (TDZ)* | Block | ❌ No (`ReferenceError`) |
| **`const`**| ✅ Yes | *None (TDZ)* | Block | ❌ No (`ReferenceError`) |
| **`function`**| ✅ Yes | Complete function code | Function/Global| ✅ Yes |

---
**Next Concept (VERY IMPORTANT):** We study **Scope in JavaScript** (The foundation of Closures and the `this` keyword).(./Scope.md)