# Interface Segregation Principle Example

The **Interface Segregation Principle (ISP)** is a principle of object-oriented design that states that clients should not be forced to depend on interfaces they do not use. This means that we should create small, focused interfaces that contain only the methods and properties that are necessary for a particular client, rather than creating large, monolithic interfaces that contain everything.

In JavaScript, we can apply the Interface Segregation Principle by using object-oriented programming techniques like interfaces and mixins. Here is an example of how we can apply the Interface Segregation Principle to a class hierarchy:

```js
class Printable {
  print() {
    throw new Error('Method not implemented');
  }
}

class Readable {
  read() {
    throw new Error('Method not implemented');
  }
}

class DocumentPrinter {
  printDocument(document) {
    if (document instanceof Printable) {
      document.print();
    } else {
      throw new Error('Document is not printable');
    }
  }
}

class DocumentReader {
  readDocument(document) {
    if (document instanceof Readable) {
      document.read();
    } else {
      throw new Error('Document is not readable');
    }
  }
}

class TextDocument extends Printable {
  constructor(text) {
    super();
    this.text = text;
  }

  print() {
    console.log(this.text);
  }
}

class BinaryDocument extends Readable {
  constructor(binaryData) {
    super();
    this.binaryData = binaryData;
  }

  read() {
    console.log(this.binaryData);
  }
}
```

In this example, we have two interfaces, *Printable* and *Readable*, that define the print and read methods, respectively. We then have two classes, *DocumentPrinter* and *DocumentReader*, that depend on these interfaces rather than concrete classes. The *DocumentPrinter* and DocumentReader classes use the instanceof operator to check whether a given document is *printable* or *readable*, and then call the appropriate method.

We also have two classes, *TextDocument* and *BinaryDocument*, that implement the Printable and Readable interfaces, respectively. These classes contain only the methods and properties that are necessary for their respective interfaces, rather than implementing large, monolithic interfaces that contain everything.

This example demonstrates how applying the Interface Segregation Principle can help make our code more flexible and maintainable. By creating small, focused interfaces, we can reduce the amount of coupling between classes and make our code more modular and extensible.


-------------------------


## 2nd Example

The dependency injection principle states that high level code should never depend on low level interfaces, and should instead use abstractions. It’s all about decoupling code.

Not following? I don’t blame you, but it’s surprisingly simple.

Let’s say we have a piece of software that runs an online store, and within that software one of the classes *(PurchaseHandler)* handles the final purchase. This class is capable of charging the user’s credit card, and does so by using a PayPal API:

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

Rather than directly call the PayPal API from our payment page, we’ll instead create another class called PaymentHandler. The interface for this class will remain the same no matter what underlying payment system we use, even if the two systems are completely different. We’ll still need to make changes to the PaymentHandler interface if we change payment processor, but our higher level interface remains unchanged.

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
