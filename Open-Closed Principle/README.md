# Open-Closed Principle Example

The **Open-Closed Principle (OCP)** is a principle of object-oriented design that states that software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. This means that we should design our software components so that they can be easily extended or modified without having to change the existing code.

In JavaScript, we can apply the *Open-Closed Principle* by using object-oriented programming techniques like inheritance and polymorphism. Here is an example of how we can apply the *Open-Closed Principle* to a class:

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

class Circle extends Shape {
  constructor(radius) {
    super();
    this.radius = radius;
  }

  area() {
    return Math.PI * this.radius ** 2;
  }
}
```

In this example, we have a `Shape` class with an `area` method that is not implemented. We then have two classes, `Rectangle` and `Circle`, that inherit from the `Shape` class and implement the area method.

If we want to add a new `shape` to our system, we can create a new class that inherits from the `Shape` class and implements the `area` method. We do not need to modify the existing `Shape`, `Rectangle`, or `Circle` classes.

This example demonstrates how applying the **Open-Closed Principle** can help make our code more flexible and extensible. By designing our classes to be open for extension but closed for modification, we can add new functionality to our system without modifying existing code, which can help reduce the risk of introducing bugs and errors.


-------------------------


## 2nd Example

The **open-closed principle** says that code should be open for extension, but closed for modification. What this means is that if we want to add additional functionality, we should be able to do so simply by extending the original functionality, without the need to modify it.

To explain this, let’s look at an example. Below we have a Vehicle class. When a Vehicle instance is created, we pass in the fuel capacity and fuel efficiency. To get our range, we simply multiply our capacity by our efficiency.

```js
class Vehicle {
    constructor(fuelCapacity, fuelEfficiency) {
        this.fuelCapacity = fuelCapacity;
        this.fuelEfficiency = fuelEfficiency;
    }

    getRange() {
        return this.fuelCapacity * this.fuelEfficiency;
    }
}

const standardVehicle = new Vehicle(10, 15);

console.log(standardVehicle.getRange()); // Outputs '150'
```
But let’s say we add a new type of vehicle; a hybrid vehicle. This vehicle doesn’t just have standard fuel-based range, it also has an electric range which it can use as well. To find out the range now, we need to modify our `getRange()` method to check if the vehicle is hybrid, and add its electric range if so:

```js
class Vehicle {
    constructor(fuelCapacity, fuelEfficiency) {
        this.fuelCapacity = fuelCapacity;
        this.fuelEfficiency = fuelEfficiency;
    }

    getRange() {
        let range = this.fuelCapacity * this.fuelEfficiency;

        if (this instanceof HybridVehicle) {
            range += this.electricRange;
        }
        return range;
    }
}

class HybridVehicle extends Vehicle {
    constructor(fuelCapacity, fuelEfficiency, electricRange) {
        super(fuelCapacity, fuelEfficiency);
        this.electricRange = electricRange;
    }
}

const standardVehicle = new Vehicle(10, 15);
const hybridVehicle = new HybridVehicle(10, 15, 50);

console.log(standardVehicle.getRange()); // Outputs '150'
console.log(hybridVehicle.getRange()); // Outputs '200'
```
This violates the open-closed principle, because whilst adding our new HybridVehicle class we have had to go back and modify the code of our Vehicle class in order to make it work. Going forward, every time we add a new type of vehicle that might have different parameters for its range, we’ll have to continually modify that existing `getRange` function.

Instead what we could do, is to override the `getRange` method in the HybridVehicle class, giving the correct output for both Vehicle types, without every modifying the original code:

```js
class Vehicle {
    constructor(fuelCapacity, fuelEfficiency) {
        this.fuelCapacity = fuelCapacity;
        this.fuelEfficiency = fuelEfficiency;
    }

    getRange() {
        return this.fuelCapacity * this.fuelEfficiency;
    }
}

class HybridVehicle extends Vehicle {
    constructor(fuelCapacity, fuelEfficiency, electricRange) {
        super(fuelCapacity, fuelEfficiency);
        this.electricRange = electricRange;
    }

    getRange() {
        return (this.fuelCapacity * this.fuelEfficiency) + this.electricRange;
    }
}

const standardVehicle = new Vehicle(10, 15);
const hybridVehicle = new HybridVehicle(10, 15, 50);

console.log(standardVehicle.getRange()); // Outputs '150'
console.log(hybridVehicle.getRange()); // Outputs '200'
```