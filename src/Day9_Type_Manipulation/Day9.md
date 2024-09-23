<div align="center"> 
  <h1>30 Days of Basics TypeScript: Type Manipulation</h1>
</div>

<div align="center"> 

<!-- Social links -->
[![Discord](https://img.shields.io/badge/Discord-%237289DA.svg?logo=discord&logoColor=white)](htttps://discord.gg/Samson#0273) [![Facebook](https://img.shields.io/badge/Facebook-%231877F2.svg?logo=Facebook&logoColor=white)](https://www.facebook.com/chiemezie.nebeolisa/) [![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?logo=Instagram&logoColor=white)](https://www.instagram.com/samson_nebeolisa/) [![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/chiemezie-samson-nebeolisa-32897310b/) [![Stack Overflow](https://img.shields.io/badge/-Stackoverflow-FE7A16?logo=stack-overflow&logoColor=white)](https://stackoverflow.com/users/20653301/nebeolisa-chiemezie-samson) [![Twitter](https://img.shields.io/badge/Twitter-%231DA1F2.svg?logo=Twitter&logoColor=white)](https://twitter.com/SamsonChiemezie) [![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?logo=YouTube&logoColor=white)](https://myaccount.google.com/u/0/?utm_source=YouTubeWeb&tab=rk&utm_medium=act&tab=rk&hl=en) 

<!-- Portfolio -->
 📰 About Me [Portfolio](https://www.nebe-samson.com/)
 <br/>
  <small>Sep, 2024</small>
</div>

[<< Day 8](../Day8_Narrowing/Day8.md) | [Day 10 >>](../Day10_Conditional_Types/Day10.md)

<div align="center"> 
  <a class="header-image" target="_blank" href="../Asset/images/Days/Day_9.webp">
    <img alt="Typescript image" src="../Asset/images/Days/Day_9.webp" width="100%" height="600px">
  </a>
</div>

## Table of Contents

- [📔 Day 9](#-day-9)
- [TypeScript Type Manipulation](#typescript-type-manipulation)
- [The `keyof` Operator](#the-keyof-operator)
  - [Index Signatures with `keyof`](#index-signatures-with-keyof)
- [The `typeof` Operator](#the-typeof-operator)
  - [How it Works](#how-it-works)
  - [Use Cases for TypeScript’s `typeof`](#use-cases-for-typescript-s-typeof)
  - [Using `typeof` with Literal Types](#using-typeof-with-literal-types)
- [Combining `typeof` with `keyof` for Object Keys](#combining-typeof-with-keyof-for-object-keys)
- [Indexed Access Types](#indexed-access-types)
  - [Accessing Nested Properties](#accessing-nested-properties)
  - [Using Indexed Access Types with Arrays](#using-indexed-access-types-with-arrays)
  - [Creating Union Types from Object Values](#creating-union-types-from-object-values)
  - [Using Indexed Access Types with Generics](#using-indexed-access-types-with-generics)
  - [Handling Optional Properties with Indexed Access Types](#handling-optional-properties-with-indexed-access-types)
- [💻 Day 9: Exercises](#-day-9-exercises)
  - [Exercise: Level 1](#exercise-level-1)
  - [Exercise: Level 2](#exercise-level-2)
  - [Exercise: Level 3](#exercise-level-3)


# 📔 Day 9

## TypeScript Type Manipulation

TypeScript offers powerful tools to work with types, especially when dealing with objects and functions. Two important operators you should know are `keyof` and `typeof`. 

## The `keyof` Operator

The `keyof` operator in TypeScript is a powerful tool that lets you create a union of all the property keys from a given object type. Think of it as a way to "list out" the keys of an object and use them directly as types. This allows you to ensure that any key you reference is valid and actually exists on the object, enhancing type safety and preventing mistakes before your code even runs. This is super useful when you want to ensure you're working with valid object keys.

Let's start with a basic example:

```ts
  type Person = {
    name: string;
    age: number;
    address: string;
  };

  type PersonKeys = keyof Person;
```

Here, `PersonKeys` will be a union of the keys of the `Person` type, which are `"name" | "age" | "address"`. Now you can use this union type to make sure you're only working with valid keys.

__Practical Example:__

Imagine you want to create a function that retrieves a value from an object. But you want to make sure the key you're passing is valid—i.e., it actually exists on the object.

```ts
  function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
  }

  const person = {
    name: "John",
    age: 30,
    address: "123 Main St",
  };

  const personName = getProperty(person, "name"); // Valid, returns "John"
  const personAge = getProperty(person, "age"); // Valid, returns 30
  const invalid = getProperty(person, "height"); // ❌ Error: 'height' does not exist on type 'Person
```

### Index Signatures with `keyof`

You can also use keyof with objects that have index signatures, which allow for dynamic keys and can be used to extract the index type. For example:

```ts
  // example 1:
  type ArrayLike = { [index: number]: unknown };

  type ArrayKey = keyof ArrayLike; // type A = number

  // Here, ArrayKey will be of type number, because the keys of the object are dynamic numbers (just like in an array).

  // example 2:
  type StringMap = { [key: string]: unknown };

  function createStringPair(property: keyof StringMap, value: string): StringMap {
    return { [property]: value };
  }

  // usage:
  const result = createStringPair("username", "Samson");
  console.log(result); // { username: "Samson" }
```

## The `typeof` Operator

The `typeof` type operator in TypeScript is a powerful tool that allows you to capture the type of a variable, not just its value. Unlike JavaScript's `typeof` operator, which evaluates a variable at runtime and returns a string like `"number"`, `"string"`, or `"object"`, TypeScript’s `typeof` works at compile time and extracts the actual type of the variable.

In essence, it enables you to reference the type of a variable or value without having to manually declare it again. This can be incredibly useful for keeping your code DRY (Don't Repeat Yourself), as it reduces redundancy and keeps your type definitions consistent.

### How it Works

In TypeScript, `typeof` doesn’t check the runtime type; instead, it reflects the type the compiler knows. You can use it to get the type signature of a variable, object, or function, and reuse it elsewhere in your code.

Here’s an example:
```ts
  // example 1:
  let greeting = "Hello, TypeScript!";

  type GreetingType = typeof greeting; // type GreetingType = string

  // example 2:
  const person = {
    name: "Samson",
    age: 30,
  };

  type PersonType = typeof person; // This will create a type equivalent to { name: string; age: number; }
```

### Use Cases for TypeScript’s `typeof`

1. __Reusable Type Definitions__: Instead of manually defining a type, you can use `typeof` to create types directly from existing variables. This ensures your types stay in sync with the actual objects or functions.

```ts
  const user = {
    name: "Samson",
    age: 25,
  };

  type UserType = typeof user; // type UserType =  { name: string; age: number; }

  const anotherUser: UserType = {
    name: "Bob",
    age: 30,
  };
```

2. __Function Type Extraction__: You can use `typeof` to reference the type of a function, making it easy to pass around or use as a parameter without having to manually define the function’s signature.

```ts
  function add(a: number, b: number): number {
    return a + b;
  }

  type AddFunctionType = typeof add; // type AddFunctionType = (a: number, b: number) => number

  const multiply: AddFunctionType = (x, y) => x * y;
```

Now, `AddFunctionType` is the type for any function that takes two numbers and returns a number. We can reuse this type for other operations like `multiply`.

3. __Avoid Redundant Type Declarations__: When you’re working with large objects or complex types, using typeof lets you avoid duplicating type definitions. You can refer to the type of an existing value directly, keeping your codebase more maintainable.

### Using `typeof` with Literal Types

The typeof type operator in TypeScript isn’t just for basic values like number or string. It can also capture specific literal values. If you're not familiar with Literal Types yet, no need to worry! We'll dive into that in an upcoming lesson, so you'll have a solid understanding soon.

```ts
  const status5 = "success";
  type StatusType = typeof status5; // StatusType is 'success'

  let currentStatus: StatusType = "success"; // This works
  currentStatus = "failure"; // ❌ Error: Type '"failure"' is not assignable to type '"success"'
```

In this example, `StatusType` is not just a `string`, but exactly the string `"success"`. This can be useful when you want to enforce specific values instead of allowing any string.

## Combining `typeof` with `keyof` for Object Keys

The `keyof` operator is used to get all the keys (or property names) of an object as a union type. When you combine it with `typeof`, it creates a type based on the keys of an existing object.

```ts
  const colors = {
    red: "#ff0000",
    green: "#00ff00",
    blue: "#0000ff",
  };

  type ColorKeys = keyof typeof colors; // 'red' | 'green' | 'blue'

  function getColor(color: ColorKeys): string {
    return colors[color];
  }

  console.log(getColor("red")); // "#ff0000"

  // Trying to pass anything outside 'red' | 'green' | 'blue' values causes an error
  console.log(getColor("yellow")); // ❌ Error: Argument of type '"yellow"' is not assignable to parameter of type 'ColorKeys'.
```

## Indexed Access Types

__Indexed access types__ let you access the type of a specific property or element from an object or array. This means you can extract and reuse the type of any property, keeping your code more dynamic.

Basic example:

```ts
  interface Person {
    name: string;
    age: number;
    address: {
      street: string;
      city: string;
    };
  }

  type Age = Person['age'];  // Age is 'number'
  type Address = Person['address'];  // Address is '{ street: string; city: string }'
```

### Accessing Nested Properties

You can access deeper levels of an object by stacking the square brackets:

```ts 
  // accessing our previous Person
  type Street = Person['address']['street'];  // Street is 'string'

  // next example
  type Company = {
    employees: {
      name: string;
      position: string;
    }[];
  };

  type EmployeeNameType = Company['employees'][number]['name']; // EmployeeNameType is 'string'
```

### Using Indexed Access Types with Arrays

When you use indexed access types with arrays, TypeScript helps you get the type of array elements easily.

```ts
  type Numbers = number[];

  type FirstElementType = Numbers[0]; // FirstElementType is 'number'
```

`Numbers[0]`: This notation tells TypeScript to access the type of the element at index `0` in the `Numbers` array. Since the array is defined as `number[]`, every element in the array, including the first one (`Numbers[0]`), is of type `number`.

Let's see another example:

```ts
  const fruits = ['apple', 'banana', 'orange'] as const;

  type Fruit = typeof fruits[number];  // 'apple' | 'banana' | 'orange'
```

### Creating Union Types from Object Values

You can use keyof and indexed access types to create union types based on the values of an object.

```ts
  // example 1:
  interface ColorVariants {
    primary: 'blue';
    secondary: 'green';
    tertiary: 'red';
  }

  type Color = ColorVariants[keyof ColorVariants];  // 'blue' | 'green' | 'red'
  type Color2 = ColorVariants["primary" | "secondary"]; // "green" | "blue"

  // example 2:
  type Car = {
    make: string;
    model: string;
    year: number;
  };

  type CarProperty = keyof Car; // 'make' | 'model' | 'year'
  type CarValueType = Car[CarProperty];  // 'string' | 'number'
```

### Using Indexed Access Types with Generics

Indexed access types become even more powerful when combined with generics. This lets you extract types dynamically based on the type of object or key passed in. The above 'Practical Example' is a good example of indexed access types with generics.

```ts
  interface Person {
    name: string;
    age: number;
    address: {
      street: string;
      city: string;
    };
  }

  const person: Person = {
    name: 'Samson',
    age: 30,
    address: { street: 'Main St', city: 'New York' }
  };

  function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
  }

  const personName = getProperty(person, 'name');  // Type of personName is 'string'
  const personAge = getProperty(person, 'age');    // Type of personAge is 'number'
```

### Handling Optional Properties with Indexed Access Types

Indexed access types also work with optional properties, which means you can handle cases where properties may be `undefined`.

```ts
  type User = {
    id: number;
    name?: string; // optional property
  };

  type UserNameType = User['name']; // UserNameType is 'string | undefined'
```

Because `name` is optional, `UserNameType` becomes `string | undefined`, giving you the flexibility to handle both cases safely.

🌟 Awesome job! You’ve successfully completed your Day 9, and you're well on your way to becoming a great developer. Keep up the momentum! Now, let's keep your mind sharp and your body active with some quick exercises.

## 💻 Day 9: Exercises

### Exercise: Level 1

1. You have the following object representing a person's contact details:

```ts
  const contact = {
    phone: "123-456-7890",
    email: "example@example.com",
    address: "123 Main St",
  };
```
Using `keyof`, create a type that represents the keys of the contact object.

- Define a type `ContactKeys` that will result in the union of `'phone' | 'email' | 'address'`.

2. create a `user` object and use the `typeof` operator to define a type `UserType` based on the structure of the `user` object.

3. Consider the following type that represents a blog post:

```ts
  type Blog = {
    title: string;
    content: string;
    author: {
      name: string;
      email: string;
    };
    tags: string[];
  };
```

- Define a type `AuthorNameType` that extracts the type of the `name` property from the `author` object using indexed access types.

4. create a `settings` object with `theme` (string `dark` or `light`), `notificationsEnabled` boolean and `language` (string) as it's items. Use `keyof` and `typeof` to define a type `SettingsKeys`, which should be a union of all the keys in the `settings` object.

5. You are given an array of users, and you want to extract the type of a single user's `username`.  Define a type `UsernameType` that extracts the type of `username` from a user in the `Users` array using indexed access types.

### Exercise: Level 2

6. Consider the following config object:

```ts
  const config = {
    version: 1.2,
    apiKey: "1234567890abcdef",
    debug: false,
  };
```

- Write a function `updateConfig` that accepts a key (of type `keyof typeof config`) and a new value. The function should update the `config` object with the new value, ensuring that the type of the value matches the key's expected type.

7. You’re working with an online store that has the following inventory structure:

```ts
  type Inventory = {
    books: {
      title: string;
      author: string;
      stock: number;
    }[];
    electronics: {
      brand: string;
      model: string;
      stock: number;
    }[];
    furniture: {
      name: string;
      material: string;
    }[];
  };
```

- Define a type `StockType` that extracts the `stock` property type from both `books` and `electronics` in the `Inventory` type using indexed access types.

- Write a function `getItemDetails` that accepts a category (`"books" | "electronics" | "furniture"`) and an index (a number) and returns the corresponding item from that category. Use function overloading and indexed access types to ensure that the return type matches the category.

8. You have a system where you want to retrieve specific properties from an object, depending on the category:

```ts
  type Categories = {
    movies: { title: string; director: string };
    games: { title: string; developer: string };
  };
```

- Write a function `getCategoryDetail` that accepts a category (`"movies" | "games"`) and a key from that category (`"title" | "director"` for movies, `"title" | "developer"` for games), and returns the corresponding value using function overloading.

### Exercise: Level 3

9. You have a deeply nested object type representing a company structure:

```ts
  type Company = {
    departments: {
      engineering: {
        teamLead: { name: string; yearsOfExperience: number };
        teamMembers: { name: string; skills: string[] }[];
      };
      marketing: {
        teamLead: { name: string; campaignsLed: number };
        teamMembers: { name: string; projectsCompleted: number }[];
      };
    };
  };
```

- Define a type `EngineeringTeamLeadNameType` that extracts the type of the `name` property of the `teamLead` in the `engineering` department using indexed access types.

10. create a utility type that extracts all optional properties from a given type.  Define a utility type `OptionalKeys<T>` that takes a type `T` and returns a union of all the keys in `T` that are optional.


🎉 CONGRATULATIONS ! 🎉

[<< Day 8](../Day8_Narrowing/Day8.md) | [Day 10 >>](../Day10_Conditional_Types/Day10.md)