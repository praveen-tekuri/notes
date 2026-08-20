## 1. if...else statement 

> The most basic way to handle conditional logic. It lets you execute different blocks of code depending on whether a condition evaluates to true or false.

```js
if (condition) {
  // code runs if condition is true
} else {
  // code runs if condition is false
}

// Example:
let age = 18;

if (age >= 18) {
  console.log("You are an adult.");
} else {
  console.log("You are a minor.");
}
```

## 2. if...else if Statement

> The `if...else if` statement is used to execute a block of code among multiple conditions. It allows you to test several conditions and execute different blocks of code based on which condition evaluates to true.

```js
if (condition1) {
  // code runs if condition1 is true
} else if (condition2) {
  // code runs if condition2 is true
} else {
  // code runs if none of the above are true
}

// Example:

const score = 85;

if (score >= 90) {
  console.log("Grade: A");
} else if (score >= 75) {
  console.log("Grade: B");
} else if (score >= 60) {
  console.log("Grade: C");
} else {
  console.log("Grade: F");
}

// Output: "Grade: B"
```

## 3. Ternary Operator

> `Ternary operator` is a shorthand way to write `if...else` statements. It uses the syntax `condition ? exprIfTrue : exprIfFalse` and is the only operator in JavaScript that takes `three operands`.

Syntax: condition ? exprIfTrue : exprIfFalse

```js

// Example 1 Basic
const age = 20;
const beverage = age >= 18 ? "Beer" : "Juice";
console.log(beverage); // "Beer"

// Example 2 Chained Conditions
let day = 3;
let message = (day === 1) ? "Monday" :
              (day === 2) ? "Tuesday" :
              (day === 3) ? "Wednesday" : "Other day";
console.log(message); // "Wednesday"
```

## 4. Switch Statement

> `switch statements` are used to execute one block of code out of many options, based on the value of a single expression. They’re often cleaner than long chains of if...else if when checking one variable against multiple possible values.

```js
switch (expression) {
  case value1:
    // code runs if expression === value1
    break;
  case value2:
    // code runs if expression === value2
    break;
  default:
    // code runs if no case matches
}


// Example: 
let day = 3;

switch (day) {
  case 1:
    console.log("Monday");
    break;
  case 2:
    console.log("Tuesday");
    break;
  case 3:
    console.log("Wednesday");
    break;
  default:
    console.log("Other day");
}
```

## 5. Logical AND (&&)  

> Executes the right-hand expression only if the left-hand is truthy.

```js
let isLoggedIn = true;
let username = "Praveen";
isLoggedIn && console.log(`Welcome, ${username}`);

// Output: Welcome, Praveen
```

## 6. Logical OR (||)  

> Returns the first truthy value, or the last value if none are truthy.

```js
let userInput = "";
let defaultName = "Guest";
let name = userInput || defaultName;

console.log(name); // "Guest"
```

## 7. Logical NOT (!)  

> Inverts a boolean value.

```js
let isAdmin = false;

if (!isAdmin) {
  console.log("Access restricted");
}
```

## 8. Nullish Coalescing (??)  

> Returns the right-hand operand only if the left-hand is `null` or `undefined`.

```js
let userAge = null;

let age = userAge ?? 18;
console.log(age); // 18
```

## 9. Optional Chaining (?.)  

> Safely accesses nested properties without throwing errors if something is null or undefined.

```js
let user = { profile: { name: "Praveen" } };

console.log(user.profile?.name); // "Praveen"
console.log(user.address?.city); // undefined
```

## 10. Summary

1. If...Else → Basic binary decision (true/false).
2. If...Else If → Multiple sequential conditions.
3. Switch → Cleaner syntax for checking one variable against many fixed values.
4. Ternary Operator → Shorthand inline conditional (condition ? trueExpr : falseExpr).

5. Logical AND (&&) → Executes right-hand expression only if left-hand is truthy.
6. Logical OR (||) → Provides fallback/default values.
7. Logical NOT (!) → Negates a condition.
8. Nullish Coalescing (??) → Handles null/undefined safely.
9. Optional Chaining (?.) → Safely accesses nested properties.

## Mock Data

```js
const employees = [
  { id: 1, name: "Alice", department: "HR", salary: 50000 },
  { id: 2, name: "Bob", department: "Engineering", salary: 75000 },
  { id: 3, name: "Charlie", department: "Engineering", salary: 90000 },
  { id: 4, name: "Diana", department: "Finance", salary: 60000 },
  { id: 5, name: "Ethan", department: "HR", salary: 55000 },
  { id: 6, name: "Fiona", department: "Finance", salary: 70000 },
  { id: 7, name: "George", department: "Engineering", salary: 80000 },
  { id: 8, name: "Hannah", department: "Sales", salary: 65000 },
  { id: 9, name: "Ian", department: "Sales", salary: 62000 },
  { id: 10, name: "Jane", department: "HR", salary: 58000 }
];
```

## Example 1 with Nullish Coalescing (??)

```js
const summary = employees.reduce((acc, employee) => {
  const { department } = employee;
  const salary = employee.salary;

  // Initialize department data if missing
  acc.departments[department] ??= { department, count: 0 };
  acc.groupByDepartment[department] ??= [];
  acc.totalSalaryByDepartment[department] ??= { department, totalSalary: 0 };
  acc.highestSalaryByDepartment[department] ??= { department, highestSalary: -Infinity };

  // Update values
  acc.departments[department].count++;
  acc.groupByDepartment[department].push(employee);
  acc.totalSalaryByDepartment[department].totalSalary += salary;
  acc.highestSalaryByDepartment[department].highestSalary = Math.max(
    acc.highestSalaryByDepartment[department].highestSalary,
    salary
  );

  return acc;
}, {
  departments: {},
  groupByDepartment: {},
  totalSalaryByDepartment: {},
  highestSalaryByDepartment: {}
});
```

## Example 2 with modern operators (&&, ||, ??, ?., !, and ternary ?:) 

User Profile handling

```js
const user = {
  name: "Praveen",
  age: null,
  role: "editor",
  settings: {
    theme: "dark"
  }
};

const defaultName = "Guest";
const defaultAge = 18;
const salary = 50000;

// Logical OR (||) → fallback value
const displayName = user.name || defaultName;
console.log("Name:", displayName); // "Praveen"

// Nullish Coalescing (??) → only for null/undefined
const displayAge = user.age ?? defaultAge;
console.log("Age:", displayAge); // 18

// Optional Chaining (?.) → safe property access
const theme = user.settings?.theme;
console.log("Theme:", theme); // "dark"

// Logical AND (&&) → conditional execution
user.role === "editor" && console.log("Editor privileges granted");

// Logical NOT (!) → negation
const isAdmin = user.role === "admin";
if (!isAdmin) {
  console.log("Restricted access for non-admins");
}

// Ternary Operator (?:) → inline condition
const bonus = salary > 60000 ? "High bonus" : "Standard bonus";
console.log("Bonus:", bonus); // "Standard bonus"
```
