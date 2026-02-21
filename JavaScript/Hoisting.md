# Hoisting in JavaScript

Hoisting is a behavior in JavaScript where variable and function declarations are conceptually moved to the top of their respective scopes during the compilation phase, before the code is executed. 

**Wait, does code actually move?**
No. In reality, your code isn't physically moved anywhere. Instead, hoisting is a strict consequence of how the JavaScript engine creates the **Execution Context**.

---

## 🧠 The "Why": Execution Context

To truly understand hoisting, you must understand that the JavaScript engine executes code in two distinct phases within an Execution Context:

1. **Memory Creation Phase**: The engine scans your entire code block and allocates memory for variables and functions *before* executing a single line of code.
   - **Variables** declared with `var` are stored and initialized with a placeholder value: `undefined`.
   - **Variables** declared with `let` and `const` are stored, but remain *uninitialized*.
   - **Functions** (specifically function declarations) are stored entirely in memory with their complete code.
2. **Code Execution Phase**: The engine goes through the code line by line, assigns actual values to variables, and executes functions.

---

## 1. Variable Hoisting

### The `var` Keyword
When a variable is declared using `var`, it is hoisted to the top of its scope and automatically initialized with `undefined`.

```javascript
console.log(x); // Output: undefined
var x = 10;
console.log(x); // Output: 10
```

**Internal Behavior (How the engine sees it):**
```javascript
// Memory Creation Phase
var x; // Allocated memory and initialized with undefined

// Execution Phase
console.log(x); // prints undefined
x = 10;         // actual value is assigned here
console.log(x); // prints 10
```

### `let` and `const`: The Temporal Dead Zone (TDZ)
A common misconception is that `let` and `const` are not hoisted. **They are hoisted!** However, unlike `var`, they are not initialized with `undefined`. Accessing them before their initialization during runtime results in a `ReferenceError`.

The period between entering the scope and hitting the actual declaration is known as the **Temporal Dead Zone (TDZ)**.

```javascript
// TDZ for 'y' starts here at the beginning of the scope
console.log(y); // ❌ ReferenceError: Cannot access 'y' before initialization
let y = 20;     // TDZ ends here
```

---

## 2. Function Hoisting

### Function Declarations
Function declarations undergo **complete hoisting**. The JavaScript engine stores the entire function in memory during the memory creation phase. This allows you to call a function safely before it is even defined in your code text.

```javascript
greet(); // Output: "Namaste Vikash"

function greet() {
    console.log("Namaste Vikash");
}
```

### Function Expressions and Arrow Functions
It is crucial to note that **only function declarations are fully hoisted**. If you use a function expression or an arrow function assigned to a variable, it is treated strictly according to the rules of variable hoisting.

```javascript
sayHi(); // ❌ TypeError: sayHi is not a function

var sayHi = function() {
    console.log("Hi!");
};
```
*Why? Because `sayHi` is hoisted as a `var` variable and initialized to `undefined`. You are essentially trying to execute `undefined()`, which throws a TypeError.*

---

## ⚔️ Hoisting Priorities: Function vs Variable

When a variable and a function share the very same name, who wins?
During the memory creation phase, **functions take priority**. However, during the execution phase, a variable assignment will overwrite the function.

```javascript
console.log(test); // Output: [Function: test]

var test = 10;

function test() {
    console.log("I am a function");
}

console.log(test); // Output: 10
```

**What happened here?**
1. **Memory Phase**: `test` is registered as a function. Then the engine sees `var test`, but since `test` already exists in memory as a function, it ignores the `var` declaration.
2. **Execution Phase**: The first `console.log(test)` points to the function. Then, `test = 10` is executed, overwriting the function with a number. The last `console.log(test)` points to `10`.

---

## 📊 Summary Table (Very Important for Interviews)

| Keyword | Is it Hoisted? | Initial Value During Memory Phase | Accessible Before Declaration? |
|---------|:---:|:---:|:---:|
| `var` | ✅ Yes | `undefined` | ✅ Yes (as `undefined`) |
| `let` | ✅ Yes | *Uninitialized* | ❌ No (ReferenceError - TDZ) |
| `const` | ✅ Yes | *Uninitialized* | ❌ No (ReferenceError - TDZ) |
| `function` | ✅ Yes | The complete function code | ✅ Yes |

---

## 🎯 Important Interview Questions

### Q1: What is the Temporal Dead Zone (TDZ)?
**Answer:** The Temporal Dead Zone is the time period in which a variable declared with `let` or `const` cannot be accessed. It starts when the execution enters the block scope and ends when the variable is actually initialized.

### Q2: Predict the output of this trick question:
```javascript
var x = 10;

function test() {
    console.log(x);
    var x = 20;
}

test();
```
**Output:** `undefined`

**Explanation:** Inside the `test` function, `var x` is hoisted to the top of the *function's scope*, creating a local variable that **shadows** the global variable `x`. It is initialized with `undefined`. Therefore, `console.log(x)` prints `undefined` before `x` is assigned the value `20`.

### Q3: Predict the output of this code block:
```javascript
function login() {
    if (true) {
        var user = "Vikash";
    }
    console.log(user);
}

login();
```
**Output:** `Vikash`

**Explanation:** The `var` keyword is **function-scoped**, not block-scoped. Even though `user` is declared inside the `if` block, it is hoisted to the top of the `login()` function. Therefore, it is easily accessible outside the `if` block.

---

**⏩ Next Concept (VERY IMPORTANT):** We study **Scope in JavaScript** (The foundation of Closures and the `this` keyword).