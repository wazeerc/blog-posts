TypeScript is amazing, there's no doubt about it. The numbers back it up: it’s one of the fastest-growing languages. With the recent announcement of the TypeScript compiler being ported to Go, and native support coming to Node.js, the language is only getting better and more powerful.

Yet, despite its growth, many developers (especially those coming from JavaScript) overlook some of the most useful features TypeScript has to offer.

Fun fact: `any` is used over **20 Million** times in code available on GitHub, this is considered a bad practice and is a clear sign that many developers are not taking full advantage of TypeScript's capabilities.

In this article, I will talk about a bunch of cool TypeScript features that _can_ help you write cleaner, safer, and more efficient code.

## What is TypeScript?

TypeScript is simply _JavaScript with syntax for types_. Some of the benefits are:

- **Catch bugs early**: The compiler warns you before you run into runtime errors.
- **Better autocompletion**: Your editor becomes smarter with accurate suggestions and IntelliSense.
- **Improved readability & collaboration**: Clear types make code easier to read and maintain.

_Types_, if it was not obvious enough, are the very essence of **Type**Script. Hence, if you want to get the most out of the language, you need to understand how to use them effectively. Primitive types that are available out of the box are: `string`, `number`, `boolean`, `null`, `undefined`, `void`, and `any`. But that is only the tip of the iceberg. There are many more features that can help you write TypeScript better code.

## Type vs Interface

Types and Interfaces are two of the most powerful features in TypeScript. They allow you to define the shape of your data.

However, they have different use cases and strengths. If you want to reap their benefits, you need to understand when to use each.
Here’s a quick comparison:

| Feature             | Type                                         | Interface                                      |
| ------------------- | -------------------------------------------- | ---------------------------------------------- |
| Extensibility       | Uses intersection types (`&`)                | Uses `extends` keyword                         |
| Declaration merging | Not supported (cannot add new properties)    | Supported (declarations with same name merge)  |
| Advanced types      | Supports unions, tuples, mapped types        | Limited to object-like structures              |
| Performance         | Better for complex type manipulations        | Slightly better for large object structures    |
| Recommendation      | Complex type operations, unions, exact types | Object definitions, API contracts, inheritance |

- Use `type` for complex types, unions or intersections.
- Use `interface` for object-like structures and when you need extensibility.

But remember, TypeScript is flexible and you can use both interchangeably in many cases. The choice often comes down to personal or team preference.

## Generics

TypeScript Generics are similar to those in other languages such as Java or C#. They allow you to create reusable components that can work with any data type. There are different patterns for using generics:

**Generic types**: Can be used with any "type". This is useful when you want to create a type that can hold any value.

```ts
type Box<T> = {
  value: T;
};
const boxWithNumber: Box<number> = { value: 999 }; // ✅
const boxWithStrings: Box<string> = { value: "Hello, Generics!" }; // ✅
```

**Generic interfaces**: Interfaces that accept type parameters, making them flexible and reusable. They can be used to create complex data structures.

```ts
interface Pair<T, J> {
  first: T;
  second: J;
}
// This creates a pair of number & string, mapped to the generic types T and J
const pair: Pair<number, string> = { first: 1, second: "one" };
```

**Generic functions**: Functions that accept type parameters, allowing them to work with any type.

```ts
function getFirstElement<T>(array: T[]): T {
  return array[0];
}

// This will work with any type of array
const firstNumber = getFirstElement([1, 2, 3]); // ✅
const firstString = getFirstElement(["a", "b", "c"]); // ✅
```

**Generic constraints**: Constrain the types that can be passed to a generic function or type.

```ts
// T is constrained to types that have a length property
function getLength<T extends { length: number }>(item: T): number {
  return item.length;
}

// This will work because arrays have a length property
const lengthOfArray = getLength([1, 2, 3]);
// ❌ This will throw an error because number doesn't have a length property
const lengthOfNumber = getLength(123);
```

Arguably, you could use `any` instead of generics, but that defeats the purpose of TypeScript. You would lose type safety and the benefits of TypeScript’s type system.

I find **Generics** to be very useful when writing utility functions that need to work with various data types and should remain as _generic_ as possible.

## Utility Types

TypeScript has many built-in utility types that help with type transformations. These can save you time and reduce boilerplate code. Some of the common ones include:

**`Awaited<Type>`** : Converts a promise type to its resolved type.

```typescript
type MyPromise = Promise<string>;
type MyResolvedType = Awaited<MyPromise>; // This will be a string
```

**`Omit<Type>`** : Omits specific keys from a type.

```typescript
interface UserProfile {
  id: number;
  name: string;
  email: string;
}
type UserContactInfo = Omit<UserProfile, "id" | "name">;
// Result: { email: string; }
```

**`Partial<Type>`** : Makes all properties optional.

```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}
type PartialTodo = Partial<Todo>;
// Result: { title?: string; description?: string; completed?: boolean; }
```

**`Pick<Type>`** : Picks specific keys from a type.

```typescript
interface Product {
  id: string;
  name: string;
  price: number;
  inStock: boolean;
}
type ProductNameAndPrice = Pick<Product, "name" | "price">;
// Result: { name: string; price: number; }
```

**`Readonly<Type>`** : Makes all properties read-only.

```typescript
interface Config {
  apiKey: string;
  apiUrl: string;
}
type ReadonlyConfig = Readonly<Config>;

const config: ReadonlyConfig = { apiKey: "abc", apiUrl: "example.com" };
config.apiKey = "def"; // ❌ Error: Cannot assign to 'apiKey' because it is a read-only property.
```

**`Record<Key, Type>`** : Constructs a type with a set of properties Key of type Type.

```typescript
type Page = "home" | "about" | "contact";
type PageInfo = { title: string; path: string };

// This creates a mapping of page names to their info
// title is the key and path is the value
const siteMap: Record<Page, PageInfo> = {
  home: { title: "Homepage", path: "/" },
  about: { title: "About Us", path: "/about" },
  contact: { title: "Contact", path: "/contact" },
};
```

**`Required<Type>`** : Makes all properties required.

```typescript
interface UserPreferences {
  theme?: "light" | "dark";
  notifications?: boolean;
}
type CompleteUserPreferences = Required<UserPreferences>;
// { theme: "light" | "dark"; notifications: boolean; }
```

**Alternative: Extending Interfaces/Types**

Utility types are very powerful, but they can often lead to overly complex definitions. Knowing when to use them is key. In most cases, you can get away with simple interface extensions or type intersections.

For example, instead of using `Pick` or `Omit` in some scenarios, you could define a base and then extend it:

```ts
// BaseUser is the base interface with all common properties
interface BaseUser {
  id: string;
  username: string;
  createdAt: Date;
}
// UserProfile extends BaseUser and adds its own properties
interface UserProfile extends BaseUser {
  bio: string;
  profilePictureUrl?: string;
}
// UserAdminView extends BaseUser and adds its own properties
interface UserAdminView extends BaseUser {
  email: string;
  lastLogin: Date;
  isActive: boolean;
}
```

## Discriminated Unions

Discriminated unions are great for modeling scenarios where a value can be one of several types with a common property:

```ts
// Shape is a discriminated union of two types: circle and square
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; side: number };

// This function uses a discriminated union to determine the area of a shape
function getArea(shape: Shape) {
  // TypeScript narrows down the type based on the `kind` property
  if (shape.kind === "circle") return Math.PI * shape.radius ** 2;
  if (shape.kind === "square") return shape.side ** 2;
}
```

**Conditional Types** are also a great way to achieve similar results, but they can be more complex and less readable, as seen in the example below:

```ts
// This type checks if T is a circle or square and infers the radius or side length. If neither, it returns `never`.
type Area<T> = T extends { kind: "circle"; radius: infer R }
  ? number
  : T extends { kind: "square"; side: infer S }
    ? number
    : never;
```

## Useful Keywords

There are several keywords in TypeScript that are often overlooked but can be very useful. Here are a few:

**`as`**: Used for type assertions. It tells the compiler to treat a value as a specific type.

```ts
// This is a type assertion, telling TypeScript to treat value as a string
const value: unknown = "Hello, TypeScript!";
const strValue: string = value as string;
```

**`as const`**: Treats the value as a constant. It’s particularly useful for creating immutable objects or arrays.

I personally prefer using `as const` instead of `enum`. Enums are not natively supported in JavaScript, and they can lead to unexpected behaviors. Using `as const` allows you to create a similar effect without the downsides of enums.

```ts
// This creates a constant object with readonly properties
const Status = {
  isLoading: "isLoading",
  isSuccess: "isSuccess",
  isError: "isError",
} as const;
// This type will be a union of the values of the Status object
type TStatus = (typeof Status)[keyof typeof Status];
// The values can be accessed using the keys
const defaultStatus: TStatus = Status.isLoading;
```

**Note**: `keyof` is used to get the keys of an object type as a union of string literals and `typeof` is used to get the type of a variable or object.

**`infer`**: Used in conditional types to infer a type within a scope. It can be used to extract types from other types.

```ts
// This conditional type checks if T is a function and infers its return type.
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

// This will extract the return type of MyFunction, which is string
type MyFunction = (x: number) => string;
type MyReturnType = ReturnType<MyFunction>; // string
```

**`is`**: Used in type guards to check if a value is of a specific type.

```ts
// This is a type guard function that checks if a value is a string
function isString(value: unknown): value is string {
  return typeof value === "string";
}

const value: unknown = "Hello, TypeScript!";
if (isString(value)) {
  console.log(value.toUpperCase()); // Safe to use as string
}
```

**`never`**: Represents a type that never occurs. It’s useful for functions that throw errors or never return.

```ts
// This function will never return a value
function throwError(message: string): never {
  throw new Error(message);
}
```

**`satisfies`**: Checks if a value satisfies a type without changing its type.

```ts
const obj = {
  name: "TypeScript",
  version: 4.9,
} satisfies { name: string; version: number };
// ✅ This is a valid object, as it satisfies the type

const obj2 = {
  name: "TypeScript",
  version: 4.9,
} satisfies { name: string; version: number; extraProp: string };
// ❌ This will throw an error, as extraProp is not in the type
```

**`unknown`**: A type-safe counterpart to `any`. It forces you to perform type checks before using the value.

```ts
// This is a type-safe way to handle unknown values
const value: unknown = "Hello, TypeScript!";

if (typeof value === "string") {
  console.log(value.toUpperCase()); // Safe to use as string
} else {
  console.log(typeof value, "is not a string"); // Handle other types
}
```

## TypeScript Under the Hood

Having explored several key TypeScript features and their practical uses, let’s now dive into how the compiler works behind the scenes. This is not something you need to know to use TypeScript, but it can help you understand how things work under the hood and how it can help you debug your code.

TypeScript is a _superset of JavaScript_, it extends the JavaScript syntax with type annotations. The TypeScript compiler (tsc) takes your code and compiles it to JavaScript. This process involves several steps:

1. **Parsing**: The compiler reads the code (.ts files) and converts it into an Abstract Syntax Tree (AST). This is a tree representation of the code structure (variables, functions, etc.).
2. **Type Checking**: The compiler walks the AST to check each node against its respective declared or inferred type. This is where TypeScript checks for type errors and ensures that the code adheres to the defined types. It uses a bi-directional inference algorithm, when explicit annotations exist, it uses them to check the types (top-down) and where they are not present, it infers the types based on the code structure (bottom-up).
3. **High-Level Type system**: This allows for complex type manipulations. It includes features like generics, unions, intersections, and conditional types; they are never emitted to the output JavaScript code.
4. **Emitting JavaScript**: After type checking, the compiler generates the plain JavaScript code. This code is equivalent to the original TypeScript code but without type annotations. The generated code is valid JavaScript and can be run in any JavaScript environment. Depending on the configuration, the compiler can target different versions of JavaScript (ES5, ES6, etc.) and can also include polyfills for newer features.
5. **Source Maps**: The compiler can generate source maps (.js.map files) that map the TypeScript code to the generated JavaScript code. This allows you to debug your TypeScript code in the browser or in a Node.js environment, even though the code running is JavaScript. It can also emit declaration files (.d.ts).
6. **Watch Mode**: The compiler can run in watch mode, which automatically recompiles the code when changes are detected. This is useful for development, as it provides instant feedback on type errors and allows for a smoother development experience.
7. **Incremental Compilation**: The compiler can perform incremental compilation, which means it only recompiles the files that have changed since the last compilation. This speeds up the build process, especially for large projects.
8. **Compiler API and Language Services**: TypeScript provides a compiler API that allows you to programmatically interact with the compiler. This is useful for building tools, IDEs, and other integrations. The language services provide features like autocompletion, type checking, and refactoring support in editors.

## Conclusion

TypeScript is a powerful language that can help you write safer and more efficient code. By understanding and using its features effectively, you can improve your development experience and the quality of your code.

You can stop using type `any` now.

---

## References

- www.typescriptlang.org. (n.d.). Handbook - The TypeScript Handbook. [online] Available at: https://www.typescriptlang.org/docs/handbook/intro.html.
