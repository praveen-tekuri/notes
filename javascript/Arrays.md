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

## 2. Array Utility Methods

### 0. for...of

> `for...of` → Iterates over the values of an iterable object (like arrays, strings, maps, sets).

```js
const nums = [1, 2, 3];
for (const n of nums) {
    console.log(n); // Logs 1, 2, 3
}
```

### 1. forEach, for...of, map, filter, reduce, find, findIndex

> `forEach / for...of` → Iterates over each element, but does not return a new array.

```js
const nums = [1, 2, 3];
nums.forEach(n => console.log(n)); // Logs 1, 2, 3

for (const n of nums) {
    console.log(n); // Logs 1, 2, 3
}
```

> `map` → Transforms each element and returns a new array.

```js
const nums = [1, 2, 3];
const doubled = nums.map(n => n * 2); // [2, 4, 6]
```

> `filter` → Keeps only elements that match a condition.

```js
const nums = [1, 2, 3, 4];
const even = nums.filter(n => n % 2 === 0); // [2, 4]
```

> `reduce` → Reduces / Accumulates the array to a single result / value.

```js
const nums = [1, 2, 3, 4];
const sum = nums.reduce((acc, curr) => acc + curr, 0); // 10

//  `reduce` can also be used to transform an array into an object.

const nums = [1, 2, 3, 4];
const obj = nums.reduce((acc, curr) => {
    acc[curr] = curr * 2;
    return acc;
}, {}); // {1: 2, 2: 4, 3: 6, 4: 8}
```

> `find` → Returns the first element that matches a condition.

```js
const nums = [1, 2, 3, 4];
const firstEven = nums.find(n => n % 2 === 0); // 2
```

> `findIndex` → Returns the index of the first element that matches a condition.

```js
const nums = [1, 2, 3, 4];
const firstEvenIndex = nums.findIndex(n => n % 2 === 0); // 1
```

### 2. some, every, includes

> `some/every` → Checks if some or all elements match a condition.

```js
const nums = [1, 2, 3, 4];
const hasEven = nums.some(n => n % 2 === 0); // true
const allEven = nums.every(n => n % 2 === 0); // false
```

> `includes` → Checks if an array contains a specific value.

```js
const nums = [1, 2, 3, 4];
const hasThree = nums.includes(3); // true
const hasFive = nums.includes(5); // false
```

- find vs includes: `find` returns the element itself, while `includes` returns a boolean indicating presence.

### 3. sort, reverse, and flat

> `sort` → Sorts the array in place. By default, it sorts elements as strings.

```js
const nums = [3, 1, 4, 2];
nums.sort(); // [1, 2, 3, 4] (sorted as strings)

// sort in ascending order (numerically)
nums.sort((a, b) => a - b); // [1, 2, 3, 4]

// sort in descending order (numerically)
nums.sort((a, b) => b - a); // [4, 3, 2, 1]

// sort strings alphabetically
const fruits = ["banana", "apple", "cherry"];
fruits.sort(); // ["apple", "banana", "cherry"]

// sort strings in reverse alphabetical order
fruits.sort((a, b) => b.localeCompare(a)); // ["cherry", "banana", "apple"]

// sort objects by a property
const people = [{ name: "Alice", age: 30 }, { name: "Bob", age: 25 }, { name: "Charlie", age: 35 }];
people.sort((a, b) => a.age - b.age); // Sort by age ascending

// sort objects by a property in descending order
people.sort((a, b) => b.age - a.age); // Sort by age descending
```

> `reverse` → Reverses the array in place.

```js
const nums = [1, 2, 3, 4];
nums.reverse(); // [4, 3, 2, 1]
```

> `flat` → Flattens nested arrays into a single array.

```js
const nested = [1, [2, [3, 4]], 5];
const flatOnce = nested.flat(); // [1, 2, [3, 4], 5]
const flatCompletely = nested.flat(Infinity); // [1, 2, 3, 4, 5]
``` 

### 4. toString, toLocaleString, and valueOf

> `toString` → Converts an array to a string, with elements separated by commas.

```js
const nums = [1, 2, 3];
console.log(nums.toString()); // "1,2,3"
```

> `toLocaleString` → Converts an array to a string, using locale-specific formatting.

```js
const nums = [123456.789, 987654.321];
console.log(nums.toLocaleString('en-US')); // "123,456.789,987,654.321"
console.log(nums.toLocaleString('de-DE')); // "123.456,789,987.654,321"
```

> `valueOf` → Returns the array itself. This is rarely used directly.

```js
const nums = [1, 2, 3];
console.log(nums.valueOf() === nums); // true
```

### 6. concat, join, and slice

> `concat` → Merges two or more arrays into a new array.

```js
const arr1 = [1, 2];
const arr2 = [3, 4];
const merged = arr1.concat(arr2); // [1, 2, 3, 4]
```

> `join` → Joins all elements of an array into a string, separated by a specified separator.

```js
const arr = [1, 2, 3];
console.log(arr.join('-')); // "1-2-3"
```

> `slice` → Returns a shallow copy of a portion of an array into a new array object.

```js
const arr = [1, 2, 3, 4, 5];
const sliced = arr.slice(1, 4); // [2, 3, 4] => (1, 4-1 = 3 = [2, 3, 4])

// 1. slice(start, end) → Returns elements from index start to end-1 
// 2. slice(start) → Returns elements from index start to the end of the array
// 3. slice() → Returns a shallow copy of the entire array
// 4. slice(-n) → Returns the last n elements of the array
```

- slice vs spread operator: `slice` returns a new array with the selected elements, while the spread operator creates a shallow copy of the entire array.

### 7. splice, push, and pop

> `splice` → Changes the contents of an array by removing or replacing existing elements and/or adding new elements in place.

```js
const arr = [1, 2, 3, 4, 5];
// Remove 2 elements starting from index 1
const removed = arr.splice(1, 2); // removed = [2, 3], arr = [1, 4, 5]  
// Add elements at index 1
arr.splice(1, 0, 'a', 'b'); // arr = [1, 'a', 'b', 4, 5]
// Replace 1 element at index 2
arr.splice(2, 1, 'c'); // arr = [1, 'a', 'c', 4, 5]

// splice(start, deleteCount, item1, item2, ...) → Removes deleteCount elements from index start and inserts item1, item2, ... at that position.
```

> `push` → Adds one or more elements to the end of an array and returns the new length of the array.

```js
const arr = [1, 2, 3];
const newLength = arr.push(4, 5); // arr = [1, 2, 3, 4, 5], newLength = 5
```

> `pop` → Removes the last element from an array and returns that element.

```js
const arr = [1, 2, 3];
const lastElement = arr.pop(); // arr = [1, 2], lastElement = 3
```

### 8. shift, unshift

> `shift` → Removes the first element from an array and returns that element.

```js
const arr = [1, 2, 3];
const firstElement = arr.shift(); // arr = [2, 3], firstElement = 1
``` 

> `unshift` → Adds one or more elements to the beginning of an array and returns the new length of the array.

```js
const arr = [2, 3];
const newLength = arr.unshift(0, 1); // arr = [0, 1, 2, 3], newLength = 4
```

### 9. indexOf, lastIndexOf

> `indexOf` → Returns the first index at which a given element can be found in the array, or -1 if it is not present.

```js
const arr = [1, 2, 3, 2];
const firstIndex = arr.indexOf(2); // 1
const notFoundIndex = arr.indexOf(4); // -1
```

> `lastIndexOf` → Returns the last index at which a given element can be found in the array, or -1 if it is not present.

```js
const arr = [1, 2, 3, 2];
const lastIndex = arr.lastIndexOf(2); // 3
const notFoundIndex = arr.lastIndexOf(4); // -1
```

### 10. Array.from, Array.of, and Array.isArray

> `Array.from` → Creates a new array instance from an array-like or iterable object.

```js
const str = "hello";
const arrFromStr = Array.from(str); // ['h', 'e', 'l', 'l', 'o']
const set = new Set([1, 2, 3]);
console.log(set); // Set(3) { 1, 2, 3 }
const arrFromSet = Array.from(set); // [1, 2, 3]
```

> `Array.of` → Creates a new array instance with a variable number of arguments, regardless of number or type of the arguments.

```js
const arr = Array.of(1, 2, 3); // [1, 2, 3]
const arrSingle = Array.of(5); // [5]
const arrEmpty = Array.of(); // []
const arrMixed = Array.of(1, 'a', true, {id: 101}); // [1, 'a', true, {id: 101}]
```

> `Array.isArray` → Checks if a value is an array.

```js
console.log(Array.isArray([1, 2, 3])); // true
console.log(Array.isArray('hello')); // false
console.log(Array.isArray({})); // false
```

## 3. Object Utility Methods

### 0. for...in

> `for...in` → Iterates over the enumerable properties of an object.

```js
const user = { name: "Alice", age: 30, city: "New York" };
for (const key in user) {
    console.log(`${key}: ${user[key]}`);
}
// Output:
// name: Alice
// age: 30
// city: New York
```

### 1. Object.entries, Object.keys, Object.values, Object.assign, Object.freeze, Object.seal, Object.hasOwn

> `Object.entries` →  Returns key-value pairs as arrays.

```js
const user = { name: "Alice", age: 30, city: "New York" };
const entries = Object.entries(user); // [['name', 'Alice'], ['age', 30], ['city', 'New York']]
```

> `Object.keys` → Returns an array of property names..

```js
const user = { name: "Alice", age: 30, city: "New York" };  
const keys = Object.keys(user); // ['name', 'age', 'city']
```

> `Object.values` → Returns an array of property values.

```js
const user = { name: "Alice", age: 30, city: "New York" };  
const values = Object.values(user); // ['Alice', 30, 'New York']
```

> Object.assign → Copies properties from one object to another.

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };      
Object.assign(target, source); // target = { a: 1, b: 4, c: 5 }

// Object.assign(target, source1, source2, ...) → Copies properties from one or more source objects to a target object.
```

> Object.freeze → Makes an object immutable..

```js
const obj = { name: "Alice" };
Object.freeze(obj);
obj.name = "Bob"; // This will not change the name property
console.log(obj.name); // Output: "Alice"
```

> Object.seal → Prevents adding or removing properties from an object, but allows modifying existing properties.

```js
const obj = { name: "Alice" };
Object.seal(obj);
obj.name = "Bob"; // This will change the name property
obj.age = 30; // This will not add a new property
console.log(obj); // Output: { name: "Bob" }
```

> Object.hasOwn → Checks if an object has a specific property as its own (not inherited).

```js
const obj = { name: "Alice" };
console.log(Object.hasOwn(obj, "name")); // true
console.log(Object.hasOwn(obj, "age")); // false
```

### 4. new Map, new Set, and Object.fromEntries

> `new Map` → A collection of key-value pairs where keys can be any type (objects, functions, primitives).

```js
const map = new Map();
map.set('name', 'Alice');
map.set('age', 30);
console.log(map.get('name')); // Output: 'Alice'
console.log(map.has('age')); // Output: true
console.log(map.size); // Output: 2
```

> `new Set` → A collection of unique values (no duplicates).

```js
const set = new Set();
set.add(1);
set.add(2);
set.add(2); // Duplicate value, will not be added
console.log(set.has(1)); // Output: true
console.log(set.size); // Output: 2
```

> `Object.fromEntries` → Converts a list of key-value pairs into an object.

```js
const entries = [['name', 'Alice'], ['age', 30]];
const obj = Object.fromEntries(entries); 
console.log(obj); // Output: { name: 'Alice', age: 30 }
```

#### ➡️ Understanding the role of `Map` and `Set`: 

```js
const employees = [
  { name: "Alice Smith", department: "Engineering", salary: 85000 },
  { name: "Bob Jones", department: "Finance", salary: 62000 },
  { name: "Charlie Brown", department: "HR", salary: 55000 },
  { name: "Diana Prince", department: "Finance", salary: 95000 }
];
```

> Using Map for grouping: (Group employees by department)

```js
const deptMap = new Map();

employees.forEach(emp => {
  if (!deptMap.has(emp.department)) {
    deptMap.set(emp.department, []);
  }
  deptMap.get(emp.department).push(emp);
});

console.log(deptMap.get("Finance"));
// [
//   { name: "Bob Jones", department: "Finance", salary: 62000 },
//   { name: "Diana Prince", department: "Finance", salary: 95000 }
// ]
```

Why use `Map` here?

- Keys (department) are flexible.
- Easy to check existence with .has().
- Values can be arrays, objects, or anything.

> Using Set for unique values:

```js
// Find all unique departments.
const uniqueDepartments = new Set(employees.map(emp => emp.department));
console.log(uniqueDepartments); // Set { 'Engineering', 'Finance', 'HR' }

// Check if a department exists
console.log(uniqueDepartments.has('Finance')); // true
console.log(uniqueDepartments.has('Marketing')); // false
```

Why use `Set` here?
- Automatically removes duplicates.
- Provides easy methods like .has() to check existence.
- Cleaner than filtering arrays repetitively.

Use `Map` when you need `key → value` associations (like grouping employees by department).

Use `Set` when you need `unique collections` (like listing distinct departments).


```js
const employees = [
  { name: "Alice Smith", department: "Engineering", salary: 85000 },
  { name: "Bob Jones", department: "Finance", salary: 62000 },
  { name: "Charlie Brown", department: "HR", salary: 55000 },
  { name: "Diana Prince", department: "Finance", salary: 95000 }
];
```

> Example: use a Map to store department-level stats: total salary, headcount, and average salary.:

```js
const deptMap = new Map();

employees.forEach(emp => {
  if (!deptMap.has(emp.department)) {
    deptMap.set(emp.department, { total: 0, count: 0 });
  }
  const stats = deptMap.get(emp.department);
  stats.total += emp.salary;
  stats.count++;
});

deptMap.forEach((stats, dept) => {
  console.log(`${dept} average salary: ${stats.total / stats.count}`);
});

// Output
// Engineering average salary: 85000
// Finance average salary: 78500
// HR average salary: 55000

deptStats.forEach((stats, dept) => {
  stats.average = stats.total / stats.count;
});
console.log(deptStats);

// Output

/*
Map {
  "Engineering" => { total: 85000, count: 1, average: 85000 },
  "Finance" => { total: 157000, count: 2, average: 78500 },
  "HR" => { total: 55000, count: 1, average: 55000 }
}
*/
```

> HR can track which departments currently have job openings (Set)

```js
const openDepartments = new Set(["Engineering", "HR"]);

console.log(openDepartments.has("Finance")); // false
console.log(openDepartments.has("HR"));      // true

deptStats.forEach((stats, dept) => {
  const opening = openDepartments.has(dept) ? "OPEN" : "Closed";
  console.log(`${dept}: ${stats.count} employees, Avg Salary = ${stats.average}, Status = ${opening}`);
});

// Output:
// Engineering: 1 employees, Avg Salary = 85000, Status = OPEN
// Finance: 2 employees, Avg Salary = 78500, Status = Closed
// HR: 1 employees, Avg Salary = 55000, Status = OPEN
```

> Dynamic HR Dashboard: Combine Map and Set for real-time insights.

```js

// 🧩 Step 1: Data Setup
const employees = [
  { name: "Alice Smith", department: "Engineering", salary: 85000 },
  { name: "Bob Jones", department: "Finance", salary: 62000 },
  { name: "Charlie Brown", department: "HR", salary: 55000 },
  { name: "Diana Prince", department: "Finance", salary: 95000 }
];

const openDepartments = new Set(["Engineering", "HR"]);

// 🧮 Step 2: Compute Department Stats with Map
const deptStats = new Map();

employees.forEach(emp => {
  if (!deptStats.has(emp.department)) {
    deptStats.set(emp.department, { total: 0, count: 0 });
  }
  const stats = deptStats.get(emp.department);
  stats.total += emp.salary;
  stats.count++;
});

deptStats.forEach((stats, dept) => {
  stats.average = stats.total / stats.count;
});

// 📊 Step 3: Render Dashboard (HTML + Chart.js)
<div>
  <canvas id="salaryChart"></canvas>
  <table>
    <tr><th>Department</th><th>Employees</th><th>Avg Salary</th><th>Status</th></tr>
    <tbody id="deptTable"></tbody>
  </table>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
  const labels = [];
  const data = [];

  deptStats.forEach((stats, dept) => {
    labels.push(dept);
    data.push(stats.average);

    const status = openDepartments.has(dept) ? "OPEN" : "Closed";
    document.getElementById("deptTable").innerHTML += `
      <tr>
        <td>${dept}</td>
        <td>${stats.count}</td>
        <td>$${stats.average}</td>
        <td>${status}</td>
      </tr>`;
  });

  new Chart(document.getElementById("salaryChart"), {
    type: "bar",
    data: {
      labels,
      datasets: [{
        label: "Average Salary by Department",
        data,
        backgroundColor: ["#4e79a7", "#59a14f", "#f28e2b"]
      }]
    }
  });
</script>
```
