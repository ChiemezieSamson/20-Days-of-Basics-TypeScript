<div align="center"> 
  <h1>20 Days of Basics TypeScript: Conditional Types</h1>
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

[<< Day 9](../Day9_Type_Manipulation/Day9.md) | [Day 11 >>](../Day11_Mapped_Types/Day11.md)

<div align="center"> 
  <a class="header-image" target="_blank" href="../Asset/images/Days/Day_10.webp">
    <img alt="Typescript image" src="../Asset/images/Days/Day_10.webp" width="100%" height="600px">
  </a>
</div>

## Table of Contents

- [📔 Day 10](#-day-10)
- [TypeScript Conditional Types](#typescript-conditional-types)
  - [Checking Subtypes](#checking-subtypes)
  - [Handling Union Types with Conditional Types](#handling-union-types-with-conditional-types)
  - [Multiple Conditions in Conditional](#multiple-conditions-in-conditional)
  - [Inferring Types with `infer` Keyword](#inferring-types-with-infer-keyword)
  - [Conditional Types for Function Return Types](#conditional-types-for-function-return-types)
  - [Discriminated Unions with Conditional Types](#discriminated-unions-with-conditional-types)
  - [Distributive Conditional Types](#distributive-conditional-types)
- [Conditional Type Constraints](#conditional-type-constraints)
  - [Basic Conditional Type Constraints](#basic-conditional-type-constraints)
  - [Using `keyof` with Constraints](#using-keyof-with-constraints)
  - [Return Types with Function Constraints](#return-types-with-function-constraints)
  - [Safe Property Access for Deeply Nested Objects](#safe-property-access-for-deeply-nested-objects)
- [💻 Day 10: Exercises](#-day-10-exercises)
  - [Exercise: Level 1](#exercise-level-1)
  - [Exercise: Level 2](#exercise-level-2)
  - [Exercise: Level 3](#exercise-level-3)


# 📔 Day 10

## TypeScript Conditional Types

In TypeScript, conditional types are a way to write types that change based on certain conditions. This is similar to using `if` statements in JavaScript, but instead of checking values, you're checking types. It's a powerful tool that allows you to make your types more dynamic and flexible.

The basic syntax of a conditional type looks like this:

```ts
  T extends U ? X : Y
```

This means:

- __T__: The type you're checking.
- __U__: The type you're comparing against.
- __X__: The type that gets returned if the condition is true (if `T` extends `U`).
- __Y__: The type that gets returned if the condition is false (if `T` does not extend `U`).

Here's a simple example where we check if a type is a string or not:

```ts
  type IsString<T> = T extends string ? true : false;

  type Result1 = IsString<"hello">;  // true (because "hello" is a string)
  type Result2 = IsString<42>; // false (because 42 is not a string)

  // We can even return custom messages based on the condition:
  type IsString2<T> = T extends string ? "Yes, it's a string!" : "No, it's not a string.";

  type Result3 = IsString2<string>;  // "Yes, it's a string!"
  type Result4 = IsString2<number>;  // "No, it's not a string."
```

### Checking Subtypes

Conditional types also work with inheritance. For example, if we want to check whether a type extends another type:

```ts 
  interface Animal {
    live(): void;
  }
  
  interface Dog extends Animal {
    woof(): void;
  }

  type Example = Dog extends Animal ? number : string;  // Example is number
```

Here, `Dog` extends `Animal`, so the condition is true, and `Example` is assigned the type `number`.

### Handling Union Types with Conditional Types

When you're working with union types, conditional types can check each member of the union separately. Here's an example that removes null and undefined from a type:

```ts
  type NonNullableState<T> = T extends null | undefined ? never : T;

  type Type = NonNullable<string | null | undefined>;  // string
```

Here, we have `string | null | undefined`. So it removes `null` and `undefined`, leaving just `string`.

### Multiple Conditions in Conditional 

You can also chain multiple conditions:

```ts
  type CheckType<T> = T extends string ? "String" : T extends number ? "Number" : "Other";

  type Result = CheckType<string | number>;  // "String" | "Number"
  type Result = CheckType<boolean>; // "Other"
```

### Inferring Types with `infer` Keyword

The `infer` keyword allows you to extract or infer a type from within another type. For example, you can infer the element type of an array:

```ts
  type ArrayElementType<T> = T extends (infer U)[] ? U : T;

  type Item = ArrayElementType<string[]>;  // string
  type NoItem = ArrayElementType<number>;  // number
  type NumberArray = ArrayElementType<number[]>; // number
```

### Conditional Types for Function Return Types

You can use conditional types to extract the return type of a function based on the input types:

```ts
  type ReturnType<T> = T extends (arg: infer U) => infer R ? R : never;

  type MyFunction = (input: string) => number;

  type ResultType = ReturnType<MyFunction>;  // number
```

Here, `ReturnType2` checks if `T` is a function type. If it is, it extracts the argument (`U`) and return type (`R`). We only care about the return type here, so `ResultType` will be `number`.

### Discriminated Unions with Conditional Types

You can also use conditional types to handle discriminated unions—types that have a common property used to distinguish between variants. This allows you to create types based on specific conditions: 

```ts
  type Shape = 
    | { kind: "circle"; radius: number }
    | { kind: "square"; side: number };

  type Area<T> = T extends { kind: "circle"; radius: infer R } ? number 
    : T extends { kind: "square"; side: infer S } ? number 
    : never;

  type CircleArea = Area<Shape>;  // number 
```

Here, we're checking if `T` is a `circle` or `square`, and depending on that, we return the type `number`.

### Distributive Conditional Types

When you apply a conditional type to a union, TypeScript automatically applies the conditional type to each member of the union. For example:

```ts
  type ToArray<T> = T extends any ? T[] : never;

  type StrArrOrNumArr = ToArray<string | number>;  // string[] | number[]
```

Here, `ToArray<string | number>` gets distributed across the union, resulting in `string[] | number[]`.

If you want to avoid this distributive behavior, you can wrap each side of the extends keyword with square brackets:

```ts
  type ToArrayNonDist<Type> = [Type] extends [any] ? Type[] : never;

  type ArrOfStrOrNum = ToArrayNonDist<string | number>;  // ArrOfStrOrNum is now (string | number)[]
```

## Conditional Type Constraints

In TypeScript, conditional type constraints allow you to add extra conditions to generic types. This feature makes your types more flexible and precise, helping you handle complex typing scenarios.

### Basic Conditional Type Constraints

You can use conditional type constraints to control how a type behaves based on certain conditions. Here's a simple example where we constrain a type to either number or string:

```ts
  type NumberOrString<T extends number | string> = T extends number ? 'number' : 'string';

  // This type accepts only number or string values.
  // If the input type is a number, it returns the string 'number'.
  // If it's a string, it returns 'string'.

  type Result1 = NumberOrString<42>;     // 'number'
  type Result2 = NumberOrString<'hello'>;  // 'string'
  type Result3 = NumberOrString<boolean>;  // ❌ Error: Type 'boolean' does not satisfy the constraint 'number | string'
```

### Using `keyof` with Constraints

You can combine conditional types with `keyof` to access properties dynamically and safely. In this case, we create a type to fetch the type of a property from an object:

```ts
  type GetPropertyType<T, K extends keyof T> = T[K];

  interface User {
    id: number;
    name: string;
    email: string;
  }

  type IdType = GetPropertyType<User, 'id'>;  // number
  type NameType = GetPropertyType<User, 'name'>;  // string
  type AgeType = GetPropertyType<User, 'age'>;  // ❌ Error: ''age' is not assignable to parameter of type 'keyof User'
```

### Return Types with Function Constraints

You can also apply constraints to function types to extract their return values. For example, this type extracts the return type of any function:

```ts
  type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;

  // This checks if T is a function type and then infers the return type (R).
  // If T is a function, it returns the return type (R); otherwise, it returns any.

  function greet(name: string): string {
    return `Hello, ${name}!`;
  }

  type GreetReturn = ReturnType<typeof greet>;  // string
```

In this case, `GreetReturn` is `string`, as that is the return type of the `greet` function.

### Safe Property Access for Deeply Nested Objects

Imagine you have a deeply nested object, and you want to safely access its properties. You can create a complex conditional type that handles this for you:

```ts
  type PropType<T, Path extends string> = Path extends keyof T 
    ? T[Path] 
    : Path extends `${infer K}.${infer R}`
      ? K extends keyof T 
        ? PropType<T[K], R> 
        : unknown 
      : unknown;

  // If the Path matches a direct property of T, it returns T[Path].
  // If Path is in the form of a nested key like "a.b.c", it breaks down the string and checks each level, inferring the type at each step.
  // If the path doesn’t exist, it returns unknown.

  interface DeepObject {
    a: {
      b: {
        c: string;
      };
      d: number;
    };
    e: boolean;
  }

  type CType = PropType<DeepObject, 'a.b.c'>;  // string
  type DType = PropType<DeepObject, 'a.d'>;    // number
  type EType = PropType<DeepObject, 'e'>;      // boolean
  type UnknownType = PropType<DeepObject, 'a.b.f'>;  // unknown
```

In this example:

- `CType` is `string`, because `DeepObject.a.b.c` is a string.
- `DType` is `number`, because `DeepObject.a.d` is a number.
- `UnknownType` is `unknown`, because `DeepObject.a.b.f` doesn’t exist.

🌟 Awesome job! You’ve successfully completed your Day 10, and you're well on your way to becoming a great developer. Keep up the momentum! Now, let's keep your mind sharp and your body active with some quick exercises.

## 💻 Day 10: Exercises

### Exercise: Level 1

1. You have an array type, and you want to create a type that checks whether the given type is an array or not. If it is an array, return `"Array"`, otherwise return `"Not an Array"`.

2. Create a conditional type that extracts the first argument type from a function. If the type passed is not a function, return `"Not a function"`.

3. Write a conditional type that checks if a type is a `Promise`. If the type is a `Promise`, return `"Promise"`, otherwise return `"Not a Promise"`.

### Exercise: Level 2

4. You have an object representing user settings:

```ts
  interface Settings {
    theme: {
      color: string;
      darkMode: boolean;
    };
    notifications: {
      email: boolean;
      sms: boolean;
    };
  }
```

- Write a conditional type `ExtractNestedType<T, K>` that extracts the type of a nested property. Use it to extract the type of `email` under `notifications`.

5. Create a conditional type `ConvertToString<T>` that converts each member of a union into a `string`. For example, `ConvertToString<number | boolean>` should result in `"string" | "string"`.

6. You have an interface `Person` with optional properties:

```ts
  interface Person {
    name: string;
    age?: number;
    address?: string;
  }
```

Write a conditional type `IsOptional<T, K>` that checks if a property `K` is optional in an object `T`. Use it to check whether `age` and `name` are optional in `Person`.

### Exercise: Level 3

7. You want to ensure that a certain object has a specific structure: It must contain an `id` property of type `number`, and if there is a `name` property, it must be a string. Create a conditional type `EnforceStructure` and enforcethe following:

- { id: 1, name: "John" }
- { id: 1, name: 123 } (this should result in an error)

8. Write a conditional type `AsyncReturnType<T>` that extracts the return type of an async function (a function returning a `Promise`).

9. Write a conditional type `ValidateShape<T, ExpectedShape>` that checks if the shape of type `T` matches the structure of `ExpectedShape`. For example, you want to ensure that an object has exactly the properties defined in the `ExpectedShape`, and nothing more.

10. Write a conditional type `FlattenArray<T>` that flattens nested arrays into a single-level array. Use FlattenArray to flatten the following:

- number[][][]
- string[][]

🎉 CONGRATULATIONS ! 🎉

[<< Day 9](../Day9_Type_Manipulation/Day9.md) | [Day 11 >>](../Day11_Mapped_Types/Day11.md)