<div align="center"> 
  <h1>20 Days of Basics TypeScript: Arrays</h1>
</div>

<div align="center"> 

<!-- Social links -->
[![Discord](https://img.shields.io/badge/Discord-%237289DA.svg?logo=discord&logoColor=white)](htttps://discord.gg/Samson#0273) [![Facebook](https://img.shields.io/badge/Facebook-%231877F2.svg?logo=Facebook&logoColor=white)](https://www.facebook.com/chiemezie.nebeolisa/) [![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?logo=Instagram&logoColor=white)](https://www.instagram.com/samson_nebeolisa/) [![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/chiemezie-samson-nebeolisa-32897310b/) [![Stack Overflow](https://img.shields.io/badge/-Stackoverflow-FE7A16?logo=stack-overflow&logoColor=white)](https://stackoverflow.com/users/20653301/nebeolisa-chiemezie-samson) [![Twitter](https://img.shields.io/badge/Twitter-%231DA1F2.svg?logo=Twitter&logoColor=white)](https://twitter.com/SamsonChiemezie) [![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?logo=YouTube&logoColor=white)](https://myaccount.google.com/u/0/?utm_source=YouTubeWeb&tab=rk&utm_medium=act&tab=rk&hl=en) 

<!-- Portfolio -->
 📰 About Me [Portfolio](https://www.nebe-samson.com/)
 <br/>
  <small>Sep, 2024</small>
</div>

[<< Day 2](../Day2_Types/Day2.md) | [Day 4 >>](../Day4_Tuple/Day4.md)

<div align="center"> 
  <a class="header-image" target="_blank" href="../Asset/images/Days/Day_3.webp">
    <img alt="Typescript image" src="../Asset/images/Days/Day_3.webp" width="100%" height="600px">
  </a>
</div>

## Table of Contents

- [📔 Day 3](#-day-3)
- [Arrays in TypeScript](#arrays-in-typeScript)
  - [Typed Arrays](#typed-arrays)
  - [Generic Arrays](#generic-arrays)
  - [Arrays with Multiple Data Types](#arrays-with-multiple-data-types)
  - [Arrays of Objects](#arrays-of-objects)
    - [Array of Objects with Nested Objects](#array-of-objects-with-nested-objects)
    - [Array of Objects with Optional Properties](#array-of-objects-with-optional-properties)
    - [Common Operations with Arrays of Objects](#common-operations-with-arrays-of-objects)
  - [Multi-Dimensional Arrays](#multi-dimensional-arrays)
  - [Arrays as Function Parameters](#arrays-as-function-parameters)
  - [Array of Functions](#array-of-functions)
  - [Readonly Arrays](#readonly-arrays)
  - [💻 Day 3: Exercises](#-day-3-exercises)

# 📔 Day 3

# Arrays in TypeScript

If you're coming from JavaScript, arrays are likely an old friend. You've used them to store lists of values, loop through data, and perform countless other operations. But in TypeScript, arrays get a serious upgrade with strong typing. This means that while you still get the flexibility of arrays, you now also get the safety of knowing exactly what type of data those arrays are holding at any given moment. This extra layer of type-checking helps you catch errors early and makes your code more predictable, reliable, and easier to debug.

> As I mentioned at the beginning of this course, I won't be diving deeply into basic JavaScript concepts when discussing TypeScript. I’m assuming you already have a foundational understanding of JavaScript arrays. This allows us to focus more on how TypeScript enhances these familiar structures with features like strong typing and better error handling.

## Typed Arrays

TypeScript has a specific syntax for typing arrays.

```ts
  const numbers = [1, 2, 3];  // inferred as number[], meaning it can only hold number
  const names: string[] = []; // declared as string[], meaning it can only hold strings
  const ages: number[] = [];  // declared as number[], meaning it can only hold number
```

TypeScript will now prevent you from adding the wrong type of data to these arrays.

```ts
  numbers.push(4);  // This works fine, since 4 is a number
  names.push("Dylan");  // No error, as "Dylan" is a string
  ages.push(55);  // No error, as 55 is a number

  // But if you try to push something that doesn’t match the type:
  names.push(4); // ❌ Error: Argument of type 'number' is not assignable to parameter of type 'string'.
```

TypeScript helps catch this mistake at compile time before you even run the code. let's see another example:

```ts
  let myArray: string[] = ["hello", "world"];

  // You can add more strings to it:
  myArray.push("!");  // This works fine
  console.log(myArray);  // Output: ["hello", "world", "!"]

  // But if you try to add something else, TypeScript will catch the mistake:
  myArray.push(8); // ❌ Error: Argument of type 'number' is not assignable to parameter of type 'string'.
```

## Generic Arrays

Generics allow you to define reusable components, which can work with multiple types rather than a single one. Let’s break this down with __Generic Arrays__. Syntax for Generic Arrays:

```ts
  let myArray: Array<Type>;
```

Where ```Type``` is the type of elements the array will hold.

Examples:

```ts
  // 1. Number Array (Generic Style)
  let numberArray: Array<number> = [10, 20, 30, 40];

  numberArray.push(50); // Works fine
  numberArray.push("string");  // ❌ Error: Argument of type 'string' is not assignable to type 'number'

  // 2. String Array (Generic Style)
  let stringArray: Array<string> = ["apple", "banana", "cherry"];

  stringArray.push("date"); // Works fine
  stringArray.push(100); // ❌ Error: Argument of type 'number' is not assignable to type 'string'

  // 3. Generic Functions with Arrays
  function getFirstElement<T>(array: Array<T>): T {
    return array[0];
  }

  let firstNumber = getFirstElement([1, 2, 3]);  // number type inferred
  let firstString = getFirstElement(["a", "b", "c"]);  // string type inferred

  console.log(firstNumber); // 1
  console.log(firstString); // "a"

  // Here, <T> is a generic type variable. It means this function works with any type of array (numbers, strings, objects), and TypeScript will infer the type automatically.

  // 4. Working with Objects in Generic Arrays
  interface Person {
    name: string;
    age: number;
  }

  let peopleArray: Array<Person> = [
      { name: "Samson", age: 30 },
      { name: "Bob", age: 25 }
  ];

  peopleArray.push({ name: "Charlie", age: 35 });
  peopleArray.push({ name: "David", age: "unknown" }); // ❌ Error: Type 'string' is not assignable to type 'number'
```

Let’s create a function that can reverse arrays of any type, using generics:

```ts
  function reverseArray<T>(items: Array<T>): Array<T> {
    return items.reverse();
  }

  let reversedNumbers = reverseArray([1, 2, 3]); // returns [3, 2, 1]
  let reversedStrings = reverseArray(["a", "b", "c"]); // returns ["c", "b", "a"]

  console.log(reversedNumbers);
  console.log(reversedStrings);
```

>[!NOTE]
> Whenever we write out types like ```number[] or string[]```, that’s really just a shorthand for ```Array<number>``` and ```Array<string>``` Generic object.

If there's anything from the above that isn't completely clear right now, don't worry—by the end of this course, you'll have a solid understanding of it all.

## Arrays with Multiple Data Types

You can define arrays that hold different types of data using union types (```|```). This is useful when you need an array to store mixed values like strings and numbers.

```ts
  let mixedArray: (number | string)[] = [1, "two", 3, "four"];

  mixedArray.push(5);     // Works fine
  mixedArray.push("six"); // Works fine
  mixedArray.push(true);  // ❌ Error: Argument of type 'boolean' is not assignable to parameter of type 'string | number'.
```

## Arrays of Objects

It’s common to work with arrays of objects in development. For example, an array of people with ```id```, ```name```, ```email``` and ```age``` fields. ```Types``` or ```interfaces``` is used to define the shape (structure) of an object, which helps with type safety.

```ts
  interface User {
    id: number;
    name: string;
    email: string;
  }

  // simple users array
  const users: User[] = [
    { id: 1, name: 'John Doe', email: 'john@example.com' },
    { id: 2, name: 'Jane Smith', email: 'jane@example.com' },
  ];


  interface Person {
    name: string;
    age: number;
  }

  let people: Person[] = [
    { name: "Alice", age: 25 },
    { name: "Bob", age: 30 }
  ];

  people.push({ name: "Charlie", age: 35 });  // Works fine
  people.push({ name: "David", age: "forty" }); // ❌ Error: Type 'string' is not assignable to 'number'.
```

### **Array of Objects with Nested Objects**
TypeScript just like in javaScript also allows objects within objects. Here’s how you can handle nested objects:

```ts
  interface Address {
    city: string;
    country: string;
  }

  interface User {
    id: number;
    name: string;
    email: string;
    address: Address; // Nested Object
  }

  const users: User[] = [
    { id: 1, name: 'John Doe', email: 'john@example.com', address: { city: 'New York', country: 'USA' } },
    { id: 2, name: 'Jane Smith', email: 'jane@example.com', address: { city: 'London', country: 'UK' } },
  ];

  // Accessing nested properties
  console.log(users[0].address.city); // Output: 'New York'
```

### **Array of Objects with Optional Properties**

As a developer, you’ll often encounter situations where not all properties of an object are immediately available or applicable. For instance, when working with product data, there may be times when some products lack certain details, like a price. This could happen because the price is not yet available in the database, or it's a product feature that might not apply to all items.

To handle such cases, TypeScript provides a way to define optional properties using the ? symbol. This allows you to specify that a property may or may not be present on an object, giving you flexibility when dealing with incomplete data.

```ts
  interface Product {
    id: number;
    name: string;
    price?: number; // Optional property
  }

  const products: Product[] = [
    { id: 1, name: 'Laptop', price: 1200 },
    { id: 2, name: 'Phone' }, // Price is optional
  ];

  // Check if the optional property exists
  products.forEach(product => {
    console.log(product.name, product.price ? product.price : 'Price not available');
  });
```

### **Common Operations with Arrays of Objects** 

```ts
  // Add a new object to the array
  const newProduct: Product = { id: 3, name: 'Smartwatch', price: 300 };
  products.push(newProduct);

  console.log(products);

  // Filter the array 
  const expensiveProducts = products.filter(product => product.price && product.price > 500);

  console.log(expensiveProducts); // Output: [{ id: 1, name: 'Laptop', price: 1200 }]

  // Find a specific object in the array
  const phone = products.find(product => product.name === 'Phone');

  console.log(phone); // Output: { id: 2, name: 'Phone' }

  // Update an object in the array
  const updatedProduct = products.find(product => product.id === 2);

  if (updatedProduct) {
    updatedProduct.price = 850;
  }

  console.log(products);

  // Remove an object from the array
  const updatedProducts = products.filter(product => product.id !== 1);

  console.log(updatedProducts);
```

## Multi-Dimensional Arrays 
 
 This is same with javaScript ```Array of Arrays```. This structure consists of multiple arrays nested inside a single array. It's useful when you want to group related sets of data, like a grid of numbers or a collection of lists.

An array of arrays is essentially a multi-dimensional array where each element is itself an array.

```ts
  let matrix: number[][] = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
  ];

  // accessing
  console.log(matrix[0][1]); // Output: 2 (1st row, 2nd column)
  console.log(matrix[2][0]); // Output: 7 (3rd row, 1st column)

  // assigning
  matrix[0][0] = 10;  // Works fine
  matrix[0][1] = "11"; // ❌ Error: Type 'string' is not assignable to type 'number'.
```

## Arrays as Function Parameters

When passing an array to a function, you can also specify the type of array the function should accept. This ensures that the array passed in matches the expected type.

```ts
  // Array as a function parameter (generic)
  function doSomething(value: Array<string>) {
    // ...
  }

  doSomething(myArray);
  doSomething(new Array("hello", "world"));


  // type annotations
  function printStrings(values: string[]) {
    values.forEach(value => console.log(value));
  }

  printStrings(["hello", "world"]);

  printStrings([1, 2, 3]); // ❌ Error: 'number[]' is not assignable to 'string[]'.
```

## Array of Functions

You can also create an array of functions, which is handy in many scenarios:

```ts
  const functionArray: Array<() => void> = [
    () => console.log("First function"),
    () => console.log("Second function")
  ];

  functionArray.forEach(func => func());  // This will call both functions.
```

## Readonly Arrays

Sometimes you might want an array that cannot be changed. TypeScript provides a way to create arrays that are "read-only," meaning no one can add, remove, or change the items in the array.

```ts
  const colors: ReadonlyArray<string> = ["red", "green", "blue"];
  console.log(colors[0]);  // Output: "red"
  colors.push("yellow"); // ❌ Error: Property 'push' does not exist on type 'readonly string[]'.

  // same as above
  const colors: readonly string[] = ["red", "green", "blue"];


  // os function parameter
  function doSomething(values: ReadonlyArray<string>) {
    // We can read from 'values'...
    console.log(`The first value is ${values[0]}`);
  
    // ...but we can't mutate 'values'.
    values.push("hello!"); // ❌ Error: Property 'push' does not exist on type 'readonly string[]'.
  }

  // same as above
  function doSomething(values: readonly string[]) { 
    // ....
  } 
```

🌟 Awesome job! You’ve successfully completed your Day 3, and you're well on your way to becoming a great developer. Keep up the momentum! Now, let's keep your mind sharp and your body active with some quick exercises.

## 💻 Day 3: Exercises

1. Create an array called ```ages``` that holds the numbers ```18```, ```25```, ```30```. Add a new age ```40``` to the array. Log the updated array.

```ts
  console.log(ages); // [..., 40]
```

2. Create an array called ```colors``` that holds the strings ```"red"```, ```"green"```, and ```"blue"```. Add a new color ```"yellow"``` to the array. Log the last color in the array. 

```ts
  console.log(lastColor); 
```

3. Create an array called ```mixedValues``` that can store both numbers and strings. Add some numbers and strings to the array, and log the entire array.

```ts
  console.log(mixedValues); // [..., 40]
```

4. Create a 2D array (matrix) called ```grid``` with the values:

```ts 
  [
    [3, 10, 20],
    [1, 5, 17],
    [37, 13, 19]
  ]
```

Update the value at position ```13, 5``` and ```10``` to ```33, 55``` and ```100``` and log the entire matrix.

5. Write a function called ```printNames``` that accepts an array of strings as a parameter and logs each name. Pass an array of your choice to the function and call it.


6. Create a generic function ```getLastElement``` that takes an array of any type and returns the last element of the array. Test it with both a number array and a string array.

```ts 
  const numArray = [1, 2, 3, 4]; // get 4
  const strArray = ["a", "b", "c"]; // get "c"
```

7. Define an array of objects representing people, where each object has a ```id```, ```name```, ```email```, ```address```, ``````gender``` and ```age``` property. Use a generic array type to ensure type safety and write a function ```logPeople``` that logs each person’s details.

8. Create a tuple ```user``` that holds a ```number```, a ```string```, and a ```boolean```. Write a function that takes this tuple and logs the values in a readable format.

🎉 CONGRATULATIONS ! 🎉

[<< Day 2](../Day2_Types/Day2.md) | [Day 4 >>](../Day4_Tuple/Day4.md)