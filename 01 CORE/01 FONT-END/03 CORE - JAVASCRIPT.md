### **1.1 JavaScript Language Fundamentals**

Master modern JavaScript (ES6+) as the essential language of web development. Learn variables, types, operators, and ES6+ powerful features (arrow functions, destructuring, spread/rest, template literals). Think in modern JavaScript—not legacy ES5, but module-based, async-ready JavaScript used in production. Understand strict mode, scope, hoisting, and modern syntax. This prevents writing outdated code and establishes patterns for React/Node.js. Jobs expect ES6+ knowledge; legacy JavaScript skills are insufficient.

|Topic|Focus & Purpose|
|---|---|
|**[[1.1.1 Modern JavaScript Setup]]**|Node.js installation; npm/yarn; package.json; local development environment; browser console; VS Code setup; ES modules vs CommonJS; JavaScript runtime environments. Professional environment setup.|
|**[[1.1.2 Variables & Data Types]]**|let, const, var differences; block scope; primitives (string, number, boolean, null, undefined, symbol, bigint); typeof operator; type coercion; truthy/falsy values; strict equality (===). Modern variable declaration.|
|**[[1.1.3 Operators & Expressions]]**|Arithmetic, comparison, logical operators; nullish coalescing (??); optional chaining (?.); spread operator (...); rest parameters; destructuring (arrays, objects); ternary operator; short-circuit evaluation. Modern syntax patterns.|
|**[[1.1.4 Functions & Arrow Functions]]**|Function declarations vs expressions; arrow functions; parameters and arguments; default parameters; rest parameters; return statements; function scope; IIFE pattern; callbacks; function hoisting. Core JavaScript concept.|
|**[[1.1.5 Arrays & Array Methods]]**|Array creation; indexing; length; push, pop, shift, unshift; slice, splice; map, filter, reduce, forEach, find, some, every; array spread; array destructuring; when to use each method. Essential data structure.|
|**[[1.1.6 Objects & Object Methods]]**|Object literals; dot vs bracket notation; object methods; this keyword; object destructuring; computed property names; shorthand properties; Object.keys/values/entries; spread in objects. Fundamental data type.|
|**[[1.1.7 Template Literals & Strings]]**|Template literals; string interpolation; multiline strings; tagged templates; string methods (includes, startsWith, slice, split, trim); regular expressions basics; string manipulation. Modern string handling.|
|**[[1.1.8 Control Flow]]**|if/else statements; switch statement; loops (for, while, do-while); for...of loop; for...in loop; break and continue; when to use each loop type; loop performance. Program control.|

### **1.2 Advanced JavaScript Concepts**

Modern JavaScript requires deep understanding of scope, closures, prototypes, and asynchronous programming. Master execution context, the event loop, promises, and async/await. Learn this binding, prototype chain, and how JavaScript actually works under the hood. Understand these concepts are non-negotiable—frameworks depend on them, interviews test them. Write clean asynchronous code, understand memory management, and debug complex issues. This section bridges basic JavaScript to professional development.

|Topic|Focus & Purpose|
|---|---|
|**[[1.2.1 Scope & Closures]]**|Global, function, block scope; lexical scoping; closure definition; practical closures; closure use cases; memory implications; module pattern; encapsulation through closures. Core JavaScript mechanism.|
|**[[1.2.2 The 'this' Keyword]]**|this in different contexts; implicit binding; explicit binding (call, apply, bind); arrow functions and this; this in methods; this gotchas; lexical this; strict mode impact. Understanding context.|
|**[[1.2.3 Prototypes & Inheritance]]**|Prototype chain; **proto** vs prototype; Object.create(); prototypal inheritance; constructor functions; class syntax as syntactic sugar; when prototypes matter; prototype methods. JavaScript inheritance model.|
|**[[1.2.4 Asynchronous JavaScript]]**|Synchronous vs asynchronous; callbacks; callback hell; promises; promise chains; Promise.all/race/allSettled; error handling in promises; async/await; try-catch with async. Essential async patterns.|
|**[[1.2.5 Event Loop & Concurrency]]**|Call stack; callback queue; event loop mechanism; microtasks vs macrotasks; setTimeout/setInterval; requestAnimationFrame; execution order; blocking vs non-blocking code. Understanding JavaScript runtime.|
|**[[1.2.6 Error Handling]]**|try-catch-finally; throw statement; Error objects; custom errors; async error handling; error propagation; error best practices; debugging strategies. Robust error management.|
|**[[1.2.7 Memory Management]]**|Garbage collection; memory leaks; common leak patterns; weak references; WeakMap/WeakSet; performance profiling; memory optimization; cleanup patterns. Resource management.|

### **1.3 DOM Manipulation & Browser APIs**

Master the Document Object Model—the bridge between JavaScript and HTML. Learn element selection, manipulation, event handling, and browser APIs. Understand how JavaScript interacts with web pages, handles user input, and creates dynamic interfaces. Learn modern DOM APIs, event delegation, and performance considerations. This section is essential before learning frameworks—React/Vue abstract the DOM, but you must understand what's underneath. Write performant, accessible DOM code.

|Topic|Focus & Purpose|
|---|---|
|**[[1.3.1 DOM Selection & Traversal]]**|querySelector/querySelectorAll; getElementById/getElementsByClassName; element relationships (parent, children, siblings); NodeList vs HTMLCollection; traversing the DOM; performance considerations. Accessing elements.|
|**[[1.3.2 DOM Manipulation]]**|innerHTML vs textContent; createElement, appendChild; insertBefore, insertAdjacentElement; cloneNode; removeChild; element.remove(); setAttribute/getAttribute; classList (add, remove, toggle). Modifying the page.|
|**[[1.3.3 Event Handling]]**|addEventListener; event types (click, submit, input, keydown); event object; event.preventDefault(); event.stopPropagation(); event delegation; removing listeners; custom events. User interaction.|
|**[[1.3.4 Forms & Input Validation]]**|Form submission; FormData API; input values; validation; client-side validation patterns; preventing default submission; form serialization; file uploads. Form handling.|
|**[[1.3.5 Browser APIs]]**|localStorage/sessionStorage; Fetch API; Geolocation API; Intersection Observer; MutationObserver; History API; Clipboard API; Web Storage best practices. Browser capabilities.|
|**[[1.3.6 Window & Document Objects]]**|window object; document object; navigator; location; screen; scrolling methods; window.open; setTimeout/setInterval; global scope; browser information. Browser environment.|
|**[[1.3.7 Performance & Best Practices]]**|Reflows and repaints; document fragments; debouncing and throttling; efficient event listeners; minimizing DOM access; virtual DOM concept; performance monitoring. Optimized DOM manipulation.|

### **1.4 ES6+ Modern JavaScript Features**

Modern JavaScript is vastly improved with ES6+ features. Master classes, modules, destructuring, async/await, and generators. Learn symbols, proxies, iterators, and advanced array/object methods. Understand these features are standard—production code uses them, frameworks require them. Write concise, readable modern JavaScript using the latest language features. This section covers what makes JavaScript a mature, professional language.

|Topic|Focus & Purpose|
|---|---|
|**[[1.4.1 ES6 Classes]]**|Class declaration; constructor; methods; static methods; inheritance (extends); super keyword; private fields (#); getters and setters; class expressions. OOP in JavaScript.|
|**[[1.4.2 Modules (Import/Export)]]**|ES6 modules; named exports/imports; default exports; import aliases; import *; dynamic imports; module scope; CommonJS vs ES modules; module bundlers. Code organization.|
|**[[1.4.3 Destructuring & Spread]]**|Array destructuring; object destructuring; nested destructuring; default values; rest parameters; spread operator for arrays/objects; function parameter destructuring. Clean syntax.|
|**[[1.4.4 Advanced Array Methods]]**|flatMap; flat; Array.from; Array.of; includes; findIndex; findLast; reduceRight; chaining methods; immutable operations; method composition. Powerful array manipulation.|
|**[[1.4.5 Iterators & Generators]]**|Iterable protocol; iterator protocol; for...of loop; generator functions (function*); yield keyword; generator use cases; async generators; custom iterators. Advanced iteration.|
|**[[1.4.6 Symbols & Well-Known Symbols]]**|Symbol primitive; unique symbols; Symbol.for/keyFor; well-known symbols (iterator, toStringTag); symbol use cases; private-like properties. Unique identifiers.|
|**[[1.4.7 Proxy & Reflect]]**|Proxy object; handler traps; get, set, has traps; validation with proxies; Reflect API; proxy use cases; meta-programming. Advanced object control.|
|**[[1.4.8 Maps, Sets, WeakMap, WeakSet]]**|Map vs objects; Set operations; has, add, delete; iterating collections; WeakMap/WeakSet; when to use each; collection performance. Advanced data structures.|

### **1.5 Asynchronous JavaScript & APIs**

Asynchronous programming is core to modern JavaScript. Master promises, async/await, error handling, and concurrent operations. Learn Fetch API for HTTP requests, handling JSON, CORS, and API integration. Understand Web Workers for multithreading, service workers for offline capabilities, and WebSockets for real-time communication. This section is essential for building modern web applications that communicate with servers and handle concurrent operations.

|Topic|Focus & Purpose|
|---|---|
|**[[1.5.1 Promises Deep Dive]]**|Promise states; creating promises; promise chaining; promise composition; Promise.all; Promise.race; Promise.allSettled; Promise.any; promise error handling; promise patterns. Async foundation.|
|**[[1.5.2 Async/Await Mastery]]**|async functions; await keyword; error handling with try-catch; async function return values; top-level await; parallel vs sequential async; async best practices. Modern async syntax.|
|**[[1.5.3 Fetch API & HTTP]]**|fetch() function; request options; response object; handling JSON; HTTP methods (GET, POST, PUT, DELETE); headers; status codes; error handling; request cancellation. API communication.|
|**[[1.5.4 Working with APIs]]**|REST API concepts; API authentication (API keys, tokens); query parameters; request/response bodies; CORS; rate limiting; API error handling; JSON parsing. Real-world integration.|
|**[[1.5.5 WebSockets & Real-time]]**|WebSocket API; connection establishment; sending/receiving messages; event handlers; connection states; when to use WebSockets; Socket.io basics; real-time patterns. Bidirectional communication.|
|**[[1.5.6 Web Workers]]**|Worker creation; posting messages; worker scope; when workers help; shared workers; terminating workers; worker limitations; offloading computation. Background processing.|
|**[[1.5.7 Service Workers & PWA]]**|Service worker lifecycle; caching strategies; offline functionality; push notifications; background sync; PWA fundamentals; manifest.json; installable web apps. Advanced capabilities.|

### **1.6 JavaScript Testing & Quality**

Professional JavaScript requires testing, debugging, and quality assurance. Master Jest/Vitest for unit testing, testing strategies, mocking, and TDD workflow. Learn debugging techniques, browser DevTools, performance profiling, and memory leak detection. Understand ESLint for code quality, Prettier for formatting, and TypeScript for type safety. This section separates hobbyists from professionals—production code is tested, debugged, and maintained to standards.

|Topic|Focus & Purpose|
|---|---|
|**[[1.6.1 Unit Testing with Jest]]**|Test structure; describe and it blocks; assertions (expect); matchers; beforeEach/afterEach; test coverage; mocking functions; testing async code; TDD workflow. Ensuring correctness.|
|**[[1.6.2 Testing Strategies]]**|Unit vs integration vs E2E tests; test organization; what to test; testing best practices; test doubles (mocks, stubs, spies); testing edge cases; test-driven development. Quality assurance.|
|**[[1.6.3 Debugging Techniques]]**|console methods; debugger statement; browser DevTools; breakpoints; step debugging; call stack inspection; watching variables; debugging async code. Development efficiency.|
|**[[1.6.4 Performance Profiling]]**|Performance tab in DevTools; measuring execution time; memory profiling; identifying bottlenecks; performance.now(); console.time(); lighthouse audits; optimization strategies. Speed optimization.|
|**[[1.6.5 Code Quality Tools]]**|ESLint configuration; linting rules; Prettier formatting; Husky for git hooks; pre-commit checks; automated formatting; code review automation. Maintaining standards.|
|**[[1.6.6 TypeScript Fundamentals]]**|Type annotations; interfaces; type inference; generics; union types; type guards; tsconfig.json; when TypeScript helps; gradual adoption. Type safety.|
|**[[1.6.7 Documentation & JSDoc]]**|JSDoc comments; documenting functions; type definitions in JSDoc; generating documentation; code documentation best practices; self-documenting code. Code maintainability.|

### **1.7 JavaScript Design Patterns & Architecture**

Master design patterns essential for scalable JavaScript applications. Learn module patterns, observer pattern, factory pattern, and singleton. Understand functional programming concepts, composition, immutability, and pure functions. Learn state management patterns, pub-sub systems, and architectural approaches. This section covers professional software engineering in JavaScript—patterns used in frameworks, libraries, and production applications. Essential for senior-level development.

|Topic|Focus & Purpose|
|---|---|
|**[[1.7.1 Module Pattern & Namespacing]]**|IIFE modules; revealing module pattern; namespace pattern; avoiding global scope pollution; encapsulation; module organization; private members. Code organization.|
|**[[1.7.2 Common Design Patterns]]**|Singleton; Factory; Observer; Pub-Sub; Decorator; Strategy; Facade; Module; Command; when to use each pattern. Reusable solutions.|
|**[[1.7.3 Functional Programming Concepts]]**|Pure functions; immutability; higher-order functions; function composition; currying; partial application; point-free style; avoiding side effects. Functional approach.|
|**[[1.7.4 Immutability & State]]**|Immutable data structures; Object.freeze; spread for copying; immutability benefits; libraries (Immer); avoiding mutations; immutable updates. Safe state management.|
|**[[1.7.5 Composition vs Inheritance]]**|Object composition; mixins; composition patterns; favor composition; when inheritance is appropriate; flexible code structures. Code reuse strategies.|
|**[[1.7.6 Event-Driven Architecture]]**|Event emitters; custom events; decoupling with events; pub-sub implementation; event buses; observer pattern; reactive programming basics. Loosely coupled systems.|
|**[[1.7.7 State Management Patterns]]**|Centralized state; state machines; reducer pattern; flux architecture; state immutability; state updates; managing complexity. Application state.|

### **1.8 Build Tools & Modern Workflow**

Modern JavaScript development requires tooling and build processes. Master bundlers (Webpack, Vite), transpilers (Babel), and task automation. Learn npm/yarn, package.json configuration, and dependency management. Understand module bundling, tree shaking, code splitting, and optimization. Learn Git workflows, CI/CD basics, and deployment strategies. This section covers the complete development workflow—from code to production. Essential for professional environments.

|Topic|Focus & Purpose|
|---|---|
|**[[1.8.1 npm & Package Management]]**|package.json; npm install; dependencies vs devDependencies; semantic versioning; package.lock; npm scripts; yarn/pnpm alternatives; managing packages. Dependency management.|
|**[[1.8.2 Module Bundlers]]**|Webpack basics; Vite; entry points; output; loaders; plugins; dev server; hot module replacement; build optimization; when bundlers are needed. Build process.|
|**[[1.8.3 Babel & Transpilation]]**|Babel configuration; presets; plugins; transpiling ES6+; polyfills; browser compatibility; target environments; source maps. Cross-browser JavaScript.|
|**[[1.8.4 Code Splitting & Optimization]]**|Dynamic imports; lazy loading; chunk splitting; tree shaking; minification; dead code elimination; bundle analysis; performance optimization. Load optimization.|
|**[[1.8.5 Development Environment]]**|Local development servers; hot reloading; environment variables; .env files; development vs production builds; debugging in development. Workflow efficiency.|
|**[[1.8.6 Version Control Workflow]]**|Git for JavaScript projects; .gitignore; branching strategies; pull requests; code review; semantic commits; Git hooks; collaboration. Team development.|
|**[[1.8.7 Deployment & CI/CD]]**|Build process; deployment strategies; environment configuration; continuous integration basics; automated testing; deployment platforms (Netlify, Vercel); production optimization. Going live.|

### **1.9 Browser Compatibility & Polyfills**

Professional JavaScript works across all browsers and devices. Master feature detection, progressive enhancement, and graceful degradation. Learn polyfills for newer features, transpilation strategies, and browser testing. Understand vendor prefixes, browser quirks, and cross-browser issues. This section ensures your JavaScript works everywhere—essential for production applications reaching diverse users.

|Topic|Focus & Purpose|
|---|---|
|**[[1.9.1 Feature Detection]]**|Detecting browser features; modernizr; feature vs browser detection; graceful degradation; progressive enhancement; fallback strategies. Cross-browser compatibility.|
|**[[1.9.2 Polyfills & Shims]]**|What polyfills are; core-js; polyfill.io; when polyfills are needed; loading polyfills conditionally; custom polyfills. Backward compatibility.|
|**[[1.9.3 Browser Testing]]**|Testing in multiple browsers; BrowserStack; device testing; responsive testing; debugging tools per browser; browser dev tools. Quality assurance.|
|**[[1.9.4 Mobile Considerations]]**|Touch events; mobile performance; viewport considerations; mobile-specific APIs; testing on mobile; responsive JavaScript; mobile optimization. Mobile-first development.|