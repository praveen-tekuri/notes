## 1. Object Aggression and Grouping Patterns:

### `count ++ or ++ count` vs `count+1`

`++` changes the value of the variable itself (mutation), while `+ 1` only calculates a new value without changing the variable (expression).

- Use `+ 1` when you want to calculate a value for a one-time comparison or argument (e.g., Math.max(acc[curr], curr.salary) or index + 1).
- Use `++` when you are inside loops (for, forEach) or counters where your only goal is to bump the variable up by one.

> The behavior of `+1`: Pure math. It reads the current value, adds 1 to it, and gives you the result. The original variable remains completely untouched.

```js
let count = 5;

console.log(count + 1); // Output: 6
console.log(count);     // Output: 5 (It did not change!)
```
To update the variable using addition, you must explicitly assign it: `count = count + 1`.

> The behavior of post increment `++` : It increments the variable, but returns the old value first.
```js
let x = 5;
let result = x++; 

console.log(result); // Output: 5 (Returns old value first)
console.log(x);      // Output: 6 (Variable was updated)
```

> The behavior of pre increment `++`: It increments the variable and returns the new value immediately.

```js
let y = 5;
let result = ++y; 

console.log(result); // Output: 6 (Returns new value immediately)
console.log(y);      // Output: 6 (Variable was updated)
```

> Students Array

```js
const employees = [
  { name: "Alice Smith", department: "Engineering", salary: 85000 },
  { name: "Bob Jones", department: "Finance", salary: 62000 },
  { name: "Charlie Brown", department: "HR", salary: 55000 },
  { name: "Diana Prince", department: "Finance", salary: 95000 }
];
```

### 1. Count Employees by department

```js
/* 
Expected Output:

{ Engineering: 1, Finance: 2, HR: 1 }

*/  

const employeesCountByDept = employees.reduce((acc, curr) => {
    if(!acc[curr.department]) {
        acc[curr.department] = 0
    }
    acc[curr.department]++

    return acc;
},{})

console.log(employeesCountByDept);
```

### 2. Count Employees by department

```js

/* 
Expected Output:

{
  Engineering: { department: 'Engineering', count: 1 },
  Finance: { department: 'Finance', count: 2 },
  HR: { department: 'HR', count: 1 }
}
*/

const employeesCountByDeptObj = employees.reduce((acc, curr) => {
    if(!acc[curr.department]){
        acc[curr.department] = {department: curr.department, count: 0}
    }
    acc[curr.department].count++
    
    return acc;
},{})

console.log(employeesCountByDeptObj);

// Object.values(employeesCountByDeptObj) => Converts the Object values into Array
```

### 3. Group Employees By Department

```js
/* 
Expected Output:

{
  Engineering: [ { name: 'Alice Smith', department: 'Engineering', salary: 85000 } ],
  Finance: [
    { name: 'Bob Jones', department: 'Finance', salary: 62000 },
    { name: 'Diana Prince', department: 'Finance', salary: 95000 }
  ],
  HR: [ { name: 'Charlie Brown', department: 'HR', salary: 55000 } ]
}
*/

const groupEmployeesByDept = employees.reduce((acc, curr) => {
    if(!acc[curr.department]){
        acc[curr.department] = []
    }
    acc[curr.department].push(curr);
    
    return acc;
},{})

console.log(groupEmployeesByDept);
```

### 4. Sum Salary By Department

```js
/*
Expected Output:

{ Engineering: 85000, Finance: 157000, HR: 55000 }

*/
const sumSalaryByDept = employees.reduce((acc, curr) => {
    if(!acc[curr.department]){
        acc[curr.department] = 0
    }
    acc[curr.department] += curr.salary;
    
    return acc;
},{})

console.log(sumSalaryByDept);
```

### 5. Find Highest Salary Per Department

```js
/*
Expected Output:

{ Engineering: 85000, HR: 55000, Finance: 95000 }

*/

const findHighestSalaryByDept = employees.reduce((acc, curr) => {
    if(!acc[curr.department]){
        acc[curr.department] = -Infinity
    }
    acc[curr.department] = Math.max(acc[curr.department], curr.salary);
    
    return acc;
},{})

console.log(findHighestSalaryByDept);
```

### 6. Count Occurrences of Characters

```js
/*
Expected Output:

{ s: 1, a: 2, n: 1, g: 1, r: 1, e: 1, d: 2, y: 1 }

*/

const word = "sangareddy";

const countChar = [...word].reduce((acc, curr) => {
    if(!acc[curr]){
        acc[curr] = 0;
    }
    acc[curr]++
    return acc;
},{})

console.log(countChar);
```

### 7. Count Occurrences of Characters

```js
/*
Expected Output:
{ '1': 3, '2': 2, '3': 1, '4': 1 }

*/

const numbers = [1, 2, 1, 3, 2, 1, 4];

const countNumbers = numbers.reduce((acc, curr) => {
    if(!acc[curr]){
        acc[curr] = 0;
    }
    acc[curr]++
    return acc;
},{})

console.log(countNumbers);
```


