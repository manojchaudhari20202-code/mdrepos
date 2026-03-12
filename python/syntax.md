# Python Syntax & Features — Complete Reference

## 1. Core Language Features

### Variables & Data Types
```python
# Dynamic typing
x = 10          # int
y = 3.14        # float
z = 2+3j        # complex
s = "hello"     # str
b = True        # bool
n = None        # NoneType

# Type hints (PEP 484)
name: str = "Alice"
age: int = 30
```

### Operators
```python
# Arithmetic
+  -  *  /  //  %  **

# Comparison
==  !=  >  <  >=  <=

# Logical
and  or  not

# Bitwise
&  |  ^  ~  <<  >>

# Assignment
=  +=  -=  *=  /=  //=  %=  **=  &=  |=  ^=  <<=  >>=

# Identity & Membership
is  is not  in  not in

# Walrus (PEP 572)
if (n := len(data)) > 10:
    print(n)
```

### Strings
```python
s = "hello"
s = 'hello'
s = """multiline
string"""
s = r"\n raw string"
s = b"bytes"
s = f"Hello {name}"          # f-string
s = f"{value:.2f}"           # f-string formatting
s = f"{obj!r}"               # repr in f-string

# Methods
s.upper() / s.lower()
s.strip() / s.lstrip() / s.rstrip()
s.split(",") / s.join(lst)
s.replace("a", "b")
s.startswith("hi") / s.endswith("!")
s.find("x") / s.index("x")
s.format(arg)
s.encode("utf-8")
s.zfill(5) / s.center(10)
```

### Control Flow
```python
# if / elif / else
if x > 0:
    pass
elif x == 0:
    pass
else:
    pass

# Ternary
result = "yes" if condition else "no"

# match / case (Python 3.10+)
match command:
    case "quit":
        quit()
    case "go" | "move":
        go()
    case {"action": action}:       # dict pattern
        handle(action)
    case Point(x, y):              # class pattern
        draw(x, y)
    case [x, *rest]:               # sequence pattern
        process(x, rest)
    case _:
        default()
```

### Loops
```python
# for
for i in range(10):
    continue / break / pass

# for-else
for item in lst:
    if found: break
else:
    print("not found")

# while
while condition:
    pass

# while-else
while queue:
    process()
else:
    done()
```

### Functions
```python
# Basic
def func(a, b): return a + b

# Default args
def func(x, y=10): ...

# *args / **kwargs
def func(*args, **kwargs): ...

# Keyword-only
def func(a, *, b): ...          # b must be keyword

# Positional-only (3.8+)
def func(a, b, /, c): ...       # a,b positional only

# Lambda
square = lambda x: x ** 2

# Annotations
def add(a: int, b: int) -> int: ...

# Nested functions
def outer():
    def inner(): ...
    return inner

# Closures
def make_counter():
    count = 0
    def increment():
        nonlocal count
        count += 1
    return increment

# Decorators
@decorator
def func(): ...

@decorator_with_args(param)
def func(): ...

# Generator functions
def gen():
    yield 1
    yield from range(10)

# Async functions
async def fetch():
    await asyncio.sleep(1)
```

### Exception Handling
```python
try:
    risky()
except ValueError as e:
    handle(e)
except (TypeError, KeyError):
    pass
else:
    success()         # runs if no exception
finally:
    cleanup()         # always runs

# Raise
raise ValueError("msg")
raise                 # re-raise

# Exception chaining
raise RuntimeError("msg") from original_exc

# Custom exceptions
class AppError(Exception):
    def __init__(self, msg, code):
        super().__init__(msg)
        self.code = code

# Exception groups (3.11+)
try:
    async with asyncio.TaskGroup() as tg:
        tg.create_task(coro())
except* ValueError as eg:
    for e in eg.exceptions: ...
```

### Context Managers
```python
with open("file.txt") as f:
    data = f.read()

# Multiple
with open("a") as a, open("b") as b: ...

# Custom
class Ctx:
    def __enter__(self): return self
    def __exit__(self, exc_type, exc_val, tb): return False

# @contextmanager
from contextlib import contextmanager

@contextmanager
def managed():
    setup()
    try:
        yield resource
    finally:
        teardown()
```

---

## 2. OOP — Object-Oriented Programming

### Classes
```python
class Animal:
    # Class variable
    kingdom = "Animalia"

    # Constructor
    def __init__(self, name: str):
        self.name = name          # instance variable
        self._protected = True    # convention: protected
        self.__private = True     # name-mangled

    # Instance method
    def speak(self): ...

    # Class method
    @classmethod
    def create(cls, data): return cls(data)

    # Static method
    @staticmethod
    def validate(name): return bool(name)

    # Property
    @property
    def label(self): return self.name.upper()

    @label.setter
    def label(self, value): self.name = value.lower()

    @label.deleter
    def label(self): del self.name
```

### Inheritance
```python
class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed

    def speak(self):               # override
        return "Woof"

# Multiple inheritance
class C(A, B): ...                 # MRO: C → A → B → object

# MRO inspection
C.__mro__
C.mro()
```

### Dunder / Magic Methods
```python
__init__      # constructor
__new__       # object creation
__del__       # destructor
__repr__      # repr(obj)
__str__       # str(obj)
__len__       # len(obj)
__getitem__   # obj[key]
__setitem__   # obj[key] = val
__delitem__   # del obj[key]
__contains__  # in operator
__iter__      # iter(obj)
__next__      # next(obj)
__call__      # obj()
__enter__ / __exit__   # with statement
__add__  __sub__  __mul__  __truediv__   # arithmetic
__eq__  __lt__  __le__  __gt__  __ge__   # comparisons
__hash__      # hash(obj)
__bool__      # bool(obj)
__getattr__   # fallback attribute access
__setattr__   # attribute assignment
__slots__     # restrict attributes, save memory
```

### Dataclasses
```python
from dataclasses import dataclass, field

@dataclass
class Point:
    x: float
    y: float = 0.0
    tags: list = field(default_factory=list)

@dataclass(frozen=True)     # immutable
class Config:
    host: str
    port: int = 8080

@dataclass(order=True)      # enables <, >, etc.
class Version:
    major: int
    minor: int
```

### Abstract Classes
```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self) -> float: ...

    @abstractmethod
    def perimeter(self) -> float: ...
```

### Protocols (Structural Subtyping)
```python
from typing import Protocol

class Drawable(Protocol):
    def draw(self) -> None: ...

def render(obj: Drawable): obj.draw()   # duck typing, no inheritance needed
```

### Enums
```python
from enum import Enum, IntEnum, Flag, auto

class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = auto()

class Permission(Flag):
    READ = auto()
    WRITE = auto()
    EXEC = auto()
    ALL = READ | WRITE | EXEC
```

### Descriptors
```python
class Validator:
    def __set_name__(self, owner, name):
        self.name = name

    def __get__(self, obj, objtype=None):
        return obj.__dict__.get(self.name)

    def __set__(self, obj, value):
        if not isinstance(value, int):
            raise TypeError
        obj.__dict__[self.name] = value

class MyClass:
    x = Validator()
```

---

## 3. Collections

### Built-in Types
```python
# List
lst = [1, 2, 3]
lst.append(x) / lst.extend(it) / lst.insert(i, x)
lst.pop() / lst.remove(x) / lst.clear()
lst.sort(key=fn, reverse=True) / sorted(lst)
lst.reverse() / lst.copy() / lst.count(x) / lst.index(x)

# Tuple (immutable)
t = (1, 2, 3)
t = 1, 2, 3           # packing
a, b, c = t           # unpacking
a, *rest = t          # extended unpacking

# Dict
d = {"key": "value"}
d[key] / d.get(key, default)
d.keys() / d.values() / d.items()
d.update(other) / d.pop(key) / d.setdefault(key, default)
d | other             # merge (3.9+)
d |= other            # update (3.9+)

# Set
s = {1, 2, 3}
s.add(x) / s.remove(x) / s.discard(x)
s | t  # union       s & t  # intersection
s - t  # difference  s ^ t  # symmetric diff
s.issubset(t) / s.issuperset(t)

# frozenset (immutable set)
fs = frozenset({1, 2, 3})
```

### Comprehensions
```python
# List
[x*2 for x in range(10) if x % 2 == 0]

# Dict
{k: v for k, v in pairs}

# Set
{x**2 for x in range(5)}

# Generator expression
(x**2 for x in range(100))

# Nested
[cell for row in matrix for cell in row]
```

### collections Module
```python
from collections import (
    deque,           # O(1) append/pop both ends
    Counter,         # frequency map
    defaultdict,     # default values
    OrderedDict,     # insertion-ordered dict
    namedtuple,      # tuple with named fields
    ChainMap,        # multiple dicts as one view
    UserDict,        # dict subclassing base
    UserList,        # list subclassing base
)

# deque
dq = deque(maxlen=5)
dq.appendleft(x) / dq.popleft()
dq.rotate(n)

# Counter
c = Counter("abracadabra")
c.most_common(3)

# defaultdict
dd = defaultdict(list)
dd["key"].append(1)

# namedtuple
Point = namedtuple("Point", ["x", "y"])
p = Point(1, 2); p.x

# typing.NamedTuple (typed)
class Point(NamedTuple):
    x: float
    y: float
```

### heapq & bisect
```python
import heapq
heapq.heappush(heap, item)
heapq.heappop(heap)
heapq.nlargest(n, iterable)
heapq.nsmallest(n, iterable)

import bisect
bisect.bisect_left(lst, x)     # insertion point
bisect.insort(lst, x)          # insert in sorted order
```

---

## 4. Itertools & Functools

```python
import itertools as it
it.chain(a, b)            # chain iterables
it.chain.from_iterable    # flatten
it.product(a, b)          # cartesian product
it.permutations(a, r)
it.combinations(a, r)
it.combinations_with_replacement(a, r)
it.groupby(data, key)
it.islice(it, stop)
it.cycle(it)
it.repeat(x, n)
it.accumulate(it, func)
it.compress(data, selectors)
it.dropwhile(pred, it)
it.takewhile(pred, it)
it.zip_longest(a, b, fillvalue=None)
it.starmap(func, args)
it.count(start, step)
it.pairwise(it)           # 3.10+

import functools as ft
ft.reduce(func, iterable, initializer)
ft.partial(func, *args)
ft.lru_cache(maxsize=128)
ft.cache                  # unbounded (3.9+)
ft.cached_property        # lazy property
ft.wraps(func)            # preserve metadata in decorators
ft.total_ordering         # auto-generate comparison methods
ft.singledispatch         # function overloading by type
```

---

## 5. Typing Module

```python
from typing import (
    Any, Union, Optional,
    List, Dict, Tuple, Set,
    Callable, Iterator, Generator,
    TypeVar, Generic, ClassVar,
    Literal, Final, TypedDict,
    Annotated, get_type_hints,
    Protocol, runtime_checkable,
    overload, cast, TYPE_CHECKING,
    ParamSpec, Concatenate,        # 3.10+
    TypeAlias,                     # 3.10+
    Self,                          # 3.11+
    Never, LiteralString,          # 3.11+
    TypeVarTuple, Unpack,          # 3.11+
)

T = TypeVar("T")
class Stack(Generic[T]):
    def push(self, item: T) -> None: ...
    def pop(self) -> T: ...

# Union shorthand (3.10+)
def f(x: int | str) -> None: ...

# Optional shorthand
def f(x: int | None) -> None: ...

# TypedDict
class Movie(TypedDict):
    title: str
    year: int
```

---

## 6. Concurrency

### Threading
```python
import threading

# Thread
t = threading.Thread(target=func, args=(x,), daemon=True)
t.start()
t.join(timeout=5)
t.is_alive()

# Lock
lock = threading.Lock()
with lock:
    shared_resource += 1

# RLock (reentrant)
rlock = threading.RLock()

# Event
event = threading.Event()
event.set() / event.wait() / event.clear()

# Semaphore
sem = threading.Semaphore(5)

# Condition
cond = threading.Condition()
with cond:
    cond.wait()
    cond.notify_all()

# Timer
t = threading.Timer(3.0, func)
t.start() / t.cancel()

# Barrier
b = threading.Barrier(n)
b.wait()
```

### Multiprocessing
```python
import multiprocessing as mp

p = mp.Process(target=func, args=(x,))
p.start() / p.join() / p.terminate()

# Pool
with mp.Pool(processes=4) as pool:
    results = pool.map(func, iterable)
    results = pool.starmap(func, [(a,b)])
    pool.apply_async(func, callback=cb)

# Shared memory
val = mp.Value("i", 0)
arr = mp.Array("d", [1.0, 2.0])

# Queue & Pipe
q = mp.Queue()
parent_conn, child_conn = mp.Pipe()

# Manager
with mp.Manager() as mgr:
    d = mgr.dict()
    lst = mgr.list()
```

### Asyncio
```python
import asyncio

# Coroutine
async def main():
    await asyncio.sleep(1)

asyncio.run(main())

# Tasks
task = asyncio.create_task(coro())
await task
task.cancel()

# Gather (concurrent execution)
results = await asyncio.gather(coro1(), coro2(), return_exceptions=True)

# TaskGroup (3.11+)
async with asyncio.TaskGroup() as tg:
    t1 = tg.create_task(coro1())
    t2 = tg.create_task(coro2())

# Timeout
async with asyncio.timeout(5.0):     # 3.11+
    await long_running()

try:
    await asyncio.wait_for(coro(), timeout=5.0)
except asyncio.TimeoutError: ...

# Async iteration
async for item in async_generator():
    process(item)

# Async context manager
async with aiofiles.open("f") as f:
    data = await f.read()

# Queue
q = asyncio.Queue()
await q.put(item)
item = await q.get()

# Semaphore / Lock / Event
sem = asyncio.Semaphore(10)
lock = asyncio.Lock()
event = asyncio.Event()

# Streams
reader, writer = await asyncio.open_connection(host, port)

# Async generator
async def ticker():
    for i in range(10):
        yield i
        await asyncio.sleep(0.1)

# Async comprehension
results = [x async for x in aiter]
results = {x async for x in aiter}
```

### concurrent.futures
```python
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor, as_completed

with ThreadPoolExecutor(max_workers=4) as ex:
    future = ex.submit(func, arg)
    results = list(ex.map(func, iterable))

for future in as_completed(futures):
    result = future.result()
    future.exception()
    future.cancel()
```

---

## 7. Design Patterns

### Creational
```python
# Singleton
class Singleton:
    _instance = None
    def __new__(cls):
        if not cls._instance:
            cls._instance = super().__new__(cls)
        return cls._instance

# Factory Method
class Creator:
    def create(self) -> Product: ...

class ConcreteCreator(Creator):
    def create(self): return ConcreteProduct()

# Abstract Factory
class GUIFactory(ABC):
    @abstractmethod
    def create_button(self): ...
    @abstractmethod
    def create_checkbox(self): ...

# Builder
class QueryBuilder:
    def select(self, *cols): self._cols = cols; return self
    def where(self, cond): self._cond = cond; return self
    def build(self): return Query(self._cols, self._cond)

# Prototype
import copy
clone = copy.deepcopy(original)
```

### Structural
```python
# Adapter
class Adapter:
    def __init__(self, adaptee): self._a = adaptee
    def request(self): return self._a.specific_request()

# Decorator Pattern
class LoggingDecorator:
    def __init__(self, component): self._c = component
    def operation(self):
        log(); return self._c.operation()

# Facade
class Facade:
    def __init__(self):
        self._sub1 = Subsystem1()
        self._sub2 = Subsystem2()
    def operation(self):
        self._sub1.op1(); self._sub2.op2()

# Proxy
class Proxy:
    def __init__(self, real): self._real = real
    def request(self):
        if self._check_access():
            return self._real.request()

# Composite
class Component(ABC):
    @abstractmethod
    def operation(self): ...

class Leaf(Component):
    def operation(self): return "Leaf"

class Composite(Component):
    def __init__(self): self._children = []
    def add(self, c): self._children.append(c)
    def operation(self):
        return [c.operation() for c in self._children]
```

### Behavioral
```python
# Observer
class EventEmitter:
    def __init__(self): self._listeners = defaultdict(list)
    def on(self, event, fn): self._listeners[event].append(fn)
    def emit(self, event, *args):
        for fn in self._listeners[event]: fn(*args)

# Strategy
class Sorter:
    def __init__(self, strategy): self._strategy = strategy
    def sort(self, data): return self._strategy(data)

bubble_sorter = Sorter(bubble_sort)

# Command
class Command(ABC):
    @abstractmethod
    def execute(self): ...
    @abstractmethod
    def undo(self): ...

# Iterator
class CountDown:
    def __init__(self, n): self.n = n
    def __iter__(self): return self
    def __next__(self):
        if self.n <= 0: raise StopIteration
        self.n -= 1; return self.n

# Template Method
class DataProcessor(ABC):
    def process(self):           # template
        data = self.read()
        data = self.transform(data)
        self.write(data)
    @abstractmethod
    def read(self): ...
    @abstractmethod
    def transform(self, data): ...
    @abstractmethod
    def write(self, data): ...

# Chain of Responsibility
class Handler(ABC):
    def __init__(self): self._next = None
    def set_next(self, handler):
        self._next = handler; return handler
    def handle(self, request):
        if self._next: return self._next.handle(request)

# State
class TrafficLight:
    def __init__(self): self.state = RedState()
    def change(self): self.state = self.state.next()

# Visitor
class Visitor(ABC):
    @abstractmethod
    def visit_circle(self, c): ...
    @abstractmethod
    def visit_rect(self, r): ...
```

---

## 8. Best Practices

### Pythonic Idioms
```python
# Unpacking
a, b = b, a                        # swap
first, *rest = lst
*init, last = lst

# Enumerate & zip
for i, val in enumerate(lst, start=1): ...
for a, b in zip(list1, list2): ...
for a, b, c in zip(l1, l2, l3, strict=True): ...  # 3.10+

# Dict merge
merged = {**dict1, **dict2}        # or dict1 | dict2 (3.9+)

# Conditional import
try:
    import ujson as json
except ImportError:
    import json

# Safe navigation
value = d.get("key", {}).get("nested")

# Boolean checks
if lst:            # not if len(lst) > 0
if not lst:
if x is None:      # not if x == None
if x is not None:

# One-liner class
from types import SimpleNamespace
cfg = SimpleNamespace(host="localhost", port=8080)
```

### Decorators (Common)
```python
from functools import wraps

def retry(times=3):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(times):
                try: return func(*args, **kwargs)
                except Exception: pass
        return wrapper
    return decorator

@retry(times=5)
def flaky_call(): ...

# Built-in decorators
@staticmethod
@classmethod
@property
@abstractmethod
@functools.lru_cache(maxsize=256)
@functools.cached_property
@functools.total_ordering
@functools.singledispatch
@contextlib.contextmanager
@dataclasses.dataclass
```

### File & I/O
```python
# Always use context managers
with open("file.txt", "r", encoding="utf-8") as f:
    for line in f:          # memory-efficient
        process(line)

# pathlib (preferred over os.path)
from pathlib import Path
p = Path("dir") / "sub" / "file.txt"
p.read_text() / p.write_text(data)
p.exists() / p.is_file() / p.is_dir()
p.mkdir(parents=True, exist_ok=True)
list(p.glob("**/*.py"))
p.stem / p.suffix / p.parent / p.name
```

### Logging
```python
import logging

logging.basicConfig(level=logging.DEBUG,
                    format="%(asctime)s %(levelname)s %(name)s: %(message)s")

logger = logging.getLogger(__name__)
logger.debug("msg") / logger.info() / logger.warning()
logger.error() / logger.exception() / logger.critical()

# Use lazy formatting
logger.info("Value: %s", value)    # not f"Value: {value}"
```

### Testing
```python
import unittest

class TestMath(unittest.TestCase):
    def setUp(self): ...
    def tearDown(self): ...
    def test_add(self):
        self.assertEqual(add(1, 2), 3)
        self.assertRaises(ValueError, bad_func)
        self.assertIn(item, collection)
        self.assertTrue(condition)

# pytest style
def test_add():
    assert add(1, 2) == 3

# Fixtures (pytest)
import pytest

@pytest.fixture
def client():
    return TestClient()

def test_endpoint(client):
    assert client.get("/").status_code == 200

@pytest.mark.parametrize("a,b,expected", [(1,2,3),(0,0,0)])
def test_add(a, b, expected):
    assert add(a, b) == expected
```

---

## 9. Design Principles

```
SOLID
─────────────────────────────────────────────────────────
S  Single Responsibility   One class = one reason to change
O  Open/Closed             Open for extension, closed for modification
L  Liskov Substitution     Subclasses must be substitutable for base
I  Interface Segregation   Many specific interfaces > one general
D  Dependency Inversion    Depend on abstractions, not concretions

Other Core Principles
─────────────────────────────────────────────────────────
DRY   Don't Repeat Yourself     — extract reusable abstractions
KISS  Keep It Simple, Stupid    — simplest working solution first
YAGNI You Aren't Gonna Need It  — don't build for imagined futures
LoD   Law of Demeter            — talk only to immediate neighbors
SoC   Separation of Concerns    — keep layers and responsibilities distinct
CoC   Convention over Config    — sensible defaults reduce boilerplate
Fail Fast                       — surface errors early and explicitly
Composition over Inheritance    — prefer has-a over is-a for flexibility
Explicit over Implicit          — (The Zen of Python)
```

### Dependency Injection in Python
```python
# Poor: hard dependency
class Service:
    def __init__(self):
        self.db = PostgresDB()       # tightly coupled

# Good: injected dependency
class Service:
    def __init__(self, db: Database):  # loosely coupled
        self.db = db

svc = Service(db=MockDB())            # easy to test/swap
```

---

This covers virtually the entire Python syntax surface area. Each section can be deepened further — let me know if you'd like expanded examples or deep dives into any specific area!