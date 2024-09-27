<div align="center"> 
  <h1>20 Days of Basics TypeScript: Objects</h1>
</div>

<div align="center"> 

<!-- Social links -->
[![Discord](https://img.shields.io/badge/Discord-%237289DA.svg?logo=discord&logoColor=white)](htttps://discord.gg/Samson#0273) [![Facebook](https://img.shields.io/badge/Facebook-%231877F2.svg?logo=Facebook&logoColor=white)](https://www.facebook.com/chiemezie.nebeolisa/) [![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?logo=Instagram&logoColor=white)](https://www.instagram.com/samson_nebeolisa/) [![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/chiemezie-samson-nebeolisa-32897310b/) [![Stack Overflow](https://img.shields.io/badge/-Stackoverflow-FE7A16?logo=stack-overflow&logoColor=white)](https://stackoverflow.com/users/20653301/nebeolisa-chiemezie-samson) [![Twitter](https://img.shields.io/badge/Twitter-%231DA1F2.svg?logo=Twitter&logoColor=white)](https://twitter.com/SamsonChiemezie) [![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?logo=YouTube&logoColor=white)](https://myaccount.google.com/u/0/?utm_source=YouTubeWeb&tab=rk&utm_medium=act&tab=rk&hl=en) 

<!-- Portfolio -->
 📰 About Me [Portfolio](https://www.nebe-samson.com/)
 <br/>
  <small>Sep, 2024</small>

  <p>
    <small>Support the <strong>author</strong> to create more educational materials</small>
     <br/>
     
  [![PayPal](https://img.shields.io/badge/PayPal-00457C?logo=paypal&logoColor=white)](https://paypal.me/ChiemezieSamson?country.x=KR&locale.x=en_US)
  </p>
</div>

[<< Day 4](../Day4_Tuple/Day4.md) | [Day 6 >>](../Day6_Functions_1/Day6.md)

<div align="center"> 
  <a class="header-image" target="_blank" href="../Asset/images/Days/Day_5.webp">
    <img alt="Typescript image" src="../Asset/images/Days/Day_5.webp" width="100%" height="600px">
  </a>
</div>

## Table of Contents

- [📔 Day 5](#-day-5)
- [Object Types in TypeScript](#object-types-in-typescript)
  - [Optional Properties](#optional-properties)
  - [Readonly Properties](#readonly-properties)
  - [Readonly with Nested Objects](#readonly-with-nested-objects)
- [Index Signatures](#index-signatures)
  - [Using Index Signatures](#using-index-signatures)
  - [Combining Index Signatures with Regular Properties](#combining-index-signatures-with-regular-properties)
  - [Readonly Index Signatures](#readonly-index-signatures)
- [Extending Types in TypeScript](#extending-types-in-typeScript)
  - [Extending Interfaces with the `extends` Keyword](#extending-interfaces-with-the-extends-keyword)
  - [Extending Multiple Interfaces](#extending-multiple-interfaces)
  - [Extending Type Aliases with Intersection Types](#extending-type-aliases-with-intersection-types)
- [Generic Object Types in TypeScript](#generic-object-types-in-typeScript)
  - [Using Generics in Functions](#using-generics-in-functions)
  - [Using Generics with Interfaces](#using-generics-with-interfaces)
  - [Generics in Type Aliases](#generics-in-type-aliases)
  - [Generic Interfaces with Functions](#generic-interfaces-with-functions)
  - [Why Use Generics?](#why-use-generics?)
- [💻 Day 5: Exercises](#-day-5-exercises)
  - [Exercise: Level 1](#exercise-level-1)
  - [Exercise: Level 2](#exercise-level-2)
  - [Exercise: Level 3](#exercise-level-3)

# 📔 Day 5

## Object Types in TypeScript

An object in TypeScript, just like in JavaScript, is a collection of key-value pairs, where each key (or property) is associated with a value. TypeScript enhances objects by allowing us to define the types of the properties in an object, adding type safety to our code.

In TypeScript, you can define the shape of an object by specifying the types of its properties. This can be done using an inline type annotation or by creating an interface or type alias. objects can also be anonymous.

let's see ways to define an object:

```ts 
  // type annotation
  let user: { name: string; age: number; isAdmin: boolean };

  user = {
    name: "Samson",
    age: 25,
    isAdmin: true,
  };

  console.log(user); // Output: { name: "Samson", age: 25, isAdmin: true }
  // TypeScript infers types based on the object values
  user.name = "James" // works
  user.name = 23 // ❌ Error: Type 'number' is not assignable to type 'string'

  // type aliases
  type Address = {
    street: string;
    city: string;
    country: string;
  };

  let homeAddress: Address = {
    street: "123 Main St",
    city: "New York",
    country: "USA",
  };

  console.log(homeAddress);

  // interfaces 
  interface User {
    name: string;
    age: number;
    isAdmin: boolean;
  }

  let admin: User = {
    name: "John",
    age: 30,
    isAdmin: true,
  };

  console.log(admin);

  // anonymous
  function printName(obj: { first: string; last?: string }) {
    console.log(obj.first + " " + (obj.last || ""));
  }

  printName({ first: "Bob" }); // First name is required, last is optional
  printName({ first: "Alice", last: "Alisson" }); // Both first and last names
```

As a developer, you'll be working with objects constantly in JavaScript, and TypeScript gives us the ability to define the types of these objects more strictly, which helps prevent errors and makes our code easier to understand and maintain.

### Optional Properties

Sometimes, not all properties in an object are required. You can mark properties as optional by using the `?` symbol just like we did with the `printName` anonymous object.

```ts 
  // example 1
  let product: { name: string; price?: number };

  product = { name: "Laptop" }; // price is optional
  console.log(product); // Output: { name: "Laptop" }

  product = { name: "Phone", price: 999 };
  console.log(product); // Output: { name: "Phone", price: 999 }

  // example 2
  const car: { type: string; mileage?: number } = {
    type: "Toyota", // required property
  };

  car.mileage = 2000; // Optional property
```

>[!NOTE]
> When a property is optional, callers can pass `undefined`, which behaves just like omitting the property altogether. It simulates a 'missing' argument without causing any errors.:

```ts
  interface PaintOptions {
    shape: string;
    xPos?: number; // (property) PaintOptions.xPos?: number | undefined
    yPos?: number; // (property) PaintOptions.yPos?: number | undefined
  }

  function paintShape(opts: PaintOptions) {
    // ...
  }

  paintShape({ shape: "shape" }); // required
  paintShape({ shape: "shape", xPos: 100 }); // Optional
  paintShape({ shape: "shape", yPos: 100 }); // Optional
  paintShape({ shape: "shape", xPos: 100, yPos: 100 }); // Optional
```

### Readonly Properties

In some cases, you may want to ensure that certain properties of an object cannot be modified after the object is created. For this, TypeScript provides the `readonly` modifier.

```ts
  // example 1
  let car: { readonly brand: string; model: string };

  car = { brand: "Toyota", model: "Corolla" };

  // Trying to change the readonly property will cause an error
  car.brand = "Honda"; // ❌ Error: Cannot assign to 'brand' because it is a read-only property.

  car.model = "Camry"; // This is allowed
  console.log(car); // Output: { brand: 'Toyota', model: 'Camry' }

  // example 2
  interface SomeType {
    readonly prop: string;
  }

  function doSomething(obj: SomeType) {
    // We can read from 'obj.prop'.
    console.log(`prop has the value '${obj.prop}'`);

    // But we can't re-assign it.
    obj.prop = "newValue"; // ❌ Error: Cannot assign to 'prop' because it is a read-only property.
  }
```

### Readonly with Nested Objects

You can make object properties readonly, even if those properties are objects themselves. While you can't reassign the readonly property entirely, you can still modify the internal contents of the object it holds.

```ts
  interface Home {
    readonly resident: { name: string; age: number };
  }

  function visitForBirthday(home: Home) {
    // We can read and update properties from 'home.resident'.
    console.log(`Happy birthday ${home.resident.name}!`);
    home.resident.age++; // This is allowed

    // But we can't write to the 'resident' property itself on a 'Home'.
    home.resident = { name: "John", age: 50 }; // ❌ Error: Cannot assign to 'resident' because it is a read-only property.
  }
```

TypeScript doesn't consider whether properties are `readonly` when checking type compatibility between two objects. This means `readonly` properties can still be modified indirectly through an alias:

```ts 
  interface Person {
    name: string;
    age: number;
  }

  interface ReadonlyPerson {
    readonly name: string;
    readonly age: number;
  }

  let person: Person = { name: "Alice", age: 30 };

  // works
  let readonlyPerson: ReadonlyPerson = person;

  console.log(readonlyPerson.age); // Prints: 30
  person.age++;
  console.log(readonlyPerson.age); // Prints: 31
```

## Index Signatures

As a developer, you're likely familiar with JavaScript objects and how their properties can be accessed using dot notation or brackets. But what if you're working with objects where you don't know all the property names in advance, but you do know the type of the keys and values? That’s where index signatures in TypeScript come into play.

An index signature in TypeScript allows you to define the shape of an object where the property names (or keys) aren’t predetermined, but their types are. Essentially, you're telling TypeScript: “This object can have any number of properties, and here's what the keys and values will look like.”

Here's the syntax for an index signature:

```ts
  {
    [key: KeyType]: ValueType;
  }
```

  - `key` is just a placeholder name (it can be anything you choose, like `index` or `prop`).
  - `KeyType` usually refers to `string` or `number`, specifying the type for keys.
  - `ValueType` defines the type for the values those keys will hold.

### Using Index Signatures

Let’s say we want to create an object that maps people's names to their ages, but we don’t know all the names ahead of time. Using an index signature, we can easily define this object:

```ts
  interface AgeMap {
    [name: string]: number;
  }

  const timeFrame: AgeMap = {
    "Alice": 25,
    "Bob": 30
  };

  console.log(timeFrame["Alice"]); // Output: 25
```

In this example, we use `[name: string]` to indicate that the keys are strings (the names), and `number` to indicate that the values (ages) are numbers. 

With index signatures, you can dynamically reference properties that weren't defined in the initial object structure and assign them values of the specified type. This flexibility allows you to work with objects where the keys can vary, while still ensuring that the values adhere to a consistent type.

```ts
  const Person: { [index: string]: number } = {};

  Person.Jack = 25; // Works fine
  Person.Mark = "Fifty"; // ❌ Error: Type 'string' is not assignable to type 'number'.
```

### Combining Index Signatures with Regular Properties

You can combine index signatures with specific properties that you know in advance, while still allowing dynamic properties to be added. Here’s an example where we combine fixed properties like name and age with a flexible index signature that accepts any additional string key mapped to a string or number:

```ts
  interface Person {
    name: string;
    age: number;
    [key: string]: string | number; // This allows additional string or number properties
  }

  const person: Person = {
    name: "John",
    age: 35,
    occupation: "Developer",
    country: "USA"
  };

  console.log(person.name); // Output: "John"
  console.log(person["occupation"]); // Output: "Developer"
```

This makes your object both flexible (for dynamic properties) and structured (for known properties).

### Readonly Index Signatures

You can also make your index signatures readonly, which means that while you can still access properties dynamically, you can’t change their values once they’ve been set. Let’s see an example:

```ts
  interface Person {
    name: string;
    age: number;
    [key: string]: string | number; // Flexible string or number properties
    readonly [index: number]: string; // Readonly index signature for numeric keys
  }

  const person: Person = {
    name: "John",
    age: 35,
    occupation: "Developer",
    country: "USA"
  };

  console.log(person[0]); // Output: undefined (since there's no numeric property)
```

In the above example, numeric keys like `person[0]` are readonly, meaning they can’t be modified:

```ts
  console.log(person[3] = "Front end"); // ❌ Error: Index signature in type 'Person' only permits reading.
```

## Extending Types in TypeScript

When you're building a project, there will be times when you need to create multiple types or interfaces that share some common properties but also have their own unique characteristics. Instead of repeating the same properties in different types, you can extend a base type and build on it. This helps keep your code cleaner and easier to maintain. In TypeScript, you can extend types in two ways: by using __interfaces__ or __type aliases__.

### Extending Interfaces with the `extends` Keyword

When two interfaces share common properties, you can extend one interface from another using the extends keyword.

__Example: Extending Interfaces__

Imagine we have a `Person` interface that represents a basic person with a name and age. We can create an `Employee` interface that extends `Person` and adds more properties specific to employees.

```ts
  interface Person {
    name: string;
    age: number;
  } 

  // Employee extends Person and adds employeeId and position
  interface Employee extends Person {
    employeeId: number;
    position: string;
  }

  const employee: Employee = {
    name: "Samson",
    age: 28,
    employeeId: 12345,
    position: "Software Developer"
  };

  console.log(employee.name); // Output: "Samson"
  console.log(employee.employeeId); // Output: 12345
```

### Extending Multiple Interfaces

You’re not limited to extending just one interface. You can extend multiple interfaces at the same time by chaining them together. This is helpful when you have different sets of shared properties.

__Example:__

Let’s say you want an `Employee` that not only inherits from `Person`, but also has an `address` property. We can create a new interface `HasAddress` and extend both `Person` and `HasAddress` in our `Employee` interface.

```ts
  interface HasAddress {
    address: string;
  }

  // Employee2 extends both Person and HasAddress
  interface Employee2 extends Person, HasAddress {
    employeeId: number;
    position: string;
  }

  const employee2: Employee2 = {
    name: "Charlie",
    age: 30,
    address: "123 Main St",
    employeeId: 11111,
    position: "HR Manager"
  };

  console.log(employee2.address); // Output: "123 Main St"
```

### Extending Type Aliases with Intersection Types

Type aliases are another way of defining object shapes, just like interfaces. But to extend a type alias, we use __intersection types__, which combine multiple types into one using the `&` symbol.

Here’s how we extend a Person type alias to create an Employee type alias:

```ts
  type PersonAlias = {
    name: string;
    age: number;
  };

  // EmployeeAlias combines PersonAlias with additional properties
  type EmployeeAlias = PersonAlias & {
    employeeId: number;
    position: string;
  };

  const employeeAlias: EmployeeAlias = {
    name: "Bob",
    age: 35,
    employeeId: 67890,
    position: "Project Manager"
  };

  console.log(employeeAlias.name); // Output: "Bob"
  console.log(employeeAlias.position); // Output: "Project Manager"
```

Just like you can extend multiple interfaces, you can also combine multiple type aliases using intersection types. This is a flexible way to build new types from existing ones.

If you want to combine `PersonAlias` and `HasAddress` into a new `Employee` type using intersection types, it looks like this:

```ts
  type EmployeeIntersection = PersonAlias & HasAddress;

  const employeeIntersection: EmployeeIntersection = {
    name: "Samson",
    age: 24,
    address: "456 Park Avenue"
  };

  console.log(employeeIntersection.name); // Output: "Samson"
  console.log(employeeIntersection.address); // Output: "456 Park Avenue"
```

## Generic Object Types in TypeScript

In TypeScript, generics are a powerful tool that lets you create reusable, flexible, and type-safe code. Instead of creating multiple versions of a function or object for each data type, you can write one version that works with any type—while still making sure that TypeScript knows exactly which type it’s dealing with at any given time.

Think of generics as placeholders for types. You can create a "template" that adapts to any type you give it. When you use generics, you write flexible code that can handle different types without losing the benefits of TypeScript’s type checking.

In TypeScript, generics are defined using angle brackets (`<T>`, where `T` is just a placeholder for a type) and can be used in interfaces, functions, or type aliases. Let's break this down step by step with simple examples.

### Using Generics in Functions

When writing a function that works with multiple types, you can define a generic type by using a type parameter in angle brackets (`<T>`). This lets you create a function that can take any type as an argument but still return the correct type.

Here’s a function called `identity` that takes an argument of any type and returns it. The generic type `Type` adapts based on whatever you pass in:

```ts
  function identity<Type>(arg: Type): Type {
    return arg;
  }

  // Using the generic function
  let stringResult = identity("Hello!"); // TypeScript knows this is a string
  let numberResult = identity(123);      // TypeScript knows this is a number

  // or

  // same as the above
  let stringResult = identity1<string>("Hello!"); 
  let numberResult = identity1<number>(123);   

  console.log(stringResult); // Output: "Hello!"
  console.log(numberResult); // Output: 123
```

### Using Generics with Interfaces

You can also use generics when defining interfaces. This is helpful when you want an interface that can handle different types without needing to duplicate the interface for every single type.

```ts
  interface Box<Type> {
    content: Type;
  }

  const stringBox: Box<string> = { content: "Hello, world!" };
  const numberBox: Box<number> = { content: 123 };

  console.log(stringBox.content); // Output: "Hello, world!"
  console.log(numberBox.content); // Output: 123
```

### Generics in Type Aliases

Just like with interfaces, you can create type aliases that use generics. 

```ts
  type Box2<Type> = { 
    contents: Type;
  };

  const stringBox2: Box2<string> = { contents: "Hello, TypeScript!" };
  const numberBox2: Box2<number> = { contents: 456 };

  console.log(stringBox2.contents); // Output: "Hello, TypeScript!"
  console.log(numberBox2.contents); // Output: 456
```

### Generic Interfaces with Functions

You can also define a generic interface for a function that can work with different types. This is useful when you want to define a function signature that can take and return any type.

Here’s how you can define a generic function interface called `GenericIdentityFn`:

```ts
  interface GenericIdentityFn {
    <Type>(arg: Type): Type;
  }

  function identity1<Type>(arg: Type): Type {
    return arg;
  }

  let myIdentity: GenericIdentityFn = identity1;

  console.log(myIdentity("Test")); // Output: "Test"
  console.log(myIdentity(100));    // Output: 100
```

## Why Use Generics?

Generics make your code:

- __Reusable__: You don’t need to write separate versions of functions or interfaces for every type. One generic version works for all.

- __Type-safe__: TypeScript still checks types at compile time, meaning you get the benefits of static type checking even with flexible code.

- __Flexible__: You can work with any data type without losing type information.

🌟 Awesome job! You’ve successfully completed your Day 5, and you're well on your way to becoming a great developer. Keep up the momentum! Now, let's keep your mind sharp and your body active with some quick exercises.

## 💻 Day 5: Exercises

### Exercise: Level 1

1. Define an object type `Person` with two properties: `name` and `age`.

2. Using the `Person` type defined above, create an object called `person1` with your own details.

3. Extend the Person type to include an optional email property. Then create an object that uses this extended type.

4. Modify the Person type so that the name property is read-only.
### Exercise: Level 2

5. Define an object type `Address` with properties `street`, `city` , and `zipCode`. Now, modify the `Person` type so that it has an `address` property, which is of type `Address`.

6. Modify the `Person` type to include a property `hobbies` that is an array of strings. Then create an object `person2` using this updated type.

7. Define a new type `Employee` that intersects with `Person` and has an additional `jobTitle` property. Then create an object `employee1` using this type.

### Exercise: Level 3

8. Define a `Dictionary` object type that can have any number of properties where the keys are strings, and the values are numbers.

9. Explain the difference between `type` and `interface` in TypeScript. Provide an example of both, and show how to extend them.

10. Using a mapped type, define a type `ReadonlyPerson` where all properties of `Person` are readonly.

11. Create a conditional type `IsString<T>` that returns `true` if `T` is a `string`, otherwise returns `false`.



🎉 CONGRATULATIONS ! 🎉

[<< Day 4](../Day4_Tuple/Day4.md) | [Day 6 >>](../Day6_Functions_1/Day6.md)