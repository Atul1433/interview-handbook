### 🚀 SOLID Principles – Interview Ready Notes
SOLID is an acronym for five design principles in object-oriented programming:
- S → Single Responsibility Principle
- O → Open/Closed Principle
- L → Liskov Substitution Principle
- I → Interface Segregation Principle
- D → Dependency Inversion Principle

They are important because they reduce complexity, increase maintainability, and make the code more extensible and testable. Following SOLID helps avoid “spaghetti code” and promotes clean architecture.

**🔹 1. Single Responsibility Principle (SRP)**

**Definition (Interview-ready):**
👉 A class should have only one reason to change. It should focus on a single responsibility rather than handling multiple concerns.

Why it matters:
- Prevents God classes.
- Improves maintainability, testability, and readability.
- Makes classes easier to extend.

C# Example:
```cs
// ❌ Violates SRP – handles both report creation and printing
class Report {
    public string Generate() => "Report Data";
    public void Print(string report) => Console.WriteLine(report);
}

// ✅ Follows SRP – separate responsibilities
class Report {
    public string Generate() => "Report Data";
}
class ReportPrinter {
    public void Print(string report) => Console.WriteLine(report);
}
```
👉 “SRP means a class should have only one reason to change. For example, in my Report class, generating and printing were separate responsibilities. I split them into two classes to isolate concerns.”

### 🔹2. Open/Closed Principle (OCP)

**Definition (Interview-ready):**
👉 A class should be open for extension but closed for modification. You should be able to add new functionality without changing existing code.

Why it matters:
- Reduces regression risks.
- Promotes maintainable, extensible systems.
- Often achieved with interfaces, inheritance, or composition.

C# Example:
```cs
// ❌ Violates OCP – new payment type requires modifying class
class PaymentProcessor {
    public void Pay(string type) {
        if (type == "Credit") { /* logic */ }
        else if (type == "UPI") { /* logic */ }
    }
}

// ✅ Follows OCP – extend via new classes
interface IPayment {
    void Pay();
}
class CreditPayment : IPayment {
    public void Pay() => Console.WriteLine("Paid by Credit Card");
}
class UpiPayment : IPayment {
    public void Pay() => Console.WriteLine("Paid by UPI");
}
class PaymentProcessor {
    public void Process(IPayment payment) => payment.Pay();
}
```
👉 “OCP allows me to extend behavior without modifying tested code. In this Payment example, I can add PayPal by just creating a new class implementing IPayment, without touching PaymentProcessor.”

### 🔹 3. Liskov Substitution Principle (LSP)

**Definition (Interview-ready):**
👉 Subtypes must be substitutable for their base types without breaking functionality. In other words, derived classes should not violate the expectations set by the base class.

Why it matters:
- Prevents fragile inheritance.
- Ensures reliable polymorphism.
- Often violated when subclasses throw exceptions or alter core behavior.

C# Example:
```cs 
// ❌ Violates LSP – Ostrich breaks expectation of Bird
class Bird {
    public virtual void Fly() => Console.WriteLine("Flying");
}
class Ostrich : Bird {
    public override void Fly() => throw new NotSupportedException();
}

// ✅ Follows LSP – redesigned hierarchy
interface IBird { }
interface IFlyingBird : IBird { void Fly(); }

class Sparrow : IFlyingBird {
    public void Fly() => Console.WriteLine("Flying");
}
class Ostrich : IBird { }
```
👉 “LSP ensures subclasses don’t break contracts of base classes. For example, an Ostrich cannot fly, so putting it in the Bird hierarchy where Fly() is expected breaks LSP. I fixed it by separating IFlyingBird from IBird.”

### 🔹 4. Interface Segregation Principle (ISP)

**Definition (Interview-ready):**
👉 Clients should not be forced to depend on methods they don’t use. Interfaces should be small and role-specific instead of large and general.

Why it matters:
- Prevents “fat” interfaces.
- Makes implementations simpler and cleaner.
- Reduces chances of NotImplementedException.

C# Example:
```cs
// ❌ Violates ISP – too many responsibilities
interface IPrinter {
    void Print();
    void Scan();
    void Fax();
}
class SimplePrinter : IPrinter {
    public void Print() { }
    public void Scan() => throw new NotImplementedException();
    public void Fax() => throw new NotImplementedException();
}

// ✅ Follows ISP – segregated interfaces
interface IPrinter { void Print(); }
interface IScanner { void Scan(); }
interface IFax { void Fax(); }

class SimplePrinter : IPrinter {
    public void Print() { }
}
```
👉 “ISP means we shouldn’t force clients to depend on methods they don’t need. Instead of a fat interface with Print, Scan, and Fax, I separated them so that a simple printer only implements Print.”

### 🔹 5. Dependency Inversion Principle (DIP)

**Definition (Interview-ready):**
👉 High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions.

Why it matters:
- Reduces tight coupling.
- Improves flexibility, testing, and maintainability.
- Achieved using interfaces + Dependency Injection (DI).

C# Example:
```cs
// ❌ Violates DIP – tight coupling
class FileLogger {
    public void Log(string msg) => Console.WriteLine($"File: {msg}");
}
class OrderService {
    private FileLogger logger = new FileLogger(); // Hard dependency
    public void PlaceOrder() => logger.Log("Order placed");
}

// ✅ Follows DIP – depends on abstraction
interface ILogger {
    void Log(string msg);
}
class FileLogger : ILogger {
    public void Log(string msg) => Console.WriteLine($"File: {msg}");
}
class DbLogger : ILogger {
    public void Log(string msg) => Console.WriteLine($"DB: {msg}");
}
class OrderService {
    private readonly ILogger logger;
    public OrderService(ILogger logger) => this.logger = logger;
    public void PlaceOrder() => logger.Log("Order placed");
}
```
👉 “DIP decouples high-level modules from low-level details. In this OrderService example, instead of directly depending on FileLogger, I inject ILogger. This makes swapping implementations (DB logger, Console logger) easy without changing the service code.”

***🎯 Quick Interview Recap***

**SRP:** One class → one reason to change. (Report vs ReportPrinter)

**OCP:** Extend behavior without modifying core code. (IPayment implementations)

**LSP:** Subclasses should honor parent contracts. (Ostrich vs Bird)

**ISP:** Small, role-specific interfaces. (IPrinter, IScanner, IFax)

**DIP:** Depend on abstractions, not concrete classes. (ILogger injection)


### SOLID Principles – Interview Q&A
**🔹 General Questions**
Q1. What are the SOLID principles and why are they important in software development?

Answer:
SOLID is an acronym for five design principles in object-oriented programming:

S → Single Responsibility Principle

O → Open/Closed Principle

L → Liskov Substitution Principle

I → Interface Segregation Principle

D → Dependency Inversion Principle

They are important because they reduce complexity, increase maintainability, and make the code more extensible and testable. Following SOLID helps avoid “spaghetti code” and promotes clean architecture.

Q2. Explain the benefits of applying SOLID principles in C# projects.

Answer:

Maintainability: Easier to change or fix without breaking other parts.

Scalability: Supports adding new features without rewriting existing code.

Testability: Encourages dependency injection and modular design.

Reusability: Classes and components can be reused across projects.

Collaboration: Multiple developers can work in parallel with minimal conflicts.

Q3. How do SOLID principles contribute to writing maintainable, scalable, and testable code?

Answer:

SRP → Keeps classes small and focused (easier debugging & testing).

OCP → Enables extension without modification (safe scaling).

LSP → Ensures derived classes don’t break contracts (consistent behavior).

ISP → Keeps interfaces lean (less unused code in clients).

DIP → Promotes abstractions, enabling dependency injection and unit testing.

Q4. Can you provide a real-world example where applying a SOLID principle significantly improved a C# application?

Answer:
In an e-commerce system, we had a PaymentProcessor class handling multiple payment types. It violated OCP because every new payment method required modifying the class.

We refactored using Strategy Pattern + OCP, where each payment method implemented IPayment. The system became extensible — adding PayPal or UPI required creating a new class without modifying existing logic.

Q5. Are there any situations where you might choose to deviate from a SOLID principle, and if so, why?

Answer:
Yes. In small prototypes or POCs, strictly following SOLID may over-engineer the solution. For example, instead of introducing multiple interfaces for ISP, I may use one interface for simplicity if I know the scope is small and won’t evolve much.
👉 The key is balance between principles and practicality.


***Single Responsibility Principle (SRP)***

Q6. Provide an example of a class violating SRP and how you would refactor it in C#.
```cs
❌ Violating SRP:

public class Invoice {
    public void CalculateTotal() { /* logic */ }
    public void SaveToDatabase() { /* logic */ }
    public void PrintInvoice() { /* logic */ }
}


✅ Refactored with SRP:

public class Invoice { public void CalculateTotal() { /* logic */ } }
public class InvoiceRepository { public void SaveToDatabase(Invoice inv) { /* logic */ } }
public class InvoicePrinter { public void Print(Invoice inv) { /* logic */ } }
```

Q7. How does SRP impact code reusability and testability?

Reusability → Each class does one thing, so it can be reused independently.

Testability → Unit tests are simpler and isolated. Testing InvoicePrinter doesn’t require a database.

***Open/Closed Principle (OCP)***

Q8. Illustrate OCP with a C# example.

✅ Example:
```cs
public interface IDiscount {
    decimal ApplyDiscount(decimal amount);
}

public class NoDiscount : IDiscount {
    public decimal ApplyDiscount(decimal amount) => amount;
}

public class PercentageDiscount : IDiscount {
    private readonly decimal percent;
    public PercentageDiscount(decimal percent) { this.percent = percent; }
    public decimal ApplyDiscount(decimal amount) => amount - (amount * percent);
}
```

Here, adding a new discount type requires extension, not modification.

Q9. How do design patterns like Strategy or Template Method relate to OCP?

Strategy Pattern → Defines a family of algorithms (extension without modification).

Template Method → Defines the skeleton of an algorithm and lets subclasses override steps.
👉 Both enforce OCP by allowing behavior change without altering base code.

***Liskov Substitution Principle (LSP)***

Q10. Provide a C# example where LSP is violated.
```cs
❌ Violation:

public class Bird { public virtual void Fly() { } }
public class Ostrich : Bird { public override void Fly() { throw new Exception("Can't fly"); } }


Client code expecting Bird.Fly() breaks for Ostrich.

✅ Fix:

public abstract class Bird { }
public class FlyingBird : Bird { public virtual void Fly() { } }
public class Ostrich : Bird { public void Run() { } }
```

Q11. How does LSP ensure correct behavior when using inheritance?
LSP ensures that a derived class can replace a base class without breaking client expectations. It maintains behavioral consistency.

**Interface Segregation Principle (ISP)**

Q12. Demonstrate ISP with a C# example.
```cs
❌ Fat interface:

public interface IWorker {
    void Work();
    void Eat();
}
public class Robot : IWorker {
    public void Work() { }
    public void Eat() { throw new NotImplementedException(); }
}


✅ ISP:

public interface IWorkable { void Work(); }
public interface IFeedable { void Eat(); }

public class Robot : IWorkable { public void Work() { } }
public class Human : IWorkable, IFeedable {
    public void Work() { }
    public void Eat() { }
}
```

Q13. How does ISP prevent "fat interfaces" and improve client code?
It ensures clients only depend on what they use. No unnecessary dependencies → less coupling, better flexibility.

Dependency Inversion Principle (DIP)

Q14. Show how to apply DIP in C# using dependency injection.
```cs
public interface IMessageService {
    void Send(string message);
}

public class EmailService : IMessageService {
    public void Send(string message) => Console.WriteLine($"Email: {message}");
}

public class Notification {
    private readonly IMessageService _service;
    public Notification(IMessageService service) { _service = service; }
    public void Notify(string msg) => _service.Send(msg);
}
```

Here, Notification depends on the abstraction IMessageService, not the concrete EmailService.

Q15. How does DIP promote loose coupling and make code more testable?

By depending on interfaces, not concrete classes, DIP enables swapping implementations (e.g., Email, SMS, Mock Service for testing).

This reduces coupling and allows easier unit testing.

***🔹 Scenario-Based Questions***
Q16. You are given a legacy C# codebase. How would you identify areas where SOLID principles are violated and what steps would you take to refactor them?

Look for God classes (SRP violation).

If adding features requires editing old classes (OCP violation).

If derived classes throw exceptions for base methods (LSP violation).

If interfaces have unused methods (ISP violation).

If classes directly depend on concrete implementations (DIP violation).
👉 Refactor gradually using design patterns and automated tests.

Q17. Imagine you need to add a new payment gateway to an existing e-commerce application. How would you design the solution using SOLID principles?

SRP → Separate Payment Processing, Logging, and Persistence.

OCP → Use IPaymentGateway interface; new gateways extend without modifying old code.

LSP → Ensure all gateways respect the same contract.

ISP → Split interfaces for Refund, Capture, Authorize if needed.

DIP → Inject dependencies via abstractions.

Q18. Discuss a situation where you had to make a design decision that involved a trade-off between adhering strictly to a SOLID principle and meeting project deadlines or performance requirements.

Answer Example:
“In a past project, we had to implement file export functionality quickly. Ideally, I should have applied OCP and Strategy Pattern for different file formats (PDF, Excel, CSV). But due to deadlines, I implemented it in a single class. Later, when requirements stabilized, I refactored into separate strategies. Sometimes, delivery speed outweighs perfect design, but refactoring later is key.”