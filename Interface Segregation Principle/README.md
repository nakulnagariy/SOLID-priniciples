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

The *interface segregation principle* states that an entity should never be forced to implement an interface that contains elements which it will never use. For example, a Penguin should never be forced to implement a Bird interface if that Bird interface includes functionality relating to flying, as penguins (spoiler alert) cannot fly.

Now, this functionality is a little more difficult to demonstrate using JavaScript, due to its lack of interfaces. However, we can demonstrate it by using composition.

Composition is a subject all by itself, but I’ll give a very high level introduction: Instead of inheriting an entire class, we can instead add chunks of functionality to a class. Here’s an example that actually addresses the *interface segregation principle*:

```js
class Penguin {}

class Bird {}

const flyer = {
    fly() {
        console.log(`Flap flap, I'm flying!`);
    },
};

Object.assign(Bird.prototype, flyer);

const bird = new Bird();
bird.fly(); // Outputs 'Flap flap, I'm flying!'

const penguin = new Penguin();
penguin.fly(); // 'Error: penguin.fly is not a function'
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
```
What this example does is to add the flying functionality (or interface) only to the class(es) that require it. This means that penguins won’t be given the ability to fly, whereas birds will.

This is one method of adhering to the interface segregation principle, but it is a fairly rough example (as, once again, JavaScript doesn’t play well with interfaces).
