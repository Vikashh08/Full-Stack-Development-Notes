# Hoisting in JavaScript

Hoisting is one of the most frequently asked topics in JavaScript interviews. It may seem confusing at first, but once you understand how JavaScript executes code internally using the **Execution Context**, it becomes logical and predictable.

## Simple Explanation (The "What")

Imagine you are packing for a trip. Before putting anything inside your suitcase, you first make a list of all the items you need (shirts, shoes, toothbrush).

JavaScript behaves in a similar way.

Before executing your code, JavaScript scans the entire file and allocates memory for variables and functions. This preparation step happens before the actual execution of code. This behavior is called **Hoisting**.

> **Important:** Hoisting does NOT physically move code. Instead, JavaScript allocates memory for variables and functions during the Memory Creation Phase of the Execution Context.

---

## The Core Concept (The "Why") — Execution Context

In interviews, saying "declarations are moved to the top" is a basic explanation. The correct placement-level explanation involves the Execution Context.

Whenever JavaScript runs, it creates an Execution Context, which executes in two phases:

### Phase 1: Memory Creation Phase
JavaScript scans the code before execution and allocates memory for:

#### Variables declared with `var`
- Memory is allocated
- Initialized with special value: `undefined`

**Example internal memory state:**
```
studentName → undefined
```

#### Variables declared with `let` and `const`
- Memory is allocated
- NOT initialized
- Stored in a restricted state called the **Temporal Dead Zone (TDZ)**

**Example internal state:**
```
age → uninitialized (TDZ)
```

#### Function Declarations
The entire function definition is stored in memory.

**Example:**
```javascript
function greet() {
    console.log("Hello");
}
```

**Memory state:**
```
greet → complete function definition
```

### Phase 2: Code Execution Phase
JavaScript executes code line by line and assigns actual values to variables.

**Example:**
```javascript
var studentName = "Vikash";
```

**Execution phase:**
```
studentName → "Vikash"
```

---

## 1. Variable Hoisting

### `var` Hoisting (Initialized with `undefined`)
Because `var` is initialized with `undefined` during the memory phase, accessing it before declaration does NOT throw an error.

**Example:**
```javascript
console.log(studentName); // undefined

var studentName = "Vikash";

console.log(studentName); // Vikash
```

**Internal behavior:**
```javascript
var studentName; // hoisted and initialized with undefined

console.log(studentName);

studentName = "Vikash";
```

### `let` and `const` Hoisting (Temporal Dead Zone)
`let` and `const` are hoisted, but unlike `var`, they are NOT initialized.

Accessing them before declaration results in a `ReferenceError`.

**Example:**
```javascript
console.log(age); // ReferenceError

let age = 25;
```
This happens because the variable is inside the **Temporal Dead Zone (TDZ)**.

### Temporal Dead Zone (TDZ) Definition
The Temporal Dead Zone is the time between memory allocation and variable initialization, where accessing the variable results in a `ReferenceError`.

**Example:**
```javascript
{
    console.log(x); // ReferenceError
    let x = 10;
}
```

> **Interview Tip:** TDZ helps prevent accidental usage of variables before initialization, making code safer and more predictable.

---

## 2. Function Hoisting

### Function Declarations (Fully Hoisted)
Function declarations are completely hoisted.

You can call them before their declaration.

**Example:**
```javascript
sayHello();

function sayHello() {
    console.log("Hello string!");
}
```

**Output:**
```
Hello string!
```

**Memory phase:**
```
sayHello → complete function definition
```

### Function Expressions and Arrow Functions (Behave Like Variables)
Function expressions follow variable hoisting rules.

**Example:**
```javascript
greet();

var greet = function() {
    console.log("Hi there!");
};
```

**Output:**
```
TypeError: greet is not a function
```

**Explanation:**

**Memory phase:**
```
greet → undefined
```

**Execution phase:**
```
greet → function assigned
```
Calling `undefined()` causes the `TypeError`.

**Arrow function example:**
```javascript
test();

const test = () => {
    console.log("Hello");
};
```

**Output:**
```
ReferenceError
```
Because `const` remains in Temporal Dead Zone.

---

## Function vs Variable Priority

When a function and variable share the same name:
- **Memory Phase** → Function declaration takes priority
- **Execution Phase** → Variable assignment overrides function

**Example:**
```javascript
console.log(magic);

var magic = 100;

function magic() {
    console.log("I am a function");
}

console.log(magic);
```

**Output:**
```
function magic() { console.log("I am a function"); }
100
```

**Explanation:**

**Memory phase:**
```
magic → function definition
var magic → ignored (already exists)
```

**Execution phase:**
```
magic → 100
```

---

## Interview Questions on Hoisting

### Q1: What is Hoisting?
**Basic Answer:**
Hoisting is JavaScript's behavior of allocating memory for variables and functions before code execution.

**Placement-Level Answer:**
Hoisting occurs during the Memory Creation Phase of the Execution Context, where JavaScript allocates memory for variables and functions before executing code line by line.

### Q2: Are `let` and `const` hoisted?
Yes. They are hoisted but NOT initialized. They remain in the Temporal Dead Zone until initialization.

### Q3: What is Temporal Dead Zone?
The Temporal Dead Zone is the period between memory allocation and initialization of variables declared with `let` and `const`.

Accessing variables in TDZ causes `ReferenceError`.

### Q4: Predict the Output
```javascript
var x = 20;

function test() {
    console.log(x);
    var x = 40;
}

test();
```

**Answer:**
```
undefined
```

**Explanation:**
Local variable is hoisted and shadows global variable.

### Q5: Predict the Output
```javascript
foo();

var foo = function() {
    console.log("Hello!");
};
```

**Answer:**
```
TypeError: foo is not a function
```

### Q6: Predict the Output
```javascript
function login() {

    if(true) {
        var user = "Vikash";
        let role = "Admin";
    }

    console.log(user);
    console.log(role);
}

login();
```

**Answer:**
```
Vikash
ReferenceError: role is not defined
```

**Explanation:**
- `var` → function scoped
- `let` → block scoped

---

## Quick Summary Table (Memorize This)

| Keyword | Hoisted | Initialized in Memory Phase | Scope | Accessible Before Declaration |
| :--- | :--- | :--- | :--- | :--- |
| `var` | Yes | `undefined` | Function/Global | Yes |
| `let` | Yes | No (TDZ) | Block | No |
| `const` | Yes | No (TDZ) | Block | No |
| `function` declaration | Yes | Complete function | Function/Global | Yes |
| `function` expression | Yes | `undefined` | Depends | No |

---

## Final Placement-Level Definition
**Hoisting** is the behavior in JavaScript where variable and function declarations are allocated memory during the Memory Creation Phase of the Execution Context before code execution. Variables declared with `var` are initialized with `undefined`, while `let` and `const` remain in the Temporal Dead Zone until initialized. Function declarations are fully hoisted.

**Next Topic:** [Scope](./Scope.md)