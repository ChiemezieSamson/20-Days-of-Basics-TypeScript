<div align="center"> 
  <h1>20 Days of Basics TypeScript: Enums</h1>
</div>

<div align="center"> 

<!-- Social links -->
[![Discord](https://img.shields.io/badge/Discord-%237289DA.svg?logo=discord&logoColor=white)](htttps://discord.gg/Samson#0273) [![Facebook](https://img.shields.io/badge/Facebook-%231877F2.svg?logo=Facebook&logoColor=white)](https://www.facebook.com/chiemezie.nebeolisa/) [![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?logo=Instagram&logoColor=white)](https://www.instagram.com/samson_nebeolisa/) [![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/chiemezie-samson-nebeolisa-32897310b/) [![Stack Overflow](https://img.shields.io/badge/-Stackoverflow-FE7A16?logo=stack-overflow&logoColor=white)](https://stackoverflow.com/users/20653301/nebeolisa-chiemezie-samson) [![Twitter](https://img.shields.io/badge/Twitter-%231DA1F2.svg?logo=Twitter&logoColor=white)](https://twitter.com/SamsonChiemezie) [![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?logo=YouTube&logoColor=white)](https://myaccount.google.com/u/0/?utm_source=YouTubeWeb&tab=rk&utm_medium=act&tab=rk&hl=en) 

<!-- Portfolio -->
 📰 About Me [Portfolio](https://www.nebe-samson.com/)
 <br/>
  <small>Sep, 2024</small>
</div>

[<< Day 16](../Day16_Utility_Types/Day16.md) | [Day 18 >>](../Day18_DOM/Day18.md)

<div align="center"> 
  <a class="header-image" target="_blank" href="../Asset/images/Days/Day_17.webp">
    <img alt="Typescript image" src="../Asset/images/Days/Day_17.webp" width="100%" height="600px">
  </a>
</div>

## Table of Contents

- [📔 Day 17](#-day-17)
- [Enums in TypeScript](#enums-in-typescript)
  - [Types of Enums](#types-of-enums)
- [Numeric Enums](#numeric-enums)
  - [Numeric Enum with Custom Values](#numeric-enum-with-custom-values)
  - [Enums with Specific Values](#enums-with-specific-values)
  - [Using Enums in a Function](#using-enums-in-a-function)
- [String Enums](#string-enums)
  - [Handling HTTP Methods](#handling-http-methods)
- [Heterogeneous Enums](#heterogeneous-enums)
- [Enums with Constant and Computed Members](#enums-with-constant-and-computed-members)
- [Reverse Mapping in Numeric Enums](#reverse-mapping-in-numeric-enums)
- [Const Enums](#const-enums)
- [When to Use Enums vs Objects](#when-to-use-enums-vs-objects)
- [Real-World Use Cases for Enums](#real-world-use-cases-for-enums)
  - [Handling HTTP Status Codes](#handling-http-status-codes)
  - [User Roles in an Application](#user-roles-in-an-application)
- [💻 Day 17: Exercises](#-day-17-exercises)
  - [Exercise: Level 1](#exercise-level-1)
  - [Exercise: Level 2](#exercise-level-2)
  - [Exercise: Level 3](#exercise-level-3)


# 📔 Day 17

##  Enums in TypeScript 

Enums in TypeScript allow you to define a set of named constants that group related values together. These constants make your code more readable, maintainable, and easier to work with by providing descriptive names instead of using arbitrary values like numbers or strings. Enums can be useful when dealing with things like directions, days of the week, HTTP status codes, and more.

Enums are especially helpful when you need to represent a fixed set of related options. Instead of relying on raw numbers or strings, which can be unclear, you can use an enum to give each value a meaningful name. 

### Types of Enums

TypeScript provides two main types of enums:

1. __Numeric Enums__: The most common type, where each member of the enum is associated with a number.
2. __String Enums__: Enums where each member is associated with a string.

Let’s start with Numeric Enums.

## Numeric Enums

In numeric enums, the values assigned to the members start from 0 by default and increment automatically. You can also manually assign values if needed.

```ts
  enum Direction {
    Up,    // 0
    Down,  // 1
    Left,  // 2
    Right  // 3
  }

  let move: Direction = Direction.Up;

  console.log(move);            // Output: 0
  console.log(Direction.Down);   // Output: 1
  console.log(Direction.Left);   // Output: 2
  console.log(Direction.Right);  // Output: 3
```

In this example, the `Direction` enum has four members: `Up`, `Down`, `Left`, and `Right` and by default, `Up` is assigned the value `0`, and each subsequent member gets the next number (`1`, `2`, `3`).

### Numeric Enum with Custom Values

You can also assign a specific starting value to an enum member. When you assign a value to one member, the next members increment from that value.

```ts
  enum Direction {
    Up = 8,   // Starts at 8
    Down,     // 9
    Left,     // 10
    Right     // 11
  }

  let move: Direction = Direction.Up;

  console.log(move);            // Output: 8
  console.log(Direction.Left);  // Output: 10
```

Here, the `Up` member is explicitly set to `8` and the other members automatically increment from `8`, so `Down` becomes `9`, `Left` becomes `10`, and `Right` becomes `11`.

### Enums with Specific Values

Enums are often used to represent specific sets of constants like __HTTP status codes__. You can assign specific values to each member of the enum.

```ts
  enum StatusCode {
    Success = 200,
    BadRequest = 400,
    Unauthorized = 401,
    Forbidden = 403
  }

  console.log(StatusCode.Success);      // Output: 200
  console.log(StatusCode.Unauthorized); // Output: 401
```

### Using Enums in a Function

Enums can be very useful for making your functions more descriptive and easier to maintain. Let’s say you’re building a game, and you want to control the movement of a player. Instead of passing raw numbers or strings, you can use an enum to represent the different directions.

```ts
  enum Movement {
    Forward,
    Backward,
    Left,
    Right
  }

  function movePlayer(direction: Movement) {
    switch (direction) {
      case Movement.Forward:
        console.log('Moving forward');
        break;
      case Movement.Backward:
        console.log('Moving backward');
        break;
      case Movement.Left:
        console.log('Moving left');
        break;
      case Movement.Right:
        console.log('Moving right');
        break;
    }
  }

  movePlayer(Movement.Left); // Output: Moving left
```

## String Enums

String Enums allow you to assign string values to each member of the enum. This is helpful when you need more descriptive or human-readable values rather than relying on auto-incrementing numbers. Every member of a string enum needs to be explicitly assigned a string literal.

```ts
  enum Role {
    Admin = "ADMIN",
    User = "USER",
    Guest = "GUEST"
  }

  let userRole: Role = Role.User;

  console.log(userRole); // Output: "USER"
```

String enums are particularly useful when you want to handle specific roles, actions, or options in your application that benefit from being human-readable. They serialize well and work well in logging, network requests, or role-based access control.

### Handling HTTP Methods

Imagine you’re building a frontend that interacts with a server. You need to specify the type of HTTP request to send (GET, POST, PUT, DELETE, etc.). String enums help you handle these method names consistently.

```ts
  enum HttpMethod {
    GET = "GET",
    POST = "POST",
    PUT = "PUT",
    DELETE = "DELETE"
  }

  function sendRequest(method: HttpMethod, url: string) {
    console.log(`Sending ${method} request to ${url}`);
  }

  sendRequest(HttpMethod.GET, "/api/users"); // Output: Sending GET request to /api/users
```

## Heterogeneous Enums

__Heterogeneous Enums__ are a combination of both numeric and string values. While this is possible in TypeScript, it’s not usually recommended because it can create confusion. However, there are certain use cases where it might be useful—such as representing options that mix different types, like boolean-like values (`Yes/No`, `0/1`).

```ts
  enum MixEnum {
    No = 0,
    Yes = "YES"
  }

  console.log(MixEnum.No);  // Output: 0
  console.log(MixEnum.Yes); // Output: "YES"
```

This type of enum could be useful when you’re working with options that involve different types of values, like when representing boolean-like values (`0` for false, `"YES"` for true).

## Enums with Constant and Computed Members

In TypeScript, enum members can either be __constant__ or __computed__. Constant values are those that TypeScript can fully evaluate at compile time. Computed values, on the other hand, are calculated at runtime.

```ts
  enum FileAccess {
    None,                    // Constant (0)
    Read = 1 << 1,           // Computed (2)
    Write = 1 << 2,          // Computed (4)
    ReadWrite = Read | Write, // Computed by combining Read and Write (6)
    G = "123".length          // Computed string length (3)
  }

  console.log(FileAccess.Read);      // Output: 2
  console.log(FileAccess.ReadWrite); // Output: 6
  console.log(FileAccess.G);         // Output: 3
```

## Reverse Mapping in Numeric Enums

A unique feature of __numeric enums__ in TypeScript is __reverse mapping__. This means that not only can you access the enum value by its name, but you can also get the enum name from its value.

```ts
  enum newColors {
    Red = 1,
    Green,
    Blue
  }

  let colorName: string = newColors[2];
  console.log(colorName);  // Output: "Green"
```

>[!NOTE] Reverse mapping works only for numeric enums, not string enums.

## Const Enums

__Const enums__ are a special kind of enum that gets inlined at compile time, meaning the enum itself is completely removed from the final JavaScript output. This is useful when you want to optimize performance, especially in large applications, by reducing the overhead of enum objects.

```ts
  const enum newDirection {
    Up,
    Down,
    Left,
    Right
  }

  let newMove: newDirection = newDirection.Up;
  console.log(newMove); // Output: 0
```

In the compiled JavaScript, the enum is replaced with the raw values:

```ts
  let newMove = 0; // newDirection.Up is replaced with 0
```

Const enums are great for scenarios where you don't need the extra features of enums (like reverse mapping), and want to improve performance by avoiding additional JavaScript code.

## When to Use Enums vs Objects

In modern TypeScript, you might not always need enums. Sometimes, using an object with `as const` can provide similar functionality, especially for simple use cases:

```ts
  const Directions = {
    Up: "UP",
    Down: "DOWN",
    Left: "LEFT",
    Right: "RIGHT"
  } as const;

  type Direction = typeof Directions[keyof typeof Directions];

  function move(direction: Direction) {
    console.log(`Moving ${direction}`);
  }

  move(Directions.Up); // Output: Moving UP
```

Using as const allows you to define constant objects that work like enums but with less overhead.

## Real-World Use Cases for Enums

### Handling HTTP Status Codes

Enums can simplify how you manage HTTP status codes in your application. Instead of using hardcoded numbers, you can use descriptive enum values.

```ts
  enum HttpStatus {
    OK = 200,
    NotFound = 404,
    InternalServerError = 500
  }

  function handleResponse(status: HttpStatus) {
    switch (status) {
      case HttpStatus.OK:
        console.log("Success!");
        break;
      case HttpStatus.NotFound:
        console.log("Page not found");
        break;
      case HttpStatus.InternalServerError:
        console.log("Server error");
        break;
    }
  }

  handleResponse(HttpStatus.NotFound); // Output: Page not found
```

### User Roles in an Application

You can use string enums to represent different user roles in an application, such as `Admin`, `Editor`, or `Viewer`.

```ts
  enum UserRole {
    Admin = "ADMIN",
    Editor = "EDITOR",
    Viewer = "VIEWER"
  }

  function checkAccess(role: UserRole) {
    if (role === UserRole.Admin) {
      console.log("Full access");
    } else if (role === UserRole.Editor) {
      console.log("Edit access");
    } else {
      console.log("Read-only access");
    }
  }

  checkAccess(UserRole.Editor); // Output: Edit access
```

String enums make it clear what role each user has, and you don’t need to worry about using confusing numbers.

🌟 Awesome job! You’ve successfully completed your Day 17, and you're well on your way to becoming a great developer. Keep up the momentum! Now, let's keep your mind sharp and your body active with some quick exercises.

## 💻 Day 17: Exercises

### Exercise: Level 1

1. Create an enum `Days` for the days of the week, starting with `Sunday` and ending with `Saturday`. Then write a function `isWeekday` that returns `true` if the given day is a weekday (Monday to Friday) and `false` if it’s a weekend (Saturday or Sunday). Also use it to check whether a given day is a weekend.

2. Create a string enum called `OrderStatus` with values `'Pending'`, `'Shipped'`, `'Delivered'`, and `'Cancelled'`. Then write a function `getStatusMessage` that takes an OrderStatus and returns an appropriate message for each status.

```ts
  console.log(getStatusMessage(OrderStatus.Pending)); // "Your order is pending."
  console.log(getStatusMessage(OrderStatus.Shipped)); // "Your order has been shipped."
```

3. Create a numeric enum `FilePermission` where `Read` is `4`, `Write` is `2`, and `Execute` is `1`. Write a function `canRead` that checks if the file permissions allow reading, based on the permission code passed in.

```ts
  console.log(canRead(4)); // true
  console.log(canRead(2)); // false
```

### Exercise: Level 2

4. Create an enum `Priority` where `Low` starts from `1`, `Medium` is `Low + 1`, and `High` is `Medium + 1`. Write a function `describePriority` that returns a string describing the priority level.

```ts
  console.log(describePriority(Priority.Low)); // "This is a low priority."
  console.log(describePriority(Priority.High)); // "This is a high priority."
```

5. You are given an enum `Color` with values `Red = 1`, `Green = 2`, and `Blue = 3`. Write a function `getColorName` that takes a number and returns the name of the color. Use reverse mapping to achieve this.

6. Create an enum `TransportMode` with values `Car`, `Bus`, and `Bicycle`. Write a function `estimateTravelTime` that takes a `TransportMode` and a `distance` in kilometers, then returns the estimated travel time based on the mode. Assume average speeds: Car = 100km/h, Bus = 60km/h, Bicycle = 20km/h.

```ts
  console.log(estimateTravelTime(TransportMode.Car, 200)); // "Estimated travel time by Car: 2 hours"
  console.log(estimateTravelTime(TransportMode.Bicycle, 20)); // "Estimated travel time by Bicycle: 1 hour"
```

### Exercise: Level 3

7. Create an enum `ShapeType` with values `Circle`, `Square`, and `Rectangle`. Write a function `calculateArea` that overloads based on the shape:

- For `Circle`, it accepts the radius.
- For `Square`, it accepts the side length.
- For `Rectangle`, it accepts the width and height.

```ts
  console.log(calculateArea(ShapeType.Circle, 5)); // Output the area of a circle with radius 5
  console.log(calculateArea(ShapeType.Rectangle, 4, 6)); // Output the area of a 
```

8. Create a heterogeneous enum `ErrorType` where `NotFound` has a numeric value `404`, and `ServerError` has a string value `"SERVER_ERROR"`. Write a function `handleError` that takes an `ErrorType` and returns an appropriate message based on the type of error.

```ts
  console.log(handleError(ErrorType.NotFound)); // "Error 404: Not Found"
  console.log(handleError(ErrorType.ServerError)); // "Server error encountered"
```

9. Instead of using an enum, try creating an object `AnimalSounds` with string literal types `'Bark'`, `'Meow'`, and `'Roar'` using `as const`. Then, write a function `getAnimalSound` that accepts a key from `AnimalSounds` and returns the corresponding sound.

```ts
  console.log(getAnimalSound('Dog')); // "Bark"
  console.log(getAnimalSound('Lion')); // "Roar"
```

🎉 CONGRATULATIONS ! 🎉

[<< Day 16](../Day16_Utility_Types/Day16.md) | [Day 18 >>](../Day18_DOM/Day18.md)