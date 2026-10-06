---
layout: minimal
title: "JavaScript Master Interview Manual"
permalink: /
---

![JavaScript Master Interview Manual Banner](assets/banner.png)

<p align="center">
  <b><a href="https://github.com/Priyammondal/javascript-interview-manual">💻 GitHub Repository</a></b> | 
  <b><a href="https://github.com/Priyammondal/javascript-interview-manual/issues">🪲 Report an Anomaly (Issues)</a></b> | 
  <b><a href="https://github.com/Priyammondal/javascript-interview-manual/pulls">🤝 Open a Pull Request</a></b>
</p>

<p align="center">
  <img src="https://badgen.net/github/stars/Priyammondal/javascript-interview-manual?icon=github&color=yellow&label=STARS" alt="GitHub Stars">
  <img src="https://badgen.net/github/forks/Priyammondal/javascript-interview-manual?icon=github&color=orange&label=FORKS" alt="GitHub Forks">
  <img src="https://badgen.net/github/last-commit/Priyammondal/javascript-interview-manual?icon=github&color=green&label=LAST%20COMMIT" alt="Last Commit">
  <img src="https://img.shields.io/badge/PRs-welcome-blueviolet?style=flat" alt="PRs Welcome">
  <img src="https://img.shields.io/badge/License-MIT-blue?style=flat" alt="MIT License">
</p>

## Table of Contents
* [History, Engines, and Runtimes of JavaScript](#history-engines-and-runtimes-of-javascript)
* [Integrating JavaScript into HTML](#integrating-javascript-into-html)
* [Variables and Scopes in JavaScript](#variables-and-scopes-in-javascript)
* [Lexical Scope vs. Closure](#lexical-scope-vs-closure)
* [The Two Phases of Code Execution](#the-two-phases-of-code-execution)
* [Hoisting and the Temporal Dead Zone (TDZ)](#hoisting-and-the-temporal-dead-zone-tdz)
* [JavaScript Execution Pipeline](#javascript-execution-pipeline)
* [var vs. let vs. const](#var-vs-let-vs-const)
* [Data Types in JavaScript](#data-types-in-javascript)
* [Advanced Collections: Map and Set](#advanced-collections-map-and-set)
* [Memory Management: WeakMap and WeakSet](#memory-management-weakmap-and-weakset)
* [The typeof Operator Quirks](#the-typeof-operator-quirks)
* [Type Conversions](#type-conversions)
* [Basic Operators](#basic-operators)
* [Conditional Operators](#conditional-operators)
* [Logical Operators](#logical-operators)
* [Statements vs. Expressions](#statements-vs-expressions)
* [The switch Statement](#the-switch-statement)
* [Loops](#loops)
* [Functions](#functions)
* [Modern Class Architecture](#modern-class-architecture)
* [Objects](#objects)
* [The "this" Keyword in Objects](#the-this-keyword-in-objects)
* [Property Descriptors and Object Mutability](#property-descriptors-and-object-mutability)
* [Prototype and Prototypal Inheritance](#prototype-and-prototypal-inheritance)
* [Symbol Type](#symbol-type)
* [Metaprogramming: Proxy and Reflect](#metaprogramming-proxy-and-reflect)
* [Asynchronous JavaScript and the Event Loop](#asynchronous-javascript-and-the-event-loop)
* [Asynchronous Patterns: .then() vs. async/await](#asynchronous-patterns-then-vs-asyncawait)
* [Advanced JavaScript Error Handling](#advanced-javascript-error-handling)
    

# History, Engines, and Runtimes of JavaScript

### ⏳ The Evolutionary Timeline
* **1995** – JavaScript was invented by **Brendan Eich** while working at Netscape. It was initially named **Mocha**, briefly changed to **LiveScript**, and finally settled on **JavaScript**.
* **1997** – Developed originally for Netscape 2, it was submitted to the ECMA committee to standardize the language so it wouldn’t be locked down by a single company. This created the **ECMA-262 standard (ES1)**.
* **The Rebirth** – Internet Explorer (IE4) was the first browser to support ES1. When Netscape eventually collapsed, its engineers founded the **Mozilla Project**, inheriting the original codebase to continue evolving JavaScript through Firefox.

### 📅 Major ECMAScript Edition Milestones
* **1997** – ES1 *(The initial standardized baseline)*
* **2009** – ES5 *(Introduced strict mode and native JSON parsing)*
* **2015** – ES6 *(The largest overhaul: classes, arrow functions, promises, let/const)*
* **2023** – ES14 *(Introduced modern change-by-copy array methods)*
* **2025** – ES16 *(The latest iteration optimizing synchronization and pipelines)*

> 🔗 For a deep chronological breakdown, see the full [Wikipedia ECMAScript Version History](https://en.wikipedia.org/wiki/ECMAScript_version_history).

---

### 🚀 Core JavaScript Engines
Initially, JavaScript was built strictly to run inside web browsers. Different browsers use their own dedicated scripting engines to interpret and execute the code:

* **V8** ──> Google Chrome, Microsoft Edge, Opera
* **SpiderMonkey** ──> Mozilla Firefox
* **JavaScriptCore** ──> Apple Safari

---

### 🛠️ Breaking Out of the Browser
In **2009**, developer **Ryan Dahl** took Google Chrome's high-performance open-source **V8 engine** and wrapped it in a native C++ platform layer. This created **Node.js**, a runtime environment that made it possible to run JavaScript outside the browser entirely (on servers and local machines).

# Integrating JavaScript into HTML

## The 2 Methods of Inclusion

You can introduce JavaScript into an HTML document using two distinct structural approaches:

### 1. Internal JavaScript

Code is written directly inside the HTML file, encapsulated within `<script></script>` tags. The browser allows **multiple script tags** to be declared and executed within the same HTML file.

```
<script>
  console.log("This is internal JavaScript executing inside the HTML file.");
</script>
```

### 2. External JavaScript

Code is completely decoupled into a standalone `.js` file. It is linked back to the HTML document by utilizing the `src` (source) attribute on the script tag.

```
<script src="path/to/script.js"></script>
```

## Performance Optimization: Script Loading Attributes

When using External JS, you will often see two crucial attributes used to control how and when the script loads so it doesn't block the browser from rendering the HTML page:

### 1. `async`

-   **Behavior:** The script downloads in the background and runs the exact moment it finishes downloading. The EXACT moment download completes, the HTML parser halts, the script executes immediately, and then HTML parsing resumes.
    
-   **Warning:** Executed completely out of chronological order. Can block DOM parsing mid-way. _(Great for independent scripts like analytics)._
    

### 2. `defer`

-   **Behavior:** The script downloads in the background but waits to execute until the entire HTML document has finished parsing.
    
-   **Guarantees:** Scripts execute in the exact chronological order they are typed in the file. _(Best for scripts that rely on the HTML structure being fully loaded)._
# Variables and Scopes in JavaScript

We can declare a variable using three different keywords: "var", "let", "const"

## 6 Types of Scope

Scope determines the accessibility (visibility) of variables, functions, and objects.

### Global Scope

A variable is global if it is accessible anywhere in your entire application. Any variable declared outside of a function or block using var, or functions declared normally, are placed in the Global Scope. In non-module browser scripts, global var variables automatically become properties of the window object (e.g., window.myVar).

### Function Scope (Local Scope)

Any variable declared inside a function (whether using var, let, or const) is local to that function. It cannot be accessed outside the function.

### Block Scope

Introduced in ES6 (2015) with let and const. A block is defined by curly braces {} (like in if statements, for loops, or standalone braces). let and const are block-scoped. They cannot escape the {}. var is NOT block-scoped. If you declare a var inside an if block, it leaks out into the surrounding function or global scope.

### Script Scope

This sits right below the Global scope. When you use let or const at the very top level of a traditional HTML , they enter the Script Scope. They are still accessible from anywhere in that script file, but they do not attach to the window object. This prevents global namespace pollution.

### Module Scope

When your script tag uses type="module". Code inside a module is strictly isolated. A variable declared at the top level of a module is only available inside that module unless it is explicitly exported and imported into another file. Modules run in "strict mode" by default, and top-level var declarations here do not become global.

### Closure

When a function is being returned from another function, then it forms a closure which is the direct impact of lexical scope. Because of this the returned/inner function can access all the variables defined in the parent fn even after the parent fn got executed and removed from call stack.

# Lexical Scope vs. Closure

### Lexical Scope (Static Scope)

The rule that a function's scope is determined by where the function was written in the source code, not where it is called. Inner functions can look "upward" into parent scopes.

### Closure

The combination of a function and the lexical environment within which it was declared. It is the "backpack" of data that a returned inner function carries with it, keeping the outer function's variables alive in memory even after the outer function has finished executing.

# The Two Phases of Code Execution

In JavaScript, code execution happens in two main phases every time a script runs:

## 1. Memory Creation Phase (Creation Phase / Hoisting Phase)

Before a single line of code runs, JavaScript creates the Global Execution Context (GEC) and sets up the Variable Environment (Memory Component).

-   **Variable Declared with `var`:** Hoisted and automatically initialized with `undefined`.
    
-   **Variable Declared with `let` & `const`:** Hoisted, but trapped in the Temporal Dead Zone as `<uninitialized>`.
    
-   **Function Declaration:** Fully hoisted along with its actual body contents.
    
-   **Function Expressions:** If you write a function as an expression (e.g., `var add = (x, y) => x + y;`), it is treated exactly like a variable. This means it will be initialized with `undefined` (or remain uninitialized if using `let`/`const`) during the memory phase.
    
-   **Objects:** Objects do not get special treatment in the memory phase. They are stored inside variables, so they follow the exact same rules as variables (`var obj;` is initialized as `undefined` in the creation phase; the actual object reference pointer is only assigned later during the execution phase).
    
-   **Classes:** Classes are hoisted, but they behave exactly like `let` and `const`. They are placed in memory but remain uninitialized in the Temporal Dead Zone (TDZ). Unlike a regular function declaration, you cannot instantiate a class before its actual definition line in the code.
    

## 2. Code Execution Phase (Execution Phase / Run-time Phase)

The Thread of Execution runs the code sequentially line-by-line, updating the values in the memory component as it goes.

### Example: Variable Assignment

```
var a = 10;
```

-   **Engine Action:** Updates the stored memory value of `a` from `undefined` to `10`.
    

### Example: Function Declaration

```
function add(x, y) { return x + y; }
```

-   **Engine Action:** Completely ignored during this phase because its structural definition was already handled during the memory creation phase.
    

# Hoisting and the Temporal Dead Zone (TDZ)

## Hoisting

Hoisting is a mechanism where variables and function declarations are moved to the top of the scope before the code executes.

### Note on Physical Movement

While we often say declarations are "moved to the top," the physical code doesn't move. The JavaScript engine simply scans the file and sets up memory references first.

That's why the below code won't give an error, but `undefined` will be printed on the console:

```
console.log(age); // undefined
var age = 24;
```

## Temporal Dead Zone (TDZ)

The Temporal Dead Zone (TDZ) is a term used to describe the state of a block-scoped variable (`let` or `const`) from the moment its block scope is entered until the moment the engine encounters its actual declaration line and initializes it.

If you attempt to access a variable while it is trapped in this zone, JavaScript will throw a `ReferenceError`. So, to prevent JavaScript from throwing such an error, you’ve got to remember to access your variables from outside the temporal dead zone.

### But where exactly does the TDZ begin and end?

TDZ is different for `var` in comparison to `const` and `let`.

### Example 1

```
{
  // 🟢 bestFood’s TDZ starts here (at the beginning of this block scope)
  // 🟡 bestFood’s TDZ continues...
  
  console.log(bestFood); // ❌ ReferenceError: Cannot access 'bestFood' before initialization
  
  // 🟡 bestFood’s TDZ continues...
  let bestFood = "Vegetable Fried Rice"; // 🛑 bestFood’s TDZ ends here!
  
  // ⚪ bestFood’s TDZ does not exist here anymore
  console.log(bestFood); // "Vegetable Fried Rice"
}
```

### Example 2

```
{
  // 🟢 TDZ starts here
  // 🟡 bestFood’s TDZ continues...
  
  let bestFood; // 🛑 bestFood’s TDZ ends here! (Engine initializes it to undefined)
  
  console.log(bestFood); // 🟢 Prints: undefined (Safe to invoke, TDZ is gone)
  bestFood = "Vegetable Fried Rice"; 
  console.log(bestFood); // 🟢 Prints: "Vegetable Fried Rice"
}
```

A `let` variable's TDZ ends the moment its declaration line is evaluated, even if you don't explicitly assign it a value right away. JavaScript will automatically initialize it to `undefined` at that exact milestone.

## How `var` Works Differently

Because `var` is hoisted and initialized with `undefined` at the exact same moment during the Memory Creation phase, it never experiences a TDZ.

```
{
  // ❌ bestFood has NO TDZ here. It is already initialized to undefined.
  console.log(bestFood); // 🟢 Prints: undefined
  
  var bestFood = "Vegetable Fried Rice"; // Code execution updates the value in memory
  
  console.log(bestFood); // 🟢 Prints: "Vegetable Fried Rice"
}
```

When the computer hoists a `var` variable, it automatically initializes the variable with the value `undefined`. In contrast, JavaScript does not initialize a `let` (or `const`) variable with any value whenever it hoists the variable. Instead, the variable remains dead and inaccessible.

Therefore, a `let` (or `const`) variable’s TDZ ends when JavaScript fully initializes it with the value specified during its declaration. However, `var` does not have a Temporal Dead Zone at all. Because a `var` variable is allocated memory and initialized to `undefined` simultaneously during the creation phase, it is instantly accessible. It never enters a "dead" zone.

## Classes and the TDZ

Classes follow the exact same rules as `let` and `const`. Under the hood, a class is hoisted during the Memory Creation phase, but it remains uninitialized, meaning it is trapped inside the Temporal Dead Zone (TDZ).

The TDZ for a class starts at the beginning of the block and ends only when the engine executes the actual class declaration line. If you try to instantiate (`new`) or access the class before that line, JavaScript will throw a `ReferenceError`.

```
{
  // 🟢 MyClass's TDZ starts here (beginning of block scope)
  
  // ❌ ReferenceError: Cannot access 'MyClass' before initialization
  const user = new MyClass("Alice"); 
  
  // 🟡 MyClass's TDZ continues...
  
  class MyClass { // 🛑 MyClass's TDZ ends here!
    constructor(name) {
      this.name = name;
    }
  }
  
  // ⚪ TDZ does not exist here anymore
  const user2 = new MyClass("Bob"); // 🟢 Works perfectly!
}
```

# JavaScript Execution Pipeline

The execution pipeline has evolved into a highly advanced multi-tiered system. Using Google's V8 (Chrome, Edge, Node.js) as the gold standard, this is exactly how your code travels from text to CPU:

Plaintext

```
[ JS Source Code ] 
       │
       ▼
   [ Parser ] ───► Generates Abstract Syntax Tree (AST) & Catches Syntax Errors
       │
       ▼
 1. [ Ignition ] ──► (Interpreter) Instantly turns AST to Bytecode & executes it.
       │              Gathers type feedback (Feedback Vectors).
       ▼
 2. [ Sparkplug ] ─► (Baseline JIT) Blazing-fast compilation. Skips optimization 
       │              entirely just to remove interpreter overhead.
       ▼
 3. [ Maglev ] ────► (Mid-Tier JIT) Quick, "good enough" optimization using 
       │              the gathered feedback vectors.
       ▼
 4. [ TurboFan ] ──► (Top-Tier JIT) High-investment, maximum optimization for the
                      absolute "hottest" loops and functions.

```

## Parsing Phase

The engine reads your raw source strings, tokens them, and constructs an Abstract Syntax Tree (AST).

This is where the engine runs Early Error checking. If you have a syntax error (like `const a = ;`), the pipeline aborts right here before executing anything.

## Bytecode Generation & Interpretation (Tier 1: Ignition)

The AST is converted into a stream of bytecode. V8's interpreter, Ignition, immediately starts executing this bytecode.

As Ignition executes the bytecode line-by-line, it tracks how the code behaves. It attaches a Feedback Vector to your functions, recording operational characteristics: "What data types are actually passing through this function? Numbers? Strings? Objects of a specific shape?"

## The JIT Compiler Escalation Ladder (Tiers 2, 3, and 4)

If a piece of code is executed frequently, it becomes "hot," and the engine starts scaling it up through the JIT layers to run natively on the CPU.

### Tier 2: Sparkplug (Baseline Compiler)

If a function runs more than a few times, Sparkplug takes the Ignition bytecode and compiles it directly into machine code almost instantly. It doesn't do any complex optimization; it just makes it run natively to save interpreter overhead.

### Tier 3: Maglev (Mid-Tier Optimizer)

If the code stays hot, Maglev steps in. It looks at the Feedback Vector collected by the interpreter and performs fast, mid-level optimizations.

### Tier 4: TurboFan (Peak Optimizer)

For code paths running heavily (like a massive game loop or data processing math), TurboFan takes over. It spends significant CPU time analyzing the graph, aggressively guessing that your types won't change, and spits out hyper-optimized machine code tailored precisely to your CPU architecture.

## Deoptimization (The Guardrails)

JavaScript is dynamically typed, meaning a variable can change from an integer to a string at any moment. TurboFan speculates that this won't happen to keep your code fast.

If TurboFan optimized a function assuming `x` and `y` are always integers, and you suddenly pass a string (`"hello"`), a safety guard fails. The engine immediately triggers Deoptimization. It throws away the optimized TurboFan machine code, reconstructs the execution state, and safely drops execution back down to the Sparkplug or Ignition layers to process the string safely (albeit more slowly).
    

# var vs. let vs. const

### Global Object Attachment

-   **`var`:** If declared in the global scope, it becomes a property of the browser's global window object (e.g., `window.myVar`).
    
-   **`let` & `const`:** Even when declared in the global scope, they enter the Script Scope and do not attach to the window object.
    

### Scope Boundaries

-   **`var`:** It is Function-scoped (or globally scoped if not inside a function). It completely ignores block boundaries like if statements or for loops.
    
-   **`let` & `const`:** They are strictly Block-scoped. They are securely trapped inside whichever pair of curly braces `{}` they were created in.
    

### Mutation and Re-declaration Rules

-   **`var`:** Both re-declaration and re-assignment are fully allowed anywhere in the scope.
    
-   **`let`:** Re-declaration is not allowed (throws a `SyntaxError`). Re-assignment is fully allowed.
    
-   **`const`:** Neither re-declaration nor re-assignment is allowed. It must be initialized with a value immediately upon declaration. The value of a `const` is not immutable. If it holds an object, you can change the object's properties.
    

### Hoisting and Initialization Behaviors

-   **`var`:** Hoisted and automatically initialized with `undefined`. Safe to access before its declaration line (returns `undefined`).
    
-   **`let` & `const`:** Hoisted but left `<uninitialized>`. Accessing them before their declaration line throws a `ReferenceError` because they are trapped in the Temporal Dead Zone (TDZ).
    

## Quick Reference Summary Table


| Feature Matrix                 | `var`          | `let`                     | `const`                   |
| :----------------------------- | :------------- | :------------------------ | :------------------------ |
| **Scope Boundary**             | Function Scope | Block Scope               | Block Scope               |
| **Global Window Attachment**   | Yes            | No                        | No                        |
| **Re-declaration Support**     | Yes            | No (Throws `SyntaxError`) | No (Throws `SyntaxError`) |
| **Re-assignment Support**      | Yes            | Yes                       | No (Throws `TypeError`)   |
| **Initial Memory Phase State** | `undefined`    | `<uninitialized>` (TDZ)   | `<uninitialized>` (TDZ)   |

    

# Data Types in JavaScript

In JavaScript, there are 2 types of datatypes: Primitive and Non-Primitive (Reference Types).

## 1. Primitive Datatypes

Only a single value can be stored and of one single type. Primitives are copied by value, not by reference. They are immutable (cannot be changed) and are stored directly in the stack memory. When you copy a primitive, you create a brand-new, independent copy of the value.

There are a total of 7 primitive datatypes in JavaScript.

### number

It could be an integer value or it could be a floating value.

```
let n = 45;
let x = 34.67; 
```

Besides regular numbers, there are so-called "special numeric values" which also belong to this data type: `Infinity`, `-Infinity`, and `NaN`.

Infinity represents the mathematical Infinity ∞. It is a special value that’s greater than or less than any number. We can get it as a result of division by zero:

```
alert( 1 / 0 ); // Infinity
```

`NaN` represents a computational error. It is a result of an incorrect or an undefined mathematical operation:

```
alert( "not a number" / 2 ); // NaN, such division is erroneous
```

`NaN` is sticky. Any further mathematical operation on `NaN` returns `NaN`:

```
alert( NaN + 1 ); // NaN
alert( 3 * NaN ); // NaN
alert( "not a number" / 2 - 1 ); // NaN
```

So, if there’s a `NaN` somewhere in a mathematical expression, it propagates to the whole result (there’s only one exception to that: `NaN 0` is `1`).

### BigInt

In JavaScript, the `number` type cannot safely represent integer values larger than (2<sup>53</sup> - 1) (that’s `9007199254740991`), or less than -(2<sup>53</sup> - 1) for negatives.

For most purposes, the ±(2<sup>53</sup> - 1) range is quite enough. However, sometimes you need the entire range of massive integers—such as for cryptography or microsecond-precision timestamps. The `BigInt` type was added to the language to represent integers of arbitrary length.

A `BigInt` value is created by appending an `n` to the end of an integer:

```
// The "n" at the end tells the engine it's a BigInt
const bigInt = 1234567890123456789012345678901234567890n;
```

### String

A string in JavaScript must be surrounded by quotes. In JavaScript, there are 3 types of quotes:

-   Double quotes: `"Hello"`
    
-   Single quotes: `'Hello'`
    
-   Backticks: `` `Hello` ``
    

Double and single quotes are “simple” quotes. There’s practically no difference between them in JavaScript.

Backticks are “extended functionality” quotes. They allow us to embed variables and expressions into a string by wrapping them in `${...}`:

```
let name = "John";

// embed a variable
alert( `Hello, ${name}!` ); // Hello, John!

// embed an expression
alert( `the result is ${1 + 2}` ); // the result is 3
```

### Boolean

The boolean type has only two values: `true` and `false`.

```
let nameFieldChecked = true; // yes, name field is checked
let ageFieldChecked = false; // no, age field is not changed
```

### null

Intentional absence of data.

```
let age = null;
```

In JavaScript, `null` is not a “reference to a non-existing object” or a “null pointer” like in some other languages. It’s just a special value which represents “nothing”, “empty” or “value unknown”. The code above states that age is unknown.

### undefined

The meaning of `undefined` is “value is not assigned”. If a variable is declared, but not assigned, then its value is `undefined`:

```
let age;
alert(age); // shows "undefined"
```

Technically, it is possible to explicitly assign `undefined` to a variable:

```
age = undefined;
alert(age); // "undefined"
```

But it is not recommended. Normally, one uses `null` to assign an “empty” or “unknown” value to a variable, while `undefined` is reserved as a default initial value for unassigned things.

### symbol

Introduced in ES6 to create completely unique, immutable identifiers. Even if you create two symbols with the exact same description, they are fundamentally unique. This is used to create unique identifiers for objects.

```
let id = Symbol("id");
```

## 2. Reference Types (Passed by Reference)

### object

The object type is special. Unlike primitives, objects are mutable collections of key-value pairs stored in the heap memory. Variables do not hold the object itself; they hold a memory pointer (reference) to where the object sits.

All other types are called “primitive” because their values can contain only a single thing (be it a string or a number or whatever). In contrast, objects are used to store collections of data and more complex entities. And these are copied by reference.

```
let userInfo = {
    "name" : "Ramesh",
    "age" : 24,
    "getOccupation" : function(){
        console.log("Ramesh is a Software Developer");
    }
};
```

**Crucial Sub-types:** Arrays (ordered lists) and Functions (callable objects). Dates are technically classified as structural objects under the hood.

# Advanced Collections: Map and Set

## Map

Map is a collection of keyed data items, just like an Object. But the main difference is that Map allows keys of any type.

### Initialization and Methods

-   `new Map([iterable])` – creates the map, with optional iterable (e.g. array) of `[key,value]` pairs for initialization.
    
-   `map.set(key, value)` – stores the value by the key, returns the map itself.
    
-   `map.get(key)` – returns the value by the key, `undefined` if key doesn’t exist in map.
    
-   `map.has(key)` – returns `true` if the key exists, `false` otherwise.
    
-   `map.delete(key)` – removes the element by the key, returns `true` if key existed at the moment of the call, otherwise `false`.
    
-   `map.clear()` – removes everything from the map.
    
-   `map.size` – returns the current element count.
    

```
let recipeMap = new Map([
  ['cucumber', 500],
  ['tomatoes', 350],
  ['onion',     50]
]);
```

### Looping Over a Map

For looping over a map, there are 3 methods:

-   `map.keys()` – returns an iterable for keys,
    
-   `map.values()` – returns an iterable for values,
    
-   `map.entries()` – returns an iterable for entries `[key, value]`, it’s used by default in `for..of`.
    

### Chaining Methods

Chaining is also possible as `map.set` call returns the map itself:

```
map.set('1', 'str1')
  .set(1, 'num1')
  .set(true, 'bool1');
```

### Converting Objects and Maps Flawlessly

**Map from plain object**

```
let obj = {
  name: "John",
  age: 30
};

let map = new Map(Object.entries(obj));
alert( map.get('name') ); // John
```

**Object from Map**

```
let prices = Object.fromEntries([
  ['banana', 1],
  ['orange', 2],
  ['meat', 4]
]);

// now prices = { banana: 1, orange: 2, meat: 4 }
alert(prices.orange); // 2
```

**Flawless Conversion Snippet Summary**

```
// A. Plain Object ──► Map
let obj = { name: "John", age: 30 };
let mapFromObj = new Map(Object.entries(obj));

// B. Map ──► Plain Object
let mapToObj = Object.fromEntries(mapFromObj);
```

## Set

Set is a collection of unique values.

### Methods and Properties

-   `new Set([iterable])` – creates the set, and if an iterable object is provided (usually an array), copies values from it into the set.
    
-   `set.add(value)` – adds a value, returns the set itself.
    
-   `set.delete(value)` – removes the value, returns `true` if value existed at the moment of the call, otherwise `false`.
    
-   `set.has(value)` – returns `true` if the value exists in the set, otherwise `false`.
    
-   `set.clear()` – removes everything from the set.
    
-   `set.size` – is the elements count.
    

The main feature is that repeated calls of `set.add(value)` with the same value don’t do anything. That’s the reason why each value appears in a Set only once.

### Iteration Methods

The same methods Map has for iterators are also supported for compatibility:

-   `set.keys()` – returns an iterable object for values,
    
-   `set.values()` – same as `set.keys()`, for compatibility with Map,
    
-   `set.entries()` – returns an iterable object for entries `[value, value]`, exists for compatibility with Map.
    

# Memory Management: WeakMap and WeakSet

JavaScript engine keeps a value in memory while it is “reachable” and can potentially be used.

If we put an object into an array, then while the array is alive, the object will be alive as well, even if there are no other references to it.

```
let john = { name: "John" };
let array = [ john ];

john = null; // overwrite the reference

// the object previously referenced by john is stored inside the array
// therefore it won't be garbage-collected
// we can get it as array[0]
```

Similar to that, if we use an object as the key in a regular Map, then while the Map exists, that object exists as well. It occupies memory and may not be garbage collected.

```
let john = { name: "John" };
let map = new Map();
map.set(john, "...");

john = null; // overwrite the reference

// john is stored inside the map,
// we can get it by using map.keys()
```

## WeakMap

WeakMap is fundamentally different in this aspect. It doesn’t prevent garbage-collection of key objects. A WeakMap is a collection of key-value pairs where the keys must be Objects or unique Symbols, and the values can be anything.

### The Core Behavior

It holds a "weak" link to the key. If that key object has no other strong references pointing to it anywhere else in the application, the Garbage Collector will vaporize it from RAM, and its entry inside the WeakMap will vanish automatically.

Now, if we use an object as the key in it, and there are no other references to that object – it will be removed from memory (and from the map) automatically.

```
let john = { name: "John" };
let weakMap = new WeakMap();
weakMap.set(john, "...");

john = null; // overwrite the reference

// john is removed from memory!
```

### Allowed vs. Forbidden Keys (Post-ES2023)

```
let weakMap = new WeakMap();

// 🟢 Allowed Keys: Objects and Symbols
let objKey = {};
let symKey = Symbol("unique");
weakMap.set(objKey, "data"); 
weakMap.set(symKey, "more data"); 

// ❌ Forbidden Keys: Registered Symbols & Traditional Primitives
weakMap.set(Symbol.for("global"), "error"); // TypeError (Registered symbols don't clean up)
weakMap.set("stringKey", "error");          // TypeError
```

### API Limitations

WeakMap does not support iteration and methods `keys()`, `values()`, `entries()`, so there’s no way to get all keys or values from it.

WeakMap has only the following methods:

-   `weakMap.set(key, value)`
    
-   `weakMap.get(key)`
    
-   `weakMap.delete(key)`
    
-   `weakMap.has(key)`
    

## WeakSet

A WeakSet is a Set-like collection that only stores unique Objects and unique Symbols. Like WeakMap, it holds weak references to its contents. Like Set, it supports `add`, `has` and `delete`, but not `size`, `keys()` and no iterations.

```
let visitedUsers = new WeakSet();

let ramesh = { name: "Ramesh" };
visitedUsers.add(ramesh);

console.log(visitedUsers.has(ramesh)); // true

ramesh = null; // The object is now floating cleanly out of memory!

```

WeakMap and WeakSet are used as “secondary” data structures in addition to the “primary” object storage. Once the object is removed from the primary storage, if it is only found as the key of WeakMap or in a WeakSet, it will be cleaned up automatically.

# The typeof Operator Quirks

The `typeof` operator returns the type of the operand.

```
typeof undefined // "undefined"
typeof 0 // "number"
typeof 10n // "bigint"
typeof true // "boolean"
typeof "foo" // "string"
typeof Symbol("id") // "symbol"
typeof Math // "object"
typeof alert // "function"
typeof null // "object"
```

### Why does `typeof null` return `"object"`?

`null` is a primitive datatype, so why then is its type `"object"` and not `"null"`? This is an error in the language; it’s not actually an object. It is not being corrected because of backward compatibility issues.
    

# Type Conversions

## 1. Implicit Conversion (Type Coercion)

This is when the JavaScript engine automatically converts types behind the scenes during expressions or comparisons.

### The `+` Operator (String Dominance)

If any operand is a string, JavaScript converts the other to a string and concatenates them together.

```
"5" + 2    // "52" (Number 2 becomes string "2")
true + "b" // "trueb"
```

### Other Mathematical Operators (`-`, `*`, `/`, `%`)

These operators always force values into Numbers.

```
"6" - "2"  // 4
"5" * true // 5 (true becomes 1)
"4" / null // Infinity (null becomes 0)
```

## 2. Explicit Conversions

This is when you intentionally use built-in constructors (`String()`, `Number()`, `Boolean()`) to cast a value from one type to another.

### String Conversion

Pretty straightforward. Using `String(value)` or calling `value.toString()` wraps the raw value in quotes.

```
String(null)      // "null"
String(undefined) // "undefined"
```

### Numeric Conversion

Occurs when using `Number(value)` or applying the unary plus operator (`+value`).

```
let value1 = "6";      // string
value1 = Number(value1); // now it is converted to a number
```

**Conversion Rules Matrix:**

-   `undefined` ──► `NaN`
    
-   `null` ──► `0`
    
-   `true` and `false` ──► `1` and `0`
    
-   **Empty String `""`:** Strips out white spaces (`\n`, `\t`) and reads an empty space as `0`.
    
-   **Valid Numeric String `" 42 "`:** Parses to `42`.
    
-   **Invalid String `"42px"`:** Results in `NaN`. `Number()` fails completely if there are non-numeric characters. _(Note: Use `parseInt("42px")` if you want to extract 42)._
    

### Boolean Conversion

When using `Boolean(value)`, JavaScript splits the entire universe into two categories: Truthy and Falsy. There are only 8 Falsy values in JavaScript that turn into `false`. Everything else resolves to `true`.

```
// The 8 Falsy Values:
Boolean(false)     // false
Boolean(0)         // false
Boolean(-0)        // false
Boolean(0n)        // false (BigInt zero)
Boolean("")        // false (Empty string)
Boolean(null)      // false
Boolean(undefined) // false
Boolean(NaN)       // false
```

# Basic Operators

## Binary Operators (`+`, `-`, `*`, `/`, `%`, )

An operator is binary if it has two operands.

```
let x = 1, y = 3;
alert( y - x ); // 2, binary minus subtracts values
alert( 4 ** (1/2) ); // 2 (power of 1/2 is the same as a square root)
alert( 8 % 3 ); // 2, the remainder of 8 divided by 3
```

### String Concatenation with Binary `+`

```
let s = "my" + "string";
alert(s); // mystring
```

If any of the operands is a string, then the other one is converted to a string too if the operand is `+`:

```
alert( '1' + 2 ); // "12"
alert( 2 + '1' ); // "21"
alert( 2 + 2 + '1' ); // "41" and not "221"
alert( '1' + 2 + 2 ); // "122" and not "14"
```

The binary `+` is the only operator that supports strings in such a way. Other arithmetic operators work only with numbers and always convert their operands to numbers.

```
alert( 6 - '2' ); // 4, converts '2' to a number
alert( '6' / '2' ); // 3, converts both operands to numbers
```

## Unary Operators (`+`, `-`)

An operator is unary if it has a single operand. For example, the unary negation `-` reverses the sign of a number:

```
let x = 1;
x = -x;
alert( x ); // -1, unary negation was applied
```

### Numeric Conversion with Unary `+`

The unary plus applied to a single value doesn’t do anything to numbers. But if the operand is not a number, the unary plus converts it into a number. It actually does the same thing as `Number(...)`, but is shorter.

```
// No effect on numbers
let x = 1;
alert( +x ); // 1

let y = -2;
alert( +y ); // -2

// Converts non-numbers
alert( +true ); // 1
alert( +"" );   // 0

let apples = "2";
let oranges = "3";
alert( apples + oranges ); // "23", the binary plus concatenates strings
```

## Assignment Operators

Assignment `=` returns a value. All operators in JavaScript return a value. That’s obvious for `+` and `-`, but also true for `=`. The call `x = value` writes the value into `x` and then returns it.

```
let a = 1;
let b = 2;
let c = 3 - (a = b + 1);

alert( a ); // 3
alert( c ); // 0
```

### Chaining Assignments

```
let a, b, c;
a = b = c = 2 + 2;

alert( a ); // 4
alert( b ); // 4
alert( c ); // 4
```

Chained assignments evaluate from right to left. First, the rightmost expression `2 + 2` is evaluated and then assigned to the variables on the left: `c`, `b`, and `a`. At the end, all the variables share a single value.

```
let n = 2;
n += 5; // now n = 7 (same as n = n + 5)
n *= 2; // now n = 14 (same as n = n * 2)
alert( n ); // 14
```

```
let n = 2;
n *= 3 + 5; // right part evaluated first, same as n *= 8
alert( n ); // 16
```

## Increment/Decrement Operators

Increasing or decreasing a number by one.

-   **Increment `++`** increases a variable by 1: `counter++` works the same as `counter = counter + 1`.
    
-   **Decrement `--`** decreases a variable by 1: `counter--` works the same as `counter = counter - 1`.
    

> Increment/decrement can only be applied to variables. Trying to use it on a value like `5++` will give an error.

The operators `++` and `--` can be placed either before or after a variable:

-   **Postfix form:** `counter++`
    
-   **Prefix form:** `++counter`
    

Both increase the value by one, but the prefix form returns the new value while the postfix form returns the old value (prior to the increment/decrement).

```
let counter = 1;
let a = ++counter; // prefix form increments counter and returns the new value, 2.
alert(a); // 2
```

```
let counter = 1;
let a = counter++; // postfix form increments counter but returns the old value, 1.
alert(a); // 1
```

If the result of the increment/decrement is not used, there is no difference in which form to use:

```
let counter = 0;
counter++;
++counter;
alert( counter ); // 2
```

### Increment/Decrement Among Other Operators

```
let counter = 1;
alert( 2 * ++counter ); // 4
```

```
let counter = 1;
alert( 2 * counter++ ); // 2, because counter++ returns the "old" value
```

## Bitwise Operators

Bitwise operators treat arguments as 32-bit integer numbers and work on the level of their binary representation.

-   **AND ( `&` )**
    
    `5 & 1`  `>>` `0101 & 0001`  `>>` `0001`  `>>` `1`
    
-   **OR ( `\|` )**
    
    `5 | 1`  `>>` `0101 | 0001`  `>>` `0101`  `>>` `5`
    
-   **XOR ( `^` )**
    
    `5 ^ 1`  `>>` `0101 ^ 0001`  `>>` `0100`  `>>` `4`
    
-   **NOT ( `~` )**
    
    Inverts all bits (including the sign bit). Formula: `~x = -(x + 1)`
    
    `~5`  `→` `-6`
    
-   **LEFT SHIFT ( `<<` )**
    
    This is a zero-fill left shift. One or more zero bits are pushed in from the right, and the leftmost bits fall off.
    
    `5 << 1`  `→` `0101 << 1`  `→` `1010`  `→` `10`
    
-   **Sign-Preserving Right Shift ( `>>` )**
    
    This operator keeps the sign of the number intact by duplicating the sign bit (the leftmost bit) as it shifts bits to the right.
    
    `5 >> 1`  `→` `0101 >> 1`  `→` `0010`  `→` `2`
    
    If the number is negative, the leftmost bit is 1, so it pushes 1s from the left. `-5 >> 1` evaluates to `-3`.
    
-   **ZERO-FILL RIGHT SHIFT ( `>>>` )**
    
    This is a zero-fill right shift. One or more zero bits are pushed in from the left, and the rightmost bits fall off.
    
    `5 >>> 1` `→` `0101 >>> 1` `→` `0010` `→` `2` (Same as `>>` for positive numbers)
    
    `-5 >>> 1` `→` `2147483645` ⚠️ _(Sign bit becomes 0, rendering it positive)_
    

## Comma Operator

The comma operator allows us to evaluate several expressions, dividing them with a comma `,`. Each of them is evaluated but only the result of the last one is returned.

```
let a = (1 + 2, 3 + 4);
alert( a ); // 7 (the result of 3 + 4)
```

## Comparison Operators

All comparison operators return a boolean value: `true` or `false`.

```
alert( 2 > 1 );  // true
alert( 2 == 1 ); // false
alert( 2 != 1 ); // true
```

### String Comparison

Strings are compared letter-by-letter in lexicographical order:

```
alert( 'Z' > 'A' ); // true
alert( 'Glow' > 'Glee' ); // true
alert( 'Bee' > 'Be' ); // true
```

### Comparison of Different Types

When comparing values of different types, JavaScript converts the values to numbers.

```
alert( '2' > 1 ); // true, string '2' becomes a number 2
alert( '01' == 1 ); // true, string '01' becomes a number 1
alert( true == 1 ); // true
alert( false == 0 ); // true
```

```
let a = 0;
alert( Boolean(a) ); // false

let b = "0";
alert( Boolean(b) ); // true

alert(a == b); // true!
```

From JavaScript’s standpoint, this result is normal. An equality check converts values using numeric conversion (hence `"0"` becomes `0`), while the explicit Boolean conversion uses another set of rules.

### Strict Equality

A strict equality operator `===` checks the equality without type conversion. If `a` and `b` are of different types, then `a === b` immediately returns `false` without an attempt to convert them. There is also a “strict non-equality” operator `!==` analogous to `!=`.

### Comparison with null and undefined

```
alert( null === undefined ); // false
alert( null == undefined ); // true
```

```
// The Strict Equality Anomalies:
console.log(NaN === NaN);   // false (Engine paradox)
console.log(-0 === +0);     // true  (Inaccurate bit representation)

// Object.is() evaluates pure, flawless identity matching:
console.log(Object.is(NaN, NaN)); // true
console.log(Object.is(-0, +0));   // false
```

> For details on operator hierarchy, see the official [W3Schools JavaScript Operator Precedence Reference](https://www.w3schools.com/js/js_precedence.asp).

# Conditional Operators

The `if(...)` statement evaluates a condition in parentheses and, if the result is `true`, executes a block of code. The expression in its parentheses is converted to a boolean. The `if` statement may contain an optional `else` block which executes when the condition is falsy.

```
let year = prompt('In which year was the ECMAScript-2015 specification published?', '');

if (year == 2015) {
  alert( 'You guessed it right!' );
} else {
  alert( 'How can you be so wrong?' ); // any value except 2015
} 
```

The `else if` clause lets us test several variants of a condition:

```
let year = prompt('In which year was the ECMAScript-2015 specification published?', '');

if (year < 2015) {
  alert( 'Too early...' );
} else if (year > 2015) {
  alert( 'Too late' );
} else {
  alert( 'Exactly!' );
}
```

## Conditional / Ternary Operator `?`

Sometimes we need to assign a variable depending on a condition.

```
let accessAllowed;
let age = prompt('How old are you?', '');

if (age > 18) {
  accessAllowed = true;
} else {
  accessAllowed = false;
}
alert(accessAllowed);
```

The conditional, question mark, or "Ternary" operator lets us do that in a shorter and simpler way. It is represented by a question mark `?`.

```
let result = condition ? value1 : value2;
```

```
let accessAllowed = age > 18 ? true : false;
```

### Multiple `?`

```
let age = prompt('age?', 18);

let message = (age < 3)   ? 'Hi, baby!' :
              (age < 18)  ? 'Hello!' :
              (age < 100) ? 'Greetings!' :
                            'What an unusual age!';

alert( message );
```

# Logical Operators

There are four logical operators in JavaScript: `||` (OR), `&&` (AND), `!` (NOT), and `??` (Nullish Coalescing).

## `||` (OR)

Logical OR handles boolean values. If any of its arguments are `true`, it returns `true`, otherwise it returns `false`.

```
let hour = 9;
if (hour < 10 || hour > 18) {
  alert( 'The office is closed.' );
}
```

We can pass more conditions:

```
let hour = 12;
let isWeekend = true;

if (hour < 10 || hour > 18 || isWeekend) {
  alert( 'The office is closed.' ); // it is the weekend
}
```

### OR `||` Finds the First Truthy Value

```
result = value1 || value2 || value3;
```

-   Evaluates operands from left to right.
    
-   For each operand, converts it to boolean. If the result is `true`, stops and returns the original value of that operand.
    
-   If all operands have been evaluated (i.e. all were false), returns the last operand.
    

```
alert( 1 || 0 ); // 1 (1 is truthy)
alert( null || 1 ); // 1 (1 is the first truthy value)
alert( null || 0 || 1 ); // 1 (the first truthy value)
alert( undefined || null || 0 ); // 0 (all falsy, returns the last value)
```

Another feature of the OR `||` operator is short-circuit evaluation. It processes its arguments until the first truthy value is reached, and then the value is returned immediately without touching the remaining arguments.

## `&&` (AND)

Returns `true` if both operands are truthy and `false` otherwise.

### AND `&&` Finds the First Falsy Value

```
result = value1 && value2 && value3;
```

-   Evaluates operands from left to right.
    
-   For each operand, converts it to a boolean. If the result is `false`, stops and returns the original value of that operand.
    
-   If all operands have been evaluated (i.e. all were truthy), returns the last operand.
    

```
// if the first operand is truthy, AND returns the second operand:
alert( 1 && 0 ); // 0
alert( 1 && 5 ); // 5

// if the first operand is falsy, AND returns it. The second operand is ignored
alert( null && 5 ); // null
alert( 0 && "no matter what" ); // 0
alert( 1 && 2 && null && 3 ); // null
alert( 1 && 2 && 3 ); // 3, the last one
```

> The precedence of the AND `&&` operator is higher than OR `||`. So the code `a && b || c && d` is essentially parsed as `(a && b) || (c && d)`.

## `!` (NOT)

The boolean NOT operator is represented with an exclamation sign `!`.

```
result = !value;
```

-   Converts the operand to boolean type: `true`/`false`.
    
-   Returns the inverse value.
    

```
alert( !true ); // false
alert( !0 ); // true
alert( !!"non-empty string" ); // true
alert( !!null ); // false
```

## `??` (Nullish Coalescing Operator)

The nullish coalescing operator is written as two question marks `??`. This operator checks specifically for both `null` and `undefined`.

The result of `a ?? b` is:

-   If `a` is neither `null` nor `undefined`, then `a`.
    
-   If `a` is either `null` or `undefined`, then `b`.
    

> For safety reasons, JavaScript forbids using `??` together with `&&` and `||` operators, unless the precedence is explicitly specified with parentheses.

```
// let x = 1 && 2 ?? 3; // Syntax error
let x = (1 && 2) ?? 3; // Works
alert(x); // 2
```

# Statements vs. Expressions

-   **Expression:** Anything that produces a value (e.g., `2 + 2`, `alert(i)` which evaluates to `undefined`, or a variable name).
    
-   **Statement:** A structural command that performs an action (e.g., `if`, `for`, `break`, `continue`).
    

Syntax constructs that are not expressions cannot be used with the ternary operator `?`. In particular, directives such as `break`/`continue` aren’t allowed there.

For example, if we take this code:

```
if (i > 5) {
  alert(i);
} else {
  continue;
}
```

...and rewrite it using a question mark:

```
(i > 5) ? alert(i) : continue; // ❌ continue isn't allowed here
```

...it stops working: there’s a syntax error. This is just another reason not to use the question mark operator `?` instead of `if`.

# The switch Statement

A `switch` statement can replace multiple `if` checks.

```
switch(x) {
  case 'value1':  // if (x === 'value1')
    ...
    [break]

  case 'value2':  // if (x === 'value2')
    ...
    [break]

  default:
    ...
    [break]
}
```

The value of `x` is checked for strict equality (`===`) to the value from the first case (that is, `value1`), then to the second (`value2`), and so on.

-   If the equality is found, `switch` starts to execute the code starting from the corresponding case, until the nearest `break` (or until the end of the `switch`).
    
-   If no case is matched, then the `default` code is executed (if it exists).
    

### Example 1

```
let a = 2 + 2;

switch (a) {
  case 3:
    alert( 'Too small' );
    break;
  case 4:
    alert( 'Exactly!' );
    break;
  case 5:
    alert( 'Too big' );
    break;
  default:
    alert( "I don't know such values" );
}
```

Here the `switch` starts to compare from the first case variant that is `3`. The match fails. Then `4`. That’s a match, so the execution starts from `case 4` until the nearest `break`.

> If there is no `break`, then the execution continues with the next case without any checks.

## Grouping of "case"

Several variants of `case` which share the same code can be grouped. For example, if we want the same code to run for `case 3` and `case 5`:

```
let a = 3;

switch (a) {
  case 4:
    alert('Right!');
    break;

  case 3: // (*) grouped two cases
  case 5:
    alert('Wrong!');
    alert("Why don't you take a math class?");
    break;

  default:
    alert('The result is strange. Really.');
}
```

# Loops

Loops are a way to repeat the same code multiple times.

## Basic Loops: while, do..while, and for

### while Loop

While the condition is truthy, the code from the loop body is executed.

```
while (condition) {
  // code
  // so-called "loop body"
}
```

If the loop body has a single statement, we can omit the curly braces `{…}`:

```
let i = 3;
while (i) alert(i--);
```

### do..while Loop

The condition check can be moved below the loop body using the `do..while` syntax.

```
do {
  // loop body
} while (condition);
```

The loop will first execute the body, then check the condition, and, while it’s truthy, execute it again and again.

> This form of syntax should only be used when you want the body of the loop to execute at least once regardless of the condition being truthy.

### for Loop

The `for` loop is more complex, but it’s also the most commonly used loop.

```
for (begin; condition; step) {
  // ... loop body ...
}
```

```
for (let i = 0; i < 3; i++) { // shows 0, then 1, then 2
  alert(i);
}
```

#### General Looping Algorithm

Plaintext

```
Run begin
→ (if condition → run body and run step)
→ (if condition → run body and run step)
→ (if condition → run body and run step)
→ ...
```

#### Skipping Parts of the for Loop

Any part of `for` can be skipped. For example, we can omit `begin` if we don’t need to do anything at the loop start:

```
let i = 0; // we have i already declared and assigned

for (; i < 3; i++) { // no need for "begin"
  alert( i ); // 0, 1, 2
}
```

We can also remove the `step` part:

```
let i = 0;

for (; i < 3;) {
  alert( i++ );
}
```

This makes the loop identical to `while (i < 3)`.

We can actually remove everything, creating an infinite loop:

```
for (;;) {
  // repeats without limits
}
```

> Please note that the two `for` semicolons `;` must be present. Otherwise, there would be a syntax error.

## break and continue

Normally, a loop exits when its condition becomes falsy. But we can force the exit at any time using special directives.

### break

It stops the loop immediately, passing control to the first line after the loop.

```
let sum = 0;

while (true) {
  let value = +prompt("Enter a number", '');

  // if the user enters an empty line or cancels the input,
  // it breaks out immediately, passing control to the alert line.
  if (value === null || value === "") break;
  sum += value;
}

alert( 'Sum: ' + sum );
```

### continue

It skips the remaining part of the current body iteration and forces the loop to move to the next cycle.

```
for (let i = 0; i < 10; i++) {
  // if true, skip the remaining part of the body
  // basically skipping the current iteration
  if (i % 2 == 0) continue;

  alert(i); // 1, then 3, 5, 7, 9
}
```

## Loop Labels

Normally, if you have a nested loop (a loop inside a loop) and you call `break` or `continue`, it only affects the inner loop you are currently standing in. What if you want to break out of the entire nested system from the inside? That is where a Label comes in.

### Labeling a for loop structure

```
// 🟢 Labeling the outer loop
outerLoop: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    
    let input = prompt(`Value at coords (${i},${j})`, '');

    // If user cancels or inputs empty, break out of EVERYTHING
    if (!input) break outerLoop; // Stops both loops instantly!

    console.log(`Coords: ${i},${j}`);
  }
}
```

Loop labels are not exclusive to `for` loops. They work seamlessly with all basic loop structures in JavaScript, including `while` and `do..while`.

### Labeling a while loop structure

```
let i = 0;

// 🟢 Labeling the outer while loop
outerWhile: while (i < 3) {
  let j = 0;
  
  while (j < 3) {
    console.log(`i: ${i}, j: ${j}`);
    
    if (i === 1 && j === 1) {
      break outerWhile; // 🛑 Terminally exits the outer while loop completely
    }
    j++;
  }
  i++;
}
```

# Functions

Functions allow code to be called many times without repetition when performing similar actions in multiple locations.

## 1. Function Declaration / Named Function / Function Statement

```
function showMessage() {
  alert( 'Hello everyone!' );
}

// calling the function by its name
showMessage();
```

## 2. Anonymous Function / Function Expression

An anonymous function is a function without a name. It is defined as an expression and often used as a callback function or assigned as a value to a variable.

```
const multiply = function(a, b) {
  return a * b;
};
```

### Function Declaration vs. Function Expression

-   **Function Declarations:** Fully hoisted with their actual body contents during the memory allocation sweep. They are callable before their lines appear in the code.
    
-   **Function Expressions:** Follow variable hoisting constraints. If bound to a `const` or `let`, they sit uninitialized in the TDZ. If bound to a `var`, they initialize as `undefined` and will crash with a `TypeError: X is not a function` if invoked early.
    

## 3. Arrow Functions

Arrow functions provide a shorter syntax compared to traditional functions. They do not have access to the `arguments` object and they do not have their own `this`.

```
const divide = (a, b) => a / b;
```

> **Inferred Naming:** JavaScript engines use Inferred Naming. If you write `const myFn = () => {}`, the engine assigns `"myFn"` to the function's internal `.name` property. This ensures it displays properly in browser stack traces instead of showing up as anonymous.

Modern JS uses the Rest Parameter syntax (`...args`) to capture dynamic arguments inside arrow functions:

```
const logArgs = (...args) => console.log(args); // args is a true array!
```

## 4. Higher Order Function (HOF)

A function that takes another function as a parameter or returns a function as a result is called a HOF.

```
function operation(add, num1, num2) { // operation is HOF
  let result = add(num1, num2);
  console.log("result: ", result);
}
function add(a, b) {
  return a + b;
}
operation(add, 7, 5);
```

## 5. First-Class Functions

In JavaScript, functions are treated as first-class citizens, just like any other data type. This means functions can be assigned to variables, passed as arguments to other functions, returned from functions, and stored in data structures like arrays or objects.

```
const add = function (a, b) {
  return a + b;
};

const result = add(2, 3); // result will be 5

const mathOperation = add; // assigning the function to another variable
const result2 = mathOperation(4, 5); // result2 will be 9
```

## 6. Pure Functions

A pure function always produces the same output for the same input and has no side effects. It does not modify external state or interact with the outside world, making it predictable and easy to reason about.

```
function square(number) {
  return number * number;
}

const result = square(4); // result will be 16
```

## 7. Impure Functions

An impure function may produce different results for the same input or have side effects like modifying external state or interacting with the outside world (e.g., changing global variables, making API calls, or modifying files).

```
let total = 0;

function addToTotal(number) {
  total += number; // Modifying external state (total)
  return total;
}

const result1 = addToTotal(5); // result1 will be 5
const result2 = addToTotal(3); // result2 will be 8 (total was modified by the previous call)
```

## 8. Callback Functions

A callback function is a function passed as an argument to another function and executed after the completion of that function.

```
function fetchData(url, callback) {
  // Code to fetch data from the URL
  // Once data is fetched, call the callback function
  callback(data);
}

function processData(data) {
  // Code to process the fetched data
}

fetchData('https://example.com/data', processData);
```

## 9. Immediately Invoked Function Expressions (IIFE)

An IIFE is a function that is executed immediately after it is defined. It is typically used to create a private scope and avoid polluting the global namespace.

```
(function() {
  // Code here is executed immediately
})();
```

## 10. Constructor Functions

Constructor functions are technical blueprints used to instantiate multiple objects of the same structural shape. They are structurally normal functions, but by convention, they are named with a Capitalized First Letter and must be invoked using the `new` keyword.

When you call a function with `new`, JavaScript secretly creates a brand new blank object `{}` in memory, assigns it to the keyword `this`, executes your constructor logic to attach properties, and then automatically returns `this` at the very end.

```
function User(name, role) {
  // Implicitly: this = Object.create(User.prototype);
  this.name = name;
  this.role = role;
  // Implicitly: return this;
}

const sde1 = new User("Ramesh", "Developer"); 
console.log(sde1.name); // "Ramesh"
```

# Modern Class Architecture

Modern JavaScript classes (ES6+) provide a clean, declarative syntax built directly over the language's native prototypal inheritance engine. Advanced class specifications introduce true, runtime-enforced security boundaries and class-level memory spaces.

```
// 🅰️ The Base Parent Class
class SecureBankAccount {
  #balance = 0; // 🔒 True Engine Private Field. Hardware-isolated in memory!
  _accountHolder; // ⚠️ Convention-only "protected" field (still public under the hood)

  constructor(holderName, initialDeposit) {
    this._accountHolder = holderName;
    this.#balance = initialDeposit;
  }

  // ⚙️ Static Initialization Block (Runs EXACTLY ONCE when the engine loads the class blueprint)
  static {
    console.log("SecureBankAccount structurally initialized by the V8 Engine!");
  }

  // 🔒 Static Method (Belongs strictly to the Class constructor namespace, not instances)
  static generateRoutingNumber() {
    return Math.floor(100000000 + Math.random() * 900000000);
  }

  // 👁️ GETTER (Accessor Property): Intercepts property reads
  get balanceView() {
    return `Account Balance: $${this.#balance}`;
  }

  // ✍️ SETTER (Accessor Property): Intercepts property writes with custom validation guards
  set deposit(amount) {
    if (typeof amount !== "number" || amount <= 0) {
      throw new TypeError("Deposit amount must be a positive number!");
    }
    this.#balance += amount;
  }
}

// 🅱️ The Inheriting Subclass
class PremiumSavingsAccount extends SecureBankAccount {
  constructor(holderName, initialDeposit, interestRate) {
    // ❌ Error Rule: Writing `this.interestRate = interestRate` here will throw a ReferenceError!
    
    super(holderName, initialDeposit); // 🟢 MUST call super() first to instantiate the parent class link!
    this.interestRate = interestRate;  // 🟢 Safe to assign subclass properties now
  }
}

// 🚀 Verification Execution Loop
const account = new SecureBankAccount("Priyam", 1000);

console.log(account.balanceView); // "Account Balance: $1000" (Invoked via getter)
account.deposit = 500;            // 🟢 Deposited via setter validation
console.log(account.balanceView); // "Account Balance: $1500"

// --- ERROR TESTING CHECKS ---
// account.deposit = -100;         // ❌ TypeError: Deposit amount must be a positive number!
// console.log(account.#balance);  // ❌ Hard SyntaxError: Private field '#balance' must be declared in an enclosing class

console.log(SecureBankAccount.generateRoutingNumber()); // 🟢 583920194 (Static lookup)
// console.log(account.generateRoutingNumber());         // ❌ TypeError: account.generateRoutingNumber is not a function
```

## 11. Generator Functions

The normal functions in JavaScript execute according to the non-preemptive or run-to-completion model, which means their execution cannot be paused in between. Generator functions possess the capability to pause execution with the help of the `yield` keyword.

These are special functions that can generate a sequence of values. Whenever called, they return a Generator Object. The generator object follows the Iterable Protocol of ES6, working similarly to iterators.

Calling the `next()` method on the generator object executes the function until the first `yield` statement, where the yielded value is returned to the caller. Repeatedly calling `next()` allows access to a sequence of objects containing two properties: `value` (associated with the `yield` statement) and `done` (a boolean flag indicating whether anything remains in the function to execute).

The `yield` keyword pauses and resumes execution. The state of the function is retained so that execution resumes directly from the last evaluated `yield` statement.

```
function* countNumber() {
  let number = 1;
  while (number <= 10) {
    yield number;
    number++;
  }
}

const numbers = countNumber();

console.log(numbers.next());
console.log(numbers.next());
console.log(numbers.next());
console.log(numbers.next());
console.log(numbers.next());
console.log(numbers.next());
console.log(numbers.next());
console.log(numbers.next());
console.log(numbers.next());
console.log(numbers.next());
console.log(numbers.next());
```

The generator function `countNumber` produces a sequence of numbers from 1 to 10 using a `yield` statement.

-   Calling `numbers.next()` for the first time starts the generator and executes until the first `yield` statement, yielding the value `1`. The `done` property is `false`.
    
-   Subsequent calls to `numbers.next()` continue the generator's execution from where it left off, yielding 2, 3, and so on, until 10 is reached. The `done` property remains `false`.
    
-   After yielding 10, the generator completes its execution, and further calls to `numbers.next()` result in `{ value: undefined, done: true }`.
    

`next()` controls the progression of the generator's execution, while `value` holds the yielded value and `done` indicates if the generator has finished.

### Usage in Loops

We can pass generator objects directly into a standard `for..of` loop:

```
for (let num of countNumber()) {
  console.log(num); // Automatically calls .next() under the hood and unwraps the value!
}
```

## 12. Currying (Functional Transformation via Closures)

Currying transforms a function `f(a, b, c)` into a callable chain `f(a)(b)(c)`.

```
const buildLogger = (environment) => (serviceName) => (message) => {
  return `[${environment.toUpperCase()}][${serviceName}] ${message}`;
};

// Create a configured pre-baked utility factory:
const logProductionError = buildLogger("production")("AuthService");

// Trigger clean, isolated data evaluation later:
console.log(logProductionError("Token Expired!")); // "[PRODUCTION][AuthService] Token Expired!"
```

# Objects

There are eight data types in JavaScript. Seven of them are called “primitive”, because their values contain only a single thing (be it a string or a number or whatever).

In contrast, objects are used to store keyed collections of various data and more complex entities.

We can only use "string" or "symbol" as keys for an Object. Otherwise it simply converts those to string type.

Object keys were historically unordered, but modern JavaScript objects follow a strict, deterministic sequence when you call `Object.keys()`, `Object.entries()`, or run a `for..in` loop:

-   **Integer Keys / Symbol Keys:** Sorted in ascending numeric order (e.g., `"2"` comes before `"10"`).
    
-   **String Keys:** Sorted in chronological order of their literal insertion.
    

## Few ways to create objects

### 1. Object Literal

```
const person = {
    name: "John",
    age: 30,
    greet: function() {
        console.log(`Hello, my name is ${this.name} and I'm ${this.age} years old.`);
    }
};
```

### 2. Constructor Function

```
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function() {
        console.log(`Hello, my name is ${this.name} and I'm ${this.age} years old.`);
    };
}

const person = new Person("John", 30);
```

### 3. Object Constructor

```
const person = new Object();
person.name = "John";
person.age = 30;
person.greet = function() {
    console.log(`Hello, my name is ${this.name} and I'm ${this.age} years old.`);
};
```

### 4. `Object.assign(target, ...sources)`

Copies all enumerable own properties from one or more source objects to a target object.

### 5. `Object.create(proto, [propertiesObject])`

-   `proto`: The object which should be the prototype of the newly created object.
    
-   `propertiesObject` _(optional)_: An object whose enumerable own properties specify property descriptors to be added to the new object.
    

`Object.create(proto)` doesn't just copy properties; it establishes a live, real-time fallback link called the Prototype Chain (`__proto__`). If you try to read a property on the new object and it doesn't exist, the JavaScript engine looks "upward" into the proto object to find it.

```
const animal = { eats: true };
const rabbit = Object.create(animal); // rabbit inherits animal

console.log(rabbit.eats); // 🟢 true (Found via the Prototype Chain fallback)
console.log(rabbit.hasOwnProperty("eats")); // 🛑 false (It belongs to the parent prototype, not rabbit directly!)
```

## Accessing Object properties and methods and some useful tricks

```
const person = {
  name: "Robin",
  age: 24,
  "work doing": "software Engineer",
  greeting: function () {
    console.log(`Good Morning Mr.${person.name}`);
  },
};
```

### 1. Dot (.) notation

```
console.log(person.name) // "Robin"
console.log(person.age) // 24
```

### 2. Square brackets

The dot requires the key to be a valid variable identifier. That implies: contains no spaces, doesn’t start with a digit, and doesn’t include special characters (`$` and `_` are allowed). There’s an alternative “square bracket notation” that works with any string.

```
console.log(person["work doing"]) // "software Engineer"
```

### 3. Calling methods (functions inside an Object)

```
person.greeting() // "Good Morning Mr. Robin"
```

### 4. Adding new properties to an Object

```
person.hobby = "Coding"
```

### 5. Deleting a property

```
delete person.hobby
```

### 6. Getting an array of the Keys of the Object

```
console.log(Object.keys(person))
```

### 7. Getting an array of the Values of the Object

```
console.log(Object.values(person))
```

### 8. Getting all the key-value pairs of the Object

```
console.log(Object.entries(person))
```

### 9. Testing whether a property is present in the Object or not

**Way 1:**

```
let user = { name: "John", age: 30 };
alert( "age" in user ); // true, user.age exists
alert( "blabla" in user ); // false, user.blabla doesn't exist
```

**Way 2:**

```
alert(user.hasOwnProperty("age")) // true, user.age exists
alert(user.hasOwnProperty("blabla")) // false, user.blabla doesn't exist
```

## Computed properties

We can use square brackets inside an object literal when creating an object. That’s called computed properties.

```
let fruit = prompt("Which fruit to buy?", "apple");
let bag = {
  [fruit]: 5, // the name of the property is taken from the variable fruit
};
alert( bag.apple ); // 5 if fruit="apple"
```

## Property names limitations

As we already know, a variable cannot have a name equal to one of the language-reserved words like “for”, “let”, “return” etc. But for an object property, there’s no such restriction:

```
// these properties are all right
let obj = {
  for: 1,
  let: 2,
  return: 3
};

alert( obj.for + obj.let + obj.return );  // 6
```

## Object references and copying

One of the fundamental differences between an object and a primitive data type is that objects are stored and copied “by reference”, whereas primitive values (strings, numbers, booleans, etc.) are always copied “as a whole value”.

```
let message = "Hello!";
let phrase = message;
```

As a result, we have two independent variables, each one storing the string `"Hello!"`.

Objects are not like that. A variable assigned to an object stores not the object itself, but its “address in memory” – in other words, “a reference” to it.

```
let user = {
  name: "John"
};
```

The object is stored somewhere in memory, while the `user` variable holds a “reference” to it. When an object variable is copied, the reference is copied, but the object itself is not duplicated.

```
let user = { name: "John" };
let admin = user; // copy the reference
```

Now we have two variables, each storing a reference to the same object.

```
let user = { name: 'John' };
let admin = user;

admin.name = 'Pete'; // changed by the "admin" reference

alert(user.name); // 'Pete', changes are seen from the "user" reference
```

## Comparison by reference

Two objects are equal only if they are the exact same object reference.

```
let a = {};
let b = a; // copy the reference

alert( a == b ); // true, both variables reference the same object
alert( a === b ); // true
```

Independent objects are not equal, even though they look alike (e.g., both are empty):

```
let a = {};
let b = {}; // two independent objects

alert( a == b ); // false
```

## Cloning Objects

Copying an object variable creates one more reference to the same object. If you need to duplicate an object, here are a few ways to do this:

### 1. Iteratively Copying

```
let user = {
  name: "John",
  age: 30
};

let clone = {}; // the new empty object

// copy all user properties into it
for (let key in user) {
  clone[key] = user[key];
}

// now clone is a fully independent object with the same content
clone.name = "Pete"; // changed the data in it

alert( user.name ); // still John in the original object 
```

### 2. Using `Object.assign`

```
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

// copies all properties from permissions1 and permissions2 into user
Object.assign(user, permissions1, permissions2);

// now user = { name: "John", canView: true, canEdit: true }
alert(user.name); // John
alert(user.canView); // true
alert(user.canEdit); // true
```

### 3. Using the ES6 Spread Operator

```
let user = {
  name: "John",
  age: 30
};

let clone = {...user}; // now clone is a fully independent object with the same content
```

> `Object.assign()` and the Spread operator (`{...obj}`) are strictly **Shallow Clones**. If your object has nested objects, they copy the memory reference pointers, not the actual nested values.

### 4. Nested Cloning

Properties can refer to other objects:

```
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

let clone = Object.assign({}, user);

alert( user.sizes === clone.sizes ); // true, same object reference

// user and clone share the same sizes object container
user.sizes.width = 60;    // change a property from one place
alert(clone.sizes.width); // 60, get the result from the other one
```

### 5. `structuredClone`

`structuredClone(object)` clones the object with all nested properties.

```
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

let clone = structuredClone(user);

alert( user.sizes === clone.sizes ); // false, different objects in memory

// user and clone are totally unrelated now
user.sizes.width = 60;    // change a property from one place
alert(clone.sizes.width); // 50, not related
```

The `structuredClone` method can clone most data types, such as objects, arrays, and primitive values. It also supports circular references, where an object property references the object itself (directly or via a chain of references).

```
let user = {};
// circular reference: user.me references the user itself
user.me = user;

let clone = structuredClone(user);
alert(clone.me === clone); // true
```

> `structuredClone` instantly crashes if it hits a property containing a Function/Method or a DOM node reference.

```
// ❌ error
structuredClone({
  f: function() {}
});
```

# The "this" Keyword in Objects

It’s common for an object method to need access to the information stored inside the object to do its job. For instance, the code inside `user.sayHi()` may need the name of the user. To access the object, a method can use the `this` keyword.

The value of `this` is the object “before the dot”, the one used to call the method.

```
let user = {
  name: "John",
  age: 30,
  sayHi() {
    // "this" is the "current object"
    alert(this.name);
  }
};
user.sayHi(); // John
```

## `this` is Not Bound

In JavaScript, the keyword `this` behaves differently from most other programming languages. It can be used in any function, even if it’s not a method of an object.

```
function sayHi() {
  alert( this.name );
}
```

The value of `this` is evaluated during runtime, depending on the invocation context (call-site). For instance, here the same function is assigned to two different objects and resolves to a different `this` in each call:

```
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

// use the same function in two objects
user.f = sayHi;
admin.f = sayHi;

user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)
```

## Calling Without an Object

```
function sayHi() {
  alert(this);
}
sayHi(); // undefined
```

In this case, `this` is `undefined` in strict mode. If we try to access `this.name`, there will be an error. In non-strict mode, the value of `this` falls back to the global object (`window`).

## Arrow Functions Have No `this`

Arrow functions are special: they don’t have their “own” `this`. If we reference `this` from such a function, it is looked up lexically from the outer surrounding execution context.

For instance, here `arrow()` uses `this` from the outer `user.sayHi()` method context:

```
let user = {
  firstName: "Ilya",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi(); // Ilya
```

This special feature of arrow functions is useful when we want to capture and preserve the `this` value of the outer context rather than binding a separate context dynamically.

# Property Descriptors and Object Mutability

In JavaScript, every object property has a hidden set of property descriptors that control how that property behaves.

The main descriptors are:

-   `value` (Data descriptor)
    
-   `writable` (Data descriptor)
    
-   `enumerable` (Data descriptor)
    
-   `configurable` (Data descriptor)
    
-   `get` (Accessor descriptor)
    
-   `set` (Accessor descriptor)
    

These attributes are managed explicitly using:

-   `Object.defineProperty(obj, key, descriptor)`
    
-   `Object.defineProperties(obj, descriptors)`
    
-   `Object.getOwnPropertyDescriptor(obj, key)`
    
-   `Object.getOwnPropertyDescriptors(obj)`
    

When you declare a regular property, JavaScript internally initializes it with all flags set to `true`:

```
const user = { name: "Priyam" };

// Internal descriptor tracking:
// {
//   value: "Priyam",
//   writable: true,
//   enumerable: true,
//   configurable: true
// }
```

## 1. Property Descriptor Flags

### value

The actual data stored inside the property.

```
const obj = {};
Object.defineProperty(obj, "age", {
  value: 25
});
console.log(obj.age); // 25
```

### writable

Controls whether the property's value can be overwritten using an assignment operator.

```
const obj = {};
Object.defineProperty(obj, "name", {
  value: "Priyam",
  writable: true // If set to false, assignments to obj.name would fail silently
});

obj.name = "John";
console.log(obj.name); // "John"
```

### enumerable

Controls whether the property appears during object iterations (such as `for..in` loops or `Object.keys()`). If set to `false`, the property is omitted from iterations. It is frequently used for internal tracking properties, hidden metadata, or framework internals.

```
const obj = {};
Object.defineProperty(obj, "name", {
  value: "Priyam",
  enumerable: false
});

console.log(Object.keys(obj)); // []
```

### configurable

Controls whether the descriptor settings can be modified later, or if the property can be removed from the host object.

**The Core Configuration Rules:**

1.  **Deletes are Blocked:** You cannot use the `delete` operator to strip this property out.
    
2.  **Redefining is Blocked:** You cannot toggle settings like `enumerable` or `configurable` back and forth. Attempting to do so triggers a `TypeError`.
    
3.  **The One Exception:** If `configurable: false`, you are allowed to change `writable` from `true` to `false`. However, you can never change it back from `false` to `true`.
    

```
const obj = {};
Object.defineProperty(obj, "name", {
  value: "Priyam",
  writable: true,       // 🟢 Starts out changeable
  enumerable: false,     // 👁️ Hidden from loops
  configurable: false    // 🔒 Locked down configuration
});

// 1. Deletion fails silently (or throws TypeError in Strict Mode)
delete obj.name;
console.log(obj.name); // "Priyam"

// 2. Redefining structure throws an error
Object.defineProperty(obj, "name", { enumerable: true }); // ❌ TypeError

// 3. THE EXCEPTION: Turning writable from true to false is ALLOWED
Object.defineProperty(obj, "name", { writable: false }); // 🟢 Works perfectly!

// 4. Turning writable back to true is permanently FORBIDDEN
Object.defineProperty(obj, "name", { writable: true }); // ❌ TypeError
```

## 2. Getters and Setters (Accessor Properties)

Instead of storing a value directly, an accessor property executes custom interceptor functions when accessed or assigned.

```
const user = {
  firstName: "Priyam",
  lastName: "Mondal",

  // GETTER: Intercepts read access
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },

  // SETTER: Intercepts write access
  set fullName(value) {
    const [first, last] = value.split(" ");
    this.firstName = first;
    this.lastName = last;
  }
};

console.log(user.fullName); // "Priyam Mondal"

user.fullName = "Rakesh Sharma";
console.log(user.fullName); // "Rakesh Sharma"
```

### Defining Accessors via defineProperty

You can define accessor functions explicitly on existing objects:

```
const obj = {
  first: "Priyam",
  last: "Mondal"
};

Object.defineProperty(obj, "fullName", {
  get() {
    return `${this.first} ${this.last}`;
  },
  set(value) {
    [this.first, this.last] = value.split(" ");
  }
});
```

## 3. Object-Level Mutability Restrictions

These methods let you lock down entire objects at once rather than configuring single properties one by one.

### `Object.preventExtensions(obj)`

Prevents any new properties from being appended to the object. Existing properties remain fully editable and can still be deleted.

```
const obj = { name: "Priyam" };
Object.preventExtensions(obj);

obj.age = 25;
console.log(obj.age); // undefined (Addition ignored)
```

### `Object.seal(obj)`

Prevents additions and deletions of properties by setting `configurable: false` across every single property automatically. Existing properties can still be modified if their `writable` flag is true.

```
const obj = { name: "Priyam" };
Object.seal(obj); // No properties can be added or deleted now
```

### `Object.freeze(obj)`

Applies maximum protection. It disables additions, deletions, and all value re-assignments by setting both `configurable: false` and `writable: false` on all existing properties simultaneously.

```
const obj = { name: "Priyam" };
Object.freeze(obj);

obj.name = "John"; 
console.log(obj.name); // "Priyam" (Value assignment blocked)
```

### Checking Object State

You can inspect the exact structural constraints applied to an object with these utility lookups:

-   `Object.isExtensible(obj)`
    
-   `Object.isSealed(obj)`
    
-   `Object.isFrozen(obj)`
    

## 💡 Practical Common Use-Cases

### 1. Hiding Sensitive or Internal Metadata

```
Object.defineProperty(obj, "_id", {
  value: 123,
  enumerable: false // Prevents leakage during logs, for..in, or JSON serialization
});
```

### 2. Creating Immutable Configuration Constants

```
Object.defineProperty(config, "API_URL", {
  value: "example.com",
  writable: false,
  configurable: false // Cannot be updated or redefined anywhere else in the application
});
```

### 3. Data Cleansing and Encapsulation with Setters

```
Object.defineProperty(user, "email", {
  set(value) {
    this._email = value.trim().toLowerCase(); // Forces clean data storage automatically
  }
});
```

# Prototype and Prototypal Inheritance

Prototypal inheritance is the core mechanism that drives the entire language. Under the hood, JavaScript does not use traditional class-based inheritance (like Java or C++). Instead, it uses a system of objects linking to other objects.

### Prototype

Every object in JavaScript has a secret, built-in property that points to either `null` or another object. This linked object is called its **prototype**.

When you try to read a property or call a method on an object, the JavaScript engine runs a lookup sequence:

1.  It first checks the object's own properties.
    
2.  If it doesn't find it there, it looks at the object's prototype.
    
3.  If it still doesn't find it, it looks at that prototype's prototype, moving up the chain until it hits `null`.
    

This fallback sequence is known as the **Prototype Chain**.

## Setting and Reading Prototypes

Historically, developers used the internal property `__proto__` (pronounced "dunder proto") to get or set prototypes. In modern production code, you should use the official standard standard methods.

### The Modern Standard API

-   `Object.getPrototypeOf(obj)`: Returns the prototype of an object.
    
-   `Object.setPrototypeOf(obj, proto)`: Changes the prototype of an existing object.
    
-   `Object.create(proto)`: Creates a brand new object with a specified prototype linked immediately.
    

```
let animal = {
  eats: true,
  walk() {
    console.log("Animal walking...");
  }
};

// Create rabbit, linking its prototype directly to animal
let rabbit = Object.create(animal);

console.log(rabbit.eats); // 🟢 true (Found on the prototype: animal)
rabbit.walk();           // 🟢 "Animal walking..."

// Checking ownership
console.log(rabbit.hasOwnProperty("eats")); // 🛑 false (Inherited, not its own!)
```

### `Object.create(null)` — The Prototype-less Object

```
const pureObj = Object.create(null);
```

-   This object does not inherit any built-in methods. It has no `.toString()`, no `.__proto__`, and no `.hasOwnProperty()`.
    
-   It is used extensively by library authors for high-security dictionary lookups to prevent malicious users from overriding or breaking built-in methods via prototype pollution vulnerabilities.
    

## `__proto__` vs. prototype

### A. `__proto__` (Object Reference)

Every individual object instance has a `__proto__` reference linking to its architectural prototype. It exists on all objects (Arrays, Functions, Objects).

### B. `prototype` (Constructor Property)

Only Constructor Functions (or Classes) have a `.prototype` property. It is a plain object containing properties and methods that will be assigned as the `__proto__` link for any future instances created using the `new` keyword.

```
function Person(name) {
  this.name = name;
}

// Attaching a method to the constructor's prototype blueprint
Person.prototype.sayName = function() {
  console.log(`My name is ${this.name}`);
};

const user = new Person("Priyam");

// Proof of the structural link:
console.log(user.__proto__ === Person.prototype); // 🟢 true
```

## The "this" Context in Prototypes

No matter where a method is found—whether directly on the object or high up on the prototype chain—the `this` keyword always points to the object before the dot used to invoke the method.

> Prototypes do not share state; they only share structural methods.

```
let user = {
  name: "Guest",
  identify() {
    console.log(`Logged in as: ${this.name}`);
  }
};

let admin = Object.create(user);
admin.name = "Priyam"; // Sets 'name' directly on admin, doesn't touch user!

admin.identify(); // 🟢 "Logged in as: Priyam" ('this' points to admin)
user.identify();  // 🟢 "Logged in as: Guest"  ('this' points to user)
```

## Prototypal Shadowing (Overriding)

If an object defines a property with the exact same name as a property sitting higher up on its prototype chain, the object's property **shadows** (hides) the prototype's version.

```
let parent = { job: "Unemployed" };
let child = Object.create(parent);

console.log(child.job); // "Unemployed" (Read from prototype)

child.job = "Engineer"; // Shadows the parent's property
console.log(child.job);  // 🟢 "Engineer" (Read directly from child)
console.log(parent.job); // 🟢 "Unemployed" (Parent remains completely untouched)
```

## Performance and Memory Optimization

Prototypal inheritance is highly optimized for performance and memory management.

Instead of copying functions into every single object instance (which wastes RAM when creating thousands of objects), you store the method once on the prototype blueprint. Every instance shares the exact same memory address for that function.

```
// ❌ Bad memory practice: creates a new function copy inside every single instance
function BadUser(name) {
  this.name = name;
  this.login = function() { /* ... */ }; 
}

// 🟢 Good memory practice: One function shared in memory by all instances
function GoodUser(name) {
  this.name = name;
}
GoodUser.prototype.login = function() { /* ... */ };
```

# Symbol Type

By specification, only two primitive types may serve as object property keys:

-   String type
    
-   Symbol type
    

If you use any other type (such as a number or boolean), the JavaScript engine auto-converts it to a string behind the scenes. This makes `obj[1]` identical to `obj["1"]`, and `obj[true]` identical to `obj["true"]`.

## What is a Symbol?

A Symbol represents a completely unique identifier. A value of this type can be instantiated using the factory function `Symbol()`:

```
let id = Symbol();
```

Upon creation, we can give symbols an optional description (also known as a symbol name). This description is used primarily for debugging purposes:

```
// id is a symbol with the description "id"
let id = Symbol("id");
```

### Guaranteed Uniqueness

Symbols are guaranteed to be structurally unique. Even if we create multiple symbols with the exact same description string, they are completely distinct values in memory:

```
let id1 = Symbol("id");
let id2 = Symbol("id");

alert(id1 == id2); // false
```

### Explicit Conversion Guard

Most primitive values in JavaScript support implicit conversion to a string. Symbols are special and do not auto-convert:

```
let id = Symbol("id");
alert(id); // ❌ TypeError: Cannot convert a Symbol value to a string
```

This acts as a built-in language guard to prevent mixing up fundamentally different primitive data types. If you want to log or display a symbol, you must call `.toString()` explicitly:

```
let id = Symbol("id");
alert(id.toString()); // "Symbol(id)"
```

## Symbols as Object Properties

To use a symbol as a key inside an object literal `{...}`, you must wrap the symbol variable name in computed property square brackets:

```
let id = Symbol("id");

let user = {
  name: "John",
  [id]: 123 // Using computed syntax, not "id": 123
};
```

### Hidden Properties Principle

Symbolic properties are intentionally skipped by standard object parsing mechanisms:

-   They do not participate in `for..in` loops.
    
-   `Object.keys(user)` and `Object.values(user)` completely ignore them.
    

```
let id = Symbol("id");
let user = {
  name: "John",
  age: 30,
  [id]: 123
};

for (let key in user) {
  alert(key); // Prints: "name", then "age" (symbol key is skipped)
}

// Direct key access via the symbol variable still works perfectly
alert("Direct: " + user[id]); // Direct: 123
```

This encapsulation ensures that external scripts, tools, or third-party libraries looping over your objects cannot unexpectedly discover or modify these symbolic configurations.

### The Object.assign Exception

While loop mechanisms hide symbols, `Object.assign()` breaks this rule. It copies both string and symbol properties when shallow-cloning:

```
let id = Symbol("id");
let user = {
  [id]: 123
};

let clone = Object.assign({}, user);
alert(clone[id]); // 123
```

## Global Symbol Registry

Sometimes we want completely separate parts of our codebase (or different independent scripts) to share access to the exact same structural symbol. To solve this, JavaScript maintains a runtime **Global Symbol Registry**.

### `Symbol.for(key)`

This method searches the global registry for a symbol matching the given description string. If found, it returns that symbol reference. If it doesn't exist, it instantiates it globally first, then hands it back:

```
// Read from the global registry (instantiates it here)
let id = Symbol.for("id"); 

// Read it again from another part of the code
let idAgain = Symbol.for("id");

// Both variables point to the exact same registry reference
alert(id === idAgain); // true
```

### Reverse Registry Lookups: `Symbol.keyFor(sym)`

To pass a global symbol in and retrieve its global registry description string, use `Symbol.keyFor()`:

```
let sym = Symbol.for("name");
let sym2 = Symbol.for("id");

alert(Symbol.keyFor(sym));  // "name"
alert(Symbol.keyFor(sym2)); // "id"
```

### Registry Symbols vs. Local Symbols

`Symbol.keyFor()` uses the global registry space. It cannot parse regular, locally instantiated symbols. If a symbol is not global, it returns `undefined`. However, all symbols have a native `.description` getter property to read their raw labels locally:

```
let globalSymbol = Symbol.for("name");
let localSymbol = Symbol("name");

alert(Symbol.keyFor(globalSymbol)); // "name" (Found in registry)
alert(Symbol.keyFor(localSymbol));  // undefined (Local symbol not in registry)

alert(localSymbol.description);     // "name" (Read via instance property)
```

> For additional details, see the official [JavaScript.info Symbol Guide](https://javascript.info/symbol#symbols-in-an-object-literal).
# Metaprogramming: Proxy and Reflect

## Proxy

An object that wraps a target object and intercepts its fundamental lower-level operations (like reading, writing, or deleting properties). These interceptions are called **Traps**.

## Reflect

A built-in global object containing static methods that match the exact signatures of the Proxy traps. It is used to forward and perform the original, unaltered behavior inside a trap safely.

## Why Reflect was Added to JavaScript

### 1. Unified Namespace Architecture

Historically, if you wanted to perform basic object manipulation operations, you had to combine keywords with entirely different global runtime APIs:

-   **To delete a key:** `delete obj.key` _(keyword construct)_
    
-   **To check if a key exists:** `"key" in obj` _(keyword construct)_
    
-   **To configure property descriptors:** `Object.defineProperty()` _(global static method)_
    

Reflect unifies all of these operations under a single, centralized namespace: `Reflect.deleteProperty()`, `Reflect.has()`, and `Reflect.defineProperty()`.

### 2. Streamlined Error Handling

Older legacy methods like `Object.defineProperty()` throw a fatal, app-crashing `TypeError` if an operation fails (e.g., attempting to modify a property on a non-configurable or frozen object). This legacy behavior forces you to wrap basic metadata modifications in verbose `try...catch` blocks.

Reflect methods do not throw exceptions on standard execution failures; instead, they return a simple `true` or `false` boolean flag indicating whether the action succeeded.

```
const user = { name: "Priyam" };
Object.freeze(user); // Lock the object completely

// ❌ The Old Way: Drops a fatal crash error unless caught
try {
  Object.defineProperty(user, "age", { value: 25 });
} catch (e) {
  console.log("Failed to modify!");
}

// 🟢 The Reflect Way: Gracefully returns a boolean flag
const success = Reflect.defineProperty(user, "age", { value: 25 });
if (!success) {
  console.log("Failed to modify, but code kept running smoothly!");
}
```

## Creating a Basic Proxy

To create a Proxy, you instantiate a `new Proxy()` constructor passing it two arguments: the **target** (the raw object data) and the **handler** (the configuration object containing your custom interceptor traps).

```
const targetObj = { name: "Priyam", score: 85 };

const handler = {
  // 'get' trap intercepts whenever someone reads a property
  get(target, prop, receiver) {
    console.log(`🔍 Intercepted READ for property: ${prop}`);
    return Reflect.get(target, prop, receiver); // Forward action safely
  },

  // 'set' trap intercepts whenever someone writes to a property
  set(target, prop, value, receiver) {
    console.log(`✍️ Intercepted WRITE for property: ${prop} to value: ${value}`);
    return Reflect.set(target, prop, value, receiver); // Save action safely
  }
};

const proxyObj = new Proxy(targetObj, handler);
```

### Signature Mapping

Every trap you can write in a Proxy has a perfectly identical matching utility counterpart method in Reflect:

Plaintext

```
get(target, prop, receiver)        ──► Reflect.get(...)
set(target, prop, val, receiver)   ──► Reflect.set(...)
has(target, prop)                  ──► Reflect.has(...)
deleteProperty(target, prop)       ──► Reflect.deleteProperty(...)
ownKeys(target)                    ──► Reflect.ownKeys(...)
```

## Real-World Production Uses

### A. Data Validation & Type Safety

```
const userProfile = { username: "priyam_m", age: 24 };

const secureProfile = new Proxy(userProfile, {
  set(target, prop, value, receiver) {
    if (prop === "age") {
      if (typeof value !== "number" || value < 0) {
        throw new TypeError("Age must be a positive number!"); // 🛑 Blocks invalid data entries
      }
    }
    // 🟢 If data passes validation, use Reflect to persist it safely
    return Reflect.set(target, prop, value, receiver);
  }
});

secureProfile.age = 25;       // 🟢 Works perfectly
// secureProfile.age = "twenty"; // ❌ Throws TypeError immediately!
```

### B. Schema Protection & Privacy

```
const DBRecord = {
  _id: "SECRET_999",
  title: "Master Database",
  status: "active"
};

const hiddenRecord = new Proxy(DBRecord, {
  // Hide internal fields from "prop in object" lookups
  has(target, prop) {
    if (prop.startsWith("_")) return false; // 🔒 Emulate non-existence
    return Reflect.has(target, prop);
  },
  
  // Hide internal fields from Object.keys() and loops
  ownKeys(target) {
    return Reflect.ownKeys(target).filter(prop => !prop.startsWith("_"));
  }
});

console.log("_id" in hiddenRecord);        // 🛑 false
console.log(Object.keys(hiddenRecord)); // 🟢 ["title", "status"] (Secret key is filtered out!)
```

### C. Setting Fallbacks for Missing Keys

```
const translation = { hello: "Bonjour", goodbye: "Au revoir" };

const safeTranslation = new Proxy(translation, {
  get(target, prop, receiver) {
    if (!Reflect.has(target, prop)) {
      return `⚠️ [Key "${prop}" missing]`; // 🟢 Return custom fallback message instead of undefined
    }
    return Reflect.get(target, prop, receiver);
  }
});

console.log(safeTranslation.hello);   // "Bonjour"
console.log(safeTranslation.welcome); // "⚠️ [Key "welcome" missing]"
```

## ⚠️ Architectural Warning: The Context Trap

A common mistake when writing a `get` trap is directly returning the raw property lookup from the target reference:

```
// ❌ Dangerous Anti-Pattern
get(target, prop) {
  return target[prop]; 
}
```

**Why this breaks your code:** If another object subsequently inherits your proxy via prototypal inheritance (e.g., `const subclass = Object.create(proxyObj)`), using `target[prop]` breaks the dynamic execution binding of the `this` keyword. It forces `this` inside methods to point backward to the original base proxy target object rather than evaluating the current active calling instance.

Always pass along the `receiver` parameter using Reflect:

```
// 🟢 Production Standard Best Practice
get(target, prop, receiver) {
  return Reflect.get(target, prop, receiver); 
}
```

The `receiver` argument acts as a precise context tracking link, ensuring that the `this` context binds to the correct object executing the operation anywhere up and down the prototype chain.

# Asynchronous JavaScript and the Event Loop

Asynchronous engineering in JavaScript evolved across three major eras: **Callbacks** ──► **Promises (`.then()`)** ──► **async/await**.

## The Pre-ES6 Era: Pure Callbacks

Before 2015, JavaScript handled all asynchronous execution paths using callbacks processed through the browser's Macrotask Queue (Callback Queue).

### The Two Major Architectural Flaws

#### A. Callback Hell (The Pyramid of Doom)

When asynchronous operations depend on the results of previous ones, code nests deeply inside functions. This forces code to grow horizontally rather than vertically, making it brittle and unreadable.

```
getUser(id, function(user) {
  getPosts(user.id, function(posts) {
    getComments(posts[0].id, function(comments) {
      // 😵 Deeply nested, unmaintainable dependency tree
    });
  });
});
```

#### B. Inversion of Control (Loss of Trust)

When you pass a callback function to a third-party script or external API, you surrender control of your execution thread. You cannot guarantee:

-   If the callback will be called at all.
    
-   If it will be called multiple times accidentally (e.g., a payment gateway charging a card twice).
    
-   If it will be called synchronously instead of asynchronously, breaking variable tracking.
    

## The ES6 Paradigm Shift: Promises

Introduced in ES6 (2015), a Promise is a native language object that acts as a trustable placeholder container for the eventual result of an asynchronous operation.

Promises solve Inversion of Control by bringing control back to your script: instead of passing a callback to an external function, the external function immediately hands a standard Promise object back to you.

### The 3 Immutable States of a Promise

A promise state can only change once—moving permanently from Pending to either Fulfilled or Rejected. It can never change states again.

-   **Pending 🟡** – Initial state; the async task is still processing.
    
-   **Fulfilled ✅** – Operation succeeded; `resolve(value)` was triggered.
    
-   **Rejected ❌** – Operation failed; `reject(error)` was triggered.
    

```
const executor = new Promise((resolve, reject) => {
  const success = true;
  success ? resolve("Done!") : reject("Failed!");
});

executor
  .then(result => console.log("Success:", result))     // Runs on Fulfilled
  .catch(error => console.error("Error:", error))     // Runs on Rejected
  .finally(() => console.log("Cleanup complete."));   // Runs on Settlement (any state)
```

### The 4 Crucial Promise Combinators

When managing parallel asynchronous pipelines, you must deploy the exact combinator matching your architectural requirements:

-   **`Promise.all([p1, p2])`**
    
    -   _Behavior:_ All-or-Nothing. Runs arrays in parallel.
        
    -   _Result:_ Resolves only if every single promise succeeds. Short-circuits and rejects instantly if even one promise fails.
        
-   **`Promise.allSettled([p1, p2])`**
    
    -   _Behavior:_ No short-circuiting.
        
    -   _Result:_ Waits for all input promises to settle (win or lose) and returns an array detailing the exact outcome status of each item.
        
-   **`Promise.race([p1, p2])`**
    
    -   _Behavior:_ Speed-driven.
        
    -   _Result:_ Settles immediately as soon as the very first promise in the array settles (whether it resolves or rejects). Ideal for network timeouts.
        
-   **`Promise.any([p1, p2])`**
    
    -   _Behavior:_ First-success optimization.
        
    -   _Result:_ Resolves as soon as the first successful promise resolves. It ignores all rejections completely unless every single promise fails.

    -   _Interview drill:_ `Promise.race` settles on the first outcome, including a rejection; `Promise.any` fulfills on the first fulfillment. When implementing `promiseAny(iterable)`, how would you normalize values and thenables, handle empty input, and retain every rejection reason if all candidates fail? Use `Promise.resolve` for normalization and reject only then with an `AggregateError` whose `errors` stay in input iteration order—not completion order. Practice the edge cases with a [free question and full reference answer](https://frontendatlas.com/javascript/coding/js-promise-any).
        

## Architectural Ownership: Engine vs. Host

To understand asynchronous execution, we must split components by who owns them:

### What the JavaScript Engine Natively Owns (Pure JS)

The JavaScript language specification (ECMAScript) strictly dictates everything that happens inside the Engine (like Google's V8 or Safari's JavaScriptCore). The engine owns:

-   **The Call Stack:** Executes your synchronous code frames line-by-line (Last-In, First-Out).
    
-   **The Memory Heap:** Allocates memory space for your objects, variables, and closures.
    
-   **The Microtask Queue (Job Queue):** The JavaScript specification explicitly mandates the creation of the Microtask Queue specifically to manage Promises (`.then`, `.catch`, `async/await`).
    

Because Promises own their own queue inside the engine, they are a fundamental language feature.

### What the Host Environment Owns (Browser or Node.js)

The JavaScript engine by itself is completely single-threaded and synchronous. It has no native ability to talk to the internet, wait for a timer, or read a file. The host environment provides the actual asynchronous heavy lifters:

-   **Web APIs / Platform APIs:** Features like `fetch()`, `setTimeout()`, and DOM events are not part of JavaScript. They are external tools provided by the browser (or Node.js C++ APIs) that run on separate background threads.
    
-   **The Macrotask Queue (Callback Queue):** Where standard environmental async callbacks wait (like a `setTimeout` finishing its countdown).
    
-   **The Event Loop:** A coordination mechanism provided by the environment that constantly monitors the Call Stack and the queues to decide what runs next.
    

### How They Work Together (Execution Priority Example)

```
console.log("1. Sync");

setTimeout(() => console.log("2. MacroTask (Browser)"), 0);

Promise.resolve().then(() => console.log("3. MicroTask (Pure JS)"));

console.log("4. Sync");
```

#### The Step-by-Step Execution Flow:

1.  **Synchronous Run:** `console.log("1. Sync")` and `console.log("4. Sync")` hit the Call Stack and execute instantly.
    
2.  **The Browser Hand-off:** The engine encounters `setTimeout`. Since timers belong to the browser, the engine hands it off to the Browser's Web API thread. When the timer hits 0, the browser drops the callback into the **Macrotask Queue**.
    
3.  **The Engine Retention:** The engine encounters the Promise. Because Promises are native JavaScript, the engine keeps this operation internal and schedules the `.then()` callback directly into its own internal **Microtask Queue**.
    
4.  **The Event Loop Interception:** Once the Call Stack is completely empty, the Event Loop wakes up to process the queues. The engine forces strict priority: **It will completely empty the native Microtask Queue before it even looks at the browser's Macrotask Queue.**
    

#### Output

Plaintext

```
1. Sync
4. Sync
3. MicroTask (Pure JS)       <-- Standard JS Promise wins priority!
2. MacroTask (Browser)      <-- Browser tool runs last.
```

# Asynchronous Patterns: .then() vs. async/await

Introduced in ES8 (2017), `async/await` is a layer of syntactic sugar built directly over Promises and Generator Functions. It completely flattens asynchronous code, transforming nested chaining arrays into clean, linear, sequential blocks.

-   **`async` keyword:** Automatically wraps the return value of any function in a resolved Promise.
    
-   **`await` keyword:** Literally pauses execution within the local function context. It slices off the remainder of the function, steps off the Call Stack to keep the main thread completely responsive, and waits for the targeted promise to settle. Once settled, the remaining block is injected into the engine's high-priority Microtask Queue to resume execution.
    

While both mechanisms use the native Microtask Queue to handle asynchronous results, they handle the execution stream of your code differently. `async/await` pauses the execution of its local function block, whereas `.then()` schedules a callback and lets the rest of the function continue running immediately.

## 1. What happens when the engine encounters `.then()`

The `.then()` approach is completely non-blocking to the rest of the surrounding function. When the engine hits a line with `.then()`, it splits the execution path.

```
function processThen() {
  console.log("1. Sync Start");

  // A. The promise initializes synchronously
  fetch("https://api.example.com/data") 
    .then(res => {
      // C. Sometime later, this callback is pushed to the Microtask Queue
      console.log("3. Async Response handling"); 
    });

  // B. The engine instantly skips down here without waiting for the fetch
  console.log("2. Sync End"); 
}
```

### The Step-by-Step Flow

1.  **Invocation:** The engine starts executing `processThen()` line-by-line on the Call Stack. It prints `"1. Sync Start"`.
    
2.  **Handoff:** It encounters `fetch()`. The engine starts the request, hands the networking task over to the browser’s background threads, and registers the function inside `.then()` as a pending callback.
    
3.  **No Pausing:** The engine does not stop. It immediately exits the promise block and executes the next line, printing `"2. Sync End"`. The function finishes executing and leaves the Call Stack.
    
4.  **Resolution:** When the browser's background thread finishes downloading the data, the Promise changes to "fulfilled." The engine takes the callback inside `.then()` and drops it into the Microtask Queue.
    
5.  **Execution:** Once the primary script is finished and the Call Stack is completely empty, the Event Loop pulls `"3. Async Response handling"` out of the Microtask Queue and runs it.
    

## 2. What happens when the engine encounters `async/await`

```
async function processAwait() {
  console.log("1. Sync Start");

  // A. The expression to the right of await runs synchronously
  // B. The rest of this function is sliced off and suspended!
  const res = await fetch("https://api.example.com/data"); 

  // C. This code only runs after the suspended function wakes up
  console.log("3. Async Response handling"); 
}

processAwait();
console.log("2. Global Sync continuation");
```

### The Step-by-Step Flow

1.  **Invocation:** `processAwait()` enters the Call Stack. It prints `"1. Sync Start"`.
    
2.  **The Right-Side Run:** The engine encounters `await fetch(...)`. Crucial nuance: **The code directly to the right of the `await` keyword runs synchronously first.** So, the `fetch()` request is fired off immediately to the browser background threads.
    
3.  **The Freeze and Slice:** The moment that fetch is fired, the `await` keyword pauses the `processAwait()` function. The engine takes the entire remainder of the function (everything below the await line), wraps it up into a hidden microtask closure, and pops the function off the Call Stack.
    
4.  **The Unblocked Main Thread:** Because `processAwait()` stepped off the Call Stack, the main thread is not frozen. The engine jumps right down to the next global line and prints `"2. Global Sync continuation"`.
    
5.  **The Wake-Up Call:** When the network request finishes, the browser alerts the engine. The engine takes that wrapped-up remainder of our function and places it into the Microtask Queue.
    
6.  **Resuming:** Once the Call Stack is empty, the Event Loop grabs the remaining block from the Microtask Queue, pushes `processAwait()` back onto the Call Stack right where it left off, populates the `res` variable with the data, and finally executes line `"3. Async Response handling"`.
    

## 🔄 The Event Loop Tick Rhythm

Plaintext

```
Step 1: Grab the OLDEST single task from the Macrotask Queue.
Step 2: Push it onto the Call Stack and execute it until it finishes.
        │
        ▼ 
Step 3: Is the Call Stack empty? Yes.
Step 4: Check the Microtask Queue. 
        Are there tasks inside? 
        ├──► YES: Pull ALL of them out and execute them one by one 
        │         until the Microtask Queue is completely empty (0 tasks).
        └──► NO:  Move to Step 5.
        │
        ▼
Step 5: Render UI updates (if running in a browser environment and a redraw is needed).
Step 6: Loop back to Step 1 and grab the NEXT single Macrotask.
```

### Microtask Starvation (The Event Loop Bottleneck)

If a microtask continuously schedules another microtask (e.g., a recursive promise chain), the engine will remain permanently trapped inside the Microtask Queue. The browser will never move to the Macrotask Queue, user I/O callbacks will hang, and the browser UI thread will freeze.

```
// 🛑 WARNING: Microtask Starvation Bug / Infinite Loop
function starveEventLoop() {
  Promise.resolve().then(() => {
    // This microtask immediately spawns another microtask
    starveEventLoop(); 
  });
}
```

# Advanced JavaScript Error Handling

`try...catch` is a synchronous block statement, while `.catch()` is an instance method called on an asynchronous Promise object.

## 1. `try...catch` (The Statement Approach)

This is a native control-flow block structure built directly into JavaScript. It watches a specific block of code and immediately diverts execution to the catch block if any synchronous error is thrown.

```
try {
  let user = JSON.parse(brokenJson); // Throws a synchronous SyntaxError
  console.log(user);
} catch (error) {
  console.error("Caught a synchronous error:", error.message);
}
```

> Because `try...catch` is completely synchronous, it executes and finishes immediately on the current Call Stack frame. It cannot intercept something that happens in the future.

```
// ❌ Anti-Pattern: This try...catch is blind to the asynchronous exception
try {
  setTimeout(() => {
    throw new Error("Boom!"); // Thrown 1 second later
  }, 1000);
} catch (e) {
  console.log("This line will NEVER run!"); 
}
```

The `try` block executes, schedules the timer with the browser, and finishes running instantly. One second later, when the timer callback executes on a completely fresh Call Stack frame, the `try...catch` block is already long gone from memory. The error goes unhandled.

The exact same thing happens if you try to wrap a raw Promise in a synchronous `try...catch`:

```
// ❌ BROKEN: The try...catch cannot see the rejection inside the microtask queue
try {
  Promise.reject("Database Error");
} catch (e) {
  console.log("Will not catch this!"); 
}
```

## 2. `.catch()` (The Method Approach)

This is an instance method belonging to the `Promise.prototype` object. It does not monitor a block of code; instead, it registers a callback function that listens down a Promise Chain for a rejected state signal.

```
fetch("https://api.example.com/data") // Returns a Promise
  .then(res => res.json())
  .catch(error => {
    console.error("Caught an asynchronous promise rejection:", error);
  });
```

## 3. The Unifier: `async / await` with `try...catch`

The `async/await` syntax acts as a structural bridge. Because the `await` keyword pauses the function context and steps off the Call Stack, it forces an asynchronous microtask rejection to behave like a local, synchronous runtime error.

This allows you to use standard `try...catch` blocks for asynchronous code natively.

```
async function loadData() {
  try {
    // Because of 'await', if the fetch rejects, it gets thrown 
    // directly into this local catch block!
    const res = await fetch("https://api.example.com/data");
    const data = await res.json();
    return data;
  } catch (error) {
    console.error("Caught an ASYNC error using SYNC syntax:", error);
  }
}
```

## ⚡ Critical Engineering Nuances

### Nuance A: The Silent Danger of Unhandled Rejections

If an asynchronous Promise or async function throws an error/rejects, and you forget to attach a `.catch()` or wrap it in a `try...catch`, the error does not surface on the standard synchronous exception layer. Instead, it lingers silently inside the engine's internal memory pool.

To prevent silent application crashes, implement a global native event listener:

```
// Global catch-all guardrail for browser environments:
window.addEventListener("unhandledrejection", (event) => {
  console.error("Critical: Unhandled Promise Rejection Detected!", event.reason);
  // Optional: Send to logging service (e.g., Sentry)
});
```

### Nuance B: The `await` Return Trap

A common architectural bug happens when you try to catch an error from an async utility function but return the promise without using the `await` keyword inside the `try` block.

```
// ❌ BROKEN ARCHITECTURE
async function getResource() {
  try {
    return fetch("https://api.broken-url.com"); // Missing 'await'!
  } catch (error) {
    console.log("This catch block is bypassed!"); // NEVER RUNS
  }
}
```

Without `await`, the function immediately passes the pending promise object out of the function block. The function finishes execution successfully, exits the `try...catch`, and when the network request eventually rejects inside the microtask queue moments later, the local catch block is no longer listening.

```
// 🟢 CORRECT ARCHITECTURE
async function getResource() {
  try {
    return await fetch("https://api.broken-url.com"); // 'await' forces local resolution
  } catch (error) {
    console.log("Successfully caught the network failure here!"); 
  }
}
```

### Nuance C: `finally {}` vs. `.finally()` Execution Rule

Both block variants accept a clean-up frame that is guaranteed to run whether the main path succeeded or exploded. The synchronous `finally { ... }` block statement behaves with the exact same priority as the asynchronous `.finally()` method.

If a `return` statement is encountered inside a `try` or a `catch` block, the JavaScript engine does not exit the function immediately. Instead, **it pauses the return operation, executes everything inside the `finally { ... }` block first, and only then completes the return.**

> Never write a `return` statement inside a `finally {}` block. If the `finally` block returns a value, it will completely overwrite and discard any return values that were previously prepared by the `try` or `catch` blocks.


---

## 🤝 Contributing & Feedback

This manual is completely open-source and community-driven. JavaScript is a massive ecosystem with continuous engine updates—if you spot a technical anomaly, an outdated concept, a syntax typo, or want to add a tricky interview problem you recently faced, contributions are highly encouraged!

### How to Contribute:
1. **Fork** the repository.
2. Create a feature branch (`git checkout -b feature/AmazingContribution`).
3. Commit your updates (`git commit -m 'docs: update Event Loop edge-cases'`).
4. Push to the branch (`git push origin feature/AmazingContribution`).
5. Open a **Pull Request** (PR).

*Alternatively, if you find an issue but don't have time to fix it yourself, feel free to open a detailed **[GitHub Issue](https://github.com/Priyammondal/javascript-interview-manual/issues)**.*

---

<p align="center">
  If this manual helped you ace an interview or learn something new, consider giving it a 🌟 <b>Star</b> to support the project!
</p>