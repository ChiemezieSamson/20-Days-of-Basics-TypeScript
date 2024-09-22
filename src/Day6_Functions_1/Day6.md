<div align="center"> 
  <h1>30 Days of Basics TypeScript: Functions 1</h1>
</div>

<div align="center"> 

<!-- Social links -->
[![Discord](https://img.shields.io/badge/Discord-%237289DA.svg?logo=discord&logoColor=white)](htttps://discord.gg/Samson#0273) [![Facebook](https://img.shields.io/badge/Facebook-%231877F2.svg?logo=Facebook&logoColor=white)](https://www.facebook.com/chiemezie.nebeolisa/) [![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?logo=Instagram&logoColor=white)](https://www.instagram.com/samson_nebeolisa/) [![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/chiemezie-samson-nebeolisa-32897310b/) [![Stack Overflow](https://img.shields.io/badge/-Stackoverflow-FE7A16?logo=stack-overflow&logoColor=white)](https://stackoverflow.com/users/20653301/nebeolisa-chiemezie-samson) [![Twitter](https://img.shields.io/badge/Twitter-%231DA1F2.svg?logo=Twitter&logoColor=white)](https://twitter.com/SamsonChiemezie) [![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?logo=YouTube&logoColor=white)](https://myaccount.google.com/u/0/?utm_source=YouTubeWeb&tab=rk&utm_medium=act&tab=rk&hl=en) 

<!-- Portfolio -->
 📰 About Me [Portfolio](https://www.nebe-samson.com/)
 <br/>
  <small>Sep, 2024</small>
</div>

[<< Day 5](../Day5_Objects/Day5.md) | [Day 7 >>](../Day7_Functions_2/Day7.md)

<div align="center"> 
  <a class="header-image" target="_blank" href="../Asset/images/Days/Day_6.webp">
    <img alt="Typescript image" src="../Asset/images/Days/Day_6.webp" width="100%" height="600px">
  </a>
</div>

## Table of Contents

- [📔 Day 6](#-day-6)
- [Functions in TypeScript](#functions-in-typescript)
- [Function Parameters in TypeScript](#function-parameters-in-typescript)
  - [Parameter Type Annotations](#parameter-type-annotations)
  - [Optional Parameters](#optional-parameters)
  - [Default Parameters](#default-parameters)
  - [Named Parameters with Object Destructuring](#named-parameters-with-object-destructuring)
  - [Rest Parameters](#rest-parameters)
- [Function Types with Type Aliases](#function-types-with-type-aliases)
- [Return Type Annotations](#return-type-annotations)
- [Functions Returning Promises](#functions-returning-promises)
- [Anonymous Functions](#anonymous-functions)
- [Generic Functions](#generic-functions)
  - [Single Type Parameter](#single-type-parameter)
  - [Multiple Type Parameters](#multiple-type-parameters)
  - [Generic Functions with Arrays](#generic-functions-with-arrays)
  - [Generic Functions with Union Types](#generic-functions-with-union-types)
  - [Generic Parameter Defaults](#generic-parameter-defaults)
  - [Generic Constraints: Setting Boundaries for Flexibility](#generic-constraints:-setting-boundaries-for-flexibility)
- [Guidelines for Writing Good Generic Functions](#guidelines-for-writing-good-generic-functions)
  - [Why Use Generics?](#why-use-generics?)
- [💻 Day 6: Exercises](#-day-6-exercises)
  - [Exercise: Level 1](#exercise-level-1)
  - [Exercise: Level 2](#exercise-level-2)


# 📔 Day 6

## Functions in TypeScript

Functions are one of the most essential tools in JavaScript, and when working with TypeScript, they get even more powerful because you can specify the types of data that go in (inputs) and come out (outputs). This helps to catch errors early and makes your code more reliable.

> As I mentioned at the beginning of this course, I won't be diving deeply into basic JavaScript concepts when discussing TypeScript. I’m assuming you already have a foundational understanding of JavaScript functions. This allows us to focus more on how TypeScript enhances these familiar structures with features like strong typing and better error handling.

Different ways to define a function:

```ts
  // A. Basic functions
  function printToConsole(s: string) { 
    // The inferred return type is void
    return;
  }

  // same as the above
  function printToConsole(s: string): void {
    // 's' is a string, and the function doesn't return anything (void)
    console.log(s);
  }

  // returned type is number
  function adding(a: number, b: number): number {
    return a + b;
  }


  // B. Function expression
  const subtract = function (a: number, b: number): number {
    return a - b;
  };


  // C. Arrow functions 
  const multiply = (a: number, b: number): number => a * b;


  // D. Passing functions as an argument
  function greeter(fn: (a: string) => void) {
    fn("Hello, TypeScript!");
  }

  // In arrow functions
  const callWithHello = (fn: (a: string) => void) => {
    fn("Hello, Arrow Function!");
  };


  // E. Type aliases for functions
  type GreetFunction = (a: string) => void;

  function typeAlias(fn: GreetFunction) {
    fn("Hello, Type Alias!");
  }

  // F. Functions that never return
  function fail(msg: string): never {
    throw new Error(msg);
  }
```

In this guide, we'll focus on some of the most straightforward ways to describe and work with functions in TypeScript. 

## Function Parameters in TypeScript

In TypeScript, defining parameters for functions goes beyond just passing values. You can specify parameter types, make them optional, set defaults, and even use advanced techniques like destructuring and rest parameters. Let's dive into these concepts in a way that's easy to grasp.

### Parameter Type Annotations

In TypeScript, you can define the type of each parameter to ensure the function only accepts the correct type of input. The type annotation comes right after the parameter name.

```ts
  // The parameter name is explicitly set to be of type string.
  const greet = (name: string) => {
    console.log("Hello, " + name.toUpperCase());
  };

  greet();      // ❌ Error: Expected 1 argument, but got 0.
  greet("Samson"); // Works fine
  greet(23);    // ❌ Error: Argument of type 'number' is not assignable to parameter of type 'string'.
```

Even if you don’t explicitly specify types for your parameters, TypeScript will still check that you’re passing the right number of arguments.

### Optional Parameters

Sometimes, you might want a parameter to be optional. This means the function can be called without that parameter. You can make a parameter optional by adding a `?` after the parameter name.

```ts
  function add(a: number, b: number, c?: number) {  // (parameter) c: number | undefined
    return a + b + (c || 0); // If 'c' is not provided, it defaults to 0
  }

  add(23, 56);         // Works, 'c' is missing
  add(23, 56, 23);     // Works, 'c' is provided
  add(23, 56, undefined); // Also works, 'c' is explicitly set to undefined
```

>[!NOTE] When a parameter is optional, callers have the flexibility to pass `undefined`, which effectively behaves the same as if the argument was left out entirely, simulating a 'missing' argument.

### Default Parameters

You can also provide default values for parameters. If the caller doesn’t provide a value for that parameter, the default is used.

```ts
  function pow(value: number, exponent: number = 10) {
    return value ** exponent;
  }

  pow(2);     // 1024 (2 raised to the power of 10)
  pow(2, 3);  // 8 (2 raised to the power of 3)
```

### Named Parameters with Object Destructuring

When a function accepts many parameters, it’s better to pass them as an object and use destructuring to access them. This also allows the caller to pass values in any order.

```ts
  // example 1
  function divide({ dividend, divisor }: { dividend: number; divisor: number }) {
    return dividend / divisor;
  }

  // same as above
  type myType = { dividend: number, divisor: number }

  function divide({ dividend, divisor }: myType) {
    return dividend / divisor;
  }

  divide({ dividend: 20, divisor: 5 }); // 4

  // example 2
  function sum({ a, b, c }: { a: number; b: number; c: number }) {
    console.log(a + b + c);
  }

  sum({ a: 10, b: 3, c: 9 }); // Outputs: 22
```

This approach is great because it makes the code more readable, especially when you have many parameters.

### Rest Parameters

Sometimes, you don’t know how many arguments you’ll receive. TypeScript allows you to use __rest parameters__, which gather all the extra arguments into an array. This is useful when you have a dynamic number of inputs.

```ts
  function plus(a: number, b: number, ...rest: number[]) {
    return a + b + rest.reduce((prev, current) => prev + current, 0);
  }

  plus(10, 1, 2, 3, 4); // 10 + 1 + 2 + 3 + 4 = 20
```

## Function Types with Type Aliases

Instead of always defining function types inline, you can separate the type declaration from the function itself by using type aliases. This makes your code cleaner and easier to reuse.

```ts
  type Negate = (value: number) => number;

  const negateFunction: Negate = (value) => value * -1;
```

- Here, we defined a type alias `Negate` for a function that takes a number and returns a number.
- The `negateFunction` adheres to this type, taking a `value` and returning it negated (multiplied by -1).

## Return Type Annotations

You can explicitly define what type a function will return by adding a return type annotation after the parameter list. This makes it clear what kind of value the function will output.

```ts
  const count = (): number => {
    let num = 2 + 24;
    return num;
  };

  // The annotation (): number specifies that the function will return a number.
```

>[!Tip]
> TypeScript can often infer the return type automatically, based on what the function returns. But using explicit return types helps make your code more readable and maintainable.

## Functions Returning Promises

When a function returns a promise, TypeScript allows you to specify the Promise type as the return type. This is particularly useful when working with asynchronous functions.

```ts
  const getNumber = async (): Promise<number> => {
    return 26;
  };
```

> Why is this helpful? It helps TypeScript (and you) understand that `getNumber` will return a promise, not a regular number, so you’ll need to handle it with `.then()` or `await`.

## Anonymous Functions

An anonymous function is simply a function without a name. These are often used as callbacks in JavaScript methods like `.forEach()` and `.map()`.

```ts 
  const people = ["Alice", "Bob", "Eve"];

  people.forEach(function (s) {
    console.log(s.toUpperCase());
  });

  // You can also use arrow functions for a more concise syntax:
  people.map((s) => {
    console.log(s.toUpperCase());
  });
```

The function inside `.map()` is also anonymous but uses an arrow function for brevity.

__Contextual Typing__

In examples like `.forEach()` and `.map()`, the function doesn't have explicit type annotations, yet TypeScript contextually infers the types based on the array’s content. This is known as contextual typing.

- Since `people` is an array of strings, TypeScript knows that the parameter `s` in both `.forEach()` and `.map()` is a string, so it doesn’t complain when you call `toUpperCase()`.

## Generic Functions

A generic function in TypeScript is a powerful way to write reusable code that works with any data type. Instead of writing separate functions for different types of data, you can create one generic function that adapts to whatever type you pass in. This makes your code more flexible and prevents redundancy.

### Single Type Parameter

Let's start with a basic example of a generic function. A generic function allows you to define a "placeholder" type that gets determined when the function is called.

```ts
  // 'Type' is a placeholder for whatever type will be used later
  function identity<Type>(arg: Type): Type {
    return arg;
  }

  // Without Specifying Type Arguments
  let output1 = identity("Hello, TypeScript!");  // TypeScript infers the type as string
  let output2 = identity(42);  // TypeScript infers the type as number

  // Specifying Type Arguments Explicitly
  let output3 = identity<string>("Hello, TypeScript!");  // Here Type is string
  let output4 = identity<number>(100);  // Here Type is number
```

### Multiple Type Parameters

You can define generic functions that work with more than one type. This is useful when you need to handle multiple data types at the same time.

```ts
  function pair<T, U>(first: T, second: U): [T, U] {
    return [first, second];
  }

  let pair1 = pair<string, number>("Age", 25);  // T is string, U is number
  let pair2 = pair<boolean, string>(true, "Success"); // T is boolean, U is string
```

### Generic Functions with Arrays

You can also use generics to work with arrays. Here's a simple function that takes an array of any type and returns its length.

```ts 
  function getArrayLength<T>(arr: T[]): number {
    return arr.length;
  }

  let arrayLength = getArrayLength<number>([1, 2, 3]);  // T is number, array length is 3
```

### Generic Functions with Union Types

Sometimes, you want to allow multiple types for a single parameter. In that case, you can use union types in your generic function.

```ts 
  function wrapInArray<T>(value: T): T[] {
    return [value];
  }

  let stringArray = wrapInArray<string | number>("Hello");  // T can be string or number
  let numberArray = wrapInArray<string | number>(42);       // T can be string or number
```

### Generic Parameter Defaults

A really cool feature in TypeScript is generic parameter defaults, which lets you provide a default type for a generic function's parameters. If the caller doesn't specify the type, the function will use the default type.

```ts
  function createArray<T = string>(length: number, value: T): T[] {
    return Array(length).fill(value);
  }
```

- Here, T is a generic type with a default of string. If the user doesn’t specify a type, T will be assumed to be string.

__Example Usage:__

```ts
  // Without specifying a type (defaults to string):
  const stringArray = createArray(3, 'hello');
  console.log(stringArray); // Output: ['hello', 'hello', 'hello']

  // Specifying a type:
  const numberArray = createArray<number>(3, 42);
  console.log(numberArray); // Output: [42, 42, 42]

  // Specifying a different type:
  const booleanArray = createArray<boolean>(3, true);
  console.log(booleanArray); // Output: [true, true, true]
```

### Generic Constraints: Setting Boundaries for Flexibility

In some cases, you want to restrict the types that can be passed to a generic function. For instance, if you're working with objects that must have certain properties, you can use constraints.

```ts
  interface Lengthwise {
    length: number;
  }

  function logLength<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
  }
```

The logLength function now only accepts types that have a length property (like strings or arrays). The `<T extends Lengthwise>` constraint ensures this.

__Example Usage:__

```ts
  logLength("Hello!");  // Works, since string has a length property
  logLength([1, 2, 3, 4]);  // Works, since arrays have a length property

  // Attempting to pass a type without a length property—such as a number—will result in a compile-time error, preventing potential runtime issues.

  logLength(10); // ❌ Error: Argument of type 'number' is not assignable to parameter of type 'Lengthwise'.
```

## Guidelines for Writing Good Generic Functions

When creating generic functions, it's important to follow some best practices to ensure your code is clean, efficient, and easy to understand.

### 1. Keep It Simple and Purposeful

Your generic functions should be straightforward and serve a clear purpose. Avoid unnecessary complexity.

```ts
  // Good: Simple and effective
  function createPair<T, U>(first: T, second: U): [T, U] {
    return [first, second];
  }

  // Bad: Adding unnecessary complexity with extra generic parameters
  function createTriple<T, U, V>(first: T, second: U, third: V): [T, U, V] {
    return [first, second, third];
  }
```

### 2. Use Meaningful Type Parameter Names

Instead of always using T, U, or V, use more descriptive names for your type parameters when it makes sense. This helps communicate what the generic is representing.

### 3. Leverage Type Inference

You don’t always need to specify types explicitly. TypeScript is smart enough to infer types based on the values you pass. Let TypeScript do the heavy lifting when possible!

```ts
  function getFirstElement<T>(arr: T[]): T | undefined {
    return arr.length > 0 ? arr[0] : undefined;
  }

  const firstNumber = getFirstElement([1, 2, 3]); // TypeScript knows this returns a number
```

### 4. Use Constraints to Restrict Type Parameters

Constraints ensure that your generic functions work only with the types you expect. Use them when you want to restrict the types that can be passed in.

```ts
  function logLength<T extends { length: number }>(arg: T): T {
    console.log(arg.length);
    return arg;
  }
```

### 5. Avoid Excessive Generic Parameters

Too many generic parameters can make a function harder to read and understand. Stick to what's necessary.

### 6. Ensure Type Safety and Consistency

Make sure your generic functions are consistent in how they handle types, and that they return what users would expect.

```ts
  // Good: Consistent and type-safe
  function getFirstElement<T>(arr: T[]): T | undefined {
    return arr.length > 0 ? arr[0] : undefined;
  }

  // Bad: Inconsistent and potentially unsafe
  function getFirstElementBad(arr: any[]): any {
    return arr.length > 0 ? arr[0] : null; // This returns null, which is unexpected
  }
```

### 7. Document Your Generic Functions

Add comments to explain what your generic functions are doing. This makes them easier to use and understand, especially for other developers.

```ts
  /**
   * Creates an array filled with a specific value.
   * @param length - The number of elements in the array.
   * @param value - The value to fill the array with.
   * @returns An array of the specified length, filled with the given value.
   */
  function createArray<T = string>(length: number, value: T): T[] {
    return Array(length).fill(value);
  }
```

🌟 Awesome job! You’ve successfully completed your Day 6, and you're well on your way to becoming a great developer. Keep up the momentum! Now, let's keep your mind sharp and your body active with some quick exercises.

## 💻 Day 6: Exercises

### Exercise: Level 1

1. Create a function called `greet` that accepts a `name` as a parameter and logs `"Hello, [name]!"` to the console.

2. Write a function called `add` that takes two numbers, `a` and `b`, and an optional number `c`. If c is provided, return the sum of all three; otherwise, return the sum of `a` and `b`.

3. Convert the following regular function to an arrow function:

```ts
  function multiply(a: number, b: number): number {
    return a * b;
  }
```

4. Write a function `square` that takes a number as input and returns its square. Ensure that TypeScript can infer the return type.

### Exercise: Level 2

5. Write a generic function `reverseArray` that takes an array of any type and returns the reversed version of that array.

6. Write a function `calculatePower` that takes a number `value` and an optional `exponent` (default is 2). The function should return `value` raised to the power of `exponent`.

7. Given an array of numbers, use an anonymous function to return an array where each number is doubled.

8. Write an asynchronous function `fetchData` that simulates fetching data from an API and returns a string. Use `async` and `await` to handle the promise and return the fetched data.

9. Write a function `logFirstElement` that takes an array and logs its first element. Use TypeScript's contextual typing to ensure TypeScript automatically infers the type of the array elements.

10. Define a type alias LogMessage for a function that takes a string as input and logs it to the console.

- Then, write a function logWarning that uses this type alias to log a warning message to the console.

🎉 CONGRATULATIONS ! 🎉

[<< Day 5](../Day5_Objects/Day5.md) | [Day 7 >>](../Day7_Functions_2/Day7.md)