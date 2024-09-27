<div align="center"> 
  <h1>20 Days of Basics TypeScript: Template Literal Types</h1>
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

[<< Day 11](../Day11_Mapped_Types/Day11.md) | [Day 13 >>](../Day13_Classes_1/Day13.md)

<div align="center"> 
  <a class="header-image" target="_blank" href="../Asset/images/Days/Day_12.webp">
    <img alt="Typescript image" src="../Asset/images/Days/Day_12.webp" width="100%" height="600px">
  </a>
</div>

## Table of Contents

- [📔 Day 12](#-day-12)
- [Template Literal Types in TypeScript](#template-literal-types-in-typescript)
  - [Enforcing Specific String Patterns](#enforcing-specific-string-patterns)
  - [Combining Literal Types for Specific Cases](#combining-literal-types-for-specific-cases)
  - [Template Literal Types with Generics](#template-literal-types-with-generics)
  - [Reactivity and Events with Template Literal Types](#reactivity-and-events-with-template-literal-types)
- [Intrinsic String Manipulation Types](#intrinsic-string-manipulation-types)
  - [Intrinsic String Manipulation Utilities](#intrinsic-string-manipulation-utilities)
  - [Dynamic URL Paths](#dynamic-url-paths)
  - [Parsing URL Parameters](#parsing-url-parameters)
  - [Advanced API Route Generation](#advanced-api-route-generation)
  - [Inferring Values from Template Literals](#inferring-values-from-template-literals)
  - [Building Object Types from URL Parameters](#building-object-types-from-url-parameters)
- [💻 Day 12: Exercises](#-day-12-exercises)
  - [Exercise: Level 1](#exercise-level-1)
  - [Exercise: Level 2](#exercise-level-2)
  - [Exercise: Level 3](#exercise-level-3)


# 📔 Day 12

## Template Literal Types in TypeScript

Template Literal Types in TypeScript are a powerful way to control and enforce the patterns of strings in your code. Think of them as similar to JavaScript's template literals but used at the type level. They are especially useful when you need to make sure that strings follow specific rules or patterns, like CSS class names or URLs, while still maintaining flexibility. This is great when need consistency in things like class names, states, or events.

You can create a new string type by combining a literal value (like `"prefix-"`) with a variable part (like `${string}`):

```ts
  type NewType = `prefix-${string}`;
```

### Enforcing Specific String Patterns

For example, let’s say you want to enforce a greeting that starts with "Hello" followed by any other text:

```ts
  type Greeting = `Hello, ${string}!`;
  let greeting: Greeting = "Hello, World!";  // This works
  let invalidGreeting: Greeting = "Hi there!";  // ❌ Error: Type '"Hi there!"' is not assignable to type '`Hello, ${string}!`'.
```

Suppose you want all button CSS classes in your project to start with `"btn-"`. You can enforce this rule using template literal types:

```ts
  type ButtonClass = `btn-${string}`;

  let primaryButton: ButtonClass = "btn-primary";  // valid
  let invalidButton: ButtonClass = "card-primary";  // ❌ Error: 'card-primary' is not assignable to type 'ButtonClass'.
```

By doing this, you're making sure that only class names that start with `"btn-"` are valid.

### Combining Literal Types for Specific Cases

You can also combine different literal types to get even more specific. For example, let's say you want to define a set of allowed button states (like `"primary"`, `"secondary"`, or `"danger"`), and combine them with the `"btn-"` prefix:

```ts
  type ButtonState = "primary" | "secondary" | "danger";
  type ButtonClass = `btn-${ButtonState}`;  // Valid values: 'btn-primary', 'btn-secondary', 'btn-danger'

  let primaryButton2: ButtonClass2 = "btn-primary";  // valid
  let secondaryButton: ButtonClass2 = "btn-secondary";  // valid
  let dangerButton: ButtonClass2 = "btn-danger";  // valid
  let invalidButton: ButtonClass2 = "btn-warning";  // ❌ Error: Type '"btn-warning"' is not assignable to type '"btn-primary" | "btn-secondary" | "btn-danger"'.
```

Now, you're not just controlling that the class starts with `"btn-"`, but also limiting it to only specific states like `"primary"`, `"secondary"`, or `"danger"`.

__More Complex String Patterns Example:__

Imagine you have a CSS class naming convention for colored items where the color comes first and the quantity follows (like `"red_two"`, `"blue_one"`). You can create a type that enforces this pattern:

```ts
  type Colors = "red" | "blue" | "green";
  type Quantity = "one" | "two" | "three";
  type ColoredItem = `${Colors}_${Quantity}`;  // 'red_one', 'blue_two', 'green_three', etc.

  let item: ColoredItem = "red_two";  // valid
  let invalidItem: ColoredItem = "yellow_four";  // ❌ Error: 'yellow' or 'four' are not valid options
```

This gives you precise control over valid combinations.

### Template Literal Types with Generics

Now, what if you want to apply these template literal types to object keys? For instance, you might want to prefix all keys of an object with `"person_"`. You can do this dynamically using generics:

```ts
  type Person = {
    firstName: string;
    lastName: string;
  };

  type PrefixKeys<T> = {
    [K in keyof T as `person_${string & K}`]: T[K];
  };

  type PrefixedPerson = PrefixKeys<Person>;

  // Now 'PrefixedPerson' will have 'person_firstName' and 'person_lastName' as keys
  let Person: PrefixedPerson = {
    person_firstName: "Samson",
    person_lastName: "Nebeolisa"
  };
```

### Reactivity and Events with Template Literal Types

In a real-world application, you might want to trigger specific events when object properties change. You can enforce that certain event names match the pattern of the object keys. Here’s an example of how to do that:

```ts
  type PropEventSource<Type> = {
      on<Key extends keyof Type>(eventName: `${string & Key}Changed`, callback: (newValue: Type[Key]) => void): void;
  };

  declare function makeWatchedObject<Type>(obj: Type): Type & PropEventSource<Type>;

  const person = makeWatchedObject({
      firstName: "John",
      lastName: "Doe",
      age: 30
  });

  person.on("firstNameChanged", newName => {
      console.log(`New name is ${newName.toUpperCase()}`);
  });

  person.on("firstNameModified", () => {});  // ❌ Error: Argument of type '"firstNameModified"' is not assignable to parameter of type '"firstNameChanged" | "lastNameChanged" | "ageChanged"'.
```

Here, the event names must follow the pattern of `"propertyNameChanged"`, where `propertyName` is one of the keys of the object. If you try to listen for an event like `"firstNameModified"`, TypeScript will give you an error because it doesn’t match the required pattern.


## Intrinsic String Manipulation Types

In TypeScript, Intrinsic String Manipulation Types are utility types that help you modify and manipulate string types. They are built directly into the TypeScript compiler for speed and efficiency. These utilities allow you to transform strings into uppercase, lowercase, and more, right at the type level, ensuring your strings follow certain rules even before your code runs.

### Intrinsic String Manipulation Utilities

Here are the built-in string manipulation types you can use:

- `Uppercase<S>`: Converts a string type `S` to uppercase.
- `Lowercase<S>`: Converts a string type `S` to lowercase.
- `Capitalize<S>`: Capitalizes the first letter of the string `S`.
- `Uncapitalize<S>`: Uncapitalizes the first letter of the string `S`.

Let’s see them in action:

```ts
  type Color = "red" | "blue" | "green";

  // Uppercase<StringType>
  type UppercaseColors = Uppercase<Color>;  // "RED" | "BLUE" | "GREEN"

  type LowercaseGreeting = "hello, world";
  type UppercaseGreeting = Uppercase<LowercaseGreeting>; // "HELLO, WORLD"

  // cache keys
  type ASCIICacheKey<Str extends string> = `ID-${Uppercase<Str>}`
  type MainID = ASCIICacheKey<"my_app"> // MainID = "ID-MY_APP"


  // Lowercase<StringType>
  type LowercaseGreeting = Lowercase<"HELLO WORLD">;  // "hello world"

  type LowercaseGreeting = Lowercase<UppercaseGreeting>; // "hello, world"

  // In function
  function normalizeEmail(email: string): Lowercase<string> {
    return email.toLowerCase() as Lowercase<string>;
  }

  const email = normalizeEmail("JohnDoe@Example.COM"); // "johndoe@example.com"

  // With object type 
  type ApiResponse = {
    UserName: string;
    EmailAddress: string;
    phoneNumber: string;
  };

  type NormalizedApiResponse = {
    [K in keyof ApiResponse as Lowercase<K>]: ApiResponse[K];
  };

  
  // Capitalize<StringType>
  type CapitalizedColor = Capitalize<Color>;  // "Red" | "Blue" | "Green"

  type LowercaseTitle = "frontend developer";
  type CapitalizedTitle = Capitalize<LowercaseTitle>; // "Frontend developer"


  // Uncapitalize<StringType>
  type UncapitalizedGreeting = Uncapitalize<"Hello World">;  // "hello World"

  type CapitalizedTitle2 = "Frontend Developer";
  type UncapitalizedTitle = Uncapitalize<CapitalizedTitle>; // "frontend developer"
```

These utilities give you flexibility in handling string transformations right inside TypeScript, ensuring consistent naming across your codebase.

Let's see some complex example of template literal:

### Dynamic URL Paths

When building frontend applications, you often work with dynamic URLs, such as user profiles (`/users/{id}`). Using template literal types, you can ensure your URLs follow the correct structure.

```ts
  type UserId = `${number}`;  // UserId can be any number
  type UserProfileURL = `/users/${UserId}`;

  let validProfile: UserProfileURL = "/users/123";  // This is valid
  let invalidProfile: UserProfileURL = "/profile/123";  // ❌ Error: Type '"/profile/123"' is not assignable to type 'UserProfileURL'.
```

This is helpful when you need to strictly control the format of URLs, preventing typos or errors when constructing dynamic paths.

### Parsing URL Parameters

If you have URLs with dynamic parameters, like `/users/:id/posts/:postId`, you can use template literals to extract those parameters:

```ts
  type ParseRouteParams<Route extends string> = 
    Route extends `${string}:${infer Param}/${infer Rest}`
      ? Param | ParseRouteParams<Rest>
      : Route extends `${string}:${infer Param}`
        ? Param
        : never;

  type UserRouteParams = ParseRouteParams<"users/:id/posts/:postId">;  // "id" | "postId"
```

This type parses the URL structure and extracts the dynamic parts (like id and postId) for easy access.

### Advanced API Route Generation

When working with APIs, you might have different HTTP methods (GET, POST, etc.) and paths. You can use TypeScript’s template literal types to create types that represent valid API routes.

```ts
  type HTTPMethod = "GET" | "POST" | "PUT" | "DELETE";
  type LogMessage = `${HTTPMethod} request sent to ${string}`;

  let log: LogMessage = "GET request sent to /api/users";  // valid
  let log1: LogMessage = "POST request sent to /api/users/123";  // valid
  let invalidLog: LogMessage = "FETCH request sent to /api/users";  // ❌ Error: Type '"FETCH request sent to /api/users"' is not assignable to type 'LogMessage'.
```

You can also dynamically generate API routes for users based on our HTTP methods:

```ts
  type APIRoute<Method extends HTTPMethod, Path extends string> = 
    `${Uppercase<Method>} /${Path}`;

  type UserRoutes = 
    | APIRoute<"GET", "users">
    | APIRoute<"GET", "users/:id">
    | APIRoute<"POST", "users">
    | APIRoute<"PUT", "users/:id">
    | APIRoute<"DELETE", "users/:id">;
    // UserRoutes = "GET /users" | "GET /users/:id" | "POST /users" | "PUT /users/:id" | "DELETE /users/:id"

  const validRoute: UserRoutes = "GET /users/:id";  // valid
  const invalidRoute: UserRoutes = "PATCH /users";  // ❌ Error: PATCH is not a valid method
```

This pattern ensures that all API routes follow a specific format, reducing the chance of errors when making requests.

### Inferring Values from Template Literals

You can also extract certain parts of a string using template literals. For example, if you want to extract the event name from strings like `"onClick"` or `"onHover"`, you can do this with TypeScript’s infer keyword:

```ts
  type EventName = "onClick" | "onHover" | "onFocus";
  type ExtractEvent<T> = T extends `on${infer Event}` ? Event : never;

  type ClickEvent = ExtractEvent<"onClick">;  // "Click"
  type HoverEvent = ExtractEvent<"onHover">;  // "Hover"
  type FocusEvent = ExtractEvent<"onFocus">;  // "Focus"
```

This gives you the ability to dynamically extract parts of strings for more complex type operations.

### Building Object Types from URL Parameters

You can take it a step further and use the parsed parameters to build an object type. This is useful when you want to pass route parameters to functions:

```ts
  type RouteParams<Route extends string> = {
      [K in ParseRouteParams<Route>]: string;
  };

  type UserPostRouteParams = RouteParams<"users/:userId/posts/:postId">;
  // UserPostRouteParams = { userId: string; postId: string; }

  function fetchUserPost(params: UserPostRouteParams) {
      console.log(`Fetching post ${params.postId} for user ${params.userId}`);
  }

  fetchUserPost({ userId: "123", postId: "456" });  // valid
  fetchUserPost({ userId: "123" });  // ❌ Error: Property 'postId' is missing
```

🌟 Awesome job! You’ve successfully completed your Day 12, and you're well on your way to becoming a great developer. Keep up the momentum! Now, let's keep your mind sharp and your body active with some quick exercises.

## 💻 Day 12: Exercises

### Exercise: Level 1

1. Create a type `PrefixedUsername` that ensures all usernames start with `"user_"` and follow it with any string.

2. Create a type `ColorTheme` that only accepts `"light"` or `"dark"` and a type `ThemeClassName` that combines those values with a `"theme-"` prefix.

3. Write a function `greetUser` that accepts a user's name as an argument and returns a string greeting them with `"Hello, [name]!"`. However, the function should only accept names that start with `"user_"`. Define the function `greetUser` and try calling it with `"user_mike"` and `"admin_mike"`. What happens in both cases?

### Exercise: Level 2

4. You're creating an application that has specific URL patterns. Define a type `ProductURL` that only allows URLs that follow the pattern `/products/{id}` where `{id}` is a number.

5. TypeScript provides utility types to manipulate strings like `Uppercase`, `Lowercase`, and `Capitalize`.

- Create a type `UserRole` with values `"admin"`, `"user"`, and `"guest"`.
- Use the `Capitalize` utility type to create a new type `CapitalizedUserRole` where all roles are capitalized (e.g., `"Admin"`, `"User"`, `"Guest"`).

6. Define an overloaded function `logEvent` that:

- When passed `"click"`, logs `"Click event detected"`.
- When passed `"hover"`, logs `"Hover event detected"`.
- When passed `"scroll"`, logs `"Scroll event detected"`.
- Any other string should result in an error.


### Exercise: Level 3

7. You have an object type User:

```ts
  type User = {
    id: number;
    name: string;
    email: string;
  };
```

Create a type `UserKeysAsCSS` that converts the keys of the `User` object into CSS-friendly strings, like `"user-id"`, `"user-name"`, and `"user-email"`.

8. You are building a routing system that involves dynamic parameters in URLs, like `/posts/:postId/comments/:commentId`. Write a TypeScript type `ExtractParams` that can extract the dynamic parameters (`postId` and `commentId`) from a URL string. Test this type on the URL string `"/posts/:postId/comments/:commentId"`.

9. Write a function `getStatusMessage` that:

- Accepts a status code (like `200`, `404`, or `500`).
- Returns a message string depending on the code:
  - `"Success"` for `200`,
  - `"Not Found"` for `404`,
  - `"Server Error"` for `500`.

10. Create a type-safe function `createClassName` that:

- Accepts a prefix (like `"btn"` or `"card"`) and a state (like `"primary"`, `"secondary"`, or `"disabled"`).
- Returns a combined class name like `"btn-primary"` or `"card-disabled"`.

🎉 CONGRATULATIONS ! 🎉

[<< Day 11](../Day11_Mapped_Types/Day11.md) | [Day 13 >>](../Day13_Classes_1/Day13.md)