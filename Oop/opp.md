## Object-Oriented Programming (OOP) Principles

OOP is a programming paradigm based on the concept of objects, which represent real-world entities with state (data) and behavior (methods).
The 4 main principles of OOP are:
Encapsulation
Abstraction
Inheritance
Polymorphism

### 1 Encapsulation
**Definition:**
Encapsulation is the process of hiding internal details of a class and only exposing necessary parts through public methods/properties. It keeps data safe from unauthorized access and ensures controlled interaction.

**Explanation:**
- Data (fields/variables) is kept private.
- Access is provided via public getters/setters (properties in C#).
- Improves security, maintainability, and flexibility.

**Real-world analogy:**
Think of a capsule medicine – you only swallow the capsule, you don’t need to know its internal composition.
**Example:**
```csharp
public class BankAccount
{
    private decimal balance; // hidden data

    public BankAccount(decimal initialBalance)
    {
        balance = initialBalance;
    }

    // Encapsulation through property
    public decimal Balance
    {
        get { return balance; }  // controlled access
        private set { balance = value; } // restrict modification
    }

    public void Deposit(decimal amount)
    {
        if (amount > 0)
            balance += amount;
    }

    public void Withdraw(decimal amount)
    {
        if (amount > 0 && amount <= balance)
            balance -= amount;
    }
}
```
**How to explain in interview:**
👉 “Encapsulation means wrapping data and methods together, restricting direct access to internal details. For example, in my BankAccount class, the balance field is private, and I expose it via public methods like Deposit and Withdraw. This way, I control how the data is accessed and modified.”

### Abstraction
**Definition:**
Abstraction is the concept of hiding implementation details and showing only essential features to the user.

**Explanation:**
- Achieved using abstract classes and interfaces.
- The consumer of the class only needs to know what it does, not how it does.
- Promotes loose coupling.

**Real-world analogy:**
When you drive a car, you use the steering wheel, brakes, and accelerator. You don’t need to know the internal combustion process or engine wiring.

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
### Inheritance

**Definition:** Inheritance is a mechanism where a child class (derived class) can reuse fields and methods from a parent class (base class).

**Explanation:**

- Promotes code reuse.
- Establishes an “is-a” relationship.
- Supports hierarchical classification.

**Real-world analogy:**
A Dog is a type of Animal. Dog inherits features of Animal (like breathing, eating) but also has its own behavior (like barking).

C# Example:
```cs
// Base class
public class Animal
{
    public void Eat() => Console.WriteLine("Eating...");
}

// Derived class
public class Dog : Animal
{
    public void Bark() => Console.WriteLine("Barking...");
}

// Usage
class Program
{
    static void Main()
    {
        Dog dog = new Dog();
        dog.Eat();  // inherited
        dog.Bark(); // own behavior
    }
}
```
**How to explain in interview:**
👉 “Inheritance allows a class to reuse code from another class. For instance, Dog inherits from Animal, so it automatically has the ability to Eat without rewriting the logic. It also has its own behavior like Bark.”

### 4️⃣ Polymorphism

**Definition:** Polymorphism means one entity having many forms. In OOP, it allows a method to behave differently based on the object calling it.

**Types:**

**Compile-time (Method Overloading)**
- Same method name, different parameter list.

**Runtime (Method Overriding)**
- Derived class provides its own implementation of a base class method (using virtual & override).

**Real-world analogy:**
The word “Draw” can mean: draw a shape, draw money, draw attention. Same word, different meaning depending on context.

C# Example (Overloading & Overriding):
```cs
// Compile-time Polymorphism (Overloading)
public class Calculator
{
    public int Add(int a, int b) => a + b;
    public double Add(double a, double b) => a + b;
}

// Runtime Polymorphism (Overriding)
public class Shape
{
    public virtual void Draw()
    {
        Console.WriteLine("Drawing a shape...");
    }
}

public class Circle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a Circle...");
    }
}

public class Square : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a Square...");
    }
}

```

**How to explain in interview:**
👉 “Polymorphism allows the same method to take different forms. In my example, the Add method in Calculator is overloaded to handle both integers and doubles (compile-time). For runtime, I have a base class Shape with a virtual method Draw. Derived classes like Circle and Square override it to provide their own implementations. The correct method is chosen at runtime.”

### 🎯 Quick Recap for Interviews

**Encapsulation** → Protects internal data (Bank Account).

**Abstraction** → Hides implementation, shows only functionality (Payment system).

**Inheritance** → Reuse parent features (Animal → Dog).

**Polymorphism** → One method, many forms (Overloading & Overriding).