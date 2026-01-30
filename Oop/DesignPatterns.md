### Creational Design Patterns 
**Defination :** The Creational Design Patterns in C# focus on centralizing and simplifying object creation, reducing tight coupling, and making systems more flexible. Creational patterns abstract the instantiation process, so client code doesn’t depend on concrete classes.
They include Singleton, Factory Method, Abstract Factory, Prototype, and Builder patterns—each solving specific object instantiation problems.

**Problem Solved:** Prevents complicated client logic when multiple objects (Customer, Product, Invoice, Payment, etc.) are created under different conditions.

**When to Use:**
- When system configuration decides object creation dynamically.
- When families of related objects must work together.
- When managing expensive resources (e.g., DB connections).
- When hiding implementation details of object pools.

### 🏗 Types of Creational Design Patterns
1. **Singleton:** Ensures only one instance of a class exists. \
Use Case: Logging, caching, configuration settings.
Extra Note: Thread-safe Singleton often uses Lazy<T> in .NET.
2. **Factory Method:**
Defines an interface for creating objects, but lets subclasses decide which class to instantiate. \
Use Case: Payment gateway selection (CreditCardFactory, UPIFactory).
3. **Abstract Factory:**
Creates families of related objects without specifying concrete classes. \
Use Case: UI themes (DarkThemeFactory, LightThemeFactory).

4. **Prototype:**
Creates new objects by cloning an existing prototype. \
Use Case: Document templates, game character cloning.

Extra Note: Supports shallow and deep copy.

5. **Builder:**
Separates construction from representation, allowing step-by-step creation. \
Use Case: Building complex invoices with optional discounts, taxes, and line items.

Extra Note: Often combined with Fluent Interface for readability.

| Pattern          | Purpose                         | Example Use Case   |
| ---------------- | ------------------------------- | ------------------ |
| Singleton        | One global instance             | Logging service    |
| Factory Method   | Delegate creation to subclasses | Payment gateways   |
| Abstract Factory | Families of related objects     | UI themes          |
| Prototype        | Clone existing object           | Document templates |
| Builder          | Step-by-step construction       | Complex invoices   |

🏗 Factory Design Pattern -- Key Points
--------------------------------------

-   **Definition**: Encapsulates object creation logic in a centralized factory class, returning objects via a common interface.

-   **Problem Solved**: Avoids tight coupling between client code and concrete classes. Adding new types doesn't require modifying client logic.

-   **When to Use**:
    -   Multiple related classes implement a common interface.
    -   Object creation depends on runtime conditions (e.g., user choice, configuration).
    -   You want **loose coupling** and **scalability**.
📘 UML Components
-----------------
-   **Product Interface** → Defines contract (e.g., `IPaymentGateway`).
-   **Concrete Products** → Implementations (e.g., `PayPalGateway`, `StripeGateway`).
-   **Factory Class** → Centralized creator (`PaymentGatewayFactory`).
-   **Client** → Consumes products without knowing creation details.
~~~
using System;

namespace FactoryDesignPatternDemo

{
    // Step 1: Product Interface
    public interface IPaymentGateway
    {
        void ProcessPayment(decimal amount);
    }
    // Step 2: Concrete Products

    public class PayPalGateway : IPaymentGateway
    {
        public void ProcessPayment(decimal amount)
        {
            Console.WriteLine($"Processing {amount:C} via PayPal...");
        }
    }
    public class StripeGateway : IPaymentGateway
    {
        public void ProcessPayment(decimal amount)
        {
            Console.WriteLine($"Processing {amount:C} via Stripe...");
        }
    }
    public class CreditCardGateway : IPaymentGateway
    {
        public void ProcessPayment(decimal amount)
        {
            Console.WriteLine($"Processing {amount:C} via Credit Card...");
        }
    }

    // Step 3: Factory Class
    public static class PaymentGatewayFactory
    {
        public static IPaymentGateway CreatePaymentGateway(string gatewayType)
        {
            switch (gatewayType.ToLower())
            {
                case "paypal": return new PayPalGateway();
                case "stripe": return new StripeGateway();
                case "creditcard": return new CreditCardGateway();
                default: throw new ArgumentException("Invalid gateway type");
            }
        }
    }
    // Step 4: Client Code
    class Program
    {
        static void Main()
        {
            Console.WriteLine("Enter Payment Gateway (PayPal / Stripe / CreditCard):");
            string choice = Console.ReadLine();
            IPaymentGateway gateway = PaymentGatewayFactory.CreatePaymentGateway(choice);
            gateway.ProcessPayment(1000.00M);
            Console.ReadKey();
        }
    }
}
~~~

🎯 Real-Time Examples You Can Mention
-------------------------------------
1.  **Payment Gateway Integration** -- Multiple gateways (PayPal, Stripe, CreditCard).
2.  **Document Conversion System** -- Converters for PDF, DOCX, TXT.
3.  **Logging System** -- Console logger, file logger, DB logger.
4.  **Notification System** -- Email, SMS, Push notifications.
5.  **Transport Application** -- Car, Bus, Bike selection.

📊 Advantages
-------------
-   **Loose Coupling** -- Client depends only on interface.
-   **Scalability** -- Easy to add new product types.
-   **Single Responsibility** -- Factory handles creation logic.
-   **Open/Closed Principle** -- Extend without modifying client code.

🚀 Interview Tip
----------------

When asked, **start with the definition**, then **draw a quick UML diagram**, and finally **walk through the Payment Gateway example**. End by mentioning **real-world relevance** (e.g., your EV charging project uses multiple payment gateways, so Factory Pattern is ideal).

## 2. Factory Method Design Pattern ##
**The Factory Method Design Pattern in C# defines an interface for creating objects but lets subclasses decide which class to instantiate. It's widely used in real-world applications like payment gateways, notification systems, and document converters because it promotes loose coupling and scalability.**

🏗 Factory Method Design Pattern -- Key Concepts
-----------------------------------------------

-   **Category**: Creational Design Pattern.
-   **Difference from Simple Factory**:
    -   *Simple Factory*: Centralized class decides which object to create.
    -   *Factory Method*: Abstract creator delegates instantiation to subclasses.

-   **Structure**:
    -   **Product** → Interface/abstract class (e.g., `CreditCard`).
    -   **ConcreteProduct** → Implementations (`MoneyBack`, `Titanium`, `Platinum`).
    -   **Creator (Abstract Factory)** → Declares factory method (`MakeProduct`).
    -   **ConcreteCreator** → Subclasses override factory method to return specific product.

📘 Interview-Ready Example (Credit Card System)
-----------------------------------------------
```
using System;
namespace FactoryMethodDesignPattern
{
    // Product Interface
    public interface CreditCard
    {
        string GetCardType();
        int GetCreditLimit();
        int GetAnnualCharge();
    }
    // Concrete Products
    public class MoneyBack : CreditCard
    {
        public string GetCardType() => "MoneyBack";
        public int GetCreditLimit() => 15000;
        public int GetAnnualCharge() => 500;
    }
    public class Titanium : CreditCard
    {
        public string GetCardType() => "Titanium Edge";
        public int GetCreditLimit() => 25000;
        public int GetAnnualCharge() => 1500;
    }
    public class Platinum : CreditCard
    {
        public string GetCardType() => "Platinum Plus";
        public int GetCreditLimit() => 35000;
        public int GetAnnualCharge() => 2000;
    }
    // Abstract Creator
    public abstract class CreditCardFactory
    {
        protected abstract CreditCard MakeProduct();
        public CreditCard CreateProduct() => MakeProduct();
    }
    // Concrete Creators
    public class MoneyBackFactory : CreditCardFactory
    {
        protected override CreditCard MakeProduct() => new MoneyBack();
    }
    public class TitaniumFactory : CreditCardFactory
    {
        protected override CreditCard MakeProduct() => new Titanium();
    }
    public class PlatinumFactory : CreditCardFactory
    {
        protected override CreditCard MakeProduct() => new Platinum();
    }
    // Client
    class Program
    {
        static void Main()
        {
            CreditCard card = new PlatinumFactory().CreateProduct();
            Console.WriteLine($"Card Type: {card.GetCardType()}");
            Console.WriteLine($"Credit Limit: {card.GetCreditLimit()}");
            Console.WriteLine($"Annual Charge: {card.GetAnnualCharge()}");

            card = new MoneyBackFactory().CreateProduct();
            Console.WriteLine($"Card Type: {card.GetCardType()}");
            Console.WriteLine($"Credit Limit: {card.GetCreditLimit()}");
            Console.WriteLine($"Annual Charge: {card.GetAnnualCharge()}");
        }
    }
}
```
🎯 Real-Time Examples (from tutorial)
-------------------------------------
1.  **Notification System** -- Factories for Email, SMS, Push notifications.
2.  **Document Converters** -- Factories for PDF, DOCX, TXT export.
3.  **Report Generators** -- Chart, Tabular, Summary reports.
4.  **Payment Gateway Integration** -- CreditCard, PayPal, Bitcoin.
5.  **Logistics Application** -- Transport modes (Truck, Ship).
📊 Advantages
-------------
-   **Loose Coupling** -- Client depends only on abstract product.
-   **Open/Closed Principle** -- Add new products without modifying client code.
-   **Single Responsibility** -- Creation logic separated from business logic.
-   **Flexibility** -- Easy to extend with new product types.

⚠️ Trade-offs
-------------
-   **Subclass Proliferation** -- Each product requires a new factory class.
-   **Complexity** -- Can be overkill for simple scenarios.
-   **Indirect Code Flow** -- Object creation logic is less obvious.

## 3. Abstract Factory ##
The Abstract Factory Design Pattern in C# is a "factory of factories" that lets you create families of related objects without specifying their concrete classes. It's especially useful when your system needs to work with multiple product families (e.g., Regular vs. Sports vehicles, or CreditCard vs. PayPal gateways).

🏗 Abstract Factory Pattern -- Key Concepts
------------------------------------------

-   **Category**: Creational Design Pattern.

-   **Definition (GoF)**: Provides an interface for creating families of related or dependent objects without specifying their concrete classes.

-   **Purpose**: Decouple client code from concrete implementations, ensuring consistency across product families.

-   **Structure**:

    -   **AbstractFactory** → Declares methods for creating abstract products.

    -   **ConcreteFactory** → Implements creation of concrete products.

    -   **AbstractProduct** → Defines product interfaces.

    -   **ConcreteProduct** → Implements product interfaces.

    -   **Client** → Uses only abstract interfaces, not concrete classes.

📘 Example -- Vehicle Factory (Tutorial Example)
-----------------------------------------------
~~~
// Abstract Products
public interface IBike { void GetDetails(); }
public interface ICar { void GetDetails(); }

// Concrete Products
public class RegularBike : IBike { public void GetDetails() => Console.WriteLine("Regular Bike"); }
public class SportsBike : IBike { public void GetDetails() => Console.WriteLine("Sports Bike"); }
public class RegularCar : ICar { public void GetDetails() => Console.WriteLine("Regular Car"); }
public class SportsCar : ICar { public void GetDetails() => Console.WriteLine("Sports Car"); }

// Abstract Factory
public interface IVehicleFactory
{
    IBike CreateBike();
    ICar CreateCar();
}

// Concrete Factories
public class RegularVehicleFactory : IVehicleFactory
{
    public IBike CreateBike() => new RegularBike();
    public ICar CreateCar() => new RegularCar();
}

public class SportsVehicleFactory : IVehicleFactory
{
    public IBike CreateBike() => new SportsBike();
    public ICar CreateCar() => new SportsCar();
}

// Client
class Program
{
    static void Main()
    {
        IVehicleFactory factory = new RegularVehicleFactory();
        factory.CreateBike().GetDetails();
        factory.CreateCar().GetDetails();

        factory = new SportsVehicleFactory();
        factory.CreateBike().GetDetails();
        factory.CreateCar().GetDetails();
    }
}
~~~
Real-Time Example -- Payment Gateway (E-commerce)
------------------------------------------------
~~~
// Abstract Products
public interface IPaymentAuthorization { bool AuthorizePayment(decimal amount); }
public interface IPaymentTransfer { bool Transfer(decimal amount); }

// Concrete Products
public class CreditCardAuthorization : IPaymentAuthorization
{
    public bool AuthorizePayment(decimal amount) { Console.WriteLine($"Authorize {amount} via Credit Card"); return true; }
}
public class CreditCardTransfer : IPaymentTransfer
{
    public bool Transfer(decimal amount) { Console.WriteLine($"Transfer {amount} via Credit Card"); return true; }
}

public class PayPalAuthorization : IPaymentAuthorization
{
    public bool AuthorizePayment(decimal amount) { Console.WriteLine($"Authorize {amount} via PayPal"); return true; }
}
public class PayPalTransfer : IPaymentTransfer
{
    public bool Transfer(decimal amount) { Console.WriteLine($"Transfer {amount} via PayPal"); return true; }
}

// Abstract Factory
public interface IPaymentFactory
{
    IPaymentAuthorization CreateAuthorization();
    IPaymentTransfer CreateTransfer();
}

// Concrete Factories
public class CreditCardPaymentFactory : IPaymentFactory
{
    public IPaymentAuthorization CreateAuthorization() => new CreditCardAuthorization();
    public IPaymentTransfer CreateTransfer() => new CreditCardTransfer();
}
public class PayPalPaymentFactory : IPaymentFactory
{
    public IPaymentAuthorization CreateAuthorization() => new PayPalAuthorization();
    public IPaymentTransfer CreateTransfer() => new PayPalTransfer();
}

// Client
public class PaymentProcessor
{
    private readonly IPaymentAuthorization _auth;
    private readonly IPaymentTransfer _transfer;

    public PaymentProcessor(IPaymentFactory factory)
    {
        _auth = factory.CreateAuthorization();
        _transfer = factory.CreateTransfer();
    }

    public void Process(decimal amount)
    {
        if (_auth.AuthorizePayment(amount))
            _transfer.Transfer(amount);
    }
}

class Program
{
    static void Main()
    {
        var creditCardProcessor = new PaymentProcessor(new CreditCardPaymentFactory());
        creditCardProcessor.Process(500);

        var paypalProcessor = new PaymentProcessor(new PayPalPaymentFactory());
        paypalProcessor.Process(1000);
    }
}
~~~

