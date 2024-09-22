<div align="center"> 
  <h1>30 Days of Basics TypeScript: Narrowing</h1>
</div>

<div align="center"> 

<!-- Social links -->
[![Discord](https://img.shields.io/badge/Discord-%237289DA.svg?logo=discord&logoColor=white)](htttps://discord.gg/Samson#0273) [![Facebook](https://img.shields.io/badge/Facebook-%231877F2.svg?logo=Facebook&logoColor=white)](https://www.facebook.com/chiemezie.nebeolisa/) [![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?logo=Instagram&logoColor=white)](https://www.instagram.com/samson_nebeolisa/) [![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/chiemezie-samson-nebeolisa-32897310b/) [![Stack Overflow](https://img.shields.io/badge/-Stackoverflow-FE7A16?logo=stack-overflow&logoColor=white)](https://stackoverflow.com/users/20653301/nebeolisa-chiemezie-samson) [![Twitter](https://img.shields.io/badge/Twitter-%231DA1F2.svg?logo=Twitter&logoColor=white)](https://twitter.com/SamsonChiemezie) [![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?logo=YouTube&logoColor=white)](https://myaccount.google.com/u/0/?utm_source=YouTubeWeb&tab=rk&utm_medium=act&tab=rk&hl=en) 

<!-- Portfolio -->
 📰 About Me [Portfolio](https://www.nebe-samson.com/)
 <br/>
  <small>Sep, 2024</small>
</div>

[<< Day 7](../Day7_Functions_2/Day7.md) | [Day 9 >>](../Day9_Type_Manipulation/Day9)

<div align="center"> 
  <a class="header-image" target="_blank" href="../Asset/images/Days/Day_8.webp">
    <img alt="Typescript image" src="../Asset/images/Days/Day_8.webp" width="100%" height="600px">
  </a>
</div>

## Table of Contents

- [📔 Day 8](#-day-8)
- [TypeScript Narrowing](#typescript_narrowing)
  - [1. Truthiness Narrowing](#1-truthiness-narrowing)
  - [2. Equality Narrowing](#2-equality-narrowing)
- [Why Narrowing Matters](#why-narrowing-matters)
- [Truthiness Narrowing](#truthiness-narrowing)
- [Checking for Specific Properties in Objects](#checking-for-specific-properties-in-objects)
- [Narrowing with Optional Properties](#narrowing-with-optional-properties)
- [Handling `null` and `undefined`](#handling-null-and-undefined)
- [TypeScript Type Predicates](#typeScript-type-predicates)
  - [What is a Type Predicate?](#what-is-a-type-predicate?)
- [💻 Day 8: Exercises](#-day-8-exercises)
  - [Exercise: Level 1](#exercise-level-1)
  - [Exercise: Level 2](#exercise-level-2)
  - [Exercise: Level 3](#exercise-level-3)


# 📔 Day 8

## TypeScript Narrowing

Imagine you're working on a project, and you're dealing with a variable that could either be a string or a number. TypeScript needs to know what type the variable is before it can decide which operations are allowed. This process of guiding TypeScript to understand the specific type of a variable is called narrowing.

As a developer, you’re often dealing with dynamic data—like user inputs, API responses, or even states that can be different types depending on the situation. Narrowing helps ensure that your code works reliably, no matter what the data looks like.

TypeScript offers several ways to narrow down the type of a variable. Here are a few key ones:

### 1. Truthiness Narrowing

- __Logical AND (`&&`)__: Ensures that both operands are truthy. If the left-hand operand is falsy, it short-circuits, and TypeScript knows the second operand won't run.

```ts
  function checkValue(value: string | null) {
    value && console.log(value.toUpperCase());  // Runs only if value is truthy
  }
```

- __Double negation (`!!`)__: Converts any value into its strict Boolean equivalent. This can help TypeScript understand that a value exists and isn't `null` or `undefined`.

```ts
  const isTruthy = !!value;  // Converts any value to true or false
```

- __Logical OR (`||`)__: Returns the first truthy value or the last falsy value, depending on the context. It's commonly used to provide fallback values.

```ts
  const finalValue = value || "default";  // If 'value' is falsy, use "default"
```

- __Negation (`!`)__: Inverts the truthiness of a value. This is especially useful when checking if something is falsy.

```ts
  if (!value) {
    console.log("Value is falsy");  // Runs if 'value' is falsy
  }
```

- __Boolean() constructor__: Explicitly converts a value into a boolean (`true` or `false`). This can be useful when you need to ensure that something is interpreted as a boolean in your logic.

```ts
  const isValid = Boolean(value);  // Explicitly converts 'value' to true or false
```

- __`.every(Boolean)`__: Used with arrays, this method checks if all elements in the array are truthy. It can narrow down an array of possible values to only those that are meaningful.

```ts
  const allTruthy = [1, "hello", true].every(Boolean);  // Returns true if all values are truthy
```

- __`if` statements__: The most common form of truthiness narrowing. Inside the if block, TypeScript knows that the value being checked is truthy.

```ts
  if (value) {
    console.log(value);  // 'value' is guaranteed to be truthy here
  }
```

### 2. Equality Narrowing

Equality narrowing involves comparing values using equality operators to help TypeScript narrow down the type of a variable.

- __Strict equality (`===`)__: This operator checks both value and type. It's a precise way to compare two values, allowing TypeScript to narrow the type based on the result.

```ts
  function checkEqual(value: string | number) {
    if (value === "specificString") {
      // TypeScript now knows 'value' is a string and matches "specificString"
    }
  }
```

- __Strict inequality (`!==`)__: Like `===`, but it checks if two values are not equal. It helps TypeScript eliminate one type when narrowing down the possibilities.

```ts
  if (value !== null) {
    // TypeScript knows 'value' isn't null
  }
```

- __Loose equality (`==`) and inequality (`!=`)__: These operators compare values after coercing them to a common type. While less safe than strict equality, they can still be useful in certain scenarios for type narrowing, especially in legacy code or when dealing with mixed data types.

```ts
  if (value == 0) {
    // TypeScript narrows 'value' down to something that can be coerced to 0 (like number or string)
  }
```

- __`switch` statements__: These allow you to compare a variable against multiple possible values, effectively narrowing down the type inside each case block.

```ts 
  function checkSwitch(value: string | number | boolean) {
    switch (typeof value) {
      case "string":
        console.log("It's a string!");
        break;
      case "number":
        console.log("It's a number!");
        break;
      case "boolean":
        console.log("It's a boolean!");
        break;
    }
  }
```

## Why Narrowing Matters

Let's say you have a function that takes a parameter which could either be a string or a number, and you want to log the length of the string if it's a string. If it's a number, you'll just log the number itself.

```ts
  function printLength(value: string | number) {
    console.log(value.toLocaleUpperCase()); // ❌ Error: Property 'toLocaleUpperCase' does not exist on type 'string | number'.
    // Property 'toLocaleUpperCase' does not exist on type 'number'.
  }
```

In this example, you get an error because TypeScript doesn't know if value is a string or a number, so it can't safely allow string-specific methods like toLocaleUpperCase() to run.

This is where narrowing comes into play. By using some checks, you can help TypeScript figure out exactly what type value is. To fix this, you can use a type guard like typeof to help TypeScript understand the type. Here's how:

```ts
  function printLength(value: string | number) {
    // Check if the value is a string
    if (typeof value === "string") {
      // Inside this block, TypeScript now knows 'value' is a string
      console.log(value.length);  // String-specific method
    } else {
      // Inside this block, TypeScript knows 'value' is a number
      console.log(value);  // Number-specific logic
    }
  }
```

## Truthiness Narrowing

As seen above, __Truthiness narrowing__ happens when you check whether a value is "truthy" (exists and is not `null`, `undefined`, `0`, `""`, etc.) or "falsy" (like `null`, `undefined`, or `false`). TypeScript automatically understands the type better after you make such checks.

```ts
  interface User {
    name: string;
    age: number;
  }

  function getUserAge(user: User | null): string {
    if (user && user.age) { // Checking both user and user.age
      return `User's age is ${user.age}`;
    } else if (user) {  // If user exists but age is missing
      return "User's age is unknown";
    } else {  // If user is null
      return "No user found";
    }
  }
```

__Truthiness narrowing__ is especially useful when working with nullable values (like null or undefined), which are common in real-world applications. This pattern ensures your code doesn’t break due to missing data.

## Checking for Specific Properties in Objects

The `in` operator is used to check if a certain property exists within an object. This is another powerful tool for narrowing down the type, especially when dealing with union types (types that could be one of several shapes or forms).

```ts
  type Dog = {
    breed: string;
    bark: () => void;
  };

  type Cat = {
    breed: string;
    meow: () => void;
  };

  type Pet = Dog | Cat;

  function makeSound(pet: Pet) {
    if ("bark" in pet) {
      // TypeScript knows here that pet is a Dog
      pet.bark();
    } else {
      // TypeScript knows here that pet is a Cat
      pet.meow();
    }
  }
```

This is a clean and type-safe way to work with union types, where an object can take different shapes.

## Narrowing with Optional Properties

Sometimes, your data may have optional properties. In these cases, narrowing helps TypeScript understand whether a certain property exists before you use it. You can combine the in operator with optional properties to handle different cases gracefully.

```ts
  interface UserWithAddress {
    name: string;
    address: {
      street: string;
      city: string;
    };
  }

  interface UserWithoutAddress {
    name: string;
  }

  type UserAddress = UserWithAddress | UserWithoutAddress;

  function printAddress(user: UserAddress) {
    if ("address" in user) {
      // TypeScript knows here that user has an address (UserWithAddress)
      console.log(`User lives in ${user.address.city}`);
    } else {
      // TypeScript knows here that user does not have an address (UserWithoutAddress)
      console.log("User's address is unknown");
    }
  }
```

## Handling `null` and `undefined`

In TypeScript, checking for null automatically excludes undefined from the type, and vice versa. This allows for cleaner handling of nullable values.

```ts
  interface Container {
    value: number | null | undefined;
  }

  function multiplyValue(container: Container, factor: number) {
    // Remove both 'null' and 'undefined' from the type
    if (container.value != null) {
      // Now we can safely multiply 'container.value'
      container.value *= factor;
    }
  }
```

## TypeScript Type Predicates

When working with TypeScript, sometimes you need more control over how you manage different types in your code. Type predicates allow you to tell TypeScript exactly what type you're dealing with inside a conditional block. This is super useful when you're working with complex types that may have shared properties but need different handling.

Let’s break down what type predicates are, and how they can simplify your life as a front-end developer.

### What is a Type Predicate?

A type predicate is a special return type for a function that tells TypeScript, “Hey, this variable is actually this specific type.” This helps TypeScript understand how to narrow down the type based on conditions you define yourself.

Here’s the basic syntax:

```ts 
  function isType(value: any): value is SpecificType {
    // logic to determine if 'value' is of 'SpecificType'
  }
```

Let’s revisit the example of handling pets where we want to determine whether a pet is a `Dog` or a `Cat`.

```ts
  type Dog = {
    breed: string;
    bark: () => void;
  };

  type Cat = {
    breed: string;
    meow: () => void;
  };

  type Pet = Dog | Cat;

  function isDog(pet: Pet): pet is Dog {
    // If 'pet' has a 'bark' method, it's a Dog
    return (pet as Dog).bark !== undefined;
  }

  function makeSound(pet: Pet) {
    if (isDog(pet)) {
      pet.bark(); // TypeScript now knows pet is a Dog
    } else {
      pet.meow(); // TypeScript now knows pet is a Cat
    }
  }
```

Let’s apply this to a more real-world scenario where you’re dealing with user roles, like `Admin` and `Leader`. Depending on the role, the user might have different privileges.

```ts
  interface Admin {
    role: "admin";
    privileges: string[];
  }

  interface Leader {
    role: "user";
    email: string;
  }

  type Person = Admin | Leader;

  function isAdmin(person: Person): person is Admin {
    // If the role is 'admin', the person is an Admin
    return person.role === "admin";
  }

  function getPrivileges(person: Person): string[] {
    if (isAdmin(person)) {
      return person.privileges; // TypeScript knows person is an Admin here
    } else {
      return []; // TypeScript knows person is a Leader, no privileges here
    }
  }
```

Here’s a more dynamic example where we need to check whether an animal can swim or fly. This time, let’s handle animals that are either `Fish` or `Bird`.

```ts
  type Fish = { swim: () => void };
  type Bird = { fly: () => void };

  function isFish(animal: Fish | Bird): animal is Fish {
    // If 'animal' has a 'swim' method, it's a Fish
    return (animal as Fish).swim !== undefined;
  }

  function moveAnimals(animals: (Fish | Bird)[]) {
    animals.forEach((animal) => {
      if (isFish(animal)) {
        animal.swim(); // TypeScript knows this animal is a Fish
      } else {
        animal.fly(); // TypeScript knows this animal is a Bird
      }
    });
  }
```

🌟 Awesome job! You’ve successfully completed your Day 8, and you're well on your way to becoming a great developer. Keep up the momentum! Now, let's keep your mind sharp and your body active with some quick exercises.

## 💻 Day 8: Exercises

### Exercise: Level 1

1. You have a function that takes a parameter `data` which could either be a `string` or a `number`. Write a function that checks the type of `data` using `typeof` and logs the length of the string if it's a string, or the doubled value if it's a number.

2. You have an object `config` that could either be `null` or an object with a property `enabled` (boolean). Write a function that returns `true` if `enabled` is `true`, and `false` otherwise.

3. You have two types, `Book` and `Magazine`, both with a `title` property, but only `Magazine` has a `publicationFrequency`. Write a function that checks if an object is a `Magazine` using the `in` operator and logs the `publicationFrequency` if it’s a `Magazine`.

### Exercise: Level 2

4. You have two types, `Fruit` and `Vegetable`. Both have a `color` property, but only `Fruit` has a `sweetness` property. Write a type predicate `isFruit` and use it to differentiate between `Fruit` and `Vegetable` in a function that logs details about the item.

5. You are working with a type `User` that may or may not have an optional `address` property. Write a function that logs the user's `city` if the `address` exists, and "No address provided" if not.

6. You want to create a function `calculateArea` that can accept either a square or a circle. For a square, it should return the area based on the length of a side, and for a circle, it should return the area based on the radius. Use function overloading to handle this.

### Exercise: Level 3

7. You have an array of items that could be either `Car` or `Bike`. Write a type predicate to filter out only the `Car` objects from the array and return an array of `Car` types.

8. You need to create a utility function `getValue` that returns a different result based on the type of the value passed in. If it’s a `string`, return its length, if it’s a `number`, return its square, and for other types, return `"Unknown"`.

9. You’re given a recursive object type where each node can have children of the same type or be null. Write a function that traverses the tree and counts the total number of nodes.

🎉 CONGRATULATIONS ! 🎉

[<< Day 7](../Day7_Functions_2/Day7.md) | [Day 9 >>](../Day9_Type_Manipulation/Day9)