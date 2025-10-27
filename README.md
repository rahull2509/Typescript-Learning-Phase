# TypeScript Learning Guide - Day 2 to Day 6 🚀
## Table of Contents
- [Day 2 - Core Type System](#day-2---core-type-system)
- [Day 3 - Type Inference & Type Annotations](#day-3---type-inference--type-annotations)
- [Day 4 - Interfaces & Type Aliases (Advanced)](#day-4---interfaces--type-aliases-advanced)
- [Day 5 - Classes & Object-Oriented Concepts](#day-5---classes--object-oriented-concepts)
- [Day 6 - Advanced TypeScript Concepts](#day-6---advanced-typescript-concepts)

---

## ⚡ Day 2 - Core Type System

### Primitive Types

**Kya hai?** Primitive types wo basic data types hain jo JavaScript mein already hain - string, number, boolean, null, aur undefined.

```typescript
// String - text data ke liye
let naam: string = "Rahul";
let city: string = 'Mumbai';

// Number - sabhi numbers (integers, decimals, etc.)
let age: number = 25;
let price: number = 99.99;

// Boolean - true ya false values
let isActive: boolean = true;
let hasCompleted: boolean = false;

// Null - intentionally empty value
let data: null = null;

// Undefined - value assign nahi hui
let value: undefined = undefined;
```

### The `any` and `unknown` types

**`any` type:** Jab aapko type pata nahi ho ya kisi bhi type ki value allow karni ho.

```typescript
let randomValue: any = 10;
randomValue = "Hello"; // Koi error nahi
randomValue = true; // Ye bhi chalega
randomValue.toUpperCase(); // TypeScript complain nahi karega (dangerous!)
```

**`unknown` type:** Ye bhi kisi bhi type ko accept karta hai, but safer hai kyunki use karne se pehle type check karna padta hai.

```typescript
let userInput: unknown = 10;
userInput = "Hello";

// Error aayega - type check karna zaroori hai
// userInput.toUpperCase(); 

// Sahi tarika - pehle check karo
if (typeof userInput === "string") {
    console.log(userInput.toUpperCase()); // Ab safe hai
}
```

### Union and Intersection Types

**Union Types:** Ek variable multiple types mein se ek ho sakta hai (OR logic).

```typescript
let id: string | number;
id = 101; // Valid
id = "ABC123"; // Ye bhi valid

function printId(id: string | number) {
    console.log("Your ID is: " + id);
}

printId(101);
printId("XYZ789");
```

**Intersection Types:** Multiple types ko combine karna (AND logic).

```typescript
type Person = {
    name: string;
    age: number;
};

type Employee = {
    empId: number;
    department: string;
};

// Dono types ki properties hongi
type Staff = Person & Employee;

let staff: Staff = {
    name: "Amit",
    age: 30,
    empId: 1001,
    department: "IT"
};
```

### Arrays and Tuples

**Arrays:** Same type ke elements ka collection.

```typescript
// Number array
let numbers: number[] = [1, 2, 3, 4, 5];

// String array - alternate syntax
let fruits: Array<string> = ["Apple", "Banana", "Mango"];

// Mixed types with union
let mixed: (string | number)[] = [1, "two", 3, "four"];
```

**Tuples:** Fixed length array jisme har position ki type fixed hai.

```typescript
// [string, number] tuple - pehla string, doosra number hona chahiye
let person: [string, number] = ["Rahul", 25];

// Aise galat hai
// let wrong: [string, number] = [25, "Rahul"]; // Error!

// Practical example - coordinates
let coordinate: [number, number] = [10.5, 20.3];

// Tuple with multiple types
let user: [string, number, boolean] = ["Priya", 28, true];
console.log(user[0]); // "Priya"
console.log(user[1]); // 28
```

### Enums

**Kya hai?** Named constants ka collection. Jab aapko fixed values chahiye (jaise days of week, status codes).

```typescript
// Numeric Enum - by default 0 se start hota hai
enum Direction {
    Up,      // 0
    Down,    // 1
    Left,    // 2
    Right    // 3
}

let move: Direction = Direction.Up;
console.log(move); // 0

// Custom values ke saath
enum Status {
    Pending = 1,
    Approved = 2,
    Rejected = 3
}

let orderStatus: Status = Status.Approved;

// String Enum
enum Colors {
    Red = "RED",
    Green = "GREEN",
    Blue = "BLUE"
}

let favoriteColor: Colors = Colors.Blue;
console.log(favoriteColor); // "BLUE"
```

### Type Aliases

**Kya hai?** Custom type names create karna - code ko readable aur reusable banana.

```typescript
// Simple type alias
type ID = string | number;

let userId: ID = 123;
let productId: ID = "PROD-456";

// Object type alias
type User = {
    name: string;
    age: number;
    email: string;
};

let user1: User = {
    name: "Sanjay",
    age: 32,
    email: "sanjay@example.com"
};

// Function type alias
type GreetFunction = (name: string) => string;

let greet: GreetFunction = (name) => {
    return `Hello, ${name}!`;
};
```

---

## ⚡ Day 3 - Type Inference & Type Annotations

### Type Inference - How TypeScript automatically detects types

**Kya hai?** TypeScript khud se samajh jata hai ki variable ki type kya honi chahiye initial value dekh kar.

```typescript
// TypeScript automatically samajh gaya ki ye number hai
let score = 100; // Type: number
// score = "hundred"; // Error! string assign nahi kar sakte

let message = "Hello"; // Type: string
let isValid = true; // Type: boolean

// Arrays mein bhi
let numbers = [1, 2, 3]; // Type: number[]
let names = ["Ram", "Shyam"]; // Type: string[]
```

### Type Inference in variables, functions, and return types

**Variables:**
```typescript
let x = 10; // Type: number automatically infer hua
let y = "TypeScript"; // Type: string

// Best value inference - union type ban jayega
let mixed = Math.random() > 0.5 ? 10 : "ten"; // Type: number | string
```

**Functions - Return Type Inference:**
```typescript
// TypeScript samajh jayega ki return type string hai
function getName() {
    return "Rohan";
}

// Return type number hai automatically
function add(a: number, b: number) {
    return a + b; // TypeScript jaanta hai result number hoga
}

// Multiple returns - common type inference
function getValue(flag: boolean) {
    if (flag) {
        return 42;
    }
    return "Not a number";
} // Return type: string | number
```

### Type Annotations - Explicitly defining types

**Kya hai?** Jab hum khud explicitly type define karte hain.

```typescript
// Type annotation - explicitly bata rahe hain
let username: string = "Ajay";
let age: number = 25;
let isStudent: boolean = false;

// Function parameters mein annotations zaroori hain
function greet(name: string, age: number): string {
    return `Hello ${name}, you are ${age} years old`;
}

// Object type annotation
let person: {
    name: string;
    age: number;
    city: string;
} = {
    name: "Priya",
    age: 28,
    city: "Delhi"
};

// Array annotation
let scores: number[] = [85, 90, 78];
```

### Comparing inferred vs annotated types

```typescript
// Type Inference - TypeScript khud figure out karega
let city = "Mumbai"; // Inferred as string

// Type Annotation - Hum explicitly define kar rahe hain
let country: string = "India";

// Dono same kaam karte hain, but annotation mein clarity hai

// Complex example
let user = {
    name: "Vikram",
    age: 30
}; // TypeScript infer karega: { name: string; age: number; }

// Same thing with annotation
let user2: {
    name: string;
    age: number;
} = {
    name: "Vikram",
    age: 30
};
```

### When to rely on inference vs when to use annotations

**Inference use karo jab:**
- Simple assignments ho
- Type obvious ho initial value se
- Code concise rakhna ho

```typescript
let count = 0; // Inference is fine
let items = ["apple", "banana"]; // Clear hai ki string[] hai
```

**Annotations use karo jab:**
- Function parameters define kar rahe ho (mandatory hai)
- Return type explicitly chahiye (better documentation)
- Variable declare kar rahe ho bina value ke
- Type se default value different ho sakti hai

```typescript
// Variable without initial value - annotation zaroori
let userId: number;
userId = 101;

// Function - parameters mein annotation zaroori
function calculateTotal(price: number, tax: number): number {
    return price + tax;
}

// Jab aap specific type enforce karna chahte ho
let data: string | number;
data = 100;
data = "hundred"; // Dono valid
```

---

## ⚡ Day 4 - Interfaces & Type Aliases (Advanced)

### Defining Interfaces

**Kya hai?** Interface ek contract hai jo batata hai ki object ki shape kaisi honi chahiye.

```typescript
// Basic interface definition
interface User {
    name: string;
    age: number;
    email: string;
}

// Interface use karna
let user: User = {
    name: "Amit",
    age: 25,
    email: "amit@example.com"
};

// Function mein interface use karna
function displayUser(user: User) {
    console.log(`Name: ${user.name}, Age: ${user.age}`);
}

displayUser(user);
```

### Using Interfaces to define object shapes

```typescript
// Optional properties - question mark se
interface Product {
    id: number;
    name: string;
    price: number;
    description?: string; // Optional hai
}

let product1: Product = {
    id: 1,
    name: "Laptop",
    price: 45000
    // description nahi diya - no problem
};

let product2: Product = {
    id: 2,
    name: "Phone",
    price: 20000,
    description: "Latest smartphone"
};

// Readonly properties - change nahi kar sakte
interface Config {
    readonly apiKey: string;
    readonly endpoint: string;
    timeout?: number;
}

let config: Config = {
    apiKey: "ABC123",
    endpoint: "https://api.example.com"
};

// config.apiKey = "NEW_KEY"; // Error! Readonly hai

// Function types in interface
interface MathOperation {
    (x: number, y: number): number;
}

let add: MathOperation = (a, b) => a + b;
let multiply: MathOperation = (a, b) => a * b;
```

### Extending Interfaces

**Kya hai?** Ek interface ko doosre interface se extend kar sakte hain (inheritance).

```typescript
// Base interface
interface Person {
    name: string;
    age: number;
}

// Person ko extend kar rahe hain
interface Employee extends Person {
    empId: number;
    department: string;
}

let employee: Employee = {
    name: "Ravi",
    age: 30,
    empId: 1001,
    department: "Sales"
    // Person ke saare properties + Employee ke properties
};

// Multiple interfaces extend karna
interface Animal {
    name: string;
    sound: string;
}

interface CanFly {
    wingSpan: number;
    maxAltitude: number;
}

interface Bird extends Animal, CanFly {
    species: string;
}

let eagle: Bird = {
    name: "Golden Eagle",
    sound: "Screech",
    wingSpan: 2.2,
    maxAltitude: 3000,
    species: "Aquila chrysaetos"
};
```

### Type Aliases - Using `type` keyword

**Kya hai?** `type` keyword se custom types bana sakte hain.

```typescript
// Primitive type alias
type ID = string | number;

let userId: ID = 101;
let productId: ID = "PROD_456";

// Object type alias
type Point = {
    x: number;
    y: number;
};

let coordinate: Point = { x: 10, y: 20 };

// Function type alias
type Operation = (a: number, b: number) => number;

let subtract: Operation = (a, b) => a - b;

// Union types
type Status = "pending" | "approved" | "rejected";

let orderStatus: Status = "approved";
// orderStatus = "cancelled"; // Error! Valid values mein se nahi hai

// Complex type alias
type Address = {
    street: string;
    city: string;
    pincode: number;
};

type Customer = {
    id: number;
    name: string;
    address: Address;
    status: Status;
};

let customer: Customer = {
    id: 1,
    name: "Suresh",
    address: {
        street: "MG Road",
        city: "Bangalore",
        pincode: 560001
    },
    status: "approved"
};
```

### Intersection Types - Combining multiple types

**Kya hai?** Multiple types ko combine karke ek naya type banana (`&` operator se).

```typescript
// Do types ko combine karna
type HasName = {
    name: string;
};

type HasAge = {
    age: number;
};

type Person = HasName & HasAge;

let person: Person = {
    name: "Meera",
    age: 27
    // Dono properties chahiye
};

// Practical example - Multiple capabilities combine karna
type Printable = {
    print: () => void;
};

type Scannable = {
    scan: () => void;
};

type AllInOnePrinter = Printable & Scannable;

let printer: AllInOnePrinter = {
    print: () => console.log("Printing..."),
    scan: () => console.log("Scanning...")
};

// Type aliases with intersection
type BusinessPartner = {
    name: string;
    credit: number;
};

type Contact = {
    email: string;
    phone: string;
};

type Supplier = BusinessPartner & Contact;

let supplier: Supplier = {
    name: "ABC Suppliers",
    credit: 100000,
    email: "contact@abc.com",
    phone: "9876543210"
};
```

**Interface vs Type Alias:**
```typescript
// Interface - extend kar sakte hain, declaration merging hota hai
interface User {
    name: string;
}

interface User {
    age: number; // Same name, merge ho jayega
}

let user: User = {
    name: "Raj",
    age: 25
};

// Type - extend nahi kar sakte same name se, unions/intersections better
type Admin = {
    name: string;
    role: string;
};

// type Admin = { level: number }; // Error! Redeclaration nahi kar sakte
```

---

## ⚡ Day 5 - Classes & Object-Oriented Concepts

### Classes and Objects

**Kya hai?** Class ek blueprint hai objects banane ke liye.

```typescript
// Simple class definition
class Person {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    greet() {
        console.log(`Hello, mera naam ${this.name} hai`);
    }
}

// Object creation
let person1 = new Person("Rahul", 25);
person1.greet(); // "Hello, mera naam Rahul hai"

let person2 = new Person("Priya", 28);
console.log(person2.age); // 28
```

### Constructors

**Kya hai?** Constructor wo special method hai jo object create hote time automatically call hota hai.

```typescript
class Car {
    brand: string;
    model: string;
    year: number;

    // Constructor - object initialization ke liye
    constructor(brand: string, model: string, year: number) {
        this.brand = brand;
        this.model = model;
        this.year = year;
        console.log("Nayi car bani!");
    }

    getDetails() {
        return `${this.brand} ${this.model} (${this.year})`;
    }
}

let myCar = new Car("Toyota", "Fortuner", 2023);
console.log(myCar.getDetails()); // "Toyota Fortuner (2023)"
```

### Access Modifiers

**Kya hai?** Properties aur methods ki visibility control karte hain.

```typescript
class BankAccount {
    // Public - kahin se bhi access kar sakte (default)
    public accountHolder: string;
    
    // Private - sirf class ke andar access ho sakta
    private balance: number;
    
    // Protected - class aur uski child classes mein access ho sakta
    protected accountType: string;

    constructor(holder: string, initialBalance: number) {
        this.accountHolder = holder;
        this.balance = initialBalance;
        this.accountType = "Savings";
    }

    // Public method
    public deposit(amount: number) {
        this.balance += amount;
        console.log(`${amount} rupees deposit kiye`);
    }

    // Private method - sirf class ke andar use ho sakta
    private calculateInterest() {
        return this.balance * 0.04;
    }

    public getBalance() {
        return this.balance;
    }
}

let account = new BankAccount("Amit", 5000);
console.log(account.accountHolder); // OK - public hai
account.deposit(2000); // OK - public method
console.log(account.getBalance()); // 7000

// console.log(account.balance); // Error! Private hai
// account.calculateInterest(); // Error! Private method
```

### Readonly Properties

**Kya hai?** Readonly properties sirf initialization ke time set ho sakti hain, baad mein change nahi kar sakte.

```typescript
class Book {
    readonly isbn: string;
    readonly title: string;
    author: string;

    constructor(isbn: string, title: string, author: string) {
        this.isbn = isbn;
        this.title = title;
        this.author = author;
    }

    updateAuthor(newAuthor: string) {
        this.author = newAuthor; // OK - readonly nahi hai
        // this.isbn = "new-isbn"; // Error! Readonly property change nahi kar sakte
    }
}

let book = new Book("978-0-123456-78-9", "TypeScript Guide", "John Doe");
console.log(book.isbn); // Read kar sakte hain
// book.isbn = "new-isbn"; // Error! Change nahi kar sakte
book.updateAuthor("Jane Doe"); // Author change ho sakta hai
```

### Optional Properties

**Kya hai?** Properties jo optional hain - provide karna zaroori nahi.

```typescript
class User {
    name: string;
    email: string;
    phone?: string; // Optional property
    address?: string; // Optional property

    constructor(name: string, email: string, phone?: string) {
        this.name = name;
        this.email = email;
        this.phone = phone; // Ho sakta hai undefined
    }

    getContactInfo() {
        let info = `Name: ${this.name}, Email: ${this.email}`;
        if (this.phone) {
            info += `, Phone: ${this.phone}`;
        }
        return info;
    }
}

let user1 = new User("Raj", "raj@example.com", "9876543210");
let user2 = new User("Simran", "simran@example.com"); // Phone nahi diya

console.log(user1.getContactInfo());
console.log(user2.getContactInfo());
```

### Parameter Properties

**Kya hai?** Constructor parameters mein directly access modifiers use karke properties define karna - shorthand syntax.

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

// Parameter Properties - shorthand (same kaam, kam code)
class Student2 {
    constructor(
        public name: string,
        public rollNo: number,
        private marks: number
    ) {
        // Automatically properties ban gayi aur assign ho gayi
    }

    getMarks() {
        return this.marks; // Private property access kar sakte hain
    }
}

let student = new Student2("Vikram", 101, 85);
console.log(student.name); // "Vikram"
console.log(student.rollNo); // 101
// console.log(student.marks); // Error! Private hai
console.log(student.getMarks()); // 85
```

### Getters and Setters

**Kya hai?** Special methods jo properties ko read aur write karne ke liye use hote hain with additional logic.

```typescript
class Employee {
    private _salary: number;

    constructor(private name: string, salary: number) {
        this._salary = salary;
    }

    // Getter - property ki tarah use hota hai but method hai
    get salary(): number {
        console.log("Salary access ki gayi");
        return this._salary;
    }

    // Setter - value set karne ke liye
    set salary(newSalary: number) {
        if (newSalary < 0) {
            console.log("Salary negative nahi ho sakti!");
            return;
        }
        console.log("Salary update ho gayi");
        this._salary = newSalary;
    }

    // Computed property - getter se
    get annualSalary(): number {
        return this._salary * 12;
    }
}

let emp = new Employee("Arun", 50000);
console.log(emp.salary); // Getter call hoga - 50000
emp.salary = 55000; // Setter call hoga
emp.salary = -1000; // Validation fail - "Salary negative nahi ho sakti!"
console.log(emp.annualSalary); // Computed value - 660000
```

### Static Members

**Kya hai?** Static members class ke hote hain, individual objects ke nahi. Class name se directly access karte hain.

```typescript
class MathHelper {
    // Static property - sabhi instances ke liye same
    static PI: number = 3.14159;
    
    // Static method - class se directly call karo
    static calculateCircleArea(radius: number): number {
        return this.PI * radius * radius;
    }

    static max(a: number, b: number): number {
        return a > b ? a : b;
    }
}

// Static members ko class name se access karo
console.log(MathHelper.PI); // 3.14159
console.log(MathHelper.calculateCircleArea(5)); // 78.53975
console.log(MathHelper.max(10, 20)); // 20

// Object se access nahi kar sakte
// let helper = new MathHelper();
// console.log(helper.PI); // Error!

// Practical example - Counter
class Counter {
    static count: number = 0;

    constructor() {
        Counter.count++; // Har object create hone par count badhe
    }

    static getCount() {
        return Counter.count;
    }

    static resetCount() {
        Counter.count = 0;
    }
}

new Counter();
new Counter();
new Counter();
console.log(Counter.getCount()); // 3
Counter.resetCount();
console.log(Counter.getCount()); // 0
```

### Abstract Classes and Methods

**Kya hai?** Abstract classes wo base classes hain jinse directly object nahi bana sakte. Ye blueprint provide karte hain child classes ke liye.

```typescript
// Abstract class - directly instantiate nahi kar sakte
abstract class Animal {
    constructor(public name: string) {}

    // Normal method
    move(): void {
        console.log(`${this.name} move kar raha hai`);
    }

    // Abstract method - child class mein implementation zaroori hai
    abstract makeSound(): void;
}

// Concrete class - abstract class ko extend kar raha
class Dog extends Animal {
    // Abstract method ka implementation zaroori hai
    makeSound(): void {
        console.log(`${this.name} bhow bhow kar raha hai`);
    }

    fetch(): void {
        console.log(`${this.name} ball la raha hai`);
    }
}

class Cat extends Animal {
    makeSound(): void {
        console.log(`${this.name} meow kar rahi hai`);
    }
}

// let animal = new Animal("Generic"); // Error! Abstract class hai

let dog = new Dog("Tommy");
dog.move(); // "Tommy move kar raha hai"
dog.makeSound(); // "Tommy bhow bhow kar raha hai"
dog.fetch();

let cat = new Cat("Fluffy");
cat.makeSound(); // "Fluffy meow kar rahi hai"

// Real-world example
abstract class Shape {
    constructor(public color: string) {}

    abstract calculateArea(): number;
    abstract calculatePerimeter(): number;

    describe(): void {
        console.log(`Ye ${this.color} color ki shape hai`);
    }
}

class Circle extends Shape {
    constructor(color: string, public radius: number) {
        super(color);
    }

    calculateArea(): number {
        return Math.PI * this.radius * this.radius;
    }

    calculatePerimeter(): number {
        return 2 * Math.PI * this.radius;
    }
}

class Rectangle extends Shape {
    constructor(color: string, public width: number, public height: number) {
        super(color);
    }

    calculateArea(): number {
        return this.width * this.height;
    }

    calculatePerimeter(): number {
        return 2 * (this.width + this.height);
    }
}

let circle = new Circle("red", 5);
circle.describe();
console.log("Area:", circle.calculateArea());

let rectangle = new Rectangle("blue", 4, 6);
rectangle.describe();
console.log("Area:", rectangle.calculateArea());
```

---

## ⚡ Day 6 - Advanced TypeScript Concepts

### 🧠 Functions

#### Function Types

**Kya hai?** Function ki type define karna - parameters aur return type specify karna.

```typescript
// Function type definition
let add: (a: number, b: number) => number;

add = (x, y) => {
    return x + y;
};

console.log(add(5, 3)); // 8

// Function type as alias
type MathOperation = (a: number, b: number) => number;

let multiply: MathOperation = (x, y) => x * y;
let divide: MathOperation = (x, y) => x / y;

// Function type in interface
interface Calculator {
    operation: (a: number, b: number) => number;
}

let calc: Calculator = {
    operation: (a, b) => a + b
};
```

#### Optional & Default Parameters

**Optional Parameters:** Question mark (`?`) se - provide karna zaroori nahi.

```typescript
// Optional parameter
function greet(name: string, greeting?: string) {
    if (greeting) {
        return `${greeting}, ${name}!`;
    }
    return `Hello, ${name}!`;
}

console.log(greet("Raj")); // "Hello, Raj!"
console.log(greet("Raj", "Namaste")); // "Namaste, Raj!"

// Default parameters - agar value nahi di to ye use hoga
function createUser(name: string, age: number = 18, city: string = "Mumbai") {
    return { name, age, city };
}

console.log(createUser("Amit")); 
// { name: "Amit", age: 18, city: "Mumbai" }

console.log(createUser("Priya", 25)); 
// { name: "Priya", age: 25, city: "Mumbai" }

console.log(createUser("Rohan", 30, "Delhi")); 
// { name: "Rohan", age: 30, city: "Delhi" }

// Optional must come after required parameters
function register(name: string, email?: string, age?: number) {
    // Valid order
}

// function badOrder(name?: string, age: number) {} // Error!
```

#### Rest Parameters

**Kya hai?** Variable number of arguments accept karna - array ki tarah treat hote hain.

```typescript
// Rest parameters - 3 dots se
function sum(...numbers: number[]): number {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3)); // 6
console.log(sum(10, 20, 30, 40)); // 100
console.log(sum(5)); // 5

// Mixed parameters - rest parameter last mein hona chahiye
function makeAnnouncement(title: string, ...points: string[]) {
    console.log(`Announcement: ${title}`);
    points.forEach((point, i) => {
        console.log(`${i + 1}. ${point}`);
    });
}

makeAnnouncement(
    "Meeting Notice",
    "Meeting at 3 PM",
    "Bring your laptops",
    "Attendance is mandatory"
);

// Practical example
function createFullName(firstName: string, lastName: string, ...middleNames: string[]): string {
    return [firstName, ...middleNames, lastName].join(" ");
}

console.log(createFullName("Amit", "Sharma")); 
// "Amit Sharma"

console.log(createFullName("Amit", "Kumar", "Sharma")); 
// "Amit Kumar Sharma"

console.log(createFullName("Raj", "Kumar", "Singh", "Patel")); 
// "Raj Kumar Singh Patel"
```

#### Function Overloads

**Kya hai?** Same function name ke saath different parameter types/counts ke liye alag-alag signatures.

```typescript
// Overload signatures
function processInput(input: string): string;
function processInput(input: number): number;
function processInput(input: boolean): string;

// Implementation signature
function processInput(input: string | number | boolean): string | number {
    if (typeof input === "string") {
        return input.toUpperCase();
    } else if (typeof input === "number") {
        return input * 2;
    } else {
        return input ? "YES" : "NO";
    }
}

console.log(processInput("hello")); // "HELLO"
console.log(processInput(5)); // 10
console.log(processInput(true)); // "YES"

// Practical example - different ways to create a date
function createDate(timestamp: number): Date;
function createDate(year: number, month: number, day: number): Date;
function createDate(dateString: string): Date;

function createDate(a: number | string, b?: number, c?: number): Date {
    if (typeof a === "string") {
        return new Date(a);
    } else if (b !== undefined && c !== undefined) {
        return new Date(a, b - 1, c); // month 0-indexed hai
    } else {
        return new Date(a);
    }
}

console.log(createDate(1609459200000)); // Timestamp se
console.log(createDate(2024, 1, 15)); // Year, month, day se
console.log(createDate("2024-01-15")); // String se

// Real-world example - search function
function search(query: string): string[];
function search(query: string, limit: number): string[];
function search(query: string, category: string, limit: number): string[];

function search(query: string, categoryOrLimit?: string | number, limit?: number): string[] {
    console.log(`Searching for: ${query}`);
    
    if (typeof categoryOrLimit === "number") {
        console.log(`Limit: ${categoryOrLimit}`);
    } else if (typeof categoryOrLimit === "string") {
        console.log(`Category: ${categoryOrLimit}, Limit: ${limit}`);
    }
    
    return ["result1", "result2"]; // Dummy results
}

search("laptop");
search("laptop", 10);
search("laptop", "electronics", 10);
```

### ⚙️ Generics

#### Generic Functions

**Kya hai?** Functions jo kisi bhi type ke saath kaam kar sakein - type parameter use karke.

```typescript
// Generic function - <T> ek type parameter hai
function identity<T>(value: T): T {
    return value;
}

// Different types ke saath use kar sakte hain
let num = identity<number>(42);
let str = identity<string>("Hello");
let bool = identity<boolean>(true);

// Type inference se bhi kaam karega
let auto = identity(100); // TypeScript samajh jayega ki number hai

// Practical example - array ka first element
function getFirstElement<T>(arr: T[]): T | undefined {
    return arr[0];
}

let firstNum = getFirstElement([1, 2, 3]); // Type: number | undefined
let firstName = getFirstElement(["a", "b", "c"]); // Type: string | undefined

// Multiple type parameters
function pair<T, U>(first: T, second: U): [T, U] {
    return [first, second];
}

let nameAge = pair<string, number>("Raj", 25);
let cityCountry = pair("Mumbai", "India"); // Type inference

// Generic with constraints
interface HasLength {
    length: number;
}

function logLength<T extends HasLength>(item: T): void {
    console.log(`Length: ${item.length}`);
}

logLength("Hello"); // String has length
logLength([1, 2, 3]); // Array has length
logLength({ length: 10, name: "Test" }); // Object with length property
// logLength(123); // Error! Number doesn't have length
```

#### Generic Interfaces

**Kya hai?** Interfaces mein bhi generics use kar sakte hain.

```typescript
// Generic interface
interface Container<T> {
    value: T;
    getValue(): T;
    setValue(val: T): void;
}

// Number container
let numberContainer: Container<number> = {
    value: 100,
    getValue() {
        return this.value;
    },
    setValue(val) {
        this.value = val;
    }
};

numberContainer.setValue(200);
console.log(numberContainer.getValue()); // 200

// String container
let stringContainer: Container<string> = {
    value: "Hello",
    getValue() {
        return this.value;
    },
    setValue(val) {
        this.value = val;
    }
};

// Generic interface with multiple type parameters
interface KeyValuePair<K, V> {
    key: K;
    value: V;
}

let userAge: KeyValuePair<string, number> = {
    key: "Amit",
    value: 25
};

let setting: KeyValuePair<string, boolean> = {
    key: "darkMode",
    value: true
};

// API response interface
interface ApiResponse<T> {
    data: T;
    status: number;
    message: string;
}

interface User {
    id: number;
    name: string;
    email: string;
}

let userResponse: ApiResponse<User> = {
    data: {
        id: 1,
        name: "Raj",
        email: "raj@example.com"
    },
    status: 200,
    message: "Success"
};

let usersResponse: ApiResponse<User[]> = {
    data: [
        { id: 1, name: "Raj", email: "raj@example.com" },
        { id: 2, name: "Priya", email: "priya@example.com" }
    ],
    status: 200,
    message: "Success"
};
```

#### Generic Classes

**Kya hai?** Classes mein generics use karke reusable code banana.

```typescript
// Generic class
class Box<T> {
    private content: T;

    constructor(value: T) {
        this.content = value;
    }

    getContent(): T {
        return this.content;
    }

    setContent(value: T): void {
        this.content = value;
    }
}

// Different types ke boxes
let numberBox = new Box<number>(123);
console.log(numberBox.getContent()); // 123

let stringBox = new Box<string>("Hello");
console.log(stringBox.getContent()); // "Hello"

let userBox = new Box<User>({
    id: 1,
    name: "Amit",
    email: "amit@example.com"
});

// Generic Stack implementation
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

    isEmpty(): boolean {
        return this.items.length === 0;
    }

    size(): number {
        return this.items.length;
    }
}

let numberStack = new Stack<number>();
numberStack.push(1);
numberStack.push(2);
numberStack.push(3);
console.log(numberStack.pop()); // 3
console.log(numberStack.peek()); // 2

let stringStack = new Stack<string>();
stringStack.push("first");
stringStack.push("second");
console.log(stringStack.size()); // 2

// Generic with constraints in class
class DataStore<T extends { id: number }> {
    private data: T[] = [];

    add(item: T): void {
        this.data.push(item);
    }

    findById(id: number): T | undefined {
        return this.data.find(item => item.id === id);
    }

    getAll(): T[] {
        return this.data;
    }
}

let userStore = new DataStore<User>();
userStore.add({ id: 1, name: "Raj", email: "raj@example.com" });
userStore.add({ id: 2, name: "Priya", email: "priya@example.com" });

console.log(userStore.findById(1));
```

### 📦 Modules

#### Importing and Exporting Modules

**Kya hai?** Code ko alag-alag files mein organize karna aur unhe import/export karna.

**File 1: `math.ts` (Named Exports)**
```typescript
// Named exports - multiple exports ek file se
export function add(a: number, b: number): number {
    return a + b;
}

export function subtract(a: number, b: number): number {
    return a - b;
}

export const PI = 3.14159;

export class Calculator {
    multiply(a: number, b: number): number {
        return a * b;
    }
}

// Ya fir ek saath export kar sakte hain
// export { add, subtract, PI, Calculator };
```

**File 2: `app.ts` (Importing)**
```typescript
// Named imports
import { add, subtract, PI } from './math';

console.log(add(5, 3)); // 8
console.log(subtract(10, 4)); // 6
console.log(PI); // 3.14159

// Alias ke saath import
import { add as sum, subtract as minus } from './math';

console.log(sum(2, 3)); // 5
console.log(minus(10, 5)); // 5

// Sab kuch ek saath import
import * as MathUtils from './math';

console.log(MathUtils.add(1, 2));
console.log(MathUtils.PI);
let calc = new MathUtils.Calculator();
```

#### Default Exports

**Kya hai?** Ek file se sirf ek cheez default export kar sakte hain.

**File 1: `user.ts` (Default Export)**
```typescript
// Default export - ek file se ek hi default export ho sakta
export default class User {
    constructor(
        public name: string,
        public email: string
    ) {}

    displayInfo() {
        console.log(`${this.name} - ${this.email}`);
    }
}

// Ya function ko default export
// export default function createUser(name: string) {
//     return new User(name, `${name}@example.com`);
// }
```

**File 2: `app.ts` (Importing Default)**
```typescript
// Default import - koi bhi naam de sakte hain
import User from './user';

let user = new User("Raj", "raj@example.com");
user.displayInfo();

// Default aur named exports ek saath
// File: utils.ts
export default function mainFunction() {
    console.log("Main function");
}

export function helperFunction() {
    console.log("Helper function");
}

// Import karna
import mainFn, { helperFunction } from './utils';

mainFn();
helperFunction();
```

#### Code Organization with Modules

**Practical Example - Project Structure:**

```typescript
// File: models/user.model.ts
export interface IUser {
    id: number;
    name: string;
    email: string;
}

export class User implements IUser {
    constructor(
        public id: number,
        public name: string,
        public email: string
    ) {}
}

// File: services/user.service.ts
import { User, IUser } from '../models/user.model';

export class UserService {
    private users: User[] = [];

    addUser(user: IUser): void {
        this.users.push(new User(user.id, user.name, user.email));
    }

    getUserById(id: number): User | undefined {
        return this.users.find(u => u.id === id);
    }

    getAllUsers(): User[] {
        return this.users;
    }
}

// File: utils/validators.ts
export function isValidEmail(email: string): boolean {
    return email.includes('@');
}

export function isValidId(id: number): boolean {
    return id > 0;
}

// File: index.ts (Main file)
import { UserService } from './services/user.service';
import { isValidEmail, isValidId } from './utils/validators';

let userService = new UserService();

if (isValidId(1) && isValidEmail("test@example.com")) {
    userService.addUser({
        id: 1,
        name: "Raj",
        email: "test@example.com"
    });
}

console.log(userService.getAllUsers());
```

### 🔒 Type Assertions & Guards

#### Type Assertion & Type Casting

**Kya hai?** TypeScript ko batana ki hum type ke baare mein zyada jaante hain.

```typescript
// Type Assertion - 'as' syntax (recommended)
let someValue: unknown = "Hello TypeScript";
let strLength: number = (someValue as string).length;

console.log(strLength); // 16

// Angle bracket syntax (JSX mein kaam nahi karta)
let anotherValue: unknown = "Test";
let len: number = (<string>anotherValue).length;

// Practical example - DOM manipulation
let inputElement = document.getElementById('username') as HTMLInputElement;
inputElement.value = "Raj"; // Ab safely access kar sakte hain

// Object type assertion
interface User {
    name: string;
    age: number;
}

let userData: unknown = {
    name: "Amit",
    age: 25
};

let user = userData as User;
console.log(user.name); // "Amit"

// Type assertion with API response
interface ApiResponse {
    data: any;
}

let response: ApiResponse = {
    data: { id: 1, title: "Post 1" }
};

interface Post {
    id: number;
    title: string;
}

let post = response.data as Post;
console.log(post.title);
```

#### Non-null Assertion Operator

**Kya hai?** TypeScript ko batana ki value null/undefined nahi hai (!) operator se.

```typescript
// Non-null assertion operator (!)
function getValue(): string | null {
    return "Hello";
}

let value = getValue();
// console.log(value.length); // Error! value null ho sakta hai

// Non-null assertion - hum guarantee de rahe hain ki null nahi hai
let definiteValue = getValue()!;
console.log(definiteValue.length); // No error

// DOM example
let button = document.getElementById('btn')!; // Guarantee hai ki element milega
button.addEventListener('click', () => {
    console.log('Clicked!');
});

// Array access
let numbers = [1, 2, 3, 4, 5];
let lastNumber = numbers[numbers.length - 1]!;

// ⚠️ Warning: Carefully use karo - runtime error aa sakta hai agar galat guarantee di
```

#### Type Guards

**Kya hai?** Runtime par types check karna - safe operations ke liye.

**`typeof` Type Guard:**
```typescript
function processValue(value: string | number) {
    // typeof type guard
    if (typeof value === "string") {
        // Is block mein TypeScript jaanta hai ki value string hai
        console.log(value.toUpperCase());
    } else {
        // Yaha TypeScript jaanta hai ki value number hai
        console.log(value.toFixed(2));
    }
}

processValue("hello"); // "HELLO"
processValue(42.567); // "42.57"

// Multiple types check karna
function display(input: string | number | boolean) {
    if (typeof input === "string") {
        console.log("String:", input.toUpperCase());
    } else if (typeof input === "number") {
        console.log("Number:", input * 2);
    } else {
        console.log("Boolean:", input ? "YES" : "NO");
    }
}
```

**`instanceof` Type Guard:**
```typescript
class Dog {
    bark() {
        console.log("Bhow bhow!");
    }
}

class Cat {
    meow() {
        console.log("Meow!");
    }
}

function makeSound(animal: Dog | Cat) {
    // instanceof type guard
    if (animal instanceof Dog) {
        animal.bark(); // Safe - TypeScript jaanta hai ye Dog hai
    } else {
        animal.meow(); // Safe - TypeScript jaanta hai ye Cat hai
    }
}

let myDog = new Dog();
let myCat = new Cat();

makeSound(myDog); // "Bhow bhow!"
makeSound(myCat); // "Meow!"

// Date example
function formatDate(date: Date | string) {
    if (date instanceof Date) {
        return date.toLocaleDateString();
    } else {
        return new Date(date).toLocaleDateString();
    }
}
```

**Custom Type Guards:**
```typescript
// Custom type guard function
interface Car {
    drive(): void;
    wheels: number;
}

interface Boat {
    sail(): void;
    hasMotor: boolean;
}

// Type predicate function (return type: 'parameter is Type')
function isCar(vehicle: Car | Boat): vehicle is Car {
    return (vehicle as Car).drive !== undefined;
}

function operateVehicle(vehicle: Car | Boat) {
    if (isCar(vehicle)) {
        vehicle.drive(); // TypeScript jaanta hai ye Car hai
        console.log(`Wheels: ${vehicle.wheels}`);
    } else {
        vehicle.sail(); // TypeScript jaanta hai ye Boat hai
        console.log(`Has motor: ${vehicle.hasMotor}`);
    }
}

// Array type guard
function isStringArray(value: unknown): value is string[] {
    return Array.isArray(value) && value.every(item => typeof item === "string");
}

function processArray(arr: unknown) {
    if (isStringArray(arr)) {
        // Ab safely string array methods use kar sakte hain
        arr.forEach(str => console.log(str.toUpperCase()));
    }
}
```

**`in` Operator Type Guard:**
```typescript
interface Admin {
    name: string;
    privileges: string[];
}

interface Employee {
    name: string;
    startDate: Date;
}

type UnknownEmployee = Admin | Employee;

function printEmployeeInfo(emp: UnknownEmployee) {
    console.log(`Name: ${emp.name}`);
    
    // 'in' operator se property check karo
    if ('privileges' in emp) {
        console.log(`Privileges: ${emp.privileges.join(', ')}`);
    }
    
    if ('startDate' in emp) {
        console.log(`Start Date: ${emp.startDate.toDateString()}`);
    }
}

let admin: Admin = {
    name: "Raj",
    privileges: ["create-server", "delete-user"]
};

let employee: Employee = {
    name: "Amit",
    startDate: new Date()
};

printEmployeeInfo(admin);
printEmployeeInfo(employee);
```

### 🧩 Utility Types

#### `Partial<T>`

**Kya hai?** Type ki saari properties ko optional bana deta hai.

```typescript
interface User {
    id: number;
    name: string;
    email: string;
    age: number;
}

// Partial - sabhi properties optional ban jaati hain
type PartialUser = Partial<User>;

// Equivalent to:
// {
//     id?: number;
//     name?: string;
//     email?: string;
//     age?: number;
// }

// Update function mein useful
function updateUser(id: number, updates: Partial<User>) {
    // Sirf jo properties di gayi hain wo update hongi
    console.log(`Updating user ${id}`, updates);
}

updateUser(1, { name: "Raj" }); // Sirf name update
updateUser(2, { email: "new@example.com", age: 26 }); // Email aur age update

// Practical example - form updates
interface Product {
    id: number;
    title: string;
    price: number;
    description: string;
    category: string;
}

function updateProduct(productId: number, changes: Partial<Product>) {
    // Database mein sirf jo fields changed hain wo update karo
    console.log(`Updating product ${productId}:`, changes);
}

updateProduct(101, { price: 999 });
updateProduct(102, { title: "New Title", description: "New Description" });
```

#### `Required<T>`

**Kya hai?** Type ki saari properties ko required (mandatory) bana deta hai.

```typescript
interface UserProfile {
    name?: string;
    email?: string;
    phone?: string;
    address?: string;
}

// Required - sabhi properties mandatory ban jaati hain
type CompleteProfile = Required<UserProfile>;

// Equivalent to:
// {
//     name: string;
//     email: string;
//     phone: string;
//     address: string;
// }

function createCompleteProfile(profile: CompleteProfile) {
    // Sabhi fields zaroori hain
    console.log("Complete profile:", profile);
}

// createCompleteProfile({ name: "Raj" }); // Error! Sabhi fields chahiye

createCompleteProfile({
    name: "Raj",
    email: "raj@example.com",
    phone: "9876543210",
    address: "Mumbai"
}); // Valid

// Practical example - config validation
interface AppConfig {
    apiUrl?: string;
    timeout?: number;
    retries?: number;
}

type ValidatedConfig = Required<AppConfig>;

function validateConfig(config: AppConfig): config is ValidatedConfig {
    return config.apiUrl !== undefined &&
           config.timeout !== undefined &&
           config.retries !== undefined;
}
```

#### `Readonly<T>`

**Kya hai?** Type ki saari properties ko readonly bana deta hai - modify nahi kar sakte.

```typescript
interface User {
    id: number;
    name: string;
    email: string;
}

// Readonly - sabhi properties read-only ban jaati hain
type ReadonlyUser = Readonly<User>;

// Equivalent to:
// {
//     readonly id: number;
//     readonly name: string;
//     readonly email: string;
// }

let user: ReadonlyUser = {
    id: 1,
    name: "Raj",
    email: "raj@example.com"
};

console.log(user.name); // Read kar sakte hain
// user.name = "New Name"; // Error! Modify nahi kar sakte

// Practical example - immutable data
interface Config {
    host: string;
    port: number;
    ssl: boolean;
}

let config: Readonly<Config> = {
    host: "localhost",
    port: 3000,
    ssl: true
};

// config.port = 8080; // Error! Configuration change nahi kar sakte

// Function return type mein readonly
function getAppConfig(): Readonly<Config> {
    return {
        host: "api.example.com",
        port: 443,
        ssl: true
    };
}

let appConfig = getAppConfig();
// appConfig.host = "new-host"; // Error! Readonly hai
```

#### Use of `typeof` and `instanceof`

**`typeof` Operator:**
```typescript
// typeof - variable ki type ko type annotation mein use karna

let person = {
    name: "Raj",
    age: 25,
    city: "Mumbai"
};

// person ki type ko reuse karna
type Person = typeof person;

let anotherPerson: Person = {
    name: "Amit",
    age: 30,
    city: "Delhi"
};

// Function type ko capture karna
function add(a: number, b: number) {
    return a + b;
}

type AddFunction = typeof add;

let multiply: AddFunction = (x, y) => x * y;

// Enum ki type
enum Status {
    Active = "ACTIVE",
    Inactive = "INACTIVE"
}

type StatusType = typeof Status;

// Constant object ko type ki tarah use karna
const ROLES = {
    ADMIN: "admin",
    USER: "user",
    GUEST: "guest"
} as const;

type RoleType = typeof ROLES;
type Role = typeof ROLES[keyof typeof ROLES];

let userRole: Role = "admin"; // Valid
// let invalidRole: Role = "superuser"; // Error!
```

**`instanceof` with Classes:**
```typescript
class Animal {
    name: string;
    
    constructor(name: string) {
        this.name = name;
    }
}

class Dog extends Animal {
    bark() {
        console.log("Bhow!");
    }
}

class Cat extends Animal {
    meow() {
        console.log("Meow!");
    }
}

function handleAnimal(animal: Animal) {
    console.log(`Animal name: ${animal.name}`);
    
    // instanceof check
    if (animal instanceof Dog) {
        animal.bark(); // Safe - TypeScript jaanta hai ye Dog hai
    } else if (animal instanceof Cat) {
        animal.meow(); // Safe - TypeScript jaanta hai ye Cat hai
    }
}

let dog = new Dog("Tommy");
let cat = new Cat("Fluffy");

handleAnimal(dog);
handleAnimal(cat);

// Error handling with instanceof
class ValidationError extends Error {
    constructor(message: string) {
        super(message);
        this.name = "ValidationError";
    }
}

class NetworkError extends Error {
    constructor(message: string) {
        super(message);
        this.name = "NetworkError";
    }
}

function handleError(error: Error) {
    if (error instanceof ValidationError) {
        console.log("Validation failed:", error.message);
    } else if (error instanceof NetworkError) {
        console.log("Network issue:", error.message);
    } else {
        console.log("Unknown error:", error.message);
    }
}
```

---

