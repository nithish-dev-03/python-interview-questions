# Python & FastAPI Interview Preparation — Advanced Questions with Answers

This guide contains 155 intermediate-to-advanced interview questions in ten sections. Answers are intentionally concise; expand them with a production example during an interview.

## Part 1 — Python Internals and Memory Management

### Q1. How does Python execute source code?
**Answer:** CPython parses source code, compiles it to bytecode, and executes that bytecode on the Python virtual machine. Compilation errors occur before runtime execution.

### Q2. What is bytecode in Python?
**Answer:** Bytecode is an intermediate instruction set stored in `.pyc` files inside `__pycache__`. It is implementation-specific and is executed by the CPython virtual machine.

### Q3. What is the difference between CPython, PyPy, and other implementations?
**Answer:** CPython is the reference implementation and uses a bytecode interpreter with a GIL. PyPy uses a tracing JIT and can improve long-running pure-Python code; implementations such as Jython and IronPython target other virtual machines.

### Q4. How does Python's memory allocator work?
**Answer:** Python manages objects on a private heap. CPython's `pymalloc` efficiently handles many small allocations, while larger allocations are delegated to the platform allocator; freed memory may remain in Python's arenas for reuse.

### Q5. What is reference counting?
**Answer:** CPython stores a reference count for each object. When the count reaches zero, the object can be deallocated immediately, although reference counting alone cannot collect cycles.

### Q6. How does cyclic garbage collection work?
**Answer:** The cyclic garbage collector supplements reference counting by finding groups of container objects that reference one another but are unreachable from application roots. `gc` can inspect, tune, or manually collect generations.

### Q7. What are strong and weak references?
**Answer:** A strong reference keeps an object alive. A `weakref` refers to an object without preventing its collection, which is useful for caches and registries that must not own their values.

### Q8. What is interning in Python?
**Answer:** Interning reuses immutable objects, commonly some strings and small integers, to save memory and speed comparisons. It is an implementation optimization, so never rely on interning for identity comparisons.

### Q9. What is the difference between `is` and `==`?
**Answer:** `==` compares values; `is` compares object identity. Use identity checks for singletons such as `None`, not as a general equality test.

### Q10. What happens when you assign one variable to another?
**Answer:** Assignment binds another name to the same object; it does not copy the object. Mutating a shared mutable object is therefore visible through both names.

### Q11. How are mutable and immutable objects handled internally?
**Answer:** Immutable objects cannot change their value after creation and can often be safely shared or hashed. Mutable objects retain identity while their internal state changes; this affects aliases, hashing, and concurrency.

### Q12. What is stack memory versus heap memory in Python?
**Answer:** Python objects live on a managed heap. Python frames and references are conceptually associated with call-stack execution, but the exact layout is implementation-dependent; avoid applying low-level C memory rules too literally to Python.

### Q13. What are `__slots__`?
**Answer:** `__slots__` declares permitted instance attributes and can remove the per-instance `__dict__`, reducing memory for many objects. It restricts dynamic attributes and needs care with inheritance and weak references.

### Q14. How can you investigate a memory leak?
**Answer:** Reproduce it, measure process and object growth, inspect references, and compare snapshots over time. Tools include `tracemalloc`, `gc`, allocation profilers, heap dumps, and production metrics; distinguish a leak from allocator memory retained for reuse.

### Q15. How do you use `tracemalloc`?
**Answer:** Start tracing early with `tracemalloc.start()`, take snapshots, and compare them by line or traceback. The largest positive differences identify allocation sites that deserve investigation.

## Part 2 — Advanced Object-Oriented Programming

### Q16. What is method resolution order (MRO)?
**Answer:** MRO is the order Python searches classes for an attribute or method. Python uses C3 linearization, which preserves local precedence and a consistent ordering for multiple inheritance; inspect it with `Class.mro()`.

### Q17. How does `super()` work with multiple inheritance?
**Answer:** `super()` delegates to the next class in the MRO, not necessarily the direct parent. Cooperative classes should accept compatible arguments and call `super()` so every class participates exactly once.

### Q18. What is the diamond inheritance problem?
**Answer:** It occurs when two classes inherit from a common base and another class inherits from both. C3 MRO and cooperative `super()` calls prevent the common base from being initialized twice unexpectedly.

### Q19. Composition versus inheritance?
**Answer:** Inheritance models an “is-a” relationship and reuses a type hierarchy. Composition models “has-a” relationships and usually gives better flexibility because behavior can be replaced without changing the class hierarchy.

### Q20. Abstraction versus encapsulation?
**Answer:** Abstraction exposes essential behavior while hiding implementation details. Encapsulation groups state and behavior and controls access to state; Python uses conventions, properties, and modules rather than strict private fields.

### Q21. How do abstract base classes work?
**Answer:** `abc.ABC` and `@abstractmethod` define required interfaces. A class with unimplemented abstract methods cannot be instantiated, which helps enforce contracts while still allowing polymorphism.

### Q22. What are descriptors?
**Answer:** A descriptor is an object defining `__get__`, `__set__`, or `__delete__`. Descriptors power properties, methods, ORM fields, and validation frameworks by controlling attribute access.

### Q23. How do `__get__`, `__set__`, and `__delete__` work?
**Answer:** They are called when an attribute is read, assigned, or deleted. A descriptor defining `__set__` or `__delete__` is a data descriptor and takes precedence over an instance dictionary entry.

### Q24. Data versus non-data descriptors?
**Answer:** Data descriptors define `__set__` or `__delete__` and override instance attributes. Non-data descriptors define only `__get__`, so an instance attribute can shadow them.

### Q25. How do metaclasses work?
**Answer:** A metaclass constructs classes, just as a class constructs instances. `type` is the default metaclass; custom metaclasses can register classes or enforce rules, but they add complexity and should be used only when class decorators or `__init_subclass__` are insufficient.

### Q26. What are class decorators?
**Answer:** A class decorator receives a class and returns the same or a modified/replacement class. They are often simpler than metaclasses for registration, validation, or adding methods.

### Q27. What is dependency inversion?
**Answer:** High-level business logic should depend on abstractions rather than concrete infrastructure. In Python, inject collaborators through constructors or function parameters and provide database or service implementations separately.

### Q28. What are the SOLID principles?
**Answer:** Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, and Dependency Inversion. Apply them pragmatically: keep responsibilities focused, extend through stable interfaces, and avoid abstractions that do not remove real coupling.

### Q29. How do you implement a custom context manager?
**Answer:** Implement `__enter__` and `__exit__`, or use `contextlib.contextmanager`. `__exit__` receives exception information and can suppress an exception only by returning true; cleanup belongs there or in a `finally` block.

### Q30. How would you design a plugin architecture?
**Answer:** Define a small stable interface, discover plugins through explicit registration or Python entry points, validate versions, isolate failures, and inject dependencies. Keep plugin state and lifecycle separate from the core application.

### Part 2 practical example — cooperative multiple inheritance

```python
class Logged:
    def __init__(self, **kwargs):
        print("initializing Logged")
        super().__init__(**kwargs)

class Configured:
    def __init__(self, **kwargs):
        self.configured = True
        super().__init__(**kwargs)

class Service(Logged, Configured):
    pass

service = Service()
print(Service.mro())
```

Every class cooperates by calling `super()`, so the MRO can initialize the hierarchy once and in a predictable order.

## Part 3 — Advanced Decorators and Generators

### Q31. How do you write a decorator that accepts arguments?
**Answer:** Use three layers: an outer function for decorator configuration, a decorator function receiving the target, and a wrapper receiving calls. Apply `functools.wraps` to preserve metadata.

### Q32. Why is `functools.wraps()` important?
**Answer:** It copies metadata such as `__name__`, `__doc__`, and `__annotations__`, and sets `__wrapped__`. Frameworks and debugging tools rely on this metadata to inspect the original function.

### Q33. How do nested decorators execute?
**Answer:** Decorators are applied bottom-up at definition time, while wrapper calls execute outermost first. For `@A` above `@B`, the result is `A(B(function))`.

### Q34. Generator expression versus list comprehension?
**Answer:** A list comprehension evaluates immediately and stores all results. A generator expression evaluates lazily, uses less memory, and can be consumed only once.

### Q35. What do `yield from` and `send()` do?
**Answer:** `yield from` delegates iteration and value/exception handling to another iterable or generator. `send(value)` resumes a suspended generator and makes the value of its paused `yield` expression equal to `value`.

### Q36. What is the difference between `throw()` and `close()` on a generator?
**Answer:** `throw()` injects an exception at the suspended `yield`. `close()` raises `GeneratorExit`, allowing the generator to clean up; yielding after `GeneratorExit` is an error.

### Q37. How do generators process large files?
**Answer:** They read and yield one line or chunk at a time, avoiding a full in-memory copy. Compose generators so filtering and transformation remain streaming operations.

### Q38. How would you implement a retry decorator?
**Answer:** Catch only retryable exceptions, use bounded attempts, exponential backoff with jitter, and a maximum delay. Do not retry non-idempotent operations unless the operation has an idempotency strategy.

### Q39. How would you implement a caching decorator?
**Answer:** Build a stable key from arguments, store results with a bounded size and TTL, and define invalidation behavior. `functools.lru_cache` is suitable for pure, hashable, in-process functions; it is not a distributed cache.

### Q40. What is a coroutine versus a generator?
**Answer:** Both can suspend with `yield`-like machinery, but a coroutine represents an asynchronous computation driven by an event loop, while a generator primarily produces a sequence. Native coroutines use `async def` and `await`.

### Part 3 practical example — retry decorator with backoff

```python
from functools import wraps
import time

def retry(attempts=3, delay=0.1):
    def decorate(function):
        @wraps(function)
        def wrapper(*args, **kwargs):
            for attempt in range(attempts):
                try:
                    return function(*args, **kwargs)
                except TimeoutError:
                    if attempt == attempts - 1:
                        raise
                    time.sleep(delay * (2 ** attempt))
        return wrapper
    return decorate
```

Retry only transient failures and add jitter in production so many workers do not retry simultaneously.

## Part 4 — Advanced Concurrency

### Q41. What is the GIL?
**Answer:** In standard CPython, the Global Interpreter Lock permits one thread to execute Python bytecode at a time within a process. Threads remain useful for I/O-bound work; CPU-bound work often needs processes or native code.

### Q42. Concurrency versus parallelism?
**Answer:** Concurrency overlaps progress among tasks. Parallelism executes tasks simultaneously on separate cores; multiprocessing is the common Python approach for CPU-bound parallel work.

### Q43. How does the asyncio event loop work?
**Answer:** The event loop schedules coroutines and resumes them when awaited I/O is ready. A blocking call prevents the loop from serving other tasks, so blocking work must be replaced, isolated, or offloaded.

### Q44. `asyncio.gather()` versus `TaskGroup`?
**Answer:** `gather()` combines awaitables and has configurable exception behavior. `TaskGroup` provides structured concurrency: child tasks share a lifecycle and failures cancel the group, making cleanup and failure propagation clearer.

### Q45. What is cancellation in asyncio?
**Answer:** Cancellation injects `CancelledError` at an await point. Code should clean up in `finally`, avoid swallowing cancellation accidentally, and use timeouts so tasks cannot wait forever.

### Q46. How do you prevent blocking the event loop?
**Answer:** Use async-compatible clients for I/O and move unavoidable blocking work to `asyncio.to_thread()` or a process pool. Measure event-loop lag and review dependencies, not only endpoint definitions.

### Q47. What are race conditions?
**Answer:** A race occurs when correctness depends on timing between concurrent operations. Protect shared mutable state with locks or redesign around message passing and immutable data.

### Q48. Threading versus multiprocessing?
**Answer:** Threads share memory and are convenient for I/O but are constrained by the CPython GIL for Python bytecode. Processes provide isolation and CPU parallelism but have higher startup and data-transfer costs.

### Q49. What is a deadlock?
**Answer:** A deadlock occurs when tasks wait forever for locks held by one another. Prevent it with consistent lock ordering, short critical sections, timeouts, and fewer shared locks.

### Q50. What is backpressure?
**Answer:** Backpressure limits producers when consumers cannot keep up. Use bounded queues, flow-control APIs, rate limits, and explicit overload behavior instead of allowing unbounded memory growth.

### Q51. How should shared state be handled across workers?
**Answer:** Process-local state is not shared reliably across workers. Use a database, Redis, a queue, or another external coordination mechanism, and design caches and locks with failure and expiry semantics.

### Q52. What is a process pool?
**Answer:** A process pool reuses worker processes for CPU-bound functions. Inputs and outputs must be serializable, and pool size should reflect CPU capacity and the service's overall resource budget.

### Q53. How do you handle timeouts and retries?
**Answer:** Set connect, read, and total deadlines; retry only transient failures; use exponential backoff and jitter; and cap attempts. Combine retries with circuit breaking to avoid amplifying an outage.

### Q54. What is a semaphore?
**Answer:** A semaphore limits how many tasks enter a section at once. It is useful for bounding concurrent requests to a downstream service or connection pool.

### Q55. How do you shut down concurrent work safely?
**Answer:** Stop accepting new work, signal cancellation, await active tasks with a deadline, close clients and pools, and report unfinished work. Shutdown must be idempotent because orchestrators may send repeated signals.

### Part 4 practical example — bounded asynchronous concurrency

```python
import asyncio

async def limited_gather(items, limit=10):
    semaphore = asyncio.Semaphore(limit)

    async def run(item):
        async with semaphore:
            return await fetch(item)

    return await asyncio.gather(*(run(item) for item in items))
```

The semaphore provides backpressure by limiting simultaneous downstream calls. Add per-call timeouts and cancellation-safe cleanup around real network clients.

## Part 5 — Testing and Debugging

### Q56. Unit, integration, and end-to-end tests?
**Answer:** Unit tests isolate a small piece of logic. Integration tests verify real component boundaries such as a database. End-to-end tests exercise the user-visible workflow and are slower but valuable for critical paths.

### Q57. What makes a test reliable?
**Answer:** It is deterministic, isolated, focused, fast enough to run frequently, and explicit about dependencies. Avoid time, randomness, network calls, and shared mutable fixtures unless controlled.

### Q58. What is pytest fixture scope?
**Answer:** Fixtures can be function-, class-, module-, package-, or session-scoped. Choose the narrowest scope that gives acceptable speed; broader scopes risk state leakage.

### Q59. What is mocking?
**Answer:** Mocking replaces a dependency with a controlled test double. Mock the boundary owned by the code under test, assert meaningful behavior, and avoid over-specifying implementation details.

### Q60. What are contract tests?
**Answer:** Contract tests verify that a producer and consumer agree on an API's schema and behavior. They catch breaking changes without requiring every service to run end to end.

### Q61. How do you test async code?
**Answer:** Use an async-aware test plugin or framework, await the coroutine directly, and use async clients and database fixtures. Ensure the test event loop and resource lifecycle are managed consistently.

### Q62. How do you test FastAPI dependencies?
**Answer:** Use `app.dependency_overrides` to substitute authentication, database, or external-service dependencies. Clear overrides after each test so cases remain isolated.

### Q63. What is property-based testing?
**Answer:** It generates many inputs and checks general properties rather than only examples. It is useful for parsers, serializers, validation, and algorithms with broad input spaces.

### Q64. What is mutation testing?
**Answer:** Mutation testing changes code in small ways and checks whether tests fail. A surviving mutation indicates a gap in test assertions or an untested behavior.

### Q65. How do you debug a production exception?
**Answer:** Start with a correlation ID, structured logs, stack trace, deployment version, inputs safe to inspect, and relevant traces/metrics. Reproduce with sanitized data, fix the root cause, and add a regression test.

### Q66. How do you debug a memory leak?
**Answer:** Compare `tracemalloc` snapshots, inspect growing containers and caches, check event listeners and task references, and reproduce under controlled load. Verify that memory returns or stabilizes after work completes.

### Q67. How do you debug a deadlock?
**Answer:** Capture thread or task stacks, identify lock ownership and wait cycles, and add timeouts or lock-order instrumentation. Reduce the critical section and test the failure path.

### Q68. What is code coverage?
**Answer:** Coverage measures executed lines, branches, or paths; it is a diagnostic, not proof of correctness. Prioritize behavior, error paths, authorization, and high-risk integrations over maximizing a percentage.

### Q69. How do you test database code?
**Answer:** Use isolated transactional fixtures or disposable databases, apply real migrations, test constraints and rollback behavior, and avoid assuming mocks behave like the database engine.

### Q70. How do you test external APIs?
**Answer:** Use deterministic fakes or recorded responses for most tests, then run a small number of controlled contract or integration tests. Verify timeouts, malformed responses, retries, and authentication failures.

## Part 6 — FastAPI Request Lifecycle and Internals

### Q71. What happens during a FastAPI request?
**Answer:** The ASGI server receives the request, middleware wraps it, routing selects an endpoint, dependencies resolve, request data is validated, the endpoint runs, and the result is serialized into a response. Exceptions pass through middleware and exception handlers.

### Q72. What is ASGI?
**Answer:** ASGI is an asynchronous server/application interface supporting HTTP, WebSockets, and lifespan events. FastAPI uses Starlette's ASGI toolkit and can run under servers such as Uvicorn.

### Q73. What is the difference between `def` and `async def` endpoints?
**Answer:** Async endpoints run as coroutines and should use non-blocking libraries. Synchronous endpoints are executed in a thread pool by Starlette, but excessive blocking still consumes worker capacity.

### Q74. What is dependency injection in FastAPI?
**Answer:** `Depends` declares reusable providers for authentication, sessions, configuration, and shared logic. FastAPI resolves the dependency graph per request and can cache a dependency result within that request.

### Q75. How do dependency scopes affect resources?
**Answer:** A dependency can yield a resource and clean it up after the request. Scope determines when setup and teardown occur; database sessions commonly use request scope.

### Q76. Path, query, header, cookie, and body parameters?
**Answer:** FastAPI infers parameters from their type and position, while `Path`, `Query`, `Header`, `Cookie`, and Pydantic models make intent and constraints explicit. Request bodies are normally used for structured write data.

### Q77. How does response validation work?
**Answer:** A declared `response_model` filters and serializes returned data and documents the response schema. It prevents accidental exposure of fields such as password hashes.

### Q78. What are Pydantic validators?
**Answer:** Validators implement field or model-level rules beyond basic types. Keep validation deterministic and focused on data correctness; authorization and database checks belong in application services.

### Q79. Pydantic model versus ORM model?
**Answer:** Pydantic models describe API input/output. ORM models describe persistence. Separate them to avoid leaking database details and to control which fields clients can write or read.

### Q80. How does OpenAPI generation work?
**Answer:** FastAPI builds an OpenAPI document from routes, type hints, dependencies, request models, and response models. Swagger UI and ReDoc render that schema.

### Q81. How do routers help structure an application?
**Answer:** `APIRouter` groups related endpoints, prefixes, tags, dependencies, and response configuration. Include routers from `main.py` so modules remain independently testable.

### Q82. What is middleware?
**Answer:** Middleware wraps every request and response, making it suitable for logging, timing, headers, CORS, and correlation IDs. Keep it lightweight and avoid hiding business logic there.

### Q83. What is the lifespan event?
**Answer:** Lifespan setup runs when the application starts and teardown runs when it stops. Use it for shared clients, model loading, and pool initialization, with explicit cleanup.

### Q84. How do background tasks work?
**Answer:** Background tasks run after a response in the same application process. They are suitable for short best-effort work; use a durable queue for retries, long jobs, or work that must survive process failure.

### Q85. How do you stream a response?
**Answer:** Return a `StreamingResponse` backed by an iterator or async iterator. Stream large files or generated output without buffering everything, and handle client disconnects and resource cleanup.

## Part 7 — Authentication and Authorization

### Q86. Authentication versus authorization?
**Answer:** Authentication establishes identity; authorization evaluates permissions for a specific action and resource. Both must be enforced server-side.

### Q87. What is OAuth2?
**Answer:** OAuth2 is an authorization framework for delegated access using tokens and scopes. It does not itself prescribe every token format or prove identity; OpenID Connect adds an identity layer.

### Q88. What is a JWT?
**Answer:** A JWT is a signed token containing claims. Its signature protects integrity, not confidentiality; avoid sensitive data in the payload and validate signature, algorithm, issuer, audience, and expiry.

### Q89. Access token versus refresh token?
**Answer:** Access tokens are short-lived credentials sent to APIs. Refresh tokens are longer-lived credentials used to obtain new access tokens and require stronger storage, rotation, revocation, and reuse detection.

### Q90. How should passwords be stored?
**Answer:** Store only slow, salted password hashes using a modern password-hashing algorithm such as Argon2id or bcrypt. Never encrypt or log plaintext passwords.

### Q91. What is least privilege?
**Answer:** Give users, services, tokens, and database roles only the permissions required for their task and duration. Review permissions and default to denial.

### Q92. What are scopes and roles?
**Answer:** Scopes represent granted capabilities, while roles group permissions for users or service identities. Enforce them at the operation and resource level, not only at login.

### Q93. How do you prevent IDOR vulnerabilities?
**Answer:** Do not trust an object ID supplied by the client. Query the object together with the authenticated principal's ownership or permission predicate and return a safe not-found response when unauthorized.

### Q94. How do you secure cookies?
**Answer:** Use `Secure`, `HttpOnly`, and an appropriate `SameSite` policy, set narrow domains and paths, rotate sessions, and use CSRF protection when cookies authenticate browser requests.

### Q95. What is CORS?
**Answer:** CORS is a browser policy controlling cross-origin reads. Configure explicit allowed origins, methods, and headers; wildcard origins cannot be combined safely with credentialed requests.

### Q96. How do you prevent brute force attacks?
**Answer:** Rate-limit login and sensitive endpoints, add progressive delays or challenge mechanisms, monitor failures, and use strong password hashing. Avoid revealing whether a username exists.

### Q97. What is CSRF?
**Answer:** CSRF tricks a browser into sending an authenticated request to a site. SameSite cookies, CSRF tokens, origin checks, and avoiding ambient cookie authentication for APIs reduce the risk.

### Q98. How do you rotate signing keys?
**Answer:** Publish key identifiers, accept current and recently valid keys during a transition, sign new tokens with the new key, and retire old keys after their maximum lifetime and incident window.

### Q99. How do you revoke JWTs?
**Answer:** Stateless access tokens are difficult to revoke individually. Use short expirations, refresh-token rotation and revocation, a token-version check, or a denylist when immediate invalidation is required.

### Q100. How do you handle service-to-service authentication?
**Answer:** Use short-lived workload credentials, mTLS, signed requests, or an identity provider. Validate audience and identity, rotate secrets automatically, and apply authorization per service operation.

## Part 8 — SQLAlchemy and Database Integration

### Q101. What is an ORM?
**Answer:** An ORM maps application objects to relational data and generates SQL. It improves productivity but does not remove the need to understand transactions, indexes, query plans, and SQL behavior.

### Q102. Engine, connection, and session?
**Answer:** The engine manages database connectivity and pooling. A connection represents a database connection. A session tracks ORM identity and transactions and should usually be scoped to a unit of work or request.

### Q103. What is a transaction?
**Answer:** A transaction groups changes atomically under ACID rules. Commit successful work, roll back failures, and keep transaction boundaries aligned with business operations.

### Q104. What is the unit-of-work pattern?
**Answer:** It collects changes and commits them as one logical operation. A service layer can own the transaction while repositories focus on persistence queries.

### Q105. What is lazy loading?
**Answer:** Lazy loading fetches a relationship when accessed. It can cause N+1 queries, so use explicit eager loading or projection for request paths with known data needs.

### Q106. What is eager loading?
**Answer:** Eager loading fetches related data in planned queries, using strategies such as `selectinload` or `joinedload`. Select the strategy based on cardinality and result size.

### Q107. What is the N+1 problem?
**Answer:** One query loads parent rows and an additional query runs for each parent. Detect it with SQL logging or tracing and fix it using joins, batching, eager loading, or a tailored query.

### Q108. How do migrations work?
**Answer:** Migration tools such as Alembic version schema changes as code. Review generated migrations, test upgrade and downgrade where supported, and deploy schema changes in backward-compatible stages.

### Q109. What are indexes?
**Answer:** Indexes accelerate reads for selected access patterns but consume storage and slow writes. Index columns used in filters, joins, and ordering after reviewing query plans.

### Q110. Optimistic versus pessimistic locking?
**Answer:** Optimistic locking detects conflicts using a version or timestamp and retries or rejects stale updates. Pessimistic locking holds database locks during a transaction and is useful when conflicts are likely but increases contention.

### Q111. What are isolation levels?
**Answer:** Isolation controls how transactions observe one another, trading consistency for concurrency. Understand dirty reads, non-repeatable reads, and phantom reads before choosing a level.

### Q112. How do you prevent SQL injection?
**Answer:** Use bound parameters and ORM expression APIs; never concatenate untrusted input into SQL. Validate identifiers separately because parameter binding cannot substitute table or column names.

### Q113. How do connection pools work?
**Answer:** A pool reuses database connections and bounds concurrent connections. Tune pool size, overflow, timeout, and recycle settings to the database and deployment topology.

### Q114. How should sessions be used in FastAPI?
**Answer:** Yield a request-scoped session from a dependency, close it after the request, roll back on errors, and avoid passing it to background tasks after teardown.

### Q115. How do you handle database retries?
**Answer:** Retry only transient connection or serialization failures, with bounded backoff. Ensure the transaction is rolled back and that the operation is safe to repeat.

### Q116. What is pagination at the database layer?
**Answer:** Offset pagination is simple but slows at deep pages and can shift under writes. Keyset or cursor pagination uses stable ordered columns and performs better for large, changing datasets.

### Q117. What is a repository?
**Answer:** A repository encapsulates persistence queries and hides storage mechanics from business logic. Keep it focused; do not turn it into a generic abstraction that obscures important SQL behavior.

### Q118. How do you avoid over-fetching?
**Answer:** Select only required columns and relationships, define response-specific projections, and paginate. Avoid returning ORM entities directly when their full graph is larger than the API contract.

### Q119. How do you handle soft deletes?
**Answer:** Mark rows deleted with metadata and apply a default filter, while providing controlled administrative access to deleted records. Consider uniqueness, foreign keys, retention, and restore semantics.

### Q120. How do you handle database migrations with zero downtime?
**Answer:** Use expand-and-contract migrations: add compatible schema first, deploy code that supports both forms, backfill safely, switch reads/writes, then remove obsolete schema after verification.

## Part 9 — Production Deployment and Performance

### Q121. How do you run FastAPI in production?
**Answer:** Run it behind a process manager and reverse proxy, use multiple appropriately sized workers, configure timeouts and graceful shutdown, and expose health and readiness checks.

### Q122. What is the difference between workers and threads?
**Answer:** Workers are separate processes with isolated memory; threads share a process address space. More workers improve parallelism but multiply memory and connection usage.

### Q123. What is a container health check?
**Answer:** A liveness check determines whether a process should be restarted. A readiness check determines whether it can receive traffic; readiness should include dependencies only when their absence means the service cannot safely serve requests.

### Q124. What is graceful shutdown?
**Answer:** Stop new traffic, finish or cancel in-flight work within a deadline, close pools and clients, and flush telemetry. This prevents dropped requests and corrupted partial work.

### Q125. How do you manage configuration?
**Answer:** Keep configuration outside code, validate it at startup, separate environments, and use a secret manager for credentials. Never commit secrets or print them in logs.

### Q126. What is twelve-factor configuration?
**Answer:** It treats configuration as environment-provided data, separates build from release and run, and keeps processes stateless. Adapt the principles to the deployment platform rather than copying them blindly.

### Q127. How do you improve endpoint performance?
**Answer:** Measure first, then optimize query plans, indexes, external calls, serialization, payload sizes, caching, and connection pools. Avoid blocking the event loop and set realistic deadlines.

### Q128. What is caching?
**Answer:** Caching stores reusable results closer to the consumer. Define keys, TTLs, invalidation, consistency expectations, size bounds, and behavior when the cache is unavailable.

### Q129. What is cache stampede prevention?
**Answer:** When many requests miss simultaneously, use request coalescing, locking, early refresh, jittered TTLs, or stale-while-revalidate behavior so all requests do not recompute at once.

### Q130. What are logs, metrics, and traces?
**Answer:** Logs provide event detail, metrics provide aggregatable measurements, and traces show a request across components. Use structured formats, correlation IDs, useful dimensions, and privacy controls.

### Q131. What should be measured for an API?
**Answer:** Track request rate, error rate, latency percentiles, saturation, dependency latency, queue depth, database pool usage, and resource utilization. Averages alone hide tail latency.

### Q132. What is a circuit breaker?
**Answer:** A circuit breaker opens after repeated downstream failures, fails fast, and periodically tests recovery. It prevents an unhealthy dependency from consuming all local capacity.

### Q133. What is rate limiting?
**Answer:** Rate limiting bounds requests by identity, token, IP, or resource. Token bucket and leaky bucket algorithms support different burst and smoothing behavior; return clear retry information.

### Q134. How do you protect an API from oversized requests?
**Answer:** Limit body size, upload size, pagination limits, nesting depth, and processing time at the proxy and application layers. Reject early before expensive parsing or storage.

### Q135. How do you handle file uploads safely?
**Answer:** Validate size and type by content, generate safe names, store outside executable paths, scan when necessary, stream rather than buffering huge files, and authorize access to every download.

### Q136. How do you deploy secrets safely?
**Answer:** Inject them through a managed secret store or protected runtime configuration, rotate them, restrict access, and prevent values from appearing in logs, images, crash reports, or error responses.

### Q137. What is blue-green deployment?
**Answer:** Two environments are maintained; traffic switches from the old environment to the new after validation. It enables fast rollback but requires compatible data and infrastructure capacity.

### Q138. What is canary deployment?
**Answer:** A small percentage of traffic goes to the new version while metrics and errors are monitored. Increase gradually or roll back automatically when defined thresholds are exceeded.

### Q139. How do you design for horizontal scaling?
**Answer:** Keep API instances stateless, externalize sessions and shared coordination, use idempotent operations, and ensure databases, queues, and caches have capacity and failure strategies.

### Q140. What is a graceful degradation strategy?
**Answer:** Define which features may be disabled or served stale when dependencies fail. Return useful partial responses where safe, fail fast for critical operations, and make degraded mode observable.

## Part 10 — Advanced API Design

### Q141. How do you design REST resources?
**Answer:** Model stable resources and relationships, use HTTP methods and status codes consistently, keep representations explicit, and avoid leaking internal database structure into public contracts.

### Q142. What is idempotency?
**Answer:** An idempotent operation has the same intended effect when repeated. For retriable writes, use an idempotency key stored atomically with the result and reject conflicting reuse.

### Q143. How do you version an API?
**Answer:** Use a clear compatibility policy and version only when a breaking change is necessary. URL, header, or media-type versioning can work; document sunset dates and migration paths.

### Q144. How should API errors be designed?
**Answer:** Return a stable machine-readable code, human-readable message, validation details where safe, and a correlation ID. Do not expose stack traces, SQL, tokens, or internal topology.

### Q145. Offset versus cursor pagination?
**Answer:** Offset pagination is easy and supports jumping to a page but becomes expensive and unstable at depth. Cursor pagination is efficient and stable when based on a unique ordered key.

### Q146. How do you design filtering and sorting?
**Answer:** Whitelist fields and operators, validate values and limits, map public names to safe database expressions, and cap expensive combinations. Never pass arbitrary field names directly to SQL.

### Q147. What is optimistic concurrency control?
**Answer:** The client sends a version or ETag it previously read. The server updates only if the current version matches, returning a conflict when another writer changed the resource.

### Q148. What are ETags and conditional requests?

**Answer:** An ETag identifies a representation. `If-None-Match` can avoid sending an unchanged response, while `If-Match` can prevent lost updates; generate validators from stable resource state.

### Q149. How do webhooks work reliably?

**Answer:** Sign payloads, include an event ID and timestamp, retry with backoff, require consumers to acknowledge quickly, and make consumers idempotent. Provide delivery status and replay controls.

### Q150. WebSockets versus server-sent events?

**Answer:** WebSockets are bidirectional and suit interactive sessions. Server-sent events are simpler one-way server-to-browser streams over HTTP; choose based on directionality, infrastructure, and reconnect requirements.

### Q151. When should you use a message queue?

**Answer:** Use a queue for asynchronous, retryable, burst-buffered, or independently scalable work. Define delivery semantics, idempotency, ordering, visibility timeouts, dead-letter handling, and monitoring.

### Q152. What is eventual consistency?

**Answer:** Replicas or derived views may temporarily differ but converge after updates propagate. Make stale-read behavior explicit and use stronger consistency only where the business operation requires it.

### Q153. How do you design a multi-tenant API?

**Answer:** Resolve tenant identity from a trusted credential, enforce tenant predicates in every data path, isolate caches and queues, and test cross-tenant access negatively. Choose shared-schema, separate-schema, or separate-database isolation deliberately.

### Q154. How do you make an API observable?

**Answer:** Emit structured logs with correlation IDs, RED metrics (rate, errors, duration), traces across dependencies, and domain events where useful. Define alerts around user impact and actionable thresholds.

### Q155. How would you design a production FastAPI service?

**Answer:** Separate routers, schemas, services, repositories, and infrastructure; validate at boundaries; use request-scoped transactions; secure and authorize every resource; add timeouts, retries, rate limits, metrics, traces, and structured logs; and deploy with health checks, migrations, rollback, and graceful shutdown.

## Final preparation checklist

- Explain trade-offs, not only definitions.
- Relate every FastAPI answer to the request lifecycle.
- Practice writing tests and small working examples.
- Discuss failure modes, observability, security, and data consistency for production questions.
