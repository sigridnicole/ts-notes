# TypeScript

# Definition

- TypeScript is a language that is a *superset* of JavaScript: JS syntax is therefore legal TS.
- TypeScript doesn’t consider any JavaScript code to be an error because of its syntax.
- TypeScript is a ***typed* superset**, meaning that it adds rules about how different kinds of values can be used.
- Once your code is compiled, the resulting plain JS code has no type information.
- The goal of TypeScript is to be a **static typechecker** for JavaScript programs - in other words, a tool that runs before your code runs (static) and ensures that the types of the program are correct (typechecked).

<aside>
💡 **Superset** 
A set which includes another set or sets.

</aside>

# Benefits

The main benefit of TypeScript is that **it can highlight unexpected behavior in your code**, lowering the chance of bugs.

Typescript solves: The most common kinds of errors that programmers write can be described as type errors: a certain kind of value was used where a different kind of value was expected.

# Basics

**Static type-checking**

*Static types systems* describe the shapes and behaviors of what our values will be when we run our programs. A type-checker like TypeScript uses that information and tells us when things might be going off the rails.

**tsc commands**

- ***tsc <file.ts>*** | the typescript compiler
- ***tsc --noEmitOnError <file.ts>*** | dont update js file if with errors.

**Downleveling**

This process of moving from a newer or “higher” version of ECMAScript down to an older or “lower” one is sometimes called *downleveling*.

**By default TypeScript targets ES3**, an extremely old version of ECMAScript.

**TypeScript Settings**

TypeScript can require a little extra work, but generally speaking it pays for itself in the long run, and enables more thorough checks and more accurate tooling. When possible, a new codebase should always turn these strictness checks on.

`noImplicitAny`

Turning on the `[noImplicitAny](https://www.typescriptlang.org/tsconfig#noImplicitAny)` flag will issue an error on any variables whose type is implicitly inferred as `any`.

`strictNullChecks`

The `[strictNullChecks](https://www.typescriptlang.org/tsconfig#strictNullChecks)` flag makes handling `null` and `undefined` more explicit, and *spares* us from worrying about whether we *forgot* to handle `null` and `undefined`.

```jsx
function doSomething(x: string | null) {
  if (x === null) {
    // do nothing
  } else {
    console.log("Hello, " + x.toUpperCase());
  }
}
```

# **Everyday Types**

## Primitives

- String
- Number
- Boolean
- etc

## **Functions**

1. Parameter Type Annotation
    
    ```jsx
    function greet(name: **string**) {
      console.log("Hello, " + name.toUpperCase() + "!!");
    }
    ```
    
2. Return Type Annotations
    
    Much like variable type annotations, you usually don’t need a return type annotation because TypeScript will infer the function’s return type based on its `return` statements.
    
    ```jsx
    function getFavoriteNumber(): **number** {
      return 26;
    }
    ```
    
3. Anonymous Functions
    
    When a function appears in a place where TypeScript can determine how it’s going to be called, the parameters of that function are automatically given types.
    
    ```jsx
    // No type annotations here, but TypeScript can spot the bug
    const names = ["Alice", "Bob", "Eve"];
     
    // Contextual typing for function
    names.forEach(function (s) {
      console.log(s.toUppercase());
    Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?
    });
     
    // Contextual typing also applies to arrow functions
    names.forEach((s) => {
      console.log(s.toUppercase());
    Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?
    });
    ```
    
    > Even though the parameter `s` didn’t have a type annotation, TypeScript used the types of the `forEach` function, along with the inferred type of the array, to determine the type `s` will have.
    > 
    
    > This process is called ***contextual typing*** because the *context* that the function occurred within informs what type it should have.
    > 
    

***contextual typing***

This process is called *contextual typing* because the *context* that the function occurred within informs what type it should have.

## **Object Types**

```jsx
// The parameter's type annotation is an object type
function printCoord(pt: { x: number; y: number }) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
printCoord({ x: 3, y: 7 });
```

**Optional Properties**

add a `?` after the property name

```jsx
//...
function printName(obj: { first: string; last?: string }) {
//...
}
```

## **Union Types**

A **union type** is a type formed from two or more other types, representing values that may be ***any one*** of those types.

```jsx
function printId(id: number | string) {
  console.log("Your ID is: " + id);
}
// OK
printId(101);
// OK
printId("202");
// Error
printId({ myID: 22342 });
// Argument of type '{ myID: number; }' is not assignable to parameter of type 'string | number'.
```

```jsx
function logValue(x: Date | string) {
  if (x instanceof Date) {
    console.log(x.toUTCString());
               
(parameter) x: Date
  } else {
    console.log(x.toUpperCase());
               
(parameter) x: string
  }
}
```

## **Type Aliases**

A *type alias* is - a *name* for any *type*. Custom type. 

```jsx
type ID = number | string;

type Point = {
  x: number;
  y: number;
};
 
// Exactly the same as the earlier example
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
 
printCoord({ x: 100, y: 100 });
______________________

type UserInputSanitizedString = string;
 
function sanitizeInput(str: string): UserInputSanitizedString {
  return sanitize(str);
}
 
// Create a sanitized input
let userInput = sanitizeInput(getInput());
 
// Can still be re-assigned with a string though
userInput = "new input";
```

**Interfaces**

An *interface declaration* is another way to name an object type:

```jsx
interface Point {
  x: number;
  y: number;
}

function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}

printCoord({ x: 100, y: 100 });
```

```jsx
interface User {
  name: string;
  id: number;
}
 
class UserAccount {
  name: string;
  id: number;
 
  constructor(name: string, id: number) {
    this.name = name;
    this.id = id;
  }
}
 
const user: User = new UserAccount("Murphy", 1);
```

### Differences Between Type Aliases and Interfaces

Type aliases and interfaces are very similar, and in many cases you can choose between them freely. Almost all features of an `interface` are available in `type`, **the key distinction is that a type cannot be re-opened to add new properties vs an interface which is always extendable.**

![Untitled](TypeScript%20ca8d1/Untitled.png)

## **Type Assertions**

Sometimes you will have information about the type of a value that TypeScript can’t/does not know about. In this situation, **you can use a *type assertion* to specify a more specific type**:

```jsx
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
///or
const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");
```

### Literal Types

By themselves, literal types aren’t very valuable:

```jsx
let x: "hello" = "hello";
// OK
x = "hello";
// ...
x = "howdy";
//Type '"howdy"' is not assignable to type '"hello"'.Type '"howdy"' is not assignable to type '"hello"'.

```

**Use-case: *combining* literals into unions**

```jsx
function printText(s: string, alignment: "left" | "right" | "center") {
  // ...
}
printText("Hello, world", "left");
printText("G'day, mate", "centre");
//Argument of type '"centre"' is not assignable to parameter of type '"left" | "right" | "center"'.Argument of type '"centre"' is not assignable to parameter of type '"left" | "right" | "center"'.
```

- Numeric literal types work the same way - *function compare(a: string, b: string): -1 | 0 | 1 {*
- You can combine these with non-literal types - *function configure(x: Options | "auto") {*
- Boolean literals: true | false

### Literal Interface

1. You can change the inference by **adding a type assertion** in either location.

```jsx
declare function handleRequest(url: string, method: "GET" | "POST"): void;

//Error: In the above example req.method is inferred to be string, not "GET".
const req = { url: "https://example.com", method: "GET" };
handleRequest(req.url, req.method);

// Change 1:
const req = { url: "https://example.com", method: "GET" as "GET" };
// Change 2
handleRequest(req.url, req.method as "GET");
```

Change 1 means “I intend for `req.method` to always have the *literal type* `"GET"`”, preventing the possible assignment of `"GUESS"` to that field after. 

Change 2 means “I know for other reasons that `req.method` has the value `"GET"`“.

1. You can use `as const` to convert the entire object to be type literals: 

```jsx
const req = { url: "https://example.com", method: "GET" } as const;
handleRequest(req.url, req.method);
```

// the heck is this syntax — 

- A **const** declaration does not mean that the value of the variable cannot be changed. It just means that the **variable identifier cannot be reassigned**.
- Objects and arrays are **passed by reference**, not by value.

### null and undefined

**strictNullChecks on**- when a value is `null`
 or `undefined`, you will need to test for those values before using methods or properties on that value.

**Non-null Assertion Operator (Postfix`!`)**

- only perform the function if not null.
- Writing `!` after any expression is effectively a type assertion that the value isn’t `null` or `undefined`

```jsx
function liveDangerously(x?: number | null) {
  // No error
  console.log(x!.toFixed());
}
```

**non-null assertion**

(a `!` after `shape.radius`) for example:

```
function getArea(shape: Shape) {
  if (shape.kind === "circle") {
    return Math.PI * shape.radius! ** 2;
  }
}

```

[Cheatsheets](https://www.notion.so/Cheatsheets-c231c98d206d4e06b6fd05e43f09308b)

[Typescript App Ideas](https://www.notion.so/Typescript-App-Ideas-0a45390fe31d4da4a46eb6e8a69a3679)

## **Narrowing**

*Narrowing* occurs when TypeScript can deduce a more specific type for a value based on the structure of the code.

> *Basically, TypeScript expects the code to handle both types if union type annotation is used.*
> 

Sometimes you’ll have a union where all the members have something in common. For example, both arrays and strings have a `slice` method. If every member in a union has a property in common, you can use that property without narrowing:

```
// Return type is inferred as number[] | string
function getFirstThree(x: number[] | string) {
  return x.slice(0, 3);
}
```

***type guard***

In TypeScript, checking against the value returned by `typeof` is a **type guard**.

```jsx
function padLeft(padding: number | string, input: string) {
  if (typeof padding === "number") {
    return " ".repeat(padding) + input;
                        
// hover to padding: (parameter) padding: number
  }
  return padding + input;      
// hover to padding: (parameter) padding: string
}
```

**Truthiness narrowing**

example:

```jsx
function printAll(strs: string | string[] | null) {
  if (strs && typeof strs === "object") {
    for (const s of strs) {
      console.log(s);
    }
  } else if (typeof strs === "string") {
    console.log(strs);
  }
}
```

**Equality Narrowing**

TypeScript also uses `switch`
 statements and equality checks like `===`
, `!==` , `==` , and `!=` to narrow types.

```jsx
function example(x: string | number, y: string | boolean) {
  if (x === y) {
    // We can now call any 'string' method on 'x' or 'y'.
    x.toUpperCase();
          
//hover (method) String.toUpperCase(): string
    y.toLowerCase();
          
//hover (method) String.toLowerCase(): string
  } else {
    console.log(x);
               
//hover (parameter) x: string | number
    console.log(y);
               
//hover (parameter) y: string | boolean
  }
}
```

**The`in`operator narrowing**

- Javascript - **in** is used to check if an object has a property with that name.
- TypeScript - uses **in** to narrow down potential types.

example: 

```jsx
type Fish = { swim: () => void };
type Bird = { fly: () => void };
 
function move(animal: Fish | Bird) {
  if ("swim" in animal) {
    return animal.swim();
  }
 
  return animal.fly();
}
```

**instanceof narrowing**

`instanceof` is also a type guard, and TypeScript narrows in branches guarded by `instanceof`s.

example:

**Assignments**

TypeScript type assignment / type assertion looks at the right side of the assignment and narrows the left side appropriately.

```jsx
let x = Math.random() < 0.5 ? 10 : "hello world!";
   
//let x: string | number
x = 1;
 
console.log(x);
           
//let x: number
x = "goodbye!";
 
console.log(x);
           
//*let x: string*
```

## Control Flow Analysis

**Analysis of code based on reachability.** TypeScript uses this flow analysis to narrow types as it encounters **type guards** and **assignments**. 

Example is the same as above in Assignments where the type changes depending on the current value.

## T**ype Predicates**

To define a **user-defined type guard**, we simply need to define a function whose return type is a ***type predicate***:

```tsx
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}

// Both calls to 'swim' and 'fly' are now okay.
let pet = getSmallPet();
 
if (isFish(pet)) {
  pet.swim();
} else {
  pet.fly();
}
```

> Any time `isFish` is called with some variable, TypeScript will *narrow* that variable to that specific type if the original type is compatible.
> 

A clearer? example:

```jsx
interface Cat {
  numberOfLives: number;
}
interface Dog {
  isAGoodBoy: boolean;
}

## type predicate snippet #1 
function isCat(animal: Cat | Dog): animal is Cat {
  return typeof (animal as Cat).numberOfLives === 'number';
}

## type predicate snippet #2
function isCat(animal: Cat | Dog): animal is Cat {
  return (animal as Cat).numberOfLives !== undefined;
}

console.log('is animal a Cat', isCat({numberOfLives: 1}) )
```

## Discriminated Unions

When every type in a union contains **a common property with literal types**, TypeScript considers that to be a ***discriminated union***, and can narrow out the members of the union.

```tsx
interface Circle {
  kind: "circle";
  radius: number;
}
 
interface Square {
  kind: "square";
  sideLength: number;
}
 
type Shape = Circle | Square;

function getArea(shape: Shape) {
  if (shape.kind === "circle") {
    return Math.PI * shape.radius ** 2;                    
//(parameter) shape: Circle
  }
}
```

This works with switch as well.

```tsx
function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;                    
//(parameter) shape: Circle
    case "square":
      return shape.sideLength ** 2;
//(parameter) shape: Square
  }
}
```

## The never type

- The `never` type is a type that contains **no values**.
- The `never` type represents the return type of a function that **always throws an error** or a function that contains an **indefinite loop**.
- The `never` type is assignable to every type; however, no type is assignable to `never` (except `never` itself).

For example: using never for default shape. If Shape extends another union, the function getArea will return an error message.

*Type 'Triangle' is not assignable to type 'never'.*

```tsx
type Shape = Circle | Square;
 
function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
    default:
      const _exhaustiveCheck: never = shape; 
      return _exhaustiveCheck;
  }
}
```

# More on Functions

**Function Type Expressions**

```tsx
function greeter(fn: (a: string) => void) {
  fn("Hello, World");
}

//OR USING A TYPE ALIAS for a function
type GreetFunction = (a: string) => void;
function greeter(fn: GreetFunction) {
  // ...
}
```

`(a: string) => void` means “a function with one parameter, named `a`, of type string, that doesn’t have a return value”.

**Call Signatures**

If we want to describe something callable with properties, we can write a *call signature*
 in an object — idk what this means. For future reference: [https://stackoverflow.com/a/32043715/10913205](https://stackoverflow.com/a/32043715/10913205)

```tsx
type DescribableFunction = {
  description: string;
  (someArg: number): boolean;
};
function doSomething(fn: DescribableFunction) {
  console.log(fn.description + " returned " + fn(6));
}
```

**Construct Signatures**

JavaScript functions can also be invoked with the `new` operator. TypeScript refers to these as *constructors* because they usually create a new object.

```jsx
type SomeConstructor = {
  new (s: string): SomeObject;
};
function fn(ctor: SomeConstructor) {
  return new ctor("hello");
}
```

Some objects, like JavaScript’s `Date` object, can be called with or without `new`. You can combine call and construct signatures in the same type arbitrarily

```jsx
interface CallOrConstruct {
  new (s: string): Date;
  (n?: number): number;
}
```

### **Optional Parameters**

We can model this in TypeScript by marking the parameter as *optional* with `?`:

```tsx
function f(x?: number) {
  // ...
}
f(); // OK
f(10); // OK

```

You can also provide a parameter *default*:

```
function f(x = 10) {
  // ...
}
```

### **Optional Parameters in Callbacks**

## **Generic Functions**

This is used when the type of the input and output is not certain. Hence, generic.

By adding a type parameter `Type` to this function and using it in two places, we’ve created **a link between the input of the function (the array) and the output (the return value).** Now when we call it, a more specific type comes out:

```tsx
function firstElement<Type>(arr: Type[]): Type | undefined {
  return arr[0];
}

// s is of type 'string'
const s = firstElement(["a", "b", "c"]);
// n is of type 'number'
const n = firstElement([1, 2, 3]);
// u is of type undefined
const u = firstElement([]);
```

### Inference

The type was ***inferred*** - **chosen automatically** - by TypeScript.

In this snippet, both input and output types have been inferred.

```jsx
function map<Input, Output>(arr: Input[], func: (arg: Input) => Output): Output[] {
  return arr.map(func);
}

// Parameter 'n' is of type 'string'
// 'parsed' is of type 'number[]'
const parsed = map(["1", "2", "3"], (n) => parseInt(n));

```

### Constraints

We can use a ***constraint*** to limit the kinds of types that a type parameter can accept. We *constrain* the type parameter to that type by writing an `extends` clause:

```jsx
function longest<Type extends { length: number }>(a: Type, b: Type) {
  if (a.length >= b.length) {
    return a;
  } else {
    return b;
  }
}
 
// longerArray is of type 'number[]'
const longerArray = longest([1, 2], [1, 2, 3]);
// longerString is of type 'alice' | 'bob'
const longerString = longest("alice", "bob");
// Error! Numbers don't have a 'length' property
const notOK = longest(10, 100);
//Argument of type 'number' is not assignable to parameter of type '{ length: number; }'.
```

- The types of `longerArray` and `longerString` were inferred based on the arguments. Since both string type and array types have length property.
- The call to `longest(10, 100)` is rejected because the `number` type doesn’t have a `.length` property.

<aside>
💡 **Remember, generics are all about relating two or more values with the same type!**

</aside>

### **Guidelines for Writing Good Generic Functions**

1. When possible, use the type parameter itself rather than constraining it:

```tsx
function firstElement1<Type>(arr: Type[]) {
  return arr[0];
}
 
function firstElement2<Type extends any[]>(arr: Type) {
  return arr[0];
}
 
// a: number (good)
const a = firstElement1([1, 2, 3]);
// b: any (bad)
const b = firstElement2([1, 2, 3]);
```

1. Always use as few type parameters as possible****

```tsx
function filter1<Type>(arr: Type[], func: (arg: Type) => boolean): Type[] {
  return arr.filter(func);
}
 
function filter2<Type, Func extends (arg: Type) => boolean>(
  arr: Type[],
  func: Func
): Type[] {
  return arr.filter(func);
}
```

1. If a type parameter only appears in one location, strongly reconsider if you actually need it.****

Sometimes we forget that a function might not need to be generic:

```tsx
function greet<Str extends string>(s: Str) {
  console.log("Hello, " + s);
}

greet("world");
```

We could just as easily have written a simpler version:

```tsx
function greet(s: string) {
  console.log("Hello, " + s);
}
```
