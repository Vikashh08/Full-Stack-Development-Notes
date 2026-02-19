# Execution Context in JavaScript

**Execution Context** is the environment in which JavaScript code is executed. It contains everything the code needs to run, including:

- **Variables**
- **Functions**
- **Scope**
- **`this` keyword**

In simple terms:  
 **Execution Context is like a container where your code runs.**

---

##  Real-Life Example
**Imagine you are writing an exam in a classroom.**

The **Classroom** provides:
- Desk
- Question paper
- Answer sheet
- Environment to write

Similarly, **Execution Context** provides:
- Memory for variables
- Access to functions
- Environment to execute code

---

## Types of Execution Context
There are **3 types** of Execution Context:

1. **Global Execution Context (GEC)**
2. **Function Execution Context (FEC)**
3. **Eval Execution Context** (rarely used)

>  **For placements, focus on the first two.**

### 1.Global Execution Context (GEC)
- This is created when the JavaScript program starts.
- It is the **default** execution context.

**Example:**
```javascript
let x = 10;

function greet() {
    console.log("Hello");
}

console.log(x);
greet();
```
*When this code runs, the **Global Execution Context** is created.*

#### What Global Execution Context Contains
It has two main components:
1. **Memory Component** (Variable Environment)
2. **Code Component** (Thread of Execution)

---

##  How Execution Context Works (Two Phases)
**This is VERY IMPORTANT.**

JavaScript runs in **two phases**:

1. **Memory Creation Phase**
2. **Execution Phase**

### Phase 1: Memory Creation Phase
JavaScript scans the code and **allocates memory**.
- **Variables** → assigned `undefined`
- **Functions** → stored completely

**Example:**
```javascript
var x = 10;

function greet() {
    console.log("Hello");
}
```

**Memory creation phase:**
```javascript
x → undefined
greet → function definition
```

### Phase 2: Execution Phase
Now JavaScript executes code **line by line**.

```scss
x → 10
greet() → executed
```

---

## Example: Step-by-Step Execution

```javascript
var x = 10;

function test() {
    var y = 20;
    console.log(y);
}

console.log(x);
test();
```

### Step 1: Global Execution Context Created

**Memory Phase:**
```bash
x → undefined
test → function definition
```

**Execution Phase:**
```scss
x → 10
console.log(x) → 10
test() called
```

### Step 2: Function Execution Context Created
When the function is called (`test()`), a **New Execution Context** is created for that function.

**Memory Phase:**
```css
y → undefined
```

**Execution Phase:**
```css
y → 20
console.log(y) → 20
```

*Function execution finishes → context removed.*

---

## Execution Context Stack (Call Stack)
Execution contexts are managed using the **Call Stack**.

**Example:**
```javascript
function one() {
    two();
}

function two() {
    console.log("Hello");
}

one();
```

**Call stack flow:**
```scss
Global Execution Context
one()
two()
```

**After execution:**
```scss
two() removed
one() removed
Global remains
```

### Visual Representation

**Call Stack:**
```text
| two() |
| one() |
| GEC   |
```

**After completion:**
```text
| GEC |
```

---

##  What Execution Context Contains Internally (Important)
Execution Context has **3 components**:

1. **Variable Environment**
2. **Scope Chain**
3. **`this` keyword**

*(We will study each later in detail.)*

---

##  Hoisting Example
**Shows Memory and Execution clearly.**

```javascript
console.log(x);
var x = 5;
```

**Memory Phase:**
```css
x → undefined
```

**Execution Phase:**
```css
console.log(x) → undefined
x → 5
```
*(This is called **hoisting**, which we will study later.)*

---

## Real Industry Example
When you click a login button:

```javascript
function login(user) {
    let status = "success";
    return status;
}

login("Vikash");
```

**Execution Context created for `login` function:**

**Memory:**
```javascript
user → undefined
status → undefined
```

**Execution:**
```lua
user → Vikash
status → success
```

---

##  Interview Questions
**Very commonly asked questions:**

#### Q1: What is Execution Context?
**Answer:** Execution Context is the environment where JavaScript code runs, containing variables, functions, scope, and the `this` keyword.

#### Q2: How many phases are there?
**Answer:** Two phases:
1. Memory Creation Phase
2. Execution Phase

#### Q3: What happens in the memory phase?
**Answer:**
- Variables are assigned `undefined`
- Functions are stored completely

#### Q4: What happens when a function is called?
**Answer:** A **New Function Execution Context** is created.

---

##  Why Execution Context is the Most Important Concept
Because it explains:
- Hoisting
- Closures
- Scope
- `this` keyword
- Call stack
- Event loop

*Without understanding Execution Context, you cannot understand JavaScript deeply.*

> **Placement Tip (Very Important)**
>
> If an interviewer asks: **"How JavaScript executes code internally?"**
>
> **Answer using Execution Context and Call Stack.** This shows strong understanding.

---
**Next Concept:** Foundation of Execution Context