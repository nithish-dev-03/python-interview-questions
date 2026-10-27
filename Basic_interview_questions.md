# Comprehensive Python Interview Questions & Answers
**From Very Basic / Beginner to Advanced Topics**

---
## 1. Beginner Level Questions

### Q1: What is Python and what are its key features?
**Answer:**
Python is a **high-level, general-purpose programming language** known for its **readable syntax and extensive ecosystem.**

Key features:
- Simple and readable syntax.
- Dynamic typing.
- Automatic memory management.
- Extensive standard library.
- Exception handling.
- Support for asynchronous programming.
- Large ecosystem for web development, automation, data science, and AI.

### Q2. Is Python compiled or interpreted?
**Answer:**
Python is commonly described as an **interpreted language**, but its **execution involves multiple stages.**

In CPython, the usual process is:
1. Python source code is parsed.
2. The source code is compiled into **bytecode**.
3. The Python virtual machine executes that **bytecode**.

### Q3. What are Python's built-in data types?
**Answer:**
| Category | Types | Example |
|---|---|---|
| Numeric | `int`, `float`, `complex` | `10`, `3.14`, `2 + 3j` |
| Boolean | `bool` | `True`, `False` |
| Text | `str` | `"Python"` |
| Sequence | `list`, `tuple`, `range` | `[1, 2]` |
| Mapping | `dict` | `{"name": "Nithish"}` |
| Set | `set`, `frozenset` | `{1, 2, 3}` |
| Binary | `bytes`, `bytearray`, `memoryview` | `b"hello"` |
| Special | `NoneType` | `None` |

### Q4. What is dynamic typing in Python?
**Answer:**
Dynamic typing means that a **variable's type** is determined at **runtime**. A variable can reference objects of different types at different points in the program.
```python
x = 10  # x is an integer
x = "Hello"  # Now x is a string
x = [1, 2, 3]  # Now x is a list
```

### Q5. What is the difference between mutable and immutable objects?
**Answer:**
A mutable object can be modified after creation. An immutable object cannot be modified after creation.
| Mutable | Immutable |
|---|---|
| `list` | `tuple` |
| `dict` | `str` |
| `set` | `int` |
| `bytearray` | `float` |
| | `frozenset` |

## Why does mutability matter? ## 
- It affects object sharing, function arguments, caching, concurrency, and bugs caused by modifying shared objects.

### Q6. What is the difference between == and is? ###
**Answer:**
- `==` compares **values** (checks if two objects are equal).
- `is` compares **identities** (checks if two references point to the same object).

### Q7. What is type conversion in Python? ###
**Answer:**
Type conversion changes a value from one data type to another.

### Q8. What are truthy and falsy values? ###
**Answer:**
Python evaluates objects as either truthy or falsy in conditional expressions.

### Truthy values ###
All non-zero numbers, non-empty sequences, non-empty mappings, non-empty sets, `True`.

### Falsy values ###
`None`, `False`, `0`, `0.0`, `""`, `[]`, `()`, `{}`, `set()`, `range(0)`

## What is the difference between if value and if value is not None? ##

**Answer:**
`if value` rejects all falsy values.
`if value is not None` rejects only `None`.

### Q9. What is the difference between break, continue, and pass? ###
**Answer:**
- **break**: Exits the nearest enclosing loop.
- **continue**: Skips the rest of the current iteration.
- **pass**: Does nothing; it serves as a placeholder.

### Q10. What is the difference between range(), zip() and enumerate()? ###
**Answer:**
- **range()**: Generates a sequence of integers.
- **enumerate()**: Provides an index and the corresponding item while iterating over an iterable.
- **zip()**: Pairs elements from two or more iterables.

### Q11. What are the different types of operators in Python? ###
**Answer:**
Python supports:
1. Arithmetic operators: +, -, *, /, //, %, **
2. Comparison operators: ==, !=, <, >, <=, >=
3. Logical operators: and, or, not
4. Assignment operators: =, +=, -=, *=
5. Membership operators: in, not in
6. Identity operators: is, is not
7. Bitwise operators: &, |, ^, ~, <<, >>

### Q12. What is the difference between / and //? ###
**Answer:**
- `/` is float division
- `//` is floor division (rounds down to the nearest integer)

### Q13. What is the difference between `and` and `or`? ###
**Answer:**
- `and` returns `True` if both operands are true.
- `or` returns `True` if at least one operand is true.

### Q14. What is the difference between if, elif, and else? ###
**Answer:**
- `if` checks a condition and executes a block of code if the condition is true.
- `elif` checks an additional condition if the previous `if` or `elif` conditions were false.
- `else` executes a block of code if none of the preceding `if` or `elif` conditions were true.

### Q15. What is the difference between for and while loops? ###
**Answer:**
A for loop iterates over an iterable. A while loop repeats as long as its condition is true.

### Q16. What is a nested loop? ###
**Answer:**
A loop inside another loop.

### Q17. What is pattern matching in Python? ###
**Answer:**
Pattern matching is a feature introduced in Python 3.10 that allows you to match values against patterns and execute code based on the match.

### Q18. Why are strings immutable? ###
**Answer:**
Python strings cannot be modified in place. Operations that appear to change a string create a new string.
- Immutability makes strings easier to share safely and allows implementations to optimize their handling.

### Q19. What is string slicing? ###
**Answer:**
Slicing extracts a portion of a sequence.
```
sequence[start:stop:step]
```

### Q20. What is the difference between a list and a tuple? ###
**Answer:**
A list is mutable, whereas a tuple is immutable.
```
list = [1, 2, 3] # mutable
tuple = (1, 2, 3) # immutable
```

### Q21. What is a list comprehension? ###
**Answer:**
A list comprehension provides a concise way to construct a list.
```
squares = [number ** 2 for number in range(5)]
even_numbers = [
    number
    for number in range(10)
    if number % 2 == 0
]

print(squares)
# [0, 1, 4, 9, 16]
```

### Q22. What is the difference between a list comprehension and a generator expression? ###
**Answer:**
A list comprehension creates a list immediately. A generator expression produces values lazily as they are requested.
```
list_comp = [x * x for x in range(5)]  # List comprehension
generator = (x * x for x in range(5))  # Generator expression
```
 - Generators are useful when processing large datasets or streams.

### Q23. What is the difference between a set and a list? ###
**Answer:**
A list maintains order and permits duplicate values. A set stores unique hashable elements and does not provide positional indexing.
- Average membership-test complexity for a Python set is typically \(O(1)\), although worst-case behavior can be worse.

### Q24. What is a dictionary in Python? ###
**Answer:**
A dictionary stores key-value pairs.
```
user = {
    "name": "Nithish",
    "role": "Developer",
    "active": True
}

print(user["name"])
```
- Dictionary keys must be hashable.
- You can safely retrieve an optional key using get():
```
print(user.get("email"))  # returns None
print(user.get("email", "default@example.com"))  # returns a default value
```
- Dictionary iteration order matches insertion order (since Python 3.7).

### Q25. What is the difference between dict.get() and dictionary indexing? ###
**Answer:**
- Dictionary indexing `d[key]` raises a `KeyError` if the key is not found.
- `d.get(key)` returns `None` by default if the key is not found.

### Q26. How do you remove duplicates from a list? ###
**Answer:**
- Using SET
```
numbers = [1, 2, 2, 3, 3, 4]

result = list(set(numbers))
```
- Preserved with original order
```
numbers = [1, 2, 2, 3, 3, 4]

result = list(dict.fromkeys(numbers))

print(result)
# [1, 2, 3, 4]
```

### Q27. What is unpacking in Python? ###
**Answer:**
Unpacking assigns elements from an iterable to multiple variables.

### Q28. What is the difference between shallow copy and deep copy? ###
**Answer:**
A shallow copy creates a new outer object but retains references to nested objects. A deep copy recursively copies nested objects where supported.
```
import copy

original = [[1, 2], [3, 4]]

shallow = copy.copy(original)
deep = copy.deepcopy(original)

original[0].append(99)

print(shallow)  # [[1, 2, 99], [3, 4]]
print(deep)     # [[1, 2], [3, 4]]
```

### Q29. What is a function in Python? ###
**Answer:**
 - A function is a reusable block of code that performs a task.
 - Functions improve code reuse, readability, testing, and maintainability.

### Q30. What is the difference between parameters and arguments?

**Answer:**
Parameters are the names in a function definition; arguments are the values supplied when the function is called.

```python
def greet(name):       # name is a parameter
    return f"Hello, {name}"

greet("Nithish")       # "Nithish" is an argument
```

### Q31. What are positional and keyword arguments? ###
**Answer:**
Positional arguments are matched according to position. Keyword arguments are matched by parameter name.
```
def create_user(name, age):
    return {"name": name, "age": age}

create_user("Nithish", 25)

create_user(name="Nithish", age=25)

create_user(age=25, name="Nithish")
```

### Q32. What are `*args` and `**kwargs`? ###
**Answer:**
- `*args` collects additional positional arguments into a tuple.
- `**kwargs` collects additional keyword arguments into a dictionary.
```
def flexible_function(a, b, *args, **kwargs):
    print("Positional arguments:", a, b)
    print("Extra positional arguments (*args):", args)
    print("Keyword arguments (**kwargs):", kwargs)

flexible_function(
    10,
    20,
    30, 40, 50,
    name="Alice",
    city="New York"
)
```
- They are useful for flexible APIs, decorators, and wrapper functions.

### Q33. What is the mutable default argument problem? ###
**Answer:**
- Default argument values are evaluated once when the function is defined, not every time it is called.
- This can cause unexpected state sharing.

### Q34. What is the difference between return and yield? ###

**Answer:**
`return` ends a function and sends one result to the caller. `yield` turns a function into a generator, producing values lazily while preserving its execution state between iterations.

```python
def numbers(limit):
    for number in range(limit):
        yield number

for number in numbers(3):
    print(number)
```

### Q35. What is a lambda function?

**Answer:**
A lambda is a small anonymous function containing one expression. It is useful for simple callbacks and sorting keys; use a named function for complex logic.

```python
users = [{"name": "A", "age": 30}, {"name": "B", "age": 20}]
users.sort(key=lambda user: user["age"])
```

### Q36. What is the LEGB rule?

**Answer:**
Python resolves names in this order: Local scope, Enclosing function scope, Global module scope, and Built-in scope. Use `nonlocal` for an enclosing variable and `global` for a module-level variable, but prefer passing values explicitly when possible.

### Q37. What are decorators?

**Answer:**
A decorator wraps a function or class to add behavior without changing its source code.

```python
from functools import wraps

def logged(function):
    @wraps(function)
    def wrapper(*args, **kwargs):
        print(f"Calling {function.__name__}")
        return function(*args, **kwargs)
    return wrapper

@logged
def add(a, b):
    return a + b
```

Use `functools.wraps` so metadata such as the function name and docstring is preserved.

### Q38. What is a closure?

**Answer:**
A closure is an inner function that remembers values from its enclosing scope even after the outer function has returned.

```python
def multiplier(factor):
    def multiply(value):
        return value * factor
    return multiply

double = multiplier(2)
assert double(5) == 10
```

### Q39. What is the difference between an iterator and an iterable?

**Answer:**
An iterable can produce an iterator through `iter()`. An iterator implements `__next__()` and returns one item at a time until it raises `StopIteration`. Lists are iterable; a list iterator is an iterator.

### Q40. What is exception handling in Python?

**Answer:**
Use `try` for risky code, `except` for expected failures, `else` for successful execution, and `finally` for cleanup.

```python
try:
    value = int(text)
except ValueError:
    value = 0
else:
    print("Parsed successfully")
finally:
    print("Finished")
```

Catch specific exceptions and avoid silently swallowing errors.

### Q41. What is the difference between `raise` and `assert`?

**Answer:**
`raise` deliberately throws an exception and is appropriate for runtime validation. `assert` checks a developer assumption and may be disabled with Python optimization flags, so it must not be used for user-input validation or security checks.

### Q42. What are classes, objects, and inheritance?

**Answer:**
A class is a blueprint; an object is an instance of that class. Inheritance lets a child class reuse or specialize a parent class. Encapsulation, abstraction, inheritance, and polymorphism are the four commonly cited OOP principles.

```python
class Animal:
    def speak(self):
        raise NotImplementedError

class Dog(Animal):
    def speak(self):
        return "woof"
```

Prefer composition over deep inheritance hierarchies when components can be combined independently.

### Q43. What are `classmethod`, `staticmethod`, and `property`?

**Answer:**
`classmethod` receives the class as `cls`, `staticmethod` receives neither `self` nor `cls`, and `property` exposes method-backed behavior through attribute syntax.

### Q44. What is the difference between `__str__` and `__repr__`?

**Answer:**
`__str__` is a human-friendly representation. `__repr__` is intended to be unambiguous and useful for debugging. Implement `__repr__` first when a useful diagnostic representation is important.

### Q45. What is a context manager?

**Answer:**
A context manager guarantees setup and cleanup around a block, commonly through `with`.

```python
with open("data.txt", encoding="utf-8") as file:
    contents = file.read()
```

It can be implemented with `__enter__` and `__exit__`, or with `contextlib.contextmanager`.

---

## 2. Advanced Python and Concurrency

### Q46. What is the GIL?

**Answer:**
In the standard CPython implementation, the Global Interpreter Lock allows only one thread at a time to execute Python bytecode in a process. Threads are still useful for I/O-bound work, while CPU-bound work may benefit from multiprocessing, native extensions, or a separate service. The exact behavior depends on the Python implementation and version.

### Q47. What is the difference between concurrency and parallelism?

**Answer:**
Concurrency is managing multiple tasks whose execution overlaps. Parallelism is executing multiple tasks simultaneously, usually on multiple CPU cores. `asyncio` provides cooperative concurrency; processes provide true CPU parallelism.

### Q48. What are `async` and `await`?

**Answer:**
`async def` defines a coroutine. `await` suspends that coroutine until another awaitable completes, allowing the event loop to run other work.

```python
import asyncio

async def main():
    await asyncio.sleep(0.1)
    return "done"

result = asyncio.run(main())
```

Do not call blocking database, filesystem, or network operations directly inside an async endpoint unless they are moved to a thread or replaced with async-compatible libraries.

### Q49. What is a generator and when should you use one?

**Answer:**
A generator produces values on demand, reducing memory usage and enabling streaming pipelines. It is useful for large files, database results, and incremental transformations.

### Q50. What are type hints and why do they matter?

**Answer:**
Type hints document expected types and enable static analysis, IDE support, refactoring, and framework validation. Python does not enforce them automatically at runtime.

```python
def total(prices: list[float]) -> float:
    return sum(prices)
```

### Q51. What is a dataclass?

**Answer:**
`dataclasses.dataclass` generates common methods such as `__init__`, `__repr__`, and equality methods for data-focused classes.

```python
from dataclasses import dataclass

@dataclass
class User:
    name: str
    active: bool = True
```

### Q52. What are common Python performance practices?

**Answer:**
Choose appropriate data structures, avoid repeated work, use iterators for large streams, batch I/O, profile before optimizing, and measure with representative data. Algorithmic complexity usually matters more than micro-optimizations.

---

## 3. FastAPI Interview Questions

### Q53. What is FastAPI?

**Answer:**
FastAPI is a modern Python web framework for building APIs. It uses Starlette for web capabilities and Pydantic for data validation and serialization. Type hints drive request parsing, validation, dependency injection, and OpenAPI documentation.

### Q54. How do you create a basic FastAPI endpoint?

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/health")
async def health_check():
    return {"status": "ok"}
```

Run it with `uvicorn main:app --reload` during development.

### Q55. What is the difference between path, query, and body parameters?

**Answer:**
Path parameters identify a resource in the URL, query parameters filter or control a request, and a request body carries structured data, usually for `POST`, `PUT`, or `PATCH`.

```python
from fastapi import Query
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float

@app.post("/items/{item_id}")
async def create_item(item_id: int, item: Item, limit: int = Query(10, ge=1)):
    return {"id": item_id, "item": item, "limit": limit}
```

### Q56. What is Pydantic used for in FastAPI?

**Answer:**
Pydantic models validate incoming data, define response schemas, convert compatible values, and generate accurate OpenAPI documentation. Keep request and response models separate when their fields or security requirements differ.

### Q57. How do you validate input?

```python
from pydantic import BaseModel, Field

class Product(BaseModel):
    name: str = Field(min_length=1, max_length=100)
    price: float = Field(gt=0)
```

FastAPI returns a structured `422 Unprocessable Entity` response when validation fails.

### Q58. What is dependency injection in FastAPI?

**Answer:**
Dependencies are reusable functions whose results FastAPI resolves and injects into endpoints using `Depends`. They are useful for authentication, database sessions, pagination, and shared request logic.

```python
from fastapi import Depends

def get_current_user():
    return {"id": 1}

@app.get("/me")
async def me(user=Depends(get_current_user)):
    return user
```

### Q59. How do you manage database sessions?

**Answer:**
Create a session per request, yield it from a dependency, and close it in `finally`. Commit successful writes, roll back failed transactions, and never share a request-scoped session across unrelated requests.

### Q60. What are middleware and CORS?

**Answer:**
Middleware wraps request processing for concerns such as logging, timing, correlation IDs, and headers. CORS controls which browser origins may call the API. Configure explicit trusted origins and avoid allowing credentials with a wildcard origin.

### Q61. How do you handle errors?

**Answer:**
Raise `HTTPException` for expected HTTP errors, define custom exception handlers for consistent error payloads, and log unexpected exceptions with a request ID without exposing stack traces or secrets to clients.

```python
from fastapi import HTTPException

if item is None:
    raise HTTPException(status_code=404, detail="Item not found")
```

### Q62. How do authentication and authorization differ?

**Answer:**
Authentication verifies who the caller is. Authorization decides what that caller may do. Common API approaches include OAuth2 bearer tokens, JWTs, API keys, and session cookies. Validate token signature, issuer, audience, expiry, and scopes; enforce authorization at the resource boundary.

### Q63. How do you test a FastAPI application?

**Answer:**
Use unit tests for isolated logic and `TestClient` or an async HTTP client for endpoint tests. Override dependencies to use a test database and fake external services.

```python
from fastapi.testclient import TestClient

client = TestClient(app)

def test_health():
    response = client.get("/health")
    assert response.status_code == 200
    assert response.json() == {"status": "ok"}
```

### Q64. How should a FastAPI project be organized?

**Answer:**
Separate routers, schemas, models, services, repositories, dependencies, configuration, and tests. Keep business rules out of route functions so they can be tested independently.

```text
app/
  main.py
  api/routers/
  core/                 # settings, security, logging
  models/
  schemas/
  services/
  repositories/
tests/
```

### Q65. How do you improve FastAPI performance?

**Answer:**
Use async endpoints for non-blocking I/O, avoid blocking calls on the event loop, add database indexes, prevent N+1 queries, paginate large results, cache safe read-heavy data, compress suitable responses, and profile latency before changing code.

### Q66. How do you deploy FastAPI in production?

**Answer:**
Pin dependencies, configure environment-based settings, run behind a process manager and reverse proxy, use multiple workers where appropriate, add health checks, structured logs, metrics, timeouts, TLS, and graceful shutdown. Containers should be immutable and secrets should come from a secret manager or environment injection rather than source control.

### Q67. What are background tasks and when should you avoid them?

**Answer:**
FastAPI background tasks run work after a response is sent, suitable for small non-critical tasks such as sending a notification. For durable, retryable, or long-running jobs, use a queue and worker system such as Celery, Dramatiq, or a cloud-native job service.

### Q68. What is a WebSocket?

**Answer:**
A WebSocket provides a persistent, bidirectional connection for real-time communication. Authenticate the connection, validate messages, handle disconnects, apply backpressure, and avoid assuming that one process can hold all connection state when the application is horizontally scaled.

---

## 4. SQL, Architecture, and System Design

### Q69. What is a transaction?

**Answer:**
A transaction groups operations into an atomic unit. The ACID properties are atomicity, consistency, isolation, and durability. Choose an isolation level based on the consistency and contention requirements of the operation.

### Q70. What is the N+1 query problem?

**Answer:**
It occurs when one query loads a collection and then one additional query runs for each item. Detect it with query logging and fix it with joins, eager loading, batching, or carefully designed repository queries.

### Q71. How do you design a robust API?

**Answer:**
Define resource-oriented routes, consistent status codes and error shapes, pagination, filtering, validation, idempotency for retried writes, authentication and authorization, rate limits, observability, and versioning or a compatibility policy.

### Q72. How would you design a production FastAPI service?

**Answer:**
Start with clear bounded responsibilities and a stateless API layer. Add a relational database with migrations, a cache only where measured useful, a queue for asynchronous work, centralized logs and metrics, tracing, health/readiness endpoints, timeouts and retries with limits, and a deployment strategy that supports rollback. Document failure modes and data ownership before choosing infrastructure.

### Q73. How do you make an endpoint idempotent?

**Answer:**
Define an idempotency key for retriable writes, store the key with the result, reject conflicting reuse, and make the database operation atomic. This prevents duplicate orders or payments when clients retry after a timeout.

### Q74. What should you discuss when debugging a slow endpoint?

**Answer:**
Measure total latency and break it down into queue time, application time, database time, external calls, serialization, and network time. Check logs, traces, query plans, connection pools, payload size, and recent changes before applying a fix.

### Q75. What are common API security risks?

**Answer:**
Broken access control, injection, insecure secrets, excessive data exposure, weak authentication, missing rate limits, unsafe deserialization, SSRF, and inadequate logging are common risks. Validate at boundaries, use least privilege, parameterize queries, protect secrets, and test authorization with negative cases.

---

