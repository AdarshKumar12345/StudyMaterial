# Python – Complete Language Roadmap

**Goal:** Learn Python from fundamentals to advanced language features, with practical exercises and interview preparation.

## 1. Python Fundamentals
- [ ] What is Python? Features and use cases
- [ ] Installing Python and setting up the environment
- [ ] Python interpreter and execution
- [ ] Running scripts and using the REPL
- [ ] Syntax, indentation, comments
- [ ] Keywords and identifiers
- [ ] Variables and naming conventions
- [ ] Input and output (`input()`, `print()`)
- [ ] Type checking with `type()`
- [ ] Type conversion and casting
- [ ] Dynamic typing
- [ ] Mutable vs immutable objects
- [ ] Object identity, equality, and references

## 2. Data Types and Operators
### Built-in data types
- [ ] Integers and floating-point numbers
- [ ] Complex numbers
- [ ] Strings
- [ ] Booleans
- [ ] `None`
- [ ] Lists
- [ ] Tuples
- [ ] Sets and frozensets
- [ ] Dictionaries
- [ ] Range objects
- [ ] Bytes and bytearray

### Operators
- [ ] Arithmetic operators
- [ ] Comparison operators
- [ ] Logical operators
- [ ] Assignment operators
- [ ] Bitwise operators
- [ ] Membership operators (`in`, `not in`)
- [ ] Identity operators (`is`, `is not`)
- [ ] Operator precedence

## 3. Control Flow
- [ ] `if`, `elif`, and `else`
- [ ] Nested conditions
- [ ] `for` loops
- [ ] `while` loops
- [ ] `range()`
- [ ] `break` and `continue`
- [ ] `pass`
- [ ] Loop `else`
- [ ] `match` / `case`
- [ ] Conditional expressions (ternary operator)

## 4. Strings
- [ ] String creation and indexing
- [ ] String slicing
- [ ] String immutability
- [ ] String methods
- [ ] Searching and replacing
- [ ] Splitting and joining
- [ ] String formatting: f-strings, `format()`
- [ ] Escape characters and raw strings
- [ ] Unicode basics
- [ ] Regular expressions with `re`

## 5. Collections and Data Structures
### Lists
- [ ] Creating, indexing, and slicing lists
- [ ] Adding, updating, and removing elements
- [ ] List methods
- [ ] List copying and aliasing
- [ ] Nested lists
- [ ] List comprehensions

### Tuples
- [ ] Creating and unpacking tuples
- [ ] Tuple immutability
- [ ] Tuple methods
- [ ] Named tuples

### Sets
- [ ] Creating and modifying sets
- [ ] Union, intersection, difference
- [ ] Symmetric difference
- [ ] Set comprehensions
- [ ] Membership testing

### Dictionaries
- [ ] Creating, accessing, and updating dictionaries
- [ ] Dictionary methods
- [ ] Iterating over keys, values, and items
- [ ] Nested dictionaries
- [ ] Dictionary comprehensions
- [ ] Dictionary unpacking

### Useful concepts
- [ ] Slicing
- [ ] Unpacking and starred expressions
- [ ] `enumerate()`
- [ ] `zip()`
- [ ] `sorted()` and `reversed()`
- [ ] `any()` and `all()`
- [ ] `min()`, `max()`, and `sum()`

## 6. Functions
- [ ] Defining and calling functions
- [ ] Parameters and arguments
- [ ] Positional and keyword arguments
- [ ] Default arguments
- [ ] `*args` and `**kwargs`
- [ ] Return values
- [ ] Scope: local, global, and nonlocal
- [ ] LEGB name resolution
- [ ] Docstrings
- [ ] Type hints
- [ ] Lambda functions
- [ ] First-class functions
- [ ] Higher-order functions
- [ ] Closures
- [ ] Recursion
- [ ] Decorators
- [ ] Function annotations

## 7. Modules and Packages
- [ ] Importing modules
- [ ] `import` vs `from ... import`
- [ ] Aliases with `as`
- [ ] Creating custom modules
- [ ] Creating packages
- [ ] `__init__.py`
- [ ] Absolute vs relative imports
- [ ] `__name__ == "__main__"`
- [ ] Python standard library overview
- [ ] Module search path (`sys.path`)
- [ ] Avoiding circular imports

## 8. File Handling
- [ ] Opening and closing files
- [ ] `with` statement and context managers
- [ ] Read, write, and append modes
- [ ] Text vs binary files
- [ ] File cursor and seek
- [ ] Paths with `pathlib`
- [ ] Working with CSV files
- [ ] Working with JSON files
- [ ] Reading and writing files safely
- [ ] Exception handling during file operations

## 9. Exception Handling
- [ ] Syntax and runtime errors
- [ ] `try` and `except`
- [ ] Handling multiple exceptions
- [ ] `else` and `finally`
- [ ] Raising exceptions with `raise`
- [ ] Creating custom exceptions
- [ ] Exception chaining
- [ ] Assertions
- [ ] Best practices for error handling

## 10. Object-Oriented Programming (OOP)
- [ ] Classes and objects
- [ ] `__init__()` constructor
- [ ] Instance attributes and methods
- [ ] Class attributes and methods
- [ ] Static methods
- [ ] `self` and `cls`
- [ ] Encapsulation and properties
- [ ] Inheritance
- [ ] Multiple inheritance
- [ ] Method overriding
- [ ] Polymorphism
- [ ] Abstraction and abstract base classes
- [ ] Composition vs inheritance
- [ ] Method Resolution Order (MRO)
- [ ] `super()`
- [ ] Magic / dunder methods
- [ ] `@property`
- [ ] Dataclasses

## 11. Iterators and Generators
- [ ] Iterable vs iterator
- [ ] `iter()` and `next()`
- [ ] Creating custom iterators
- [ ] Generator functions
- [ ] `yield`
- [ ] Generator expressions
- [ ] `yield from`
- [ ] Lazy evaluation
- [ ] Memory benefits of generators

## 12. Comprehensions and Functional Tools
- [ ] List comprehensions
- [ ] Dictionary comprehensions
- [ ] Set comprehensions
- [ ] Generator expressions
- [ ] `map()`
- [ ] `filter()`
- [ ] `reduce()`
- [ ] `functools`
- [ ] `itertools`
- [ ] `operator`
- [ ] Functional programming concepts

## 13. Advanced Python
- [ ] Decorators in depth
- [ ] Context managers
- [ ] Creating context managers with classes
- [ ] `contextlib`
- [ ] Descriptors
- [ ] Metaclasses (conceptual understanding)
- [ ] Attribute access methods
- [ ] Shallow copy vs deep copy
- [ ] Garbage collection and reference counting
- [ ] Memory management basics
- [ ] Hashability and hash functions
- [ ] Equality and ordering methods
- [ ] `__slots__`
- [ ] Pattern matching
- [ ] Type hints in depth
- [ ] Generics and `typing`
- [ ] Protocols and structural typing

## 14. Concurrency and Asynchronous Programming
- [ ] Processes vs threads
- [ ] Concurrency vs parallelism
- [ ] Threading module
- [ ] Multiprocessing module
- [ ] Global Interpreter Lock (GIL)
- [ ] Thread synchronization and locks
- [ ] Race conditions and deadlocks
- [ ] Futures and executors
- [ ] `async` and `await`
- [ ] Event loop
- [ ] Coroutines and tasks
- [ ] `asyncio`
- [ ] Async context managers and iterators
- [ ] When to use threading, multiprocessing, or asyncio

## 15. Python Environment and Package Management
- [ ] `pip`
- [ ] Virtual environments with `venv`
- [ ] `requirements.txt`
- [ ] `pyproject.toml`
- [ ] Package installation and version pinning
- [ ] Dependency management
- [ ] Editable installs
- [ ] Environment variables
- [ ] `pipx` and tools overview
- [ ] Code formatting and linting tools

## 16. Testing and Debugging
- [ ] Debugging with `pdb`
- [ ] Logging with `logging`
- [ ] Unit testing with `unittest`
- [ ] pytest fundamentals
- [ ] Fixtures
- [ ] Parametrized tests
- [ ] Mocking and patching
- [ ] Test coverage
- [ ] Integration testing basics
- [ ] Assertions and test organization

## 17. Standard Library Essentials
- [ ] `os` and `sys`
- [ ] `pathlib`
- [ ] `datetime` and `time`
- [ ] `math` and `random`
- [ ] `collections`
- [ ] `itertools` and `functools`
- [ ] `re`
- [ ] `json` and `csv`
- [ ] `logging`
- [ ] `argparse`
- [ ] `subprocess`
- [ ] ` shutil`
- [ ] `sqlite3`
- [ ] `typing`
- [ ] `dataclasses`
- [ ] `enum`

## 18. Database Connectivity
- [ ] Relational database basics
- [ ] Connect Python to PostgreSQL / MySQL
- [ ] SQL queries from Python
- [ ] DB-API concepts
- [ ] Connection management
- [ ] Transactions and commits
- [ ] SQLAlchemy fundamentals
- [ ] ORM vs raw SQL
- [ ] Migrations with Alembic
- [ ] MongoDB basics with PyMongo
- [ ] Connection pooling
- [ ] Prevent SQL injection with parameterized queries

## 19. Coding Practice and Problem Solving
- [ ] Number-based problems
- [ ] String manipulation problems
- [ ] List and dictionary problems
- [ ] Set-based problems
- [ ] Sorting and searching
- [ ] Hash maps and frequency counting
- [ ] Stack and queue implementation
- [ ] Linked lists, trees, and graphs
- [ ] Recursion and backtracking
- [ ] Time and space complexity
- [ ] Solve problems using Pythonic approaches

## 20. Interview Preparation
- [ ] Mutable vs immutable objects
- [ ] List vs tuple
- [ ] Set vs dictionary
- [ ] `==` vs `is`
- [ ] Shallow copy vs deep copy
- [ ] List comprehension vs generator expression
- [ ] Iterator vs generator
- [ ] `*args` vs `**kwargs`
- [ ] Decorators and closures
- [ ] OOP principles in Python
- [ ] MRO and multiple inheritance
- [ ] GIL and concurrency
- [ ] Threading vs multiprocessing vs asyncio
- [ ] Exception handling best practices
- [ ] Memory management and garbage collection
- [ ] `__new__()` vs `__init__()`
- [ ] `@staticmethod` vs `@classmethod` vs instance method
- [ ] Context managers
- [ ] Type hints and dataclasses

## Recommended Learning Order

| Priority | Topics |
|---|---|
| P0 – Core | Fundamentals, data types, control flow, strings, collections, functions |
| P0 – Core | Modules, file handling, exceptions, OOP |
| P1 – Intermediate | Iterators, generators, comprehensions, decorators, standard library |
| P1 – Intermediate | Virtual environments, testing, debugging, databases |
| P2 – Advanced | Asyncio, threading, multiprocessing, memory management, descriptors, metaclasses |

## Mini Projects

- [ ] Command-line calculator
- [ ] Number guessing game
- [ ] Contact book using dictionaries and JSON
- [ ] File organizer using `pathlib`
- [ ] Expense tracker with CSV / SQLite
- [ ] Student management system using OOP
- [ ] REST API using FastAPI
- [ ] Async web scraper using asyncio and HTTPX
- [ ] Task queue / background worker
- [ ] Small database-backed application

## Quick Revision

- **List:** Ordered, mutable collection.
- **Tuple:** Ordered, immutable collection.
- **Set:** Collection of unique hashable elements.
- **Dictionary:** Key-value mapping.
- **Decorator:** Callable that wraps or modifies another callable.
- **Generator:** Iterator that produces values lazily using `yield`.
- **Iterator:** Object that returns values one at a time.
- **Context Manager:** Manages setup and cleanup, commonly with `with`.
- **GIL:** CPython mechanism affecting parallel execution of Python bytecode in threads; behavior is evolving, so check the Python version.
- **Coroutine:** Function defined with `async def` that can suspend and resume.

## Official Resources

- Python Tutorial: https://docs.python.org/3/tutorial/
- Python Language Reference: https://docs.python.org/3/reference/
- Python Standard Library: https://docs.python.org/3/library/
- Python Packaging Guide: https://packaging.python.org/
- pytest: https://docs.pytest.org/
- Real Python: https://realpython.com/
