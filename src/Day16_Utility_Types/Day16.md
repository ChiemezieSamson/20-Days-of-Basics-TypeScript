<div align="center"> 
  <h1>20 Days of Basics TypeScript: Utility Types</h1>
</div>

<div align="center"> 

<!-- Social links -->
[![Discord](https://img.shields.io/badge/Discord-%237289DA.svg?logo=discord&logoColor=white)](htttps://discord.gg/Samson#0273) [![Facebook](https://img.shields.io/badge/Facebook-%231877F2.svg?logo=Facebook&logoColor=white)](https://www.facebook.com/chiemezie.nebeolisa/) [![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?logo=Instagram&logoColor=white)](https://www.instagram.com/samson_nebeolisa/) [![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/chiemezie-samson-nebeolisa-32897310b/) [![Stack Overflow](https://img.shields.io/badge/-Stackoverflow-FE7A16?logo=stack-overflow&logoColor=white)](https://stackoverflow.com/users/20653301/nebeolisa-chiemezie-samson) [![Twitter](https://img.shields.io/badge/Twitter-%231DA1F2.svg?logo=Twitter&logoColor=white)](https://twitter.com/SamsonChiemezie) [![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?logo=YouTube&logoColor=white)](https://myaccount.google.com/u/0/?utm_source=YouTubeWeb&tab=rk&utm_medium=act&tab=rk&hl=en) 

<!-- Portfolio -->
 📰 About Me [Portfolio](https://www.nebe-samson.com/)
 <br/>
  <small>Sep, 2024</small>
</div>

[<< Day 15](../Day15_Modules/Day15.md) | [Day 17 >>](../Day17_Utility_Types/Day17.md)

<div align="center"> 
  <a class="header-image" target="_blank" href="../Asset/images/Days/Day_16.webp">
    <img alt="Typescript image" src="../Asset/images/Days/Day_16.webp" width="100%" height="600px">
  </a>
</div>

## Table of Contents

- [📔 Day 16](#-day-16)
- [Utility Types in TypeScript](#utility-types-in-typescript)
  - [`Partial<Type>`](#partialtype)
  - [`Required<Type>`](#requiredtype)
  - [`Readonly<Type>`](#readonlytype)
  - [`Pick<Type, Keys>`](#picktype-keys)
  - [`Omit<Type, Keys>`](#omittype-keys)
    - [Use Case Example](#use-case-example)
  - [`Record<Keys, Type>`](#recordkeys-type)
  - [`Exclude<Type, ExcludedUnion>`](#excludetype-excludedUnion)
  - [`Extract<Type, Union>`](#extracttype-union)
  - [`NonNullable<Type>`](#nonnullabletype)
  - [`ReturnType<Type>`](#returntypetype)
  - [`Parameters<Type>`](#parameterstype)
  - [`Awaited<Type>`](#awaitedtype)
    - [Why Use `Awaited<Type>`?](#why-use-awaitedtype)
    - [Using `Awaited<Type>` with Async Functions](#using-awaitedtype-with-async-functions)
    - [Handling Nested Promises in Async Functions](#handling-nested-promises-in-async-functions)
- [💻 Day 16: Exercises](#-day-16-exercises)
  - [Exercise: Level 1](#exercise-level-1)
  - [Exercise: Level 2](#exercise-level-2)
  - [Exercise: Level 3](#exercise-level-3)


# 📔 Day 16

##  Utility Types in TypeScript 

Utility types in TypeScript are a set of built-in types that help you manipulate and create new types from existing ones. They make it easier to work with complex data structures and ensure your code is reusable, maintainable, and type-safe. Let's explore some of the most commonly used utility types with simple examples.

###  `Partial<Type>`

The `Partial` utility type allows you to create a new type where all properties of an existing type become optional. This is especially useful when you need to update or modify just part of an object, rather than all its properties.

```ts
  interface User {
    id: number;
    name: string;
    email: string;
  }

  function updateUser(id: number, userUpdate: Partial<User>) {
    // You can update just a few fields without providing the entire object
    console.log(`Updating user ${id}`, userUpdate);
  }

  updateUser(1, { name: 'Samson' });  // Only updates the name
  updateUser(2, { email: 'samson@example.com' });  // Only updates the email
```

Here, `userUpdate` can include any combination of the `User` properties (or none at all), since `Partial<User> `makes all properties optional.

### `Required<Type>`

The `Required` utility does the opposite of `Partial`. It makes all optional properties of a type mandatory. This is useful when you need to ensure that all fields are filled out.

```ts
  interface Config {
    host?: string;
    port?: number;
    protocol?: 'http' | 'https';
  }

  const getFullConfig = (config: Config): Required<Config> => {
    return {
      host: config.host || 'localhost',
      port: config.port || 8080,
      protocol: config.protocol || 'http'
    };
  };

  const partialConfig: Config = { host: 'example.com' };
  const fullConfig = getFullConfig(partialConfig);
  console.log(fullConfig);
  // Output: { host: "example.com", port: 8080, protocol: "http" }
```

In this case, `getFullConfig` ensures that `host`, `port`, and `protocol` are always defined, even if the initial configuration is incomplete.

### `Readonly<Type>`

The `Readonly` utility type turns all properties of an object into read-only properties, meaning they cannot be changed after the object is created.

```ts
  interface User {
    id: number;
    name: string;
    email: string;
  }
  
  const myProfile: Readonly<User> = {
    id: 2,
    name: "Samson",
    email: "Samson@example.com"
  }

  myProfile.name = "John"; // ❌ Error: Cannot assign to 'name' because it is a read-only property.
```

Once `myProfile` is defined, its properties cannot be reassigned because it's marked as `Readonly<User>`.

### `Pick<Type, Keys>`

The `Pick` utility type allows you to create a new type by picking a specific set of properties from an existing type. This is useful when you only need a subset of the original type.

```ts
  interface User {
    id: number;
    name: string;
    email: string;
  }

  type UserOverview = Pick<User, 'id' | 'email'>;

  const myView: UserOverview = {
    id: 2,
    email: 'Samson@example.com'
    // The 'name' property is excluded
  };
```

Here, `UserOverview` only includes the `id` and `email` properties from the `User` type.

### `Omit<Type, Keys>`

The `Omit` utility type allows you to create a new type by excluding certain properties from an existing type. This is the inverse of `Pick`.

```ts
  interface User {
    id: number;
    name: string;
    email: string;
  }

  type HideEmail = Omit<User, 'email'>;

  const owner: HideEmail = {
    id: 123,
    name: 'John Doe'
    // 'email' is excluded from this type
  };  
```

In this example, `HideEmail` has all the properties of `User` except for `email`.

#### Use Case Example

Let’s break down how TypeScript utility types can be applied in a real-world scenario. Imagine you’re building a blog system where you handle different types of blog-related operations. Utility types make working with types easier and more flexible, allowing you to manage parts of your application in a cleaner, more efficient way.

First, we define a `BlogPost` interface, which represents a blog post object.

```ts
  interface BlogPost {
    id: number;
    title: string;
    content: string;
    authorId: number;
    published: boolean;
  }
```

Now, let’s see how we can use different utility types with this interface.

- `Partial<BlogPost>`: This is useful for updates, where you don’t want to send all fields—only the ones you want to change.

```ts
  function updatePost(id: number, postUpdate: Partial<BlogPost>) {
    // This function can accept an update with just the title, content, or any combination
    console.log(`Updating post ${id}`, postUpdate);
  }

  updatePost(1, { title: 'New Title' }); // Only updates the title
  updatePost(2, { published: false });   // Only updates the published status
```

- `Pick<Type, Keys>`: if you want to display only the `title` and `content` of a blog post, but don’t need the other fields.

```ts
  type BlogPostPreview = Pick<BlogPost, 'title' | 'content'>;

  const preview: BlogPostPreview = {
    title: 'Understanding TypeScript Utility Types',
    content: 'In this post, we will explore...'
  };
```

- `Omit<Type, Keys>`: For instance, when displaying a blog post, you might want to exclude sensitive information like the `authorId`.

```ts
  type BlogPostForDisplay = Omit<BlogPost, 'authorId'>;

  const displayPost: BlogPostForDisplay = {
    id: 1,
    title: 'Learning TypeScript',
    content: 'This is a blog post about TypeScript utility types.',
    published: true
    // 'authorId' is excluded
  };
```

### `Record<Keys, Type>`

The `Record` utility type allows you to create an object type that maps a set of properties (keys) to a specific type for the values. This is especially useful when you need to ensure that an object has a consistent structure.

```ts
  type Status = 'active' | 'inactive' | 'suspended';

  interface UserStatus {
    name: string;
    status: Status;
  }

  const userStatusMap: Record<number, UserStatus> = {
    1: { name: 'Samson', status: 'active' },
    2: { name: 'Peter', status: 'inactive' },
    3: { name: 'Charlie', status: 'suspended' }
  };
```

Here, `Record<number, UserStatus>` means that the keys in `userStatusMap` are numbers, and the values follow the structure of `UserStatus`.

let's see another example:

```ts
  type Weekday = 'Mon' | 'Tue' | 'Wed' | 'Thu' | 'Fri';

  type DailySchedule = Record<Weekday, string>;

  const mySchedule: DailySchedule = {
    Mon: "TypeScript Study",
    Tue: "React Project",
    Wed: "Team Meeting",
    Thu: "Code Review",
    Fri: "Open Source Contribution"
  };
```

In this example, `Record<Weekday, string>` ensures that each day of the week has a corresponding task, and the keys are restricted to the weekdays we defined.

### `Exclude<Type, ExcludedUnion>`

`Exclude` allows you to create a new type by excluding certain types from an existing union type. This is useful when you want to refine a type and remove certain values.

```ts
  type Roles = 'admin' | 'editor' | 'viewer';

  type BasicRoles = Exclude<Roles, 'admin'>;

  const role1: BasicRoles = 'editor'; // Valid
  const role2: BasicRoles = 'viewer'; // Valid
  const role3: BasicRoles = 'admin'; // ❌ Error: Type '"admin"' is not assignable to type 'BasicRoles'.
```

In this case, `BasicRoles` excludes `'admin'` from the `Roles` union, so only `'editor'` and `'viewer'` are allowed.

let's see another example with `Exclude`:

```ts
  type AllColors = 'red' | 'green' | 'blue' | 'yellow' | 'purple';

  type PrimaryColors = 'red' | 'green' | 'blue';

  type SecondaryColors = Exclude<AllColors, PrimaryColors>;
  // SecondaryColors is now 'yellow' | 'purple'

  const secondaryColor: SecondaryColors = 'yellow'; // Valid
  const invalidColor: SecondaryColors = 'red'; // ❌ Error: 'red' is excluded
```

### `Extract<Type, Union>`

`Extract` works the opposite way to `Exclude`. It extracts specific types from a union type. This is helpful when you only want to work with certain values from a larger set.

```ts
  type UserActions = 'login' | 'logout' | 'signup';

  type LoginAction = Extract<UserActions, 'login'>;

  const action: LoginAction = 'login'; // Only 'login' is allowed
```

### `NonNullable<Type>`

`NonNullable` removes `null` and `undefined` from a type, ensuring that your values cannot be `null` or `undefined`. This is useful when you want to make sure your type is always valid and defined.

```ts
type StringOrNull = string | null | undefined;

type NonNullableString = NonNullable<StringOrNull>;

const validString: NonNullableString = 'Hello'; // Valid
const invalidValue: NonNullableString = null; // ❌ Error: Type 'null' is not assignable to type 'NonNullableString'.
```

In this example, `NonNullable<StringOrNull>` removes `null` and `undefined`, so NonNullableString only allows non-nullable `string` values.

### `ReturnType<Type>`

`ReturnType` allows you to infer the return type of a function, which can be useful when you want to ensure consistency across your code without manually defining the return type.

```ts
  function getUserInfo() {
    return {
      id: 1,
      name: 'Samson',
      age: 30
    };
  }

  type UserInfo = ReturnType<typeof getUserInfo>;

  const guest: UserInfo = {
    id: 1,
    name: 'Samson',
    age: 30
  };
```

### `Parameters<Type>`

The `Parameters` utility type extracts the parameter types of a function as a tuple. This is useful when you want to reuse or manipulate the types of a function’s parameters.

```ts
  function createNewAssign(name: string, age: number): string {
    return `Hello, ${name}. You are ${age} years old.`;
  }

  // Extract parameter types of 'createNewAssign'
  type GreetParams = Parameters<typeof createNewAssign>;

  // GreetParams is now [string, number]
  const params: GreetParams = ["Samson", 30];

  // Use the extracted params in the function
  createNewAssign(...params); // Output: Hello, Samson. You are 30 years old.


  // You can also use "Parameters" with arrow functions.
  const sumNum = (a: number, b: number): number => a + b;

  // Extract parameter types
  type SumParams = Parameters<typeof sumNum>; // [number, number]

  const sumArgs: SumParams = [5, 10]; // Valid argument types

  sumNum(...sumArgs); // Output: 15
```

This is helpful when you want to reuse the parameter types of a function in multiple places.

### `Awaited<Type>`

When you're working with asynchronous code in TypeScript, you often use `Promises` and `async/await`. But sometimes it’s tricky to figure out the actual type of a value returned from a `Promise`, especially when the `Promise` is nested or combined with other types. This is where the `Awaited<Type>` utility type comes in!

`Awaited<Type>` helps you extract the final resolved value of a `Promise`, whether it's a simple `Promise`, a nested `Promise`, or even a combination of types (like unions). It's like simulating the behavior of `await` in an `async` function.

#### Why Use `Awaited<Type>`?

- `Awaited<Type>` helps you get the resolved type of a `Promise`. If it's nested (like `Promise<Promise<Type>>`), it unwraps all the layers and gives you the final type.

- It can also deal with union types, like `string | Promise<number>`, and return the correct combined type.

Let's explore this with some easy-to-understand examples.

```ts
  // Basic usage with Promise
  type Z = Awaited<Promise<string>>; // Z is string

  // Handling nested Promises
  type B = Awaited<Promise<Promise<number>>>; // B is number
  // Awaited<Type> will unwrap all layers and give you the final resolved type.

  // Working with Union Types
  type C = Awaited<string | Promise<number>>; // C is string | number
```

#### Using `Awaited<Type>` with Async Functions

In real-world scenarios, you'll often work with async functions that return `Promises`. `Awaited<Type>` can be used to extract the resolved type of a promise returned by an async function.

```ts
  async function fetchData(): Promise<string> {
    return "Data fetched";
  }

  // Extracting the resolved type
  type FetchedDataType = Awaited<ReturnType<typeof fetchData>>;

  // FetchedDataType is now 'string'
  const data: FetchedDataType = "Data fetched";
```

#### Handling Nested Promises in Async Functions

You might also come across nested promises in your async functions. `Awaited` is perfect for unwrapping these promises, so you don’t have to worry about how many layers deep the promise goes.

```ts
  async function getNestedPromise(): Promise<Promise<number>> {
    return Promise.resolve(42);
  }

  // Extracting the resolved type of the nested promise
  type NestedPromiseType = Awaited<ReturnType<typeof getNestedPromise>>;

  // NestedPromiseType is now 'number'
  const result: NestedPromiseType = 42;
```

Even though `getNestedPromise` returns `Promise<Promise<number>>`, `Awaited` continues unwrapping the promise until it reaches the final type, which is `number`.

🌟 Awesome job! You’ve successfully completed your Day 16, and you're well on your way to becoming a great developer. Keep up the momentum! Now, let's keep your mind sharp and your body active with some quick exercises.

## 💻 Day 16: Exercises

### Exercise: Level 1

1. You have an interface `Product` with the properties `id`, `name`, and `price`. Use the `Partial` utility type to create a function `updateProduct` that can accept partial updates to a product without needing all the fields.

2. You have an interface `Car` with the properties `make`, `model`, `year`, and `price`. Use the `Pick` utility type to create a new type called `CarInfo` that only contains the `make` and `model` fields.

3. You have an interface `UserProfile` with properties `id`, `name`, `email`, `password`, and `role`. Create a new type `PublicProfile` using `Omit` that excludes `password` and `role`.

### Exercise: Level 2

4. Create a function `createOrder` that returns an object with `orderId`, `customerName`, and `amount`. Use the `ReturnType` utility type to define a type `Order` that represents the return type of the `createOrder` function.

5. Given a union type `PaymentMethods` with the values `'credit'`, `'paypal'`, and `'cash'`, use `Exclude` to create a new type called `OnlinePayments` that excludes `'cash'`. Then, use `Extract` to create a new type called `PhysicalPayments` that only includes `'cash'`.

6. Create a custom utility type `Writable<T>` that makes all properties of a type writable, overriding any `readonly` properties. Use it on an interface `Book` to ensure its properties are writable.


### Exercise: Level 3

7. You have an async function `fetchUser` that returns a `Promise<{ id: number; name: string; }>` object. Use `Awaited` to define a type `UserData` that represents the resolved type of the promise.

8. Create a function `calculateTotal` that takes two arguments: `quantity` (number) and `price` (number). Use the `Parameters` utility type to extract the parameter types of `calculateTotal` and use it in another function called `applyDiscount`.

9. Create a generic function `mergeObjects<T, U>` that takes two objects of types `T` and `U` and merges them into a new object. Ensure that `T` and `U` can only be objects.

10. You have a type `UserInfo` that can either be `string`, `null`, or `undefined`. Use the `NonNullable` utility type to create a new type `ValidUserInfo` that excludes `null` and `undefined`.


🎉 CONGRATULATIONS ! 🎉

[<< Day 15](../Day15_Modules/Day15.md) | [Day 17 >>](../Day17_Utility_Types/Day17.md)
