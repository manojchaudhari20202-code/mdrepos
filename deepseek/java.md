## Java Fundamentals

### Java platform overview
- Java is a platform-independent, object-oriented programming language based on the principle of "Write Once, Run Anywhere" (WORA) due to its bytecode execution on the Java Virtual Machine (JVM).
- The platform consists of the Java Virtual Machine (JVM), the Java Runtime Environment (JRE), and the Java Development Kit (JDK), providing a complete environment for developing and running Java applications.
- Java is widely used for enterprise applications, Android development, web applications, and big data technologies, owing to its robustness, security, and extensive ecosystem.
- The platform includes a rich set of APIs (Application Programming Interfaces) for tasks such as I/O, networking, collections, concurrency, and GUI development.
- Java's architecture is designed to be secure, with features like bytecode verification, class loaders, and a security manager to protect against malicious code.

### JVM, JRE, JDK differences
- **JDK (Java Development Kit)** is a software development kit that includes the JRE, development tools (like javac, jar, javadoc), and additional libraries for developing Java applications.
- **JRE (Java Runtime Environment)** provides the libraries, JVM, and other components necessary to run Java applications but does not include development tools.
- **JVM (Java Virtual Machine)** is the abstract machine that executes Java bytecode, providing runtime environment features like memory management, garbage collection, and security.
- The JVM is platform-specific; there are different implementations for different operating systems, but they all execute the same bytecode, ensuring platform independence.
- Understanding the distinction is crucial for deployment: JDK is needed for development, JRE for running applications, and JVM is the core runtime engine.

### Java compilation process
- Java source code (.java files) is compiled by the Java compiler (javac) into bytecode (.class files), which is platform-independent.
- The bytecode is then interpreted and/or compiled by the JVM into native machine code at runtime, using Just-In-Time (JIT) compilation for performance.
- The compilation process involves lexical analysis, parsing, semantic analysis, and bytecode generation, ensuring type safety and other language constraints.
- Unlike compiled languages like C++, Java's compilation to bytecode allows for dynamic class loading and linking at runtime.
- The compilation process can be customized with annotation processors that generate additional source files or resources during compilation.

### Bytecode
- Bytecode is an intermediate, platform-independent representation of Java code, stored in .class files, consisting of instructions for the JVM.
- Each bytecode instruction is one byte in length, giving the JVM a compact instruction set (e.g., iload, invokespecial, etc.).
- Bytecode is executed by the JVM through interpretation or JIT compilation, which translates frequently executed bytecode sequences into native machine code for performance.
- The JVM verifies bytecode before execution to ensure it doesn't violate security or type constraints (bytecode verification).
- Tools like javap can disassemble class files to view bytecode, which is useful for debugging and understanding JVM behavior.

### Java program structure
- A Java program consists of one or more classes, with at least one class containing a main method as the entry point: `public static void main(String[] args)`.
- Classes can be organized into packages to avoid naming conflicts and provide access control.
- Java source files typically have the same name as the public class they contain, and the file extension is .java.
- The program can include import statements to bring other classes into scope, and comments for documentation.
- Beyond classes, Java programs may use interfaces, enums, annotations, and records, all of which are top-level types.

### Data types
- Java has two categories of data types: primitive types (byte, short, int, long, float, double, char, boolean) and reference types (classes, interfaces, arrays, enums).
- Primitive types hold their values directly in memory, while reference types hold references to objects stored in the heap.
- Each primitive type has a fixed size and range, e.g., int is 32-bit signed, float is 32-bit IEEE 754 floating point.
- Java also supports literals for primitives (e.g., 42, 3.14, true, 'a') and special literals like null for references.
- Understanding default values: primitives have defaults (e.g., 0 for numeric types, false for boolean), while references default to null.

### Primitive vs reference types
- Primitive types store actual values, while reference types store addresses to objects in memory.
- Primitives are stored on the stack for local variables, whereas reference variables are on the stack but the object they point to is on the heap.
- Operations on primitives are generally faster than on objects due to no indirection or garbage collection overhead.
- Reference types can be null, while primitives cannot; this can lead to NullPointerException when using reference types.
- Primitives are passed by value, while references are also passed by value (the reference itself is copied, but both refer to the same object).

### Variables and scope
- Variables in Java have a specific scope: class-level (static), instance-level (non-static), local (method/block), and parameter scope.
- Local variables must be initialized before use; instance and class variables get default values if not initialized.
- Scope determines visibility and lifetime: local variables exist only within the block, instance variables live as long as the object, static variables live for the entire program.
- Shadowing occurs when a variable in an inner scope has the same name as a variable in an outer scope, hiding the outer variable.
- The `this` keyword is used to refer to the current instance variable when shadowed by a local variable or parameter.

### Operators
- Java provides a rich set of operators: arithmetic (`+`, `-`, `*`, `/`, `%`), relational (`<`, `>`, `<=`, `>=`, `==`, `!=`), logical (`&&`, `||`, `!`), bitwise (`&`, `|`, `^`, `~`, `<<`, `>>`, `>>>`), assignment (`=`, `+=`, etc.), and ternary (`? :`).
- Operators have precedence and associativity rules that determine evaluation order; parentheses can override.
- The `instanceof` operator checks whether an object is an instance of a particular class or interface.
- Short-circuit logical operators (`&&` and `||`) evaluate the second operand only if necessary, which can prevent errors or improve performance.
- Bitwise operators are used for low-level operations, such as flags or performance-critical code.

### Control flow statements
- Control flow statements include decision-making (`if-else`, `switch`), looping (`for`, `while`, `do-while`), and branching (`break`, `continue`, `return`).
- The `switch` statement can now use strings, enums, and primitive types; since Java 12, switch expressions with arrow syntax and yield return values.
- `if-else` can be chained, and nested loops are common, but excessive nesting can reduce readability.
- Enhanced for-each loop (`for (Type var : iterable)`) simplifies iteration over arrays and collections.
- `break` and `continue` can have labels to break out of nested loops, though this can make code harder to follow.

### Loops
- `for` loop: classic three-part loop (initialization, condition, update) used when iteration count is known.
- `while` loop: condition checked before entry, used when number of iterations is unknown.
- `do-while` loop: executes body at least once, condition checked after.
- Enhanced for-each loop: simpler syntax for iterating over arrays and collections, but does not provide index access.
- Loop performance considerations: avoid repeated method calls in condition, use primitive arrays for performance, and consider Stream API for functional style.

### Arrays
- Arrays in Java are objects that hold a fixed number of values of a single type, created with `new` or by initializer (`int[] arr = {1,2,3}`).
- Arrays are zero-indexed, have a `length` property, and provide bounds checking at runtime (throws `ArrayIndexOutOfBoundsException`).
- Multidimensional arrays are arrays of arrays, which can be non-rectangular (jagged arrays).
- Arrays are covariant: an array of a subtype can be assigned to an array of its supertype, which can lead to runtime `ArrayStoreException`.
- Utility methods: `java.util.Arrays` provides sorting, searching, copying, and conversion to List.

### Strings and immutability
- Strings in Java are immutable; once created, their value cannot be changed. Any operation that modifies a string creates a new String object.
- Immutability provides benefits like thread safety, security (e.g., sensitive data can't be altered), and caching (String pool).
- String concatenation using `+` inside loops can be inefficient due to many intermediate objects; `StringBuilder` or `StringBuffer` should be used.
- String class is `final`, so it cannot be subclassed, preserving its immutability guarantees.
- Methods like `substring()`, `concat()`, `replace()` return new strings, leaving original unchanged.

### String pool
- The String pool is a special area in the heap (or metaspace in some JVMs) that stores literal strings and interned strings to save memory and improve performance.
- When a string literal is created, JVM checks the pool; if exists, returns reference; else creates new string and puts in pool.
- Strings created with `new String("...")` are not automatically placed in the pool; they can be manually interned using `intern()` method.
- The pool is maintained by the String class and is garbage-collected like other objects, though interned strings may have longer lifetime.
- Use of `intern()` can reduce memory footprint for duplicate strings, but can also cause performance overhead due to pool management.

### Wrapper classes
- Wrapper classes (Integer, Long, Float, Double, Byte, Short, Character, Boolean) provide object representations of primitive types.
- They are used in collections (which require objects), generics, and for utility methods (e.g., parsing strings to numbers).
- Wrapper classes are immutable and final; they override `equals()`, `hashCode()`, and `toString()`.
- Each wrapper class has caching for common values (e.g., Integer cache for -128 to 127) to improve performance of autoboxing.
- Conversions: `valueOf()` returns cached instances where possible, while `new Integer()` always creates new object (deprecated in Java 9+).

### Autoboxing and unboxing
- Autoboxing is the automatic conversion of primitive types to their corresponding wrapper classes (e.g., `int` to `Integer`) by the compiler.
- Unboxing is the reverse conversion, from wrapper to primitive.
- These conversions happen in assignments, method arguments, and operations (e.g., arithmetic on wrapper objects triggers unboxing).
- Performance considerations: frequent autoboxing/unboxing in loops can create many temporary objects, impacting performance; prefer primitives for heavy numeric computations.
- Null pointer issues: unboxing a null wrapper throws `NullPointerException`; careful handling required.

### Type casting
- Type casting converts a value from one type to another; implicit casting (widening) happens automatically for compatible primitive types (e.g., int to long).
- Explicit casting (narrowing) is required for converting larger to smaller types, potentially losing data; done with `(targetType) value`.
- Reference type casting: upcasting (subclass to superclass) is automatic; downcasting (superclass to subclass) requires explicit cast and may throw `ClassCastException` if object is not of the target type.
- The `instanceof` operator should be used before downcasting to avoid exceptions.
- Casting with generics involves type erasure, leading to unchecked warnings; best to avoid raw types.

### Pass by value
- Java is strictly pass-by-value: when passing a primitive, a copy of the value is passed; when passing an object reference, a copy of the reference is passed.
- This means that inside a method, reassigning the parameter variable does not affect the original variable outside.
- However, modifications to the object's state via the reference are visible to the caller because both references point to the same object.
- Understanding this is crucial for avoiding confusion in method design and debugging.
- This concept is often tested in interviews; be prepared to explain with examples.

### Java keywords
- Java has about 50 reserved keywords (e.g., `class`, `public`, `static`, `void`, `if`, `else`, `for`, `while`, `return`, `new`, `try`, `catch`, `finally`, `throw`, `throws`, `import`, `package`, `extends`, `implements`, `interface`, `enum`, `synchronized`, `volatile`, `transient`, `native`, `strictfp`, `assert`, `goto` (not used), etc.)
- Keywords have special meaning and cannot be used as identifiers (variable names, class names, etc.)
- `const` and `goto` are reserved but not used; `true`, `false`, `null` are literals, not keywords.
- Newer Java versions have added keywords like `var` (reserved type name), `yield`, `record`, `sealed`, `permits` (contextual keywords).
- Understanding keywords is fundamental to writing correct Java code; misuse leads to compilation errors.

## Object-Oriented Programming

### OOP principles
- The four fundamental OOP principles are Encapsulation, Inheritance, Polymorphism, and Abstraction.
- Encapsulation bundles data and methods that operate on that data, hiding internal state and requiring interaction through public methods.
- Inheritance allows a class to inherit properties and behaviors from a parent class, promoting code reuse and establishing a hierarchical relationship.
- Polymorphism enables objects of different classes to be treated as objects of a common superclass, with methods behaving appropriately based on actual object type (runtime polymorphism).
- Abstraction hides complex implementation details and exposes only essential features, often achieved through abstract classes and interfaces.

### Encapsulation
- Encapsulation is achieved by declaring instance variables as private and providing public getter and setter methods to access and modify them.
- It protects the integrity of data by controlling how it is accessed and updated, allowing validation or other logic in setters.
- Encapsulation helps in achieving loose coupling between components, as internal implementation can change without affecting external code.
- In Java, encapsulation is also supported by access modifiers (private, protected, public) and packages.
- Proper encapsulation reduces complexity and increases maintainability by clearly defining the interface of a class.

### Inheritance
- Inheritance in Java is implemented using the `extends` keyword for classes and `implements` for interfaces.
- A subclass inherits all non-private members (fields, methods) from its superclass, but constructors are not inherited.
- Java supports single inheritance for classes (a class can have only one direct superclass) to avoid diamond problem, but multiple inheritance of type is allowed through interfaces.
- The `super` keyword is used to refer to superclass members and invoke superclass constructors.
- Inheritance can lead to fragile base class problems if not designed carefully; composition is often preferred over inheritance for code reuse.

### Polymorphism
- Polymorphism in Java comes in two forms: compile-time (static) polymorphism via method overloading, and runtime (dynamic) polymorphism via method overriding.
- Runtime polymorphism relies on inheritance and virtual methods (non-static, non-final, non-private methods) to invoke the overridden method based on the actual object type, not reference type.
- Polymorphism allows code to be written against interfaces or abstract classes, making it flexible and extensible.
- It enables the implementation of design patterns like Strategy, Template Method, and Factory.
- Understanding polymorphism is key to leveraging Java's object-oriented capabilities effectively.

### Abstraction
- Abstraction focuses on hiding complex implementation details and showing only the essential features of an object.
- In Java, abstraction can be achieved through abstract classes (which can have abstract and concrete methods) and interfaces (which provide pure abstraction until Java 8 introduced default and static methods).
- Abstract classes are used when there is a common base with some shared implementation; interfaces are used to define a contract that multiple unrelated classes can implement.
- Abstraction helps in reducing code duplication and improving maintainability by separating what an object does from how it does it.
- It also supports modularity and scalability in large systems.

### Method overloading
- Method overloading allows multiple methods in the same class to have the same name but different parameter lists (different type, number, or order).
- Overloading is resolved at compile time based on the method signature (static polymorphism).
- Return type alone is not sufficient to distinguish overloaded methods; the parameter list must differ.
- Overloading is commonly used in constructors and utility methods (e.g., `System.out.println()` overloaded for all primitive types).
- Care must be taken with autoboxing and varargs in overloaded methods, as they can lead to ambiguous calls.

### Method overriding
- Method overriding occurs when a subclass provides a specific implementation of a method already defined in its superclass, with the same name, parameters, and return type (covariant return types allowed).
- The overridden method in the subclass must not have a more restrictive access modifier, and cannot throw broader checked exceptions than the superclass method.
- Overriding is the basis of runtime polymorphism; the `@Override` annotation is recommended to catch errors at compile time.
- Static methods cannot be overridden, only hidden (if subclass defines a static method with same signature, it hides the superclass method).
- Final methods cannot be overridden; private methods are not inherited and thus cannot be overridden.

### Constructors
- Constructors are special methods invoked when an object is created, used to initialize the object's state.
- They have the same name as the class and no return type (not even void).
- If no constructor is defined, the compiler provides a default no-argument constructor.
- Constructors can be overloaded; one constructor can call another using `this()` (must be first statement).
- In inheritance, the subclass constructor must call a superclass constructor, either implicitly (super() added by compiler) or explicitly with `super(...)`.

### Object lifecycle
- The lifecycle of an object in Java includes creation, usage, and destruction.
- Creation: memory allocated on heap, constructor executed, object becomes reachable.
- Usage: object referenced and used; its state may change.
- Destruction: when object becomes unreachable (no live references), it becomes eligible for garbage collection; finalization (deprecated) or cleaner can be used for cleanup before GC.
- The JVM's garbage collector automatically reclaims memory, but objects with non-memory resources (files, connections) should be closed explicitly (try-with-resources).
- Understanding object lifecycle is crucial for managing resources and avoiding memory leaks.

### this vs super
- `this` refers to the current object instance; it can be used to access instance variables, methods, and call other constructors (`this(...)`) in the same class.
- `super` refers to the immediate superclass; it is used to access superclass members (fields, methods) and call superclass constructors (`super(...)`).
- Both `this` and `super` cannot be used in static contexts.
- In case of method overriding, `super.method()` can be used to invoke the superclass version.
- `this` can be passed as an argument to another method or returned from a method to enable method chaining.

### Static members
- Static members (variables, methods, blocks) belong to the class itself, not to any instance; they are shared across all instances.
- Static variables are initialized when the class is first loaded; they have a single copy in memory.
- Static methods can only directly access other static members (cannot access instance variables/methods without an object reference).
- Static blocks are used for static initialization; they execute once when the class is loaded.
- Static members are accessed via class name, though they can also be accessed via instance references (not recommended, as it can be misleading).

### Final keyword
- The `final` keyword can be applied to variables, methods, and classes, with different effects:
  - final variable: cannot be reassigned; for primitives, value fixed; for references, reference cannot point to another object (but object state can change).
  - final method: cannot be overridden by subclasses; useful to prevent modification of critical behavior.
  - final class: cannot be subclassed; used for immutability or security (e.g., String, Integer).
- Final variables must be initialized either at declaration or in constructor (for instance finals) or static initializer (for static finals).
- Using final can help with thread safety and performance (compiler optimizations).

### Access modifiers
- Access modifiers control visibility of classes, methods, and fields: `private`, `protected`, `public`, and package-private (default, no modifier).
- `private`: accessible only within the same class.
- Package-private (default): accessible within the same package.
- `protected`: accessible within same package and by subclasses (even in different packages).
- `public`: accessible from anywhere.
- Proper use of access modifiers enforces encapsulation and defines the API surface.

### Composition vs inheritance
- Composition is a design principle where a class contains references to objects of other classes, representing a "has-a" relationship.
- Inheritance represents an "is-a" relationship; a subclass extends a superclass.
- Composition is often favored over inheritance because it promotes greater flexibility, looser coupling, and easier testing.
- Inheritance can lead to fragile code if the superclass changes; composition uses interfaces to define behavior.
- Both can be used together; for example, using composition with interfaces to achieve polymorphism (strategy pattern).

### Association, aggregation
- Association represents a relationship between two classes where objects of one class know about objects of another class; it can be unidirectional or bidirectional.
- Aggregation is a specialized form of association representing a whole-part relationship where the part can exist independently of the whole (e.g., Department and Employee).
- Composition is a stronger form of aggregation where the part cannot exist without the whole; the whole is responsible for creation/destruction of parts (e.g., House and Room).
- In UML, aggregation is denoted by a hollow diamond, composition by a filled diamond.
- These concepts are important in object-oriented design to model real-world relationships appropriately.

### Object equality (equals/hashCode)
- The `equals()` method determines logical equality of objects; by default, it compares references (same as `==`), so it must be overridden for value-based equality.
- The `hashCode()` method returns an integer hash code, used in hash-based collections (HashMap, HashSet) to efficiently locate objects.
- Contract: if two objects are equal according to `equals()`, they must have the same hashCode; if not equal, they may have same hashCode (but collisions degrade performance).
- Overriding `equals` requires following properties: reflexive, symmetric, transitive, consistent, and non-null comparison.
- Always override `hashCode` when overriding `equals` to maintain the contract; use Objects.hash() or a well-defined algorithm.

### toString contract
- The `toString()` method returns a string representation of the object; by default, it returns class name + @ + hashCode in hex.
- It's recommended to override `toString()` to provide meaningful information about the object's state for debugging and logging.
- There's no formal contract like for equals/hashCode, but it should generally return a concise, informative representation.
- The `toString()` method is implicitly called when an object is concatenated with a string using `+` or passed to `System.out.println()`.
- Override `toString()` in domain objects to improve log readability; consider using Apache Commons ToStringBuilder or IDE-generated code.

## Java Classes & Core APIs

### Object class methods
- The `Object` class is the root of the class hierarchy; all classes inherit its methods: `equals()`, `hashCode()`, `toString()`, `clone()`, `finalize()`, `getClass()`, `wait()`, `notify()`, `notifyAll()`.
- `getClass()` returns the runtime class of the object, which can be used for reflection.
- `clone()` creates a shallow copy of an object; the class must implement `Cloneable` marker interface to avoid `CloneNotSupportedException`.
- `finalize()` is deprecated (Java 9) as it's unpredictable; use `Cleaner` or `PhantomReference` for resource cleanup.
- `wait()`, `notify()`, `notifyAll()` are used for inter-thread communication and must be called from synchronized context.

### String vs StringBuilder vs StringBuffer
- `String` is immutable; any modification creates a new object, making it thread-safe but inefficient for frequent changes.
- `StringBuilder` is mutable and not thread-safe, providing better performance for single-threaded string manipulations.
- `StringBuffer` is mutable and thread-safe (synchronized methods), but slower than StringBuilder due to synchronization overhead.
- Choose `String` for fixed strings, `StringBuilder` for single-threaded concatenation, `StringBuffer` for multi-threaded scenarios (rare).
- All three implement `CharSequence` interface, allowing polymorphic usage.

### Math API
- The `java.lang.Math` class provides static methods for common mathematical operations: `abs()`, `ceil()`, `floor()`, `round()`, `max()`, `min()`, `pow()`, `sqrt()`, `sin()`, `cos()`, `tan()`, `log()`, `exp()`, etc.
- It also contains constants `Math.PI` and `Math.E`.
- Methods like `random()` generate a pseudorandom double between 0.0 and 1.0.
- For more advanced random number generation, use `java.util.Random` or `ThreadLocalRandom`.
- StrictMath ensures reproducible results across platforms, while Math may use platform-specific optimizations.

### Date and time API (Java 8+)
- The `java.time` package (JSR-310) provides a comprehensive date-time API, addressing flaws in legacy `Date` and `Calendar`.
- Key classes: `LocalDate`, `LocalTime`, `LocalDateTime` (no time zone), `ZonedDateTime`, `Instant` (machine timestamp), `Duration`, `Period`.
- The API is immutable and thread-safe, with fluent methods for manipulation.
- It supports time zones via `ZoneId`, formatting/parsing with `DateTimeFormatter`.
- Legacy code can be converted using `Date.from(instant)` and `Date.toInstant()`.

### Optional class
- `Optional<T>` is a container object that may or may not contain a non-null value, used to avoid null checks and `NullPointerException`.
- Common methods: `of()`, `ofNullable()`, `empty()`, `isPresent()`, `ifPresent()`, `orElse()`, `orElseGet()`, `orElseThrow()`, `map()`, `flatMap()`, `filter()`.
- It is not intended to replace all null checks but to indicate that a value may be absent, promoting better API design.
- Avoid using `Optional` for fields, method parameters, or collections (use empty collections instead).
- Overuse can lead to performance overhead and code complexity; use judiciously.

### UUID
- `UUID` (Universally Unique Identifier) is a 128-bit value represented as a hexadecimal string, used for unique identifiers.
- The `java.util.UUID` class provides methods to generate random (type 4) and time-based (type 1) UUIDs via `randomUUID()` and `nameUUIDFromBytes()`.
- UUIDs are immutable and can be parsed from strings using `UUID.fromString()`.
- They are commonly used as primary keys in distributed systems, session IDs, or file names.
- Collisions are extremely unlikely, but not impossible; suitable for most non-security-critical applications.

### Enums
- Enums in Java are special classes that define a fixed set of constants, using the `enum` keyword.
- They are implicitly `final`, extend `java.lang.Enum`, and cannot be instantiated with `new`.
- Enums can have fields, methods, and constructors (private), allowing behavior attached to each constant.
- They provide type safety and can be used in `switch` statements; also have built-in methods like `values()`, `valueOf()`, and `ordinal()`.
- Enums can implement interfaces, making them useful for implementing singleton patterns or strategy patterns.

### Records (Java 14+)
- Records are a special kind of class designed to model immutable data carriers, with a concise syntax: `record Point(int x, int y) {}`.
- They implicitly have a canonical constructor, `equals()`, `hashCode()`, and `toString()` based on all components.
- Records are `final` and cannot be extended; they can implement interfaces but cannot extend other classes.
- They can have additional static fields, methods, and custom constructors (with validation), but cannot have instance fields beyond the components.
- Records reduce boilerplate code and are ideal for DTOs, value objects, and tuple-like structures.

### Annotations
- Annotations provide metadata about code, which can be processed at compile time or runtime via reflection.
- Built-in annotations: `@Override`, `@Deprecated`, `@SuppressWarnings`, `@FunctionalInterface`.
- Meta-annotations: `@Retention` (source, class, runtime), `@Target` (where annotation can be applied), `@Inherited`, `@Documented`.
- Custom annotations can be defined with `@interface` and can have elements (methods) with default values.
- Annotation processing can be done at compile-time using annotation processors (e.g., Lombok) or runtime via reflection.

### Packages
- Packages organize classes into namespaces, preventing naming conflicts and providing access control.
- They correspond to directories in the file system, following naming conventions (reverse domain, e.g., `com.example.myapp`).
- Classes in the same package can access each other's package-private members.
- The `import` statement allows using classes from other packages without fully qualified names.
- Modularization in Java 9+ with modules (JPMS) adds another layer of encapsulation beyond packages.

### Class loading basics
- Class loading is the process of loading class files into memory, performed by the ClassLoader subsystem.
- Java has a hierarchical class loader architecture: Bootstrap (core classes), Extension (optional), and Application/System (classpath).
- Class loaders follow delegation model: when asked to load a class, they delegate to parent first; if parent cannot find, they attempt to load themselves.
- Custom class loaders can be created by extending `ClassLoader` to load classes from non-standard sources (network, database).
- Understanding class loading is important for application servers, dynamic code loading, and avoiding issues like `ClassNotFoundException` or `NoClassDefFoundError`.

## Collections Framework

### Collection hierarchy
- The Java Collections Framework provides a unified architecture for storing and manipulating groups of objects.
- Core interfaces: `Collection` (root interface), `List`, `Set`, `Queue`, `Deque`, and `Map` (separate hierarchy).
- `Collection` extends `Iterable`, providing ability to iterate using for-each loop.
- Main implementations: `ArrayList`, `LinkedList`, `HashSet`, `LinkedHashSet`, `TreeSet`, `ArrayDeque`, `HashMap`, `LinkedHashMap`, `TreeMap`, etc.
- The framework also includes utility classes `Collections` and `Arrays` for sorting, searching, and synchronization.

### List interface
- `List` is an ordered collection (sequence) that allows duplicate elements and provides positional access.
- Main implementations: `ArrayList` (resizable array), `LinkedList` (doubly-linked list), `Vector` (legacy, synchronized), `Stack`.
- Key methods: `add(index, element)`, `get(index)`, `set(index, element)`, `remove(index)`, `indexOf()`, `subList()`.
- `List` supports `ListIterator` for bidirectional traversal.
- Lists can be sorted using `Collections.sort()` or `List.sort()` (since Java 8).

### Set interface
- `Set` is a collection that cannot contain duplicate elements, modeling mathematical set abstraction.
- Main implementations: `HashSet` (hash table, unordered), `LinkedHashSet` (maintains insertion order), `TreeSet` (sorted, based on natural order or comparator).
- `Set` uses `equals()` and `hashCode()` to determine element uniqueness.
- No positional access or ordering guarantees except for specific implementations.
- Useful for eliminating duplicates and membership testing.

### Map interface
- `Map` stores key-value pairs, with each key mapping to at most one value; keys are unique.
- Main implementations: `HashMap` (hash table, unordered), `LinkedHashMap` (maintains insertion or access order), `TreeMap` (sorted by keys), `Hashtable` (legacy, synchronized), `ConcurrentHashMap`.
- Key methods: `put(key, value)`, `get(key)`, `remove(key)`, `containsKey()`, `containsValue()`, `keySet()`, `values()`, `entrySet()`.
- Maps are not part of `Collection` hierarchy but are integral to the framework.
- `Map.Entry` interface represents a key-value pair.

### Queue interface
- `Queue` represents a collection designed for holding elements prior to processing, typically following FIFO order.
- Main implementations: `LinkedList` (also implements `Queue`), `PriorityQueue` (priority heap), `ArrayDeque` (resizable array, double-ended).
- Queue methods: `offer()` (insert), `poll()` (retrieve and remove head, or null if empty), `remove()` (similar but throws exception), `peek()` (retrieve head without removal, or null), `element()` (throws exception).
- `Deque` (double-ended queue) extends `Queue`, allowing insertion/removal at both ends.

### ArrayList internals
- `ArrayList` is a resizable array implementation of `List` interface, backed by an Object array.
- Initial capacity is 10 by default (since Java 8), but can be specified; grows dynamically when needed by creating a new array and copying (using `Arrays.copyOf`).
- Growth factor is roughly 1.5x (oldCapacity + (oldCapacity >> 1)).
- Random access is O(1), but insertion/removal at arbitrary positions is O(n) due to shifting elements.
- Not thread-safe; concurrent modifications can cause `ConcurrentModificationException`; use `CopyOnWriteArrayList` for concurrent scenarios.

### LinkedList internals
- `LinkedList` is a doubly-linked list implementation of `List` and `Deque` interfaces.
- Each element (node) contains data and references to previous and next nodes.
- Insertion/removal at beginning or end is O(1); at arbitrary position requires traversal O(n).
- Implements both `List` and `Deque`, so can be used as stack, queue, or double-ended queue.
- More memory overhead per element compared to `ArrayList` due to node objects.

### HashMap internals
- `HashMap` stores key-value pairs in an array of buckets (Node<K,V>[] table), using hashing to determine bucket index.
- Default initial capacity 16, load factor 0.75; when threshold exceeded, resizes (doubles capacity) and rehashes.
- In case of hash collisions, entries are stored in linked lists (or tree bins since Java 8 for performance when many collisions, converting to balanced tree when list length > 8 and capacity >= 64).
- `hashCode()` of key determines bucket; `equals()` used to find exact key within bucket.
- Not thread-safe; can cause infinite loops in concurrent resize (pre-Java 8) or data corruption; use `ConcurrentHashMap` for concurrency.

### HashSet internals
- `HashSet` is implemented using a `HashMap` internally, where each element is stored as a key in the map with a constant dummy value (present).
- It uses the key's `hashCode()` and `equals()` for uniqueness.
- Operations like `add()`, `remove()`, `contains()` delegate to the underlying `HashMap`.
- Order is not guaranteed; iteration order may change over time.
- `LinkedHashSet` extends `HashSet` but uses `LinkedHashMap` to maintain insertion order.

### TreeMap and TreeSet
- `TreeMap` is a Red-Black tree based implementation of `NavigableMap`, storing keys in sorted order (natural ordering or by provided `Comparator`).
- `TreeSet` is backed by a `TreeMap`, with elements as keys, providing sorted set.
- Operations like `put()`, `get()`, `remove()` have O(log n) time complexity.
- They implement `NavigableSet`/`NavigableMap` offering methods like `lower()`, `floor()`, `ceiling()`, `higher()` for range queries.
- Not thread-safe; can be wrapped with `Collections.synchronizedSortedMap()`.

### Comparable vs Comparator
- `Comparable` is an interface (java.lang) that allows an object to compare itself to another of the same type, defining natural ordering via `compareTo()` method.
- `Comparator` is an interface (java.util) that compares two objects of possibly different types, allowing custom ordering strategies.
- If a class implements `Comparable`, its objects can be sorted automatically (e.g., `Collections.sort(list)`).
- `Comparator` can be passed to sort methods to override natural order or for classes not implementing `Comparable`.
- Both should be consistent with `equals()` to avoid unexpected behavior in sorted collections.

### Sorting mechanisms
- Sorting in collections can be done using `Collections.sort()` (for lists) or `Arrays.sort()` (for arrays).
- `Collections.sort()` internally uses `Arrays.sort()` which for objects uses TimSort (a hybrid stable sort) or legacy MergeSort.
- For primitive arrays, `Arrays.sort()` uses dual-pivot Quicksort (for ints, etc.) for performance.
- Custom sorting can be achieved by providing a `Comparator`.
- Sorting stability: stable sorts preserve the relative order of equal elements; TimSort is stable.

### Iterators
- Iterators allow traversal of collections without exposing underlying structure.
- `Iterator` interface provides `hasNext()`, `next()`, and `remove()` (optional).
- `ListIterator` extends `Iterator` for bidirectional traversal and modification of lists.
- Enhanced for-each loop uses `Iterator` behind the scenes; any class implementing `Iterable` can be used in for-each.
- Iterators are fail-fast: if collection is structurally modified after iterator creation (except via iterator's own `remove()`), they throw `ConcurrentModificationException`.

### Fail-fast vs fail-safe
- Fail-fast iterators detect concurrent modification and throw `ConcurrentModificationException` to prevent undefined behavior (e.g., iterators of `ArrayList`, `HashMap`).
- Fail-safe iterators operate on a snapshot of the collection or allow modification without throwing exceptions, typically used in concurrent collections (e.g., `CopyOnWriteArrayList`, `ConcurrentHashMap` iterators).
- Fail-safe iterators do not throw `ConcurrentModificationException` but may not reflect the latest changes.
- The distinction is important in multi-threaded environments to choose appropriate collections.
- Note: The term "fail-safe" is sometimes used loosely; in Java, concurrent collections provide weakly consistent iterators.

### Concurrent collections
- Java provides thread-safe collections in `java.util.concurrent` package for high concurrency.
- Examples: `ConcurrentHashMap` (highly concurrent, lock striping), `CopyOnWriteArrayList` (for read-heavy lists), `ConcurrentLinkedQueue` (non-blocking queue), `BlockingQueue` implementations (`ArrayBlockingQueue`, `LinkedBlockingQueue`).
- These collections use advanced techniques like lock-free algorithms, volatile variables, and CAS (compare-and-swap) to achieve thread-safety with better performance than synchronized wrappers.
- They are designed for concurrent access and provide weakly consistent iterators.
- Choosing the right concurrent collection is crucial for scalable multi-threaded applications.

### Immutable collections
- Immutable collections cannot be modified after creation, providing thread safety and predictability.
- In Java 9+, `List.of()`, `Set.of()`, `Map.of()` create immutable collections (unmodifiable, not just views).
- Prior to Java 9, `Collections.unmodifiableList()` etc. create unmodifiable views of existing collections, but the backing collection can still change.
- Immutable collections do not allow `null` elements and throw `UnsupportedOperationException` on modification attempts.
- They are useful for defining constants and sharing data without defensive copying.

### Collection performance tradeoffs
- `ArrayList`: O(1) random access, O(n) insertion/removal in middle; good for frequent reads and appends.
- `LinkedList`: O(n) random access, O(1) insertion/removal at ends; better for frequent modifications at ends.
- `HashSet`/`HashMap`: O(1) average for add/remove/contains, assuming good hash distribution; but higher memory overhead.
- `TreeSet`/`TreeMap`: O(log n) operations, sorted, but slower than hash-based.
- Choosing the right collection depends on access patterns, concurrency requirements, and memory constraints.

## Exception Handling

### Exception hierarchy
- All exceptions and errors are subclasses of `Throwable` (java.lang).
- Two main branches: `Exception` (for recoverable conditions) and `Error` (for serious problems, typically not handled).
- `RuntimeException` and its subclasses are unchecked exceptions; all other `Exception` subclasses are checked.
- Common checked exceptions: `IOException`, `SQLException`, `ClassNotFoundException`.
- Common unchecked exceptions: `NullPointerException`, `IllegalArgumentException`, `IndexOutOfBoundsException`, `ArithmeticException`.

### Checked vs unchecked exceptions
- Checked exceptions are checked at compile time; the method must either handle them (try-catch) or declare them in its throws clause.
- Unchecked exceptions (runtime exceptions and errors) are not checked at compile time; they can be left unhandled (though they may cause program termination).
- Checked exceptions represent conditions outside the program's control (e.g., file not found), while unchecked typically indicate programming bugs (e.g., null pointer).
- Overuse of checked exceptions can lead to cluttered code; many modern frameworks prefer unchecked exceptions.
- Decision to use checked vs unchecked should be based on whether the caller can reasonably recover.

### try-catch-finally
- `try` block encloses code that may throw exceptions.
- `catch` blocks handle specific exception types; multiple catch blocks can be used (order matters: more specific first).
- `finally` block executes regardless of whether an exception is thrown or caught, typically for cleanup (closing resources).
- Since Java 7, try-with-resources automatically closes resources that implement `AutoCloseable`, reducing need for finally.
- `catch` can also catch multiple exceptions in one block using multi-catch (e.g., `catch (IOException | SQLException e)`).

### throw vs throws
- `throw` is used to explicitly throw an exception from a method or block (e.g., `throw new IllegalArgumentException("msg")`).
- `throws` is used in method signature to declare that the method may throw certain checked exceptions (e.g., `void readFile() throws IOException`).
- `throw` transfers control to the nearest catch block; `throws` is part of the method contract for callers.
- A method can `throw` multiple exceptions, but only need to declare them if they are checked.
- Runtime exceptions can be thrown without declaration.

### Custom exceptions
- Custom exceptions are created by extending `Exception` (for checked) or `RuntimeException` (for unchecked).
- Typically, include constructors that mimic superclass constructors (e.g., with message, cause).
- Can add custom fields and methods to provide additional error information.
- Should follow naming convention: class name ending with "Exception".
- Use custom exceptions to represent domain-specific error conditions, improving code clarity and maintainability.

### Exception propagation
- When an exception is thrown and not caught in the current method, it propagates up the call stack to the calling method, and so on.
- If no catch block handles it, the program terminates (for main thread) and prints stack trace.
- Checked exceptions must be either caught or declared in every method along the propagation chain.
- Propagation can be controlled by rethrowing exceptions, possibly after logging or wrapping.
- Understanding propagation helps in designing error handling strategies.

### Best practices
- Catch specific exceptions rather than generic `Exception` or `Throwable`.
- Avoid catching `Error` or `RuntimeException` unless you have a specific reason.
- Use try-with-resources for auto-closable resources.
- Log exceptions with appropriate details (stack trace, context) for debugging.
- Don't ignore exceptions with empty catch blocks; at least log them.
- Throw exceptions early (fail-fast) and handle them late at appropriate layers.
- Consider using unchecked exceptions for programming errors and checked for recoverable conditions.

## Java Generics

### Generic classes
- Generic classes allow type parameters, enabling type-safe operations on various types without casting.
- Syntax: `class Box<T> { private T t; ... }` where T is a type parameter.
- Multiple type parameters: `class Pair<K, V> { ... }`.
- Type parameters can be used for fields, method return types, and parameter types.
- Instantiations: `Box<String> box = new Box<>();` (diamond operator infers type).
- Generic classes are checked at compile time for type consistency.

### Generic methods
- Generic methods introduce their own type parameters, independent of class generics.
- Syntax: `public <T> T method(T t) { ... }` (type parameter before return type).
- Can be static or non-static; can have multiple bounds.
- Type inference: the compiler deduces type arguments from method invocation.
- Useful for utility methods (e.g., `Collections.sort()` with generic type).

### Type erasure
- Generics are implemented via type erasure: generic type information is removed at compile time and replaced with bounds or Object.
- This ensures backward compatibility with pre-generics code (raw types).
- After erasure, `List<String>` becomes `List` (raw), and type casts are inserted where needed.
- Bridge methods may be generated to preserve polymorphism for overriding.
- Type erasure has implications: cannot use instanceof with parameterized types, cannot create arrays of parameterized types, cannot use primitive type arguments.

### Bounded types
- Bounded type parameters restrict the types that can be used as type arguments.
- Upper bound: `<T extends Number>` means T must be Number or its subclass; can also specify interfaces with `&`.
- Lower bound: not directly on type parameter, but used in wildcards (`? super Integer`).
- Multiple bounds: `<T extends Number & Serializable>`; class first, then interfaces.
- Bounded types allow calling methods defined in the bound.

### Wildcards
- Wildcards (`?`) represent unknown types in generics, used in method signatures and field declarations.
- Unbounded wildcard: `List<?>` means list of unknown type; can only read as Object, cannot add (except null).
- Upper bounded wildcard: `List<? extends Number>` can read as Number, cannot add (except null) because actual type could be subclass.
- Lower bounded wildcard: `List<? super Integer>` can add Integer or its subclasses, but reading yields Object.
- Wildcards provide flexibility for API design, enabling covariance and contravariance.

### Covariance and contravariance
- Covariance: allows a subtype to be used where a supertype is expected (e.g., `List<? extends Number>` is covariant for reading).
- Contravariance: allows a supertype to be used where a subtype is expected (e.g., `List<? super Integer>` is contravariant for writing).
- Arrays are covariant: `Number[] numArr = new Integer[10];` but this can cause `ArrayStoreException`.
- Generics are invariant by default: `List<Number>` is not a subtype of `List<Integer>`.
- Wildcards enable covariance and contravariance for generics, providing type safety.

### PECS principle
- PECS stands for "Producer Extends, Consumer Super" (from Effective Java by Joshua Bloch).
- When a collection is a producer of elements (you read from it), use `? extends T` to accept any subtype of T.
- When a collection is a consumer of elements (you write to it), use `? super T` to accept any supertype of T.
- If both reading and writing, do not use wildcard; use exact type.
- This principle ensures type safety and flexibility when using wildcards in APIs.

## Java I/O & NIO

### File handling
- File handling in Java traditionally uses `java.io.File` class (represents file/directory path) but has limitations (e.g., lack of symbolic link support, error handling).
- Since Java 7, `java.nio.file.Path` and `Files` classes provide more robust file operations.
- Common operations: creating, deleting, moving, copying files; checking permissions; reading/writing attributes.
- `Files` utility class has methods like `readAllLines()`, `write()`, `copy()`, `move()`, `delete()`, `exists()`.
- Use try-with-resources for streams to ensure proper closure.

### Streams API (I/O streams)
- I/O streams are sequences of data, either byte-oriented (`InputStream`, `OutputStream`) or character-oriented (`Reader`, `Writer`).
- Streams are used for reading/writing data from/to sources like files, network sockets, memory.
- They are blocking I/O by default; for non-blocking, use NIO channels.
- Streams can be chained with decorators (e.g., `BufferedInputStream` adds buffering).
- Key methods: `read()`, `write()`, `close()`, `flush()`.

### Byte streams vs character streams
- Byte streams handle raw binary data (bytes); classes end with `Stream` (e.g., `FileInputStream`, `ByteArrayOutputStream`).
- Character streams handle text data, converting bytes to characters using charset encoding; classes end with `Reader`/`Writer` (e.g., `FileReader`, `BufferedWriter`).
- Character streams are built on top of byte streams with encoding/decoding.
- For text files, character streams are recommended; for images, binary data, byte streams.
- Always specify charset explicitly (e.g., `StandardCharsets.UTF_8`) to avoid platform-dependent behavior.

### Buffered streams
- Buffered streams wrap unbuffered streams to reduce the number of I/O operations by using an internal buffer.
- `BufferedInputStream` and `BufferedOutputStream` for bytes; `BufferedReader` and `BufferedWriter` for characters.
- They improve performance by reading/writing larger chunks at once.
- Important to `flush()` buffered output streams to ensure data is written.
- `BufferedReader` provides `readLine()` for convenient line-by-line reading.

### Serialization
- Serialization converts an object into a byte stream to save to file, send over network, etc.; deserialization reconstructs object.
- A class must implement `Serializable` (marker interface) to be serialized.
- `transient` fields are not serialized; `static` fields belong to class, not object, so not serialized.
- Default serialization can be customized by implementing `writeObject()` and `readObject()` methods.
- `serialVersionUID` is a version control identifier; if not declared, JVM generates one, which can cause `InvalidClassException` if class changes.

### Externalization
- `Externalizable` interface extends `Serializable`, giving full control over serialization process via `writeExternal()` and `readExternal()` methods.
- It requires a public no-arg constructor for deserialization.
- Externalization can improve performance by allowing custom format and selective serialization.
- Unlike default serialization, externalization does not automatically write object fields; you must explicitly write/read them.
- Use when default serialization is insufficient or too slow.

### NIO overview
- Java NIO (New I/O, introduced in Java 1.4, enhanced in Java 7 with NIO.2) provides non-blocking I/O, buffers, channels, and selectors.
- NIO.2 added improved file system APIs (`Path`, `Files`, `FileSystem`), asynchronous I/O, and file system operations.
- NIO is more scalable for high-concurrency applications (e.g., servers) due to non-blocking capabilities.
- Core concepts: Channels (connections to entities like files, sockets), Buffers (data containers), Selectors (multiplexing of channels).
- NIO also includes memory-mapped files and file locking.

### Channels and buffers
- Channels are similar to streams but bidirectional; can read and write concurrently (e.g., `FileChannel`, `SocketChannel`).
- Buffers are fixed-size blocks of memory used to transfer data between channel and application.
- Buffer types: `ByteBuffer`, `CharBuffer`, `IntBuffer`, etc.; have properties: capacity, position, limit, mark.
- Data flow: read from channel into buffer, then process buffer; or fill buffer and write to channel.
- Channels can operate in non-blocking mode, allowing efficient thread usage.

### File system APIs (NIO.2)
- `Path` interface represents a file or directory path; created via `Paths.get()` or `FileSystem.getPath()`.
- `Files` class provides static methods for file operations: `copy()`, `move()`, `delete()`, `createDirectory()`, `walkFileTree()`, `find()`, `lines()`.
- `FileSystem` represents the underlying file system; can access file stores, root directories.
- Supports symbolic links, file attributes (e.g., `BasicFileAttributes`), and file change notifications via `WatchService`.
- These APIs are more robust and feature-rich than legacy `java.io.File`.

### Memory-mapped files
- Memory-mapped files allow direct reading/writing of file data as if it were in memory, using virtual memory.
- Achieved via `FileChannel.map()` returning `MappedByteBuffer`.
- Useful for large files and high-performance I/O, as data is loaded lazily and can be shared between processes.
- Changes to the buffer are eventually written to disk; can be forced with `force()`.
- Potential issues: file locking, size limitations, and platform-specific behavior.

## Multithreading & Concurrency

### Thread lifecycle
- Thread states: NEW (created but not started), RUNNABLE (executing or ready to run), BLOCKED (waiting for monitor lock), WAITING (waiting indefinitely), TIMED_WAITING (waiting for specified time), TERMINATED (completed).
- Transitions: `start()` moves from NEW to RUNNABLE; scheduler decides running.
- `wait()`, `notify()`, `join()`, `sleep()` cause state changes.
- `interrupt()` can wake a thread from waiting/sleeping, setting interrupt flag.
- Understanding lifecycle helps in debugging concurrency issues.

### Runnable vs Callable
- `Runnable` is a functional interface with `run()` method (no return value, cannot throw checked exceptions).
- `Callable<V>` is a functional interface with `call()` method (returns a value, can throw checked exceptions).
- `Callable` is used with `ExecutorService.submit()` returning `Future`.
- `Runnable` tasks can be adapted to `Callable` using `Executors.callable()`.
- Prefer `Callable` when result or exception handling is needed.

### Thread creation
- Two ways: extend `Thread` class and override `run()`; or implement `Runnable` and pass to `Thread` constructor.
- Implementing `Runnable` is preferred because it separates task from execution and allows extending other classes.
- Starting a thread: call `start()` (not `run()`, which executes in current thread).
- Threads can also be created via `ExecutorService` for better management.
- Daemon threads (setDaemon(true)) run in background and terminate when all user threads finish.

### Synchronization
- Synchronization controls access to shared resources by multiple threads to prevent data inconsistency.
- Achieved using `synchronized` keyword on methods or blocks, using an intrinsic lock (monitor).
- Synchronized methods lock on the instance (for instance methods) or class (for static methods).
- Synchronized blocks allow locking on any object, providing finer control.
- Synchronization ensures visibility and atomicity but can cause contention and deadlocks.

### Intrinsic locks
- Every Java object has an intrinsic lock (monitor) associated with it.
- When a thread enters a synchronized method/block, it acquires the lock; other threads block until release.
- Locks are reentrant: a thread holding a lock can acquire it again without deadlock.
- Intrinsic locks are simple but lack flexibility (no timeout, interruptible acquisition).
- For advanced locking, use `java.util.concurrent.locks` package.

### Deadlocks
- Deadlock occurs when two or more threads are blocked forever, each waiting for a lock held by another.
- Necessary conditions: mutual exclusion, hold and wait, no preemption, circular wait.
- Prevention strategies: lock ordering, timeout, using tryLock, avoiding nested locks, using higher-level concurrency utilities.
- Detection: tools like jstack, thread dumps, visual VM.
- Deadlocks are hard to debug; design to avoid them.

### Race conditions
- Race condition occurs when the outcome of a program depends on the timing of thread execution.
- Common in read-modify-write operations (e.g., counter increment) without synchronization.
- Can lead to data corruption and unpredictable behavior.
- Prevention: use synchronization, atomic variables, or immutable data.
- Even with synchronization, race conditions can exist if not all accesses are protected.

### Volatile keyword
- `volatile` ensures that reads and writes to a variable are directly from/to main memory, not thread-local caches.
- It provides visibility guarantee: changes by one thread are visible to others immediately.
- Does not provide atomicity (e.g., increment operation still needs synchronization).
- Useful for flags, state variables, and double-checked locking (with proper semantics).
- Since Java 5, volatile has acquire/release semantics.

### Thread safety
- A class is thread-safe if it behaves correctly when accessed from multiple threads.
- Achieved through immutability, synchronization, or using thread-safe classes (e.g., `ConcurrentHashMap`).
- Stateless objects are inherently thread-safe.
- Thread-safe classes can be composed with care; need to ensure atomicity of compound actions.
- Document thread-safety guarantees.

### Executor framework
- The `Executor` framework decouples task submission from execution.
- `Executor` interface has `execute(Runnable)`. `ExecutorService` extends with `submit()`, `shutdown()`, etc.
- Provides thread pool management, task scheduling, and future results.
- Common implementations: `ThreadPoolExecutor`, `ScheduledThreadPoolExecutor`, `ForkJoinPool`.
- Simplifies concurrent programming and resource management.

### Thread pools
- Thread pools manage a pool of worker threads, reusing them for multiple tasks to reduce overhead.
- Fixed thread pool: `Executors.newFixedThreadPool(n)`.
- Cached thread pool: `Executors.newCachedThreadPool()` creates threads as needed, reuses idle ones.
- Single thread executor: `Executors.newSingleThreadExecutor()` ensures sequential execution.
- Scheduled thread pool: `Executors.newScheduledThreadPool()` for delayed/periodic tasks.
- Proper sizing: consider CPU cores, I/O vs CPU-bound tasks.

### Future and CompletableFuture
- `Future` represents the result of an asynchronous computation, with methods to check completion, wait, and retrieve result.
- Limitations: blocking `get()`, no chaining, no manual completion.
- `CompletableFuture` (Java 8) implements `Future` and `CompletionStage`, providing non-blocking operations, chaining, and combining.
- Supports callbacks (`thenApply`, `thenAccept`, `thenRun`), combining (`thenCombine`), error handling (`exceptionally`), and composing.
- Used for asynchronous programming and parallel tasks.

### Locks and ReentrantLock
- `Lock` interface in `java.util.concurrent.locks` provides more flexible locking than intrinsic locks.
- `ReentrantLock` is a reentrant mutual exclusion Lock with features: tryLock (with timeout), lockInterruptibly, fairness policy.
- Requires explicit lock/unlock, usually in try-finally block.
- Condition objects (from `newCondition()`) allow wait/notify-like behavior with multiple wait sets.
- Prefer `ReentrantLock` when advanced features needed; otherwise, synchronized is simpler.

### ReadWriteLock
- `ReadWriteLock` maintains a pair of locks: one for read-only operations, one for writes.
- Multiple threads can hold read lock concurrently; write lock is exclusive.
- Useful for data structures with frequent reads and occasional writes, improving concurrency.
- `ReentrantReadWriteLock` is the primary implementation.
- Beware of lock downgrading (from write to read) and potential starvation.

### Concurrent collections
- Already covered earlier but important for concurrency: `ConcurrentHashMap`, `CopyOnWriteArrayList`, `ConcurrentLinkedQueue`, `BlockingQueue` implementations.
- They provide thread-safe operations without explicit synchronization.
- `ConcurrentHashMap` uses lock striping and volatile semantics.
- `CopyOnWriteArrayList` creates a new copy on modification, suitable for read-heavy scenarios.
- `BlockingQueue` supports producer-consumer patterns with blocking put/take.

### Atomic variables
- `java.util.concurrent.atomic` package provides classes like `AtomicInteger`, `AtomicLong`, `AtomicReference` for lock-free thread-safe operations on single variables.
- They use CAS (Compare-And-Swap) instructions at hardware level for high performance.
- Operations like `incrementAndGet()`, `compareAndSet()`, `updateAndGet()`.
- Avoids synchronization overhead and provides better scalability.
- Useful for counters, accumulators, and non-blocking algorithms.

### Fork/Join framework
- Fork/Join framework (Java 7) is designed for parallel processing of tasks that can be recursively split.
- Core classes: `ForkJoinPool`, `ForkJoinTask`, `RecursiveTask` (with result), `RecursiveAction` (no result).
- Uses work-stealing algorithm: idle threads steal tasks from busy threads' queues to improve load balancing.
- Ideal for divide-and-conquer problems like parallel sorting, matrix multiplication.
- Parallel streams (Java 8) use Fork/Join under the hood.

### Parallel streams
- Parallel streams enable concurrent processing of stream elements using `parallel()` or `parallelStream()`.
- They leverage the common ForkJoinPool to execute operations in parallel.
- Useful for CPU-intensive tasks with large data sets; but overhead may outweigh benefits for small data or I/O-bound tasks.
- Must ensure that operations are stateless, non-interfering, and associative to avoid concurrency issues.
- Parallel streams can cause performance degradation if not used carefully; monitor with benchmarks.

### Producer-consumer pattern
- Classic concurrency pattern where producers generate data and consumers process it, often with a shared buffer.
- Implemented using `BlockingQueue` (e.g., `ArrayBlockingQueue`) which handles synchronization automatically.
- Producer calls `put()` (blocks if queue full); consumer calls `take()` (blocks if empty).
- Alternative: use `wait()` and `notifyAll()` with explicit synchronization, but `BlockingQueue` simplifies.
- Important to handle interruption and shutdown properly.

## Java Memory Model & JVM Internals

### JVM architecture
- JVM consists of class loader subsystem, runtime data areas (heap, stack, PC registers, method area, native method stack), execution engine, and native interface.
- Runtime data areas: heap (shared), method area (shared), Java stacks (per thread), PC registers (per thread), native method stacks (per thread).
- Execution engine includes interpreter, JIT compiler, and garbage collector.
- JVM specifications are implemented by various vendors (HotSpot, OpenJ9, etc.).
- Understanding architecture aids in performance tuning and troubleshooting.

### Heap vs stack
- Heap is the runtime data area where objects and arrays are allocated; shared among all threads.
- Stack is per-thread, storing frames (local variables, partial results, method calls). Each frame has operand stack and local variable array.
- Stack memory is faster, but limited; heap is larger but slower and managed by GC.
- Stack stores primitive local variables and object references; objects themselves are in heap.
- StackOverflowError occurs when stack depth exceeded; OutOfMemoryError when heap full.

### Method area
- Method area (part of heap in HotSpot) stores class-level data: runtime constant pool, field and method data, constructor and method code, special methods.
- It is shared among all threads.
- Before Java 8, method area was part of PermGen (permanent generation); now replaced by Metaspace.
- Contains static variables, which are references to objects (objects themselves on heap).

### Metaspace
- Metaspace (introduced in Java 8) replaces PermGen for storing class metadata.
- Unlike PermGen, Metaspace is not contiguous to heap; it uses native memory, sized by operating system's available memory.
- This change reduces the risk of `OutOfMemoryError: PermGen space`.
- Metaspace size can be controlled with `-XX:MaxMetaspaceSize`.
- Class metadata is garbage collected when classes are unloaded.

### Class loaders
- Class loaders are responsible for loading class files into memory.
- Three built-in class loaders: Bootstrap (loads core Java classes), Platform/Extension (loads extensions), Application/System (loads from classpath).
- Delegation model: a class loader delegates loading to parent before trying itself.
- Custom class loaders can be created for dynamic loading (e.g., web servers).
- Class loading is dynamic and lazy (classes loaded when first referenced).

### Garbage collection basics
- Garbage collection automatically reclaims memory occupied by unreachable objects.
- Objects become unreachable when no references point to them (from roots: stack variables, static fields, JNI references).
- GC roots include active threads, static fields, local variables, JNI references.
- GC algorithms involve marking reachable objects, and sweeping/compacting unreachable ones.
- Tuning GC is critical for application performance.

### GC algorithms
- Common algorithms: Serial (single-threaded, stop-the-world), Parallel (multithreaded), CMS (concurrent mark-sweep, deprecated), G1 (garbage-first), ZGC (low-latency), Shenandoah.
- Serial GC suitable for small apps; Parallel for throughput; G1 for balanced; ZGC/Shenandoah for low latency.
- Each has trade-offs in throughput, latency, and footprint.
- GC logs and monitoring tools help choose appropriate algorithm.

### G1, ZGC, Shenandoah
- G1 (Garbage First) is the default since Java 9, divides heap into regions, prioritizes garbage collection in regions with most garbage, aims for low pause times.
- ZGC (Java 11+) is a scalable low-latency collector, handles heaps up to multi-terabytes, pause times <10ms, uses colored pointers and load barriers.
- Shenandoah (Java 12+, not in all builds) also low-latency, concurrent evacuation, reduces pause times.
- These collectors are designed for applications requiring predictable and low pause times.

### Memory leaks
- In Java, memory leaks occur when objects are no longer needed but still referenced, preventing GC.
- Common causes: static collections accumulating objects, unclosed resources, listeners not deregistered, ThreadLocal misuse, cache without weak references.
- Memory leaks degrade performance and lead to OutOfMemoryError.
- Detection: profiling tools (VisualVM, JProfiler), heap dumps, GC logs.
- Prevention: use weak references where appropriate, clear collections, close resources, etc.

### Object allocation
- Objects are typically allocated on heap (eden space) using TLAB (Thread Local Allocation Buffer) for performance.
- Escape analysis (since Java 6) can determine if an object is not shared, allowing allocation on stack or even scalar replacement (object fields as local variables), reducing GC pressure.
- Allocation can also happen in old generation if object is large (direct to old).
- Object allocation is fast due to bump-pointer allocation and TLABs.

### Escape analysis
- Escape analysis is an optimization technique that analyzes object scope.
- If an object does not escape method or thread, JIT can allocate it on stack instead of heap, or even eliminate it (scalar replacement).
- This reduces GC overhead and improves performance.
- Enabled by default in modern JVMs; controlled with `-XX:+DoEscapeAnalysis`.
- It's a compiler optimization, not visible at source level.

### JIT compilation
- Just-In-Time (JIT) compiler compiles bytecode to native machine code at runtime for frequently executed code (hot spots).
- Two main compilers in HotSpot: C1 (client, quick) and C2 (server, more optimized); tiered compilation uses both.
- JIT improves performance significantly by adaptive optimization.
- It profiles code to identify bottlenecks and applies inlining, loop unrolling, etc.
- JIT-compiled code is stored in code cache.

### Bytecode execution
- Bytecode can be executed by interpreter (slow) or JIT-compiled (fast).
- Initially, interpreter runs, collecting profiling data; hot methods are JIT-compiled.
- Stack-based architecture: operations use operand stack.
- Bytecode instructions are simple (e.g., iload, iadd, invokevirtual).
- Execution engine ensures type safety and stack integrity.

### JVM tuning basics
- JVM tuning involves adjusting heap size, GC algorithm, and other parameters for optimal performance.
- Common options: `-Xms` (initial heap), `-Xmx` (max heap), `-XX:+UseG1GC`, `-XX:MaxGCPauseMillis`.
- Monitor GC logs, heap usage, thread dumps to identify issues.
- Tuning aims to balance throughput, latency, and memory footprint.
- Over-tuning can be counterproductive; start with defaults and adjust based on profiling.

## Functional Programming & Streams

### Lambda expressions
- Lambda expressions provide a concise way to represent anonymous functions, enabling functional programming in Java.
- Syntax: `(parameters) -> expression` or `(parameters) -> { statements; }`.
- They can be used where a functional interface is expected.
- Lambdas capture effectively final variables from enclosing scope.
- Improve code readability and enable behavior parameterization.

### Functional interfaces
- A functional interface has exactly one abstract method (SAM), may have default and static methods.
- Annotated with `@FunctionalInterface` for compile-time check.
- Common functional interfaces: `Predicate<T>`, `Function<T,R>`, `Consumer<T>`, `Supplier<T>`, `Comparator<T>`, `Runnable`, `Callable`.
- Java 8 provides many in `java.util.function` package.
- Lambdas can be assigned to functional interface types.

### Method references
- Method references are shorthand for lambdas calling a specific method.
- Four types: static method (`Class::staticMethod`), instance method of a particular object (`instance::method`), instance method of any object of a particular type (`Class::instanceMethod`), constructor (`Class::new`).
- They are more concise and often more readable.
- Can be used where lambda would just call an existing method.

### Stream API
- Stream API (java.util.stream) enables functional-style operations on sequences of elements.
- Streams are not data structures; they pull data from sources (collections, arrays, I/O channels).
- Operations are divided into intermediate (lazy, return new stream) and terminal (eager, produce result).
- Streams can be sequential or parallel.
- Designed for internal iteration, allowing optimizations like lazy evaluation and short-circuiting.

### Intermediate operations
- Intermediate operations transform a stream into another stream, e.g., `filter()`, `map()`, `flatMap()`, `distinct()`, `sorted()`, `peek()`, `limit()`, `skip()`.
- They are lazy; execution only occurs when a terminal operation is invoked.
- `map()` applies a function to each element; `flatMap()` flattens nested structures.
- `filter()` selects elements based on predicate.
- Intermediate operations can be chained.

### Terminal operations
- Terminal operations produce a result or side effect, e.g., `forEach()`, `collect()`, `toArray()`, `reduce()`, `count()`, `anyMatch()`, `allMatch()`, `noneMatch()`, `findFirst()`, `findAny()`.
- After terminal operation, the stream is consumed and cannot be reused.
- `reduce()` performs associative aggregation.
- `collect()` accumulates elements into a collection or other result using `Collectors`.

### Collectors
- `Collectors` utility class provides implementations for `collect()`.
- Common collectors: `toList()`, `toSet()`, `toMap()`, `joining()`, `groupingBy()`, `partitioningBy()`, `summarizingInt()`, `averagingInt()`.
- Custom collectors can be created using `Collector.of()`.
- `groupingBy()` classifies elements by a classifier function.
- `partitioningBy()` splits into two groups based on predicate.

### Parallel streams
- Parallel streams use common ForkJoinPool to process elements concurrently.
- Enable via `parallelStream()` or `stream().parallel()`.
- Suitable for CPU-intensive tasks with large data; but overhead of splitting and combining can degrade performance for small data or non-independent operations.
- Operations must be stateless, non-interfering, and associative.
- Use `forEachOrdered()` to preserve encounter order when needed.

### Stream performance
- Streams can have overhead due to lambda creation, boxing, and pipeline setup.
- For simple operations on small data, traditional loops may be faster.
- Parallel streams can speed up large data processing if operations are CPU-bound and data can be split evenly.
- Use primitive streams (`IntStream`, `LongStream`, `DoubleStream`) to avoid boxing overhead.
- Profiling and benchmarking (JMH) recommended to evaluate performance.

### Optional usage
- `Optional` is used to represent potentially absent values in stream operations.
- Terminal operations like `findFirst()` return `Optional`.
- Use `ifPresent()`, `orElse()`, `orElseGet()` to handle optional results gracefully.
- Avoid using `Optional` as method parameters or fields; it's designed for return types.
- In streams, `Optional` can be chained with methods like `map()` and `filter()`.

## Reflection & Class Loading

### Reflection API
- Reflection allows examining or modifying runtime behavior of classes, interfaces, fields, and methods.
- Key classes: `Class`, `Field`, `Method`, `Constructor`, `Modifier`, `Array`.
- Can access private members (via `setAccessible(true)`), but security manager may restrict.
- Used in frameworks (Spring, Hibernate) for dependency injection, ORM, and serialization.
- Performance overhead due to runtime type checking; avoid in performance-critical code.

### Dynamic class loading
- Dynamic class loading loads classes at runtime using `Class.forName()` or class loaders.
- `Class.forName(String className)` loads class and returns `Class` object, initializing it.
- Can also load with custom class loader.
- Used in plugin architectures, JDBC drivers (`Class.forName("com.mysql.jdbc.Driver")`).
- Enables runtime flexibility but adds complexity.

### ClassLoader hierarchy
- As mentioned earlier, class loaders follow delegation model.
- Understanding custom class loaders is essential for application servers and modular applications.
- `Thread.currentThread().getContextClassLoader()` often used to load classes in web apps.
- Class loaders can define packages and resolve classes from different sources.
- ClassLoader leaks can occur if classes are not unloaded, often in PermGen/Metaspace.

### Annotations processing
- Annotation processing can occur at compile-time (via annotation processors) or runtime (via reflection).
- Compile-time processing: apt tool (old) or `javac` with processors, generating source files, etc.
- Runtime processing: reflection to read annotations and take actions.
- Popular libraries use runtime processing (e.g., JUnit, Spring) or compile-time (Lombok).
- `@Retention` policy determines if annotation is available at runtime.

### Runtime introspection
- Introspection is the ability to examine object properties and methods at runtime.
- Reflection provides introspection capabilities, e.g., `getFields()`, `getMethods()`, `getDeclaredAnnotations()`.
- JavaBeans introspection uses `Introspector` to discover properties.
- Used in frameworks for dynamic behavior.
- Introspection can be slow; caching results can mitigate.

### Dynamic proxies
- Dynamic proxies allow creation of proxy instances for interfaces at runtime.
- `java.lang.reflect.Proxy` creates proxy classes that implement specified interfaces.
- Method calls are dispatched to an `InvocationHandler`.
- Used in AOP frameworks (Spring AOP) to add cross-cutting concerns.
- For classes (not interfaces), use bytecode manipulation libraries like CGLIB or ByteBuddy.

## Java Networking

### Sockets
- Sockets are endpoints for communication between two machines over a network.
- Java provides `Socket` (client) and `ServerSocket` (server) for TCP communication.
- `DatagramSocket` for UDP.
- Sockets allow reading/writing data via streams.
- Example: client connects to server IP and port, exchanges data.

### TCP vs UDP
- TCP (Transmission Control Protocol) is connection-oriented, reliable, ordered, and error-checked; used for HTTP, email, FTP.
- UDP (User Datagram Protocol) is connectionless, unreliable, but faster; used for streaming, DNS, gaming.
- TCP ensures data integrity but has overhead; UDP is lightweight but can lose packets.
- Java supports both via `Socket`/`ServerSocket` (TCP) and `DatagramSocket` (UDP).
- Choice depends on application requirements.

### HTTP basics
- HTTP (Hypertext Transfer Protocol) is the foundation of web communication.
- Request methods: GET, POST, PUT, DELETE, etc.
- Status codes: 200 OK, 404 Not Found, 500 Internal Server Error.
- Headers provide metadata (content type, caching, authorization).
- Java provides `HttpURLConnection` (since 1.1) and newer `HttpClient` (since Java 11).

### URL connections
- `URL` class represents a Uniform Resource Locator.
- `URLConnection` (and `HttpURLConnection`) allows reading from and writing to a URL.
- Can set request properties, handle redirects, timeouts.
- Since Java 11, `HttpClient` provides a more modern API for HTTP/1.1 and HTTP/2, with asynchronous support.
- `HttpClient` supports synchronous and asynchronous requests, WebSocket.

### REST clients
- REST (Representational State Transfer) is an architectural style for web services.
- Java REST clients: `HttpClient`, Jersey Client, Apache HttpClient, Spring RestTemplate (now WebClient).
- `HttpClient` (Java 11) is recommended for new projects.
- REST clients interact with REST APIs using HTTP methods, JSON/XML payloads.
- Serialization often handled by libraries like Jackson or Gson.

## Advanced Java Language Features

### Inner classes
- Inner classes are non-static nested classes defined within another class.
- They have access to all members (including private) of the outer class.
- Each inner class instance is associated with an outer class instance.
- Used for logically grouping classes that are used in only one place, increasing encapsulation.
- Types: member inner class, local inner class (inside method), anonymous inner class.

### Anonymous classes
- Anonymous classes are inner classes without a name, declared and instantiated in a single expression.
- They are useful for implementing interfaces or extending classes on the spot (e.g., event handlers).
- Syntax: `new Interface() { ... }` or `new Class() { ... }`.
- Can capture effectively final variables from enclosing scope.
- Since Java 8, often replaced by lambda expressions for functional interfaces.

### Nested classes
- Nested classes are classes defined within another class, either static (static nested) or non-static (inner).
- Static nested classes behave like top-level classes but are nested for packaging convenience.
- They cannot access instance members of outer class directly (only via object reference).
- Static nested classes are often used as helpers or builders.

### Var keyword (local variable type inference)
- `var` (Java 10+) allows local variable type inference; the compiler infers type from initializer.
- Can be used in local variables, for-loop indices, and try-with-resources (Java 11).
- Not allowed for fields, method parameters, or return types.
- Improves readability by reducing boilerplate, but overuse can obscure type.
- Type is inferred at compile time, not runtime; `var` is not a dynamic type.

### Switch expressions (Java 12+)
- Switch expressions extend switch statement to return a value and have new syntax.
- Arrow syntax: `case X -> result;` (no fall-through).
- Use `yield` to return value from a block.
- Switch expressions are exhaustive (cover all cases) for enums and sealed types.
- They improve code conciseness and reduce bugs from missing break.

### Pattern matching (Java 14+ preview, 16+ standard)
- Pattern matching for `instanceof` allows variable declaration and casting in one step.
- Example: `if (obj instanceof String s) { System.out.println(s.length()); }`
- Pattern matching for switch (preview in later versions) allows patterns in case labels.
- Reduces boilerplate and improves safety.
- Evolving feature with more patterns (deconstruction) in future.

### Sealed classes (Java 15+ preview, 17+ standard)
- Sealed classes restrict which classes or interfaces may extend/implement them.
- Declared with `sealed` modifier, then `permits` clause listing permitted subclasses.
- Permitted subclasses must be `final`, `sealed`, or `non-sealed`.
- Enable exhaustive pattern matching and better domain modeling.
- Enhance encapsulation by controlling inheritance.

### Modules (Java 9+)
- Java Platform Module System (JPMS) introduces modules for better encapsulation and reliable configuration.
- Module descriptor `module-info.java` specifies module name, exports packages, requires other modules.
- Modules help in creating maintainable and scalable applications.
- Strong encapsulation: public types are not accessible unless package is exported.
- Enables reliable configuration and reduced footprint.

## JDBC

### JDBC architecture
- JDBC (Java Database Connectivity) is an API for connecting and executing queries on databases.
- Consists of interfaces (Driver, Connection, Statement, ResultSet) implemented by database-specific drivers.
- Two-tier architecture: Java app directly talks to DB via driver.
- Three-tier: app server in between (e.g., using connection pooling).
- JDBC provides both high-level and low-level access.

### Drivers
- JDBC drivers are implementations of JDBC interfaces for specific databases.
- Four types: Type 1 (JDBC-ODBC bridge, deprecated), Type 2 (native API, partially Java), Type 3 (network protocol), Type 4 (pure Java, thin driver).
- Type 4 drivers are most common (e.g., MySQL Connector/J, PostgreSQL JDBC).
- DriverManager loads drivers and manages connections.
- Also DataSource for connection pooling.

### Connections
- Connection represents a session with the database; obtained via `DriverManager.getConnection()` or `DataSource`.
- Connection properties: URL, username, password.
- Connection can set auto-commit mode, transaction isolation, etc.
- Must be closed after use (try-with-resources).
- Connection pooling reuses connections to improve performance.

### Statements
- Statement: used to execute static SQL queries (no parameters). Vulnerable to SQL injection.
- PreparedStatement: precompiled SQL with placeholders (parameters), preventing SQL injection and improving performance for repeated queries.
- CallableStatement: for stored procedures.
- Execute methods: `executeQuery()` for SELECT, `executeUpdate()` for INSERT/UPDATE/DELETE, `execute()` for any.

### Prepared statements
- PreparedStatement extends Statement, allows parameterized SQL with `?` placeholders.
- Parameters set using setXxx methods (e.g., `setString(1, value)`).
- Benefits: performance (precompiled), security (escapes input), readability.
- Important for preventing SQL injection attacks.
- Can be reused with different parameter values.

### ResultSet handling
- ResultSet represents the result of a query; cursor initially before first row.
- `next()` advances cursor, returns false when no more rows.
- Retrieve column values with getXxx methods (by index or column name).
- Types: forward-only, scrollable, updatable; concurrency: read-only, updatable.
- Must close ResultSet when done (usually automatically when Statement closed).

### Transactions
- Transactions ensure ACID properties (Atomicity, Consistency, Isolation, Durability).
- By default, each SQL statement is auto-committed.
- To group multiple statements, disable auto-commit: `connection.setAutoCommit(false)`.
- Commit: `connection.commit()`, rollback: `connection.rollback()`.
- Savepoints allow partial rollback.

### Batch processing
- Batch processing sends multiple SQL statements in one database round-trip for performance.
- Statement: `addBatch()`, `executeBatch()`.
- PreparedStatement can batch multiple parameter sets.
- Returns array of update counts.
- Useful for bulk inserts/updates.

### Connection pooling
- Connection pooling manages a pool of database connections for reuse, reducing overhead of creating connections.
- Implemented by DataSource (e.g., HikariCP, Apache DBCP, c3p0).
- Application borrows connection from pool, returns it after use.
- Improves scalability and response time.
- Configure pool size based on workload.

## Performance & Optimization

### Profiling Java apps
- Profiling tools (JProfiler, VisualVM, YourKit, Java Flight Recorder) help identify performance bottlenecks.
- CPU profiling shows hot methods; memory profiling reveals leaks and allocation patterns.
- Thread profiling detects contention and deadlocks.
- Use profiling during development and under realistic load.
- Focus on optimizing hot spots, not premature optimization.

### Memory tuning
- Tune heap sizes: `-Xms`, `-Xmx` to avoid frequent GC and OutOfMemoryError.
- Adjust young generation size (`-XX:NewRatio`, `-Xmn`) based on application object allocation patterns.
- Set appropriate Metaspace size.
- Monitor heap usage with tools; look for memory leaks.
- Use `-XX:+HeapDumpOnOutOfMemoryError` to capture dumps.

### GC tuning
- Goal: minimize GC overhead while meeting pause time requirements.
- Choose GC algorithm based on application needs: G1 for balanced, Parallel for throughput, ZGC for low-latency.
- Tune parameters like `-XX:MaxGCPauseMillis` for G1, `-XX:G1HeapRegionSize`.
- Monitor GC logs with tools (GCViewer, gceasy) to understand behavior.
- Aim for high throughput and low pauses.

### Thread tuning
- Thread pool sizing: number of threads depends on CPU cores and I/O vs CPU-bound tasks.
- For CPU-bound, pool size around number of cores; for I/O-bound, can be larger.
- Use `ThreadPoolExecutor` with bounded queues to prevent resource exhaustion.
- Monitor thread usage, stack traces, and deadlocks.
- Avoid excessive context switching.

### CPU optimization
- Identify CPU-intensive operations: algorithms, loops, object creation.
- Optimize code: use efficient data structures, avoid unnecessary boxing, reduce method calls.
- Use parallel streams or Fork/Join for parallelizable CPU tasks.
- JIT optimizations like inlining can help, but code clarity matters.
- Profiling guides where to focus.

### I/O optimization
- I/O operations (disk, network) are often bottlenecks.
- Use buffering (BufferedInputStream, BufferedWriter) to reduce system calls.
- For file I/O, consider memory-mapped files or NIO for large files.
- For network, use non-blocking I/O (NIO) or asynchronous I/O.
- Connection pooling for databases and HTTP clients.

## Security

### Java security model
- Java provides a comprehensive security model: bytecode verification, class loaders, security manager, access control, cryptography.
- Sandboxing: untrusted code runs in restricted environment.
- Security manager enforces permissions (file, network, reflection) based on policy.
- Since Java 17, Security Manager is deprecated for removal, but still used in legacy apps.
- Modern security relies on module system and encapsulation.

### Cryptography basics
- Java Cryptography Architecture (JCA) provides APIs for encryption, decryption, hashing, digital signatures.
- Key classes: `Cipher`, `KeyGenerator`, `SecretKey`, `MessageDigest`, `Signature`.
- Supports algorithms: AES, RSA, SHA, HMAC.
- Use `SecureRandom` for cryptographically strong random numbers.
- Java Cryptography Extension (JCE) provides unlimited strength jurisdiction policy files (now bundled).

### SSL/TLS
- SSL/TLS provides secure communication over networks.
- Java supports SSL/TLS via `javax.net.ssl` package: `SSLSocket`, `SSLServerSocket`, `HttpsURLConnection`.
- Key management: keystores (private keys) and truststores (trusted certificates).
- `SSLContext` can be configured with specific protocols and cipher suites.
- In Java, default TLS version can be set; ensure weak protocols disabled.

### Secure coding practices
- Validate input to prevent injection attacks (SQL, command, XSS).
- Use prepared statements for SQL queries.
- Avoid exposing sensitive data (passwords, tokens) in logs or exceptions.
- Use proper access control: least privilege principle.
- Keep libraries updated to avoid known vulnerabilities.
- Use static analysis tools (FindBugs, SonarQube) to detect security flaws.

### Authentication & authorization
- Authentication verifies identity (who you are); authorization determines access rights (what you can do).
- Java EE/Spring Security provides frameworks for authentication/authorization.
- JAAS (Java Authentication and Authorization Service) is a pluggable authentication module.
- Use secure password storage (hashing with salt, bcrypt).
- Implement role-based access control (RBAC) or OAuth2 for APIs.
