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
- `wait()`, `notify()`, `join()`,
