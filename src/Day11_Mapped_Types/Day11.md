<div align="center"> 
  <h1>20 Days of Basics TypeScript: Mapped Types</h1>
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

[<< Day 10](../Day10_Conditional_Types/Day10.md) | [Day 12 >>](../Day12_Template_Literal_Types/Day12.md)

<div align="center"> 
  <a class="header-image" target="_blank" href="../Asset/images/Days/Day_11.webp">
    <img alt="Typescript image" src="../Asset/images/Days/Day_11.webp" width="100%" height="600px">
  </a>
</div>

## Table of Contents

- [📔 Day 11](#-day-11)
- [TypeScript Mapped Types](#typescript-mapped-types)
  - [Changing the Type of All Properties](#changing-the-type-of-all-properties)
  - [Making Properties Nullable](#making-paroperties-nullable)
  - [Mapping Modifiers](#mapping-modifiers)
  - [Making All Properties Read-Only](#making-all-properties-read-only)
  - [Making All Properties Optional](#making-all-properties-optional)
  - [Adding and Removing Modifiers](#adding-and-removing-modifiers)
- [Mapping with Conditional Types](#mapping-with-conditional-types)
  - [Extracting Only Function Properties](#extracting-only-function-properties)
  - [Remapping Keys Using `as` and Template Literal Types](#remapping-keys-using-as-and-template-literal-types)
  - [Filtering Properties by Type](#filtering-properties-by-type)
  - [Removing Specific Properties](#removing-specific-properties)
  - [Making All Properties Deeply Read-Only](#making-all-properties-deeply-read-only)
- [💻 Day 11: Exercises](#-day-11-exercises)
  - [Exercise: Level 1](#exercise-level-1)
  - [Exercise: Level 2](#exercise-level-2)
  - [Exercise: Level 3](#exercise-level-3)


# 📔 Day 11

## TypeScript Mapped Types

In TypeScript, mapped types allow you to take an existing type and create a new one by "mapping over" its properties and applying transformations. This feature is useful for tasks like making properties optional, readonly, or even modifying their types.

A basic mapped type looks like this:

```ts
  type MappedType<T> = { [Key in keyof T]: "some transformation" };
```

-  `T`: The original type you're mapping over.
-  `keyof T`: This grabs all the keys (property names) of the type T.
-  `Key`: A placeholder that represents each property in T.
-  `"some transformation"`: You define how each property will be transformed in the new type.

### Changing the Type of All Properties

You can use mapped types to change all the properties of an object to a different type. For example, let's turn all properties of the `User` object into `string`:

```ts
  type User = {
    name: string;
    age: number;
    email: string;
    height: number;
  };

  type AllString<T> = { [Key in keyof T]: string };
  type StringUser = AllString<User>; //  StringUser = {name: string; age: string; email: string; height: string;}
```

### Making Properties Nullable

You can modify properties to make them nullable using mapped types. This means each property can either be its original type or `null`.

```ts
  type Nullable<T> = { [K in keyof T]: T[K] | null };

  type NullablePerson = Nullable<User>;

  // Now, when you create an object like this:
  const jane: NullablePerson = { name: "Jane", age: null, email: "samson@example.com", height: 6.7 };
  // NullablePerson = {name: string | null; age: number | null; email: string | null; height: number | null;}
```

## Mapping Modifiers

When using mapped types, you can also control if properties are `readonly` (cannot be changed) or `optional` (`?`) (can be left out) by using modifiers. You can add or remove these modifiers by using `+` or `-`. If no sign is provided, TypeScript assumes a `+`.

### Making All Properties Read-Only

When we make an object’s properties read-only, it means they can be assigned a value once but cannot be changed afterward. You can do this easily with mapped types in TypeScript.

```ts 
  type User = {
    name: string;
    age: number;
    email: string;
    height: number;
  };

  type ReadonlyPerson = {
    readonly [K in keyof User]: User[K];
  };

  const john: ReadonlyPerson = { name: "John", age: 30, email: "samson@example.com", height: 6.7 };
  // {readonly name: string; readonly age: number; readonly email: string; readonly height: number;}

  // now trying to mutate this item will result to an error.
  john.name = "Jane"; // ❌ Error: Cannot assign to 'name' because it is a read-only property.
```

This is great when you want to ensure that certain data (like user info) is locked and can't be changed accidentally after it's set.

### Making All Properties Optional

Here’s how you can make every property optional using a mapped type:

```ts
  type User = {
    name: string;
    age: number;
    email: string;
    height: number;
  };

  type Optional<T> = {
    [Key in keyof T]?: T[Key];
  };

  type OptionalUser = Optional<User>;
  // OptionalUser = { name?: string | undefined; age?: number | undefined; email?: string | undefined; height?: number | undefined;}

  const userWithMissingProps: OptionalUser = { name: "Samson" }; // This is perfectly valid
```

### Adding and Removing Modifiers

What if you have a type where all properties are read-only or optional, but you want to undo that for certain use cases? TypeScript gives you the ability to add or remove these modifiers by using the `+` and `-` signs. Let’s look at some examples.

```ts
  // remove readonly from our above "ReadonlyPerson"
  type Mutable<T> = {
    -readonly [K in keyof T]: T[K];
  };

  type MutablePerson = Mutable<ReadonlyPerson>;

  const bob: MutablePerson = { name: "Bob", age: 25, email: "samson@example.com", height: 6.7 };

  // This is now allowed!
  bob.name = "Robert"; 

  // remove optional from our above "OptionalUser"
  type removeOptional<T> = {
    [Key in keyof T]-?: T[Key];
  };

  type RequiredUser = removeOptional<OptionalUser>;

  // Now, all fields are mandatory again:
  const validUser: RequiredUser = { name: "Alice", age: 29, email: "alice@example.com", height: 5.4 }; // Valid

  const invalidUser: RequiredUser = { name: "Alice" }; // ❌ Error: Property 'age', 'email', and 'height' are missing.
```

## Mapping with Conditional Types

In TypeScript, mapped types give you the power to transform properties of an object, while conditional types allow you to apply different transformations based on conditions. Combining these two concepts enables even more advanced type manipulation. 

basic example:

```ts
  type Booleanize<T> = {
    [Key in keyof T]: T[Key] extends string ? boolean : T[Key];
  };
```

This `Booleanize` type checks if each property of `T` is a `string`. If it is, it converts the property type to `boolean`; otherwise, it keeps the original type. Let’s try this with our `User` Object type above.

```ts
  type BooleanizedUser = Booleanize<User>;
  // { name: boolean; age: number; email: boolean; height: number;}
```

### Extracting Only Function Properties

To extract the function properties from a type. 

```ts
  type FunctionProperties<T> = {
    [K in keyof T]: T[K] extends Function ? K : never;
  }[keyof T];
```

This type checks each property of `T`. If the property is a function, it keeps the property name; otherwise, it replaces it with `never` (which means "nothing"). The `[keyof T]` at the end returns only the keys that were not turned into `never`.

For example:

```ts
  interface Calculator {
    add: (a: number, b: number) => number;
    subtract: (a: number, b: number) => number;
    value: number;
  }

  type CalculatorFunctions = FunctionProperties<Calculator>;
  // "add" | "subtract"
```

### Remapping Keys Using `as` and Template Literal Types

You can also remap the keys of a type while using a mapped type. Let’s say you want to generate getter methods for every property in a type. You can do this by combining mapped types with template literal types:

```ts
  type Getters<Type> = {
    [Property in keyof Type as `get${Capitalize<string & Property>}`]: () => Type[Property]
  };
```

This creates a new type where every property is transformed into a getter method. For example, applying this the `User` Object type:

```ts
  type PersonGetters = Getters<User>;
  // {
  //   getName: () => string;
  //   getAge: () => number;
  //   getEmail: () => string;
  //   getHeight: () => number;
  // }
```

### Filtering Properties by Type

Sometimes you want to pick only certain properties based on their type. For instance, you might want all properties that are `string`. You can do this with mapped types and conditional types:

```ts
  type PickByType<T, U> = {
    [P in keyof T as T[P] extends U ? P : never]: T[P];
  };

  type StringProps = PickByType<User, string>;
  // { name: string; email: string; }
```

### Removing Specific Properties

You can also exclude specific properties from a type using the `Exclude` utility type. Let’s say we want to remove the `name` property from our `User` Object type:

```ts
  type RemoveNameField<Type> = {
    [Property in keyof Type as Exclude<Property, "name">]: Type[Property]
  };

  type noNameField = RemoveNameField<User>;
  // { age: number; email: string; height: number; }
```

> Don't worry if some of this doesn't fully click just yet — we'll be diving into utility types in future lessons. So if things feel a bit complex right now, that's totally normal. As we go deeper, it will all start to make sense!

### Making All Properties Deeply Read-Only

You can use mapped types and conditional types together to make nested objects read-only. Normally, making an object read-only affects only the top-level properties. But what if you want all nested properties to also be read-only? Here’s how to do it:

```ts
  type DeepReadonly<T> = {
    readonly [K in keyof T]: T[K] extends object
      ? T[K] extends Function
        ? T[K] // Don't make functions readonly
        : DeepReadonly<T[K]>
      : T[K];
  };

  interface NestedObject {
    a: {
      b: {
        c: string;
      };
      d: number[];
    };
    e: () => void;
  }

  // All properties and nested properties are readonly, except for the function
  type DeepReadonlyNested = DeepReadonly<NestedObject>;
```

🌟 Awesome job! You’ve successfully completed your Day 11, and you're well on your way to becoming a great developer. Keep up the momentum! Now, let's keep your mind sharp and your body active with some quick exercises.

## 💻 Day 11: Exercises

### Exercise: Level 1

1. Create a `Product` type and mapped type `AllOptional<T>` that makes all properties of the `Product` type optional. Use it to create an optional `ProductOptional` type.

2. You are given the `Game` type:

```ts
  type Game = {
    name: string;
    players: number;
    score: string;
  };
```

Write a mapped type `AllNumbers<T>` that converts all properties of `Game` into numbers, regardless of their original types. Create a new type `NumberGame` using this mapped type.

3. You have a Product type:

```ts
  type Product = {
    id: number;
    name: string;
    price: number;
    inStock: boolean;
  };
```

Write a mapped type `MakeOptional<T, K extends keyof T>` that makes only the properties specified by `K` optional. Use it to make `price` and `inStock` optional in a new type `PartialProduct`.

### Exercise: Level 2

4. Consider the following `Service` type:

```ts
  type Service = {
    start: () => void;
    stop: () => void;
    status: string;
  };  
```

Write a mapped type `NonFunctionProperties<T>` that filters out any properties that are functions, leaving only non-function properties. Create a new type `ServiceData` that filters out the function properties from `Service`.

5. You are working with the following type that represents an array of items:

```ts
  type ItemArray = {
    items: string[];
  };
```

Write a mapped type `StringToBoolean<T>` that converts each element in the `items` array from `string` to `boolean`. Create a new type `BooleanItemArray`.

6. Given the following `Vehicle` type:

```ts
  type Vehicle = {
    speed: number;
    fuel: number;
    type: string;
  };
```

Write a mapped type `RenameNumberProps<T>` that renames all properties with type `number` by appending `"Value"` to the property name. Use this mapped type to create a new type `RenamedVehicle`, where `speed` becomes `speedValue` and `fuel` becomes fuelValue.

7. You are given the `UserAction` type:

```ts
  type UserAction = {
    login: () => void;
    logout: () => void;
    isLoggedIn: boolean;
  };
```

Create a mapped type `ActionGetters<T>` that transforms each function in `UserAction` into a getter function (e.g., `getLogin: () => void`). Create a new type `UserActionGetters`.

### Exercise: Level 3

8. You are working with a `Profile` type:

```ts
  type Profile = {
    id: number;
    name: string;
    isActive: boolean;
    lastLogin: Date;
  };
```

Write a mapped type `RemovePropertiesByType<T, U>` that removes all properties of type U. Use it to remove all `boolean` properties from `Profile` and create a new type `NoBooleanProfile`.

9. Given the following `Organization` type:

```ts
  type Organization = {
    name: string;
    departments: {
      name: string;
      employees: {
        name: string;
        salary: number;
      }[];
    }[];
  };
```

Write a mapped type `DeepReadonly<T>` that recursively makes all properties in the `Organization` type readonly. Use this to create a new type `ReadonlyOrganization`.

10. You have a `Form` type representing fields of a form:

```ts
  type Form = {
    firstName: string;
    lastName: string;
    age: number;
  };
```

Write a mapped type `ToFieldNames<T>` that transforms each property key into a form field name by prefixing `"form_"` and capitalizing the first letter of the property name. Use this to create a new type `FormFields`, which looks like:

```ts
  // {
  //   form_FirstName: string;
  //   form_LastName: string;
  //   form_Age: number;
  // }
```

11. You are given a type Company with nested arrays:

```ts 
  type Company = {
    name: string;
    departments: {
      name: string;
      employees: {
        name: string;
        salary: number;
      }[];
    }[];
  };
```

Create a mapped type `DeepNullable<T>` that makes all properties of `Company` and its nested properties nullable, including properties inside arrays. Use it to create a `NullableCompany` type.

12. Given the following `Music` type:

```ts
  type Music = {
    genre: string;
    artist: string;
    year: number;
  };
```

Create a mapped type `MapKeysToGetter<T>` that transforms all keys into getter functions (e.g., `getGenre`, `getArtist`, `getYear`). Each getter should return the corresponding value's type. Use it to create a new type `MusicGetters`.


🎉 CONGRATULATIONS ! 🎉

[<< Day 10](../Day10_Conditional_Types/Day10.md) | [Day 12 >>](../Day12_Template_Literal_Types/Day12.md)