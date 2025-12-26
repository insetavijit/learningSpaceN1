---
title: "JavaScript - Quick Reference Map"
type: doc-post
framework: JavaScript
tags:
  - javascript
  - js
  - frontend
  - programming
  - cheatsheet
  - quick-reference
created: 2025-12-26
---

## 🔹 1.1 JavaScript Fundamentals & Setup

- **History & Standards** : JavaScript origins, ECMAScript (ES5 → ESNext)
    
- **Strict Mode** : `"use strict"`
    
- **Variables** : `var`, `let`, `const`
    
- **Data Types**
    
    - **Primitive** : `string`, `number`, `boolean`, `null`, `undefined`, `symbol`, `bigint`
        
    - **Reference** : `object`, `function`
        
- **Type Handling** : coercion vs explicit conversion
    
- **Operators** : arithmetic, comparison, logical, bitwise, ternary `?:`
    
- **Expressions vs Statements**
    
- **Comments** : `//`, `/* ... */`
    
- **Console** : `log`, `warn`, `error`, `table`, `time`
    
- **Script Loading**
    
    - Placement : `<head>` vs end of `<body>`
        
    - Attributes : `defer`, `async`, `type="module"`
        
- **Modules (ESM)** : `import`, `export`
    
- **Transpilation** : Babel
    
- **Bundlers** : Webpack, Vite, Parcel, Rollup
    
- **Environments** : Browser vs Node.js
    



## 🔹 1.2 Control Flow & Error Handling

- **Conditionals** : `if/else`, `switch`, ternary
    
- **Loops** : `for`, `for...of`, `for...in`, `while`, `do...while`
    
- **Flow Control** : `break`, `continue`, labels
    
- **Exceptions**
    
    - `try / catch / finally`
        
    - `throw`
        
- **Error Objects** : `Error`, custom errors
    
- **Async Errors** : `try/catch` with `await`, `.catch()`
    
- **Safe Access** : optional chaining `?.`
    
- **Defaults** : nullish coalescing `??`
    



## 🔹 1.3 Functions & Scope

- **Function Forms** : declarations, expressions, arrow `()=>{}`
    
- **Parameters** : default, rest `...args`, destructuring
    
- **Returns** : `return` values
    
- **IIFE** : immediately invoked functions
    
- **Closures** : functions remembering scope
    
- **Scope** : lexical, block, global
    
- **Hoisting** : vars, functions
    
- **`this` Keyword**
    
    - Context rules
        
    - Binding : `call`, `apply`, `bind`
        
    - Arrow lexical `this`
        
- **Higher-Order Functions** : functions as args/returns
    
- **Recursion**
    
- **Generators** : `function*`, `yield`
    
- **Async Functions** : `async / await`
    



## 🔹 1.4 Objects & Data Structures

- **Object Literals** : `{ key: value }`
    
- **Property Access** : dot, bracket, computed
    
- **Shorthand Syntax**
    
- **Destructuring** : `{a} = obj`
    
- **Spread / Rest** : `{...obj}`, `...rest`
    
- **Object Methods** : `keys`, `values`, `entries`, `assign`, `freeze`
    
- **Prototypes** : prototype chain, inheritance
    
- **Classes** : `class`, `constructor`, methods, `extends`, `super`
    
- **Maps & Sets** : `Map`, `Set`
    
- **Weak Collections** : `WeakMap`, `WeakSet`
    
- **JSON** : `JSON.parse`, `JSON.stringify`
    



## 🔹 1.5 Arrays & Iteration

- **Array Basics** : literals `[]`, indexing
    
- **Mutation Methods** : `push`, `pop`, `shift`, `unshift`, `splice`
    
- **Non-Mutating** : `slice`, `concat`
    
- **Iteration**
    
    - `forEach`
        
    - `map`
        
    - `filter`
        
    - `reduce`
        
    - `find`
        
    - `some`, `every`
        
- **Spread** : `[...arr]`
    
- **Destructuring** : `[a, b] = arr`
    
- **Factories** : `Array.from`, `Array.of`
    
- **Typed Arrays** : `Uint8Array`, etc.
    
- **Iterators** : `Symbol.iterator`
    



## 🔹 1.6 DOM Manipulation & Events

- **DOM Concept** : Document Object Model
    
- **Selecting** : `querySelector`, `getElementById`, `getElementsByClassName`
    
- **Traversing** : parent, children, siblings
    
- **Modifying**
    
    - `createElement`
        
    - `append`, `prepend`
        
    - `remove`
        
- **Attributes & Props**
    
- **Classes & Data** : `classList`, `dataset`
    
- **Content** : `innerHTML` vs `textContent`
    
- **Events**
    
    - `addEventListener`
        
    - Event object
        
    - Bubbling vs capturing
        
- **Delegation**
    
- **Common Events** : `click`, `input`, `submit`, `load`, `resize`, `scroll`
    
- **Prevent Default** : `event.preventDefault()`
    
- **Custom Events** : `new CustomEvent()`
    



## 🔹 1.7 Asynchronous JavaScript

- **Callbacks** : patterns, callback hell
    
- **Promises**
    
    - `then`, `catch`, `finally`
        
    - `Promise.all`, `race`, `allSettled`, `any`
        
- **Async/Await** : sequential async flow
    
- **Fetch API** : `fetch()`, Response handling
    
- **Async Errors** : try/catch patterns
    
- **Event Loop**
    
    - Call stack
        
    - Microtasks (promises)
        
    - Macrotasks (timers)
        
- **Web APIs** : `setTimeout`, `setInterval`, `requestAnimationFrame`
    



## 🔹 1.8 Advanced & Modern Features

- **Advanced Destructuring & Spread**
    
- **Optional Chaining / Nullish** : `?.`, `??`
    
- **New Primitives** : `BigInt`, `Symbol`
    
- **Meta Programming** : `Proxy`, `Reflect`
    
- **Iterators & Generators** : deep usage
    
- **Async Iteration** : `for await...of`
    
- **Modules Advanced** : dynamic `import()`, top-level `await`
    
- **Temporal API** : emerging date/time
    
- **Private Fields** : `#private`
    
- **Decorators** : stage 3 proposal
    
- **Logical Assignment** : `||=`, `&&=`, `??=`
    



## 🔹 1.9 Browser APIs & Web Features

- **Window & Document**
    
- **BOM** : `location`, `history`, `navigator`, `screen`
    
- **Storage**
    
    - `localStorage`
        
    - `sessionStorage`
        
    - cookies
        
- **Workers** : Web Workers
    
- **Service Workers** : PWA basics
    
- **Geolocation API**
    
- **Graphics** : Canvas, WebGL basics
    
- **Media APIs** : Audio, Video
    
- **Clipboard API**
    
- **Observers**
    
    - Intersection Observer
        
    - Resize Observer
        
- **Performance API**
    
- **Fetch Control** : `AbortController`
    



## 🔹 1.10 Tooling, Testing & Best Practices

- **Linting** : ESLint
    
- **Formatting** : Prettier
    
- **Debugging** : breakpoints, source maps
    
- **Unit Testing** : Jest, Vitest
    
- **E2E Testing** : Cypress, Playwright
    
- **Type Safety** : TypeScript basics
    
- **Security** : CSP, XSS prevention
    
- **Performance**
    
    - Debouncing
        
    - Throttling
        
    - Lazy loading
        
- **Accessibility in JS**
    
    - Focus management
        
    - ARIA live regions
        



If you would like, I can now:

- Add **`[[Wiki Links]]`** to each major topic for deep notes.
    
- Break this into **one note per chapter** with backlinks.
    
- Or compile **HTML + CSS + JS maps into a unified Frontend Master Index** note for your vault.