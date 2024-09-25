<div align="center"> 
  <h1>20 Days of Basics TypeScript: Classes 2</h1>
</div>

<div align="center"> 

<!-- Social links -->
[![Discord](https://img.shields.io/badge/Discord-%237289DA.svg?logo=discord&logoColor=white)](htttps://discord.gg/Samson#0273) [![Facebook](https://img.shields.io/badge/Facebook-%231877F2.svg?logo=Facebook&logoColor=white)](https://www.facebook.com/chiemezie.nebeolisa/) [![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?logo=Instagram&logoColor=white)](https://www.instagram.com/samson_nebeolisa/) [![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/chiemezie-samson-nebeolisa-32897310b/) [![Stack Overflow](https://img.shields.io/badge/-Stackoverflow-FE7A16?logo=stack-overflow&logoColor=white)](https://stackoverflow.com/users/20653301/nebeolisa-chiemezie-samson) [![Twitter](https://img.shields.io/badge/Twitter-%231DA1F2.svg?logo=Twitter&logoColor=white)](https://twitter.com/SamsonChiemezie) [![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?logo=YouTube&logoColor=white)](https://myaccount.google.com/u/0/?utm_source=YouTubeWeb&tab=rk&utm_medium=act&tab=rk&hl=en) 

<!-- Portfolio -->
 📰 About Me [Portfolio](https://www.nebe-samson.com/)
 <br/>
  <small>Sep, 2024</small>
</div>

[<< Day 13](../Day13_Classes_1/Day13.md) | [Day 15 >>](../Day15_Modules/Day15.md)

<div align="center"> 
  <a class="header-image" target="_blank" href="../Asset/images/Days/Day_14.webp">
    <img alt="Typescript image" src="../Asset/images/Days/Day_14.webp" width="100%" height="600px">
  </a>
</div>

## Table of Contents

- [📔 Day 14](#-day-14)
- [Classes in TypeScript](#classes-in-typescript)
- [Access Modifiers in TypeScript](#access-modifiers-in-typescript)
  - [`public` Modifier](#public-modifier)
  - [`private` Modifier](#private-modifier)
  - [`protected` Modifier](#protected-modifier)
  - [A Special Modifier: `readonly`](#a-special-modifier-readonly)
  - [Combining Access Modifiers](#combining-access-modifiers)
- [Static Members](#static-members)
  - [Why Use Static Members?](#why-use-static-members)
  - [Do We Really Need Static Classes?](#do-we-really-need-static-classes)
- [Generic Classes](#generic-classes)
  - [Building a Generic Stack](#building-a-generic-stack)
- [Abstract Classes](#abstract-classes)
  - [Extending an Abstract Class](#extending-an-abstract-class)
  - [Key Points About Abstract Classes](#key-points-about-abstract-classes)
  - [Real-World Use Case](#real-world-use-case)
- [💻 Day 14: Exercises](#-day-14-exercises)
  - [Exercise: Level 1](#exercise-level-1)
  - [Exercise: Level 2](#exercise-level-2)
  - [Exercise: Level 3](#exercise-level-3)


# 📔 Day 14

##  Classes in TypeScript

## Access Modifiers in TypeScript

As a developer, it's essential to understand how to manage the visibility and accessibility of properties and methods within your classes. TypeScript provides three main access modifiers to control this: `public`, `private`, and `protected`. These modifiers allow us to define which parts of a class are accessible from outside or inside the class, and even within its subclasses. Let’s dive into each one.

### `public` Modifier

When a property or method is marked as `public`, it means that it can be accessed from anywhere — both inside and outside the class. In fact, if you don't specify any access modifier, TypeScript will treat the property or method as `public` by default.

```ts
  class PublicExample {
    public name: string; // This is a public property

    // public method
    public sayName(name: string) {
      this.name = name; // Accessing the public property
    }
  }

  const myPublicExample = new PublicExample();
  myPublicExample.name = "Samson"; // Can access and modify it outside the class
  myPublicExample.sayName("Nebeolisa"); // Can call the method from anywhere
```

### `private` Modifier

When you use the `private` modifier, you're telling TypeScript that the property or method should only be accessed within the class it is defined in. Not even subclasses can access these private members.

```ts
  class PrivateExample {
    private name: string; // This is a private property

    private sayName(name: string) {
      this.name = name; // You can use it within the class
    }
  }

  const myPrivateExample = new PrivateExample();
  myPrivateExample.name; // ❌ Error: Property 'name' is private and only accessible within class 'PrivateExample'.
  myPrivateExample.sayName("Samson"); // ❌ Error: Cannot call a private method from outside
```

With `private`, both properties and methods are fully encapsulated. The only way to interact with these members is through public methods defined within the class.

### `protected` Modifier

The `protected` modifier is like a middle ground between `private` and `public`. When something is `protected`, it means that it's not accessible from outside the class, but it can be accessed within the class and its subclasses.

```ts
  class ProtectedExample {
    protected name: string; // This is a protected property

    protected sayName(name: string) {
      this.name = name; // Accessing it within the class
    }
  }

  class SubclassExample extends ProtectedExample {
    public display() {
      console.log(this.name); // Can access protected property in a subclass
    }
  }

  const myProtectedExample = new ProtectedExample();
  myProtectedExample.name; // ❌ Error:  Property 'name' is protected and only accessible within class 'ProtectedExample' and its subclasses.

  const mySubclassExample = new SubclassExample();
  mySubclassExample.display(); // Can access protected property in a subclass method
```

### A Special Modifier: `readonly`

In addition to the primary access modifiers, TypeScript offers the `readonly` modifier, which adds another layer of control to your class properties. As we've seen in earlier lessons, `readonly` ensures that a property's value can only be set once — typically during initialization — and then it remains fixed throughout the lifespan of the object. 

```ts
  class ReadOnlyExample {
    public readonly id: number;

    constructor(id: number) {
      this.id = id; // Can set value in constructor
    }
  }

  const myReadOnlyExample = new ReadOnlyExample(101);
  console.log(myReadOnlyExample.id); // Can read it
  myReadOnlyExample.id = 102; // ❌ Error: Cannot assign to 'id' because it is a read-only property.
```

`readonly` is particularly useful when you want to ensure that some important data, like IDs, stay unchanged once set.

### Combining Access Modifiers

To see how access modifiers work together in a real-world class, let's create an Employee class:

```ts
  class Employee {
    private id: number; // Only accessible within this class
    protected department: string; // Accessible in this class and subclasses
    public name: string; // Accessible anywhere

    constructor(id: number, name: string, department: string) {
      this.id = id;
      this.name = name;
      this.department = department;
    }

    public introduce() {
      console.log(`Hi, I'm ${this.name} from the ${this.department} department.`);
    }

    private getEmployeeId() {
      return this.id; // Only accessible within this method
    }
  }

  const emp = new Employee(1, "Samson", "FrontEnd");
  emp.introduce(); // Works fine
  console.log(emp.id); // ❌ Error: Property 'id' is private
  console.log(emp.department); // ❌ Error: Property 'department' is protected
```

This setup ensures that sensitive details (like `id`) remain hidden, while general details (like `name`) are easily accessible.

## Static Members

When working with TypeScript classes, you may sometimes need properties or methods that belong to the class itself rather than to individual instances. This is where __static members__ come into play.

Static members are properties and methods that are tied to the class itself, not to any objects created from the class (instances). In simpler terms, you can think of them as shared utilities or values that are relevant to the class as a whole, rather than specific to individual instances of the class. Static members are accessible directly from the class and don't require you to create an instance of that class. 

```ts
  class MyBasicStatic {
    static x = 0; // A static property

    static printX() { // A static method
      console.log(MyBasicStatic.x); // Accessing the static property within the class
    }
  }

  // Accessing static members directly from the class
  console.log(MyBasicStatic.x); // Outputs: 0
  MyBasicStatic.printX(); // Outputs: 0
```

### Why Use Static Members?

Static members are perfect when you want to create shared functionality or data that doesn't rely on individual instances. For example, mathematical constants or utility functions often make sense as static members.

Let's see how static members can be useful in practice by creating a `MathUtils` class that holds a constant value for `PI` and a method to calculate the circumference of a circle.

```ts
  class MathUtils {
    static PI: number = 3.14159; // Static property for PI

    static calculateCircumference(radius: number): number { // Static method
      return 2 * MathUtils.PI * radius;
    }
  }

  // Accessing static members directly from the class
  console.log(MathUtils.PI); // Outputs: 3.14159
  console.log(MathUtils.calculateCircumference(5)); // Outputs: 31.4159
```

### Do We Really Need Static Classes?

In some programming languages, you may come across the concept of a “static class” — a class that only contains static members and cannot be instantiated. In TypeScript, though, we don’t need such a special syntax. You can achieve the same functionality using simple functions or objects.

__Unnecessary Use of Static Class__

```ts
  class MyStaticClass {
    static doSomething() {
      console.log("Doing something...");
    }
  }

  MyStaticClass.doSomething();
```

While the above works, it's unnecessarily complex. Instead, you can refactor it using either a simple `function` or an `object`, which is cleaner and easier to manage.

Preferred Alternatives:

```ts
  // Using a function
  function doSomething() {
    console.log("Doing something...");
  }

  doSomething();

  // Using an object
  const MyHelperObject = {
    doSomething() {
      console.log("Doing something...");
    },
  };

  MyHelperObject.doSomething();
```

Here, we're using a plain object, which is often simpler than creating an entire class just for static members.

## Generic Classes

__A generic class__ in TypeScript is like a blueprint that works with any type of data. Instead of fixing a class to a specific type, we use a placeholder for the type. When we create an instance of the class, we specify what type it should work with — whether it's a `number`, `string`, or even a more complex type.

Let’s start with the basic syntax of a generic class:

```ts
  class GenericClass<T> {
    private value: T;

    constructor(value: T) {
      this.value = value;
    }

    getValue(): T {
      return this.value;
    }
  }
```

Here, `<T>` is a type parameter. Instead of hardcoding the type of `value`, we use `T`, which acts as a placeholder. When we create an instance of `GenericClass`, we can specify what type `T` should be.

```ts
  const numberBox = new GenericClass<number>(42); // T is a number
  console.log(numberBox.getValue()); // Output: 42

  const stringBox = new GenericClass<string>("Hello, TypeScript!"); // T is a string
  console.log(stringBox.getValue()); // Output: Hello, TypeScript!
```

Let's expand on this idea and create a more feature-rich generic class. Here’s a class that allows us to store, retrieve, and update values of any type.

```ts
  class GenericClass<T> {
    private content: T;

    constructor(value: T) {
      this.content = value;
    }

    getContent(): T {
      return this.content;
    }

    setContent(value: T): void {
      this.content = value;
    }
  }

  const numberBox = new GenericClass<number>(42); 
  console.log(numberBox.getContent()); // Output: 42
  numberBox.setContent(100); 
  console.log(numberBox.getContent()); // Output: 100

  const stringBox = new GenericClass<string>("Hello, TypeScript!");
  console.log(stringBox.getContent()); // Output: Hello, TypeScript!
  stringBox.setContent("Generics are awesome!"); 
  console.log(stringBox.getContent()); // Output: Generics are awesome!
```

### Building a Generic Stack

Stacks are a widely-used data structure where elements are added and removed in a __Last-In-First-Out (LIFO)__ manner. Think of it like stacking plates: the last plate you put on the stack is the first one you take off. Just like arrays, you can push items onto the stack, but the key difference is that you always interact with the most recently added item first. Let’s build a generic `Stack` class that can hold any type of data.

```ts
  class Stack<T> {
    private items: T[] = [];

    push(item: T): void {
      this.items.push(item); // Adds item to the stack
    }

    pop(): T | undefined {
      return this.items.pop(); // Removes and returns the last item
    }

    peek(): T | undefined {
      return this.items[this.items.length - 1]; // Looks at the last item without removing it
    }

    isEmpty(): boolean {
      return this.items.length === 0; // Checks if the stack is empty
    }
  }

  // Using a Number Stack
  const numberStack = new Stack<number>(); 
  numberStack.push(1);
  numberStack.push(2);
  numberStack.push(3);

  console.log(numberStack.pop()); // Output: 3 (last-in, first-out)
  console.log(numberStack.peek()); // Output: 2
  console.log(numberStack.isEmpty()); // Output: false

  // Using a String Stack
  const stringStack = new Stack<string>();
  stringStack.push("TypeScript");
  stringStack.push("is");
  stringStack.push("awesome");

  console.log(stringStack.pop()); // Output: awesome
  console.log(stringStack.peek()); // Output: is
  console.log(stringStack.isEmpty()); // Output: false
```

This `Stack` class can handle any type, whether it's numbers, strings, or more complex data structures.

## Abstract Classes

In TypeScript, abstract classes are like blueprints for other classes. They allow you to define methods and properties that must be implemented by subclasses. However, you can’t create an object directly from an abstract class. It’s meant to be extended by other classes that provide specific implementations for its abstract members. Think of it as a template that defines a general structure but leaves some details for the subclasses to fill in. If you try to create an instance of an abstract class, TypeScript will give you an error.

```ts
  abstract class Shape {
    abstract calculateArea(): number; // Abstract method (must be implemented by subclasses)

    displayArea() { // Regular method (can be used directly or overridden by subclasses)
      console.log(`The area is ${this.calculateArea()}`);
    }
  }

  const myShape = new Shape(); // ❌ Error: Cannot create an instance of an abstract class.
```

### Extending an Abstract Class

To use an abstract class, you need to extend it and provide implementations for the abstract methods in the subclass.

```ts
  class Circle extends Shape {
    constructor(private radius: number) {
      super(); // Calls the parent class constructor
    }

    calculateArea(): number { // Implements the abstract method
      return Math.PI * this.radius ** 2;
    }
  }

  const circle = new Circle(5); 
  circle.displayArea(); // Outputs: The area is 78.53981633974483
```

### Key Points About Abstract Classes

- You can't create an instance of an abstract class. It's designed to be extended.

```ts
  const myShape = new Shape(); // ❌ Error: Cannot create an instance of an abstract class.
```

- To use the abstract class, you need to create a subclass that extends it and provides implementations for all its abstract methods.

- Abstract methods are methods that are declared but not implemented in the abstract class. Subclasses must provide their own implementation for these methods.

- Abstract classes can also have regular methods (with full implementations) that can either be used directly by the subclass or overridden if needed.

### Real-World Use Case

Imagine you’re building a system where different shapes (like `Circle`, `Rectangle`, etc.) need to calculate their area. Instead of defining `calculateArea()` in each shape’s class separately, you can define it as an abstract method in a base `Shape` class. Each specific shape (e.g., `Circle`, `Rectangle`) will provide its own implementation of how to calculate the area.

This allows you to enforce a consistent interface across all shapes, ensuring every shape has a `calculateArea()` method.

__Example: Extending the `Shape` Class with Another Shape__

```ts
  class Rectangle extends Shape {
    constructor(private width: number, private height: number) {
      super();
    }

    calculateArea(): number {
      return this.width * this.height;
    }
  }

  const rectangle = new Rectangle(10, 20);
  rectangle.displayArea(); // Outputs: The area is 200
```

Now, you have both `Circle` and `Rectangle` classes that extend the abstract `Shape` class. Each provides its own way of calculating the area, but they share a common structure and interface.

🌟 Awesome job! You’ve successfully completed your Day 14, and you're well on your way to becoming a great developer. Keep up the momentum! Now, let's keep your mind sharp and your body active with some quick exercises.

## 💻 Day 14: Exercises

### Exercise: Level 1

1. Define a `Person` class with two properties: `firstName` and `lastName`. Both properties should be public, and write a method `getFullName()` that returns the full name of the person.

2. Create a `Converter` class with a static method `toUpperCase(text: string)` that takes a string and returns it in uppercase. You should be able to call this method without creating an instance of the class.

3. You have a `Car` class where the `make`, `model`, and `year` properties should not be modified once the object is created. How would you structure the class using access modifiers to ensure this behavior?

4. Create a `Counter` class that tracks the number of instances created. Use static members to implement this functionality. Every time a new object of the class is created, the counter should increment.

5. Create a generic class `KeyValuePair<K, V>` that holds a key-value pair. Ensure that the key is always a string or a number, but the value can be of any type. Add methods to get and set the key-value pair.


### Exercise: Level 2

6. Create an abstract class `Vehicle` with an abstract method `move()`. Create two subclasses `Bike` and `Truck`, each implementing the `move()` method differently (`"The bike pedals"` and `"The truck drives"`, respectively). Also, add a method `startEngine()` in `Truck`. What happens when you call `move()` on both subclasses?

7. Create a class `Counter` with a method `startCountdown(seconds: number) `that counts down from the given number and logs each second using `setTimeout`. Use an arrow function inside `setTimeout` to ensure that the `this` context points to the class instance. What would happen if you didn’t use an arrow function?

8. Create a generic class `DataStore<T>` with a default type of `string`. Allow users to store and retrieve data of any type, but if no type is specified, the class should default to using `string`.


### Exercise: Level 3

9. You are tasked with creating a system that supports multiple payment methods (like `CreditCard`, `PayPal`, and `BankTransfer`).

- Create an abstract class `PaymentMethod` with an abstract method `processPayment(amount: number): void`.
- Create subclasses for `CreditCard`, `PayPal`, and `BankTransfer` that implement this method differently for each payment type.

10. Create an abstract generic class `Repository<T>` that has methods `getAll(): T[]` and `add(item: T): void`. Then create a subclass `UserRepository` that extends `Repository<User>`. The `User` class should have properties `id` and `name`. Implement methods to store and retrieve `User` objects in `UserRepository`.

11. Define a class `Cache<K, V>` with static methods `set(key: K, value: V)` and `get(key: K): V`. The class should use a private static map to store key-value pairs. Only the class itself can access the storage map, but external code should be able to set and retrieve values through the static methods.

12. Create an abstract class `Factory<T>` with a static method `createInstance(): T`. Extend this class with a `CarFactory` that creates instances of the `Car` class and a `BikeFactory` that creates instances of the `Bike` class. Implement the `createInstance` method in each subclass.

🎉 CONGRATULATIONS ! 🎉

[<< Day 13](../Day13_Classes_1/Day13.md) | [Day 15 >>](../Day15_Modules/Day15.md)
