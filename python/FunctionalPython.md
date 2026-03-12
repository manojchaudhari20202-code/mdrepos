### Function definitions
Functions are defined using the `def` keyword, followed by a function name, parentheses for parameters, and a colon. The body is indented.

```python
def greet(name):
    return f"Hello, {name}!"
print(greet("Alice"))  # Output: Hello, Alice!
```

### Default arguments
Parameters can have default values. If an argument is not provided, the default is used.

```python
def power(base, exponent=2):
    return base ** exponent
print(power(3))    # Output: 9 (3^2)
print(power(3, 3)) # Output: 27 (3^3)
```

### Keyword arguments
Arguments can be passed by explicitly naming the parameter, allowing order to be changed.

```python
def describe_pet(animal, name):
    return f"{name} is a {animal}."
print(describe_pet(name="Buddy", animal="dog"))  # Output: Buddy is a dog.
```

### *args and **kwargs
`*args` collects extra positional arguments into a tuple, `**kwargs` collects extra keyword arguments into a dictionary.

```python
def show_args(*args, **kwargs):
    print("Positional:", args)
    print("Keyword:", kwargs)
show_args(1, 2, 3, name="Alice", age=30)
# Output:
# Positional: (1, 2, 3)
# Keyword: {'name': 'Alice', 'age': 30}
```

### Lambda functions
Small anonymous functions defined with `lambda`. They can have any number of arguments but only one expression.

```python
square = lambda x: x ** 2
print(square(5))  # Output: 25
```

### Closures
A closure is a function that remembers variables from the enclosing scope even after that scope has finished executing.

```python
def outer(x):
    def inner(y):
        return x + y
    return inner
add_five = outer(5)
print(add_five(3))  # Output: 8
```

### First-class functions
Functions in Python are first-class objects: they can be assigned to variables, passed as arguments, and returned from other functions.

```python
def say_hello():
    return "Hello"
greet = say_hello   # Assign function to variable
print(greet())      # Output: Hello

def call_func(func):
    return func()
print(call_func(say_hello))  # Output: Hello
```

### Recursion
A function calling itself to solve smaller instances of a problem.

```python
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)
print(factorial(5))  # Output: 120
```

### Higher-order functions
Functions that take other functions as arguments or return them.

```python
def apply_twice(func, value):
    return func(func(value))
result = apply_twice(lambda x: x * 2, 3)
print(result)  # Output: 12 (3*2=6, then 6*2=12)
```

### Decorators
Decorators are functions that modify the behavior of another function. They use the `@decorator` syntax.

```python
def timer(func):
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        print(f"Time: {time.time() - start:.2f}s")
        return result
    return wrapper

@timer
def slow_function():
    import time
    time.sleep(1)
    return "Done"
print(slow_function())  # Prints time and "Done"
```

### Function annotations
Annotations provide metadata about parameter types and return values. They are not enforced but can be used by tools.

```python
def greet(name: str, age: int) -> str:
    return f"{name} is {age} years old."
print(greet.__annotations__)  # Output: {'name': <class 'str'>, 'age': <class 'int'>, 'return': <class 'str'>}
```

### Partial functions
Partial functions fix a certain number of arguments of a function, producing a new callable. Use `functools.partial`.

```python
from functools import partial

def multiply(x, y):
    return x * y
double = partial(multiply, 2)
print(double(5))  # Output: 10
```

### Currying
Currying transforms a function that takes multiple arguments into a chain of functions each taking a single argument. Python doesn’t have built-in currying, but you can implement it or use `functools.partial`.

```python
# Manual currying example
def curry_multiply(x):
    def inner(y):
        return x * y
    return inner

curried_double = curry_multiply(2)
print(curried_double(5))  # Output: 10
```