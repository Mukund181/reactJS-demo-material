# 🚀 The Complete JavaScript Mastery Guide
>
> **Who this is for**: You know the basics. This guide takes you from *"I know JS"* to *"I understand JS deeply."*

---

## Table of Contents

1. [How JavaScript Works](#1-how-javascript-works)
2. [Variables — var, let, const](#2-variables--var-let-const)
3. [Data Types & Type System](#3-data-types--type-system)
4. [Type Conversion & Coercion](#4-type-conversion--coercion)
5. [Operators & Equality](#5-operators--equality)
6. [Conditionals & Control Flow](#6-conditionals--control-flow)
7. [Loops & Iteration](#7-loops--iteration)
8. [Functions — Deep Dive](#8-functions--deep-dive)
9. [Scope & Scope Chain](#9-scope--scope-chain)
10. [Hoisting](#10-hoisting)
11. [Closures](#11-closures)
12. [The `this` Keyword](#12-the-this-keyword)
13. [call, apply, and bind](#13-call-apply-and-bind)
14. [Arrow Functions vs Regular Functions](#14-arrow-functions-vs-regular-functions)
15. [Higher-Order Functions](#15-higher-order-functions)
16. [Currying & Partial Application](#16-currying--partial-application)
17. [Objects — In Depth](#17-objects--in-depth)
18. [Prototypes & Prototypal Inheritance](#18-prototypes--prototypal-inheritance)
19. [ES6 Classes & OOP](#19-es6-classes--oop)
20. [Arrays — Deep Dive & Methods](#20-arrays--deep-dive--methods)
21. [Destructuring (Arrays & Objects)](#21-destructuring-arrays--objects)
22. [Spread & Rest Operators](#22-spread--rest-operators)
23. [Template Literals](#23-template-literals)
24. [Symbols](#24-symbols)
25. [Iterators & Generators](#25-iterators--generators)
26. [Map, Set, WeakMap, WeakSet](#26-map-set-weakmap-weakset)
27. [Shallow Copy vs Deep Copy](#27-shallow-copy-vs-deep-copy)
28. [Error Handling (try/catch/finally)](#28-error-handling-trycatchfinally)
29. [Asynchronous JavaScript — The Event Loop](#29-asynchronous-javascript--the-event-loop)
30. [Callbacks & Callback Hell](#30-callbacks--callback-hell)
31. [Promises](#31-promises)
32. [async / await](#32-async--await)
33. [Timers — setTimeout, setInterval, requestAnimationFrame](#33-timers--settimeout-setinterval-requestanimationframe)
34. [Debouncing & Throttling](#34-debouncing--throttling)
35. [The DOM — Document Object Model](#35-the-dom--document-object-model)
36. [Event Handling & Event Propagation](#36-event-handling--event-propagation)
37. [Event Delegation](#37-event-delegation)
38. [Web APIs & Browser APIs](#38-web-apis--browser-apis)
39. [Storage — localStorage, sessionStorage, Cookies](#39-storage--localstorage-sessionstorage-cookies)
40. [Fetch API & AJAX](#40-fetch-api--ajax)
41. [Modules (ES Modules & CommonJS)](#41-modules-es-modules--commonjs)
42. [Polyfills — Writing Your Own](#42-polyfills--writing-your-own)
43. [Strict Mode](#43-strict-mode)
44. [Memory Management & Garbage Collection](#44-memory-management--garbage-collection)
45. [Design Patterns in JavaScript](#45-design-patterns-in-javascript)
46. [JavaScript Engine Internals (V8)](#46-javascript-engine-internals-v8)
47. [Output-Based Tricky Questions — Patterns & Gotchas](#47-output-based-tricky-questions--patterns--gotchas)
48. [Machine Coding Essentials](#48-machine-coding-essentials)

---

---

## 1. How JavaScript Works

Before writing a single line of code, you need to understand what happens *behind the scenes* when JavaScript runs.

### 1.1 JavaScript is Single-Threaded

JavaScript has **one call stack** and **one thread of execution**. It can only do one thing at a time. This is a fundamental architectural decision — everything else (event loop, async, callbacks) is a consequence of this.

### 1.2 The Execution Context

Every time JavaScript runs code, it creates an **Execution Context**. There are two types:

| Type | When Created |
|------|-------------|
| **Global Execution Context (GEC)** | When the script first loads. Only one exists. |
| **Function Execution Context (FEC)** | Each time a function is invoked. |

Each execution context goes through **two phases**:

#### Phase 1: Memory Creation (Creation Phase)
- JavaScript scans through the code (before executing anything).
- It allocates memory for variables and functions.
- Variables declared with `var` are stored with the value `undefined`.
- Variables declared with `let` and `const` are stored but left **uninitialized** (in the Temporal Dead Zone).
- Entire function declarations are stored **in full** (this is why function hoisting works).

#### Phase 2: Code Execution (Execution Phase)
- JavaScript goes through the code **line by line**.
- Variables are assigned their actual values.
- When a function call is encountered, a **new execution context** is created and pushed onto the Call Stack.

### 1.3 The Call Stack

The Call Stack is a **LIFO (Last In, First Out)** data structure that keeps track of execution contexts.

```js
function first() {
  console.log("first");
  second();
  console.log("first again");
}

function second() {
  console.log("second");
}

first();
```

**Call Stack progression:**
```
1. [Global]
2. [Global, first()]
3. [Global, first(), second()]    // second() pushed
4. [Global, first()]              // second() popped after completing
5. [Global]                       // first() popped
6. []                             // Global popped — program ends
```

### 1.4 Key Takeaway

JavaScript doesn't "jump around." It follows a strict, predictable path: create context → allocate memory → execute line by line → pop off the stack. Understanding this is the foundation for understanding hoisting, closures, scope, and async behavior.

---

## 2. Variables — var, let, const

### 2.1 The Three Declaration Keywords

JavaScript has three ways to declare variables, each with different behavior:

```js
var name = "Mukund";    // Function-scoped, hoisted with undefined
let age = 22;           // Block-scoped, hoisted but NOT initialized
const PI = 3.14;        // Block-scoped, hoisted but NOT initialized, cannot be reassigned
```

### 2.2 Scope Differences

This is the most important distinction:

```js
// var is FUNCTION-SCOPED
function example() {
  if (true) {
    var x = 10;
  }
  console.log(x); // 10 ✅ — var leaks out of the if block
}

// let and const are BLOCK-SCOPED
function example2() {
  if (true) {
    let y = 20;
    const z = 30;
  }
  console.log(y); // ❌ ReferenceError
  console.log(z); // ❌ ReferenceError
}
```

### 2.3 Hoisting Behavior

All three are hoisted, but differently:

```js
console.log(a); // undefined (var is hoisted and initialized to undefined)
console.log(b); // ❌ ReferenceError: Cannot access 'b' before initialization
console.log(c); // ❌ ReferenceError: Cannot access 'c' before initialization

var a = 1;
let b = 2;
const c = 3;
```

`let` and `const` exist in the **Temporal Dead Zone (TDZ)** — the period between the start of the block and the actual declaration line. The variable exists in memory but accessing it throws an error.

### 2.4 Reassignment & Redeclaration

```js
// var — can reassign AND redeclare
var x = 1;
var x = 2;  // ✅ No error
x = 3;      // ✅ No error

// let — can reassign, CANNOT redeclare
let y = 1;
// let y = 2;  // ❌ SyntaxError
y = 3;        // ✅ Fine

// const — CANNOT reassign, CANNOT redeclare
const z = 1;
// const z = 2;  // ❌ SyntaxError
// z = 3;        // ❌ TypeError: Assignment to constant variable
```

### 2.5 const with Objects and Arrays

`const` prevents **reassignment of the binding**, not mutation of the value:

```js
const user = { name: "Mukund" };
user.name = "John";     // ✅ This works — you're mutating the object
// user = { name: "X" }; // ❌ This fails — you're reassigning the variable

const arr = [1, 2, 3];
arr.push(4);            // ✅ Mutation works
// arr = [5, 6];         // ❌ Reassignment fails
```

### 2.6 When to Use What

| Keyword | Use When |
|---------|----------|
| `const` | **Default choice.** Use for values that won't be reassigned. |
| `let` | When you know the value will change (loop counters, conditionally assigned values). |
| `var` | **Never** in modern code. Only if you're maintaining legacy code. |

---

## 3. Data Types & Type System

### 3.1 Primitive Types (7 Types)

Primitives are **immutable** and stored **by value** (on the stack).

```js
// 1. String
let name = "JavaScript";

// 2. Number (integers and floats are the same type)
let age = 25;
let price = 99.99;
let special = Infinity;   // Also: -Infinity, NaN

// 3. BigInt (for numbers beyond Number.MAX_SAFE_INTEGER)
let bigNum = 9007199254740991n;

// 4. Boolean
let isActive = true;

// 5. undefined (variable declared but not assigned)
let x;
console.log(x); // undefined

// 6. null (intentional absence of value)
let data = null;

// 7. Symbol (unique identifiers — covered in detail later)
let id = Symbol("id");
```

### 3.2 Non-Primitive (Reference) Types

These are stored **by reference** (on the heap). The variable holds a pointer to the memory location.

```js
// Object
let user = { name: "Mukund", age: 22 };

// Array (technically an object)
let colors = ["red", "green", "blue"];

// Function (technically an object)
function greet() { return "Hello"; }

// Date, RegExp, Map, Set, etc. — all objects
```

### 3.3 typeof Operator

```js
typeof "hello"      // "string"
typeof 42           // "number"
typeof true         // "boolean"
typeof undefined    // "undefined"
typeof null         // "object"    ⚠️ This is a famous JS bug!
typeof Symbol("x")  // "symbol"
typeof 10n          // "bigint"
typeof {}           // "object"
typeof []           // "object"    ⚠️ Arrays are objects
typeof function(){} // "function"  (special case — it's still an object)
```

### 3.4 Checking Types Properly

```js
// For arrays
Array.isArray([1, 2, 3]);          // true
Array.isArray({ length: 3 });      // false

// For null
let val = null;
val === null;                       // true (use strict equality)

// For NaN
Number.isNaN(NaN);                  // true
Number.isNaN("hello");             // false (unlike global isNaN)

// For objects (excluding null and arrays)
function isPlainObject(val) {
  return val !== null && typeof val === "object" && !Array.isArray(val);
}
```

### 3.5 Pass by Value vs Pass by Reference

```js
// Primitives — pass by value (copies are independent)
let a = 10;
let b = a;
b = 20;
console.log(a); // 10 (unchanged)

// Objects — pass by reference (both point to same memory)
let obj1 = { x: 1 };
let obj2 = obj1;
obj2.x = 99;
console.log(obj1.x); // 99 (changed! Both point to same object)
```

This distinction is critical — it's the reason shallow/deep copy exists, and why bugs occur when you think you're working with a copy but you're actually mutating the original.

---

## 4. Type Conversion & Coercion

### 4.1 Explicit Type Conversion (You do it)

```js
// To String
String(123);         // "123"
(123).toString();    // "123"
String(true);        // "true"
String(null);        // "null"
String(undefined);   // "undefined"

// To Number
Number("42");        // 42
Number("");          // 0
Number("hello");     // NaN
Number(true);        // 1
Number(false);       // 0
Number(null);        // 0
Number(undefined);   // NaN
parseInt("42px");    // 42 (stops at first non-numeric char)
parseFloat("3.14"); // 3.14

// To Boolean
Boolean(0);          // false
Boolean("");         // false
Boolean(null);       // false
Boolean(undefined);  // false
Boolean(NaN);        // false
// Everything else is truthy:
Boolean("0");        // true (non-empty string!)
Boolean([]);         // true (empty array is truthy!)
Boolean({});         // true (empty object is truthy!)
```

### 4.2 Implicit Type Coercion (JS does it for you)

This is where JavaScript gets "weird" — but it follows rules:

```js
// String coercion (+ with a string converts the other operand to string)
"5" + 3        // "53"  (number → string)
"5" + true     // "5true"
"5" + null     // "5null"

// Numeric coercion (-, *, /, % always try to convert to number)
"5" - 3        // 2
"5" * "2"      // 10
true + true    // 2
false + 1      // 1

// Boolean coercion (in conditions)
if ("hello") { /* runs — truthy */ }
if (0) { /* doesn't run — falsy */ }
```

### 4.3 Falsy Values — Memorize These

There are exactly **8 falsy values** in JavaScript:

```js
false
0
-0
0n        // BigInt zero
""        // empty string
null
undefined
NaN
```

**Everything else is truthy** — including `"0"`, `" "`, `[]`, `{}`, and `function(){}`.

### 4.4 Coercion Gotchas

```js
[] + []          // "" (both convert to empty string)
[] + {}          // "[object Object]"
{} + []          // 0 (in some engines — {} is treated as empty block)
"" == false      // true (both coerce to 0)
null == undefined // true (special rule)
null === undefined // false
NaN == NaN       // false (NaN is not equal to anything, including itself)
```

---

## 5. Operators & Equality

### 5.1 `==` (Loose Equality) vs `===` (Strict Equality)

This is one of the most asked topics and one of the most misunderstood:

```js
// === (Strict Equality) — NO type conversion
5 === 5        // true
5 === "5"      // false (different types)
null === undefined // false

// == (Loose Equality) — performs type coercion
5 == "5"       // true ("5" converted to 5)
0 == false     // true (false converted to 0)
"" == false    // true (both coerce to 0)
null == undefined // true (special rule in the spec)
null == 0      // false (null only equals undefined and itself)
```

**Rule of thumb**: Always use `===` unless you have a specific reason to use `==`.

### 5.2 Abstract Equality Algorithm (How == Works)

When types differ, `==` follows these coercion rules:
1. `null == undefined` → `true` (and nothing else equals null/undefined)
2. Number vs String → convert String to Number
3. Boolean vs anything → convert Boolean to Number first, then compare
4. Object vs Primitive → call `valueOf()` or `toString()` on the object

### 5.3 Logical Operators (Short-Circuit Evaluation)

These don't always return `true` or `false` — they return one of the operand values:

```js
// || (OR) — returns the FIRST truthy value (or the last value if all falsy)
"hello" || "world"    // "hello"
0 || "fallback"       // "fallback"
"" || 0 || null       // null (all falsy, returns last)

// && (AND) — returns the FIRST falsy value (or the last value if all truthy)
"hello" && "world"    // "world"
0 && "hello"          // 0
"a" && "b" && "c"     // "c"

// ?? (Nullish Coalescing) — returns right side ONLY if left is null or undefined
0 ?? "fallback"       // 0 (0 is not null/undefined!)
null ?? "fallback"    // "fallback"
undefined ?? "fallback" // "fallback"
"" ?? "fallback"      // "" (empty string is not null/undefined)
```

### 5.4 When to Use || vs ??

```js
// Problem with ||: it treats 0, "", and false as "no value"
let count = 0;
let result1 = count || 10;  // 10 ❌ (you wanted 0!)
let result2 = count ?? 10;  // 0  ✅ (?? only checks null/undefined)
```

### 5.5 Optional Chaining (`?.`)

Safely access nested properties without checking each level:

```js
let user = { address: { city: "Pune" } };
console.log(user?.address?.city);     // "Pune"
console.log(user?.phone?.number);     // undefined (no error!)
console.log(user?.getProfile?.());    // undefined (safe method call)
```

---

## 6. Conditionals & Control Flow

### 6.1 if / else if / else

```js
let score = 85;

if (score >= 90) {
  console.log("A grade");
} else if (score >= 80) {
  console.log("B grade");
} else if (score >= 70) {
  console.log("C grade");
} else {
  console.log("Needs improvement");
}
```

### 6.2 Ternary Operator

```js
let age = 20;
let status = age >= 18 ? "Adult" : "Minor";

// Nested ternary (use sparingly — reduces readability)
let grade = score >= 90 ? "A" : score >= 80 ? "B" : "C";
```

### 6.3 switch Statement

```js
let day = "Monday";

switch (day) {
  case "Monday":
  case "Tuesday":
  case "Wednesday":
  case "Thursday":
  case "Friday":
    console.log("Weekday");
    break;
  case "Saturday":
  case "Sunday":
    console.log("Weekend");
    break;
  default:
    console.log("Invalid day");
}
```

> **Important**: `switch` uses **strict comparison** (`===`). If you pass `"1"` and your case is `1`, it won't match.

---

## 7. Loops & Iteration

### 7.1 for Loop

```js
for (let i = 0; i < 5; i++) {
  console.log(i); // 0, 1, 2, 3, 4
}
```

### 7.2 while and do...while

```js
// while — checks condition BEFORE executing
let i = 0;
while (i < 3) {
  console.log(i); // 0, 1, 2
  i++;
}

// do...while — executes AT LEAST ONCE
let j = 10;
do {
  console.log(j); // 10 (runs once even though condition is false)
  j++;
} while (j < 5);
```

### 7.3 for...in (for Objects)

Iterates over **enumerable property keys** (including inherited ones):

```js
let user = { name: "Mukund", age: 22, city: "Pune" };

for (let key in user) {
  console.log(`${key}: ${user[key]}`);
}
// name: Mukund
// age: 22
// city: Pune
```

> **Warning**: `for...in` also iterates over inherited properties from the prototype chain. Use `hasOwnProperty()` to filter:
```js
for (let key in user) {
  if (user.hasOwnProperty(key)) {
    console.log(key);
  }
}
```

### 7.4 for...of (for Iterables)

Iterates over **values** of iterable objects (arrays, strings, Maps, Sets — NOT plain objects):

```js
let colors = ["red", "green", "blue"];
for (let color of colors) {
  console.log(color); // red, green, blue
}

let name = "Hello";
for (let char of name) {
  console.log(char); // H, e, l, l, o
}
```

### 7.5 for...in vs for...of Summary

| Feature | `for...in` | `for...of` |
|---------|-----------|-----------|
| Iterates | Keys/property names | Values |
| Works on | Objects, arrays | Iterables (arrays, strings, Maps, Sets) |
| Prototype chain | Yes (includes inherited) | No |
| Use case | Object properties | Array values, strings |

---

## 8. Functions — Deep Dive

### 8.1 Function Declaration vs Function Expression

```js
// Function Declaration — hoisted entirely
greet(); // ✅ Works! (function is hoisted)
function greet() {
  console.log("Hello!");
}

// Function Expression — NOT hoisted (only the variable is hoisted)
// sayHi(); // ❌ TypeError: sayHi is not a function
const sayHi = function() {
  console.log("Hi!");
};
sayHi(); // ✅ Works after this line
```

### 8.2 Named vs Anonymous Function Expressions

```js
// Anonymous
const add = function(a, b) { return a + b; };

// Named function expression — the name is only visible INSIDE the function
const factorial = function fact(n) {
  return n <= 1 ? 1 : n * fact(n - 1); // 'fact' is accessible here
};
// fact(5); // ❌ ReferenceError — 'fact' is not accessible outside
factorial(5); // ✅ 120
```

Named function expressions are useful for recursion and for better stack traces when debugging.

### 8.3 Parameters & Arguments

```js
// Default parameters
function greet(name = "World") {
  console.log(`Hello, ${name}!`);
}
greet();        // Hello, World!
greet("Mukund"); // Hello, Mukund!

// Rest parameters (...args collects remaining arguments into an array)
function sum(...numbers) {
  return numbers.reduce((total, n) => total + n, 0);
}
sum(1, 2, 3, 4); // 10

// The 'arguments' object (old way — NOT available in arrow functions)
function oldSum() {
  console.log(arguments);       // [1, 2, 3] (array-like, NOT a real array)
  return [...arguments].reduce((a, b) => a + b, 0);
}
```

### 8.4 IIFE — Immediately Invoked Function Expression

A function that runs **the moment it's defined**:

```js
(function() {
  let secret = "hidden";
  console.log("IIFE executed!");
  // 'secret' is not accessible outside
})();

// With arrow function
(() => {
  console.log("Arrow IIFE!");
})();

// Named IIFE (useful for recursion)
(function init() {
  console.log("App initialized");
})();
```

**Why use IIFE?** To create a private scope — avoiding global namespace pollution. This was essential before ES6 modules existed.

### 8.5 Pure Functions

A function is **pure** if:
1. Given the same inputs, it always returns the same output.
2. It has no side effects (doesn't modify external state).

```js
// Pure ✅
function add(a, b) {
  return a + b;
}

// Impure ❌ (depends on external variable)
let tax = 0.18;
function calculateTotal(price) {
  return price + (price * tax); // depends on 'tax' variable
}

// Impure ❌ (has side effect — modifies external array)
let items = [];
function addItem(item) {
  items.push(item); // modifies external state
}
```

### 8.6 First-Class Functions

In JavaScript, functions are **first-class citizens** — they can be:
- Assigned to variables
- Passed as arguments
- Returned from other functions
- Stored in data structures

```js
// Stored in a variable
const greet = function() { return "Hi"; };

// Passed as an argument
function execute(fn) {
  return fn();
}
execute(greet); // "Hi"

// Returned from a function
function multiplier(factor) {
  return function(number) {
    return number * factor;
  };
}
const double = multiplier(2);
double(5); // 10
```

This property is what makes higher-order functions, closures, and callbacks possible.

---

## 9. Scope & Scope Chain

### 9.1 What is Scope?

Scope determines **where variables are accessible** in your code. Think of it as the "visibility" of variables.

### 9.2 Three Types of Scope

```js
// 1. GLOBAL SCOPE — accessible everywhere
var globalVar = "I'm global";
let globalLet = "I'm also global";

// 2. FUNCTION SCOPE — accessible only inside the function
function myFunc() {
  var functionScoped = "Only here";
  let alsoFunctionScoped = "Also only here";
  console.log(globalVar); // ✅ Can access global
}
// console.log(functionScoped); // ❌ ReferenceError

// 3. BLOCK SCOPE — inside { } — only for let and const
{
  let blockScoped = "Only in this block";
  const alsoBlock = "Same";
  var notBlockScoped = "I escape!"; // var ignores block scope
}
// console.log(blockScoped); // ❌ ReferenceError
console.log(notBlockScoped); // ✅ "I escape!"
```

### 9.3 The Scope Chain

When JavaScript encounters a variable, it looks for it in the current scope. If not found, it goes to the **outer scope**, then the **outer-outer scope**, all the way to the global scope. This chain of lookups is the **scope chain**.

```js
let a = "global";

function outer() {
  let b = "outer";
  
  function inner() {
    let c = "inner";
    console.log(c); // ✅ Found in current scope
    console.log(b); // ✅ Found in outer scope (one level up)
    console.log(a); // ✅ Found in global scope (two levels up)
  }
  
  inner();
  // console.log(c); // ❌ inner scope is not accessible from here
}
```

### 9.4 Lexical Scope (Static Scope)

JavaScript uses **lexical scoping** — the scope is determined by **where the function is written** in the source code, NOT where it is called.

```js
let x = 10;

function foo() {
  console.log(x); // Always looks at where foo was DEFINED
}

function bar() {
  let x = 20;
  foo(); // Still prints 10, not 20!
}

bar(); // 10
```

This is crucial for understanding closures.

---

## 10. Hoisting

### 10.1 What is Hoisting?

Hoisting is JavaScript's behavior of moving **declarations** (not assignments) to the top of their scope during the **creation phase** of the execution context.

It's not that code literally moves — it's that memory is allocated for variables and functions **before any code runs**.

### 10.2 How Different Declarations are Hoisted

```js
// ====== var ======
console.log(a); // undefined (hoisted and initialized to undefined)
var a = 5;
console.log(a); // 5

// What JS actually does:
// var a = undefined;  ← creation phase
// console.log(a);     ← undefined
// a = 5;              ← execution phase
// console.log(a);     ← 5


// ====== let & const ======
// console.log(b); // ❌ ReferenceError: Cannot access 'b' before initialization
let b = 10;

// They ARE hoisted (memory is allocated) but NOT initialized.
// The period between hoisting and declaration is the Temporal Dead Zone (TDZ).


// ====== Function Declarations ======
sayHello(); // ✅ "Hello!" — entire function is hoisted
function sayHello() {
  console.log("Hello!");
}


// ====== Function Expressions ======
// sayBye(); // ❌ TypeError: sayBye is not a function (var) or ReferenceError (let/const)
var sayBye = function() {
  console.log("Bye!");
};
```

### 10.3 The Temporal Dead Zone (TDZ)

The TDZ is the zone between the start of the scope and the line where the variable is declared. Accessing the variable in this zone throws a `ReferenceError`.

```js
{
  // TDZ for 'x' starts here ──────┐
  //                                │
  // console.log(x); // ❌ ReferenceError    │  TDZ
  //                                │
  let x = 42;       // TDZ ends ──┘
  console.log(x);   // ✅ 42
}
```

### 10.4 Hoisting Priority

When both a `var` variable and a function declaration have the same name:

```js
console.log(foo); // [Function: foo] — function declaration wins

var foo = "hello";
function foo() {
  return "world";
}

console.log(foo); // "hello" — var assignment overwrites in execution phase
```

Function declarations are hoisted **above** variable declarations. But variable **assignments** happen during execution and can overwrite.

---

## 11. Closures

### 11.1 What is a Closure?

A closure is formed when a function **remembers and accesses variables from its outer (lexical) scope**, even after the outer function has finished executing.

```js
function createCounter() {
  let count = 0;  // This variable is "closed over"
  
  return function() {
    count++;
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
// 'count' is not accessible directly, but the returned function still has access to it
```

### 11.2 Why Do Closures Exist?

Because JavaScript has:
1. **Lexical scoping** — functions remember where they were created
2. **First-class functions** — functions can be returned and passed around

When a function is returned from another function, it carries a **reference to the variables** in its birth scope. Those variables are kept alive (not garbage collected) as long as the closure exists.

### 11.3 Closures with Loops — The Classic Gotcha

```js
// ❌ Bug: Using var
for (var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
// Output: 3, 3, 3 (NOT 0, 1, 2!)
// Why? 'var' is function-scoped. All callbacks share the SAME 'i'.
// By the time setTimeout fires, the loop has finished and i === 3.

// ✅ Fix 1: Use let (block-scoped — each iteration gets its own 'i')
for (let i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
// Output: 0, 1, 2

// ✅ Fix 2: Use IIFE to create a new scope per iteration
for (var i = 0; i < 3; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(j);
    }, 1000);
  })(i);
}
// Output: 0, 1, 2
```

### 11.4 Practical Uses of Closures

**Data Privacy / Encapsulation:**
```js
function createBankAccount(initialBalance) {
  let balance = initialBalance; // Private variable
  
  return {
    deposit(amount) {
      balance += amount;
      return balance;
    },
    withdraw(amount) {
      if (amount > balance) throw new Error("Insufficient funds");
      balance -= amount;
      return balance;
    },
    getBalance() {
      return balance;
    }
  };
}

const account = createBankAccount(1000);
account.deposit(500);     // 1500
account.withdraw(200);    // 1300
account.getBalance();     // 1300
// account.balance;        // undefined — can't access directly!
```

**Function Factories:**
```js
function createMultiplier(multiplier) {
  return function(number) {
    return number * multiplier;
  };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

double(5); // 10
triple(5); // 15
```

**Memoization:**
```js
function memoize(fn) {
  const cache = {};
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache[key] !== undefined) {
      console.log("From cache");
      return cache[key];
    }
    const result = fn(...args);
    cache[key] = result;
    return result;
  };
}

const expensiveAdd = memoize((a, b) => {
  console.log("Computing...");
  return a + b;
});

expensiveAdd(1, 2); // "Computing..." → 3
expensiveAdd(1, 2); // "From cache" → 3
```

---

## 12. The `this` Keyword

### 12.1 What is `this`?

`this` is a special keyword that refers to the **context in which a function is called**. Its value is determined at **call time**, not at definition time (except for arrow functions).

### 12.2 Rules for `this` (in order of precedence)

#### Rule 1: `new` Binding
```js
function Person(name) {
  this.name = name;
}
const p = new Person("Mukund");
console.log(p.name); // "Mukund" — this = the newly created object
```

#### Rule 2: Explicit Binding (call, apply, bind)
```js
function greet() {
  console.log(this.name);
}
const user = { name: "Mukund" };
greet.call(user);  // "Mukund" — this = user
```

#### Rule 3: Implicit Binding (Method Call)
```js
const user = {
  name: "Mukund",
  greet() {
    console.log(this.name);
  }
};
user.greet(); // "Mukund" — this = the object before the dot
```

#### Rule 4: Default Binding
```js
function showThis() {
  console.log(this);
}
showThis(); // In browser: window | In strict mode: undefined
```

### 12.3 The Infamous `this` Loss Problem

```js
const user = {
  name: "Mukund",
  greet() {
    console.log(this.name);
  }
};

user.greet(); // "Mukund" ✅

const greetFn = user.greet; // Extracting the method
greetFn(); // undefined ❌ (or error in strict mode)
// Why? The function lost its context — 'this' is no longer the user object
```

**Fixes:**
```js
// Fix 1: bind
const boundGreet = user.greet.bind(user);
boundGreet(); // "Mukund" ✅

// Fix 2: Arrow function wrapper
const wrappedGreet = () => user.greet();
wrappedGreet(); // "Mukund" ✅
```

### 12.4 `this` Inside Nested Functions

```js
const user = {
  name: "Mukund",
  greet() {
    console.log(this.name); // "Mukund" ✅ (implicit binding)
    
    function innerFunc() {
      console.log(this.name); // undefined ❌ (default binding!)
    }
    innerFunc();
    
    // Fix with arrow function:
    const arrowInner = () => {
      console.log(this.name); // "Mukund" ✅ (inherits from greet's this)
    };
    arrowInner();
  }
};
user.greet();
```

### 12.5 `this` in Event Handlers

```js
const button = document.querySelector("button");

button.addEventListener("click", function() {
  console.log(this); // <button> element — this = the element that triggered the event
});

button.addEventListener("click", () => {
  console.log(this); // window — arrow function inherits outer this!
});
```

---

## 13. call, apply, and bind

These three methods allow you to **explicitly set the value of `this`** when calling a function.

### 13.1 call()

Invokes the function **immediately** with a given `this` and arguments passed **individually**.

```js
function introduce(greeting, punctuation) {
  console.log(`${greeting}, I'm ${this.name}${punctuation}`);
}

const person = { name: "Mukund" };

introduce.call(person, "Hello", "!");
// "Hello, I'm Mukund!"
```

### 13.2 apply()

Same as `call()`, but arguments are passed as an **array**.

```js
introduce.apply(person, ["Hey", "."]);
// "Hey, I'm Mukund."

// Useful when you have an array of arguments
const args = ["Hi", "!!"];
introduce.apply(person, args);
```

### 13.3 bind()

Does NOT invoke the function. Instead, it returns a **new function** with `this` permanently set.

```js
const boundIntroduce = introduce.bind(person, "Howdy");
boundIntroduce("?");  // "Howdy, I'm Mukund?"
boundIntroduce("!!");  // "Howdy, I'm Mukund!!"

// bind is useful for event handlers and callbacks
const user = {
  name: "Mukund",
  greet() {
    console.log(`Hi, ${this.name}`);
  }
};

setTimeout(user.greet.bind(user), 1000); // "Hi, Mukund" after 1 second
```

### 13.4 Comparison Table

| Feature | `call` | `apply` | `bind` |
|---------|--------|---------|--------|
| Invokes immediately? | ✅ Yes | ✅ Yes | ❌ No (returns new function) |
| Arguments | Passed individually | Passed as array | Passed individually (partial application) |
| Use case | Invoke with context | Invoke with array args | Create reusable bound function |

### 13.5 Real-World: Method Borrowing

```js
const person1 = {
  name: "Mukund",
  introduce() {
    console.log(`I'm ${this.name}`);
  }
};

const person2 = { name: "Rahul" };

// Borrow person1's method for person2
person1.introduce.call(person2); // "I'm Rahul"
```

---

## 14. Arrow Functions vs Regular Functions

### 14.1 Syntax Differences

```js
// Regular function
function add(a, b) {
  return a + b;
}

// Arrow function
const add = (a, b) => a + b;

// Single parameter — parentheses optional
const square = x => x * x;

// No parameters — parentheses required
const getRandom = () => Math.random();

// Multi-line — curly braces + explicit return required
const greet = (name) => {
  const message = `Hello, ${name}`;
  return message;
};

// Returning an object literal — wrap in parentheses
const makeUser = (name) => ({ name: name, active: true });
```

### 14.2 Behavioral Differences

| Feature | Regular Function | Arrow Function |
|---------|-----------------|----------------|
| `this` binding | Dynamic (depends on how it's called) | Lexical (inherits from enclosing scope) |
| `arguments` object | ✅ Available | ❌ Not available |
| Can be used as constructor (`new`) | ✅ Yes | ❌ No (throws TypeError) |
| Has `prototype` property | ✅ Yes | ❌ No |
| Hoisting | Function declarations are hoisted | Not hoisted (expression) |
| `super` keyword | Dynamic | Lexical |
| Method in object literal | ✅ Recommended | ❌ Avoid (wrong `this`) |

### 14.3 The `this` Difference in Practice

```js
const timer = {
  seconds: 0,
  
  // ❌ Arrow function as method — BAD
  startBad: () => {
    // 'this' is window/undefined, NOT the timer object
    setInterval(() => {
      this.seconds++; // 'this' is wrong!
    }, 1000);
  },
  
  // ✅ Regular function as method, arrow in callback — GOOD
  startGood() {
    setInterval(() => {
      this.seconds++; // 'this' correctly refers to timer
      console.log(this.seconds);
    }, 1000);
  }
};
```

### 14.4 When to Use Which

- **Arrow functions**: Callbacks, array methods (`.map`, `.filter`), short utility functions, anywhere you need lexical `this`.
- **Regular functions**: Object methods, constructor functions, functions that need their own `this`, functions that use `arguments`.

---

## 15. Higher-Order Functions

### 15.1 What is a Higher-Order Function?

A function that either:
1. **Takes a function as an argument**, OR
2. **Returns a function**

```js
// Takes a function as argument
function repeat(n, action) {
  for (let i = 0; i < n; i++) {
    action(i);
  }
}
repeat(3, console.log); // 0, 1, 2

// Returns a function
function greaterThan(n) {
  return function(m) {
    return m > n;
  };
}
const greaterThan10 = greaterThan(10);
greaterThan10(15); // true
greaterThan10(5);  // false
```

### 15.2 Built-in Higher-Order Functions

These are the most commonly used and most frequently asked about:

#### `map()` — Transform each element
```js
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);
// [2, 4, 6, 8, 10]

// Always returns a NEW array of the same length
const users = [{ name: "A", age: 20 }, { name: "B", age: 25 }];
const names = users.map(user => user.name);
// ["A", "B"]
```

#### `filter()` — Keep elements that pass a test
```js
const numbers = [1, 2, 3, 4, 5, 6];
const evens = numbers.filter(n => n % 2 === 0);
// [2, 4, 6]

// Returns a NEW array (may be shorter than original)
const adults = users.filter(user => user.age >= 21);
```

#### `reduce()` — Accumulate into a single value
```js
const numbers = [1, 2, 3, 4, 5];

// Sum
const sum = numbers.reduce((accumulator, current) => accumulator + current, 0);
// 15

// Count occurrences
const fruits = ["apple", "banana", "apple", "cherry", "banana", "apple"];
const count = fruits.reduce((acc, fruit) => {
  acc[fruit] = (acc[fruit] || 0) + 1;
  return acc;
}, {});
// { apple: 3, banana: 2, cherry: 1 }

// Flatten nested arrays
const nested = [[1, 2], [3, 4], [5, 6]];
const flat = nested.reduce((acc, arr) => acc.concat(arr), []);
// [1, 2, 3, 4, 5, 6]
```

#### `forEach()` — Execute for each element (no return value)
```js
const names = ["Mukund", "Rahul", "Priya"];
names.forEach((name, index) => {
  console.log(`${index + 1}. ${name}`);
});
// 1. Mukund
// 2. Rahul
// 3. Priya
// Note: forEach returns undefined — you can't chain it
```

#### `find()` and `findIndex()`
```js
const users = [
  { id: 1, name: "Mukund" },
  { id: 2, name: "Rahul" },
  { id: 3, name: "Priya" }
];

const user = users.find(u => u.id === 2);
// { id: 2, name: "Rahul" } — returns FIRST match or undefined

const index = users.findIndex(u => u.id === 2);
// 1 — returns index of FIRST match or -1
```

#### `some()` and `every()`
```js
const numbers = [1, 2, 3, 4, 5];

numbers.some(n => n > 3);   // true (at least one passes)
numbers.every(n => n > 0);  // true (ALL pass)
numbers.every(n => n > 3);  // false (not all pass)
```

### 15.3 Chaining Array Methods

```js
const transactions = [
  { type: "credit", amount: 1000 },
  { type: "debit", amount: 500 },
  { type: "credit", amount: 2000 },
  { type: "debit", amount: 300 },
];

const totalCredits = transactions
  .filter(t => t.type === "credit")      // Keep only credits
  .map(t => t.amount)                     // Extract amounts
  .reduce((sum, amount) => sum + amount, 0); // Sum them up
// 3000
```

---

## 16. Currying & Partial Application

### 16.1 What is Currying?

Currying transforms a function with **multiple arguments** into a **sequence of functions**, each taking **one argument**.

```js
// Normal function
function add(a, b, c) {
  return a + b + c;
}
add(1, 2, 3); // 6

// Curried version
function curriedAdd(a) {
  return function(b) {
    return function(c) {
      return a + b + c;
    };
  };
}
curriedAdd(1)(2)(3); // 6

// Arrow function version (cleaner)
const curriedAdd2 = a => b => c => a + b + c;
curriedAdd2(1)(2)(3); // 6
```

### 16.2 Why Curry?

**Reusability through partial application:**

```js
const multiply = a => b => a * b;

const double = multiply(2);  // "lock in" the first argument
const triple = multiply(3);

double(5);  // 10
triple(5);  // 15

// Practical: Creating specialized loggers
const log = level => message => `[${level}] ${message}`;
const error = log("ERROR");
const info = log("INFO");

error("Something broke"); // "[ERROR] Something broke"
info("Server started");    // "[INFO] Server started"
```

### 16.3 Generic Curry Utility

A reusable function that converts any regular function to a curried one:

```js
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    } else {
      return function(...args2) {
        return curried.apply(this, args.concat(args2));
      };
    }
  };
}

// Usage:
function volume(l, w, h) {
  return l * w * h;
}

const curriedVolume = curry(volume);
curriedVolume(2)(3)(4);    // 24
curriedVolume(2, 3)(4);    // 24
curriedVolume(2)(3, 4);    // 24
curriedVolume(2, 3, 4);    // 24
```

### 16.4 Partial Application vs Currying

They are related but different:

- **Currying**: Always transforms into single-argument functions in sequence.
- **Partial Application**: Fixes some arguments upfront and returns a function that takes the remaining ones.

```js
// Partial application (using bind)
function greet(greeting, name) {
  return `${greeting}, ${name}!`;
}

const sayHello = greet.bind(null, "Hello"); // Fix first argument
sayHello("Mukund"); // "Hello, Mukund!"
sayHello("Rahul");  // "Hello, Rahul!"
```

---

## 17. Objects — In Depth

### 17.1 Creating Objects

```js
// 1. Object literal (most common)
const user = {
  name: "Mukund",
  age: 22,
  greet() {
    return `Hi, I'm ${this.name}`;
  }
};

// 2. new Object()
const obj = new Object();
obj.name = "Test";

// 3. Object.create() — creates with specified prototype
const proto = { greet() { return "Hello"; } };
const child = Object.create(proto);
child.greet(); // "Hello" — inherited from proto

// 4. Constructor function
function Person(name, age) {
  this.name = name;
  this.age = age;
}
const p = new Person("Mukund", 22);
```

### 17.2 Accessing & Modifying Properties

```js
const user = { name: "Mukund", "full-name": "Mukund Fegade" };

// Dot notation
user.name;        // "Mukund"

// Bracket notation (required for special characters, dynamic keys)
user["full-name"];       // "Mukund Fegade"
const key = "name";
user[key];               // "Mukund"

// Adding properties
user.city = "Pune";

// Deleting properties
delete user.city;

// Checking if property exists
"name" in user;               // true
user.hasOwnProperty("name");  // true
```

### 17.3 Computed Property Names (ES6)

```js
const key = "email";
const user = {
  name: "Mukund",
  [key]: "mukund@example.com",       // email: "mukund@example.com"
  [`${key}Verified`]: true            // emailVerified: true
};
```

### 17.4 Object Methods

```js
const user = { name: "Mukund", age: 22, city: "Pune" };

// Get keys, values, entries
Object.keys(user);     // ["name", "age", "city"]
Object.values(user);   // ["Mukund", 22, "Pune"]
Object.entries(user);  // [["name", "Mukund"], ["age", 22], ["city", "Pune"]]

// Merge objects
const defaults = { theme: "dark", lang: "en" };
const prefs = { lang: "hi" };
const merged = Object.assign({}, defaults, prefs);
// { theme: "dark", lang: "hi" } — later values overwrite

// Spread syntax (preferred)
const merged2 = { ...defaults, ...prefs };

// Freeze (immutable — can't add, remove, or modify)
const frozen = Object.freeze({ x: 1, y: 2 });
frozen.x = 99;     // Silently fails (or throws in strict mode)
frozen.z = 3;      // Silently fails
console.log(frozen); // { x: 1, y: 2 }

// Seal (can modify existing, but can't add or remove)
const sealed = Object.seal({ a: 1, b: 2 });
sealed.a = 99;      // ✅ Works
sealed.c = 3;       // ❌ Silently fails
delete sealed.a;    // ❌ Silently fails
```

### 17.5 Property Descriptors

Every property in JS has hidden attributes:

```js
const user = { name: "Mukund" };

// View descriptor
Object.getOwnPropertyDescriptor(user, "name");
// { value: "Mukund", writable: true, enumerable: true, configurable: true }

// Define with custom descriptor
Object.defineProperty(user, "id", {
  value: 42,
  writable: false,      // Can't change the value
  enumerable: false,     // Won't show up in for...in or Object.keys
  configurable: false    // Can't delete or reconfigure
});

user.id = 99;           // Silently fails (or throws in strict mode)
console.log(user.id);   // 42
Object.keys(user);      // ["name"] — "id" is not enumerable
```

### 17.6 Getters and Setters

```js
const user = {
  firstName: "Mukund",
  lastName: "Fegade",
  
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  
  set fullName(value) {
    const parts = value.split(" ");
    this.firstName = parts[0];
    this.lastName = parts[1];
  }
};

console.log(user.fullName);     // "Mukund Fegade" (accessed like a property)
user.fullName = "John Doe";      // Triggers the setter
console.log(user.firstName);    // "John"
```

---

## 18. Prototypes & Prototypal Inheritance

### 18.1 What is a Prototype?

Every JavaScript object has a hidden internal property called `[[Prototype]]` (accessible via `__proto__` or `Object.getPrototypeOf()`). This is a reference to another object from which it can **inherit properties and methods**.

```js
const animal = {
  isAlive: true,
  eat() {
    console.log("Eating...");
  }
};

const dog = Object.create(animal); // dog's prototype is animal
dog.bark = function() {
  console.log("Woof!");
};

dog.bark();      // "Woof!" — own property
dog.eat();       // "Eating..." — inherited from animal
dog.isAlive;     // true — inherited from animal
```

### 18.2 The Prototype Chain

When you access a property on an object, JavaScript:
1. Looks on the object itself
2. If not found, looks on its `__proto__` (prototype)
3. If not found, looks on the prototype's prototype
4. Continues until it reaches `Object.prototype` (which has `__proto__` of `null`)

```js
const obj = { x: 1 };

// Prototype chain:
// obj → Object.prototype → null

obj.toString();  // "[object Object]" — found on Object.prototype
obj.hasOwnProperty("x"); // true — found on Object.prototype
```

### 18.3 Constructor Functions & Prototypes

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Methods should be on the prototype (shared across all instances)
Person.prototype.greet = function() {
  return `Hi, I'm ${this.name}`;
};

const p1 = new Person("Mukund", 22);
const p2 = new Person("Rahul", 25);

p1.greet(); // "Hi, I'm Mukund"
p2.greet(); // "Hi, I'm Rahul"

// Both p1 and p2 share the SAME greet function (memory efficient)
p1.greet === p2.greet; // true

// Prototype chain:
// p1 → Person.prototype → Object.prototype → null
```

### 18.4 How `new` Works Behind the Scenes

When you call `new Person("Mukund", 22)`, JavaScript does four things:

```js
// 1. Creates a new empty object
const obj = {};

// 2. Sets its prototype to Person.prototype
Object.setPrototypeOf(obj, Person.prototype);

// 3. Executes Person with 'this' = obj
Person.call(obj, "Mukund", 22); // obj.name = "Mukund", obj.age = 22

// 4. Returns the object (unless the constructor explicitly returns an object)
return obj;
```

### 18.5 Inheritance with Prototypes

```js
function Animal(name) {
  this.name = name;
}
Animal.prototype.speak = function() {
  return `${this.name} makes a sound`;
};

function Dog(name, breed) {
  Animal.call(this, name);  // Call parent constructor
  this.breed = breed;
}

// Set up the prototype chain
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog; // Fix constructor reference

Dog.prototype.bark = function() {
  return `${this.name} barks!`;
};

const rex = new Dog("Rex", "Labrador");
rex.bark();    // "Rex barks!" — own prototype
rex.speak();   // "Rex makes a sound" — inherited from Animal
rex instanceof Dog;    // true
rex instanceof Animal; // true
```

---

## 19. ES6 Classes & OOP

### 19.1 Class Syntax

ES6 classes are **syntactic sugar** over prototypal inheritance — they don't introduce a new OOP model.

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  
  greet() {
    return `Hi, I'm ${this.name}, age ${this.age}`;
  }
  
  // Static method (called on the class, not instances)
  static create(name, age) {
    return new Person(name, age);
  }
}

const p = new Person("Mukund", 22);
p.greet();          // "Hi, I'm Mukund, age 22"

const p2 = Person.create("Rahul", 25);
// p.create();      // ❌ TypeError — static methods aren't on instances
```

### 19.2 Inheritance with `extends` and `super`

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);  // MUST call super() before using 'this'
    this.breed = breed;
  }
  
  speak() {
    return `${this.name} barks!`; // Override parent method
  }
  
  info() {
    return `${super.speak()} — Breed: ${this.breed}`;
    // super.speak() calls the parent's speak()
  }
}

const rex = new Dog("Rex", "Labrador");
rex.speak(); // "Rex barks!"
rex.info();  // "Rex makes a sound — Breed: Labrador"
```

### 19.3 Private Fields and Methods (ES2022)

```js
class BankAccount {
  #balance; // Private field — only accessible inside the class
  
  constructor(initialBalance) {
    this.#balance = initialBalance;
  }
  
  deposit(amount) {
    this.#balance += amount;
  }
  
  get balance() {
    return this.#balance;
  }
  
  #validate(amount) { // Private method
    return amount > 0 && amount <= this.#balance;
  }
  
  withdraw(amount) {
    if (this.#validate(amount)) {
      this.#balance -= amount;
      return true;
    }
    return false;
  }
}

const acc = new BankAccount(1000);
acc.deposit(500);
console.log(acc.balance);   // 1500
// acc.#balance;             // ❌ SyntaxError — private field
```

### 19.4 The Four Pillars of OOP in JavaScript

```js
// 1. ENCAPSULATION — bundling data and methods together
class User {
  #password;
  constructor(name, password) {
    this.name = name;
    this.#password = password;
  }
  authenticate(input) {
    return input === this.#password;
  }
}

// 2. ABSTRACTION — hiding complexity, exposing only what's necessary
// (The User class above abstracts away password comparison logic)

// 3. INHERITANCE — creating new classes from existing ones
class Admin extends User {
  constructor(name, password) {
    super(name, password);
    this.role = "admin";
  }
  deleteUser(userId) { /* admin-only functionality */ }
}

// 4. POLYMORPHISM — same method, different behavior
class Shape {
  area() { throw new Error("Must implement area()"); }
}
class Circle extends Shape {
  constructor(r) { super(); this.r = r; }
  area() { return Math.PI * this.r ** 2; }
}
class Rectangle extends Shape {
  constructor(w, h) { super(); this.w = w; this.h = h; }
  area() { return this.w * this.h; }
}
// Both have area() but different implementations
```

---

## 20. Arrays — Deep Dive & Methods

### 20.1 Creating Arrays

```js
const arr1 = [1, 2, 3];                    // Literal (preferred)
const arr2 = new Array(3);                  // [empty × 3] (sparse array)
const arr3 = Array.of(1, 2, 3);            // [1, 2, 3]
const arr4 = Array.from("hello");           // ["h", "e", "l", "l", "o"]
const arr5 = Array.from({ length: 3 }, (_, i) => i + 1); // [1, 2, 3]
```

### 20.2 Mutating Methods (modify the original array)

```js
const arr = [1, 2, 3, 4, 5];

arr.push(6);        // [1,2,3,4,5,6] — add to end
arr.pop();          // [1,2,3,4,5] — remove from end (returns 6)
arr.unshift(0);     // [0,1,2,3,4,5] — add to start
arr.shift();        // [1,2,3,4,5] — remove from start (returns 0)

arr.splice(1, 2);             // removes 2 items at index 1 → [1, 4, 5]
arr.splice(1, 0, 2, 3);      // inserts at index 1 → [1, 2, 3, 4, 5]
arr.splice(1, 1, 99);        // replaces index 1 → [1, 99, 3, 4, 5]

arr.reverse();      // reverses in place
arr.sort((a, b) => a - b);   // sorts in place (numeric ascending)
arr.fill(0, 1, 3);           // fills from index 1 to 3 with 0
```

### 20.3 Non-Mutating Methods (return new array/value)

```js
const arr = [1, 2, 3, 4, 5];

arr.slice(1, 3);     // [2, 3] — extract portion (does NOT modify original)
arr.concat([6, 7]);  // [1, 2, 3, 4, 5, 6, 7]
arr.includes(3);     // true
arr.indexOf(3);      // 2 (first index, or -1 if not found)
arr.join("-");       // "1-2-3-4-5"
arr.flat(Infinity);  // Flattens nested arrays

// ES2023 — Non-mutating versions of mutating methods
arr.toReversed();    // Returns reversed copy (original unchanged)
arr.toSorted();      // Returns sorted copy
arr.toSpliced(1, 1); // Returns spliced copy
arr.with(0, 99);     // Returns copy with index 0 changed to 99
```

### 20.4 Flattening Arrays

```js
const nested = [1, [2, 3], [4, [5, 6]]];

nested.flat();      // [1, 2, 3, 4, [5, 6]] — one level deep
nested.flat(2);     // [1, 2, 3, 4, 5, 6] — two levels
nested.flat(Infinity); // [1, 2, 3, 4, 5, 6] — fully flattened

// Manual implementation with reduce (interview favorite)
function flatten(arr) {
  return arr.reduce((acc, item) => {
    return acc.concat(Array.isArray(item) ? flatten(item) : item);
  }, []);
}
```

---

## 21. Destructuring (Arrays & Objects)

### 21.1 Object Destructuring

```js
const user = { name: "Mukund", age: 22, city: "Pune", country: "India" };

// Basic
const { name, age } = user;
console.log(name); // "Mukund"

// With renaming
const { name: userName, age: userAge } = user;
console.log(userName); // "Mukund"

// With default values
const { name, phone = "N/A" } = user;
console.log(phone); // "N/A" (property doesn't exist)

// Nested destructuring
const response = {
  data: {
    user: {
      name: "Mukund",
      address: { city: "Pune" }
    }
  }
};
const { data: { user: { name, address: { city } } } } = response;
console.log(city); // "Pune"

// Rest in destructuring
const { name, ...rest } = user;
console.log(rest); // { age: 22, city: "Pune", country: "India" }
```

### 21.2 Array Destructuring

```js
const colors = ["red", "green", "blue", "yellow"];

// Basic
const [first, second] = colors;
console.log(first); // "red"

// Skip elements
const [, , third] = colors;
console.log(third); // "blue"

// Rest
const [head, ...tail] = colors;
console.log(tail); // ["green", "blue", "yellow"]

// Default values
const [a = 1, b = 2, c = 3] = [10];
console.log(a, b, c); // 10, 2, 3

// Swapping variables (no temp variable needed!)
let x = 1, y = 2;
[x, y] = [y, x];
console.log(x, y); // 2, 1
```

### 21.3 Destructuring in Function Parameters

```js
// Object parameter destructuring
function createUser({ name, age, city = "Unknown" }) {
  return `${name}, ${age}, ${city}`;
}
createUser({ name: "Mukund", age: 22 }); // "Mukund, 22, Unknown"

// Array parameter destructuring
function getFirstTwo([first, second]) {
  return [first, second];
}
getFirstTwo([1, 2, 3, 4]); // [1, 2]
```

---

## 22. Spread & Rest Operators

Both use `...` syntax but serve opposite purposes.

### 22.1 Spread Operator (Expands)

**Spreads an iterable into individual elements:**

```js
// Arrays
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];     // [1, 2, 3, 4, 5, 6]
const copy = [...arr1];                  // [1, 2, 3] (shallow copy)

// Objects
const defaults = { theme: "dark", lang: "en" };
const prefs = { lang: "hi", font: "large" };
const settings = { ...defaults, ...prefs };
// { theme: "dark", lang: "hi", font: "large" }

// Function arguments
const numbers = [5, 1, 8, 3];
Math.max(...numbers); // 8 (spreads array into individual args)

// String to array
[..."hello"]; // ["h", "e", "l", "l", "o"]
```

### 22.2 Rest Operator (Collects)

**Collects remaining elements into an array/object:**

```js
// In function parameters (collects remaining args)
function sum(first, ...rest) {
  console.log(first); // 1
  console.log(rest);  // [2, 3, 4, 5]
  return rest.reduce((a, b) => a + b, first);
}
sum(1, 2, 3, 4, 5); // 15

// In destructuring (collects remaining properties/elements)
const { a, ...others } = { a: 1, b: 2, c: 3 };
console.log(others); // { b: 2, c: 3 }

const [head, ...tail] = [1, 2, 3, 4];
console.log(tail); // [2, 3, 4]
```

### 22.3 Key Difference

| Spread `...` | Rest `...` |
|--------------|-----------|
| Used in function calls, array/object literals | Used in function parameters, destructuring |
| **Expands** an iterable into elements | **Collects** multiple elements into one |
| `Math.max(...[1,2,3])` | `function sum(...nums) {}` |

---

## 23. Template Literals

### 23.1 Basic Usage

```js
const name = "Mukund";
const age = 22;

// Old way
const old = "Hello, " + name + "! You are " + age + " years old.";

// Template literal — use backticks (`)
const modern = `Hello, ${name}! You are ${age} years old.`;

// Multi-line strings (preserves whitespace and newlines)
const html = `
  <div class="card">
    <h2>${name}</h2>
    <p>Age: ${age}</p>
  </div>
`;
```

### 23.2 Expressions Inside `${}`

```js
// Any valid expression works
`Total: ${10 + 20}`;                    // "Total: 30"
`Status: ${age >= 18 ? "Adult" : "Minor"}`;  // "Status: Adult"
`Items: ${[1,2,3].join(", ")}`;         // "Items: 1, 2, 3"
`Upper: ${name.toUpperCase()}`;         // "Upper: MUKUND"
```

### 23.3 Tagged Templates

A function that processes a template literal:

```js
function highlight(strings, ...values) {
  // strings = ["Hello, ", "! You are ", " years old."]
  // values = ["Mukund", 22]
  
  return strings.reduce((result, str, i) => {
    return result + str + (values[i] !== undefined ? `<b>${values[i]}</b>` : "");
  }, "");
}

const result = highlight`Hello, ${name}! You are ${age} years old.`;
// "Hello, <b>Mukund</b>! You are <b>22</b> years old."
```

Tagged templates are used in libraries like `styled-components` and `GraphQL`.

---

## 24. Symbols

### 24.1 What is a Symbol?

A `Symbol` is a **primitive data type** that creates a **unique, immutable identifier**. No two symbols are ever equal.

```js
const sym1 = Symbol("id");
const sym2 = Symbol("id");
console.log(sym1 === sym2); // false — each Symbol is unique

// Symbols as object keys (can't be accessed by accident)
const user = {
  name: "Mukund",
  [sym1]: 12345  // Symbol as key
};

console.log(user[sym1]); // 12345
console.log(user.sym1);  // undefined — dot notation doesn't work with Symbols

// Symbols are NOT included in for...in, Object.keys, JSON.stringify
Object.keys(user);       // ["name"]
JSON.stringify(user);    // '{"name":"Mukund"}'
Object.getOwnPropertySymbols(user); // [Symbol(id)]
```

### 24.2 Well-Known Symbols

JavaScript has built-in symbols that let you customize object behavior:

```js
// Symbol.iterator — makes an object iterable
const range = {
  from: 1,
  to: 5,
  [Symbol.iterator]() {
    let current = this.from;
    const last = this.to;
    return {
      next() {
        return current <= last
          ? { value: current++, done: false }
          : { done: true };
      }
    };
  }
};

for (const num of range) {
  console.log(num); // 1, 2, 3, 4, 5
}

// Symbol.toPrimitive — customize type conversion
const money = {
  amount: 1000,
  currency: "INR",
  [Symbol.toPrimitive](hint) {
    if (hint === "number") return this.amount;
    if (hint === "string") return `${this.amount} ${this.currency}`;
    return this.amount;
  }
};

+money;          // 1000
`${money}`;      // "1000 INR"
```

### 24.3 Symbol.for() — Global Symbol Registry

```js
// Symbol.for creates or retrieves a symbol from the global registry
const s1 = Symbol.for("app.id");
const s2 = Symbol.for("app.id");
console.log(s1 === s2); // true — same symbol from registry

Symbol.keyFor(s1); // "app.id" — look up the key
```

---

## 25. Iterators & Generators

### 25.1 The Iterator Protocol

An object is an **iterator** if it has a `next()` method that returns `{ value, done }`.

```js
// Manual iterator
function createRangeIterator(start, end) {
  let current = start;
  return {
    next() {
      if (current <= end) {
        return { value: current++, done: false };
      }
      return { value: undefined, done: true };
    }
  };
}

const iter = createRangeIterator(1, 3);
iter.next(); // { value: 1, done: false }
iter.next(); // { value: 2, done: false }
iter.next(); // { value: 3, done: false }
iter.next(); // { value: undefined, done: true }
```

### 25.2 The Iterable Protocol

An object is **iterable** if it has a `[Symbol.iterator]()` method that returns an iterator. This is what allows `for...of`, spread, and destructuring.

```js
const iterableRange = {
  from: 1,
  to: 5,
  [Symbol.iterator]() {
    let current = this.from;
    const last = this.to;
    return {
      next() {
        return current <= last
          ? { value: current++, done: false }
          : { done: true };
      }
    };
  }
};

// Now all these work:
for (const n of iterableRange) console.log(n);  // 1, 2, 3, 4, 5
const arr = [...iterableRange];                   // [1, 2, 3, 4, 5]
const [first, second] = iterableRange;            // 1, 2
```

### 25.3 Generator Functions

Generators are a simpler way to create iterators using `function*` and `yield`:

```js
function* numberGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = numberGenerator();
gen.next(); // { value: 1, done: false }
gen.next(); // { value: 2, done: false }
gen.next(); // { value: 3, done: false }
gen.next(); // { value: undefined, done: true }

// Generators are iterable!
for (const n of numberGenerator()) {
  console.log(n); // 1, 2, 3
}
```

### 25.4 Infinite Sequences with Generators

```js
function* fibonacci() {
  let a = 0, b = 1;
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

// Take first 10 Fibonacci numbers
const fib = fibonacci();
const first10 = [];
for (let i = 0; i < 10; i++) {
  first10.push(fib.next().value);
}
// [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

### 25.5 yield* — Delegating to Another Generator

```js
function* inner() {
  yield "a";
  yield "b";
}

function* outer() {
  yield 1;
  yield* inner(); // Delegates to inner generator
  yield 2;
}

[...outer()]; // [1, "a", "b", 2]
```

### 25.6 Sending Values into Generators

```js
function* conversation() {
  const name = yield "What is your name?";
  const age = yield `Hello ${name}! How old are you?`;
  return `${name} is ${age} years old.`;
}

const chat = conversation();
console.log(chat.next());           // { value: "What is your name?", done: false }
console.log(chat.next("Mukund"));   // { value: "Hello Mukund! How old are you?", done: false }
console.log(chat.next(22));         // { value: "Mukund is 22 years old.", done: true }
```

---

## 26. Map, Set, WeakMap, WeakSet

### 26.1 Map

A `Map` is a collection of **key-value pairs** where keys can be **any type** (unlike objects, where keys must be strings or symbols).

```js
const map = new Map();

// Set entries
map.set("name", "Mukund");
map.set(42, "the answer");
map.set(true, "boolean key");

const objKey = { id: 1 };
map.set(objKey, "object as key!");

// Get entries
map.get("name");    // "Mukund"
map.get(42);        // "the answer"
map.get(objKey);    // "object as key!"

// Size and checks
map.size;           // 4
map.has("name");    // true
map.delete(42);     // true
map.clear();        // removes all entries

// Iteration (preserves insertion order)
const userMap = new Map([
  ["name", "Mukund"],
  ["age", 22],
  ["city", "Pune"]
]);

for (const [key, value] of userMap) {
  console.log(`${key}: ${value}`);
}

userMap.forEach((value, key) => {
  console.log(`${key}: ${value}`);
});
```

### 26.2 Map vs Object

| Feature | Map | Object |
|---------|-----|--------|
| Key types | Any type | String or Symbol |
| Order | Insertion order guaranteed | Mostly ordered (but has quirks) |
| Size | `map.size` | `Object.keys(obj).length` |
| Iteration | Directly iterable | Need `Object.keys/values/entries` |
| Performance | Better for frequent add/delete | Better for static data |
| Default keys | None | Has prototype keys |

### 26.3 Set

A `Set` stores **unique values** of any type.

```js
const set = new Set([1, 2, 3, 3, 3]);
console.log(set); // Set { 1, 2, 3 } — duplicates removed

set.add(4);
set.add(1);        // Ignored — already exists
set.has(2);        // true
set.delete(2);     // true
set.size;          // 3

// Iteration
for (const value of set) {
  console.log(value);
}

// Convert to array
const arr = [...set];

// Common use: Remove duplicates from an array
const unique = [...new Set([1, 2, 2, 3, 3, 4])];
// [1, 2, 3, 4]
```

### 26.4 WeakMap

Like `Map`, but:
- Keys **must be objects** (not primitives)
- Entries are **weakly referenced** — if the key object has no other references, it can be garbage collected
- **Not iterable** and has no `.size`

```js
const weakMap = new WeakMap();

let user = { name: "Mukund" };
weakMap.set(user, "some metadata");

weakMap.get(user); // "some metadata"
weakMap.has(user); // true

// If 'user' is set to null, the entry becomes eligible for garbage collection
user = null; // WeakMap entry will be cleaned up automatically
```

**Use case**: Storing metadata about objects without preventing garbage collection — e.g., caching computed values for DOM elements.

### 26.5 WeakSet

Like `Set`, but:
- Values **must be objects**
- Weakly referenced (GC-friendly)
- **Not iterable** and has no `.size`

```js
const weakSet = new WeakSet();

let obj = { id: 1 };
weakSet.add(obj);
weakSet.has(obj); // true

obj = null; // Entry becomes eligible for GC
```

**Use case**: Tracking visited objects, marking objects as "processed" without creating memory leaks.

---

## 27. Shallow Copy vs Deep Copy

### 27.1 The Problem

When you "copy" an object, you might only copy the **reference**, not the actual data:

```js
const original = { name: "Mukund", address: { city: "Pune" } };
const copy = original;

copy.name = "Changed";
console.log(original.name); // "Changed" ❌ — same reference!
```

### 27.2 Shallow Copy

Creates a new object, but **nested objects still share the same reference**:

```js
const original = {
  name: "Mukund",
  skills: ["JS", "React"],
  address: { city: "Pune" }
};

// Method 1: Spread operator
const shallow1 = { ...original };

// Method 2: Object.assign
const shallow2 = Object.assign({}, original);

// Top-level changes are independent
shallow1.name = "Changed";
console.log(original.name); // "Mukund" ✅

// But nested objects are still shared!
shallow1.address.city = "Mumbai";
console.log(original.address.city); // "Mumbai" ❌ — shared reference!

shallow1.skills.push("Node");
console.log(original.skills); // ["JS", "React", "Node"] ❌
```

### 27.3 Deep Copy

Creates a completely independent copy at **all levels of nesting**:

```js
// Method 1: structuredClone (modern — best approach)
const deep1 = structuredClone(original);
deep1.address.city = "Delhi";
console.log(original.address.city); // "Pune" ✅

// Method 2: JSON trick (works for simple data — no functions, Dates, etc.)
const deep2 = JSON.parse(JSON.stringify(original));
// ⚠️ Limitations: loses functions, undefined, Symbols, Dates become strings, circular refs throw

// Method 3: Manual recursive function
function deepClone(obj) {
  if (obj === null || typeof obj !== "object") return obj;
  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof Array) return obj.map(item => deepClone(item));
  
  const cloned = {};
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      cloned[key] = deepClone(obj[key]);
    }
  }
  return cloned;
}
```

### 27.4 Comparison Summary

| | Assignment (`=`) | Shallow Copy | Deep Copy |
|---|---|---|---|
| Top-level primitives | Shared | Independent | Independent |
| Nested objects | Shared | **Shared** ⚠️ | Independent |
| Method | `const b = a` | `{...a}` / `Object.assign` | `structuredClone(a)` |

---

## 28. Error Handling (try/catch/finally)

### 28.1 Basic Syntax

```js
try {
  // Code that might throw an error
  const data = JSON.parse("invalid json");
} catch (error) {
  // Handle the error
  console.error("Error:", error.message); // "Unexpected token i in JSON at position 0"
  console.error("Type:", error.name);     // "SyntaxError"
  console.error("Stack:", error.stack);   // Full stack trace
} finally {
  // ALWAYS runs — whether error occurred or not
  console.log("Cleanup done");
}
```

### 28.2 Error Types

```js
// Built-in error types:
new Error("Generic error");
new SyntaxError("Invalid syntax");
new ReferenceError("Variable not defined");
new TypeError("Wrong type");
new RangeError("Value out of range");
new URIError("Invalid URI");
new EvalError("Eval error");
```

### 28.3 Custom Errors

```js
class ValidationError extends Error {
  constructor(field, message) {
    super(message);
    this.name = "ValidationError";
    this.field = field;
  }
}

class NotFoundError extends Error {
  constructor(resource) {
    super(`${resource} not found`);
    this.name = "NotFoundError";
    this.statusCode = 404;
  }
}

// Usage
function validateAge(age) {
  if (typeof age !== "number") {
    throw new ValidationError("age", "Age must be a number");
  }
  if (age < 0 || age > 150) {
    throw new RangeError("Age must be between 0 and 150");
  }
}

try {
  validateAge("twenty");
} catch (error) {
  if (error instanceof ValidationError) {
    console.log(`Field: ${error.field}, Message: ${error.message}`);
  } else if (error instanceof RangeError) {
    console.log(error.message);
  } else {
    throw error; // Re-throw unknown errors
  }
}
```

### 28.4 Error Handling in Async Code

```js
// With promises
fetch("/api/data")
  .then(res => res.json())
  .catch(error => console.error("Fetch failed:", error));

// With async/await
async function fetchData() {
  try {
    const response = await fetch("/api/data");
    if (!response.ok) throw new Error(`HTTP ${response.status}`);
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Failed to fetch:", error);
    throw error; // Re-throw if you want calling code to handle it too
  }
}
```

---

## 29. Asynchronous JavaScript — The Event Loop

This is one of the **most critical** topics for mastering JavaScript. Understanding the event loop explains *why* async code behaves the way it does.

### 29.1 The Problem

JavaScript is **single-threaded** — it has one call stack and can do one thing at a time. But web apps need to:
- Fetch data from APIs
- Wait for user clicks
- Set timers

If JS blocked while waiting, the entire page would freeze. The solution: the **Event Loop**.

### 29.2 The Architecture

```
┌──────────────────────────────────────────────┐
│                CALL STACK                     │
│  (Executes synchronous code, one frame at    │
│   a time, LIFO order)                        │
└──────────────┬───────────────────────────────┘
               │
               ▼
┌──────────────────────────────────────────────┐
│              EVENT LOOP                       │
│  (Checks: Is the call stack empty?           │
│   If yes → move next task from queues)       │
└──────┬──────────────────┬────────────────────┘
       │                  │
       ▼                  ▼
┌──────────────┐   ┌──────────────────┐
│ MICROTASK    │   │ MACROTASK        │
│ QUEUE        │   │ (CALLBACK) QUEUE │
│              │   │                  │
│ • Promises   │   │ • setTimeout     │
│ • .then()    │   │ • setInterval    │
│ • async/await│   │ • I/O operations │
│ • queueMicro │   │ • UI rendering   │
│   task()     │   │ • Event handlers │
│ • MutationObs│   │                  │
└──────────────┘   └──────────────────┘
```

### 29.3 The Rules

1. Execute all **synchronous code** first (call stack must be empty).
2. Then execute **ALL microtasks** (Promise callbacks, queueMicrotask).
3. Then execute **ONE macrotask** (setTimeout, setInterval callbacks).
4. Then repeat from step 2.

**Key insight**: Microtasks have **priority** over macrotasks. The microtask queue is fully drained before the next macrotask runs.

### 29.4 The Classic Interview Question

```js
console.log("1");

setTimeout(() => {
  console.log("2");
}, 0);

Promise.resolve().then(() => {
  console.log("3");
});

console.log("4");
```

**Output: `1, 4, 3, 2`**

**Why?**
1. `console.log("1")` — synchronous → runs immediately
2. `setTimeout(callback, 0)` — schedules callback in **macrotask queue**
3. `Promise.resolve().then(callback)` — schedules callback in **microtask queue**
4. `console.log("4")` — synchronous → runs immediately
5. Call stack is now empty → Event loop checks **microtask queue first** → `console.log("3")`
6. Microtask queue empty → Event loop takes from **macrotask queue** → `console.log("2")`

### 29.5 Complex Example

```js
console.log("Start");

setTimeout(() => console.log("Timeout 1"), 0);

Promise.resolve()
  .then(() => {
    console.log("Promise 1");
    setTimeout(() => console.log("Timeout 2"), 0);
  })
  .then(() => console.log("Promise 2"));

setTimeout(() => console.log("Timeout 3"), 0);

console.log("End");

// Output:
// Start
// End
// Promise 1
// Promise 2
// Timeout 1
// Timeout 3
// Timeout 2
```

**Breakdown:**
1. `"Start"` — sync
2. `setTimeout "Timeout 1"` → macro queue
3. Promise chain `.then` → micro queue
4. `setTimeout "Timeout 3"` → macro queue
5. `"End"` — sync
6. Stack empty → drain microtasks: `"Promise 1"`, which schedules `"Timeout 2"` to macro queue
7. Continue draining microtasks: `"Promise 2"`
8. Microtask queue empty → macrotask: `"Timeout 1"`
9. Next macrotask: `"Timeout 3"`
10. Next macrotask: `"Timeout 2"`

---

## 30. Callbacks & Callback Hell

### 30.1 What is a Callback?

A callback is simply a **function passed as an argument to another function**, to be executed later.

```js
function fetchData(callback) {
  setTimeout(() => {
    const data = { name: "Mukund", age: 22 };
    callback(data);
  }, 1000);
}

fetchData(function(data) {
  console.log(data); // { name: "Mukund", age: 22 } — after 1 second
});
```

### 30.2 Error-First Callbacks (Node.js Convention)

```js
function readFile(path, callback) {
  setTimeout(() => {
    if (!path) {
      callback(new Error("Path is required"), null);
    } else {
      callback(null, "File contents here");
    }
  }, 1000);
}

readFile("/data.txt", (error, data) => {
  if (error) {
    console.error("Error:", error.message);
    return;
  }
  console.log("Data:", data);
});
```

### 30.3 Callback Hell (Pyramid of Doom)

When you have multiple asynchronous operations that depend on each other:

```js
getUser(userId, function(user) {
  getOrders(user.id, function(orders) {
    getOrderDetails(orders[0].id, function(details) {
      getShippingInfo(details.shippingId, function(shipping) {
        console.log(shipping);
        // 🔺 This keeps going deeper — "Pyramid of Doom"
      });
    });
  });
});
```

**Problems:**
- Hard to read and maintain
- Error handling is complex (need to handle at every level)
- Difficult to add control flow (parallel operations, retries)

**Solution**: Promises and async/await (covered next).

---

## 31. Promises

### 31.1 What is a Promise?

A Promise is an object representing the **eventual completion (or failure)** of an asynchronous operation. It's a cleaner alternative to callbacks.

A promise can be in one of three states:
- **Pending** — initial state, neither fulfilled nor rejected
- **Fulfilled** — operation completed successfully
- **Rejected** — operation failed

```js
const promise = new Promise((resolve, reject) => {
  // Async operation
  setTimeout(() => {
    const success = true;
    if (success) {
      resolve("Data loaded!"); // Fulfilled
    } else {
      reject(new Error("Something went wrong")); // Rejected
    }
  }, 1000);
});

promise
  .then(data => console.log(data))     // "Data loaded!"
  .catch(error => console.error(error))
  .finally(() => console.log("Done")); // Always runs
```

### 31.2 Promise Chaining

Each `.then()` returns a **new Promise**, allowing you to chain:

```js
// This replaces callback hell
getUser(userId)
  .then(user => getOrders(user.id))
  .then(orders => getOrderDetails(orders[0].id))
  .then(details => getShippingInfo(details.shippingId))
  .then(shipping => console.log(shipping))
  .catch(error => console.error("Any error in the chain:", error));
```

### 31.3 Promise Static Methods

#### `Promise.all()` — Parallel execution, fail-fast
```js
const p1 = fetch("/api/users");
const p2 = fetch("/api/posts");
const p3 = fetch("/api/comments");

Promise.all([p1, p2, p3])
  .then(([users, posts, comments]) => {
    console.log("All loaded!");
  })
  .catch(error => {
    console.error("At least one failed:", error);
    // If ANY promise rejects, the whole thing rejects
  });
```

#### `Promise.allSettled()` — Wait for all, regardless of outcome
```js
Promise.allSettled([p1, p2, p3])
  .then(results => {
    results.forEach(result => {
      if (result.status === "fulfilled") {
        console.log("Success:", result.value);
      } else {
        console.log("Failed:", result.reason);
      }
    });
  });
```

#### `Promise.race()` — First to settle wins
```js
Promise.race([
  fetch("/api/fast"),
  new Promise((_, reject) => setTimeout(() => reject("Timeout"), 3000))
])
  .then(result => console.log("Got response before timeout"))
  .catch(error => console.log("Timed out"));
```

#### `Promise.any()` — First to FULFILL wins
```js
Promise.any([
  fetch("/mirror1/data"),
  fetch("/mirror2/data"),
  fetch("/mirror3/data")
])
  .then(firstSuccess => console.log("Got data from fastest mirror"))
  .catch(error => console.log("ALL mirrors failed"));
// Only rejects if ALL promises reject (gives AggregateError)
```

### 31.4 Comparison Table

| Method | Resolves when | Rejects when |
|--------|-------------|-------------|
| `Promise.all` | ALL fulfill | ANY rejects |
| `Promise.allSettled` | ALL settle (fulfill or reject) | Never rejects |
| `Promise.race` | First to settle (fulfill OR reject) | First to settle rejects |
| `Promise.any` | First to fulfill | ALL reject |

---

## 32. async / await

### 32.1 Basics

`async/await` is **syntactic sugar** over Promises — it makes async code look and behave like synchronous code.

```js
// Without async/await
function fetchUser() {
  return fetch("/api/user")
    .then(res => res.json())
    .then(data => data);
}

// With async/await
async function fetchUser() {
  const response = await fetch("/api/user");
  const data = await response.json();
  return data; // Automatically wrapped in a Promise
}
```

### 32.2 Key Rules

```js
// 1. 'async' functions ALWAYS return a Promise
async function greet() {
  return "Hello"; // Same as: return Promise.resolve("Hello")
}
greet().then(msg => console.log(msg)); // "Hello"

// 2. 'await' can ONLY be used inside an 'async' function (or top-level in modules)
// await fetch("/api"); // ❌ SyntaxError (outside async function)

// 3. 'await' pauses execution until the Promise resolves
async function demo() {
  console.log("Before");
  await new Promise(resolve => setTimeout(resolve, 2000));
  console.log("After 2 seconds");
}
```

### 32.3 Error Handling with async/await

```js
// try/catch (recommended)
async function fetchData() {
  try {
    const response = await fetch("/api/data");
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Failed:", error.message);
    throw error; // Re-throw if you want the caller to handle it
  }
}

// .catch() on the returned promise (alternative)
fetchData().catch(err => console.error(err));
```

### 32.4 Parallel Execution with async/await

```js
// ❌ Sequential (slow — each waits for the previous)
async function sequential() {
  const users = await fetch("/api/users");     // Wait...
  const posts = await fetch("/api/posts");     // Then wait...
  const comments = await fetch("/api/comments"); // Then wait...
  // Total time ≈ sum of all three
}

// ✅ Parallel (fast — all start at the same time)
async function parallel() {
  const [users, posts, comments] = await Promise.all([
    fetch("/api/users"),
    fetch("/api/posts"),
    fetch("/api/comments")
  ]);
  // Total time ≈ longest request
}
```

### 32.5 Top-Level await (ES Modules)

In ES modules, you can use `await` at the top level:

```js
// config.mjs (ES module)
const response = await fetch("/api/config");
export const config = await response.json();
```

### 32.6 async/await with Loops

```js
const urls = ["/api/1", "/api/2", "/api/3"];

// ❌ forEach does NOT work with async/await (runs in parallel, no waiting)
urls.forEach(async (url) => {
  const data = await fetch(url); // These DON'T wait for each other
});

// ✅ for...of for sequential processing
for (const url of urls) {
  const response = await fetch(url);
  console.log(await response.json());
}

// ✅ Promise.all for parallel processing
const results = await Promise.all(
  urls.map(url => fetch(url).then(r => r.json()))
);
```

---

## 33. Timers — setTimeout, setInterval, requestAnimationFrame

### 33.1 setTimeout

Executes a function **once** after a specified delay:

```js
const timerId = setTimeout(() => {
  console.log("Runs after 2 seconds");
}, 2000);

// Cancel before it runs
clearTimeout(timerId);

// ⚠️ setTimeout(fn, 0) does NOT run immediately
setTimeout(() => console.log("timeout"), 0);
console.log("sync");
// Output: "sync", "timeout"
// Why? setTimeout(fn, 0) adds to the macrotask queue — sync code runs first
```

### 33.2 setInterval

Executes a function **repeatedly** at the specified interval:

```js
let count = 0;
const intervalId = setInterval(() => {
  count++;
  console.log(`Tick ${count}`);
  if (count >= 5) {
    clearInterval(intervalId); // Stop after 5 ticks
  }
}, 1000);
```

### 33.3 setTimeout for Recursive Intervals

`setInterval` doesn't account for execution time. Use recursive `setTimeout` for precise intervals:

```js
// More precise than setInterval
function preciseInterval(callback, delay) {
  function tick() {
    callback();
    setTimeout(tick, delay); // Schedule next tick AFTER execution completes
  }
  setTimeout(tick, delay);
}

preciseInterval(() => console.log("Precise tick"), 1000);
```

### 33.4 requestAnimationFrame

Optimized for visual updates — runs before the next repaint (~60fps):

```js
function animate(timestamp) {
  // Update animation state
  element.style.left = `${position}px`;
  position += 2;
  
  if (position < 500) {
    requestAnimationFrame(animate); // Schedule next frame
  }
}

const animationId = requestAnimationFrame(animate);
// cancelAnimationFrame(animationId); // To stop
```

**Why use it?** It syncs with the browser's refresh rate, pauses when the tab is inactive, and is more efficient than setTimeout-based animations.

---

## 34. Debouncing & Throttling

Both are techniques to **limit how often a function runs**. They're essential for performance optimization.

### 34.1 Debouncing

**"Wait until the user stops doing something, then run."**

The function only fires after a specified period of **inactivity**. If the user keeps triggering it, the timer resets.

```js
function debounce(fn, delay) {
  let timeoutId;
  
  return function(...args) {
    clearTimeout(timeoutId); // Reset timer on every call
    timeoutId = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  };
}

// Usage: Search input — only search after user stops typing for 300ms
const searchInput = document.querySelector("#search");

const debouncedSearch = debounce((query) => {
  console.log("Searching for:", query);
  // API call here
}, 300);

searchInput.addEventListener("input", (e) => {
  debouncedSearch(e.target.value);
});
```

**Use cases**: Search input, window resize, auto-save

### 34.2 Throttling

**"Run at most once every X milliseconds."**

Guarantees a function runs at a regular rate, regardless of how many times it's triggered.

```js
function throttle(fn, limit) {
  let inThrottle = false;
  
  return function(...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;
      setTimeout(() => {
        inThrottle = false;
      }, limit);
    }
  };
}

// Usage: Scroll event — update at most every 200ms
const throttledScroll = throttle(() => {
  console.log("Scroll position:", window.scrollY);
}, 200);

window.addEventListener("scroll", throttledScroll);
```

**Use cases**: Scroll events, resize events, mouse move tracking, rate-limiting API calls

### 34.3 Debounce vs Throttle

| Feature | Debounce | Throttle |
|---------|----------|----------|
| When it fires | After **inactivity period** | At **regular intervals** |
| During rapid events | Waits, fires ONCE at the end | Fires at every interval |
| Use case | Search, auto-save | Scroll, resize, gaming |
| Analogy | Elevator: waits until people stop entering | Traffic light: lets cars through at intervals |

### 34.4 Leading vs Trailing

```js
// Leading debounce — fires IMMEDIATELY, then ignores for delay
function debounceLeading(fn, delay) {
  let timeoutId;
  return function(...args) {
    if (!timeoutId) {
      fn.apply(this, args); // Fire immediately
    }
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => {
      timeoutId = null;
    }, delay);
  };
}
```

---

## 35. The DOM — Document Object Model

### 35.1 What is the DOM?

The DOM is a **tree-structured representation** of an HTML document. JavaScript uses the DOM API to read, modify, and manipulate web pages dynamically.

```
document
└── html
    ├── head
    │   ├── title
    │   └── meta
    └── body
        ├── h1
        ├── p
        └── div
            ├── span
            └── button
```

### 35.2 Selecting Elements

```js
// By ID (returns single element)
const header = document.getElementById("header");

// By class name (returns live HTMLCollection)
const items = document.getElementsByClassName("item");

// By tag name (returns live HTMLCollection)
const paragraphs = document.getElementsByTagName("p");

// CSS selectors (recommended — most flexible)
const element = document.querySelector(".card:first-child");  // First match
const elements = document.querySelectorAll(".card");          // All matches (NodeList)

// querySelector vs getElementById
// querySelector is slower but more flexible (takes any CSS selector)
// getElementById is fastest for IDs
```

### 35.3 Manipulating Elements

```js
const el = document.querySelector("#myElement");

// Content
el.textContent = "Plain text (no HTML parsing)";
el.innerHTML = "<strong>Bold text</strong>"; // ⚠️ XSS risk with user input!
el.innerText = "Visible text only";

// Attributes
el.setAttribute("data-id", "123");
el.getAttribute("data-id");   // "123"
el.removeAttribute("data-id");
el.dataset.id;                 // "123" (shorthand for data-* attributes)

// Classes
el.classList.add("active");
el.classList.remove("active");
el.classList.toggle("active");
el.classList.contains("active");
el.classList.replace("old", "new");

// Styles
el.style.color = "red";
el.style.backgroundColor = "blue"; // camelCase
el.style.cssText = "color: red; background: blue;"; // Multiple styles
// Prefer adding/removing CSS classes over inline styles
```

### 35.4 Creating & Inserting Elements

```js
// Create
const div = document.createElement("div");
div.textContent = "New element";
div.classList.add("card");

// Insert
document.body.appendChild(div);          // Append as last child
parent.insertBefore(div, referenceNode); // Insert before specific element

// Modern methods (more intuitive)
parent.prepend(div);                     // First child
parent.append(div);                      // Last child
element.before(div);                     // Before the element
element.after(div);                      // After the element

// insertAdjacentHTML — insert HTML string at specific position
element.insertAdjacentHTML("beforebegin", "<p>Before</p>"); // Before element
element.insertAdjacentHTML("afterbegin", "<p>First child</p>");
element.insertAdjacentHTML("beforeend", "<p>Last child</p>");
element.insertAdjacentHTML("afterend", "<p>After</p>");

// Remove
element.remove();                        // Modern (self-remove)
parent.removeChild(child);               // Old way
```

### 35.5 Document Fragment

For batch DOM operations (avoids layout thrashing):

```js
const fragment = document.createDocumentFragment();

for (let i = 0; i < 1000; i++) {
  const li = document.createElement("li");
  li.textContent = `Item ${i}`;
  fragment.appendChild(li); // No reflow yet!
}

document.querySelector("ul").appendChild(fragment); // Single reflow
```

---

## 36. Event Handling & Event Propagation

### 36.1 Adding Event Listeners

```js
const button = document.querySelector("#myButton");

// addEventListener (recommended — allows multiple handlers)
button.addEventListener("click", function(event) {
  console.log("Clicked!", event.target);
});

// Remove listener (must reference the same function)
function handleClick(e) {
  console.log("Clicked!");
}
button.addEventListener("click", handleClick);
button.removeEventListener("click", handleClick);

// Old way (only one handler per event)
button.onclick = function() { console.log("Clicked!"); };
```

### 36.2 The Event Object

```js
element.addEventListener("click", function(event) {
  event.target;         // The element that triggered the event
  event.currentTarget;  // The element the listener is attached to
  event.type;           // "click"
  event.timestamp;      // When the event occurred
  
  event.preventDefault();  // Prevent default behavior (e.g., form submit)
  event.stopPropagation(); // Stop the event from propagating
});
```

### 36.3 Event Propagation — Bubbling & Capturing

When you click an element, the event travels through the DOM in three phases:

```
Phase 1: CAPTURING (top → down)
    document → html → body → div → button
    
Phase 2: TARGET
    The actual element that was clicked
    
Phase 3: BUBBLING (bottom → up) [DEFAULT]
    button → div → body → html → document
```

```js
// Bubbling (default) — event goes UP from target to ancestors
document.querySelector("#parent").addEventListener("click", () => {
  console.log("Parent clicked (bubbling)");
});

document.querySelector("#child").addEventListener("click", () => {
  console.log("Child clicked");
});
// Clicking #child logs: "Child clicked", then "Parent clicked (bubbling)"

// Capturing — event goes DOWN from document to target
document.querySelector("#parent").addEventListener("click", () => {
  console.log("Parent clicked (capturing)");
}, true); // Third argument: useCapture = true

// Stopping propagation
document.querySelector("#child").addEventListener("click", (e) => {
  e.stopPropagation(); // Prevents the event from reaching the parent
  console.log("Only child");
});
```

---

## 37. Event Delegation

### 37.1 What is Event Delegation?

Instead of adding event listeners to **every child element**, you add **one listener to the parent** and use the `event.target` to determine which child was clicked.

```js
// ❌ Bad: One listener per item (memory-heavy, doesn't work for dynamic items)
document.querySelectorAll(".item").forEach(item => {
  item.addEventListener("click", () => {
    console.log(item.textContent);
  });
});

// ✅ Good: One listener on the parent (works for dynamic items too!)
document.querySelector("#item-list").addEventListener("click", (e) => {
  if (e.target.classList.contains("item")) {
    console.log(e.target.textContent);
  }
});
```

### 37.2 Why Use Event Delegation?

1. **Performance**: Fewer event listeners = less memory
2. **Dynamic elements**: Works with elements added to the DOM after the listener was attached
3. **Cleaner code**: One handler instead of many

```js
// Works even when items are added later
const list = document.querySelector("#todo-list");

list.addEventListener("click", (e) => {
  // Handle delete button clicks
  if (e.target.matches(".delete-btn")) {
    e.target.closest("li").remove();
  }
  
  // Handle toggle complete
  if (e.target.matches(".toggle-btn")) {
    e.target.closest("li").classList.toggle("completed");
  }
});

// Adding new items — the delegation still works!
function addTodo(text) {
  const li = document.createElement("li");
  li.innerHTML = `
    <span>${text}</span>
    <button class="toggle-btn">✓</button>
    <button class="delete-btn">✕</button>
  `;
  list.appendChild(li);
}
```

---

## 38. Web APIs & Browser APIs

### 38.1 Overview

Web APIs are provided by the **browser** (not JavaScript itself). They allow JS to interact with the browser environment.

### 38.2 Important Web APIs

```js
// ===== Console API =====
console.log("Regular log");
console.warn("Warning");
console.error("Error");
console.table([{ name: "A", age: 20 }, { name: "B", age: 25 }]);
console.time("timer"); /* ... */ console.timeEnd("timer"); // Measure time
console.group("Group"); console.log("Nested"); console.groupEnd();

// ===== Geolocation API =====
navigator.geolocation.getCurrentPosition(
  (position) => {
    console.log(position.coords.latitude, position.coords.longitude);
  },
  (error) => console.error(error)
);

// ===== Intersection Observer (visibility tracking) =====
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add("visible");
      observer.unobserve(entry.target); // Stop observing
    }
  });
}, { threshold: 0.5 }); // 50% visible

document.querySelectorAll(".lazy-load").forEach(el => observer.observe(el));

// ===== Resize Observer =====
const resizeObserver = new ResizeObserver(entries => {
  entries.forEach(entry => {
    console.log("New size:", entry.contentRect.width, entry.contentRect.height);
  });
});
resizeObserver.observe(document.querySelector("#container"));

// ===== Clipboard API =====
async function copyText(text) {
  await navigator.clipboard.writeText(text);
  console.log("Copied!");
}

// ===== History API =====
history.pushState({ page: 2 }, "Page 2", "/page2");
history.replaceState({ page: 3 }, "Page 3", "/page3");
window.addEventListener("popstate", (e) => {
  console.log("Back/forward button pressed:", e.state);
});
```

---

## 39. Storage — localStorage, sessionStorage, Cookies

### 39.1 localStorage

Data persists **even after the browser is closed**. Same-origin only.

```js
// Store data (only strings!)
localStorage.setItem("user", JSON.stringify({ name: "Mukund", age: 22 }));
localStorage.setItem("theme", "dark");

// Retrieve data
const user = JSON.parse(localStorage.getItem("user"));
const theme = localStorage.getItem("theme"); // "dark"

// Remove
localStorage.removeItem("theme");
localStorage.clear(); // Remove everything

// Storage limit: ~5-10 MB per origin
```

### 39.2 sessionStorage

Same API as localStorage, but data is cleared when the **tab/window is closed**.

```js
sessionStorage.setItem("sessionId", "abc123");
sessionStorage.getItem("sessionId"); // "abc123"
// Closed tab → data gone
```

### 39.3 Cookies

```js
// Set cookie
document.cookie = "username=Mukund; expires=Thu, 31 Dec 2026 23:59:59 GMT; path=/";
document.cookie = "theme=dark; max-age=86400"; // Expires in 24 hours

// Read all cookies (returns single string)
console.log(document.cookie); // "username=Mukund; theme=dark"

// Delete cookie (set expiry in the past)
document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/";
```

### 39.4 Comparison

| Feature | localStorage | sessionStorage | Cookies |
|---------|-------------|---------------|---------|
| Capacity | ~5-10 MB | ~5-10 MB | ~4 KB |
| Lifetime | Until manually cleared | Until tab/window closes | Set by expiry date |
| Sent to server? | ❌ No | ❌ No | ✅ Yes (with every HTTP request) |
| Accessible from | Same origin | Same origin, same tab | Same origin (configurable) |
| API | Simple (get/set) | Simple (get/set) | Complex (string parsing) |

---

## 40. Fetch API & AJAX

### 40.1 The Fetch API

```js
// Basic GET request
const response = await fetch("https://api.example.com/users");
const data = await response.json();

// POST request
const response = await fetch("https://api.example.com/users", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "Authorization": "Bearer token123"
  },
  body: JSON.stringify({
    name: "Mukund",
    age: 22
  })
});

// Handle response
if (!response.ok) {
  throw new Error(`HTTP error! Status: ${response.status}`);
}
const result = await response.json();
```

### 40.2 Response Methods

```js
response.json();   // Parse as JSON → Promise<Object>
response.text();   // Parse as text → Promise<String>
response.blob();   // Parse as Blob → Promise<Blob> (for files)
response.arrayBuffer(); // Raw binary data
response.formData();    // Form data
```

### 40.3 AbortController (Cancel Requests)

```js
const controller = new AbortController();

fetch("https://api.example.com/data", {
  signal: controller.signal
})
  .then(res => res.json())
  .catch(err => {
    if (err.name === "AbortError") {
      console.log("Request was cancelled");
    }
  });

// Cancel the request
controller.abort();
```

### 40.4 Error Handling Pattern

```js
async function fetchWithHandling(url, options = {}) {
  try {
    const response = await fetch(url, options);
    
    // fetch does NOT reject on HTTP errors (404, 500, etc.)
    // You must check response.ok
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }
    
    return await response.json();
  } catch (error) {
    if (error instanceof TypeError) {
      console.error("Network error:", error.message);
    } else {
      console.error("Request failed:", error.message);
    }
    throw error;
  }
}
```

---

## 41. Modules (ES Modules & CommonJS)

### 41.1 ES Modules (ESM) — Modern Standard

```js
// ===== math.js (exporting) =====

// Named exports
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export function subtract(a, b) { return a - b; }

// Default export (one per module)
export default function multiply(a, b) { return a * b; }


// ===== app.js (importing) =====

// Import default
import multiply from "./math.js";

// Import named
import { PI, add, subtract } from "./math.js";

// Import with alias
import { add as addition } from "./math.js";

// Import all
import * as math from "./math.js";
math.add(1, 2);
math.PI;

// Import default + named together
import multiply, { add, PI } from "./math.js";
```

### 41.2 Dynamic Import (Code Splitting)

```js
// Load module only when needed
async function loadChart() {
  const { Chart } = await import("./chart.js"); // Returns a Promise
  const chart = new Chart();
  chart.render();
}

button.addEventListener("click", loadChart);
```

### 41.3 CommonJS (Node.js — Legacy)

```js
// ===== math.js (exporting) =====
module.exports = {
  add: (a, b) => a + b,
  PI: 3.14
};
// OR
exports.add = (a, b) => a + b;

// ===== app.js (importing) =====
const math = require("./math.js");
const { add, PI } = require("./math.js");
```

### 41.4 ESM vs CommonJS

| Feature | ES Modules | CommonJS |
|---------|-----------|----------|
| Syntax | `import/export` | `require/module.exports` |
| Loading | Static (at parse time) | Dynamic (at runtime) |
| Top-level `await` | ✅ Supported | ❌ Not supported |
| Tree shaking | ✅ Supported (dead code elimination) | ❌ Not supported |
| Browser support | ✅ Native | ❌ Needs bundler |
| Used in | Modern JS, browsers | Node.js (legacy) |

---

## 42. Polyfills — Writing Your Own

### 42.1 What is a Polyfill?

A polyfill is code that **implements a feature on environments that don't support it natively**. Writing polyfills is a common interview exercise that tests your deep understanding of built-in methods.

### 42.2 Polyfill for `Array.prototype.map`

```js
if (!Array.prototype.myMap) {
  Array.prototype.myMap = function(callback, thisArg) {
    if (typeof callback !== "function") {
      throw new TypeError(callback + " is not a function");
    }
    
    const result = [];
    for (let i = 0; i < this.length; i++) {
      if (i in this) { // Skip holes in sparse arrays
        result.push(callback.call(thisArg, this[i], i, this));
      }
    }
    return result;
  };
}

// Test
[1, 2, 3].myMap(x => x * 2); // [2, 4, 6]
```

### 42.3 Polyfill for `Array.prototype.filter`

```js
Array.prototype.myFilter = function(callback, thisArg) {
  if (typeof callback !== "function") {
    throw new TypeError(callback + " is not a function");
  }
  
  const result = [];
  for (let i = 0; i < this.length; i++) {
    if (i in this && callback.call(thisArg, this[i], i, this)) {
      result.push(this[i]);
    }
  }
  return result;
};

[1, 2, 3, 4, 5].myFilter(x => x > 3); // [4, 5]
```

### 42.4 Polyfill for `Array.prototype.reduce`

```js
Array.prototype.myReduce = function(callback, initialValue) {
  if (typeof callback !== "function") {
    throw new TypeError(callback + " is not a function");
  }
  
  let accumulator;
  let startIndex;
  
  if (arguments.length >= 2) {
    accumulator = initialValue;
    startIndex = 0;
  } else {
    if (this.length === 0) {
      throw new TypeError("Reduce of empty array with no initial value");
    }
    accumulator = this[0];
    startIndex = 1;
  }
  
  for (let i = startIndex; i < this.length; i++) {
    if (i in this) {
      accumulator = callback(accumulator, this[i], i, this);
    }
  }
  
  return accumulator;
};

[1, 2, 3, 4].myReduce((acc, cur) => acc + cur, 0); // 10
```

### 42.5 Polyfill for `Function.prototype.bind`

```js
Function.prototype.myBind = function(context, ...args) {
  if (typeof this !== "function") {
    throw new TypeError("Bind must be called on a function");
  }
  
  const fn = this;
  
  return function bound(...laterArgs) {
    // Support 'new' operator on bound function
    if (new.target) {
      return new fn(...args, ...laterArgs);
    }
    return fn.apply(context, [...args, ...laterArgs]);
  };
};

function greet(greeting) {
  return `${greeting}, ${this.name}`;
}
const boundGreet = greet.myBind({ name: "Mukund" });
boundGreet("Hello"); // "Hello, Mukund"
```

### 42.6 Polyfill for `Promise.all`

```js
Promise.myAll = function(promises) {
  return new Promise((resolve, reject) => {
    if (!Array.isArray(promises)) {
      return reject(new TypeError("Argument must be an array"));
    }
    
    const results = [];
    let resolvedCount = 0;
    
    if (promises.length === 0) {
      return resolve(results);
    }
    
    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then(value => {
          results[index] = value; // Preserve order
          resolvedCount++;
          if (resolvedCount === promises.length) {
            resolve(results);
          }
        })
        .catch(reject); // First rejection rejects the whole thing
    });
  });
};
```

### 42.7 Polyfill for `Array.prototype.flat`

```js
Array.prototype.myFlat = function(depth = 1) {
  const result = [];
  
  const flatten = (arr, d) => {
    for (const item of arr) {
      if (Array.isArray(item) && d > 0) {
        flatten(item, d - 1);
      } else {
        result.push(item);
      }
    }
  };
  
  flatten(this, depth);
  return result;
};

[1, [2, [3, [4]]]].myFlat(2); // [1, 2, 3, [4]]
[1, [2, [3, [4]]]].myFlat(Infinity); // [1, 2, 3, 4]
```

---

## 43. Strict Mode

### 43.1 What is Strict Mode?

Strict mode is a way to opt into a restricted variant of JavaScript. It catches common coding mistakes and throws errors for things that would normally fail silently.

```js
"use strict"; // At the top of a file or function

// OR inside a function
function strict() {
  "use strict";
  // strict mode applies here
}
```

> **Note**: ES modules (`import`/`export`) are always in strict mode.

### 43.2 What Strict Mode Prevents

```js
"use strict";

// 1. No undeclared variables
x = 10; // ❌ ReferenceError (without strict mode, this creates a global)

// 2. No duplicate parameter names
function add(a, a) { } // ❌ SyntaxError

// 3. No deleting undeletable properties
delete Object.prototype; // ❌ TypeError

// 4. No octal literals
let octal = 010; // ❌ SyntaxError (use 0o10 for octal)

// 5. No writing to read-only properties
const obj = {};
Object.defineProperty(obj, "x", { value: 1, writable: false });
obj.x = 2; // ❌ TypeError

// 6. 'this' is undefined in standalone functions (not window)
function showThis() {
  console.log(this); // undefined (not window)
}

// 7. No 'with' statement
// with (obj) { } // ❌ SyntaxError

// 8. 'eval' doesn't create variables in the surrounding scope
eval("var evalVar = 10");
// console.log(evalVar); // ❌ ReferenceError in strict mode
```

---

## 44. Memory Management & Garbage Collection

### 44.1 How Memory Works in JS

JavaScript manages memory automatically through **garbage collection**. You don't manually allocate or free memory, but you need to understand how it works to avoid memory leaks.

```
┌──────────────────────────────────────────┐
│ STACK                                     │
│ • Primitives (number, string, boolean)   │
│ • References (pointers to heap objects)   │
│ • Function call frames                    │
│ • Fast, fixed-size, auto-managed          │
└──────────────────────────────────────────┘

┌──────────────────────────────────────────┐
│ HEAP                                      │
│ • Objects, arrays, functions             │
│ • Dynamic size                            │
│ • Managed by garbage collector            │
└──────────────────────────────────────────┘
```

### 44.2 Garbage Collection — Mark and Sweep

The most common algorithm:
1. **Mark**: Start from "roots" (global object, currently executing functions), traverse all reachable objects, and mark them.
2. **Sweep**: Delete all unmarked (unreachable) objects.

```js
let user = { name: "Mukund" }; // Object is reachable via 'user'
user = null; // Object is now unreachable → garbage collected

// Circular reference — still GC'd by mark-and-sweep
let a = {};
let b = {};
a.ref = b;
b.ref = a;
a = null;
b = null;
// Both objects are unreachable from roots → collected
```

### 44.3 Common Memory Leaks

```js
// 1. Forgotten event listeners
const element = document.querySelector("#button");
element.addEventListener("click", heavyHandler);
element.remove(); // Element is removed, but listener still holds a reference!
// Fix: element.removeEventListener("click", heavyHandler); BEFORE removing

// 2. Closures holding references
function createLeak() {
  const largeData = new Array(1000000).fill("x");
  return function() {
    // Even if you don't use largeData, the closure holds a reference
    console.log("Hello");
  };
}
const leaked = createLeak(); // largeData stays in memory

// 3. Global variables
function accident() {
  leakedVar = "I'm global!"; // Missing 'let/const' creates a global
}

// 4. Forgotten timers
const intervalId = setInterval(() => {
  // This runs forever if not cleared
  doSomething();
}, 1000);
// Fix: clearInterval(intervalId) when no longer needed

// 5. Detached DOM elements
const list = document.querySelector("#list");
const items = document.querySelectorAll("li"); // Holds reference to all LIs
list.remove(); // LIs are still referenced by 'items' → not GC'd
```

### 44.4 WeakRef and FinalizationRegistry (Advanced)

```js
// WeakRef — holds a weak reference to an object
let obj = { data: "important" };
const weakRef = new WeakRef(obj);

console.log(weakRef.deref()); // { data: "important" }
obj = null; // Object may be garbage collected

// Later...
console.log(weakRef.deref()); // undefined (if GC'd) or the object (if still alive)

// FinalizationRegistry — callback when object is GC'd
const registry = new FinalizationRegistry((heldValue) => {
  console.log(`Object with id ${heldValue} was garbage collected`);
});

let myObj = { name: "test" };
registry.register(myObj, "obj-1");
myObj = null; // Eventually logs: "Object with id obj-1 was garbage collected"
```

---

## 45. Design Patterns in JavaScript

### 45.1 Module Pattern

Encapsulates "privacy" using closures (pre-ES6 modules):

```js
const Counter = (function() {
  let count = 0; // Private
  
  return {
    increment() { count++; },
    decrement() { count--; },
    getCount() { return count; }
  };
})();

Counter.increment();
Counter.increment();
Counter.getCount(); // 2
// Counter.count;    // undefined — private!
```

### 45.2 Singleton Pattern

Ensures only one instance of a class exists:

```js
class Database {
  constructor() {
    if (Database.instance) {
      return Database.instance;
    }
    this.connection = "connected";
    Database.instance = this;
  }
  
  query(sql) {
    console.log(`Executing: ${sql}`);
  }
}

const db1 = new Database();
const db2 = new Database();
console.log(db1 === db2); // true — same instance
```

### 45.3 Observer Pattern (Pub/Sub)

Allows objects to subscribe to and receive notifications about events:

```js
class EventEmitter {
  constructor() {
    this.events = {};
  }
  
  on(event, callback) {
    if (!this.events[event]) {
      this.events[event] = [];
    }
    this.events[event].push(callback);
    return this; // For chaining
  }
  
  emit(event, ...args) {
    if (this.events[event]) {
      this.events[event].forEach(cb => cb(...args));
    }
    return this;
  }
  
  off(event, callback) {
    if (this.events[event]) {
      this.events[event] = this.events[event].filter(cb => cb !== callback);
    }
    return this;
  }
  
  once(event, callback) {
    const wrapper = (...args) => {
      callback(...args);
      this.off(event, wrapper);
    };
    this.on(event, wrapper);
    return this;
  }
}

const emitter = new EventEmitter();
emitter.on("message", (data) => console.log("Received:", data));
emitter.emit("message", "Hello!"); // "Received: Hello!"
```

### 45.4 Factory Pattern

Creates objects without exposing instantiation logic:

```js
function createUser(type) {
  switch (type) {
    case "admin":
      return { role: "admin", permissions: ["read", "write", "delete"] };
    case "editor":
      return { role: "editor", permissions: ["read", "write"] };
    case "viewer":
      return { role: "viewer", permissions: ["read"] };
    default:
      throw new Error(`Unknown user type: ${type}`);
  }
}

const admin = createUser("admin");
const viewer = createUser("viewer");
```

### 45.5 Proxy Pattern

Intercept and customize operations on objects:

```js
const user = {
  name: "Mukund",
  age: 22,
  _password: "secret" // Convention: _ means "private"
};

const protectedUser = new Proxy(user, {
  get(target, prop) {
    if (prop.startsWith("_")) {
      throw new Error(`Access denied to private property: ${prop}`);
    }
    return target[prop];
  },
  
  set(target, prop, value) {
    if (prop === "age" && typeof value !== "number") {
      throw new TypeError("Age must be a number");
    }
    target[prop] = value;
    return true;
  }
});

protectedUser.name;      // "Mukund" ✅
// protectedUser._password; // ❌ Error: Access denied
protectedUser.age = 25;  // ✅
// protectedUser.age = "old"; // ❌ TypeError: Age must be a number
```

---

## 46. JavaScript Engine Internals (V8)

### 46.1 How V8 (Chrome's JS Engine) Works

```
Source Code
    │
    ▼
┌──────────────┐
│   PARSER     │  → Tokenizes and creates Abstract Syntax Tree (AST)
└──────┬───────┘
       ▼
┌──────────────┐
│  IGNITION    │  → Interpreter: converts AST to bytecode
│ (Interpreter)│     Fast startup, slower execution
└──────┬───────┘
       │
       │  (Identifies "hot" functions — frequently called)
       ▼
┌──────────────┐
│ TURBOFAN     │  → JIT Compiler: compiles hot bytecode to optimized machine code
│ (Compiler)   │     Slow compilation, very fast execution
└──────────────┘
```

### 46.2 Key Optimizations

**Hidden Classes (Shapes/Maps)**:
V8 creates hidden classes for objects to optimize property access:

```js
// ✅ Good: Objects with same property order share hidden class
function Point(x, y) {
  this.x = x; // Hidden class C0 → C1
  this.y = y; // Hidden class C1 → C2
}
const p1 = new Point(1, 2); // Uses shared hidden class C2
const p2 = new Point(3, 4); // Uses same hidden class C2 → fast!

// ❌ Bad: Different property orders create different hidden classes
const a = { x: 1, y: 2 };
const b = { y: 2, x: 1 }; // Different hidden class → slower property access
```

**Inline Caching**:
V8 caches the result of property lookups. If the same type of object always has the same hidden class, property access becomes extremely fast.

**Deoptimization**:
If V8 optimizes a function based on assumptions (e.g., "this argument is always a number"), and those assumptions are violated, it "deoptimizes" — throwing away the optimized code and going back to interpreted bytecode.

```js
// ❌ This causes deoptimization
function add(a, b) { return a + b; }
add(1, 2);       // V8 optimizes for numbers
add("a", "b");   // Different types → deoptimization!
```

### 46.3 Writing V8-Friendly Code

1. **Use consistent object shapes** — initialize all properties in the constructor
2. **Don't add/delete properties dynamically** after creation
3. **Use consistent types** for function arguments
4. **Avoid `arguments`** — use rest parameters instead
5. **Avoid `eval()` and `with`** — they prevent optimization

---

## 47. Output-Based Tricky Questions — Patterns & Gotchas

These test your deep understanding of JS concepts. Here are the patterns you need to recognize:

### 47.1 Hoisting + Temporal Dead Zone

```js
var x = 1;
function foo() {
  console.log(x); // undefined (not 1!) — local 'var x' is hoisted
  var x = 2;
  console.log(x); // 2
}
foo();
```

```js
let x = 1;
{
  // console.log(x); // ❌ ReferenceError — TDZ
  let x = 2;
}
```

### 47.2 Closures in Loops

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
// Output: 3, 3, 3 (not 0, 1, 2)

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
// Output: 0, 1, 2
```

### 47.3 `this` in Different Contexts

```js
const obj = {
  name: "Mukund",
  getName: function() { return this.name; },
  getNameArrow: () => this.name
};

console.log(obj.getName());      // "Mukund"
console.log(obj.getNameArrow()); // undefined (arrow function → lexical this → window)

const fn = obj.getName;
console.log(fn()); // undefined (lost implicit binding)
```

### 47.4 Event Loop Ordering

```js
console.log("A");
setTimeout(() => console.log("B"), 0);
Promise.resolve().then(() => console.log("C"));
setTimeout(() => console.log("D"), 0);
Promise.resolve().then(() => console.log("E"));
console.log("F");

// Output: A, F, C, E, B, D
// Sync → Microtasks → Macrotasks
```

### 47.5 Type Coercion Traps

```js
console.log([] == false);     // true ([].toString() → "" → 0, false → 0)
console.log([] == ![]);       // true (![]=false → [] == false → 0 == 0)
console.log("" == false);     // true (both coerce to 0)
console.log(null == undefined); // true
console.log(null === undefined);// false
console.log(typeof null);     // "object" (historical bug)
console.log(typeof NaN);      // "number"
console.log(NaN === NaN);     // false

console.log(0.1 + 0.2 === 0.3); // false (IEEE 754 floating point)
console.log(0.1 + 0.2);         // 0.30000000000000004
```

### 47.6 Reference vs Value

```js
let a = [1, 2, 3];
let b = a;
b.push(4);
console.log(a); // [1, 2, 3, 4] — same reference

let x = { value: 10 };
let y = x;
y = { value: 20 };     // y now points to a NEW object
console.log(x.value);  // 10 — x is unchanged (we didn't mutate, we reassigned)
```

### 47.7 Promise Microtask Ordering

```js
Promise.resolve()
  .then(() => {
    console.log(1);
    return Promise.resolve(2);
  })
  .then(val => console.log(val));

Promise.resolve()
  .then(() => console.log(3))
  .then(() => console.log(4));

// Output: 1, 3, 4, 2
// Why? Returning Promise.resolve(2) adds extra microtasks for unwrapping
```

---

## 48. Machine Coding Essentials

These are practical implementations frequently asked in coding interviews.

### 48.1 Implement debounce (with leading/trailing)

*(Covered in Section 34)*

### 48.2 Implement throttle

*(Covered in Section 34)*

### 48.3 Deep Clone

*(Covered in Section 27)*

### 48.4 Flatten Array/Object

```js
// Flatten nested array
function flattenArray(arr, depth = Infinity) {
  return arr.reduce((acc, val) => {
    if (Array.isArray(val) && depth > 0) {
      acc.push(...flattenArray(val, depth - 1));
    } else {
      acc.push(val);
    }
    return acc;
  }, []);
}

// Flatten nested object
function flattenObject(obj, prefix = "") {
  const result = {};
  
  for (const [key, value] of Object.entries(obj)) {
    const newKey = prefix ? `${prefix}.${key}` : key;
    
    if (typeof value === "object" && value !== null && !Array.isArray(value)) {
      Object.assign(result, flattenObject(value, newKey));
    } else {
      result[newKey] = value;
    }
  }
  
  return result;
}

flattenObject({ a: { b: { c: 1 } }, d: 2 });
// { "a.b.c": 1, "d": 2 }
```

### 48.5 Implement Promise.allSettled

```js
function promiseAllSettled(promises) {
  return Promise.all(
    promises.map(p =>
      Promise.resolve(p)
        .then(value => ({ status: "fulfilled", value }))
        .catch(reason => ({ status: "rejected", reason }))
    )
  );
}
```

### 48.6 Implement a Simple Event Emitter

*(Covered in Section 45.3)*

### 48.7 Implement Retry with Exponential Backoff

```js
async function retry(fn, maxRetries = 3, baseDelay = 1000) {
  for (let attempt = 0; attempt <= maxRetries; attempt++) {
    try {
      return await fn();
    } catch (error) {
      if (attempt === maxRetries) {
        throw error;
      }
      const delay = baseDelay * Math.pow(2, attempt); // 1s, 2s, 4s...
      console.log(`Attempt ${attempt + 1} failed. Retrying in ${delay}ms...`);
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
}

// Usage
await retry(() => fetch("/api/flaky-endpoint").then(r => {
  if (!r.ok) throw new Error("Failed");
  return r.json();
}));
```

### 48.8 Implement compose and pipe

```js
// compose: right to left
const compose = (...fns) => (x) => fns.reduceRight((acc, fn) => fn(acc), x);

// pipe: left to right
const pipe = (...fns) => (x) => fns.reduce((acc, fn) => fn(acc), x);

// Usage
const add10 = x => x + 10;
const double = x => x * 2;
const square = x => x * x;

const composed = compose(square, double, add10);
composed(5); // square(double(add10(5))) = square(double(15)) = square(30) = 900

const piped = pipe(add10, double, square);
piped(5); // square(double(add10(5))) = same result = 900
```

### 48.9 Implement `once`

```js
function once(fn) {
  let called = false;
  let result;
  
  return function(...args) {
    if (!called) {
      called = true;
      result = fn.apply(this, args);
    }
    return result;
  };
}

const initialize = once(() => {
  console.log("Initializing...");
  return { ready: true };
});

initialize(); // "Initializing..." → { ready: true }
initialize(); // No log → { ready: true } (returns cached result)
```

### 48.10 Implement `memoize`

*(Covered in Section 11.4)*

---

## 🎯 Mastery Checklist

Use this checklist to track your progress. You should be able to **explain each topic and write code for it without looking anything up**:

- [ ] Execution Context & Call Stack
- [ ] var, let, const (scope, hoisting, TDZ)
- [ ] Data Types & typeof quirks
- [ ] Type Coercion rules & falsy values
- [ ] == vs === (Abstract Equality Algorithm)
- [ ] Logical operators & short-circuit evaluation
- [ ] Nullish coalescing (??) vs OR (||)
- [ ] Functions (declarations, expressions, IIFE, pure functions)
- [ ] Scope chain & lexical scoping
- [ ] Hoisting (functions, var, let/const)
- [ ] Closures (loops, data privacy, memoization)
- [ ] `this` keyword (4 rules of binding)
- [ ] call, apply, bind (+ write polyfill for bind)
- [ ] Arrow functions vs regular functions
- [ ] Higher-order functions (map, filter, reduce)
- [ ] Currying & partial application
- [ ] Object methods, property descriptors, getters/setters
- [ ] Prototypal inheritance & prototype chain
- [ ] ES6 Classes, extends, super, private fields
- [ ] Array methods (mutating vs non-mutating)
- [ ] Destructuring, Spread, Rest
- [ ] Template literals & tagged templates
- [ ] Symbols & well-known symbols
- [ ] Iterators & Generators
- [ ] Map, Set, WeakMap, WeakSet
- [ ] Shallow copy vs Deep copy
- [ ] Error handling (custom errors, async error handling)
- [ ] Event Loop (microtask vs macrotask queue)
- [ ] Callbacks & callback hell
- [ ] Promises (all, allSettled, race, any)
- [ ] async/await (parallel execution, loops)
- [ ] Timers & requestAnimationFrame
- [ ] Debouncing & Throttling
- [ ] DOM manipulation & traversal
- [ ] Event bubbling, capturing, delegation
- [ ] Web APIs (Intersection Observer, Clipboard, History)
- [ ] Storage (localStorage, sessionStorage, cookies)
- [ ] Fetch API & AbortController
- [ ] ES Modules vs CommonJS
- [ ] Polyfills (map, filter, reduce, bind, Promise.all, flat)
- [ ] Strict mode
- [ ] Memory management & garbage collection
- [ ] Design patterns (Module, Singleton, Observer, Factory, Proxy)
- [ ] V8 engine internals (hidden classes, JIT compilation)
- [ ] Output-based question patterns
- [ ] Machine coding implementations

---

> **Last updated**: June 2026
>
> **Pro tip**: Don't just read — open your browser console and **type every code example**. Understanding comes from doing, not reading.
