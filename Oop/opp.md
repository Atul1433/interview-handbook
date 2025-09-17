## Object-Oriented Programming (OOP) Principles

OOP is a programming paradigm based on the concept of objects, which represent real-world entities with state (data) and behavior (methods).
The 4 main principles of OOP are:
Encapsulation
Abstraction
Inheritance
Polymorphism

### 1. Encapsulation (Data Hiding + Controlled Access)
**Definition:**
Encapsulation is the process of hiding internal details of a class and only exposing necessary parts through public methods/properties. It keeps data safe from unauthorized access and ensures controlled interaction.

**Explanation:**
- Data (fields/variables) is kept private.
- Access is provided via public getters/setters (properties in C#).
- Improves security, maintainability, and flexibility.

**📖 Advanced Definition:**
Encapsulation is not just about private variables — it’s about binding data and behavior into a single unit and enforcing rules on how external code interacts with that data. In C#, encapsulation is implemented using access modifiers (public, private, protected, internal) along with properties, methods, and constructors.

**🎯 Interview Key Points:**

- Encapsulation ≠ Abstraction (Encapsulation is “how to hide data”, Abstraction is “what to expose”).
- Achieved using access modifiers.
- Supports immutability by exposing only getters.

**Example:**
```csharp
public class User
{
    private string passwordHash; // hidden implementation

    public string Username { get; private set; }

    public User(string username, string password)
    {
        Username = username;
        SetPassword(password);
    }

    // Encapsulation ensures security
    public void SetPassword(string password)
    {
        if (password.Length < 6)
            throw new Exception("Password too short");
        
        passwordHash = HashPassword(password);
    }

    public bool ValidatePassword(string password) =>
        passwordHash == HashPassword(password);

    private string HashPassword(string password) =>
        Convert.ToBase64String(System.Text.Encoding.UTF8.GetBytes(password));
}

```
👉 In interview: emphasize **“Encapsulation ensures we don’t expose raw passwords, but only expose safe methods for interaction.”**

### 2. Abstraction (Hiding Implementation)
**Definition:**
Abstraction is the concept of hiding implementation details and showing only essential features to the user.

**Explanation:**
- Achieved using abstract classes and interfaces.
- The consumer of the class only needs to know what it does, not how it does.
- Promotes loose coupling.

**📖 Advanced Definition:**
Abstraction is about defining contracts (what must be done) without exposing the actual implementation. In C#, it’s achieved using interfaces and abstract classes.

**🎯 Interview Key Points:**

- Abstract class vs Interface:
    - Abstract class: can have fields + implemented methods + abstract methods.
    - Interface: contract only (pre C# 8.0). C# 8 introduced default interface methods.
- Abstraction promotes loose coupling (dependencies interact via contracts, not concrete classes).
- Dependency Inversion Principle (SOLID) relies heavily on abstraction.

C# Example:
```cs
// Abstraction using interface
public interface IPayment
{
    void Pay(decimal amount); // Only behavior exposed
}

// Concrete implementation
public class CreditCardPayment : IPayment
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount} using Credit Card.");
    }
}

public class UpiPayment : IPayment
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount} using UPI.");
    }
}

// High-level module depending on abstraction
public class CheckoutService
{
    private readonly IPayment _payment;

    public CheckoutService(IPayment payment)
    {
        _payment = payment;
    }

    public void ProcessOrder(decimal amount)
    {
        Console.WriteLine("Processing order...");
        _payment.Pay(amount);
    }
}

// Abstraction using abstract class
public abstract class Payment
{
    public abstract void Pay(decimal amount); // Abstract method to be implemented by subclasses
}

// Concrete implementation: Credit Card
public class CreditCardPayment : Payment
{
    public override void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount} using Credit Card.");
    }
}

// Concrete implementation: UPI
public class UpiPayment : Payment
{
    public override void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount} using UPI.");
    }
}

```
👉 In interview: emphasize **“Abstraction allows my CheckoutService to depend on IPayment, not concrete classes. This follows Dependency Inversion and allows easy extensibility.”**

### 3. Inheritance (Code Reuse + Hierarchy)

**Definition:** Inheritance is a mechanism where a child class (derived class) can reuse fields and methods from a parent class (base class).

**Explanation:**

- Promotes code reuse.
- Establishes an “is-a” relationship.
- Supports hierarchical classification.

**📖 Advanced Definition:**
Inheritance allows a derived class to reuse, extend, or override behavior of a base class. But overusing inheritance leads to tight coupling — that’s why composition is often preferred (“favor composition over inheritance”).

**🎯 Interview Key Points:**

- Supports single inheritance in C# (a class can inherit only one base class).
- Interfaces allow multiple inheritance (class can implement multiple interfaces).
- Use protected members for inheritance scenarios.
- Virtual/abstract methods enable overriding.

C# Example:
```cs
public class Animal
{
    public string Name { get; set; }
    public void Eat() => Console.WriteLine($"{Name} is eating.");
    public virtual void MakeSound() => Console.WriteLine("Generic sound");
}

public class Dog : Animal
{
    public override void MakeSound() => Console.WriteLine("Bark!");
}

public class Cat : Animal
{
    public override void MakeSound() => Console.WriteLine("Meow!");
}

```
👉 In interview: emphasize “Inheritance allows Dog and Cat to reuse Eat() while customizing MakeSound(). But if I only need behavior sharing, I’d prefer interfaces or composition to avoid tight coupling.”

### 4. Polymorphism

**Definition:** Polymorphism means one entity having many forms. In OOP, it allows a method to behave differently based on the object calling it.

**Types:**

**Compile-time (Method Overloading)**
- Same method name, different parameter list.

**Runtime (Method Overriding)**
- Derived class provides its own implementation of a base class method (using virtual & override).

**📖 Advanced Definition:**
Polymorphism allows objects to be treated as instances of their base type, but execute their actual overridden behavior at runtime. In C#, achieved via method overriding (virtual/override) and interfaces.

**🎯 Interview Key Points:**
- Compile-time polymorphism = method overloading, operator overloading.
- Runtime polymorphism = virtual/override, interface implementation.
- Polymorphism is the backbone of design patterns (Factory, Strategy, Template Method).

C# Example (Overloading & Overriding):
```cs
public abstract class Shape
{
    public abstract void Draw();
}

public class Circle : Shape
{
    public override void Draw() => Console.WriteLine("Drawing Circle");
}

public class Square : Shape
{
    public override void Draw() => Console.WriteLine("Drawing Square");
}

// Usage
class Program
{
    static void Main()
    {
        List<Shape> shapes = new List<Shape> { new Circle(), new Square() };

        foreach (var shape in shapes)
            shape.Draw(); // Actual method chosen at runtime
    }
}


```

👉 In interview: emphasize **“Polymorphism allows my code to treat all shapes uniformly but execute specific behavior at runtime. This makes my code open for extension but closed for modification (OCP from SOLID).”**

### 🎯 Quick Recap for Interviews

**Encapsulation** → “Protects internal state using access modifiers; enforces rules for safe interaction.”

**Abstraction** → “Defines contracts using interfaces/abstract classes; hides implementation and reduces coupling.”

**Inheritance** → “Reuses base functionality, but should be used carefully — composition is often better.”

**Polymorphism** → “Allows objects to behave differently while sharing the same interface; key for extensibility.”