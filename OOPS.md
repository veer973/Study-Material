# Object-Oriented Programming (OOP) in Python
### Comprehensive Placement & Interview Preparation Guide

---

## Table of Contents
1. [Introduction to OOP & Core Concepts](#1-introduction-to-oop--core-concepts)
   - [What is OOP?](#what-is-oop)
   - [Python's Object Model](#pythons-object-model)
   - [The Four Pillars of OOP (Overview)](#the-four-pillars-of-oop-overview)
   - [Classes and Objects](#classes-and-objects)
   - [Object Literal vs Class](#object-literal-vs-class)
2. [Python Constructors, `self`, and Dunder Methods](#2-python-constructors-self-and-dunder-methods)
   - [What is a Constructor?](#what-is-a-constructor)
   - [Why is the Constructor Called `__init__`?](#why-is-the-constructor-called-__init__)
   - [What is `self`?](#what-is-self)
   - [Why Does Every Instance Method Need `self`?](#why-does-every-instance-method-need-self)
   - [What Belongs Inside a Constructor?](#what-belongs-inside-a-constructor)
   - [Magic / Dunder Methods](#magic--dunder-methods)
3. [Instance Variables, Encapsulation & Access Modifiers](#3-instance-variables-encapsulation--access-modifiers)
   - [Instance Variables](#instance-variables)
   - [Access Control in Python (Public, Protected, Private)](#access-control-in-python-public-protected-private)
   - [Name Mangling & Why Nothing is Truly Private](#name-mangling--why-nothing-is-truly-private)
   - [Encapsulation in Depth](#encapsulation-in-depth)
   - [Getters and Setters](#getters-and-setters)
   - [Controlled Access vs Direct Access](#controlled-access-vs-direct-access)
   - [The `@property` Decorator (Pythonic Getters & Setters)](#the-property-decorator-pythonic-getters--setters)
4. [Python Memory Model, Mutability & Parameter Passing](#4-python-memory-model-mutability--parameter-passing)
   - [Core Data Collections Overview (`list`, `tuple`, `set`, `dict`)](#core-data-collections-overview-list-tuple-set-dict)
   - [Hashability and Set Requirements](#hashability-and-set-requirements)
   - [Tuples with Mutable Elements](#tuples-with-mutable-elements)
   - [Object Mutability vs Variable Reassignment](#object-mutability-vs-variable-reassignment)
   - [Object Identity and `id()`](#object-identity-and-id)
   - [Reference Values and Aliasing](#reference-values-and-aliasing)
   - [Pass-by-Object-Reference (Call by Sharing)](#pass-by-object-reference-call-by-sharing)
   - [Passing and Returning Objects](#passing-and-returning-objects)
   - [Mutable vs Immutable Objects Summary](#mutable-vs-immutable-objects-summary)
5. [Class Variables, Static Methods & Class Methods](#5-class-variables-static-methods--class-methods)
   - [Static Variables (Class Variables)](#static-variables-class-variables)
   - [Static Methods (`@staticmethod`)](#static-methods-staticmethod)
   - [Class Methods (`@classmethod`)](#class-methods-classmethod)
   - [Alternative Constructors Using `@classmethod`](#alternative-constructors-using-classmethod)
   - [Comparison: Instance vs Class vs Static Methods](#comparison-instance-vs-class-vs-static-methods)
6. [Object Relationships: Association, Aggregation & Composition](#6-object-relationships-association-aggregation--composition)
   - [Association (General Relationship)](#association-general-relationship)
   - [Aggregation (Weak HAS-A)](#aggregation-weak-has-a)
   - [Composition (Strong HAS-A)](#composition-strong-has-a)
   - [Comparison: Inheritance vs Aggregation vs Composition](#comparison-inheritance-vs-aggregation-vs-composition)
7. [Inheritance and Method Resolution Order (MRO)](#7-inheritance-and-method-resolution-order-mro)
   - [Inheritance Basics (IS-A Relationship)](#inheritance-basics-is-a-relationship)
   - [What Can Be Inherited in Python?](#what-can-be-inherited-in-python)
   - [Private Members in Inheritance & Name Mangling](#private-members-in-inheritance--name-mangling)
   - [Constructor Inheritance & `super().__init__()`](#constructor-inheritance--super__init__)
   - [Types of Inheritance](#types-of-inheritance)
   - [MRO (Method Resolution Order) & C3 Linearization](#mro-method-resolution-order--c3-linearization)
   - [Method & Constructor Resolution in Multiple Inheritance](#method--constructor-resolution-in-multiple-inheritance)
8. [Polymorphism](#8-polymorphism)
   - [What is Polymorphism?](#what-is-polymorphism)
   - [Method Overriding](#method-overriding)
   - [Method Overloading in Python (Why Traditional Overloading Doesn't Exist)](#method-overloading-in-python-why-traditional-overloading-doesnt-exist)
   - [Operator Overloading & Dunder Methods](#operator-overloading--dunder-methods)
   - [The `super()` Keyword](#the-super-keyword)
   - [Duck Typing (Behavior Over Type)](#duck-typing-behavior-over-type)
9. [Abstraction & Interfaces](#9-abstraction--interfaces)
   - [What is Abstraction?](#what-is-abstraction)
   - [Abstract Base Classes (ABCs) & `@abstractmethod`](#abstract-base-classes-abcs--abstractmethod)
   - [Concrete Methods in Abstract Classes](#concrete-methods-in-abstract-classes)
   - [Abstraction vs Encapsulation](#abstraction-vs-encapsulation)
   - [Interface vs Abstract Class](#interface-vs-abstract-class)
10. [Type Inspection & Built-in Object Hierarchy](#10-type-inspection--built-in-object-hierarchy)
    - [`isinstance()` Function](#isinstance-function)
    - [`issubclass()` Function](#issubclass-function)
    - [Comparison: `isinstance()` vs `issubclass()`](#comparison-isinstance-vs-issubclass)
    - [The Root `object` Class](#the-root-object-class)
    - [`__str__` vs `__repr__`](#__str__-vs-__repr__)
11. [Integrated Real-World Design Example](#11-integrated-real-world-design-example)
    - [Architecture Overview](#architecture-overview)
    - [Complete Implementation (Checkout & Payment Processing)](#complete-implementation-checkout--payment-processing)
    - [How All Concepts Connect](#how-all-concepts-connect)
12. [Placement & Interview Quick Reference](#12-placement--interview-quick-reference)
    - [Comprehensive Placement Revision Table](#comprehensive-placement-revision-table)
    - [Most Important Interview Differences](#most-important-interview-differences)
    - [29 Essential Interview Questions Checklist](#29-essential-interview-questions-checklist)

---

## 1. Introduction to OOP & Core Concepts

### What is OOP?
**Object-Oriented Programming (OOP)** is a programming paradigm and architectural style that organizes software design around **objects and classes**, rather than strictly around procedural sequences of functions or execution steps.

OOP can be understood conceptually as moving from **general concepts (blueprints)** to **specific implementations (concrete instances)**.

### Python's Object Model
In Python, **almost everything is an object**, including integers, floating-point numbers, strings, functions, modules, and classes themselves. 

> [!NOTE]
> While the phrase *"everything is an object in Python"* is commonly used in interviews, technically a few low-level grammar/language constructs (such as keywords like `if`, `while`, `def`, `pass`) are not objects. However, all data types, user-defined functions, and classes are first-class objects in memory.

### The Four Pillars of OOP (Overview)

| Pillar | Definition | Core Goal |
| :--- | :--- | :--- |
| **Encapsulation** | Bundling data (attributes) and methods together while restricting direct access to internal state. | Data security & state integrity |
| **Abstraction** | Hiding unnecessary low-level implementation details and exposing only the essential, high-level interface. | Managing complexity |
| **Inheritance** | Creating a new class based on an existing class to reuse, customize, and extend properties and behaviors. | Code reusability & extensibility (`IS-A`) |
| **Polymorphism** | Allowing the same unified interface or method call to behave differently depending on the underlying object or context. | Flexibility & loose coupling |

### Classes and Objects

#### Class
A **class** is a blueprint/template that defines:
1. **Data / Properties (Attributes)**: State that describes the object.
2. **Functions / Behaviors (Methods)**: Operations that define what the object can do.

**Example:**
* Class: `Car`
* Properties: `color`, `speed`, `fuel_level`
* Behaviors: `start()`, `brake()`, `accelerate()`

#### Object
An **object** is an instantiated, concrete realization of a class allocated in memory.

```python
class Car:
    def __init__(self, color, speed):
        self.color = color
        self.speed = speed

    def start(self):
        print(f"The {self.color} car started.")

# Creating objects (instances of Car)
car1 = Car("red", 120)
car2 = Car("blue", 150)

car1.start()  # The red car started.
```

### Object Literal vs Class

An **object literal** creates an object directly using inline literal syntax (such as dictionary `{}` or list `[]`), without explicitly defining a class blueprint first.

#### Using Object Literal:
```python
student = {
    "name": "Rahul",
    "age": 21
}
```

#### Using Class and Instance:
```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age

student = Student("Rahul", 21)
```

> [!TIP]
> **Summary:**
> * **Object Literal**: Directly construct an object in memory via literals (`{}`, `[]`, etc.). Useful for lightweight, ad-hoc key-value data.
> * **Class**: Define an explicit blueprint first, encapsulating state and associated behaviors, then instantiate one or more objects from it.

---

## 2. Python Constructors, `self`, and Dunder Methods

### What is a Constructor?
A **constructor** is a special method automatically invoked when a new object is instantiated. Its primary responsibility is to **initialize the object's attributes and set up its initial state**.

```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age

s1 = Student("Rahul", 21)
```
When `Student("Rahul", 21)` executes, Python automatically triggers `__init__()`.

### Why is the Constructor Called `__init__`?
`__init__` stands for **initialize**. It is a predefined dunder method called automatically right after the object has been allocated in memory.

> [!IMPORTANT]
> **Placement Nuance:**
> * `__new__()` actually **creates and allocates** the object in memory (it is the true constructor).
> * `__init__()` **initializes** the freshly created object's state (it acts as the initializer).
> * For placement interviews, `__init__()` is conventionally referred to as the constructor.

### What is `self`?
`self` represents the **current object / instance**. It acts as a reference through which an object accesses its own attributes and methods.

```python
class Student:
    def __init__(self, name):
        self.name = name

s1 = Student("Rahul")
s2 = Student("Amit")
```

* `s1.name` $\rightarrow$ `"Rahul"`
* `s2.name` $\rightarrow$ `"Amit"`
* Inside the class definition, `self.name` means: *the attribute `name` belonging specifically to whichever object is currently invoking this method.*

### Why Does Every Instance Method Need `self`?
Unlike Java or C++, Python does **not provide an implicit `this` pointer**. 

Instead, whenever an instance method is called on an object, Python automatically passes the invoking instance as the **first positional argument**.

```python
class Student:
    def show(self):
        print(self.name)

s1 = Student("Rahul")
s1.show()
```

Under the hood, when you invoke `s1.show()`, Python transforms the call into:
```python
Student.show(s1)
```
Therefore, `self` explicitly informs the method **which specific object's data it should work with**.

> [!NOTE]
> `self` is not a reserved Python language keyword; it is a universal community naming convention. Technically, you could name it `this` or `obj`, but you should **always use `self`** in production and interviews.

### What Belongs Inside a Constructor?
The constructor should strictly contain **initialization logic** required to put the object into a valid starting state:
* Initializing instance attributes
* Setting default property values
* Validating constructor arguments
* Initializing data structures or preparing required resources

```python
class BankAccount:
    def __init__(self, name, balance):
        if balance < 0:
            raise ValueError("Initial balance cannot be negative")
        self.name = name
        self.balance = balance
        self.is_active = True
```

> [!WARNING]
> Avoid putting heavy business workflows, complex mathematical algorithms, or unrelated blocking I/O inside `__init__()`. Keep it dedicated to state initialization.

### Magic / Dunder Methods
**Dunder** stands for **Double Underscore** (`__<name>__`). These are predefined special hook methods that Python automatically invokes in response to specific operations, syntax constructs, or built-in functions.

```python
class Student:
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return f"Student: {self.name}"

s = Student("Rahul")
print(s)  # Calls s.__str__() under the hood -> "Student: Rahul"
```

#### Core Dunder Methods Cheat-Sheet:

| Dunder Method | Trigger / Operation | Purpose |
| :--- | :--- | :--- |
| `__new__(cls, ...)` | `ClassName(...)` | Allocates and returns the new raw instance |
| `__init__(self, ...)` | `ClassName(...)` | Initializes the freshly created instance |
| `__str__(self)` | `str(obj)`, `print(obj)` | Returns an informal, readable string representation |
| `__repr__(self)` | `repr(obj)`, Interactive REPL | Returns an unambiguous, formal representation for developers/debugging |
| `__len__(self)` | `len(obj)` | Returns collection length |
| `__add__(self, other)` | `obj1 + obj2` | Overloads addition operator `+` |
| `__sub__(self, other)` | `obj1 - other` | Overloads subtraction operator `-` |
| `__eq__(self, other)` | `obj1 == obj2` | Overloads equality operator `==` |
| `__lt__(self, other)` | `obj1 < obj2` | Overloads less-than operator `<` |
| `__gt__(self, other)` | `obj1 > obj2` | Overloads greater-than operator `>` |

> [!TIP]
> **One-Line Revision:**
> * `__init__` $\rightarrow$ initializes object state.
> * `self` $\rightarrow$ points to the current instance.
> * Dunder methods $\rightarrow$ special methods automatically called by Python for built-in operations and operator protocols.

---

## 3. Instance Variables, Encapsulation & Access Modifiers

### Instance Variables
An **instance variable** is an attribute tied to a specific object instance. Every object created from a class maintains its own distinct, isolated copy of instance variables.

```python
class Student:
    def __init__(self, name, age):
        self.name = name  # Instance variable
        self.age = age    # Instance variable

s1 = Student("Rahul", 20)
s2 = Student("Amit", 22)

# Modifying s1 has zero impact on s2
s1.name = "Rahul Sharma"
print(s1.name)  # Rahul Sharma
print(s2.name)  # Amit
```

### Access Control in Python (Public, Protected, Private)
Python **does not have strict language-enforced access keywords** like `public`, `protected`, or `private` in C++ or Java. Instead, Python relies on **naming conventions** and **name mangling**.

```
                   Python Access Modifiers
        ┌───────────────────┬───────────────────┐
     Public              Protected           Private
    self.var            self._var          self.__var
  (Unrestricted)      (Internal Use       (Name Mangles to
                       Convention)       _Class__var)
```

#### 1. Public Attributes (`self.variable`)
Accessible directly from anywhere:
```python
class Bank:
    def __init__(self):
        self.balance = 1000

b = Bank()
print(b.balance)  # 1000 - direct access allowed
```

#### 2. Protected Attributes (`self._variable`)
Prefixed with a **single underscore `_`**.
* **Meaning:** *"This member is internal/protected. Please do not access or modify it directly from outside the class or subclass."*
* **Enforcement:** **Pure convention**. Python does not prevent external access.

```python
class Bank:
    def __init__(self):
        self._balance = 1000

b = Bank()
print(b._balance)  # Still executes, but violates convention!
```

#### 3. Private Attributes (`self.__variable`)
Prefixed with **double leading underscores `__`** (without trailing underscores).
* **Meaning:** Intended to be private to the declaring class.
* **Mechanism:** Python automatically activates **Name Mangling**.

```python
class Bank:
    def __init__(self):
        self.__pin = 1234

b = Bank()
# print(b.__pin)  # Raises AttributeError: 'Bank' object has no attribute '__pin'
```

### Name Mangling & Why Nothing is Truly Private
When Python encounters `__variable` inside a class named `Bank`, it internally rewrites its identifier to:
$$\text{\_ClassName\_\_variable} \implies \text{\_Bank\_\_pin}$$

Because of this rewriting, you can still technically access it from outside:
```python
print(b._Bank__pin)  # Outputs: 1234
```

> [!IMPORTANT]
> **Placement Crucial Takeaway:**
> In Python, **nothing is truly private**. Python's philosophy is *"we are all consenting adults here"*. Name mangling is primarily designed to prevent accidental namespace collisions in inheritance hierarchies, rather than acting as an impenetrable security barrier.

### Encapsulation in Depth
**Encapsulation** is the mechanism of bundling data (attributes) and the operations acting on that data (methods) within a single self-contained unit (class), while **controlling and mediating access** to the internal state.

```python
class BankAccount:
    def __init__(self, pin):
        self.__pin = pin  # Private internal state

    def get_pin(self):    # Getter: controlled read access
        return self.__pin

    def set_pin(self, pin):  # Setter: controlled write access with validation
        if len(str(pin)) == 4 and str(pin).isdigit():
            self.__pin = int(pin)
        else:
            print("Invalid PIN: Must be exactly 4 digits")
```

### Getters and Setters
* **Getter**: A method whose purpose is to inspect or retrieve an attribute value.
* **Setter**: A method whose purpose is to mutate or update an attribute value under controlled validation rules.

```python
account = BankAccount(1234)
print(account.get_pin())  # 1234

account.set_pin(99)       # Invalid PIN: Must be exactly 4 digits (Rejected)
account.set_pin(5678)     # Successfully updated
print(account.get_pin())  # 5678
```

### Controlled Access vs Direct Access

#### Direct Access (No Encapsulation):
```
Outside Code ───────────────► Attribute (Unrestricted / Vulnerable to invalid state)
```

#### Encapsulated Access via Getter/Setter:
```
Outside Code ──► Getter / Setter ──► Attribute
                       │
             ┌─────────┴─────────┐
             │ • Input Validation│
             │ • Access Control  │
             │ • Business Rules  │
             │ • Audit Logging   │
             └───────────────────┘
```

> [!TIP]
> **Why use Getters and Setters instead of direct access?**
> If you allow `account.pin = -999`, the object's integrity is destroyed. Getters and setters introduce an intermediary validation layer that enforces business rules, logging, security checks, and data transformations. Furthermore, a getter can mask sensitive data (e.g., returning `"****"` instead of plain-text passwords/PINs).

### The `@property` Decorator (Pythonic Getters & Setters)
Writing explicit `get_age()` and `set_age()` methods feels unpythonic. Python offers the `@property` decorator to expose getter and setter methods with clean, intuitive attribute-access syntax.

```python
class Student:
    def __init__(self, age):
        self._age = age

    @property
    def age(self):
        """Getter for age."""
        return self._age

    @age.setter
    def age(self, value):
        """Setter for age with validation."""
        if value < 0:
            raise ValueError("Age cannot be negative.")
        self._age = value

# Usage:
s = Student(20)
print(s.age)    # Accesses getter without parentheses -> 20

s.age = 21      # Triggers setter seamlessly
print(s.age)    # 21

# s.age = -5    # Raises ValueError: Age cannot be negative.
```

> [!NOTE]
> * `@property` $\rightarrow$ marks the method as a property getter.
> * `@<property_name>.setter` $\rightarrow$ marks the corresponding property setter.
> * This maintains backward compatibility if a previously public attribute needs validation later on without breaking the caller's `obj.attribute` syntax.

---

## 4. Python Memory Model, Mutability & Parameter Passing

### Core Data Collections Overview (`list`, `tuple`, `set`, `dict`)

| Collection | Syntax | Mutability | Ordering | Duplicates Allowed? | Element Requirements |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **List** | `[1, 2, 3]` | **Mutable** | Ordered | **Yes** | Any object |
| **Tuple** | `(1, 2, 3)` | **Immutable** | Ordered | **Yes** | Any object |
| **Set** | `{1, 2, 3}` | **Mutable** | Unordered | **No** (Unique only) | Must be **Hashable** |
| **Dictionary** | `{"k": "v"}` | **Mutable** | Preserves insertion order | Keys: **Unique**; Values: **Any** | Keys must be **Hashable** |

### Hashability and Set Requirements
It is a common misconception that set elements must strictly be "immutable". The precise requirement is that every element stored in a set must be **hashable**.

* An object is **hashable** if it has a hash value that remains constant during its lifetime (implements `__hash__()`) and can be compared to other objects (implements `__eq__()`).
* Most built-in immutable types (`int`, `float`, `str`, `bool`, and `tuple` containing only hashable items) are hashable.
* Mutable containers (`list`, `dict`, `set`) are unhashable because their contents—and thus their hashes—can change.

```python
# Valid: integers, strings, and tuples of hashable items
valid_set = {1, "hello", (10, 20)}

# Invalid: List is mutable and unhashable
# invalid_set = {[1, 2], 3}  # Raises TypeError: unhashable type: 'list'

# frozenset is an immutable, hashable set variant:
froz = frozenset([1, 2, 3])
nested_set = {froz, 4}  # Valid!
```

### Tuples with Mutable Elements
Although a tuple itself is immutable, the objects referenced inside it can be mutable.

```python
t = ([1, 2], 3)
# t[0] = [10, 20]  # Raises TypeError: 'tuple' object does not support item assignment

t[0].append(99)    # Completely VALID!
print(t)           # ([1, 2, 99], 3)
```

> [!IMPORTANT]
> **Key Insight:** Tuple immutability guarantees that the **references** held by the tuple cannot be modified, added, or replaced. It does **not** guarantee that the underlying objects pointed to by those references cannot change their internal state.

### Object Mutability vs Variable Reassignment
In Python, integers (`int`) and strings (`str`) are strictly immutable. When you modify an integer or string variable, the object itself does not change; instead, the variable name is rebound to a brand new object in memory.

```python
s = "hello"
s = "world"
```

```
Step 1:  s ─────────► "hello" object (Memory Addr A)

Step 2:  s ───┐     "hello" object (Unchanged)
              │
              └─────► "world" object (Memory Addr B)
```

Distinguish clearly between:
* **Variable**: A name/label pointing to an object in memory.
* **Object**: The actual allocated memory block containing the data and type descriptor.

### Object Identity and `id()`
The built-in `id(obj)` function returns the unique integer identity (memory address in CPython) of an object.

```python
s = "hello"
print(id(s))   # e.g., 2045182930112

s = "world"
print(id(s))   # Distinct address, e.g., 2045182938496
```

#### Small Integer Interning:
Python optimizes memory by interning small integers (typically from $-5$ to $256$) and certain string literals:
```python
a = 10
b = 10
print(a is b)      # True (both point to the identical shared integer object)
print(id(a) == id(b))  # True
```

### Reference Values and Aliasing
When you assign one variable to another, Python does **not copy the object**. It copies the **reference (pointer)**.

```python
class Student:
    def __init__(self, name):
        self.name = name

s1 = Student("Rahul")
s2 = s1  # Aliasing: s2 now references the exact same object as s1

s2.name = "Amit"
print(s1.name)  # Amit (Because both point to the same instance!)
```

```
s1 ──────┐
         ▼
    [Student Object: name = "Amit"]
         ▲
s2 ──────┘
```

### Pass-by-Object-Reference (Call by Sharing)
Python uses neither traditional *pass-by-value* nor traditional *pass-by-reference*. Python uses **pass-by-object-reference** (also known as **call-by-sharing**).

When an argument is passed to a function:
1. The function parameter receives a copy of the **object reference**.
2. If the object is **mutable**, modifying it via method calls (e.g., `.append()`) modifies the caller's object directly.
3. If the parameter variable is **reassigned**, the local reference is rebound to a new object, leaving the caller's original variable completely untouched.

#### Case 1: Mutating a Mutable Object Through Reference
```python
def modify_list(numbers):
    numbers.append(4)  # Mutates the caller's list in place

nums = [1, 2, 3]
modify_list(nums)
print(nums)  # [1, 2, 3, 4]
```

#### Case 2: Reassigning the Parameter Reference
```python
def rebind_list(numbers):
    numbers = [10, 20]  # Local name 'numbers' re-bound to a new list object

nums = [1, 2, 3]
rebind_list(nums)
print(nums)  # [1, 2, 3] (Caller's variable remains unchanged)
```

### Passing and Returning Objects
In Python, objects are first-class citizens. They can be:
* Passed as function arguments
* Returned from functions/methods
* Bound to variables
* Stored inside lists, tuples, or dictionaries

```python
class Student:
    def __init__(self, name):
        self.name = name

def display_student(s):
    print(f"Student Name: {s.name}")

def student_factory(name):
    return Student(name)

stud = student_factory("Neha")
display_student(stud)
```

### Mutable vs Immutable Objects Summary

```
                      Python Objects
           ┌─────────────────┴─────────────────┐
        Mutable                             Immutable
   • list                              • int, float, bool
   • dict                              • str
   • set                               • tuple
   • Custom class instances            • frozenset
```

> [!TIP]
> **Placement Memory Hook:**
> 1. Python passes object references by value (call by sharing).
> 2. You can mutate mutable objects in-place through their references.
> 3. Reassigning a function parameter will never reassign the caller's outer variable.

---

## 5. Class Variables, Static Methods & Class Methods

### Static Variables (Class Variables)
In Python, a static variable is called a **class variable**. It is declared directly inside the class body (outside any method) and is **shared across all instances** of that class.

```python
class Student:
    college = "VIT"  # Class / Static Variable (Shared)

    def __init__(self, name):
        self.name = name  # Instance Variable (Unique per object)

s1 = Student("Rahul")
s2 = Student("Amit")

print(s1.college)        # VIT
print(s2.college)        # VIT
print(Student.college)   # VIT (Recommended: access via Class name)
```

> [!WARNING]
> If you execute `s1.college = "IIT"`, Python does **not** change the shared class variable. Instead, it creates a new **instance attribute** on `s1` that shadows the class variable for `s1` only! To update the shared class variable for all instances, update it directly on the class: `Student.college = "IIT"`.

### Static Methods (`@staticmethod`)
A **static method** is a method that behaves like a regular standalone utility function, but lives within the class's namespace for logical organization.
* Decorated with `@staticmethod`.
* Does **not** take `self` (no instance binding) and does **not** take `cls` (no class binding).
* Cannot inspect or modify instance state or class state directly.

```python
class Calculator:
    @staticmethod
    def add(a, b):
        return a + b

print(Calculator.add(10, 20))  # 30
```

> [!IMPORTANT]
> **Interview Correction:**
> Never say: *"A static object does not need `self`."*
> Always say: *"A static method does not need `self` because it is not bound to a particular object instance."*

### Class Methods (`@classmethod`)
A **class method** is bound to the class itself rather than any individual object.
* Decorated with `@classmethod`.
* Automatically receives the class object as its first parameter, conventionally named `cls`.
* Can access and modify class-level attributes.

```python
class Student:
    college = "VIT"

    @classmethod
    def change_college(cls, new_college):
        cls.college = new_college

Student.change_college("IIT")
print(Student.college)  # IIT
```

### Alternative Constructors Using `@classmethod`
The most prominent real-world use case for `@classmethod` is creating **alternative constructors** (factory methods):

```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod
    def from_string(cls, data_str):
        """Alternative constructor parsing a CSV string."""
        name, age = data_str.split(",")
        return cls(name.strip(), int(age.strip()))

# Standard instantiation:
s1 = Student("Rahul", 21)

# Alternative instantiation via factory class method:
s2 = Student.from_string("Amit, 22")
print(s2.name, s2.age)  # Amit 22
```

### Comparison: Instance vs Class vs Static Methods

| Feature | Instance Method | Class Method (`@classmethod`) | Static Method (`@staticmethod`) |
| :--- | :--- | :--- | :--- |
| **First Parameter** | `self` (current object) | `cls` (current class) | None |
| **Binding** | Bound to object instance | Bound to the class | Independent utility in class namespace |
| **Can Access Instance State?** | **Yes** (`self.attribute`) | No | No |
| **Can Access Class State?** | **Yes** (`self.__class__.var`) | **Yes** (`cls.var`) | Only by explicitly referencing `ClassName.var` |
| **Primary Use Case** | Standard business logic operating on object state | Factory methods / Alternative constructors, modifying class state | Self-contained utility or helper algorithms |
| **Call Syntax** | `obj.method()` | `ClassName.method()` or `obj.method()` | `ClassName.method()` or `obj.method()` |

```python
class Example:
    x = 100

    def instance_method(self):
        return f"Instance method called with {self}"

    @classmethod
    def class_method(cls):
        return f"Class method called with {cls}, x = {cls.x}"

    @staticmethod
    def static_method():
        return "Static method called (no self, no cls)"
```

---

## 6. Object Relationships: Association, Aggregation & Composition

```
                        Object Relationships
         ┌───────────────────────┴───────────────────────┐
    Inheritance                                     Association
      (IS-A)                                   (General Relationship)
                                            ┌───────────┴───────────┐
                                       Aggregation              Composition
                                       (Weak HAS-A)            (Strong HAS-A)
                                    Independent Lifecycles   Dependent Lifecycles
```

### Association (General Relationship)
**Association** is the most general relationship between two separate classes where they interact or use each other, but have **no ownership** and completely independent lifecycles.

* **Example:** `Teacher` teaches `Student`.
* If the `Teacher` leaves, the `Student` continues to exist. If the `Student` graduates, the `Teacher` continues to exist.

### Aggregation (Weak HAS-A)
**Aggregation** represents a **weak "HAS-A" relationship** where one container object contains or holds references to other objects, but the contained objects have an **independent lifecycle**.

* If the container is destroyed, the contained object **still exists**.
* The contained object is typically instantiated externally and passed into the container via constructor or method.

```python
class Professor:
    def __init__(self, name):
        self.name = name

class Department:
    def __init__(self, professor):
        self.professor = professor  # Professor passed in from outside

prof = Professor("Dr. Sharma")
dept = Department(prof)

# If the department object is deleted:
del dept
# prof still exists in memory!
print(prof.name)  # Dr. Sharma
```

### Composition (Strong HAS-A)
**Composition** represents a **strong "HAS-A" relationship** where the container object has **strong ownership** of the contained object.

* The contained object's lifecycle is strictly tied to the container.
* If the container is destroyed, the contained object is destroyed along with it.
* The contained object is typically instantiated directly inside the container's constructor.

```python
class Engine:
    def start(self):
        print("Engine running...")

class Car:
    def __init__(self):
        self.engine = Engine()  # Engine is created inside Car

    def start_car(self):
        self.engine.start()

car = Car()
car.start_car()  # Engine running...
# If 'car' is deleted and garbage collected, its internal 'engine' is deleted too.
```

### Comparison: Inheritance vs Aggregation vs Composition

| Feature | Inheritance | Aggregation | Composition |
| :--- | :--- | :--- | :--- |
| **Relationship** | **IS-A** | **HAS-A** (Weak) | **HAS-A** (Strong) |
| **Coupling** | Very tight (child bound to parent) | Loose | Moderate to tight |
| **Ownership** | N/A (Hierarchical derivation) | Shared / No exclusive ownership | Exclusive ownership |
| **Lifecycle** | Unified object instance | Contained object survives container deletion | Contained object dies with container |
| **Example** | `Dog IS-A Animal` | `Department HAS-A Professor` | `Car HAS-A Engine` |

> [!TIP]
> **Placement Memory Formula:**
> * `Inheritance` $\rightarrow$ **IS-A**
> * `Aggregation` $\rightarrow$ **HAS-A** (independent lifecycles)
> * `Composition` $\rightarrow$ **HAS-A** (tied lifecycles, strong ownership)

---

## 7. Inheritance and Method Resolution Order (MRO)

### Inheritance Basics (IS-A Relationship)
**Inheritance** allows a child (derived/subclass) to inherit attributes and methods from a parent (base/superclass), fostering code reuse and hierarchy modeling.

```python
class Animal:
    def eat(self):
        print("Eating food...")

class Dog(Animal):  # Dog IS-A Animal
    def bark(self):
        print("Woof woof!")

d = Dog()
d.eat()   # Inherited from Animal -> Eating food...
d.bark()  # Defined on Dog -> Woof woof!
```

### What Can Be Inherited in Python?
When a subclass derives from a superclass, it inherits:
* $\checkmark$ **Instance methods**
* $\checkmark$ **Class methods** (`@classmethod`)
* $\checkmark$ **Static methods** (`@staticmethod`)
* $\checkmark$ **Class variables**
* $\checkmark$ **Instance attributes** (provided the parent's `__init__()` is invoked)
* $\triangle$ **Constructors (`__init__`)**: Inherited automatically if the child does not define its own `__init__()`. If the child defines `__init__()`, it overrides the parent's constructor.

### Private Members in Inheritance & Name Mangling
A common interview trick question asks: *"Are private members inherited in Python?"*

> [!IMPORTANT]
> **Accurate Interview Answer:**
> Do **not** simply say *"private members are not inherited"*. 
> In Python, attributes prefixed with `__` are **name-mangled** to `_ParentClassName__attribute`. Because of name mangling, the child class cannot access them directly using `self.__variable`. However, they still exist within the instance's dictionary (`__dict__`) and can be accessed via their mangled name or via public parent methods.

```python
class Parent:
    def __init__(self):
        self.__x = 10

class Child(Parent):
    def show(self):
        # print(self.__x)        # Raises AttributeError!
        print(self._Parent__x)  # Works because of name mangling!

c = Child()
c.show()  # 10
```

### Constructor Inheritance & `super().__init__()`
1. If the child does **not** define an `__init__()`, Python automatically delegates to the parent's `__init__()`.
2. If the child **defines its own** `__init__()`, it overrides the parent's constructor. To initialize parent attributes, you must explicitly invoke `super().__init__()`.

```python
class Person:
    def __init__(self, name):
        self.name = name

class Student(Person):
    def __init__(self, name, student_id):
        super().__init__(name)  # Delegate to Person.__init__
        self.student_id = student_id
```

### Types of Inheritance

#### 1. Single-Level Inheritance
One child class derives from a single parent class.
```
Parent ◄─── Child
```
```python
class Parent: pass
class Child(Parent): pass
```

#### 2. Multilevel Inheritance
A child class derives from another child class, forming an inheritance chain.
```
GrandParent ◄─── Parent ◄─── Child
```
```python
class GrandParent: pass
class Parent(GrandParent): pass
class Child(Parent): pass
```

#### 3. Hierarchical Inheritance
Multiple child classes inherit from the same single parent class.
```
        Parent
       ▲      ▲
       │      │
   Child1    Child2
```
```python
class Parent: pass
class Child1(Parent): pass
class Child2(Parent): pass
```

#### 4. Multiple Inheritance
A single child class inherits from **multiple parent classes**.
```
   Parent1    Parent2
       ▲        ▲
       └────┬───┘
          Child
```
```python
class Parent1: pass
class Parent2: pass
class Child(Parent1, Parent2): pass
```

---

### MRO (Method Resolution Order) & C3 Linearization
**MRO** is the exact deterministic order in which Python traverses the class inheritance tree when searching for a method or attribute.

Python determines MRO using the **C3 Linearization Algorithm** (ensuring local precedence and monotonicity).

#### Inspecting MRO:
You can inspect any class's MRO using:
1. `ClassName.mro()` (returns a list)
2. `ClassName.__mro__` (returns a tuple)

### Method & Constructor Resolution in Multiple Inheritance

Consider this classic placement problem:
```python
class Phone:
    def __init__(self, price, brand, camera):
        print("Inside Phone constructor")
        self.__price = price
        self.brand = brand
        self.camera = camera

    def buy(self):
        print("Buying a phone")

class Product:
    def buy(self):
        print("Product buy method")

class SmartPhone(Phone, Product):
    pass
```

```python
s = SmartPhone(20000, "Apple", 12)
s.buy()
```

#### Execution Analysis:
1. **Inspecting MRO:**
   ```python
   print(SmartPhone.mro())
   # [<class 'SmartPhone'>, <class 'Phone'>, <class 'Product'>, <class 'object'>]
   ```
2. **Constructor Resolution:**
   `SmartPhone` has no `__init__()`. Python follows MRO:
   $\text{SmartPhone} \rightarrow \text{Phone} \rightarrow \text{Product} \rightarrow \text{object}$
   It encounters `Phone.__init__()` first and executes it.
   **Output:** `"Inside Phone constructor"`
3. **Method Resolution (`s.buy()`):**
   Both `Phone` and `Product` define `buy()`. Because `Phone` precedes `Product` in `SmartPhone(Phone, Product)`, `Phone.buy()` wins.
   **Output:** `"Buying a phone"`

> [!WARNING]
> **Placement Note:**
> Python does **not** automatically execute constructors of all parents in multiple inheritance. Only the first one encountered in the MRO executes unless `super()` cooperative multiple inheritance chaining is explicitly designed across all parent classes.

---

## 8. Polymorphism

### What is Polymorphism?
**Polymorphism** translates literally to *"many forms"*. In programming, it represents the principle where **a single uniform interface or method call can operate on different underlying types/classes**, each providing its own specialized behavior.

```
                         Polymorphism
           ┌───────────────────┼───────────────────┐
   Method Overriding   Method Overloading   Operator Overloading
   (Child reimplements (Simulated via       (Via Dunder methods)
     parent method)     default args/*args)
```

### Method Overriding
Occurs when a child class defines a method with the **exact same name and signature** as a method already existing in its parent class, replacing the parent's default behavior.

```python
class Animal:
    def sound(self):
        print("Generic animal sound")

class Dog(Animal):
    def sound(self):  # Overrides Animal.sound()
        print("Bark")

class Cat(Animal):
    def sound(self):  # Overrides Animal.sound()
        print("Meow")

animals = [Dog(), Cat()]
for a in animals:
    a.sound()  # Same interface, different behavior!
```

### Method Overloading in Python (Why Traditional Overloading Doesn't Exist)
In languages like Java or C++, method overloading allows multiple methods within the same class to share the same name with different parameter signatures:
```java
// Java:
int add(int a, int b) { ... }
int add(int a, int b, int c) { ... }
```

#### Why Python Does Not Support Traditional Method Overloading:
Python is a dynamically typed, interpreted language. In Python, methods are first-class attribute objects stored in the class's dictionary (`__dict__`). If you declare two methods with the identical name, **the second definition overwrites the first**.

```python
class Calculator:
    def add(self, a, b):
        return a + b

    def add(self, a, b, c):  # Overwrites the previous add method!
        return a + b + c

c = Calculator()
# c.add(2, 3)     # Raises TypeError: add() missing 1 required positional argument: 'c'
print(c.add(2, 3, 4))  # 9
```

#### How Python Achieves Overloading Behavior:
1. **Default Parameter Values:**
   ```python
   class Calculator:
       def add(self, a, b, c=0):
           return a + b + c
   ```
2. **Variable-Length Arguments (`*args`):**
   ```python
   class Calculator:
       def add(self, *args):
           return sum(args)

   c = Calculator()
   print(c.add(2, 3))       # 5
   print(c.add(2, 3, 4, 5)) # 14
   ```

> [!NOTE]
> `*args` allows a function to accept an arbitrary number of positional arguments. While `*args` achieves flexible argument handling, it is conceptually variable argument handling rather than compile-time method overloading.

### Operator Overloading & Dunder Methods
Operator overloading allows standard Python operators (`+`, `-`, `*`, `==`, `<`, etc.) to exhibit customized behavior when applied to user-defined objects. Python maps operators to corresponding dunder methods.

```python
class Number:
    def __init__(self, value):
        self.value = value

    def __add__(self, other):
        """Overload the '+' operator."""
        return Number(self.value + other.value)

    def __eq__(self, other):
        """Overload the '==' operator."""
        return self.value == other.value

a = Number(10)
b = Number(20)
c = a + b  # Automatically triggers a.__add__(b)
print(c.value)  # 30
print(a == b)   # False (triggers a.__eq__(b))
```

### The `super()` Keyword
`super()` returns a proxy object that delegates method calls to a parent or sibling class according to the class's **Method Resolution Order (MRO)**.

#### 1. Invoking Parent Constructor:
```python
class Parent:
    def __init__(self):
        print("Parent constructor")

class Child(Parent):
    def __init__(self):
        super().__init__()  # Calls Parent.__init__()
        print("Child constructor")

c = Child()
# Output:
# Parent constructor
# Child constructor
```

#### 2. Invoking Overridden Parent Method:
```python
class Parent:
    def show(self):
        print("Parent functionality")

class Child(Parent):
    def show(self):
        super().show()  # Reuse parent behavior
        print("Child added functionality")

c = Child()
c.show()
```

#### Can `super()` Be Used Outside a Class?
No. Zero-argument `super()` requires the active lexical class and method context to resolve the calling class and instance. Calling bare `super()` outside a class raises a `RuntimeError`.

### Duck Typing (Behavior Over Type)
Python follows the **Duck Typing** philosophy:
> *"If it walks like a duck and quacks like a duck, it's a duck."*

In Python, an object's suitability is determined by the **presence of specific methods and properties**, rather than by its explicit inheritance from a specific parent class.

```python
class Dog:
    def sound(self):
        print("Bark")

class Cat:
    def sound(self):
        print("Meow")

class Car:
    def sound(self):
        print("Honk Honk!")

def play_sound(entity):
    # Does not care about the class of 'entity',
    # only cares that 'entity' has a callable sound() method
    entity.sound()

play_sound(Dog())  # Bark
play_sound(Cat())  # Meow
play_sound(Car())  # Honk Honk!
```

---

## 9. Abstraction & Interfaces

### What is Abstraction?
**Abstraction** means hiding internal implementation complexities and revealing only the clean, essential interface to the consumer.

* **Consumer knows:** **WHAT** the object does.
* **Consumer doesn't need to know:** **HOW** it executes internally.

**Example:**
When you turn on a car using `car.start()`, you do not need to understand electronic fuel injection, cylinder spark timing, or hydraulic valve pressure.

### Abstract Base Classes (ABCs) & `@abstractmethod`
Python implements formal abstraction via the built-in `abc` module using the `ABC` class and `@abstractmethod` decorator.

An **Abstract Base Class (ABC)** serves as a strict contract/blueprint for subclasses.
* Any class deriving from an `ABC` **must implement all abstract methods**.
* An abstract class containing at least one unimplemented abstract method **cannot be instantiated**.

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def sound(self):
        """Subclasses MUST implement this method."""
        pass

class Dog(Animal):
    def sound(self):
        print("Bark")

# a = Animal()  # Raises TypeError: Can't instantiate abstract class Animal with abstract method sound
d = Dog()       # Valid! Dog implements sound()
d.sound()       # Bark
```

### Concrete Methods in Abstract Classes
An abstract class is **not restricted** to only abstract methods. It can freely contain concrete (fully implemented) methods and state shared by all descendants:

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def sound(self):
        pass

    def eat(self):  # Concrete method shared across all animals
        print("This animal is eating food.")
```

### Abstraction vs Encapsulation

```
     Abstraction                      Encapsulation
  (Hiding Complexity)             (Controlling Data Access)
  Focus: WHAT is exposed          Focus: HOW state is modified
   "Drive car without               "Balance modified only
     engine knowledge"                via deposit/withdraw"
```

| Dimension | Abstraction | Encapsulation |
| :--- | :--- | :--- |
| **Core Goal** | Hide complexity and irrelevant implementation details | Protect internal state integrity and restrict unauthorized access |
| **Focus** | **WHAT** the object does | **HOW** the object maintains and guards its data |
| **Implementation Mechanism** | Abstract Base Classes (`ABC`), interfaces, functions | Access modifiers (`_`, `__`), getters/setters, `@property` |
| **Real-World Analogy** | Pressing the "Withdraw" button on an ATM | The ATM's internal physical cash vault and PIN verification system |

### Interface vs Abstract Class

Python does **not** have a native `interface` keyword like Java or C#. In Python:
* An **Abstract Class** can contain a mix of abstract methods, concrete methods, and instance variables.
* An **Interface** in Python is conceptually modeled as an `ABC` containing **only abstract methods** (no implementation and no state), or implicitly through duck typing and protocols (`typing.Protocol`).

```
                    Abstract Class vs Interface
        Abstract Class                       Pure Interface
  • Derived from ABC                   • Derived from ABC
  • Contains abstract methods          • Contains ONLY abstract methods
  • Contains concrete methods          • No concrete methods
  • Maintains state/attributes         • No state/attributes
```

---

## 10. Type Inspection & Built-in Object Hierarchy

### `isinstance()` Function
`isinstance(obj, class_or_tuple)` verifies whether an object is an instance of a given class or any of its subclasses.

```python
class Animal: pass
class Dog(Animal): pass

d = Dog()

print(isinstance(d, Dog))     # True
print(isinstance(d, Animal))  # True (Because Dog IS-A Animal!)
print(isinstance(d, (int, str, Animal)))  # True (Checks against a tuple of candidate types)
```

### `issubclass()` Function
`issubclass(child_class, parent_class_or_tuple)` verifies whether a class is a subclass of another class.

```python
print(issubclass(Dog, Animal))  # True
print(issubclass(Animal, Dog))  # False
print(issubclass(Dog, object))  # True
```

### Comparison: `isinstance()` vs `issubclass()`

```
  isinstance(object, Class)    ──► Checks an OBJECT against a CLASS
  issubclass(ClassA, ClassB)   ──► Checks a CLASS against another CLASS
```

| Function | Signature | First Argument | Second Argument | What it Checks |
| :--- | :--- | :--- | :--- | :--- |
| `isinstance()` | `isinstance(obj, classinfo)` | Object instance | Class or tuple of classes | *"Is this object an instance of this class or its descendants?"* |
| `issubclass()` | `issubclass(cls, classinfo)` | Class | Class or tuple of classes | *"Is this class a child/derived class of that class?"* |

### The Root `object` Class
In Python 3, all classes implicitly inherit from the ultimate built-in root class: **`object`**.

```python
class Student:
    pass

print(Student.__bases__)  # (<class 'object'>,)
print(Student.mro())      # [<class 'Student'>, <class 'object'>]
```
Because of this root hierarchy, every Python class inherits standard default dunder methods such as `__new__`, `__init__`, `__str__`, `__repr__`, `__eq__`, and `__hash__`.

### `__str__` vs `__repr__`

| Method | Built-in Hook | Target Audience | Primary Goal | Recommended Output |
| :--- | :--- | :--- | :--- | :--- |
| `__str__()` | `str(obj)`, `print(obj)` | End-user | Informal, human-readable clarity | Concise summary: `"Rahul (Age 21)"` |
| `__repr__()` | `repr(obj)`, Debuggers, REPL | Software developers | Formal, unambiguous technical detail | Code-like expression: `"Student(name='Rahul', age=21)"` |

```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return f"{self.name}, {self.age} years old"

    def __repr__(self):
        return f"Student(name={self.name!r}, age={self.age!r})"

s = Student("Rahul", 21)
print(str(s))   # Rahul, 21 years old (End-user friendly)
print(repr(s))  # Student(name='Rahul', age=21) (Developer debugging)
```

> [!NOTE]
> If a class defines `__repr__()` but lacks `__str__()`, Python will automatically fallback to `__repr__()` whenever `str(obj)` or `print(obj)` is called.

---

## 11. Integrated Real-World Design Example

In real-world software engineering and SDE interviews, these OOP principles are rarely assessed in isolation. They interact harmoniously to create scalable, robust systems.

### Architecture Overview
The following example implements an E-Commerce Payment Processing System that unifies:
* **Abstraction**: `Payment` base class defines the mandatory contract via `ABC`.
* **Inheritance**: `UPI` and `Card` inherit from `Payment`.
* **Method Overriding**: Concrete payment classes override `pay()`.
* **Encapsulation**: Private attributes with validation guards.
* **Composition / Aggregation**: `Checkout` HAS-A `payment_method`.
* **Polymorphism**: `Checkout.process()` operates polymorphically on any compliant payment provider.

### Complete Implementation (Checkout & Payment Processing)

```python
from abc import ABC, abstractmethod

# 1. ABSTRACTION & INTERFACE CONTRACT
class Payment(ABC):
    @abstractmethod
    def pay(self, amount):
        """Abstract method that all payment methods must implement."""
        pass

# 2. INHERITANCE & METHOD OVERRIDING (UPI)
class UPI(Payment):
    def __init__(self, vpa_id):
        self.__vpa_id = vpa_id  # ENCAPSULATION (Private state)

    def pay(self, amount):      # METHOD OVERRIDING
        if amount <= 0:
            raise ValueError("Amount must be positive")
        print(f"Paid Rs.{amount} successfully using UPI ID: {self.__vpa_id}")

# 3. INHERITANCE & METHOD OVERRIDING (Card)
class Card(Payment):
    def __init__(self, card_number, cvv):
        self.__card_number = card_number
        self.__cvv = cvv

    def pay(self, amount):      # METHOD OVERRIDING
        if amount <= 0:
            raise ValueError("Amount must be positive")
        masked_card = f"****-****-****-{self.__card_number[-4:]}"
        print(f"Paid Rs.{amount} successfully using Card: {masked_card}")

# 4. COMPOSITION / AGGREGATION & POLYMORPHISM
class Checkout:
    def __init__(self, payment_method: Payment):
        # Checkout HAS-A Payment method (Aggregation/Composition relationship)
        if not isinstance(payment_method, Payment):
            raise TypeError("Expected an instance of Payment")
        self.payment_method = payment_method

    def process(self, amount):
        # POLYMORPHIC INVOCATION:
        # Checkout does not care whether it's UPI, Card, or Crypto.
        # It relies on the common interface 'pay()'.
        print("Initiating checkout transaction...")
        self.payment_method.pay(amount)
        print("Transaction complete.\n")


# Execution Demonstration
upi_gateway = UPI("rahul@okaxis")
card_gateway = Card("1234567890123456", "999")

cart1 = Checkout(upi_gateway)
cart1.process(1500)

cart2 = Checkout(card_gateway)
cart2.process(4200)
```

### How All Concepts Connect
* `Payment` $\rightarrow$ **Abstraction** (hides processing mechanics).
* `UPI` / `Card` $\rightarrow$ **Inheritance** (`IS-A Payment`).
* `UPI.pay()` / `Card.pay()` $\rightarrow$ **Method Overriding** (custom payment algorithms).
* `Checkout` $\rightarrow$ **Aggregation / Composition** (`Checkout HAS-A payment_method`).
* `self.payment_method.pay(amount)` $\rightarrow$ **Polymorphism** (executes correct subclass method dynamically at runtime).

---

## 12. Placement & Interview Quick Reference

### Comprehensive Placement Revision Table

| Concept | High-Yield Definition | Syntax / Key Hook | Key Placement Rule |
| :--- | :--- | :--- | :--- |
| **Class** | Blueprint for creating objects | `class Car:` | Defines properties and behaviors |
| **Object** | Concrete instance of a class | `c = Car()` | Allocated block in heap memory |
| **Constructor** | Initializes an object's state | `def __init__(self):` | Automatically invoked upon creation |
| **`self`** | Reference to current instance | `self.attr` | Explicit first argument of instance methods |
| **Dunder Methods** | Built-in special operator hooks | `__str__`, `__len__`, `__add__` | Automatically invoked by Python syntax |
| **Encapsulation** | Bundling data & controlling access | `self.__pin` | Protects state via validation |
| **Abstraction** | Exposing WHAT, hiding HOW | `ABC`, `@abstractmethod` | Focuses on essential external interface |
| **Inheritance** | Acquiring parent features | `class Dog(Animal):` | Models **IS-A** relationship |
| **Polymorphism** | Same interface, different behavior | Method overriding, duck typing | Enables flexible, decoupled code |
| **Instance Method** | Operates on specific object | `def show(self):` | First parameter is `self` |
| **Class Method** | Operates on class-level state | `@classmethod`, `def f(cls):` | Used for alternative constructors |
| **Static Method** | Isolated utility in class namespace | `@staticmethod`, `def f():` | No `self` and no `cls` |
| **Aggregation** | Weak HAS-A relationship | Passed in via parameter | Contained object has independent lifecycle |
| **Composition** | Strong HAS-A relationship | Created inside container | Contained object dies with container |
| **Association** | General relationship between classes | `Teacher teaches Student` | Independent objects interacting |
| **ABC** | Abstract Base Class | `from abc import ABC` | Defines template for subclasses |
| **`@abstractmethod`** | Enforces child implementation | `@abstractmethod` | Classes with abstract methods cannot be instantiated |
| **`@property`** | Pythonic getter/setter syntax | `@property`, `@x.setter` | Access methods using attribute syntax |
| **Duck Typing** | Behavior over explicit type | *"Quacks like a duck"* | Focuses on what an object can DO |
| **`isinstance()`** | Checks object-to-class match | `isinstance(obj, Class)` | Evaluates inheritance hierarchy |
| **`issubclass()`** | Checks class-to-class inheritance | `issubclass(Child, Parent)` | Evaluates class relationships |
| **`object`** | Ultimate root class of Python | All classes derive from `object` | Base of Python MRO |
| **`__str__`** | Readable string representation | `str(obj)`, `print(obj)` | Formatted for end users |
| **`__repr__`** | Unambiguous debug representation | `repr(obj)` | Formatted for developers |
| **MRO** | Method Resolution Order | `Class.mro()`, C3 Linearization | Order of class traversal |
| **Pass-by-Sharing** | Object references passed by value | Parameter passing in functions | Mutate in-place vs Rebinding |

---

### Most Important Interview Differences

#### 1. Encapsulation vs Abstraction
* **Encapsulation**: Focuses on **HOW** data is accessed and kept safe (bundling data + methods, private attributes, setters with validation).
* **Abstraction**: Focuses on **WHAT** operations are exposed to the user while concealing underlying implementation complexity (ABCs, interfaces).

#### 2. Inheritance vs Composition
* **Inheritance**: Models an **IS-A** relationship (`Dog IS-A Animal`). Tightly coupled.
* **Composition**: Models a **HAS-A** relationship (`Car HAS-A Engine`). Loosely coupled and preferred when lifecycles are tightly owned.

#### 3. Aggregation vs Composition
* **Aggregation**: **Weak HAS-A**. Contained object exists independently outside the container (`Department HAS-A Professor`).
* **Composition**: **Strong HAS-A**. Contained object's lifecycle is bound to the container (`Car HAS-A Engine`).

#### 4. Instance Method vs Class Method
* **Instance Method**: Receives `self`; accesses and mutates individual object instance data.
* **Class Method**: Receives `cls`; accesses and mutates shared class-level data; used for alternative constructors.

#### 5. Class Method vs Static Method
* **Class Method**: Receives `cls`; aware of and interacts with the class hierarchy.
* **Static Method**: Receives neither `self` nor `cls`; acts as an isolated utility function scoped inside the class namespace.

#### 6. `isinstance()` vs `issubclass()`
* **`isinstance(obj, Class)`**: Tests an **object instance** against a class.
* **`issubclass(Child, Parent)`**: Tests a **class** against another class.

#### 7. Abstract Class vs Interface
* **Abstract Class**: Can contain abstract methods, concrete methods, and state.
* **Interface**: Represents a pure behavioral contract (in Python, an `ABC` containing solely abstract methods).

---

### 29 Essential Interview Questions Checklist

Be prepared to answer and write code snippets for every item on this checklist:

- [ ] **1. Class and Object**: Can you explain blueprints vs memory instances?
- [ ] **2. Constructor and `self`**: What happens when an object is instantiated?
- [ ] **3. Instance Variables**: How does memory isolation work across instances?
- [ ] **4. Class Variables**: What happens if you reassign `instance.class_var = val`?
- [ ] **5. Instance Methods**: Why is `self` passed explicitly?
- [ ] **6. Class Methods (`@classmethod`)**: How and why do you write alternative constructors?
- [ ] **7. Static Methods (`@staticmethod`)**: When should a method have neither `self` nor `cls`?
- [ ] **8. Encapsulation**: How do getters and setters guard object integrity?
- [ ] **9. Abstraction**: How do you enforce interface contracts?
- [ ] **10. ABC and `@abstractmethod`**: What prevents instantiating incomplete subclasses?
- [ ] **11. Inheritance**: How does code reusability work via `IS-A`?
- [ ] **12. Types of Inheritance**: Can you explain multiple vs multilevel vs hierarchical?
- [ ] **13. Method Overriding**: How does a subclass provide specialized behavior?
- [ ] **14. Method Overloading in Python**: Why does Python lack Java-style overloading, and how do `*args` simulate it?
- [ ] **15. Operator Overloading**: How do dunder methods like `__add__` and `__eq__` work?
- [ ] **16. `super()` Function**: How does `super()` delegate calls through the MRO?
- [ ] **17. MRO & C3 Linearization**: How does Python resolve conflicting methods in multiple inheritance?
- [ ] **18. Aggregation**: Can you write an example of a weak HAS-A relationship with independent lifecycles?
- [ ] **19. Composition**: Can you write an example of a strong HAS-A relationship with owned lifecycles?
- [ ] **20. Association**: How does general class interaction differ from aggregation?
- [ ] **21. `@property` Decorator**: How do you create Pythonic getters and setters?
- [ ] **22. Duck Typing**: Why does Python care what an object can DO rather than its class type?
- [ ] **23. Dunder Methods**: Which dunder methods are most commonly used in production?
- [ ] **24. `__str__` vs `__repr__`**: What is the difference between end-user and developer representations?
- [ ] **25. `isinstance()`**: Why is `isinstance(Dog(), Animal)` evaluated to `True`?
- [ ] **26. `issubclass()`**: How does `issubclass()` evaluate inheritance?
- [ ] **27. Mutable vs Immutable Objects**: Why are `str` and `int` immutable, and what happens during reassignment?
- [ ] **28. Python's Reference Model**: How does aliasing work with `s2 = s1`?
- [ ] **29. Pass-by-Object-Reference**: Why does in-place mutation persist while parameter rebinding does not?
