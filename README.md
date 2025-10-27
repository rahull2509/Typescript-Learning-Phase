# TypeScript Learning Guide 🚀

## Table of Contents
- [Core Type System](#core-type-system)
- [Type Inference & Type Annotations](#type-inference--type-annotations)
- [Interfaces & Type Aliases](#interfaces--type-aliases)
- [Classes & Object-Oriented Programming](#classes--object-oriented-programming)
- [Functions in TypeScript](#functions-in-typescript)
- [Generics](#generics)
- [Modules](#modules)
- [Type Assertions & Type Guards](#type-assertions--type-guards)
- [Utility Types](#utility-types)
- [Advanced Types](#advanced-types)
- [Decorators](#decorators)
- [Declaration Files](#declaration-files)

---

## ⚡ Core Type System

### Primitive Types

**Kya hai?** Basic data types jo JavaScript mein already hain.

```typescript
// String - text data
let naam: string = "Rahul";
let city: string = 'Mumbai';

// Number - sabhi numbers
let age: number = 25;
let price: number = 99.99;

// Boolean - true/false
let isActive: boolean = true;

// Null & Undefined
let data: null = null;
let value: undefined = undefined;
```

### The `any` and `unknown` Types

**`any`** - Kisi bhi type ko allow karta hai (Type safety lose ho jati hai).

```typescript
let randomValue: any = 10;
randomValue = "Hello";
randomValue.toUpperCase(); // Koi error nahi (dangerous!)
```

**`unknown`** - Safer alternative, use se pehle type check zaroori.

```typescript
let userInput: unknown = "Hello";

// Type check karna padega
if (typeof userInput === "string") {
    console.log(userInput.toUpperCase()); // Safe
}
```

### The `never` Type

**Kya hai?** Never type us value ko represent karta hai jo kabhi nahi aati.

```typescript
// Function jo kabhi return nahi karta (infinite loop)
function infiniteLoop(): never {
    while (true) {}
}

// Function jo hamesha error throw karta hai
function throwError(message: string): never {
    throw new Error(message);
}

// Never in union types - automatically remove ho jata hai
type Result = string | never; // Basically: string

// Exhaustive checks
type Shape = "circle" | "square";

function getArea(shape: Shape): number {
    switch (shape) {
        case "circle":
            return 10;
        case "square":
            return 20;
        default:
            const _exhaustive: never = shape; // Compile error agar naya case add ho
            return _exhaustive;
    }
}
```

### Union and Intersection Types

**Union Types** - Multiple types mein se ek (OR logic).

```typescript
let id: string | number;
id = 101; // Valid
id = "ABC123"; // Valid

function printId(id: string | number) {
    console.log("ID: " + id);
}
```

**Intersection Types** - Multiple types combine karna (AND logic).

```typescript
interface Person {
    name: string;
    age: number;
}

interface Employee {
    empId: number;
    department: string;
}

type Staff = Person & Employee;

let staff: Staff = {
    name: "Amit",
    age: 30,
    empId: 1001,
    department: "IT"
};
```

### Literal Types

**Kya hai?** Specific values ko type ki tarah use karna.

```typescript
// String literal types
let direction: "left" | "right" | "up" | "down";
direction = "left"; // Valid
// direction = "forward"; // Error!

// Numeric literal types
let diceRoll: 1 | 2 | 3 | 4 | 5 | 6;
diceRoll = 4; // Valid

// Boolean literal
let isTrue: true;
isTrue = true; // Valid only

// Practical example
type HttpMethod = "GET" | "POST" | "PUT" | "DELETE";

function makeRequest(url: string, method: HttpMethod) {
    console.log(`${method} request to ${url}`);
}

makeRequest("/api/users", "GET");
makeRequest("/api/users", "POST");
// makeRequest("/api/users", "PATCH"); // Error!

// Object literal types
type ButtonConfig = {
    type: "primary" | "secondary" | "danger";
    size: "small" | "medium" | "large";
};

let button: ButtonConfig = {
    type: "primary",
    size: "large"
};
```

### Arrays and Tuples

**Arrays:**
```typescript
let numbers: number[] = [1, 2, 3, 4, 5];
let fruits: Array<string> = ["Apple", "Banana"];
let mixed: (string | number)[] = [1, "two", 3];
```

**Tuples:**
```typescript
// Fixed length, fixed types
let person: [string, number] = ["Rahul", 25];
let coordinate: [number, number] = [10.5, 20.3];

// Named tuples (TypeScript 4.0+)
let user: [name: string, age: number, active: boolean] = ["Priya", 28, true];

// Optional tuple elements
let point: [number, number, number?] = [10, 20]; // Third element optional

// Rest in tuples
let scores: [string, ...number[]] = ["Math", 90, 85, 92];
```

### Enums

**Kya hai?** Named constants ka collection.

```typescript
// Numeric Enum
enum Direction {
    Up,      // 0
    Down,    // 1
    Left,    // 2
    Right    // 3
}

// String Enum
enum Status {
    Pending = "PENDING",
    Approved = "APPROVED",
    Rejected = "REJECTED"
}

let orderStatus: Status = Status.Approved;

// Const Enum (compile time mein replace ho jata hai)
const enum Colors {
    Red = "RED",
    Green = "GREEN",
    Blue = "BLUE"
}

let color = Colors.Red; // Compiled code mein "RED" ho jayega
```

### Type Aliases

**Kya hai?** Custom type names create karna.

```typescript
type ID = string | number;
type UserID = ID;

type User = {
    name: string;
    age: number;
    email: string;
};

type GreetFunction = (name: string) => string;

let greet: GreetFunction = (name) => `Hello, ${name}!`;
```

---

## ⚡ Type Inference & Type Annotations

### Type Inference

**Kya hai?** TypeScript khud type detect karta hai.

```typescript
let score = 100; // Type: number (automatic)
let message = "Hello"; // Type: string
let isValid = true; // Type: boolean

// Function return type inference
function add(a: number, b: number) {
    return a + b; // Return type: number (inferred)
}

// Best common type
let mixed = Math.random() > 0.5 ? 10 : "ten"; // Type: number | string
```

### Type Annotations

**Kya hai?** Explicitly type define karna.

```typescript
let username: string = "Ajay";
let age: number = 25;

// Function with annotations
function greet(name: string, age: number): string {
    return `Hello ${name}, you are ${age} years old`;
}

// Object annotation
let person: {
    name: string;
    age: number;
} = {
    name: "Priya",
    age: 28
};
```

### When to Use Inference vs Annotations

**Inference use karo:**
- Simple assignments
- Type obvious ho

**Annotations use karo:**
- Function parameters (mandatory)
- Variable without initial value
- Specific type enforce karna ho

```typescript
// Inference sufficient
let count = 0;

// Annotation needed
let userId: number;
userId = 101;

function calculate(price: number, tax: number): number {
    return price + tax;
}
```

---

## ⚡ Interfaces & Type Aliases

### Defining Interfaces

**Kya hai?** Object ki shape define karna.

```typescript
interface User {
    name: string;
    age: number;
    email: string;
}

let user: User = {
    name: "Amit",
    age: 25,
    email: "amit@example.com"
};

// Optional properties
interface Product {
    id: number;
    name: string;
    description?: string; // Optional
}

// Readonly properties
interface Config {
    readonly apiKey: string;
    timeout?: number;
}

// Function types in interface
interface MathOperation {
    (x: number, y: number): number;
}

let add: MathOperation = (a, b) => a + b;
```

### Index Signatures

**Kya hai?** Dynamic property names allow karna.

```typescript
// String index signature
interface StringDictionary {
    [key: string]: string;
}

let dict: StringDictionary = {
    name: "Raj",
    city: "Mumbai",
    country: "India"
};

// Number index signature
interface NumberArray {
    [index: number]: string;
}

let names: NumberArray = ["Raj", "Amit", "Priya"];

// Mixed with known properties
interface UserData {
    id: number;
    name: string;
    [key: string]: any; // Additional properties allowed
}

let userData: UserData = {
    id: 1,
    name: "Raj",
    age: 25,
    city: "Mumbai"
};

// Record utility type (better alternative)
type StringRecord = Record<string, string>;
```

### Extending Interfaces

```typescript
interface Person {
    name: string;
    age: number;
}

interface Employee extends Person {
    empId: number;
    department: string;
}

let employee: Employee = {
    name: "Ravi",
    age: 30,
    empId: 1001,
    department: "Sales"
};

// Multiple inheritance
interface Animal {
    name: string;
}

interface CanFly {
    wingSpan: number;
}

interface Bird extends Animal, CanFly {
    species: string;
}
```

### Type Aliases vs Interfaces

```typescript
// Type alias - unions, intersections, primitives
type ID = string | number;
type Point = { x: number; y: number; };

// Interface - extending, declaration merging
interface User {
    name: string;
}

interface User {
    age: number; // Merges with above
}

// Intersection types
type Admin = { role: string } & User;

// Key differences:
// 1. Interface can be reopened (declaration merging)
// 2. Type alias can use unions/intersections
// 3. Interface better for object shapes
// 4. Type alias more flexible
```

---

## ⚡ Classes & Object-Oriented Programming

### Classes and Objects

```typescript
class Person {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    greet() {
        console.log(`Hello, ${this.name}`);
    }
}

let person = new Person("Rahul", 25);
person.greet();
```

### Access Modifiers

```typescript
class BankAccount {
    public accountHolder: string;      // Anywhere accessible
    private balance: number;           // Only inside class
    protected accountType: string;     // Class + child classes

    constructor(holder: string, balance: number) {
        this.accountHolder = holder;
        this.balance = balance;
        this.accountType = "Savings";
    }

    public deposit(amount: number) {
        this.balance += amount;
    }

    private calculateInterest() {
        return this.balance * 0.04;
    }

    public getBalance() {
        return this.balance;
    }
}
```

### Readonly Properties

```typescript
class Book {
    readonly isbn: string;
    readonly title: string;

    constructor(isbn: string, title: string) {
        this.isbn = isbn;
        this.title = title;
    }
}

let book = new Book("978-123", "TypeScript Guide");
// book.isbn = "new"; // Error!
```

### Parameter Properties (Shorthand)

```typescript
// Traditional way
class Student1 {
    name: string;
    rollNo: number;

    constructor(name: string, rollNo: number) {
        this.name = name;
        this.rollNo = rollNo;
    }
}

// Shorthand with parameter properties
class Student2 {
    constructor(
        public name: string,
        public rollNo: number,
        private marks: number
    ) {}
}

let student = new Student2("Vikram", 101, 85);
```

### Getters and Setters

```typescript
class Employee {
    private _salary: number;

    constructor(private name: string, salary: number) {
        this._salary = salary;
    }

    get salary(): number {
        return this._salary;
    }

    set salary(newSalary: number) {
        if (newSalary < 0) {
            console.log("Invalid salary!");
            return;
        }
        this._salary = newSalary;
    }

    get annualSalary(): number {
        return this._salary * 12;
    }
}

let emp = new Employee("Arun", 50000);
console.log(emp.salary); // Getter
emp.salary = 55000; // Setter
```

### Static Members

```typescript
class MathHelper {
    static PI: number = 3.14159;
    
    static calculateCircleArea(radius: number): number {
        return this.PI * radius * radius;
    }
}

console.log(MathHelper.PI);
console.log(MathHelper.calculateCircleArea(5));

// Counter example
class Counter {
    static count: number = 0;

    constructor() {
        Counter.count++;
    }

    static getCount() {
        return Counter.count;
    }
}

new Counter();
new Counter();
console.log(Counter.getCount()); // 2
```

### Abstract Classes

```typescript
abstract class Animal {
    constructor(public name: string) {}

    move(): void {
        console.log(`${this.name} is moving`);
    }

    abstract makeSound(): void; // Must be implemented by child
}

class Dog extends Animal {
    makeSound(): void {
        console.log(`${this.name} barks`);
    }
}

// let animal = new Animal("Generic"); // Error!
let dog = new Dog("Tommy");
dog.makeSound();
```

### Implementing Interfaces

```typescript
interface Printable {
    print(): void;
}

interface Loggable {
    log(message: string): void;
}

class Document implements Printable, Loggable {
    constructor(private content: string) {}

    print(): void {
        console.log(this.content);
    }

    log(message: string): void {
        console.log(`Log: ${message}`);
    }
}
```

---

## ⚡ Functions in TypeScript

### Function Types

```typescript
// Function type definition
let add: (a: number, b: number) => number;

add = (x, y) => x + y;

// Type alias for functions
type MathOperation = (a: number, b: number) => number;

let multiply: MathOperation = (x, y) => x * y;
```

### Optional & Default Parameters

```typescript
// Optional parameters
function greet(name: string, greeting?: string) {
    return greeting ? `${greeting}, ${name}` : `Hello, ${name}`;
}

// Default parameters
function createUser(name: string, age: number = 18, city = "Mumbai") {
    return { name, age, city };
}

console.log(createUser("Amit"));
console.log(createUser("Priya", 25, "Delhi"));
```

### Rest Parameters

```typescript
function sum(...numbers: number[]): number {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3)); // 6
console.log(sum(10, 20, 30, 40)); // 100

// Mixed parameters
function announce(title: string, ...points: string[]) {
    console.log(title);
    points.forEach(p => console.log(`- ${p}`));
}
```

### Function Overloads

```typescript
// Overload signatures
function processInput(input: string): string;
function processInput(input: number): number;
function processInput(input: boolean): string;

// Implementation
function processInput(input: string | number | boolean): string | number {
    if (typeof input === "string") return input.toUpperCase();
    if (typeof input === "number") return input * 2;
    return input ? "YES" : "NO";
}

console.log(processInput("hello")); // "HELLO"
console.log(processInput(5)); // 10
```

### Arrow Functions

```typescript
// Arrow function syntax
const add = (a: number, b: number): number => a + b;

const greet = (name: string): string => {
    return `Hello, ${name}`;
};

// 'this' context preservation
class Handler {
    message = "Hello";

    // Arrow function preserves 'this'
    delayedGreet = () => {
        setTimeout(() => {
            console.log(this.message);
        }, 1000);
    }
}
```

---

## ⚡ Generics

### Generic Functions

```typescript
// Generic function
function identity<T>(value: T): T {
    return value;
}

let num = identity<number>(42);
let str = identity<string>("Hello");
let auto = identity(100); // Type inference

// Array operations
function getFirstElement<T>(arr: T[]): T | undefined {
    return arr[0];
}

// Multiple type parameters
function pair<T, U>(first: T, second: U): [T, U] {
    return [first, second];
}

let nameAge = pair("Raj", 25);

// Generic with constraints
interface HasLength {
    length: number;
}

function logLength<T extends HasLength>(item: T): void {
    console.log(item.length);
}

logLength("Hello");
logLength([1, 2, 3]);
```

### Generic Interfaces

```typescript
interface Container<T> {
    value: T;
    getValue(): T;
    setValue(val: T): void;
}

let numberContainer: Container<number> = {
    value: 100,
    getValue() { return this.value; },
    setValue(val) { this.value = val; }
};

// API Response interface
interface ApiResponse<T> {
    data: T;
    status: number;
    message: string;
}

interface User {
    id: number;
    name: string;
}

let response: ApiResponse<User> = {
    data: { id: 1, name: "Raj" },
    status: 200,
    message: "Success"
};
```

### Generic Classes

```typescript
class Box<T> {
    constructor(private content: T) {}

    getContent(): T {
        return this.content;
    }

    setContent(value: T): void {
        this.content = value;
    }
}

let numberBox = new Box<number>(123);
let stringBox = new Box<string>("Hello");

// Generic Stack
class Stack<T> {
    private items: T[] = [];

    push(item: T): void {
        this.items.push(item);
    }

    pop(): T | undefined {
        return this.items.pop();
    }

    peek(): T | undefined {
        return this.items[this.items.length - 1];
    }
}

let numberStack = new Stack<number>();
numberStack.push(1);
numberStack.push(2);
```

### Generic Constraints

```typescript
// Constraint using interface
interface HasId {
    id: number;
}

function findById<T extends HasId>(items: T[], id: number): T | undefined {
    return items.find(item => item.id === id);
}

// Constraint using keyof
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

let user = { id: 1, name: "Raj", email: "raj@example.com" };
let name = getProperty(user, "name"); // Valid
// let invalid = getProperty(user, "phone"); // Error!
```

---

## ⚡ Modules

### Importing and Exporting

**Named Exports:**
```typescript
// math.ts
export function add(a: number, b: number): number {
    return a + b;
}

export const PI = 3.14159;

export class Calculator {
    multiply(a: number, b: number): number {
        return a * b;
    }
}

// app.ts
import { add, PI, Calculator } from './math';

// Alias
import { add as sum } from './math';

// Import all
import * as MathUtils from './math';
```

**Default Exports:**
```typescript
// user.ts
export default class User {
    constructor(public name: string) {}
}

// app.ts
import User from './user';

// Mixed exports
export default function main() {}
export function helper() {}

import mainFn, { helper } from './utils';
```

### Re-exporting

```typescript
// models/index.ts
export { User } from './user';
export { Product } from './product';
export * from './common';

// app.ts
import { User, Product } from './models';
```

---

## ⚡ Type Assertions & Type Guards

### Type Assertion

```typescript
// 'as' syntax (recommended)
let someValue: unknown = "Hello";
let strLength = (someValue as string).length;

// DOM example
let input = document.getElementById('username') as HTMLInputElement;
input.value = "Raj";

// Non-null assertion (!)
let value = getValue()!; // Guarantee it's not null
```

### Type Guards

**typeof:**
```typescript
function processValue(value: string | number) {
    if (typeof value === "string") {
        console.log(value.toUpperCase());
    } else {
        console.log(value.toFixed(2));
    }
}
```

**instanceof:**
```typescript
class Dog {
    bark() { console.log("Bhow!"); }
}

class Cat {
    meow() { console.log("Meow!"); }
}

function makeSound(animal: Dog | Cat) {
    if (animal instanceof Dog) {
        animal.bark();
    } else {
        animal.meow();
    }
}
```

**in operator:**
```typescript
interface Admin {
    name: string;
    privileges: string[];
}

interface User {
    name: string;
    email: string;
}

function printInfo(person: Admin | User) {
    console.log(person.name);
    
    if ('privileges' in person) {
        console.log(person.privileges);
    }
    
    if ('email' in person) {
        console.log(person.email);
    }
}
```

**Custom Type Guards:**
```typescript
interface Car {
    drive(): void;
}

interface Boat {
    sail(): void;
}

function isCar(vehicle: Car | Boat): vehicle is Car {
    return (vehicle as Car).drive !== undefined;
}

function operate(vehicle: Car | Boat) {
    if (isCar(vehicle)) {
        vehicle.drive();
    } else {
        vehicle.sail();
    }
}
```

---

## ⚡ Utility Types

### `Partial<T>`

Sabhi properties ko optional bana deta hai.

```typescript
interface User {
    id: number;
    name: string;
    email: string;
}

type PartialUser = Partial<User>;

function updateUser(id: number, updates: Partial<User>) {
    console.log(`Updating user ${id}`, updates);
}

updateUser(1, { name: "Raj" });
updateUser(2, { email: "new@example.com" });
```

### `Required<T>`

Sabhi properties ko mandatory bana deta hai.

```typescript
interface Config {
    host?: string;
    port?: number;
}

type RequiredConfig = Required<Config>;

let config: RequiredConfig = {
    host: "localhost",
    port: 3000 // Dono zaroori
};
```

### `Readonly<T>`

Sabhi properties ko readonly bana deta hai.

```typescript
interface User {
    id: number;
    name: string;
}

type ReadonlyUser = Readonly<User>;

let user: ReadonlyUser = {
    id: 1,
    name: "Raj"
};

// user.name = "New"; // Error!
```

### `Pick<T, K>`

Specific properties select karna.

```typescript
interface User {
    id: number;
    name: string;
    email: string;
    age: number;
}

type UserPreview = Pick<User, 'id' | 'name'>;

let preview: UserPreview = {
    id: 1,
    name: "Raj"
};
```

### `Omit<T, K>`

Specific properties remove karna.

```typescript
interface User {
    id: number;
    name: string;
    email: string;
    password: string;
}

type UserWithoutPassword = Omit<User, 'password'>;

let safeUser: UserWithoutPassword = {
    id: 1,
    name: "Raj",
    email: "raj@example.com"
};
```

### `Record<K, T>`

Key-value pairs ka type create karna.

```typescript
type Role = 'admin' | 'user' | 'guest';

type RolePermissions = Record<Role, string[]>;

let permissions: RolePermissions = {
    admin: ['create', 'read', 'update', 'delete'],
    user: ['read', 'update'],
    guest: ['read']
};

// Dictionary pattern
type StringDictionary = Record<string, string>;

let dict: StringDictionary = {
    name: "Raj",
    city: "Mumbai"
};
```

### `Exclude<T, U>` & `Extract<T, U>`

```typescript
// Exclude - Remove types from union
type T1 = Exclude<'a' | 'b' | 'c', 'a'>; // 'b' | 'c'
type T2 = Exclude<string | number | boolean, boolean>; // string | number

// Extract - Keep only matching types
type T3 = Extract<'a' | 'b' | 'c', 'a' | 'f'>; // 'a'
type T4 = Extract<string | number | boolean, boolean>; // boolean
```

### `NonNullable<T>`

Null aur undefined remove karna.

```typescript
type T1 = NonNullable<string | number | null | undefined>; // string | number

function process(value: string | null | undefined) {
    let safe: NonNullable<typeof value> = value!;
}
```

### `ReturnType<T>` & `Parameters<T>`

```typescript
// ReturnType - function ki return type nikalna
function getUser() {
    return { id: 1, name: "Raj" };
}

type User = ReturnType<typeof getUser>; // { id: number; name: string }

// Parameters - function ke parameters ki tuple type
function createUser(name: string, age: number) {
    return { name, age };
}

type CreateUserParams = Parameters<typeof createUser>; // [string, number]

let params: CreateUserParams = ["Raj", 25];
```

---

## ⚡ Advanced Types

### Discriminated Unions

**Kya hai?** Common property (discriminant) se type differentiate karna.

```typescript
interface Circle {
    kind: "circle"; // Discriminant
    radius: number;
}

interface Square {
    kind: "square"; // Discriminant
    sideLength: number;
}

interface Rectangle {
    kind: "rectangle"; // Discriminant
    width: number;
    height: number;
}

type Shape = Circle | Square | Rectangle;

function getArea(shape: Shape): number {
    switch (shape.kind) {
        case "circle":
            return Math.PI * shape.radius ** 2;
        case "square":
            return shape.sideLength ** 2;
        case "rectangle":
            return shape.width * shape.height;
        default:
            const _exhaustive: never = shape;
            return _exhaustive;
    }
}

// Practical example - API responses
interface SuccessResponse {
    status: "success";
    data: any;
}

interface ErrorResponse {
    status: "error";
    error: string;
}

type ApiResponse = SuccessResponse | ErrorResponse;

function handleResponse(response: ApiResponse) {
    if (response.status === "success") {
        console.log(response.data);
    } else {
        console.error(response.error);
    }
}
```

### Mapped Types

**Kya hai?** Existing type se naya type banana by transforming properties.

```typescript
// Basic mapped type
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

type Optional<T> = {
    [P in keyof T]?: T[P];
};

// Practical example
interface User {
    id: number;
    name: string;
    email: string;
}

type ReadonlyUser = Readonly<User>;
type OptionalUser = Optional<User>;

// Custom mapped type - make all properties nullable
type Nullable<T> = {
    [P in keyof T]: T[P] | null;
};

type NullableUser = Nullable<User>;

// Add prefix to keys
type Getters<T> = {
    [P in keyof T as `get${Capitalize<string & P>}`]: () => T[P];
};

type UserGetters = Getters<User>;
// {
//     getId: () => number;
//     getName: () => string;
//     getEmail: () => string;
// }
```

### Conditional Types

**Kya hai?** Condition ke basis par type decide karna.

```typescript
// Basic syntax: T extends U ? X : Y

type IsString<T> = T extends string ? true : false;

type T1 = IsString<string>; // true
type T2 = IsString<number>; // false

// Extract array element type
type ElementType<T> = T extends (infer U)[] ? U : T;

type T3 = ElementType<string[]>; // string
type T4 = ElementType<number>; // number

// Practical example - extract return type
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

function getUser() {
    return { id: 1, name: "Raj" };
}

type User = ReturnType<typeof getUser>;

// Nested conditional types
type TypeName<T> = 
    T extends string ? "string" :
    T extends number ? "number" :
    T extends boolean ? "boolean" :
    T extends undefined ? "undefined" :
    T extends Function ? "function" :
    "object";

type T5 = TypeName<string>; // "string"
type T6 = TypeName<number>; // "number"

// Distributive conditional types
type ToArray<T> = T extends any ? T[] : never;

type T7 = ToArray<string | number>; // string[] | number[]
```

### Template Literal Types

**Kya hai?** String literal types ko combine karke naye types banana.

```typescript
// Basic template literal type
type Greeting = `Hello ${string}`;

let greet1: Greeting = "Hello World"; // Valid
let greet2: Greeting = "Hello TypeScript"; // Valid
// let greet3: Greeting = "Hi World"; // Error!

// Combining literal types
type Direction = "left" | "right" | "top" | "bottom";
type Padding = `padding-${Direction}`;

let padding: Padding = "padding-left"; // Valid

// Practical example - CSS properties
type Size = "small" | "medium" | "large";
type Color = "red" | "blue" | "green";

type ButtonClass = `btn-${Size}-${Color}`;

let btnClass: ButtonClass = "btn-small-red";

// Uppercase/Lowercase/Capitalize/Uncapitalize
type UpperGreeting = Uppercase<"hello">; // "HELLO"
type LowerGreeting = Lowercase<"HELLO">; // "hello"
type CapitalGreeting = Capitalize<"hello">; // "Hello"
type UncapitalGreeting = Uncapitalize<"Hello">; // "hello"

// Event handler names
type EventName = "click" | "scroll" | "mousemove";
type EventHandler = `on${Capitalize<EventName>}`;

let handler: EventHandler = "onClick"; // Valid

// Object with template literal keys
type PropEventSource<T> = {
    on<K extends string & keyof T>(
        eventName: `${K}Changed`,
        callback: (newValue: T[K]) => void
    ): void;
};

declare function makeWatchedObject<T>(obj: T): T & PropEventSource<T>;

let person = makeWatchedObject({
    firstName: "Raj",
    age: 25
});

person.on("firstNameChanged", (newName) => {
    console.log(`New name: ${newName}`);
});
```

---

## ⚡ `keyof` Operator

**Kya hai?** Object type ki sabhi keys ka union type nikalna.

```typescript
interface User {
    id: number;
    name: string;
    email: string;
}

type UserKeys = keyof User; // "id" | "name" | "email"

let key: UserKeys = "name"; // Valid
// let invalid: UserKeys = "age"; // Error!

// Function with keyof
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

let user = { id: 1, name: "Raj", email: "raj@example.com" };
let name = getProperty(user, "name"); // Type: string
let id = getProperty(user, "id"); // Type: number

// typeof with keyof
const ROLES = {
    ADMIN: "admin",
    USER: "user",
    GUEST: "guest"
} as const;

type RoleKeys = keyof typeof ROLES; // "ADMIN" | "USER" | "GUEST"
type RoleValues = typeof ROLES[RoleKeys]; // "admin" | "user" | "guest"

// Index access types
type User = {
    id: number;
    name: string;
    address: {
        street: string;
        city: string;
    }
};

type AddressType = User['address']; // { street: string; city: string }
type IdType = User['id']; // number
```

### Type Compatibility

**Kya hai?** TypeScript mein types kab compatible hain.

```typescript
// Structural typing - shape matters, not name
interface Point2D {
    x: number;
    y: number;
}

interface Point3D {
    x: number;
    y: number;
    z: number;
}

let point2D: Point2D = { x: 0, y: 0 };
let point3D: Point3D = { x: 0, y: 0, z: 0 };

point2D = point3D; // Valid - 3D has all properties of 2D
// point3D = point2D; // Error - 2D doesn't have z

// Function compatibility
type Logger = (message: string) => void;

let log: Logger = (msg: string) => {
    console.log(msg);
};

// More parameters allowed in implementation
let logWithTime: Logger = (msg: string, timestamp: Date) => {
    console.log(`[${timestamp}] ${msg}`);
};

log = logWithTime; // Valid

// Return type compatibility
type NumberProducer = () => number;
type ValueProducer = () => number | string;

let numProducer: NumberProducer = () => 42;
let valueProducer: ValueProducer = () => 42;

valueProducer = numProducer; // Valid - number is assignable to number | string
// numProducer = valueProducer; // Error
```

---

## ⚡ Decorators

**Kya hai?** Special declarations jo classes, methods, properties ko modify karte hain. (Experimental feature - `experimentalDecorators: true` needed in tsconfig)

### Class Decorators

```typescript
// Class decorator function
function sealed(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}

@sealed
class BugReport {
    type = "report";
    title: string;

    constructor(title: string) {
        this.title = title;
    }
}

// Decorator factory (returns decorator)
function Logger(logString: string) {
    return function(constructor: Function) {
        console.log(logString);
        console.log(constructor);
    };
}

@Logger('LOGGING - USER')
class User {
    constructor(public name: string) {}
}

// Practical example - adding metadata
function Component(options: { selector: string; template: string }) {
    return function<T extends { new(...args: any[]): {} }>(constructor: T) {
        return class extends constructor {
            selector = options.selector;
            template = options.template;
        };
    };
}

@Component({
    selector: 'app-user',
    template: '<div>User Component</div>'
})
class UserComponent {
    constructor(public name: string) {}
}
```

### Method Decorators

```typescript
// Method decorator
function Log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;

    descriptor.value = function(...args: any[]) {
        console.log(`Calling ${propertyKey} with args:`, args);
        const result = originalMethod.apply(this, args);
        console.log(`Result:`, result);
        return result;
    };
}

class Calculator {
    @Log
    add(a: number, b: number): number {
        return a + b;
    }
}

let calc = new Calculator();
calc.add(2, 3);
// Output:
// Calling add with args: [2, 3]
// Result: 5
```

### Property Decorators

```typescript
function MinLength(length: number) {
    return function(target: any, propertyKey: string) {
        let value: string;

        const getter = () => value;
        const setter = (newVal: string) => {
            if (newVal.length < length) {
                throw new Error(`${propertyKey} must be at least ${length} characters`);
            }
            value = newVal;
        };

        Object.defineProperty(target, propertyKey, {
            get: getter,
            set: setter
        });
    };
}

class User {
    @MinLength(3)
    username: string;

    constructor(username: string) {
        this.username = username;
    }
}

// let user = new User("ab"); // Error!
let user = new User("rahul"); // Valid
```

### Parameter Decorators

```typescript
function Required(target: Object, propertyKey: string, parameterIndex: number) {
    console.log(`Parameter ${parameterIndex} in ${propertyKey} is required`);
}

class User {
    greet(@Required name: string) {
        console.log(`Hello, ${name}`);
    }
}
```

---

## ⚡ Declaration Files (.d.ts)

**Kya hai?** Type declarations for JavaScript libraries. `.d.ts` files types provide karti hain bina implementation ke.

### Creating Declaration Files

```typescript
// mathUtils.d.ts
export function add(a: number, b: number): number;
export function subtract(a: number, b: number): number;
export const PI: number;

export class Calculator {
    constructor();
    multiply(a: number, b: number): number;
    divide(a: number, b: number): number;
}

// Using in your code
import { add, PI, Calculator } from './mathUtils';

let result = add(5, 3);
console.log(PI);
```

### Ambient Declarations

```typescript
// global.d.ts
declare const API_URL: string;
declare const DEBUG: boolean;

declare function globalFunction(param: string): void;

declare namespace MyLibrary {
    function doSomething(): void;
    function doSomethingElse(): void;
}

// Using in code
console.log(API_URL);
MyLibrary.doSomething();
```

### Module Declarations

```typescript
// declarations.d.ts

// For libraries without types
declare module 'some-library' {
    export function someFunction(param: string): number;
    export class SomeClass {
        constructor(value: string);
        method(): void;
    }
}

// For CSS modules
declare module '*.css' {
    const content: { [className: string]: string };
    export default content;
}

// For image imports
declare module '*.png' {
    const value: string;
    export default value;
}

// Using in code
import { someFunction } from 'some-library';
import styles from './App.css';
import logo from './logo.png';
```

### Triple-Slash Directives

```typescript
/// <reference path="./types.d.ts" />
/// <reference types="node" />

// types.d.ts ko include karta hai
// node types automatically include karta hai
```

---

## ⚡ Namespaces

**Kya hai?** Code organization ka purana tarika (modules prefer kiye jate hain modern code mein).

```typescript
namespace Validation {
    export interface StringValidator {
        isValid(s: string): boolean;
    }

    export class EmailValidator implements StringValidator {
        isValid(s: string): boolean {
            return s.includes('@');
        }
    }

    export class PhoneValidator implements StringValidator {
        isValid(s: string): boolean {
            return /^\d{10}$/.test(s);
        }
    }
}

// Usage
let emailValidator = new Validation.EmailValidator();
console.log(emailValidator.isValid("test@example.com")); // true

// Nested namespaces
namespace App {
    export namespace Models {
        export class User {
            constructor(public name: string) {}
        }
    }

    export namespace Services {
        export class UserService {
            getUser() {
                return new Models.User("Raj");
            }
        }
    }
}

let userService = new App.Services.UserService();
```

---

## 📚 Best Practices

### When to Use What

```typescript
// ✅ Use Interface for object shapes
interface User {
    name: string;
    age: number;
}

// ✅ Use Type for unions/intersections
type ID = string | number;
type Status = 'active' | 'inactive';

// ✅ Use Enum for fixed set of related values
enum Direction {
    Up, Down, Left, Right
}

// ✅ Use const assertions for readonly values
const ROLES = {
    ADMIN: 'admin',
    USER: 'user'
} as const;

// ✅ Use unknown instead of any for type-safe code
function processData(data: unknown) {
    if (typeof data === 'string') {
        // Safe to use as string
    }
}

// ✅ Use generics for reusable components
function identity<T>(value: T): T {
    return value;
}

// ✅ Use utility types to transform types
type ReadonlyUser = Readonly<User>;
type PartialUser = Partial<User>;
```

### Type Safety Tips

```typescript
// ✅ Enable strict mode in tsconfig.json
{
    "compilerOptions": {
        "strict": true,
        "strictNullChecks": true,
        "noImplicitAny": true
    }
}

// ✅ Avoid 'any' - use 'unknown' if needed
// ❌ Bad
function process(data: any) { }

// ✅ Good
function process(data: unknown) {
    if (typeof data === 'string') {
        // Type-safe
    }
}

// ✅ Use const assertions for literal types
// ❌ Type: string
let status1 = "pending";

// ✅ Type: "pending"
let status2 = "pending" as const;

// ✅ Use discriminated unions for complex types
type Shape = 
    | { kind: 'circle'; radius: number }
    | { kind: 'square'; size: number };
```

---

## 🎯 Summary Cheat Sheet

```typescript
// Types
let str: string = "hello";
let num: number = 42;
let bool: boolean = true;
let arr: number[] = [1, 2, 3];
let tuple: [string, number] = ["hello", 42];
let any: any = "anything";
let unknown: unknown = "safer than any";
let never: never; // Function that never returns

// Union & Intersection
type ID = string | number; // Union
type Admin = User & { role: string }; // Intersection

// Interfaces
interface User {
    name: string;
    age?: number; // Optional
    readonly id: number; // Readonly
}

// Functions
function greet(name: string): string {
    return `Hello ${name}`;
}

const add = (a: number, b: number): number => a + b;

// Generics
class Box<T> {
    constructor(private content: T) {}
}

// Utility Types
Partial<T>     // All properties optional
Required<T>    // All properties required
Readonly<T>    // All properties readonly
Pick<T, K>     // Select properties
Omit<T, K>     // Remove properties
Record<K, T>   // Key-value pairs

// Type Guards
typeof value === "string"
value instanceof ClassName
'property' in object

// Assertions
value as Type
value!  // Non-null assertion
```

---

Happy Learning! 🚀 TypeScript master bano! 💪
