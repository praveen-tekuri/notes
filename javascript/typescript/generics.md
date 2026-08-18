
`Generics` in TypeScript allow you to create `reusable` components, functions, and classes that can work with a variety of data types rather than just a single one. They provide the flexibility of the `any` type while maintaining full type safety and IntelliSense support.

`Generics` Allow us to create reusable and type-safe components, functions, or interfaces without knowing the exact type in advance. The actual type is provided when the code is used.

## 1. Simple Generic

Understand how input and output are related.

```js
function identify<T>(value: T): T{
    return value;
}

const name = identify("Praveen"); // inferred to String
const age = identify(30); // inferred to Number
```

## 2. Generic Constraint

```js
function printLength<T extends {length: number}>(value: T){
    console.log(value.length);
}

printLength("generics"); // Valid
printLength([1, 2, 3]); // Valid
printLength(123); // Invalid: number does not have length;
```

## 3. Understand keyof

```js
interface Employee{
    name: string;
    salary: number;
    department: string;
}

type EmployeeKeys = keyof Employee; 

// Result: "name" | "salary" | "department"
```

## 4. Property Getter

```js
const employee = {
    name: "Praveen",
    salary: 50000
}

function getValue<T extends object, K extends keyof T>(obj: T, key: K):T[K]{
    return obj[key];
}

const name = getValue(employee, "name"); // name: string,
const salary = getValue(employee, "salary"); // salary: number
```

## Mental Model | Generic constraints

1. Every time you see: `<T extends Something>`
    - T can be anything that satisfies something
2. Every time you see: `keyof T`
    - Give me all property names of that object / K must be a key that exists inside T 
3. Every time you see: `T[K]`
    - Give me the type of that property / key
    - T[K]: e.g user["name"] => string, user["age"] => number

## 1. T extends {id: number}

I don't care what T is, as long as it has an id

```js
function printId<T extends {id: number}>(item:T){
    console.log(item.id);
}


const user = {id: 1, name: "Praveen"};
const product = {id: 101, price: 500};
const category = {name: "Electronics"};

printId(user);
printId(product);
printId(category); // ❌ no id
```

## 2. K extends keyof T 

Now suppose we want to access a property

```js
function getProperty<T, K extends keyof T>(obj: T, key: K){
    return obj[key]
}

const user = {name: "Praveen", age: 30}

// keyof T becomes "name" | "age"
// K extends keyof T means, K must be "name" or "age"

getProperty(user, "name");
getProperty(user, "age");
getProperty(user, "email"); // ❌ 
```

## 3. T[K]

Give me the type of property K inside T.

```js
type User = {name: string; age: number}

const user:User = {name: "Praveen", age: 30}

User["name"] // string
User["age"] // number 

// Think of it like accessing a value
console.log(user["name"]); // Praveen => it's type is string
console.log(user["age"]); // 30 => it's type is number 
```

## Combine all three

- Take any object `T`, 
- Take a `key K` that exists in `T`, 
- The `value` must have the correct `type for that key`.

```js
function updateProperty<T, K extends keyof T>(obj: T, key: K, value: T[K]){
    return obj[key] = value;
}

// This function is extremely type-safe

const user = {name: "Praveen", age: 30};

updateProperty(user, "name", "Naveen");
updateProperty(user, "age", 33);
updateProperty(user, "age", "John"); // ❌

// Step: 1: T => User 
// Step: 2: keyof T => "name" | "age"
// Step: 3: Suppose K="age", T[K] becomes User["age"] which is number.
```

> Why this matters

```js
function sortBy<T, K extends keyof T>(items: T[], key:K){
    return [...items].sort((a, b) => String(a[key]).localeCompare(String(b[key])))
}

const users = [
    {name: "Praveen", age: 30},
    {name: "Naveen", age: 32},
]

const sortByName = sortBy(users, "name");
const sortByAge = sortBy(users, "age");
const sortBySalary = sortBy(users, "salary"); // ❌
```

The function/component doesn't need to know whether it's dealing with:

- User
- Product
- Employee
- Student
- Order

It just understands the relationship between the object and its keys.

## 5. Generic Table Component

Create a table that can render any object array.

```js
// Table.tsx

interface TableProps<T>{
    data: T[];
    columns: (keyof T)[]
}

// function Table<T extends {id: number}>({data, columns}: TableProps<T>) {
const Table = <T extends {id: number},>({data, columns}: TableProps<T>) =>{
  return (
        <table border={1}>
            <thead>
                <tr>
                    {columns.map((col) => <th key={String(col)}>{String(col)}</th>)}
                </tr>
            </thead>
            <tbody>
                {data.map((row) => (
                    <tr key={row.id}>
                        {columns.map((col) => <td key={String(col)}>{String(row[col])}</td>)}
                    </tr>
                ))}
            </tbody>
        </table>
    )
}

export default Table

/* 

- For every employee, create a table row. For every column, find that property from the employee and put its value inside a table cell.

- Employee → Row
- Column   → Cell
- row[column] → Cell value

*/

/* 

- <T extends { id: number }> => in a .tsx file, TypeScript/JSX can interpret as beginning of JSX element.

- <T extends { id: number },> => The trailing comma: makes it unambiguous: "This is a TypeScript generic parameter, not JSX."

- Normal function:
function Table<T extends { id: number }>(props) {}

- Arrow function:
const Table = <T extends { id: number },>(props) => {}
                                      ↑
                                   comma

- Generic arrow functions in .tsx commonly use a trailing comma to distinguish the generic from JSX.
*/

```

Usage:

```js
// App.jsx

const employees = [
  {id: 1, name: "praveen", department: "IT"},
  {id: 2, name: "naveen", department: "Finance"},
]

const products = [
  {id: 1, name: "Laptop", price: 50000},
  {id: 2, name: "Mobile", price: 20000},
]

<Table data = {employees} columns={["id", "name", "department"]}/> // Valid
<Table data = {products} columns={["id", "name", "price"]}/> // Valid

// Type '"Salary"' is not assignable to type '"id" | "name" | "department"'.ts(2322)
<Table data = {employees} columns={["Salary"]}/> // Invalid
```

## 6. Generic Search Component

Search using any property

```js
import { useState } from "react";

interface SearchProps<T extends object>{
    data: T[];
    searchKey: keyof T;
}

function Search<T extends object>({data, searchKey}: SearchProps<T>) {
  const [query, setQuery] = useState("");
  
  const filteredData = data.filter((employee) => String(employee[searchKey]).toLowerCase().includes(query.toLowerCase()));

  return (
    <div>
        <input value={query} onChange={(e) => setQuery(e.target.value)} type="text" placeholder="Search..." />
        <pre>
            {JSON.stringify(
                filteredData, 
                null, 
                2
            )}
        </pre>
    </div>
  )
}

export default Search
```

Usage:

```js
const employees = [
  {id: 1, name: "praveen", department: "IT"},
  {id: 2, name: "naveen", department: "Finance"},
]

<Search data = {employees} searchKey={"department"}/>
```

## 7. Generic Form Component

```js
import { useState } from "react";

interface FormProps<T extends object>{
    initialValues: T;
    onSubmit: (values: T) => void;
}

function Form<T extends object> ({initialValues, onSubmit}: FormProps<T>) {
  const [formData, setFormData] = useState(initialValues);
  const handleChange = (key: keyof T, value: string) => {
    setFormData(prev => ({...prev, [key]:value}))
  }
  return (
    <form onSubmit={e => {e.preventDefault(); onSubmit(formData)}}>
        {Object.keys(formData).map(key => (
            <div key={key}>
                <label htmlFor="">{key}</label>
                <input value={String(formData[key as keyof T])} onChange={e => handleChange(key as keyof T, e.target.value)} type="text" />
            </div>
        ))}
        <button>Submit</button>
    </form>
  )
}

export default Form
```

Usage: 

```js

interface Employee {
  name: string,
  email: string
}

<Form<Employee> initialValues={{name: "", email: ""}} onSubmit={employee => console.log(employee)} />
```

## 8. Generic Sorter

```js
type SortableValue = string | number;

function sortBy<T extends object, K extends keyof T>(data: T[], key: K): T[]{
    return [...data].sort((a, b) => {
        const x = a[key];
        const y = b[key];
        if(typeof x === "string" && typeof y ==="string"){
            return x.localeCompare(y);
        }
        if(typeof x === "number" && typeof y === "number"){
            return x - y;
        }
        return 0;
    });
}
```

Usage: 

```js
const employees = [
  {id: 1, name: "praveen", salary: 40000},
  {id: 2, name: "naveen", salary: 50000},
]

const sortResult = sortBy(employees, "name");
```

