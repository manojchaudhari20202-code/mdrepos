
## TypeScript Basics
- What is TypeScript
- TypeScript vs JavaScript
- Why TypeScript was created
- Superset of JavaScript concept
- Static typing vs dynamic typing
- Compile-time vs runtime
- TypeScript compiler (tsc)
- TypeScript transpilation
- TypeScript language services
- TypeScript configuration (`tsconfig.json`)

## Type System
- Type annotations
- Type inference
- Structural typing
- Nominal vs structural typing
- Duck typing
- Strict typing modes

## Primitive Types
- number
- string
- boolean
- null
- undefined
- symbol
- bigint
- void
- never
- unknown
- any

## Advanced Types
- Union types
- Intersection types
- Literal types
- Template literal types
- Type narrowing
- Type guards
- Discriminated unions
- Exhaustiveness checking

## Type Inference
- Contextual typing
- Best common type
- Type widening
- Type narrowing
- Control flow analysis

## Functions
- Function type annotations
- Optional parameters
- Default parameters
- Rest parameters
- Function overloading
- Call signatures
- Construct signatures
- Arrow functions
- Higher-order functions

## Objects
- Object type definitions
- Readonly properties
- Optional properties
- Index signatures
- Excess property checks
- Object destructuring types

## Arrays and Tuples
- Array types
- Readonly arrays
- Tuple types
- Variadic tuples
- Tuple labels
- Tuple inference

## Enums
- Numeric enums
- String enums
- Const enums
- Reverse mappings
- Enum alternatives

## Interfaces
- Interface definition
- Optional properties
- Readonly properties
- Interface extension
- Interface merging
- Callable interfaces
- Hybrid interfaces

## Type Aliases
- Alias declaration
- Union aliases
- Intersection aliases
- Recursive types
- Difference between interface and type

## Classes
- Class syntax
- Access modifiers
- public / private / protected
- readonly members
- parameter properties
- abstract classes
- static members
- getters and setters

## Generics
- Generic functions
- Generic classes
- Generic interfaces
- Generic constraints
- Default generic types
- Conditional generics

## Modules
- ES modules
- Import syntax
- Export syntax
- Default exports
- Namespace imports
- Re-exporting

## Namespaces
- Internal modules
- Namespace syntax
- Namespace merging
- Namespace vs modules

## Utility Types
- Partial
- Required
- Readonly
- Pick
- Omit
- Record
- Exclude
- Extract
- NonNullable
- Parameters
- ReturnType
- ConstructorParameters
- InstanceType

## Conditional Types
- Conditional type syntax
- Distributive conditional types
- Type inference in conditional types
- Recursive conditional types

## Mapped Types
- Mapping types
- Key remapping
- Modifier mapping
- Readonly mapping
- Optional mapping

## Template Literal Types
- String literal combinations
- Type-safe string manipulation
- Event name patterns
- API endpoint typing

## Type Guards
- typeof guards
- instanceof guards
- in operator
- Custom type guards
- Assertion functions

## Assertion Techniques
- Type assertions
- Non-null assertions
- Const assertions
- Assertion signatures

## Decorators
- Class decorators
- Method decorators
- Property decorators
- Parameter decorators
- Decorator factories
- Metadata reflection

## Declaration Files
- `.d.ts` files
- Ambient declarations
- Module augmentation
- Global declarations
- Library definition files

## Compiler Options
- strict mode
- noImplicitAny
- strictNullChecks
- target
- module
- moduleResolution
- paths and baseUrl
- sourceMap
- declaration generation

## Type Safety
- Avoid `any`
- Prefer `unknown`
- Strict mode usage
- Exhaustive type checking
- Narrow types early

## Code Organization
- Modular architecture
- Feature-based folder structure
- Domain-driven typing
- Barrel files

## Type Design
- Prefer interfaces for public APIs
- Use type aliases for unions
- Use discriminated unions
- Avoid overusing enums
- Use literal types

## Error Handling
- Typed errors
- Result pattern
- Error boundary types

## API Design
- Generic APIs
- Immutable data types
- Readonly structures
- Clear function contracts

## Maintainability
- Avoid deep type complexity
- Keep types discoverable
- Self-documenting types
- Consistent naming conventions

## SOLID Principles
- Single Responsibility Principle
- Open Closed Principle
- Liskov Substitution Principle
- Interface Segregation Principle
- Dependency Inversion Principle

## Type Design Principles
- Type safety first
- Minimize runtime checks
- Prefer compile-time guarantees
- Explicit over implicit typing
- Immutability principle

## API Design Principles
- Least surprise principle
- Self-descriptive APIs
- Stable contracts
- Backward compatibility

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
- Chain of Responsibility
- Mediator Pattern
- State Pattern
- Visitor Pattern
- Iterator Pattern
- Template Method Pattern

## Functional Patterns
- Higher Order Functions
- Function composition
- Currying
- Memoization
- Immutability pattern

## TypeScript Specific Patterns
- Type-safe builder
- Type-safe event emitter
- Fluent interfaces
- Branded types
- Discriminated union pattern

## TypeScript Misuse
- Overusing `any`
- Overusing type assertions
- Ignoring strict mode
- Unsafe type casting
- Type explosion

## Code Smells
- God types
- Deeply nested generics
- Over-engineered type hierarchies
- Circular dependencies
- Massive interfaces

## Design Issues
- Tight coupling
- Leaky abstractions
- Poor module boundaries
- Runtime type validation reliance

## Maintainability Issues
- Duplicate types
- Hidden implicit types
- Inconsistent naming
- Unclear domain models

## Logging
- Structured logging
- Log levels
- Correlation IDs
- Contextual logging

## Metrics
- Application metrics
- Performance metrics
- Business metrics

## Tracing
- Distributed tracing
- Request tracing
- Dependency tracing

## Error Monitoring
- Exception tracking
- Error boundaries
- Error reporting systems

## Monitoring Tools
- OpenTelemetry
- Prometheus
- Grafana
- Sentry
- Datadog

## Frontend Architecture
- Component architecture
- State management architecture
- Modular frontend architecture
- Microfrontend architecture

## Backend Architecture
- REST APIs
- GraphQL APIs
- Event-driven architecture
- Serverless architecture

## Application Architecture
- Layered architecture
- Hexagonal architecture
- Clean architecture
- Onion architecture

## Distributed Systems
- Service communication
- API gateway pattern
- Service discovery
- Circuit breaker

## Scalability
- Horizontal scaling
- Stateless services
- Caching strategies
- Load balancing

## Frontend Frameworks
- React with TypeScript
- Angular
- Vue with TypeScript
- Svelte

## Backend Frameworks
- Node.js with TypeScript
- NestJS
- Express with TypeScript
- Fastify

## Full Stack Frameworks
- Next.js
- Remix
- Nuxt

## State Management
- Redux Toolkit
- Zustand
- MobX
- Recoil

## Testing Frameworks
- Jest
- Vitest
- Mocha
- Jasmine

## Build Tools
- Webpack
- Vite
- Rollup
- esbuild
- SWC

## Linting & Formatting
- ESLint
- Prettier
- TypeScript ESLint

## Package Management
- npm
- yarn
- pnpm

## Runtime Platforms
- Node.js
- Deno
- Bun

## Performance Engineering
- Compilation performance
- Incremental builds
- Project references
- Lazy loading
- Tree shaking
- Bundle optimization
- Code splitting
- Dead code elimination

## Security
- Type-safe input validation
- Schema validation
- Preventing injection attacks
- Secure API contracts
- Runtime validation libraries
- Data sanitization

## Tooling
- TypeScript Language Server
- IDE integrations
- Debugging TypeScript
- Source maps
- Code generation
- Documentation generation

## Interoperability
- JavaScript interoperability
- Third-party libraries typing
- DefinitelyTyped
- Legacy JavaScript migration
- Mixed JS/TS codebases


---

# TypeScript Interview Topics — Comprehensive List

---

## 1. Core Fundamentals

### Type System Basics
- Static typing vs dynamic typing — TypeScript's role
- Type inference — when TypeScript infers vs when you annotate
- Primitive types: `string`, `number`, `boolean`, `bigint`, `symbol`, `null`, `undefined`
- `any` vs `unknown` vs `never` vs `void`
- `object`, `Object`, `{}` — differences and gotchas
- Type annotations vs type assertions (`as`, `<Type>`)
- `satisfies` operator — validate without widening
- `const` assertions (`as const`) — literal type narrowing
- Non-null assertion operator (`!`) — use and abuse

### Union & Intersection Types
- Union types (`A | B`) — representing "one of"
- Intersection types (`A & B`) — combining types
- Discriminated unions (tagged unions) — pattern and use cases
- Narrowing union types with type guards
- `never` in exhaustiveness checking

### Type Narrowing & Control Flow
- `typeof` type guards
- `instanceof` type guards
- `in` operator narrowing
- Truthiness narrowing
- Equality narrowing (`===`, `!==`)
- User-defined type guards (`is` keyword)
- Assertion functions (`asserts`)
- Control flow analysis by TypeScript compiler

### Interfaces vs Types
- `interface` vs `type` alias — key differences
- Interface declaration merging
- Extending interfaces (`extends`) vs intersecting types (`&`)
- Implementing interfaces in classes (`implements`)
- When to prefer `interface` over `type` and vice versa
- Readonly properties (`readonly`) in interfaces and types

### Generics
- Generic functions, interfaces, classes
- Type parameters and constraints (`extends`)
- Default type parameters
- Generic constraints with `keyof` and `typeof`
- Multiple type parameters
- Generic utility patterns and reusable abstractions
- Variance in generics: covariance, contravariance, invariance

### Utility Types
- `Partial<T>`, `Required<T>`, `Readonly<T>`
- `Pick<T, K>`, `Omit<T, K>`
- `Record<K, V>`
- `Exclude<T, U>`, `Extract<T, U>`
- `NonNullable<T>`
- `ReturnType<T>`, `Parameters<T>`, `ConstructorParameters<T>`
- `InstanceType<T>`
- `Awaited<T>`
- `ThisType<T>`

### Template Literal Types
- Basic template literal types
- Combining with unions for combinatorial types
- `Uppercase`, `Lowercase`, `Capitalize`, `Uncapitalize`
- Inferring within template literal types
- Real-world use cases: event names, CSS properties, route patterns

### Mapped Types
- Syntax: `{ [K in keyof T]: ... }`
- Modifiers: adding/removing `readonly` and `?`
- Key remapping with `as`
- Filtering keys with `never`
- Homomorphic vs non-homomorphic mapped types

### Conditional Types
- Basic conditional type syntax (`T extends U ? X : Y`)
- `infer` keyword — extracting types within conditionals
- Distributive conditional types
- Recursive conditional types
- Common patterns: `DeepPartial`, `DeepReadonly`, `Flatten`, `UnwrapPromise`

### Indexed Access Types & `keyof`
- `keyof T` — union of keys of a type
- `T[K]` — indexed access type
- `typeof` operator in type position
- `keyof typeof` pattern for enums and object maps

### Enums
- Numeric enums vs string enums
- `const enum` — inlining and limitations
- Heterogeneous enums (avoid)
- Enum reverse mapping (numeric only)
- Alternatives to enums: union of string literals, `as const` objects

### Functions in TypeScript
- Function type signatures
- Optional and default parameters
- Rest parameters with types
- Overloads: declaration signatures vs implementation signature
- `this` parameter in functions
- Callable types and call signatures in interfaces
- `never` return type for functions that throw or loop infinitely

### Classes in TypeScript
- Property declarations and access modifiers: `public`, `private`, `protected`, `readonly`
- TypeScript `private` vs ECMAScript `#private`
- Abstract classes and abstract methods
- Constructors: parameter properties shorthand
- Static members and static blocks
- Class expressions
- Mixins pattern with TypeScript
- Class implementing multiple interfaces

### Modules & Namespaces
- ES Modules in TypeScript (`import`/`export`)
- `import type` vs `import` — type-only imports
- Ambient modules and `.d.ts` declaration files
- Module resolution strategies: `node`, `bundler`, `node16`, `nodenext`
- Path aliases (`paths` in `tsconfig.json`)
- Namespaces (legacy) vs modules
- Barrel files (`index.ts`) — pros and cons
- Triple-slash directives (`/// <reference types="..." />`)

### Declaration Files
- `.d.ts` files — purpose and structure
- Writing declarations for untyped JS libraries
- `declare module`, `declare global`, `declare namespace`
- `@types/*` packages — DefinitelyTyped ecosystem
- Module augmentation

### `tsconfig.json` Deep Dive
- `strict` mode and all sub-flags: `strictNullChecks`, `strictFunctionTypes`, `strictBindCallApply`, `noImplicitAny`, `noImplicitThis`
- `target` vs `lib` — compilation target vs available APIs
- `module` and `moduleResolution`
- `esModuleInterop` and `allowSyntheticDefaultImports`
- `paths` and `baseUrl` for module aliases
- `include`, `exclude`, `files`
- `composite`, `references` — project references for monorepos
- `isolatedModules` — compatibility with bundlers (esbuild, SWC)
- `declaration`, `declarationMap`, `sourceMap`
- `skipLibCheck` — tradeoffs

### Advanced Type Patterns
- Recursive types
- Self-referential types
- Opaque / branded types (`Brand<T, B>`)
- Phantom types
- Builder pattern with types
- Variadic tuple types
- `infer` in complex scenarios
- Higher-kinded types (simulation in TypeScript)
- Type-level programming patterns

---

## 2. Best Practices

### Type Safety
- Enable `strict` mode from the start — never disable it
- Avoid `any` — use `unknown` and narrow explicitly
- Avoid type assertions (`as`) unless absolutely necessary; prefer type guards
- Prefer `unknown` over `any` for external data (API responses, `JSON.parse`)
- Use `satisfies` to validate shape without losing inference
- Avoid non-null assertion (`!`) — handle nullability explicitly
- Use `readonly` and `ReadonlyArray` to enforce immutability
- Avoid `Object`, `Function`, `String`, `Number` (capital) — use lowercase primitives

### Naming & Organization
- Prefix interfaces with `I` only if your team convention requires it (generally avoid)
- Use descriptive, domain-specific type names
- Group related types in dedicated type files (`types/`, `models/`)
- Co-locate types with the code that uses them for small modules
- Use barrel exports (`index.ts`) judiciously — avoid circular dependency traps

### Generics & Reusability
- Constrain generics with `extends` — never leave them unconstrained unnecessarily
- Name type parameters meaningfully (`TEntity`, `TKey`) for complex generics
- Avoid overly complex generic chains — readability over cleverness
- Use utility types before writing custom ones

### Functions & Classes
- Prefer explicit return types on public API functions
- Avoid function overloads when union types or optional params suffice
- Prefer `interface` for public API shapes (open for extension)
- Prefer `type` for internal, complex, or composed types
- Mark class members `readonly` when they should not change after construction
- Use `private` or `#private` instead of naming conventions like `_field`

### Enums & Constants
- Prefer `as const` objects over enums for plain value sets
- If using enums, prefer `const enum` only when bundler supports inlining
- Never use heterogeneous enums
- Use string enums over numeric enums for readability and debuggability

### Configuration
- Always enable `strict: true` in `tsconfig.json`
- Use `noUncheckedIndexedAccess` for safer array and record access
- Enable `exactOptionalPropertyTypes` for stricter optional handling
- Use project references for large monorepos
- Avoid `skipLibCheck: true` in library code (acceptable in apps)

### Code Quality
- Use ESLint with `@typescript-eslint` plugin — enforce TS-specific rules
- Enforce `no-explicit-any`, `no-unsafe-*` rules
- Run `tsc --noEmit` in CI for type checking
- Separate type checking from compilation (use esbuild/SWC to compile, `tsc` to type-check)
- Write types for edge cases (empty arrays, null returns, error states)

---

## 3. Design Principles

### SOLID Principles in TypeScript
- **Single Responsibility Principle** — one class/module for one concern
- **Open/Closed Principle** — extend via interfaces, not by modifying implementations
- **Liskov Substitution Principle** — subtypes must honour base type contracts
- **Interface Segregation Principle** — small, focused interfaces over fat interfaces
- **Dependency Inversion Principle** — depend on interface types, inject concrete implementations

### Type System Design Principles
- **Make illegal states unrepresentable** — encode constraints in the type system
- **Parse, don't validate** — transform untyped input into typed structures at the boundary
- **Prefer types over comments** — types as living documentation
- **Avoid stringly-typed code** — use union literals instead of raw strings
- **Principle of Least Exposure** — `private`/`protected` by default, expose deliberately
- **Type-driven development** — design types before implementation

### General Software Principles
- **Separation of Concerns** — distinct layers (data, logic, presentation)
- **DRY** — Don't Repeat Yourself; leverage generics and utility types to avoid type duplication
- **KISS** — Keep It Simple; resist over-typing for its own sake
- **YAGNI** — don't create generic abstractions until the need is proven
- **Composition over Inheritance** — use interface composition and mixins
- **Fail Fast** — use `never` and assertion functions to catch invalid states early
- **Immutability** — `readonly`, `ReadonlyArray`, `as const` as defaults
- **Dependency Injection** — use constructor injection with interface types for testability
- **Hexagonal Architecture (Ports & Adapters)** — define ports as TypeScript interfaces

### Functional Principles in TypeScript
- Pure functions with explicit typed inputs and outputs
- Immutable data structures (`Readonly<T>`, `ReadonlyArray<T>`)
- Typed functional composition
- Result/Either types for explicit error handling without exceptions
- Avoid side effects in typed core domain logic

---

## 4. Design Patterns

### Creational Patterns
- **Factory Function** — typed factory returning interface types
- **Abstract Factory** — interface-based factories for families of objects
- **Constructor Pattern** — typed classes with constructor injection
- **Singleton** — module-level singleton via ES module caching
- **Builder Pattern** — fluent, chainable typed builders
- **Prototype Pattern** — typed `clone()` methods

### Structural Patterns
- **Adapter Pattern** — wrap third-party types behind internal interface
- **Facade Pattern** — simplified typed interface over complex subsystem
- **Decorator Pattern** — class decorators and function wrappers with types
- **Proxy Pattern** — ES `Proxy` with TypeScript typing
- **Composite Pattern** — recursive typed tree structures
- **Bridge Pattern** — separating abstraction from implementation with interfaces
- **Mixin Pattern** — typed mixin functions and `implements` merging

### Behavioral Patterns
- **Strategy Pattern** — typed strategy interfaces with runtime injection
- **Observer / EventEmitter Pattern** — typed event maps
- **Command Pattern** — typed command objects and invokers
- **Chain of Responsibility** — typed middleware pipelines
- **State Pattern** — discriminated union states with transition functions
- **Template Method Pattern** — abstract base class with typed hooks
- **Mediator Pattern** — typed event bus or mediator class
- **Iterator Pattern** — typed custom iterators and generators
- **Visitor Pattern** — typed visitors with discriminated unions

### TypeScript-Specific Patterns
- **Branded / Opaque Types** — prevent mixing semantically different values of same primitive
- **Type-safe Event Bus** — `Record<EventName, Payload>` mapped event maps
- **Repository Pattern** — typed generic repository interfaces
- **Result / Either Pattern** — `Result<T, E>` for typed error handling without exceptions
- **Option / Maybe Pattern** — `Option<T>` for explicit nullable handling
- **Discriminated Union State Machines** — model state transitions in types
- **Type-safe Builder** — generic builders with `this` return type for chaining
- **Phantom Type Pattern** — encode state in type parameter without runtime cost
- **Fluent Interface** — method chaining with `this` return type
- **Type Predicate Functions** — reusable narrowing with `is` keyword
- **Assertion Functions** — `asserts x is T` for validation at runtime

### Functional Patterns
- **Currying** — typed curried functions
- **Partial Application** — typed partial application utilities
- **Function Composition & Pipe** — typed `compose` / `pipe` utilities
- **Monad-like patterns** — `Result`, `Option` with typed `map`, `flatMap`, `fold`
- **Functor** — typed mappable containers

### Module Patterns
- **Module Pattern** — encapsulation via ES module scope
- **Barrel Exports** — typed re-exports through `index.ts`
- **Plugin Pattern** — typed plugin registration systems

---

## 5. Antipatterns

### Type System Antipatterns
- **Overusing `any`** — silences type checking entirely; use `unknown` instead
- **Type Assertion Abuse (`as`)** — bypasses type safety without verification
- **Non-null Assertion Abuse (`!`)** — hides potential runtime errors
- **`any` Propagation** — one `any` infects surrounding types
- **Stringly-typed Code** — using `string` where a union literal should be used
- **Anemic Type Definitions** — types too broad to be useful (`object`, `{}`, `any[]`)
- **Over-typing / Type Gymnastics** — complex unmaintainable type-level code for marginal gain
- **Ignoring `strict` mode** — disabling flags that catch real bugs
- **Using `Object`, `Function`, `String`** — wrapper object types instead of primitives
- **Widening with unnecessary type assertions** — casting to broader types
- **Duplicate type definitions** — rewriting types that utility types could derive
- **Enum misuse** — numeric enums used as flags; prefer bit flags or string unions

### Structural Antipatterns
- **God Interface** — one interface with too many responsibilities
- **Leaking implementation types** — exposing internal types in public APIs
- **Fat barrel files** — `index.ts` exporting everything causing circular deps
- **Excessive inheritance** — deep class hierarchies instead of composition
- **Implicit `any` in untyped parameters** — relying on inference where it widens to `any`
- **Mixing concerns in types** — combining domain types with UI or persistence types

### Module & Configuration Antipatterns
- **`skipLibCheck: true` in library code** — hides type errors in dependencies
- **Disabling `strictNullChecks`** — largest source of runtime `undefined` errors
- **Path alias inconsistency** — different alias configs in `tsconfig` vs bundler
- **Circular module dependencies** — causes runtime and type-resolution failures
- **Re-exporting everything from barrel** — defeats tree-shaking

### Runtime vs Compile-Time Antipatterns
- **Trusting `as` for runtime safety** — type assertions don't exist at runtime
- **Not validating external data** — assuming API responses match declared types
- **Forgetting `isolatedModules` constraints** — type-only exports failing with esbuild/SWC
- **Using `const enum` with bundlers that don't inline** — runtime `undefined` errors
- **Relying on TypeScript for runtime type checking** — TS types are erased at compile time

### General Code Antipatterns (compounded by TypeScript)
- **Callback Hell** — still possible; use async/await with typed Promises
- **Floating Promises** — `Promise<void>` returned and ignored
- **Unhandled rejections** — async errors silently swallowed
- **God Class** — class with too many responsibilities and typed dependencies
- **Shotgun Surgery** — changing one type forces changes across many unrelated files

---

## 6. Observability

### Logging
- Structured logging with typed log schemas
- Log levels: `DEBUG`, `INFO`, `WARN`, `ERROR`, `FATAL`
- Type-safe logger interface — consistent log method signatures
- Correlation IDs typed in request context objects
- Typed log context propagation through services (AsyncLocalStorage)
- Avoiding logging sensitive typed fields (PII, tokens)
- Libraries: `pino` (typed), `winston` with typed transports

### Error Tracking & Handling
- Typed custom error classes (`class AppError extends Error`)
- Typed error hierarchies: `ValidationError`, `NotFoundError`, `AuthError`
- `Result<T, E>` pattern for typed error propagation without exceptions
- Source Maps for production TypeScript debugging
- Global handler types: `process.on('uncaughtException')`, `'unhandledRejection'`
- Sentry SDK with TypeScript — typed breadcrumbs, contexts, tags
- `window.onerror` and `addEventListener('unhandledrejection')` types in browser

### Metrics
- Typed metric interfaces: counters, gauges, histograms
- Web Vitals typed APIs: `PerformanceObserver`, `LayoutShift`, `LargestContentfulPaint`
- `performance.mark()` and `performance.measure()` typed usage
- Custom typed metric collectors
- Node.js: typed wrappers around `process.memoryUsage()`, `process.cpuUsage()`
- Prometheus client libraries with TypeScript types (`prom-client`)

### Distributed Tracing
- OpenTelemetry TypeScript SDK — typed spans, tracers, contexts
- Typed trace context propagation (W3C TraceContext)
- Auto-instrumentation vs manual typed span creation
- Typed attributes and events on spans
- Exporting to Jaeger, Zipkin, Datadog via typed exporters

### Debugging
- Source Maps configuration (`sourceMap: true` in `tsconfig.json`)
- `declarationMap: true` for navigating to source in `.d.ts` consumers
- Chrome DevTools with source maps for browser TypeScript
- VS Code debugger configuration (`.vscode/launch.json`) for TypeScript
- `ts-node` for direct Node.js debugging without compilation step
- Heap snapshots and memory profiling with typed references

### Healthchecks & Runtime Validation
- Typed healthcheck endpoint response schemas
- Runtime validation libraries: `zod`, `io-ts`, `valibot` — typed schema + validation
- Parsing external data at system boundaries (API response validation)
- Feature flags with typed configuration schemas

---

## 7. Architecture

### Application Architecture Patterns
- **Layered Architecture** — typed layers: presentation, application, domain, infrastructure
- **Hexagonal Architecture (Ports & Adapters)** — TypeScript interfaces as ports
- **Clean Architecture** — dependency rule enforced by typed interfaces
- **Vertical Slice Architecture** — feature-based module organization
- **Domain-Driven Design (DDD)** in TypeScript
    - Entities, Value Objects, Aggregates — typed domain models
    - Repositories as typed interfaces
    - Domain Events with typed payloads
    - Bounded Contexts and module boundaries
    - Ubiquitous Language in type names
- **CQRS** — typed Command and Query objects, separate handlers
- **Event Sourcing** — typed event streams and projections
- **Microservices** — typed service contracts (OpenAPI → TypeScript types)

### Module & Project Architecture
- Monorepo architecture — Nx, Turborepo with TypeScript project references
- Package-based monorepo: shared types packages, utility packages
- TypeScript project references (`composite: true`) for incremental builds
- Module boundary enforcement — typed public API surfaces
- Dependency direction rules — inner layers unaware of outer layers

### API Architecture
- REST API: typed request/response with OpenAPI-generated types
- GraphQL: typed schemas with `graphql-codegen`
- tRPC — end-to-end type-safe APIs without code generation
- gRPC with typed protobuf definitions (`ts-proto`)
- WebSocket typed message protocols
- BFF (Backend for Frontend) pattern with typed contracts

### Frontend Architecture
- Component architecture: typed props, typed emits (Vue), typed context (React)
- State management architecture: typed stores (Redux Toolkit, Zustand, Pinia)
- Typed routing: React Router, TanStack Router (full type safety)
- Micro-frontend architecture with typed shared contracts
- Design system typed component libraries

### Backend Architecture (Node.js)
- Dependency injection containers: InversifyJS, TSyringe, NestJS DI
- Typed middleware pipelines (Express, Fastify, Koa)
- Repository pattern with typed ORM integrations (Prisma, TypeORM, Drizzle)
- Typed message queue handlers (BullMQ, RabbitMQ)
- Typed cron jobs and scheduled tasks

---

## 8. Frameworks

### Frontend Frameworks
- **React with TypeScript**
    - Typing functional components: `React.FC` vs explicit props type
    - `useState`, `useReducer`, `useRef`, `useContext` generics
    - Custom hook return types
    - `forwardRef` typing
    - Event handler types: `React.MouseEvent`, `React.ChangeEvent`
    - `children` prop: `React.ReactNode` vs `React.ReactElement`
    - Higher-order components with generics
    - Component generic props pattern
- **Next.js with TypeScript**
    - `GetStaticProps`, `GetServerSideProps`, `GetStaticPaths` types
    - App Router: typed `params`, `searchParams`, `metadata`
    - API Routes typing: `NextRequest`, `NextResponse`
    - `next.config.ts` — typed configuration
- **Vue 3 with TypeScript**
    - `defineComponent`, `defineProps`, `defineEmits`, `defineExpose`
    - `ref<T>()`, `reactive<T>()`, `computed<T>()` generics
    - Typed composables
    - `PropType` utility
- **Angular (TypeScript-first)**
    - Strict template type checking
    - Typed reactive forms (`FormGroup`, `FormControl<T>`)
    - Typed HTTP client (`HttpClient` generics)
    - Dependency injection with typed tokens
    - Signal-based reactivity types

### Backend Frameworks
- **NestJS**
    - Decorators: `@Controller`, `@Injectable`, `@Module`
    - Typed dependency injection with `@Inject`
    - Guards, Pipes, Interceptors with typed interfaces
    - DTO typing with `class-validator` and `class-transformer`
    - Typed Swagger/OpenAPI generation
- **Express with TypeScript**
    - Typed `Request`, `Response`, `NextFunction`
    - Extending `Request` interface via module augmentation
    - Typed middleware and error handlers
- **Fastify with TypeScript**
    - Typed route schemas with `@fastify/type-provider-typebox`
    - TypeBox schemas for JSON Schema + TypeScript types
- **tRPC**
    - Router definitions, procedure types (`query`, `mutation`, `subscription`)
    - Typed context, middleware, and input/output validation with zod
    - Client-side type inference from server router
- **Prisma ORM**
    - Generated typed client — `PrismaClient`
    - Typed queries, relations, and transactions
    - `Prisma.validator` and `Prisma.Args` utility types

### Testing Frameworks
- **Jest with TypeScript** — `ts-jest` configuration, typed mocks
- **Vitest** — native TypeScript support, typed test utilities
- **Testing Library** — typed queries and matchers
- **Playwright / Cypress** — typed page objects and fixtures
- `jest.mock()` with typed module factories
- `vi.fn()` and `jest.fn()` with typed generics
- `MockedFunction<T>`, `MockedObject<T>` patterns

---

## 9. Tools

### Compilers & Transpilers
- **TypeScript Compiler (`tsc`)** — type checking, declaration generation
- **esbuild** — fast transpilation (strips types, no type checking)
- **SWC** (`@swc/core`) — Rust-based transpiler, `isolatedModules` requirement
- **Babel** with `@babel/preset-typescript` — transpile-only, no type checking
- **ts-node** — on-the-fly TypeScript execution for Node.js
- **tsx** — fast TypeScript runner using esbuild

### Build Tools & Bundlers
- **Vite** — TypeScript-first dev server and bundler
- **Webpack** with `ts-loader` or `babel-loader`
- **Rollup** with `@rollup/plugin-typescript` — library bundling
- **Turborepo** — monorepo task runner with TypeScript project references
- **Nx** — monorepo build system with TypeScript graph awareness
- **tsup** — zero-config TypeScript library bundler (wraps esbuild)

### Linting & Formatting
- **ESLint** with `@typescript-eslint/parser` and `@typescript-eslint/eslint-plugin`
- Key ESLint TypeScript rules: `no-explicit-any`, `no-unsafe-*`, `consistent-type-imports`
- **Prettier** — opinionated formatter, works with TypeScript out of the box
- **Biome** — fast all-in-one linter + formatter written in Rust

### Type Generation & Validation
- **zod** — runtime schema validation with inferred TypeScript types
- **valibot** — lightweight alternative to zod
- **io-ts** — functional runtime type checking with `fp-ts` integration
- **typebox** — JSON Schema + TypeScript types (used in Fastify)
- **graphql-codegen** — generate TypeScript types from GraphQL schemas
- **openapi-typescript** — generate types from OpenAPI specs
- **ts-proto** — generate TypeScript from protobuf definitions
- **Prisma** — generate typed DB client from schema

### Developer Experience
- **TypeScript Language Server (tsserver)** — powers editor intellisense
- **VS Code TypeScript integration** — go-to-definition, rename, auto-import
- **ts-morph** — TypeScript compiler API wrapper for code generation/transformation
- **type-fest** — community utility type library
- **ts-reset** — stricter types for built-in JS APIs (`JSON.parse`, `Array.filter`)

### CI & Quality Gates
- `tsc --noEmit` — type checking in CI without emitting files
- `tsc --build` — incremental project reference builds
- ESLint TypeScript rules in CI pipeline
- Type coverage tools: `type-coverage`, `typescript-coverage-report`
- **dtslint** — testing TypeScript declaration files

---

## 10. Performance Engineering

### Compilation Performance
- TypeScript compiler (`tsc`) performance bottlenecks
- `isolatedModules: true` — enables parallel transpilation with esbuild/SWC
- Project references (`composite: true`) — incremental builds for monorepos
- `skipLibCheck: true` tradeoffs — skips `.d.ts` checking for speed
- `incremental: true` and `tsBuildInfoFile` — persistent build cache
- Avoiding deep recursive types — exponential compile time cost
- Avoiding excessively large union types
- Type instantiation depth and width limits
- Narrowing down `@types/*` — include only needed type packages

### Runtime Performance
- TypeScript compiles to JavaScript — all JS performance principles apply
- Avoiding unnecessary object spreading and copying in hot paths
- Typed immutable data structures for predictable V8 optimization
- Avoiding `any` and dynamic dispatch patterns that defeat JIT optimization
- Tree-shaking with `import type` to eliminate type-only imports from bundles
- `const enum` for zero-cost abstractions (inlined at compile time)
- Dead code elimination enabled by precise type narrowing

### Bundle Performance
- `import type` — ensures type imports are stripped and not bundled
- Tree-shaking friendly named exports
- Code splitting with dynamic `import()` — typed chunk boundaries
- Avoiding barrel file re-export anti-pattern that defeats tree-shaking
- Analyzing bundle with `rollup-plugin-visualizer`, `webpack-bundle-analyzer`
- Library mode builds (tsup, Rollup) — `sideEffects: false` in `package.json`

### Memory & Profiling
- Typed object shapes consistent at creation — V8 hidden classes
- Avoiding shape polymorphism in hot code paths
- `WeakMap` and `WeakRef` for cache structures that don't prevent GC
- Heap profiling with Chrome DevTools on TypeScript source maps
- Node.js `--expose-gc` and `process.memoryUsage()` monitoring
- Typed worker thread communication (structured clone, `SharedArrayBuffer`)

### Network & I/O Performance
- Typed streaming APIs (`ReadableStream`, `WritableStream`)
- Typed batching patterns for DB and API calls
- Typed connection pooling interfaces
- HTTP/2 multiplexing — typed client implementations
- Typed caching strategies: in-memory, Redis with typed client (`ioredis`)

---

## 11. Security

### Type System Security
- **Parse, don't validate** — validate at system boundary, use typed domain models internally
- Use `unknown` for external input — never `any`
- Branded types to prevent ID confusion (e.g., `UserId` vs `OrderId`)
- Readonly types to prevent accidental mutation of sensitive data
- Avoid type assertions to bypass security-sensitive type checks
- Typed permission/role models — encode access control in types

### Input Validation & Sanitization
- Validate all external data with runtime schemas (zod, valibot, io-ts)
- Typed sanitization functions with explicit input/output types
- SQL injection prevention — typed ORM queries (Prisma, TypeORM) vs raw string queries
- XSS prevention — typed DOM manipulation (avoid `innerHTML`; use typed `textContent`)
- Typed template literal caution — no unescaped user input in SQL/HTML templates

### Authentication & Authorization
- Typed JWT payloads — decode and validate structure at runtime
- Typed session objects — typed request augmentation in Express/Fastify
- Role-based access control (RBAC) with discriminated union role types
- Typed middleware guards for route protection (NestJS Guards, Express middleware)
- Typed OAuth2 / OIDC token handling
- Typed API key management

### Secrets & Configuration
- Never type secrets as `string` literals in source
- Typed environment variable access with validation (zod + `process.env`)
- Typed configuration schemas — fail fast on invalid environment
- Avoiding secrets in types or comments that end up in `.d.ts` files

### Dependencies & Supply Chain
- `@types/*` package integrity — DefinitelyTyped is community-maintained
- Auditing transitive `@types` packages
- `npm audit` — dependency vulnerability scanning
- Pinning dependency versions for reproducible type environments
- Avoiding untyped packages without `@types` — risk of `any` infiltration

### Cryptography & Sensitive Data
- Typed wrappers for Node.js `crypto` module
- Typed interfaces for encryption/decryption operations
- Typed hashing utilities (bcrypt, argon2) — explicit input/output types
- Typed secure random generation
- Avoiding custom crypto — use typed wrappers over battle-tested libraries

### API Security
- Typed rate limiting middleware
- Typed CORS configuration objects
- Typed Content Security Policy (CSP) headers
- Typed CSRF token validation
- OpenAPI-generated types ensuring client and server contract alignment
- Typed input size limits and pagination constraints

### Runtime Security
- `Object.freeze()` for typed immutable configuration objects
- Prototype pollution prevention — typed `Object.create(null)` maps
- Typed sandbox boundaries for plugin systems
- `eval()` prohibition — ESLint `no-eval` rule
- Typed `postMessage` validation for cross-origin communication

---























































































