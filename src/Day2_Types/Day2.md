<div align="center"> 
  <h1>30 Days of Basics TypeScript: Introduction </h1>
</div>

<div align="center"> 

<!-- Social links -->
[![Discord](https://img.shields.io/badge/Discord-%237289DA.svg?logo=discord&logoColor=white)](htttps://discord.gg/Samson#0273) [![Facebook](https://img.shields.io/badge/Facebook-%231877F2.svg?logo=Facebook&logoColor=white)](https://www.facebook.com/chiemezie.nebeolisa/) [![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?logo=Instagram&logoColor=white)](https://www.instagram.com/samson_nebeolisa/) [![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/chiemezie-samson-nebeolisa-32897310b/) [![Stack Overflow](https://img.shields.io/badge/-Stackoverflow-FE7A16?logo=stack-overflow&logoColor=white)](https://stackoverflow.com/users/20653301/nebeolisa-chiemezie-samson) [![Twitter](https://img.shields.io/badge/Twitter-%231DA1F2.svg?logo=Twitter&logoColor=white)](https://twitter.com/SamsonChiemezie) [![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?logo=YouTube&logoColor=white)](https://myaccount.google.com/u/0/?utm_source=YouTubeWeb&tab=rk&utm_medium=act&tab=rk&hl=en) 

<!-- Portfolio -->
 📰 About Me [Portfolio](https://www.nebe-samson.com/)
 <br/>
  <small>Sep, 2024</small>
</div>

[<< Day 1](../Day2_Types/Day2.md) | [Day 3 >>](../Day3_Array_Type/Day3.md)

<div align="center"> 
  <a class="header-image" target="_blank" href="../Asset/images/Days/day_2.webp">
    <img alt="Typescript image" src="../Asset/images/Days/day_2.webp" width="100%" height="600px">
  </a>
</div>

## Table of Contents

- [📔 Day 2](#-day-2)
- [Types in TypeScript](#types-in-typescript)
  - [Type Guards Using ```typeof```](#type-guards-using-typeof)
    - [Basics Exampless](#basics-examples)
- [Type Annotations](#type-annotations)
- [Type Assignment: Explicit and Implicit Type](#type-assignment:-explicit-and-implicit-type)
    - [Implicit Type Assignment](#implicit-type-assignment)
    - [Explicit Type Assignment](#Explicit-type-assignment)
    - [Type Mismatch](#type-mismatch)
    - [Handling JSON](#handling-json)
- [Special Types](#special-types)
- [Union Types](#union-types)
  - [Union Types in Action](#union-types-in-action)
  - [Handling Union Type Errors](#handling-union-type-errors)
  - [Narrowing Union Types](#narrowing-union-types)
- [Type Aliases](#type-aliases)
- [Interfaces](#interfaces)
  - [Extending Interfaces](#extending-interfaces)
  - [Differences Between Type Aliases and Interfaces](#differences-between-type-aliases-and-interfaces)
- [Type Assertions](#type-assertions)
- [Literal Types](#literal-types)
  - [Literal Inference](#literal-inference)
  - [💻 Day 2: Exercises](#-day-2-exercises)

# 📔 Day 2

## Types in TypeScript

### Type Guards Using ```typeof```

As a front-end developer, one of the most important things TypeScript helps us with is type checking. TypeScript ensures that the variables and values we use in our code match their expected types. One way to perform type checking in TypeScript is using the ```typeof``` operator.

The ```typeof``` operator helps you determine what kind of data you’re dealing with at runtime (while your code is running). You can use it to safeguard your code by ensuring that variables have the right type before you use them.

Here’s a list of basic types and how you check them using typeof:

|  Type      |  Check using typeof               |
|:----------:|:---------------------------------:|
|  string    |  typeof s === "string"            |
|  number    |  typeof n === "number"            |
|  boolean   |  typeof b === "boolean"           |
|  undefined |  typeof undefined === "undefined" |
|  function  |  typeof f === "function"          |
|  array     |  Array.isArray(a) (special case)  |
|  object    |  typeof ob === "object"           |
|  bigint    |  typeof b === "bigint"            |
|  symbol    |  typeof sy === "symbol"           |


**how to use typeof with various types:**

```typescript 
  // Check if a variable is a string
  if (typeof value === "string") { /* ... */ }

  // Check if a variable is a number
  if (typeof value === "number") { /* ... */ }

  // Check if a variable is a boolean
  if (typeof value === "boolean") { /* ... */ }

  // Check if a variable is a function
  if (typeof value === "function") { /* ... */ }

  // Check if a variable is an object (but could be an array)
  if (typeof value === "object" && value !== null) { /* ... */ }

  // Specifically check for an array
  if (Array.isArray(value)) { /* ... */ }

  // Check if a variable is undefined
  if (typeof value === "undefined") { /* ... */ }
```

#### Basics Examples

**Checking a String**

Let’s say you have a variable and you want to check if it's a string before doing anything with it.

```typescript
  function printMessage(message: any) {
      if (typeof message === "string") {
        // If the value is a string, print the message in uppercase.
          console.log("Message is: " + message.toUpperCase());
      } else {
        // If the value is not a string, alert the user.
          console.log("Provided value is not a string.");
      }
  }

  printMessage("Hello!");  // Output: Message is: HELLO!
  printMessage(123);       // Output: Provided value is not a string.
```

**Checking for undefined**
In TypeScript, you often need to check if a variable is defined before using it. You can do this easily with ```typeof```.

```typescript
  let user: { name?: string } = {};

  if (typeof user.name === "undefined") {
      console.log("Name is not defined.");
  } else {
      console.log("User's name is " + user.name);
  }
```
Here, the typeof check prevents any errors if the name property is missing.


## Type Annotations

When you declare a variable, you can optionally tell TypeScript what the type of that variable should be. This is called type annotation. It helps TypeScript catch errors before they happen.

As mentioned above TypeScript has several basic types that let us define what kind of data we’re working with. Now let's use then in type annotation.

  ```typescript 

  - Type: For ```boolean``` - ```true``` or ```false``` values
    let isActive: boolean = true;

  - Type: ```number``` - For integers and floating point values
    let score: number = 100;
	
  - Type: ```string``` - For text values like "Hi!, how are you?"
    let greeting: string = "Hello, world!";
	
  - Type: ```bigint``` - For really large integers. 
    let largeNumber: bigint = BigInt(2 ** 53);
	
  - Type: ```symbol``` - For creating unique, globally available values.
    const uniqueID: symbol = Symbol("id");
  ```
	
## Type Assignment: Explicit and Implicit Type

One of the core concepts in TypeScript is how types are assigned to variables, which can be done explicitly or implicitly. Let's break these concepts down with examples.

### Implicit Type Assignment

Implicit type assignment happens when TypeScript infers the type of a variable based on the value it’s assigned. This means you don't need to explicitly declare the type; TypeScript will figure it out for you.

```typescript 
	let message = "Hello, world!"; // TypeScript infers the type as 'string'
  let count = 42; // TypeScript infers the type as 'number'
  let isActive = true; // TypeScript infers the type as 'boolean'
```

TypeScript can also infer and check types in functions and objects.

```typescript 
  // function
  function add(a: number, b: number) {
    return a + b; // TypeScript infers the return type as 'number'
  }

  let result = add(10, 20); // result is inferred as 'number'

  // object
  let user = {
    name: "Alice",
    age: 30
  }; // TypeScript infers the type as { name: string; age: number; }
```


### Explicit Type Assignment
	
Explicit type assignment involves specifying the type of a variable directly using TypeScript’s type annotations just like we did on ```Type Annotations```. This approach can make your code clearer and prevent unintended errors.

```typescript 
  let message: string = "Hello, world!";
  let count: number = 42;
  let isActive: boolean = true;

  // Type assignment on function
  function add(a: number, b: number): number {
    return a + b; // Explicitly declare the return type as 'number'
  }

  let result: number = add(10, 20); // result is explicitly declared as 'number'


  // Type assignment on object
  let user: { name: string; age: number } = {
    name: "Alice",
    age: 30
  };
```

### Type Mismatch 

TypeScript will throw an error if you try to assign a value that doesn’t match the type:

```typescript 
  
  message = "Hi" // No error

  message = 55 // ❌ Error: Type 'number' is not assignable to type 'string'.
```

### Handling JSON

TypeScript may not always infer the type correctly. A common example is when using ```JSON.parse()```, which sets type to ```any``` by default and disables type checking:

```typescript 
  const data = JSON.parse("55"); // TypeScript treats this as 'any'
```

To avoid potential errors, you can enable the ```noImplicitAny``` option in your ```tsconfig.json```.

## Special Types

1. Type: ```any``` - disables type checking and effectively allows all types to be used. The ```any``` type is useful when you don’t want to write out a long type just to convince TypeScript that a particular line of code is okay.

```typescript 
  let anything: any = true;
  anything = "Now it's a string";
  anything = 42;
  anything = [];
  anything = {};
  anything.function();
```

This should be avoided when possible because it makes your code less predictable. ```noImplicitAny``` is a TypeScript compiler option that helps ensure all variables have a specified type, preventing them from being implicitly assigned the ```any``` type.

2. Type: ```unknown``` - The ```unknown``` type is a safer alternative to ```any```. TypeScript won’t let you use it unless you first check its type.

```typescript 
  let someData: unknown = "Hello";
  someData = { greet: "hi" };

  // ❌ Error: 'someData' is of type 'unknown' unless you check the type first
  // someData.greet;

  if (typeof someData === "object" && someData !== null) {
    console.log((someData as { greet: string }).greet);
  }
```

3. Type: ```never``` - The ```never``` type is used when something should never happen. It's typically used in advanced cases, like when a function throws an error or never returns.

```typescript 
  // let notPossible: never = true; // ❌ Error: Type 'boolean' is not assignable to type 'never'.
```

```never``` is rarely used, especially by itself, its primary use is in advanced generics.

4. Type: ```undefined``` & ```null``` - are types that refer to the JavaScript primitives ```undefined``` and ```null``` respectively. heir behavior depends on whether you’ve enabled ```strictNullChecks``` in your project.

```typescript 
  let u: undefined = undefined;
  let n: null = null;
```

When ```strictNullChecks``` is enabled in the ```tsconfig.json file```, you need to handle ```null``` and ```undefined``` properly, such as with conditionals:

```typescript 
  function greet(name: string | null) {
    if (name === null) {
      console.log("No name provided");
    } else {
      console.log(`Hello, ${name}!`);
    }
  }
```

**Non-null Assertion Operator (Postfix ```!```)**

When you’re confident that a value isn’t ```null``` or ```undefined```, you can use the non-null assertion (```!```) after any expression. This tells TypeScript to treat the value as non-nullable.

```typescript 
  let value: number | null = 5;
  console.log(value!.toFixed()); // We're sure 'value' isn't null, so this is safe
```

Be cautious when using ```!```. Only use it when you’re absolutely sure the value can’t be ```null``` or ```undefined```.

## Union Types

A ```union``` type is a type formed from two or more other types. It tells TypeScript that a value can be any one of these specified types. Each type in the union is called a "member" of the union. To declare a union type, you use the ```|``` (pipe) symbol to separate the possible types. Here’s a simple example:

```typescript 
  type bool = true | false; 
```
This means ```bool``` can be either ```true``` or ```false```, which TypeScript will interpret as the ```boolean``` type.

Let’s look at some more practical examples:

```typescript 
  type DoorStates = "open" | "closed" | "half-closed";
  type BagStates = "locked" | "unlocked";
  type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
  type Mixed = [] | "" | {};  // It could be an empty array, string, or object.

  // You can even combine multiple union types to create more complex ones:
  type all = bool | DoorStates | PositiveOddNumbersUnderTen | Mixed;
```

This ```all``` type can now represent a boolean, a door state, a positive odd number under 10, or any of the types in ```Mixed```.


### Union Types in Action

Let’s say we have a function that accepts either a ```string``` or an array of ```strings```, and we want to get the length of the input. Here's how you can handle that:

```typescript 
  function getLength(input: string | string[]): number {
    return input.length;
  }
```

This works perfectly because both strings and arrays have a ```length``` property, so TypeScript knows this operation is safe.


### Handling Union Type Errors

TypeScript will only allow an operation if it is valid for every member of the union. So what happens if the members of the union don't share common properties? For example:

```typescript 
  function createId(id: number | string) {
    // return id.length; ❌ This would cause an error
    // Property 'length' does not exist on type 'string | number'.
    // Property 'length' does not exist on type 'number'.
  }
```

Why? Because numbers don’t have a ```length``` property. TypeScript catches this and throws an error, preventing potential runtime issues.

### Narrowing Union Types

The solution to this problem is __type narrowing__, where we check what type the value is at runtime, and then handle each case differently. One common way to do this is using a conditional statement, like ```typeof```:

```ts
  function createId(id: number | string) {
    if (typeof id === "string") {
      // Here, TypeScript knows 'id' is a string
      return id.length;
    } else {
      // Here, TypeScript knows 'id' is a number
      console.log(id);
    }
  }
```

Another solution to the problem is to use a function like ```Array.isArray```.

```ts 
  function welcomePeople(input: string[] | string) {
    if (Array.isArray(input)) {
      // Here, TypeScript knows 'input' is an array of strings
      console.log("Hello, " + input.join(" and "));
    } else {
      // Here, TypeScript knows 'input' is a single string
      console.log("Welcome lone traveler, " + input);
    }
  }
```

If all members of a union share a common property, you can safely use that property without narrowing. The ```getLength()``` function is good example.

## Type Aliases

As a frontend developer, you’ll often work with different kinds of data types—numbers, strings, objects, arrays, etc. What happens when you need to reuse a specific type pattern multiple times? This is where __Type Aliases__ come in handy.

A __type alias__ is like giving a nickname to a type (or combination of types) so you can reuse it easily throughout your code. Think of it as a shortcut! You create a new name for a type and use that name wherever you need it. You can use type aliases for both primitive types (like ```string```, ```number```) and more complex types (like objects or arrays).

```ts
  type Age = number | string;  // Age can be a number or a string
  type FirstName = string;
  type LastName = string;

  // Now Let’s use type aliases to describe the structure of object
  type Employee = {
    x: Age,
    y: FirstName,
    z: LastName
  };

  // Let’s use the Employee type alias to create an actual employee object:

  const age: Age = 24;  // Age can be a number or string
  const firstname: FirstName = "Samson";
  const lastname: LastName = "Nebeolisa";

  const employee: Employee = {
    x: age,      // x represents the age (number or string)
    y: firstname, // y is the first name
    z: lastname   // z is the last name
  };
```

Now, instead of repeating the structure of the Employee type throughout our code, we just refer to it by its alias. This makes the code much cleaner and more maintainable. 

Let’s take it a step further by creating a function that accepts an Employee type as a parameter. The function will log the employee’s details to the console.

```ts 
  const createEmployee = (person: Employee) => {
    console.log("The employee's age is " + person.x);
    console.log("The employee's first name is " + person.y);
    console.log("The employee's last name is " + person.z);
  };

  createEmployee({ x: 24, y: "Samson", z: "Nebeolisa" }); // function call

  // output:
  // The employee's age is 24
  // The employee's first name is Samson
  // The employee's last name is Nebeolisa
```

Let’s look at another example: 

```ts 
  type Department = "HR" | "Engineering" | "Marketing";
  type Role = "Manager" | "Developer" | "Designer";

  type EmployeeDetails = {
    name: string;
    age: number;
    department: Department;
    role: Role;
  };

  const addEmployee = (employee: EmployeeDetails) => {
    console.log(`${employee.name} is a ${employee.role} in ${employee.department}, aged ${employee.age}.`);
  };

  addEmployee({
    name: "Alice",
    age: 30,
    department: "Engineering",
    role: "Developer"
  });

  // output:
  // Alice is a Developer in Engineering, aged 30.
```

## Interfaces

Interfaces are similar to type aliases, except they only apply to object types. An interface is like a contract for an object’s shape. It tells TypeScript what properties an object must have and what types those properties should be.

For example, let’s say you’re building a room in a new house, and you need to define the size (height and width) of each room. You can use an interface to describe the room’s structure:

```ts
  interface RoomSize {
    height: number;
    width: number;
  }
```

This ```RoomSize``` interface guarantees that any object representing a room must have a ```height``` and a ```width```, and both must be numbers.

Now, let’s define a new room using the RoomSize interface:

```ts
  const myNewHome: RoomSize = {
    height: 20,
    width: 10
  };
```

Here, ```myNewHome``` must match the shape defined in the ```RoomSize``` interface, so it has two properties: ```height``` and ```width```.

Interfaces also work great in functions. Let’s create a function that takes a RoomSize object as an argument and prints out the dimensions:

```ts
  const tellMyRoomSize = (dimensions: RoomSize) => {
    console.log("The height value is " + dimensions.height);
    console.log("The width value is " + dimensions.width);
  };

  tellMyRoomSize({ height: 100, width: 100 });

  // output:
  // The height value is 100
  // The width value is 100
```

### Extending Interfaces

One of the coolest features of interfaces is that you can extend them. This means you can build on top of an existing interface by adding new properties. It’s like starting with a basic blueprint and adding extra features.

```ts
  interface Rectangle {
    height: number;
    width: number;
  }

  // extends height and width from Rectangle
  interface ColoredRectangle extends Rectangle {
    color: string;
  }
```

Now, ```ColoredRectangle``` has all the properties of ```Rectangle``` (height and width) plus a ```color```:

```ts
  const coloredRectangle: ColoredRectangle = {
    height: 20,
    width: 10,
    color: "red"
  };
```

### Differences Between Type Aliases and Interfaces

You might be wondering, "How is an interface different from a type alias?" They’re similar in many ways, but the key difference is in extendability:

  - __Interfaces can always be extended__: You can add new properties to an interface at any time, making it flexible for future changes.
  - __Type aliases can’t be re-opened__: Once you define a type alias, you can’t add new properties to it later.

## Type Assertions

As a frontend developer, you know that working with the DOM is like solving a puzzle, but the pieces aren’t always clear. Sometimes, TypeScript doesn’t know everything you do, especially when it comes to figuring out exactly what type of HTML element you’re dealing with. That's where __Type Assertions__ come in. Think of them as your secret tool for making TypeScript "see" what you already know!

A __type assertion__ is like saying, "Hey TypeScript, trust me, I know what this is!" You use it when you know more about a value than TypeScript does. For example, when grabbing an element from the DOM, TypeScript might think it's just a generic __HTMLElement__, but you know it's a specific type like an __HTMLCanvasElement__, and you need to tell TypeScript that.

example: 

```ts
  const canvas = document.getElementById('myCanvas');
```

At this point, TypeScript thinks ```canvas``` is just an ```HTMLElement```—but you know better! You need it to be an ```HTMLCanvasElement``` so you can use the 2D rendering context.

solution is to use __type assertion__ :

```ts
  const myCanvas = document.getElementById('myCanvas') as HTMLCanvasElement;
  const ctx = myCanvas.getContext('2d');
```

By adding ```as HTMLCanvasElement```, you're telling TypeScript, "Trust me, I know this is a canvas element," allowing you to safely access the canvas-specific methods like ```getContext('2d')```.

TypeScript gives you two ways to perform a type assertion:

```ts
  // Using the as syntax (most common):
  const myCanvas = document.getElementById('myCanvas') as HTMLCanvasElement;

  // Using angle brackets (less common but equivalent):
  const myCanvas = <HTMLCanvasElement>document.getElementById('myCanvas');
```

> [!IMPORTANT]
> If you're writing in a ```.tsx``` file (like when using React), avoid the angle-bracket syntax because it clashes with JSX.

> [!CAUTION]
> When you use a type assertion, TypeScript won’t check if you’re right or wrong at runtime. If you make a mistake, it won’t throw an error when the code runs.

```ts
  const x = "hello" as number; // This will break at runtime!

  // ❌ Error: Conversion of type 'string' to type 'number' may be a mistake because neither type sufficiently overlaps with the other. If this was intentional, convert the expression to 'unknown' first.
```

To avoid dangerous type assertions, TypeScript only allows type assertions between types that are closely related. For instance, you can assert a generic ```HTMLElement``` to a more specific type like ```HTMLCanvasElement```, but you can’t just assert a string as a number (like in the example above).

## Literal Types

In TypeScript, a __literal type__ means that a variable can only have a specific, predefined value. It’s like setting boundaries to ensure that your code only accepts certain options, making things more predictable and preventing bugs.

For example, you can declare a constant string like this:

```ts
  const constantString = "Hello World";
```

Here, ```constantString``` can only ever be ```"Hello World"```, nothing else. While this alone isn’t too useful, literal types become powerful when combined with unions to allow a variable to hold one of several predefined values.

**example with union:**

Imagine you’re building a text alignment feature in a UI. You want to make sure that text can only be aligned "left", "right", or "center". You don’t want any random value being used for alignment, so you use literal types to enforce the rule:

```ts
  const printText = (s: string, alignment: "left" | "right" | "center") => {
    // logic for printing text
  }

  printText("Hello, world", "left");  // ✅ Works
  printText("Hello, world", "center");  // ✅ Works

  // printText("Hello, world", "centre"); ❌ Error: Argument of type '"centre"' is not assignable to parameter of type '"left" | "right" | "center"'.
```

### Literal Inference

TypeScript can sometimes trip up when inferring literal types, especially with objects. Let’s say you want to send a request to a server, and you’re using literal types to define the valid methods ("GET" or "POST"). Here’s the setup:

```ts
  declare function handleRequest(url: string, method: "GET" | "POST"): void;

  const req = { url: "https://example.com", method: "GET" };
  handleRequest(req.url, req.method);  // ❌ Error: Argument of type 'string' is not assignable to parameter of type '"GET" | "POST"'.
```

Why does this happen? TypeScript sees ```req.method``` as just a ```string``` even though it’s clearly ```"GET"```. This is because TypeScript infers ```req.method``` as string rather than ```"GET"``` specifically.

There are a few ways to solve this inference problem. Here’s how:

```ts
  // 1. Using Type Assertions: Type assertions tell TypeScript to treat the value as a literal, like giving it a hint:
  handleRequest(req.url, req.method as "GET");  // ✅ Works!

  // 2. Using as const: This tells TypeScript to treat all properties of the object as literal types:
  const req = { url: "https://example.com", method: "GET" } as const;
  handleRequest(req.url, req.method);  // ✅ Works!
```

To avoid this error completly use literal inference only when any other methods are not usable.


🌟 Awesome job! You’ve successfully completed your Day 2, and you're well on your way to becoming a great developer. Keep up the momentum! Now, let's keep your mind sharp and your body active with some quick exercises.

## 💻 Day 2: Exercises

1. Create a TypeScript type called ```Status``` that can be one of the following string literals: ```"active"```, ```"inactive"```, or ```"pending"```.

2. Write a function getStatusMessage that takes a parameter of type Status and returns a message based on the status. For example:

- ```"active"``` should return ```"The status is active."```
- ```"inactive"``` should return ```"The status is inactive."```
- ```"pending"``` should return ```"The status is pending."```

3. Create a function ```processValue``` that takes a parameter of type ```number | string``` and performs the following:

- If the input is a string, return the length of the string.
- If the input is a number, return its square.

4. Create a function ```handleUnknown``` that takes a parameter of type ```unknown``` and performs the following:

- If the parameter is a string, return the string in uppercase.
- If the parameter is a number, return the number doubled.
- For other types, return ```"Unsupported type"```.

5. Create a new ```product``` type alias that represents a product with the following fields: ```name``` (string), ```price``` (number or string), and ```category``` (string). Then, create a function createProduct that takes a ```product``` as a parameter and prints out its details.

```ts 
  type product = {
    // Define fields here
  }

  function createProduct(item: product) {
    // Your code here
  }
```
6. Create an interface ```Car``` with properties ```make``` (string), ```model``` (string), and ```year``` (number). Then, create another interface ```ElectricCar``` that extends ```Car``` and adds a property ```batteryCapacity``` (number). Finally, create an object ```myCar``` of type ```ElectricCar``` and write a function ```printCarDetails``` that prints out the details of any car.

```ts 
  interface Car {
    // Define fields here
  }

  interface ElectricCar extends Car {
    // Define new field here
  }

  const myCar: ElectricCar = {
    // Define your car here
  };

  function printCarDetails(car: Car) {
    // Your code here
  }
```

🎉 CONGRATULATIONS ! 🎉

[<< Day 1](../Day2_Types/Day2.md) | [Day 3 >>](../Day3_Array_Type/Day3.md)