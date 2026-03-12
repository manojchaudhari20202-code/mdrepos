## JavaScript Basics
- What is JavaScript
- History and evolution of JavaScript
- ECMAScript standard
- JavaScript engines (V8, SpiderMonkey, JavaScriptCore)
- Interpreted vs compiled languages
- Just-In-Time (JIT) compilation
- Dynamic typing
- Prototype-based language
- Event-driven programming
- Single-threaded execution model

## JavaScript Runtime
- JavaScript runtime environment
- Browser runtime
- Node.js runtime
- Execution context
- Global execution context
- Function execution context
- Call stack
- Memory heap
- Event loop
- Task queue
- Microtask queue

## Variables and Scope
- var
- let
- const
- Block scope
- Function scope
- Global scope
- Temporal Dead Zone
- Hoisting
- Variable shadowing

## Data Types
- Primitive types
- number
- string
- boolean
- null
- undefined
- symbol
- bigint
- Reference types
- Objects
- Arrays
- Functions
- Type coercion
- Type conversion
- Truthy and falsy values

## Operators
- Arithmetic operators
- Comparison operators
- Logical operators
- Bitwise operators
- Assignment operators
- Nullish coalescing
- Optional chaining
- Ternary operator
- Spread operator
- Rest operator

## Functions
- Function declarations
- Function expressions
- Arrow functions
- Anonymous functions
- Higher-order functions
- Callback functions
- Pure functions
- Closures
- Immediately Invoked Function Expressions (IIFE)
- Currying
- Partial application

## Objects
- Object creation
- Object literals
- Object constructors
- Object methods
- Property descriptors
- Getters and setters
- Object immutability
- Object.freeze
- Object.seal
- Object.assign
- Object destructuring

## Arrays
- Array creation
- Array methods
- Iteration methods
- map
- filter
- reduce
- find
- some
- every
- flat
- flatMap
- Array destructuring

## Prototypes and Inheritance
- Prototype chain
- Prototype-based inheritance
- Object prototype
- Constructor functions
- Class syntax
- ES6 classes
- extends keyword
- super keyword
- Method overriding

## Modules
- ES Modules
- import/export syntax
- Default exports
- Named exports
- Dynamic imports
- CommonJS modules
- AMD modules
- Module bundling

## Asynchronous Programming
- Asynchronous programming model
- Callbacks
- Callback hell
- Promises
- Promise chaining
- Promise combinators
- Promise.all
- Promise.race
- Promise.any
- Promise.allSettled
- async/await
- Error handling in async code

## Event Loop Deep Dive
- Event loop architecture
- Macrotasks
- Microtasks
- Task scheduling
- Browser rendering cycle

## Memory Management
- Memory allocation
- Garbage collection
- Mark-and-sweep algorithm
- Memory leaks
- Retained objects
- WeakMap and WeakSet

## Functional Programming Concepts
- First-class functions
- Pure functions
- Immutability
- Function composition
- Lazy evaluation

## Meta Programming
- Reflect API
- Proxy objects
- Dynamic property interception
- Runtime behavior modification

## Iterators and Generators
- Iterator protocol
- Generator functions
- yield keyword
- Async iterators
- Async generators

## Code Quality
- Clean code principles
- Naming conventions
- Code readability
- Code modularity
- Separation of concerns

## Error Handling
- try/catch/finally
- Custom error classes
- Error propagation
- Graceful error handling

## Maintainability
- DRY principle
- KISS principle
- YAGNI principle
- Avoid global variables
- Consistent coding standards

## Asynchronous Best Practices
- Avoid callback hell
- Use async/await properly
- Proper promise chaining
- Error handling in promises

## SOLID Principles
- Single Responsibility Principle
- Open Closed Principle
- Liskov Substitution Principle
- Interface Segregation Principle
- Dependency Inversion Principle

## Software Engineering Principles
- Separation of concerns
- Loose coupling
- High cohesion
- Encapsulation
- Abstraction

## API Design Principles
- Idempotency
- Backward compatibility
- Contract-based APIs
- Versioning strategies

## Creational Patterns
- Factory Pattern
- Abstract Factory
- Builder Pattern
- Singleton Pattern
- Prototype Pattern

## Structural Patterns
- Adapter Pattern
- Bridge Pattern
- Composite Pattern
- Decorator Pattern
- Facade Pattern
- Flyweight Pattern
- Proxy Pattern

## Behavioral Patterns
- Observer Pattern
- Strategy Pattern
- Command Pattern
- Mediator Pattern
- State Pattern
- Chain of Responsibility
- Template Method
- Visitor Pattern
- Iterator Pattern

## JavaScript-Specific Patterns
- Module pattern
- Revealing module pattern
- Event emitter pattern
- Pub/Sub pattern
- Middleware pattern
- Fluent interfaces

## Code Smells
- Spaghetti code
- God objects
- Duplicate code
- Long functions
- Deep nesting

## JavaScript Specific Antipatterns
- Callback hell
- Global namespace pollution
- Overusing closures
- Overusing promises
- Tight coupling

## Architectural Antipatterns
- Big ball of mud
- Distributed monolith
- Cyclic dependencies
- Improper module boundaries

## Logging
- Structured logging
- Log levels
- Correlation IDs
- Context propagation

## Metrics
- Application metrics
- Performance metrics
- Business metrics

## Distributed Tracing
- Trace context
- Span lifecycle
- End-to-end request tracing

## Error Monitoring
- Exception tracking
- Crash reporting
- Error boundaries

## Monitoring Tools
- OpenTelemetry
- Prometheus
- Grafana
- Datadog
- Sentry

## Application Architecture
- Layered architecture
- Clean architecture
- Hexagonal architecture
- Onion architecture

## Frontend Architecture
- Component-based architecture
- State management architecture
- Microfrontend architecture
- SPA architecture

## Backend Architecture
- RESTful services
- GraphQL APIs
- Event-driven architecture
- Serverless architecture

## Distributed Systems
- Service communication
- API gateways
- Service discovery
- Circuit breaker pattern

## Scalability
- Horizontal scaling
- Stateless applications
- Caching strategies
- Load balancing

## Frontend Frameworks
- React
- Angular
- Vue
- Svelte

## Backend Frameworks
- Express.js
- Fastify
- NestJS
- Koa

## Full Stack Frameworks
- Next.js
- Remix
- Nuxt.js

## State Management
- Redux
- MobX
- Zustand
- Recoil

## Build Tools
- Webpack
- Vite
- Rollup
- esbuild
- SWC

## Linting and Formatting
- ESLint
- Prettier
- StandardJS

## Package Management
- npm
- yarn
- pnpm

## Testing Tools
- Jest
- Mocha
- Jasmine
- Vitest
- Cypress
- Playwright

## Debugging Tools
- Chrome DevTools
- Node.js Inspector
- Source maps

## JavaScript Performance
- Execution performance
- JIT optimization
- Inline caching
- Hidden classes

## Frontend Performance
- Lazy loading
- Code splitting
- Tree shaking
- Bundle optimization
- Critical rendering path

## Backend Performance
- Event loop optimization
- Non-blocking I/O
- Worker threads
- Clustering

## Performance Monitoring
- Performance API
- Profiling tools
- Memory profiling
- CPU profiling

## Web Security
- Cross-Site Scripting (XSS)
- Cross-Site Request Forgery (CSRF)
- Content Security Policy

## Authentication and Authorization
- OAuth
- JWT
- Session management

## Secure Coding
- Input validation
- Output encoding
- Secure data handling

## Dependency Security
- Dependency vulnerabilities
- Supply chain attacks
- Package auditing
























---
---

# JavaScript Interview Topics — Comprehensive List

---

## 1. Core Fundamentals

### Language Basics
- Data Types: Primitives vs Objects (`string`, `number`, `bigint`, `boolean`, `undefined`, `null`, `symbol`)
- Type Coercion & Type Conversion (implicit vs explicit)
- `typeof` vs `instanceof`
- Truthy & Falsy values
- `==` vs `===` (abstract equality vs strict equality)
- Short-circuit evaluation (`&&`, `||`, `??`)
- Nullish Coalescing (`??`) & Optional Chaining (`?.`)

### Variables & Scope
- `var` / `let` / `const` — differences, scope, hoisting
- Hoisting: variable hoisting vs function declaration hoisting
- Temporal Dead Zone (TDZ)
- Scope: global, function, block scope
- Lexical scope & scope chain

### Functions
- Function declarations vs function expressions
- Arrow functions vs regular functions
- Immediately Invoked Function Expressions (IIFE)
- First-class functions & higher-order functions
- Pure functions & side effects
- Default parameters, rest parameters (`...args`)
- Closures: definition, use cases, closure over loops

### Objects & Prototypes
- Object creation: literals, `Object.create()`, constructor functions, classes
- Prototype chain & prototypal inheritance
- `Object.freeze()` vs `Object.seal()` vs `const`
- Property descriptors: `writable`, `enumerable`, `configurable`
- Getters & setters
- Computed property names
- Spread operator & destructuring (object and array)
- `Object.assign()`, `Object.keys()`, `Object.values()`, `Object.entries()`, `Object.fromEntries()`

### The `this` Keyword
- `this` binding rules: default, implicit, explicit, `new`, arrow
- `call()`, `apply()`, `bind()`
- `this` in arrow functions vs regular functions
- `this` in class methods

### Classes & Inheritance
- ES6 `class` syntax: `constructor`, methods, static methods
- `extends` & `super`
- Private class fields (`#field`)
- Class expressions vs class declarations

### Arrays
- `map`, `filter`, `reduce`, `flat`, `flatMap`
- `find`, `findIndex`, `some`, `every`, `includes`
- `sort`, `slice`, `splice`, `concat`, `join`
- Array destructuring & spread
- `Array.from()`, `Array.isArray()`

### Strings
- Template literals & tagged templates
- `slice`, `substring`, `split`, `trim`, `padStart`, `padEnd`, `replaceAll`
- String destructuring

### Iteration & Loops
- `for...in` vs `for...of` vs `forEach`
- Iterators & the iterator protocol (`Symbol.iterator`)
- Generators (`function*`, `yield`)

### Advanced Built-ins
- `Map`, `Set`, `WeakMap`, `WeakSet`, `WeakRef`
- `Symbol` & its use cases
- `Proxy` & `Reflect` API
- `FinalizationRegistry`
- `structuredClone()` for deep copying

### Execution Model
- Execution context & call stack
- The event loop: call stack, task queue (macrotasks), microtask queue
- `setTimeout`, `setInterval`, `queueMicrotask`
- `requestAnimationFrame`

### Async JavaScript
- Callbacks & callback hell
- Promises: states (`pending`, `fulfilled`, `rejected`)
- Promise chaining (`.then`, `.catch`, `.finally`)
- `Promise.all`, `Promise.allSettled`, `Promise.any`, `Promise.race`
- `async` / `await` syntax
- Error handling with `try/catch` in async functions
- Microtasks vs Macrotasks execution order
- Concurrency vs parallelism in JS
- `AbortController` & `AbortSignal`
- Web Workers & `SharedArrayBuffer`

### Modules
- CommonJS (`require` / `module.exports`) vs ES Modules (`import` / `export`)
- Named exports vs default exports
- Dynamic `import()`
- Tree-shaking & module bundling concepts

### Memory & Performance
- Garbage collection (mark-and-sweep)
- Memory leaks: forgotten event listeners, detached DOM nodes, closed-over refs
- Debouncing & throttling
- Memoization
- Lazy loading & code splitting

### Miscellaneous
- `eval()` — behavior and risks
- `strict mode` (`'use strict'`) — effects and rationale
- Error types: `TypeError`, `ReferenceError`, `SyntaxError`, `RangeError`
- Custom error classes (`class CustomError extends Error`)
- `JSON.stringify()` & `JSON.parse()` — options and gotchas
- Regular expressions in JavaScript

---

## 2. Best Practices

### Code Style & Readability
- Prefer `const` over `let`; avoid `var`
- Meaningful, descriptive variable and function naming
- Guard clauses / early returns over deeply nested conditions
- Avoid magic numbers and strings — use named constants
- Keep functions small and focused (single purpose)
- Consistent code formatting (Prettier, ESLint)
- Document complex logic with clear, purposeful comments

### Safety & Correctness
- Always use strict equality (`===`) over loose equality (`==`)
- Validate and sanitize inputs (defensive programming)
- Always handle promise rejections (`.catch()` or `try/catch`)
- Avoid floating promises (unhandled async calls)
- Never mutate function arguments
- Prefer immutable patterns — avoid in-place mutations
- Use `structuredClone()` or spread for safe object copying

### Functions & Modules
- Keep functions pure where possible
- Avoid excessive function arguments — use an options object if >3 params
- Prefer composition over deep inheritance
- Use ES Modules (`import`/`export`) for code organization
- Tree-shaking friendly exports — avoid exporting unnecessarily

### Performance
- Memoize expensive computations
- Debounce/throttle event handlers (scroll, resize, input)
- Lazy-load modules and assets
- Avoid synchronous blocking in async contexts
- Avoid DOM thrashing (batch reads and writes)
- Use `requestAnimationFrame` for visual updates

### Security
- Sanitize user input before rendering to the DOM (prevent XSS)
- Avoid `innerHTML` with untrusted data — prefer `textContent`
- Never use `eval()` on dynamic input
- Use `Content-Security-Policy` headers

### Tooling & Workflow
- Enforce linting rules: `no-unused-vars`, `no-console` in production
- Use source maps in production builds for debugging
- Automate testing (unit, integration, E2E)
- Code review and pair programming for critical paths

---

## 3. Design Principles

### SOLID Principles (applied to JavaScript)
- **S** — Single Responsibility Principle (SRP): a module/function/class does one thing
- **O** — Open/Closed Principle (OCP): open for extension, closed for modification
- **L** — Liskov Substitution Principle (LSP): subtypes must be substitutable for base types
- **I** — Interface Segregation Principle (ISP): many specific interfaces over one general
- **D** — Dependency Inversion Principle (DIP): depend on abstractions, not concretions

### General Principles
- **DRY** — Don't Repeat Yourself
- **KISS** — Keep It Simple, Stupid
- **YAGNI** — You Aren't Gonna Need It
- **Separation of Concerns** — distinct layers for distinct responsibilities
- **Composition over Inheritance** — prefer object composition for code reuse
- **Law of Demeter** — a unit should only talk to its immediate dependencies
- **Principle of Least Privilege** — expose only what is necessary
- **Fail Fast** — detect and report errors as early as possible
- **Immutability as a design constraint** — reduce shared mutable state
- **Inversion of Control (IoC)** — frameworks call your code, not the other way
- **Dependency Injection** — supply dependencies from outside, not created inside
- **High Cohesion, Low Coupling** — related code together, unrelated code apart
- **Event-driven architecture** — decouple producers from consumers via events
- **Referential Transparency** — same input always produces same output

### Functional Programming Principles
- Pure functions
- Avoiding shared state
- Avoiding mutable data
- Composing functions
- Declarative over imperative style

---

## 4. Design Patterns

### Creational Patterns
- **Factory Function** — functions that return objects without `new`
- **Constructor Pattern** — using `new` with constructor functions or classes
- **Singleton Pattern** — ensure a class has only one instance
- **Builder Pattern** — step-by-step object construction
- **Prototype Pattern** — cloning objects from a prototype

### Structural Patterns
- **Module Pattern** (IIFE-based) — encapsulate private state
- **Revealing Module Pattern** — explicitly reveal public API
- **ES Modules** (`import`/`export`) — native module encapsulation
- **Decorator Pattern** — dynamically add behavior to objects/functions
- **Facade Pattern** — simplified interface over a complex subsystem
- **Adapter Pattern** — make incompatible interfaces work together
- **Proxy Pattern** — intercept and control access to objects
- **Mixin Pattern** — add reusable behavior to classes without inheritance

### Behavioral Patterns
- **Observer / PubSub Pattern** — subscribe and emit events
- **Strategy Pattern** — interchangeable algorithms behind a common interface
- **Command Pattern** — encapsulate actions as objects
- **Iterator Pattern** — sequential access without exposing internals
- **Mediator Pattern** — centralized communication between components
- **Chain of Responsibility** — pass requests along a handler chain
- **State Pattern** — alter behavior based on internal state
- **Template Method Pattern** — define skeleton of an algorithm, defer steps

### Functional Patterns
- **Currying** — transform `f(a, b)` into `f(a)(b)`
- **Partial Application** — pre-fill some arguments of a function
- **Function Composition** — `compose(f, g)(x)` = `f(g(x))`
- **Pipe** — left-to-right function composition
- **Maybe / Option monad-like pattern** — handle nullable values safely
- **Result / Either pattern** — represent success or failure explicitly
- **Functor pattern** — mappable containers

### Async Patterns
- **Promise Retry pattern** — retry failed async operations with backoff
- **Circuit Breaker pattern** — stop calling failing services temporarily
- **Queue / Rate Limiter pattern** — control concurrency of async operations
- **Saga pattern** — manage side effects in complex async flows

### Reactive Patterns
- **Observable pattern** (RxJS) — streams of events over time
- **Subject pattern** — multicast observable source

---

## 5. Antipatterns

### Code Quality Antipatterns
- **Callback Hell** (Pyramid of Doom) — deeply nested callbacks
- **God Object / God Function** — one class/function doing too much
- **Spaghetti Code** — tangled, unstructured logic with no clear flow
- **Copy-Paste Programming** — duplicating code instead of abstracting
- **Magic Numbers & Magic Strings** — unnamed literal values in logic
- **Misleading Naming** — vague, single-letter, or deceptive identifiers
- **Dead Code** — unreachable or unused code left in the codebase
- **Overengineering / Unnecessary Abstraction** — complexity without benefit
- **Premature Optimization** — optimizing before profiling proves it's needed
- **Boolean Trap** — functions accepting bare boolean flags as arguments

### Structural Antipatterns
- **Global Scope Pollution** — variables/functions leaking into global namespace
- **Tight Coupling** — modules that are too interdependent to change independently
- **Deep Inheritance Chains** — excessive subclassing causing fragility
- **Excessive Function Arguments** — functions with too many positional parameters
- **Deep Nesting of Conditionals** — multiple levels of if/else without guard clauses
- **Reinventing the Wheel** — reimplementing what native APIs or libraries already provide

### JavaScript-Specific Antipatterns
- **Using `var`** — hoisting pitfalls, lack of block scope
- **Implicit Type Coercion Bugs** — relying on `==` for comparisons
- **Mutating Function Arguments** — modifying objects passed as parameters
- **Prototype Pollution** — overwriting properties on `Object.prototype`
- **Using `eval()`** — security risk, performance penalty, hard to debug
- **`arguments` object in arrow functions** — doesn't exist; use rest params
- **Using `delete` on object properties in hot paths** — engine deoptimization
- **Synchronous Blocking Operations in Async Contexts** — blocking the event loop
- **Using Synchronous XHR** — freezes the browser UI
- **Not Cleaning Up Subscriptions/Timers in SPAs** — memory and side-effect leaks

### Async Antipatterns
- **Floating Promises** — fire-and-forget with no error handling
- **Unhandled Promise Rejections** — silent failures in async code
- **Sequential awaits when parallel is possible** — unnecessary performance loss
- **Mixing callbacks and Promises** — inconsistent error handling models
- **Nested `.then()` chains** — defeats the purpose of Promise chaining

### Memory & Performance Antipatterns
- **Memory Leaks: forgotten event listeners** — listeners keeping references alive
- **Memory Leaks: detached DOM nodes** — removed elements still referenced in JS
- **Memory Leaks: closures holding large references** — unintentionally retained data
- **DOM Thrashing** — interleaving reads and writes causing layout reflows
- **Using `innerHTML` with unsanitized data** — XSS vulnerability
- **Over-fetching / Under-fetching data** — poor API design causing waste

---

## 6. Observability

### The Three Pillars
- **Logs** — discrete timestamped records of events
- **Metrics** — numeric measurements over time (counters, gauges, histograms)
- **Traces** — end-to-end request flow across services

### Logging
- Structured logging (JSON logs vs unstructured plain text)
- Log levels: `DEBUG`, `INFO`, `WARN`, `ERROR`, `FATAL`
- `console` API: `log`, `warn`, `error`, `group`, `groupEnd`, `time`, `timeEnd`, `table`, `trace`, `count`
- Contextual logging: request ID, user ID, session ID in log fields
- Correlation IDs & distributed trace context propagation
- Avoiding logging sensitive data (PII, credentials)
- Log rotation & retention policies
- Centralized log aggregation (ELK Stack, Loki, Datadog Logs)

### Error Tracking & Handling
- `Error` object anatomy: `message`, `stack`, `name`
- Custom Error subclasses (`class AppError extends Error`)
- Global error handlers: `window.onerror`, `window.addEventListener('unhandledrejection')`
- `process.on('uncaughtException')` and `process.on('unhandledRejection')` in Node.js
- Source Maps for production debugging (mapping minified code to source)
- Error tracking platforms: Sentry, Bugsnag, Rollbar — SDK integration patterns
- Error boundaries (React) for UI error isolation

### Metrics
- Metric types: counters, gauges, histograms, summaries
- Web Vitals: LCP, INP (replacing FID), CLS, TTFB, FCP
- Real User Monitoring (RUM) — measuring actual user experience
- Synthetic Monitoring — scripted tests simulating user journeys
- Custom performance marks: `performance.mark()`, `performance.measure()`
- `performance.now()` for high-resolution timing
- Navigation Timing API & Resource Timing API
- Node.js runtime metrics: `process.memoryUsage()`, `process.cpuUsage()`
- Event Loop Lag monitoring (libuv queue depth)

### Distributed Tracing
- OpenTelemetry SDK in JavaScript (browser and Node.js)
- Trace context propagation (W3C TraceContext standard)
- Spans, trace IDs, span IDs, parent-child relationships
- Instrumentation: auto vs manual
- Exporting to backends: Jaeger, Zipkin, Datadog, Honeycomb

### APM & Tooling
- APM tools: Datadog, New Relic, Dynatrace — JS agent setup
- Browser DevTools: Performance panel, Memory panel, Network panel
- Heap snapshots & memory profiling
- CPU flame graphs for Node.js (clinic.js, 0x)
- Lighthouse for web performance audits
- Chrome User Experience Report (CrUX)

### Alerting & SLOs
- SLI (Service Level Indicator) — what you measure
- SLO (Service Level Objective) — target threshold
- SLA (Service Level Agreement) — contractual commitment
- Alerting strategies: static thresholds, anomaly detection, error rate spikes
- On-call runbooks linked to alerts

### Operational Practices
- Health check endpoints (`/health`, `/ready`, `/live`) in Node.js services
- Feature flags & gradual rollouts for safe observability of new code
- Canary deployments — monitoring new versions before full rollout
- A/B testing observability — tracking metrics per variant
- Telemetry pipelines: collection → transport → storage → visualization
- Data sampling strategies for high-volume traces/logs

---

