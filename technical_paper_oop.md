# Object-Oriented Programming (OOP)

## Abstract
Object-Oriented Programming (OOP) is a programming paradigm that organizes software around objects containing data and the operations that work on that data. It helps developers represent problem-domain concepts, divide large programs into smaller components, and reuse behavior. This paper introduces the main ideas behind OOP, explains its four commonly discussed principles, and demonstrates how Python supports object-oriented design.

## 1. Introduction
As software systems grow, managing related data and functionality in one large program becomes difficult. OOP addresses this challenge by grouping state and behavior into objects. A class defines the structure and behavior that its objects will have. Python supports classes and inheritance, while also allowing developers to use procedural or functional approaches when those are more suitable.

## 2. Core Concepts
A **class** is a blueprint that defines attributes and methods. An **object** is an instance of a class. Attributes represent an object's state, while methods define the actions it can perform. In Python, the `__init__` method commonly initializes an object's attributes, and `self` refers to the current instance.

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount):
        if amount > 0:
            self.balance += amount

account = BankAccount("Asha", 1000)
account.deposit(500)
print(account.balance)
```

The example models a bank account as an object. Its data and related operation are kept together, making the code easier to understand and extend.

## 3. The Four Principles of OOP

## 3.1 Encapsulation
Encapsulation groups data and the methods that operate on it within a class. It helps establish a clear boundary around an object's state. Python uses naming conventions such as a leading underscore (`_balance`) to indicate that an attribute is intended for internal use. This is a convention rather than strict access control.

### Example: Bank account

```python
class BankAccount:
    def __init__(self, opening_balance):
        self.__balance = opening_balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit must be positive")
        self.__balance += amount

    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("Withdrawal must be positive")
        if amount > self.__balance:
            raise ValueError("Insufficient balance")
        self.__balance -= amount

    def get_balance(self):
        return self.__balance


account = BankAccount(1000)
account.deposit(500)
account.withdraw(200)
print(account.get_balance())
```

Output:

```text
1300
```

## 3.2 Inheritance
**Inheritance** allows a class (the child or subclass) to reuse and specialize behaviour from another class (the parent or superclass).

It is useful when there is a genuine **is-a** relationship. For example, a `Dog` is an `Animal`.

### Example: Single inheritance

```python
class Animal:
    def eat(self):
        print("This animal is eating")


class Dog(Animal):
    def bark(self):
        print("The dog is barking")


dog = Dog()
dog.eat()
dog.bark()
```

Output:

```text
This animal is eating
The dog is barking
```

The `Dog` class inherits `eat()` from `Animal` and defines its own `bark()` method.

### Types of inheritance

1. **Single inheritance:** One child class inherits from one parent class.
2. **Multiple inheritance:** One child class inherits from more than one parent class.
3. **Multilevel inheritance:** A class inherits from a class that itself inherits from another class.
4. **Hierarchical inheritance:** Multiple child classes inherit from the same parent.
5. **Hybrid inheritance:** A design combines two or more inheritance patterns.


## 3.3 Polymorphism
**Polymorphism** means that different objects can be used through a common interface while each object provides its own behaviour.

For example, different payment methods can all provide a `pay()` operation, even though the way each payment is processed differs.

### Example: Duck typing

Python commonly uses duck typing: code cares about whether an object supports the required operation, rather than requiring it to belong to a particular class.

```python
class Dog:
    def sound(self):
        print("Bark")


class Cat:
    def sound(self):
        print("Meow")


def make_sound(animal):
    animal.sound()


make_sound(Dog())
make_sound(Cat())
```

Output:

```text
Bark
Meow
```

## 3.4 Abstraction
Abstraction exposes the essential operations of a component while hiding unnecessary implementation details. In Python, abstract base classes from the `abc` module can define methods that concrete subclasses are required to implement.

### Example: Abstract vehicle

```python
from abc import ABC, abstractmethod


class Vehicle(ABC):
    @abstractmethod
    def start(self):
        pass


class Car(Vehicle):
    def start(self):
        print("Car engine started")


class ElectricScooter(Vehicle):
    def start(self):
        print("Electric scooter started")


car = Car()
car.start()
```

Output:

```text
Car engine started
```

## 4. Benefits and Limitations
OOP can improve modularity, code reuse, maintainability, and the ability to model complex domains. It can also make testing easier when classes have clear responsibilities and dependencies. However, using classes for every small task can introduce unnecessary complexity. Deep inheritance hierarchies can make behavior difficult to follow. Developers should choose OOP when objects provide a useful way to organize the problem, rather than applying it automatically.

## OOP Quick Reference 

| Term | Quick meaning |
|---|---|
| Class | Blueprint that defines attributes and methods. |
| Object | Instance of a class. |
| Attribute | Data associated with a class or object. |
| Method | Function defined inside a class. |
| Encapsulation | Bundling state and behaviour and controlling access to state. |
| Abstraction | Exposing essential operations while hiding implementation details. |
| Inheritance | Creating a specialized class from another class. |
| Polymorphism | Using a common interface for different implementations. |
| Composition | Building an object using other objects. |
| `self` | Conventional reference to the current instance. |
| `cls` | Conventional reference to the current class. |
| `super()` | Accesses the next implementation in the MRO. |
| `@classmethod` | Declares a method that receives the class. |
| `@staticmethod` | Declares a method with no automatic instance or class argument. |
| `@property` | Exposes a method through attribute-style access. |
| `@abstractmethod` | Marks a method that concrete subclasses must implement. |
| MRO | Method Resolution Order used for attribute and method lookup. |

## 5. Conclusion
OOP is a useful approach for structuring programs around objects that combine state and behavior. Encapsulation, inheritance, polymorphism, and abstraction provide ways to organize and reuse code. Python offers a flexible implementation of these concepts, allowing developers to combine object-oriented design with other programming styles according to the needs of a project.
