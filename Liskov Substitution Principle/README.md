# Liskov Substitution Principle Example

The **Liskov Substitution Principle** (LSP) is a principle of object-oriented design that states that subtypes must be substitutable for their base types. This means that objects of a derived class should be able to be used in place of objects of the base class without causing unexpected behavior.

In JavaScript, we can apply the **Liskov Substitution Principle** by making sure that our derived classes conform to the interface and behavior of their base classes. Here is an example of how we can apply the **Liskov Substitution Principle** to a class hierarchy:

```js
class Shape {
  area() {
    throw new Error('Method not implemented');
  }
}

class Rectangle extends Shape {
  constructor(length, width) {
    super();
    this.length = length;
    this.width = width;
  }

  area() {
    return this.length * this.width;
  }
}

class Square extends Shape {
  constructor(side) {
    super();
    this.side = side;
  }

  area() {
    return this.side ** 2;
  }
}

function printArea(shape) {
  console.log(`The area is ${shape.area()}`);
}

const rectangle = new Rectangle(10, 20);
const square = new Square(10);

printArea(rectangle); // Output: The area is 200
printArea(square); // Output: The area is 100
```

In this example, we have a `Shape` class with an `area` method that is not implemented. We then have two classes, `Rectangle` and `Square`, that inherit from the `Shape` class and implement the `area` method.

Both `Rectangle` and `Square` conform to the interface and behavior of the `Shape` class, which means that objects of `Rectangle` and `Square` can be used in place of objects of the `Shape` class without causing unexpected behavior.

The `printArea` function takes an object of the `Shape` class as a parameter and calls its `area` method. The function can be called with objects of `Rectangle` and `Square`, which demonstrates that these objects are substitutable for objects of the `Shape` class.

This example demonstrates how applying the **Liskov Substitution Principle** can help make our code more flexible and interchangeable. By designing our classes to be substitutable for their base classes, we can create code that is more modular and easier to maintain.


-------------------------


## 2nd Example

The **Liskov substitution principle** states that any class should be substitutable for its parent class without unexpected consequences. In others words, if the classes `Cat` and `Dog` extend the class `Animal`, then we would expect all of the functionality held within the `Animal` class to behave normally for a `Cat` and `Dog` object.

A classic example of a Liskov substitution violation is the "`square` & `rectangle` problem”. In this problem, it is posed that a `Square` class can inherit from a `Rectangle` class. On the face of it, this makes sense; both shapes have two sides, and both calculate their area by multiplying their sides by each other.

But the problem arises when we try to utilise some `Rectangle` functionality on a `Square` object. Let’s look at an example:

```js
class Rectangle {
    constructor(height, width) {
        this.height = height;
        this.width = width;
    }

    setHeight(newHeight) {
        this.height = newHeight;
    }
}

class Square extends Rectangle {}

const rectangle = new Rectangle(4, 5);
const square = new Square(4, 4);

console.log(`Height: ${rectangle.height}, Width: ${rectangle.width}`); // Outputs 'Height: 4, Width: 5' (correct)
console.log(`Height: ${square.height}, Width: ${square.width}`); // Outputs 'Height: 4, Width: 4' (correct)

square.setHeight(5);
console.log(`Height: ${square.height}, Width: ${square.width}`); // Outputs 'Height: 5, Width: 4' (wrong)
```
In this example we initialise a `Rectangle` and `Square`, and output their dimensions. We then call the `Rectangle`.`setHeight`() on the `Square` object, and output its dimensions again. What we find is that the `square` now has a different height than its length, which of course makes for an invalid `square`.

This can be solved, using polymorphism, an if statement in the `Rectangle` class, or a variety of other methods. But the real cause of the issue is that `Square` is not a good child class of `Rectangle`, and that in reality, perhaps both shapes should inherit from a Shape class instead.
