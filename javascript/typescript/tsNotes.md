### **What is TypeScript?**

A superset of javascript that adds static typing.

- Helps you catch errors during development
- Helps provide documentation for your components
- Compiles to plain javascript
- Does not improve performance

**Typescript** : Static Typing ⇒ Compile time errors

**JavaScript** : Dynamic Typing ⇒ Run Time errors

### 1. Type Annotations

helps Typescript understand data flowing through the app.

**Basic Types:**

```jsx
string => "Hi there", "", ''
number => 0, 0.123, 999
boolean => true, false
string[] => ["Hi", "there"]
number[] => [0, 50, -250]
boolean[] => [true, false, 1 > 0]
```

```jsx
// Args a, b must be numbers & fn add returns a number
function add(a:number, b:number):number{
    return a + b;
}

add(1, 2)

// ==============

const colors:string[] = ["red", "green", "blue"]

function printColors(values:string[]){
    console.log(values);
}

printColors(colors);
```

### 2. Type Interface

- Type annotation describes type of variable
- To describe type of object, we often use interface

**Type annotation** ⇒ It properly adds type annotations for both parameters and return type, clearly communicates typescript that function expects 2 numbers as input and will return a number.

**Type interface** ⇒ It removes explicit return type declaration of Boolean, allowing typescript to infer return type based on function logic, thereby simplifying code while still ensuring the type safety.

**Describing the objects with interfaces:**

```tsx
interface Car{
    year: number,
    make: string,
    model: string
}

function formatCar(car: Car){
    return `Year: ${car.year} model: ${car.model}`
}
```

#### 2.1 Function types and function types in Props interface

Function types define the shape of function, and in props interfaces they ensure type-safe communication between components.

**Type of function in multiple ways:**

```jsx
// 1. Inline function Type
let add1: (a: number, b: number) => number;
add1 = (a, b) => a + b;

// 2. Type Alias
type AddFn = (a: number, b: number) => number; // Convention: Capitalize Type names

const add2: AddFn = (a, b) => a + b;

// 3. Interface
interface IAddFn {
  (a: number, b: number): number; 
}

const add3: IAddFn = (a, b) => a + b;
```

**Props Interface**

```tsx
interface ColorPickerProps{
  colors: string[],
  handleSelector: (color: string) => void;
}

function ColorPicker({colors, handleSelector}:ColorPickerProps){
  const renderedColors = colors.map((color) => {
    return <button key={color} onClick={() => handleSelector(color)}>{color}</button>
  })
}

// handleSelector: (color: string) => void; with parameters, does not return
// handleSelector?: () => void; optional function
// handleSelector: () => string; returning a value
```

#### **2.2 Extending Interface**

Creating a new interface based on existing one (Reuse + add more fields)

```tsx
interface ButtonProps{
  label: string,
  onClick: () => void
}

interface IconButtonProps extends ButtonProps{
  icon: string
}

const Button = ({label, onClick}: ButtonProps) => {}
const ButtonIcon = ({label, onClick, icon}: IconButtonProps) => {}
```

### 3. Type Unions | Type narrowing with type guards

To define a function that can receive multiple types as an argument, we can use type unions

```jsx
function setConfig(flag: string | boolean){}
```

Uses pipe symbol to indicate that flag parameter can be either string or boolean, thus representing flexibility needed for this function argument.

**Type Narrowing with typeof:**

```tsx
typeof("abcd") // string
typeof(123) // number
typeof(true) // boolean
typeof(undefined) // undefined
typeof({}) // object
typeof(() => {}) // function
Array.isArray([]) // true
```

```tsx
interface Image{
  src: string
}

function logOutput(value: string | number | string[] | Image){
  if(typeof value === "string") value.toUpperCase();
  if(typeof value === "number") {};
  if(Array.isArray(value)) value.join(".");
  if(typeof value === "object" && !Array.isArray(value)) {};
}
```

**Type Predicates**

A type predicate is where we define a separate function that’s gonna (type-check) narrow down a type for you.

```tsx
interface Image{
  src: string
}

interface User{
  name: string
}

function logOutput(value: Image | User){
  if("src" in value) {}
  if("name" in value) {}
}
```

#### **Optional Properties**

```tsx
interface Profile{
  likes: string[]
}

interface User{
  id: string,
  profile?:Profile
}

// If the profile exists, try access likes, otherwise if profile is undefined, like will not be executed at all.
const user:User = {id: "101"}
user.profile?.likes;
```

```tsx
interface User{
  userName: string
}

interface ShowTaskProps{
  task: Task
}

interface Task{
  title: string,
  assignedTo?:User
}

function showTask({task}:ShowTaskProps){
  let message="";
  if(task.assignedTo){
    message = `Assigned to: ${task.assignedTo.userName}`
  }else{
    message = "Task not assigned"
  }
}

// It properly checks if a task is assigned before trying to access username property, 
// ensuring that code remains robust and avoid runtime errors.
```

### 4. The Any and Unknown types

**Any**

- Special type that tells typescript to ignore type checking around this variable.
- Try to avoid in the code
- You can use an `as` type assertion to forcibly tell typescript what an `any` variable type is

```jsx
// The any Approach (Trust Me, I Know)
interface Book{
  title: string
}

async function fetchBook(){
  const resp = await fetch("/book");
  const data = await resp.json(); // TypeScript types this as 'any' by default
  return data as Book;
}
```

- **What it actually does:** It tells TypeScript to turn off type-checking for the `data` variable. By writing `as Book`, you are making a promise: *"I guarantee the API will return a `{ title: string }` object.*
- **The Danger:** If the API breaks or changes tomorrow (e.g., returns `null` or `{ name: "Harry Potter" }`), TypeScript will not warn you. The app will crash later in production when you try to read `book.title`.

**Unknown**

- Special type that tells typescript this variable can be anything
- we have to do aggressive `type narrowing` before assuming what a `unknown` variables real type is.

```jsx
// The unknown Approach (Show Me Proof)

interface Book{
  title: string
}

async function fetchBook(){
  const resp = await fetch("/book");
  const data:unknown = await resp.json(); // Explicitly typed as unknown
  if(data && typeof data === "object" && "title" in data && typeof data.title === "string"){
    return data as Book;
  }else{
    throw new Error("No book found")
  }
}
```

- **What it actually does:** It treats the incoming API data with suspicion. Because it is marked `unknown`, TypeScript forces you to run a checklist of runtime tests (`if` statements) to confirm the data structure.
- **The Safety:** If the API structure changes, your app catches it immediately inside this function and throws a controlled error, preventing unexpected crashes down the line.

`Any` disables type checking, while `unknown` enforces type safety by requiring type checks before usage. use `any` when don’t care about types, use `unknown` when value type is not known yet.

**Type Aliases**

```tsx
interface Image {
  src: string,
}

// OR
type Image = {
  src: string
}

function logValue(value: string | number | string[] | Image){}

// OR

type logValues = string | number | string[] | Image;

function logValue(value: logValues){}
```

**When should we use interface and when should we use type?**

- they kind of interchangable in many different scenorios.
- we use interface anytime we trying to describe plain object.
- we use interface anytime to extend one type with another
- Type alias can’t be extended
- We use type alias very often to compute a new type

### 5. Generics

Generics makes it easier to write functions, interfaces, and more that work with multiple different types.

**Life without Generics: You either duplicate or use unsafe any**

```tsx
// Code duplication
function wrapInStringArray(value: string): (string)[]{
  return [value]
}

function wrapInNumberArray(value: number): (number)[]{
  return [value]
}

const result1 = wrapInStringArray("Hello");
const result2 = wrapInNumberArray(123)

// Any => no type safty, can cause bugs(runtime)
function getValue(value:any):any{
  return value;
}
```

**Introduction to Generic functions (The solution)**

```tsx
// Example 1: Generics

// T acts as a placeholder, Type to wrap
function wrapInArray<T>(value: T):T[]{
  return [value]
}

const result1 = wrapInArray<string>("Hello");
const result2 = wrapInArray<number>(123);

```

```jsx
// Example 2: The Problem
function toRecord(id:number, value: string){
  return {id, value}
}

// If we deal with id as string ? error 

const r1 = toRecord(123, "test1@gmail.com");
const r2 = toRecord("B123", "test1@gmail.com");

// Generics Help with this

// The Solution
function toRecord<T>(id:T, value: string){
  return {id, value}
}

const r1 = toRecord<number>(123, "test1@gmail.com");
const r2 = toRecord<string>("B123", "test1@gmail.com");

// OR

function toRecord<T1, T2>(id:T1, value: T2){
  return {id, value}
}

const r1 = toRecord<number, string>(123, "test1@gmail.com");

// Example 3

function randomEl<T>(arr:T[]){}

const r1 = randomEl<number>([1, 2, 3]);
const r2 = randomEl<string>(["a", "b", "c"]);
```

#### 5.1 Generics with fetch

```tsx
interface User {userName: string}
interface Message {content: string}
interface Image {url: string}

async function fetchData<T>(path: string):Promise<T>{
  const resp = await fetch(path);
  const json = await resp.json(); // here json is by default type "any", solve this using type annotation on this json variable(<User> etc))
  return json as T;
}

const run = async() => {
  const users = await fetchData<User>("/users");
  const messages = await fetchData<Message>("/messages");
  const Images = await fetchData<Image>("/images");
}
```

#### 5.2 Generic type inference

Automatically figures out generic type <T> without explicitly passing it.

```jsx
const result = useState("abcd");
```

useState is a generic function, in most scenorios we will rely on generic type inference, so we don’t need to put generic type.

```tsx
function identify<T>(value:T):T{
  return value;
}

// const r1 = identify<string>("hello"); // <string> not required
const r1 = identify("hello");
```

```tsx
// Parent Component
function App(){

  const [count, setCount] = useState(0); // TS infers count => number
  return <Child count={count} setCount={setCount}/>

}

// Child Component
interface ChildProps{
  count: number,
  setCount:React.Dispatch<React.SetStateAction<number>>;
}

function Child({count, setCount}:ChildProps){
  return(
    <h1>{count}</h1>
  )
}
```

Generic type inference allows TS to automatically determine the type parameter based on function arguments, reducing the need for explicit type annotations.

Use `<T>` Manually when:

- Inference is wrong
- working with empty values
- you want stricter control

#### 5.3 Generic type Constraints

Constraints let you restrict what types a generic can accept using extends, ensuring type safty.

```tsx
// ex: 1
function merge<T extends object, U extends object>(objA:T, objB:U): T & U{
  return {...objA, ...objB}
}

const result1 = merge({id: "db123"}, {color: "red"});

// ex: 2
function collect<T extends keyof U, U extends object>(key: T, arr:U[]){
  return arr.map((el) => el[key]);
}

const result2 = collect("name", [{count: 1, name: "applle"}, {count: 2, name: "banana"}])

// ex: 3
function printLength<T>(value: T){
  return value.length; // error: T does not know T has length;
}

// ex: 4 now safe.
function printLength<T extends {length: number}>(value: T){
  return value.length;
}

printLength([1, 2, 3]) // array has length;
printLength("hello") // string has length;
printLength(10) // number has No length;

// ex: 5

interface User{id: number}
function getId<T extends User>(obj:T){
  return obj.id
}
```

➡ Extends in Generics = Must follow this shape

**Summary:**

1. Primative types represents basic values like string, number, and boolean forming the foundation of type.
2. Any disables type checking, allowing any value but sacrifysing type saftey.
3. Unknown represents any value but enforces type checking before usage, making it safer than any.
4. Interface defines contracts for object structures, enabling extension and scalable design.
5. Type alias creates reusable type definations, supporting unions, inter-sections and primatives.
6. Generics enable reusable and type-safe components by allowing dynamic types.