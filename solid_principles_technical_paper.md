# SOLID Principles in Object-Oriented Software Design

## Abstract
SOLID is a group of five principles used to guide object-oriented software design. The principles encourage developers to create components that are easier to maintain, test, understand, and extend. They address common design problems such as classes with too many responsibilities, rigid dependencies, and inheritance structures that break expected behavior. This paper explains each SOLID principle and demonstrates how the ideas can be applied in Python.

## 1. Introduction
Software requirements change over time. When a system is tightly coupled or poorly divided, even a small change can affect many unrelated parts of the application. SOLID provides design guidance for managing these changes. 

The acronym represents:

- **S — Single Responsibility Principle**
- **O — Open/Closed Principle**
- **L — Liskov Substitution Principle**
- **I — Interface Segregation Principle**
- **D — Dependency Inversion Principle**

These are guidelines rather than strict language rules; they should be applied where they make a design clearer.

## 2. The Five SOLID Principles

### 2.1 Single Responsibility Principle (SRP)
A class should have one responsibility and one main reason to change. For example, a class that calculates an invoice, saves it to a database, and formats it for printing combines separate concerns. Separating calculation and persistence into different components makes each easier to modify and test independently.

### Example: Without SRP

```python
class Report:
    def generate(self):
        print("Generating report")

    def save_to_file(self, report):
        print("Saving report to a file")

    def send_email(self, report):
        print("Sending report by email")
```

This class has several reasons to change:

- The report-generation rules may change.
- The file-saving format or location may change.
- The email delivery process may change.

### Example: Following SRP

```python
class Report:
    def generate(self):
        return "Monthly sales report"


class ReportSaver:
    def save(self, report):
        print(f"Saving: {report}")


class EmailSender:
    def send(self, report):
        print(f"Emailing: {report}")


report = Report().generate()
ReportSaver().save(report)
EmailSender().send(report)
```

Each class has a focused responsibility:

- `Report` creates report content.
- `ReportSaver` handles persistence.
- `EmailSender` handles email delivery.

### 2.2 Open/Closed Principle (OCP)
Software components should be open for extension but closed for modification. New behavior should preferably be added through new implementations or extension points instead of repeatedly changing stable code. For example, a payment system can define a common `pay()` operation and provide separate implementations for card and bank-transfer payments.

### Example: Without OCP

```python
class Discount:
    def calculate(self, customer_type):
        if customer_type == "regular":
            return 10
        elif customer_type == "premium":
            return 20
        elif customer_type == "employee":
            return 30
        return 0
```

Every new customer category requires modifying `Discount.calculate()`. As the number of categories grows, the conditional logic becomes harder to maintain.

### Example: Following OCP

```python
class Discount:
    def calculate(self):
        raise NotImplementedError


class RegularDiscount(Discount):
    def calculate(self):
        return 10


class PremiumDiscount(Discount):
    def calculate(self):
        return 20


class EmployeeDiscount(Discount):
    def calculate(self):
        return 30


def show_discount(discount):
    print(discount.calculate())


show_discount(RegularDiscount())
show_discount(PremiumDiscount())
show_discount(EmployeeDiscount())
```

A new discount type can be introduced as another implementation without changing the existing discount classes or `show_discount()` function.

In production code, an abstract base class or a `Protocol` can define the required interface more explicitly.

### 2.3 Liskov Substitution Principle (LSP)
Objects of a subtype should be usable wherever objects of the parent type are expected without changing the correctness of the program. A subclass should preserve the behavior promised by its base class. If a subclass cannot honor that contract, the inheritance relationship may be inappropriate. For instance, a design should not require every `Bird` subtype to implement flying if some birds cannot fly.

### Example: Violating LSP

```python
class Bird:
    def fly(self):
        print("Bird is flying")


class Penguin(Bird):
    def fly(self):
        raise RuntimeError("Penguins cannot fly")


def make_bird_fly(bird):
    bird.fly()


make_bird_fly(Penguin())
```

The `make_bird_fly()` function expects a bird that can fly. A `Penguin` cannot satisfy that expectation, even though it inherits from `Bird`.

The problem is the parent abstraction: not every bird can fly.

### Example: Following LSP

```python
class Bird:
    def move(self):
        raise NotImplementedError


class FlyingBird(Bird):
    def move(self):
        self.fly()

    def fly(self):
        print("Bird is flying")


class Penguin(Bird):
    def move(self):
        self.swim()

    def swim(self):
        print("Penguin is swimming")


def move_bird(bird):
    bird.move()


move_bird(FlyingBird())
move_bird(Penguin())
```

Both subclasses satisfy the general promise that a `Bird` can move. Their specific movement behaviour differs.

### 2.4 Interface Segregation Principle (ISP)
**Definition:** Clients should not be forced to depend on methods they do not use.

An interface should be focused on the needs of its clients. Large interfaces often force simple implementations to provide irrelevant methods.

Python does not have a dedicated `interface` keyword. Interfaces can be represented using abstract base classes or structural typing with `typing.Protocol`.

### Example: Without ISP

```python
class Machine:
    def print_document(self, document):
        raise NotImplementedError

    def scan_document(self, document):
        raise NotImplementedError

    def fax_document(self, document):
        raise NotImplementedError


class BasicPrinter(Machine):
    def print_document(self, document):
        print(f"Printing: {document}")

    def scan_document(self, document):
        raise NotImplementedError("Scanning is not supported")

    def fax_document(self, document):
        raise NotImplementedError("Faxing is not supported")
```

A basic printer is forced to depend on scanning and faxing operations that it does not support.

### Example: Following ISP

```python
class Printer:
    def print_document(self, document):
        print(f"Printing: {document}")


class Scanner:
    def scan_document(self, document):
        print(f"Scanning: {document}")


class Fax:
    def fax_document(self, document):
        print(f"Faxing: {document}")


class MultiFunctionPrinter(Printer, Scanner, Fax):
    pass


class BasicPrinter(Printer):
    pass


basic_printer = BasicPrinter()
basic_printer.print_document("Report.pdf")
```

Now:

- `BasicPrinter` supports printing only.
- `MultiFunctionPrinter` supports printing, scanning and faxing.
- Neither class is forced to implement irrelevant operations.

For larger applications, these capabilities can be formalized as separate abstract base classes or protocols.

### 2.5 Dependency Inversion Principle (DIP)
High-level business logic should not depend directly on low-level implementation details. Both should depend on abstractions. For example, a notification service should rely on an object that provides a `send()` operation rather than constructing a specific email provider internally.

### Example: Without DIP

```python
class EmailService:
    def send(self, message):
        print(f"Sending email: {message}")


class Notification:
    def __init__(self):
        self.email_service = EmailService()

    def notify(self, message):
        self.email_service.send(message)


notification = Notification()
notification.notify("Your order has shipped")
```

`Notification` directly creates an `EmailService`. It is tied to that specific implementation.

### Example: Following DIP

```python
from abc import ABC, abstractmethod


class MessageService(ABC):
    @abstractmethod
    def send(self, message):
        pass


class EmailService(MessageService):
    def send(self, message):
        print(f"Sending email: {message}")


class SMSService(MessageService):
    def send(self, message):
        print(f"Sending SMS: {message}")


class Notification:
    def __init__(self, message_service):
        self.message_service = message_service

    def notify(self, message):
        self.message_service.send(message)


email_notification = Notification(EmailService())
sms_notification = Notification(SMSService())

email_notification.notify("Your order has shipped")
sms_notification.notify("Your order has shipped")
```

Output:

```text
Sending email: Your order has shipped
Sending SMS: Your order has shipped
```
**Explanation:**

- `MessageService` is the abstraction.
- `EmailService` and `SMSService` are concrete implementations.
- `Notification` depends on the abstraction's expected operation rather than creating a specific service.
- A service is supplied from outside when `Notification` is constructed.

## 3. Why SOLID Matters
Applying SOLID thoughtfully can reduce coupling, improve testability, and make changes safer. Small responsibilities help isolate bugs. Extension points make it easier to add new features. Clear contracts help ensure that different implementations can be substituted safely. Focused interfaces avoid unnecessary dependencies, while dependency inversion makes components easier to test with alternative implementations.

However, applying every principle mechanically can create too many small classes and abstractions. A simple program may not need an elaborate architecture. The goal is to reduce complexity, not to increase the number of files or classes.

## 4. Conclusion
SOLID provides five complementary guidelines for designing maintainable object-oriented systems. Together, the principles encourage focused responsibilities, controlled extension, reliable substitution, small interfaces, and dependencies on abstractions. Python developers can apply these ideas using classes, inheritance, abstract base classes, protocols, and dependency injection. The most effective design is one that uses these principles where they solve real problems while keeping the code straightforward.
