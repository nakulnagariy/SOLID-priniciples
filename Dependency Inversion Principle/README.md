# Dependency Inversion Principle Example

The **Dependency Inversion Principle** (DIP) is a principle of object-oriented design that states that high-level modules should not depend on low-level modules. Instead, both high-level and low-level modules should depend on abstractions. This means that we should design our software components so that they can be easily extended or modified without breaking the system.

In JavaScript, we can apply the **Dependency Inversion Principle** by using abstractions like interfaces and dependency injection. Here is an example of how we can apply the **Dependency Inversion Principle** to a class hierarchy:

```js
class Database {
  save(data) {
    throw new Error('Method not implemented');
  }
}

class MySQLDatabase extends Database {
  save(data) {
    console.log(`Saving ${data} to MySQL database`);
  }
}

class PostgreSQLDatabase extends Database {
  save(data) {
    console.log(`Saving ${data} to PostgreSQL database`);
  }
}

class DataProcessor {
  constructor(database) {
    this.database = database;
  }

  processData(data) {
    this.database.save(data);
  }
}
```

In this example, we have two concrete implementations of the `Database` class, `MySQLDatabase` and `PostgreSQLDatabase`, that implement the save method. We also have a `DataProcessor` class that depends on the `Database` class to save data.

Instead of depending on a concrete implementation of the `Database` class, the `DataProcessor` class depends on an abstraction (i.e., the `Database` class). The `DataProcessor` class takes a `Database` object as a constructor parameter, which allows us to inject different implementations of the `Database` class at runtime.

By using dependency injection and depending on abstractions rather than concrete implementations, we can create code that is more flexible and easier to maintain. We can easily extend the `DataProcessor` class by adding new implementations of the `Database` class without breaking the existing code. This makes our code more modular and extensible.


-------------------------


## 2nd Example

The dependency injection principle states that high level code should never depend on low level interfaces, and should instead use abstractions. It’s all about decoupling code.

Not following? I don’t blame you, but it’s surprisingly simple.

Let’s say we have a piece of software that runs an online store, and within that software one of the classes (`PurchaseHandler`) handles the final purchase. This class is capable of charging the user’s credit card, and does so by using a PayPal API:

```js
class PurchaseHandler {
    processPayment(paymentDetails, amount) {
        // Complicated, PayPal specific logic goes here
        const paymentSuccess = PayPal.requestPayment(paymentDetails, amount);

        if (paymentSuccess) {
            // Do something
            return true;
        }

        // Do something
        return false;
    }
}
```
The problem here is that if we change from PayPal to Square (another payment processor) in 6 months time, this code breaks. We need to go back and swap out our PayPal API calls for Square API calls. But in addition, what if the Square API wants different types of data? Or perhaps it wants us to “stage” a payment first, and then to process it once staging has completed?

That’s bad, and so we need to abstract the functionality out instead.

Rather than directly call the PayPal API from our payment page, we’ll instead create another class called `PaymentHandler`. The interface for this class will remain the same no matter what underlying payment system we use, even if the two systems are completely different. We’ll still need to make changes to the `PaymentHandler` interface if we change payment processor, but our higher level interface remains unchanged.

```js

class PurchaseHandler {
    processPayment(paymentDetails, amount) {
        const paymentSuccess = PaymentHandler.requestPayment(
            paymentDetails,
            amount
        );

        if (paymentSuccess) {
            // Do something
            return true;
        }

        // Do something
        return false;
    }
}

class PaymentHandler {
    requestPayment(paymentDetails, amount) {
        // Complicated, PayPal specific logic goes here
        return PayPal.requestPayment(paymentDetails, amount);
    }
}
```

Now you may be looking at this and thinking “but wait, that’s way more code”, and you’d be right. Like many of the SOLID principles (and indeed OO principles in general), the objective is less about writing less code or writing it quicker, and more about writing better code. The above change will save you days or maybe even weeks further down the line, in exchange for spending a few hours on it now.