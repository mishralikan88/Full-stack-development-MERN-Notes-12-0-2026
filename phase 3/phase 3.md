Phase 3 — JavaScript


#### JAVASCRIPT CORE

6.1 Variables: var, let, const

JavaScript gives us three ways to declare variables:

var oldWay = "JavaScript";
let age = 30;
const company = "ABC";

For modern development:

const → DEFAULT choice
let   → when reassignment is required
var   → normally avoid, but MASTER for interviews
1. let

let allows reassignment:

let count = 10;


count = 20;


console.log(count);

Output:

20

But you cannot redeclare it in the same scope:

let count = 10;
let count = 20; // ❌ SyntaxError
2. const

const cannot be reassigned:

const age = 30;


age = 31; // ❌ TypeError

But this is allowed:

const user = {
    name: "John"
};


user.name = "David";


console.log(user.name);

Output:

David

Why?

Because we didn't do:

user = {};

We changed the contents of the existing object.

So:

const prevents reassignment. It does NOT automatically make objects immutable.

🔥 Very common interview question.

3. var
var age = 30;


age = 40;      // ✅
var age = 50;  // ✅


console.log(age);

Output:

50

var allows both:

reassignment ✅
redeclaration ✅

4. Biggest difference — Scope

let and const are block scoped.

if (true) {
    let a = 10;
    const b = 20;
}


console.log(a); // ❌
console.log(b); // ❌

The { } creates a block.

But var is function scoped, not block scoped:

if (true) {
    var a = 10;
}


console.log(a);

Output:

10

🔥 This is one major reason modern JavaScript prefers let/const.

5. Interview table
Feature	var	let	const
Reassign	✅	✅	❌
Redeclare same scope	✅	❌	❌
Block scoped	❌	✅	✅
Function scoped	✅	—	—
Hoisted	✅	✅	✅
TDZ	❌*	✅	✅
Modern default	❌	Sometimes	✅

var is hoisted and initialized with undefined. let and const are also hoisted, but cannot be accessed before initialization because of the Temporal Dead Zone (TDZ).

We'll study hoisting/TDZ deeply under JavaScript Internals, exactly where your syllabus places them.

6. Senior mentality

Don't say:

"const is constant."

Better interview answer:

"const prevents reassignment of the binding. If it references an object or array, the object's contents can still be mutated."

And don't say:

"let is better than var because it's newer."

Say:

"let provides block scoping, prevents same-scope redeclaration, and avoids several confusing behaviors associated with var."

That's a senior-quality answer.

# >>


6.2 — JavaScript Data Types

JavaScript has 8 data types.

7 Primitive Types
// 1. String
const name = "Likan";


// 2. Number
const age = 33;


// 3. Boolean
const isActive = true;


// 4. Undefined
let salary;


// 5. Null
const selectedUser = null;


// 6. BigInt
const bigNumber = 12345678901234567890n;


// 7. Symbol
const id = Symbol("id");

And one non-primitive category:

// 8. Object
const user = {
    name: "Likan",
    age: 33
};

Arrays and functions are also related to the object system:

const skills = ["HTML", "JavaScript"];


function greet() {
    console.log("Hello");
}
Primitive vs Object

This distinction is very important for interviews.

Primitive values behave like values:

let a = 10;
let b = a;


b = 20;


console.log(a); // 10
console.log(b); // 20

Changing b doesn't change a.

With objects:

const user1 = {
    name: "John"
};


const user2 = user1;


user2.name = "David";


console.log(user1.name); // David

Both variables refer to the same object.

Senior-level wording:

JavaScript passes values. For an object, that value points to the object. So two variables can point to the same object, and a change through one can be seen through the other.

typeof - 

We can inspect types:

typeof "hello";       // "string"
typeof 100;           // "number"
typeof true;          // "boolean"
typeof undefined;     // "undefined"
typeof 100n;          // "bigint"
typeof Symbol();      // "symbol"
typeof {};            // "object"
typeof function() {}; // "function"
Famous JavaScript interview trap
typeof null;

returns:

"object"

This is a historical JavaScript behavior.

So don't check null like this:

typeof value === "object"

alone.

Use:

value === null

when specifically checking for null.

Array trap
typeof [];

returns:

"object"

To detect an array:

Array.isArray([]);

returns:

true
Remember these 5 points
JavaScript has 7 primitive types.
Objects are non-primitive.
typeof null             → "object"
typeof []               → "object"
Array.isArray([])       → true

And:

primitive copy → independent value


object copy → copied reference value
              → both can point to same object

That's enough for 6.2 Data Types at this stage. The deeper memory/reference behavior will naturally come back under JavaScript internals.

# >>


6.3 — Type Conversion & Type Coercion 🔄

This is important because in real applications we constantly receive values from forms, APIs, URL parameters, localStorage, etc.

1. Type Conversion = we change it ourselves

Imagine an input gives us:

const age = "30";

It's a string:

console.log(typeof age); // "string"

But we need a number:

const convertedAge = Number(age);


console.log(convertedAge);        // 30
console.log(typeof convertedAge); // "number"

Common conversions:

Number("100");     // 100
String(100);       // "100"
Boolean(1);        // true
Boolean(0);        // false

This is explicit conversion because we asked JavaScript to convert it.

2. Type Coercion = JavaScript converts automatically

Look at:

console.log("10" + 5);

Result:

105

JavaScript sees a string and +, so 5 becomes "5":

"10" + "5"
     ↓
   "105"

But:

console.log("10" - 5);

Result:

5

Here JavaScript converts "10" to number 10.

That's coercion — JavaScript did the conversion automatically.

3. Real project example

Form values commonly arrive as strings.

<input type="text" id="salary">

Suppose the user enters 50000.

You may receive:

const salary = "50000";

Doing this could cause a bug:

console.log(salary + 5000);


// "500005000" ❌

Instead:

const salaryNumber = Number(salary);


console.log(salaryNumber + 5000);


// 55000 ✅

That's the practical reason you need to understand conversion.

4. Number() cases you should know
Number("100");       // 100
Number("10.5");      // 10.5
Number("");          // 0
Number("hello");     // NaN
Number(true);        // 1
Number(false);       // 0
Number(null);        // 0
Number(undefined);   // NaN
What is NaN?
const result = Number("hello");


console.log(result); // NaN

It means the conversion couldn't produce a valid number.

Strange but important:

typeof NaN; // "number"

To check it:

Number.isNaN(result);
5. Truthy and Falsy 🔥

This is used everywhere in JavaScript and React.

const user = "John";


if (user) {
    console.log("User exists");
}

JavaScript converts user to a boolean automatically.

You should know the main falsy values:

false
0
-0
0n
""
null
undefined
NaN

Everything else is generally truthy.

For example:

Boolean("");       // false
Boolean("hello");  // true


Boolean(0);        // false
Boolean(100);      // true


Boolean(null);     // false


Boolean([]);       // true
Boolean({});       // true

🔥 Important:

Boolean("false"); // true

Why?

Because "false" is a non-empty string.

6. == vs === 🔥🔥
5 == "5"

returns:

true

because == allows type conversion.

But:

5 === "5"

returns:

false

because the types are different:

5      → number
"5"    → string

In normal application code, prefer:

===

and:

!==

because the comparison is clearer and doesn't perform loose type coercion.

Easy interview answer

What's the difference between == and ===?

"== can convert the values before comparing them. === compares without that type conversion. I normally use === because its behavior is more predictable."

Easy English, but still a solid interview answer.

# >>>>


6.4 — JavaScript Operators 

Operators are simply symbols we use to calculate, compare, assign, or make decisions with values.

Don't memorize a giant list. Understand where you use them.

1. Arithmetic operators

Used for calculations:

const price = 100;
const quantity = 3;


console.log(price + quantity); // 103
console.log(price - quantity); // 97
console.log(price * quantity); // 300
console.log(price / quantity); // 33.33...
console.log(price % quantity); // 1
console.log(2 ** 3);           // 8

Important ones:

+    addition
-    subtraction
*    multiplication
/    division
%    remainder
**   power
% is very useful

For example, checking even/odd:

const number = 10;


if (number % 2 === 0) {
    console.log("Even");
}

Because:

10 % 2 → 0
2. Assignment operators

Basic:

let salary = 50000;


salary = 60000;

Short forms:

let count = 10;


count += 5; // count = count + 5
count -= 2;
count *= 2;
count /= 2;

Very common in loops/counters.

3. Increment / Decrement
let count = 1;


count++;
console.log(count); // 2


count--;
console.log(count); // 1

There is an important difference between:

count++;
++count;

Example:

let a = 5;


const b = a++;


console.log(a); // 6
console.log(b); // 5

a++ → use the old value first, then increase.

But:

let a = 5;


const b = ++a;


console.log(a); // 6
console.log(b); // 6

++a → increase first, then use the new value.

You don't need to mug this up. Just remember:

a++ → use → increase

++a → increase → use

4. Comparison operators 🔥

These return true or false.

10 > 5;     // true
10 < 5;     // false


10 >= 10;   // true
10 <= 5;    // false


10 === 10;  // true
10 !== 5;   // true

We already covered:

5 == "5";   // true
5 === "5";  // false

For normal project code, prefer:

===
!==
5. Logical operators 🔥🔥

Extremely common in JavaScript and React.

AND — &&

Both conditions must be true:

const isLoggedIn = true;
const isAdmin = true;


if (isLoggedIn && isAdmin) {
    console.log("Open admin page");
}

Think:

true && true  → true
true && false → false
OR — ||

At least one needs to be true:

const isAdmin = false;
const isManager = true;


if (isAdmin || isManager) {
    console.log("Access allowed");
}
NOT — !

Reverses the boolean result:

const isLoggedIn = false;


console.log(!isLoggedIn); // true

You'll see this constantly:

if (!user) {
    console.log("User not found");
}
6. Short-circuit behavior 🔥

&& and || don't always return true/false.

Example:

const user = "John";


const result = user && "Welcome";


console.log(result); // "Welcome"

JavaScript evaluates from left to right.

This is why React code often looks like:

{isLoggedIn && <Dashboard />}

Meaning:

If isLoggedIn is truthy, render Dashboard.

|| for fallback values
const username = "";


const displayName = username || "Guest";


console.log(displayName); // Guest

But there's a problem.

Imagine:

const quantity = 0;


const result = quantity || 10;


console.log(result); // 10

Maybe 0 is a valid quantity.

That's where the next operator helps.

7. Nullish coalescing — ?? 🔥
const quantity = 0;


const result = quantity ?? 10;


console.log(result); // 0

?? uses the fallback only when the left side is:

null
undefined

So:

0 ?? 10;          // 0
"" ?? "Guest";    // ""
false ?? true;    // false


null ?? "Guest";       // "Guest"
undefined ?? "Guest";  // "Guest"
|| vs ??

This is interview-important.

0 || 100;  // 100
0 ?? 100;  // 0

Easy explanation:

|| falls back for any falsy value. ?? falls back only for null or undefined.

In project code, ?? is useful when values such as 0, false, or "" are valid.

8. Optional chaining — ?. 🔥🔥

Very useful with API responses.

Suppose:

const employee = {
    name: "John",
    address: {
        city: "Mumbai"
    }
};

We can access:

employee.address.city;

But what if address doesn't exist?

This can crash:

employee.address.city;

Instead:

employee.address?.city;

If address is null or undefined, we get:

undefined

instead of trying to read city from it.

Very common:

const city = employee.address?.city ?? "Not provided";

Read this naturally:

Get the employee's city if available; otherwise use "Not provided".

9. Ternary operator 🔥

Short if/else when choosing a value:

const age = 20;


const status = age >= 18
    ? "Adult"
    : "Minor";


console.log(status); // Adult

Think:

condition ? whenTrue : whenFalse

Real example:

const message = isLoggedIn
    ? "Welcome back"
    : "Please login";

Avoid giant nested ternaries. If the logic becomes difficult to read, use normal conditions.

10. typeof

We already used this:

typeof "hello"; // "string"
typeof 100;     // "number"
typeof true;    // "boolean"

typeof is technically an operator, which is why I'm mentioning it again here.

11. Senior interview situations to understand

You don't need complicated wording.

Why prefer === over ==?

== can convert types before comparing. === doesn't, so I normally use === because the result is easier to predict.

|| vs ???

|| treats all falsy values as needing a fallback. ?? only uses the fallback for null or undefined.

Why use ?.?

It safely accesses something that may be missing, which is common with API data.

That's enough wording.

# >> 

6.5 — CONDITIONS

Conditions allow JavaScript to make decisions.

Simple idea:

If something is true
    → do this


Otherwise
    → do something else
1. if

Use if when code should execute only when a condition is true.

const age = 20;


if (age >= 18) {
    console.log("Eligible");
}

Output:

Eligible
Flow
age = 20


age >= 18
   ↓
20 >= 18
   ↓
true
   ↓
execute if block

If:

const age = 15;


if (age >= 18) {
    console.log("Eligible");
}

Condition:

15 >= 18
↓
false

The block is skipped.

Formula
if (condition) {
    // runs when condition is true
}
2. if...else

Use else when you want another block to execute when the condition is false.

const age = 16;


if (age >= 18) {
    console.log("Adult");
} else {
    console.log("Minor");
}

Output:

Minor

Flow:

age >= 18
    ↓
  false
    ↓
   else
    ↓
"Minor"

Only one of these blocks executes.

3. else if

Use else if when there are multiple conditions.

const marks = 75;


if (marks >= 90) {
    console.log("Grade A");
} else if (marks >= 70) {
    console.log("Grade B");
} else if (marks >= 50) {
    console.log("Grade C");
} else {
    console.log("Fail");
}

Output:

Grade B
How JavaScript checks it
marks = 75


75 >= 90
❌ false


75 >= 70
✅ true


Print "Grade B"


STOP

JavaScript doesn't continue checking the remaining else if blocks after one condition matches.

4. Order of conditions matters 🔥

Consider:

const marks = 95;


if (marks >= 50) {
    console.log("Pass");
} else if (marks >= 90) {
    console.log("Excellent");
}

Output:

Pass

Even though:

95 >= 90

is true.

Why?

The first condition:

marks >= 50

is already true.

JavaScript executes it and stops checking the chain.

Better:

if (marks >= 90) {
    console.log("Excellent");
} else if (marks >= 50) {
    console.log("Pass");
}

So generally check the more specific/stronger condition first when conditions overlap.

5. Truthy and Falsy in conditions 🔥

Conditions don't always need to contain something like:

age >= 18

You can write:

const name = "John";


if (name) {
    console.log("Name exists");
}

Output:

Name exists

Why?

Because "John" is truthy.

Important falsy values:

false
0
-0
0n
""
null
undefined
NaN

Everything else is generally truthy.

Example:

const username = "";


if (username) {
    console.log("Username exists");
} else {
    console.log("Username missing");
}

Output:

Username missing

because:

"" → falsy
6. Logical AND && inside conditions
const age = 25;
const hasLicense = true;


if (age >= 18 && hasLicense) {
    console.log("Can drive");
}

&& means:

Both conditions must be truthy.

age >= 18       → true
hasLicense      → true


true && true
     ↓
   true
7. Logical OR ||
const isAdmin = false;
const isManager = true;


if (isAdmin || isManager) {
    console.log("Access granted");
}

|| means:

At least one condition must be truthy.

false || true
      ↓
    true

So:

Access granted
8. NOT !

! reverses truthiness.

const isLoggedIn = false;


if (!isLoggedIn) {
    console.log("Please login");
}

Here:

isLoggedIn
→ false


!isLoggedIn
→ true

So the block executes.

9. Ternary Operator

A ternary is a shorter way to choose between two values/expressions based on a condition.

Instead of:

const age = 20;
let status;


if (age >= 18) {
    status = "Adult";
} else {
    status = "Minor";
}

we can write:

const age = 20;


const status = age >= 18 ? "Adult" : "Minor";
Formula
condition ? valueIfTrue : valueIfFalse

So:

age >= 18 ? "Adult" : "Minor"

means:

Is age >= 18?


YES → "Adult"
NO  → "Minor"

Don't use deeply nested ternaries just to make code shorter. For complicated decision logic, if/else is usually easier to read.

10. switch

switch is useful when you're comparing one expression against several exact values.

Example:

const role = "admin";


switch (role) {
    case "admin":
        console.log("Admin Dashboard");
        break;


    case "manager":
        console.log("Manager Dashboard");
        break;


    case "employee":
        console.log("Employee Dashboard");
        break;


    default:
        console.log("Unknown Role");
}

Output:

Admin Dashboard
Flow
role = "admin"


       ↓


case "admin"
     ↓
match ✅
     ↓
execute code
     ↓
break
     ↓
leave switch
11. Why break?

Look:

const role = "admin";


switch (role) {
    case "admin":
        console.log("Admin");


    case "manager":
        console.log("Manager");


    default:
        console.log("Unknown");
}

Without break, after matching "admin", execution continues into following cases.

Output:

Admin
Manager
Unknown

This is called fall-through.

Usually:

break;

stops that:

case "admin":
    console.log("Admin");
    break;

Intentional fall-through exists, but don't use it accidentally.

12. default

default runs when no case matches.

const role = "guest";


switch (role) {
    case "admin":
        console.log("Admin");
        break;


    case "manager":
        console.log("Manager");
        break;


    default:
        console.log("Unknown Role");
}

Output:

Unknown Role
13. switch uses strict matching

An important interview point:

const value = "1";


switch (value) {
    case 1:
        console.log("Number");
        break;


    case "1":
        console.log("String");
        break;
}

Output:

String

Because switch case matching behaves like strict equality:

"1" === 1
→ false


"1" === "1"
→ true
14. if/else vs switch

Use if/else when you're dealing with ranges or complex conditions:

if (salary > 100000 && experience > 10) {
    // ...
}

switch can be convenient when checking one expression against several exact values:

switch (role) {
    case "admin":
    case "manager":
    case "employee":
}

It's mainly about writing clear code.

15. Nested conditions

You can put an if inside another if:

const isLoggedIn = true;
const isAdmin = true;


if (isLoggedIn) {


    if (isAdmin) {
        console.log("Admin Dashboard");
    }


}

But excessive nesting:

if
 └── if
      └── if
           └── if

makes code difficult to read.

In real applications, we often simplify such logic with early returns where appropriate.

16. Interview traps 🔥
0 is falsy
if (0) {
    console.log("Yes");
} else {
    console.log("No");
}

Output:

No
"0" is truthy
if ("0") {
    console.log("Yes");
}

Output:

Yes

Because "0" is a non-empty string.

Empty array is truthy
if ([]) {
    console.log("Yes");
}

Output:

Yes
Empty object is truthy
if ({}) {
    console.log("Yes");
}

Output:

Yes

So remember:

[]  → truthy
{}  → truthy
""  → falsy
0   → falsy
Interview answers
What are conditional statements?

Conditional statements allow JavaScript to execute different code based on whether conditions are true or false.

Difference between if and switch?

if/else works well for ranges and complex boolean conditions. switch is useful when comparing one expression against multiple exact values.

What is a ternary operator?

A ternary is a conditional expression using condition ? valueIfTrue : valueIfFalse.

What are truthy and falsy values?

JavaScript converts values to boolean when they're used in a condition. Values behaving as true are truthy; values behaving as false are falsy.

🧠 Final Conditions Cheat Sheet
if
→ run when condition is true


if / else
→ choose between two paths


else if
→ multiple conditions


&&
→ both must be truthy


||
→ at least one must be truthy


!
→ reverse truthiness


condition ? A : B
→ ternary


switch
→ compare one expression against cases


break
→ exit switch / prevent fall-through


default
→ no case matched
FALSY:


false
0
-0
0n
""
null
undefined
NaN


Everything else → generally TRUTHY

# >>

6.6 — LOOPS

Loops allow us to run the same block of code multiple times.

Instead of writing:

console.log(1);
console.log(2);
console.log(3);
console.log(4);
console.log(5);

we can use:

for (let i = 1; i <= 5; i++) {
    console.log(i);
}

Output:

1
2
3
4
5
1. Why do we need loops?

Imagine an array:

const employees = ["John", "David", "Sam"];

We could write:

console.log(employees[0]);
console.log(employees[1]);
console.log(employees[2]);

But what if there are 1000 employees?

We use a loop.

for (let i = 0; i < employees.length; i++) {
    console.log(employees[i]);
}

Output:

John
David
Sam
2. for loop 🔥

Syntax:

for (initialization; condition; update) {
    // code
}

Example:

for (let i = 1; i <= 5; i++) {
    console.log(i);
}

Break it into three parts:

let i = 1
   ↓
Starting point


i <= 5
   ↓
Continue while this is true


i++
   ↓
Increase i after every iteration
3. Exact execution flow

Take:

for (let i = 1; i <= 3; i++) {
    console.log(i);
}
Start
i = 1

Check:

1 <= 3 → true

Run:

console.log(1);

Then:

i++
↓
i = 2

Check again:

2 <= 3 → true

Print:

2

Increase:

i = 3

Check:

3 <= 3 → true

Print:

3

Increase:

i = 4

Check:

4 <= 3 → false

Loop stops.

Formula
Initialize
    ↓
Check condition
    ↓ true
Run code
    ↓
Update
    ↓
Check condition again
    ↓
...
    ↓ false
STOP
4. Loop through an array
const fruits = ["Apple", "Mango", "Orange"];


for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}

Remember arrays start at:

index 0

So:

i = 0 → fruits[0] → Apple
i = 1 → fruits[1] → Mango
i = 2 → fruits[2] → Orange

Why:

i < fruits.length

and not:

i <= fruits.length

Because:

fruits.length = 3


Valid indexes:
0
1
2

fruits[3] is:

undefined

🔥 This is a common off-by-one error.

5. while loop

Syntax:

while (condition) {
    // code
}

Example:

let i = 1;


while (i <= 5) {
    console.log(i);
    i++;
}

Output:

1
2
3
4
5

Flow:

i = 1


while (i <= 5)
       ↓
     true
       ↓
   print i
       ↓
     i++
       ↓
check again
6. for vs while

Use a for loop when the initialization, condition and update naturally belong together:

for (let i = 0; i < 10; i++) {
    // ...
}

A while loop is useful when the important idea is:

Keep doing something while this condition remains true.

Example:

let attempts = 0;


while (attempts < 3) {
    console.log("Trying...");
    attempts++;
}
7. do...while

A do...while executes the code at least once.

let i = 1;


do {
    console.log(i);
    i++;
} while (i <= 5);

Output:

1
2
3
4
5

The important difference is the order.

while
Check condition
     ↓
Run code
do...while
Run code
     ↓
Check condition

That's why do...while runs at least once.

8. Important example
let i = 10;


while (i < 5) {
    console.log(i);
}

Output:

Nothing

Because:

10 < 5 → false

But:

let i = 10;


do {
    console.log(i);
} while (i < 5);

Output:

10

Even though the condition is false.

Why?

Because do runs before checking the condition.

9. break 🔥

break completely stops a loop.

for (let i = 1; i <= 10; i++) {


    if (i === 5) {
        break;
    }


    console.log(i);
}

Output:

1
2
3
4

When:

i = 5

this becomes true:

if (i === 5)

Then:

break;

means:

Exit the loop completely.

10. continue

continue does not stop the whole loop.

It skips the current iteration and continues with the next one.

for (let i = 1; i <= 5; i++) {


    if (i === 3) {
        continue;
    }


    console.log(i);
}

Output:

1
2
4
5

When:

i = 3

continue says:

Skip this iteration
       ↓
continue with i = 4
break vs continue
break
→ STOP the entire loop


continue
→ SKIP current iteration
→ keep looping
11. for...of 🔥

Very useful when you want the values from an iterable such as an array.

const employees = ["John", "David", "Sam"];


for (const employee of employees) {
    console.log(employee);
}

Output:

John
David
Sam

Easy formula:

for (const VALUE of ARRAY)

Example:

for (const employee of employees)

Each time:

employee → John
employee → David
employee → Sam

No manual index needed.

12. for...in

for...in iterates over enumerable property keys.

Example with an object:

const employee = {
    name: "John",
    age: 30,
    role: "Developer"
};


for (const key in employee) {
    console.log(key);
}

Output:

name
age
role

To get the values:

for (const key in employee) {
    console.log(employee[key]);
}

Output:

John
30
Developer

Why bracket notation?

Because:

employee[key]

uses the value stored inside key.

For example:

key = "name"


employee[key]
     ↓
employee["name"]
     ↓
"John"
13. for...of vs for...in 🔥

This is important.

Array:

const fruits = ["Apple", "Mango", "Orange"];
for...of
for (const fruit of fruits) {
    console.log(fruit);
}

Gives:

Apple
Mango
Orange

Think:

OF → values

for...in
for (const index in fruits) {
    console.log(index);
}

Gives property keys, which for this simple array are:

0
1
2

Think:

IN → keys/property names

For normal array iteration, prefer:

for...of

over for...in.

14. Nested loops

A loop can exist inside another loop.

for (let i = 1; i <= 3; i++) {


    for (let j = 1; j <= 2; j++) {
        console.log(i, j);
    }


}

Output:

1 1
1 2


2 1
2 2


3 1
3 2

Flow:

i = 1
   ↓
   j = 1
   j = 2


i = 2
   ↓
   j = 1
   j = 2


i = 3
   ↓
   j = 1
   j = 2

Nested loops are common in coding problems, but don't use them unnecessarily because they can increase work significantly as input grows.

15. Infinite loop ⚠️

This is dangerous:

let i = 1;


while (i <= 5) {
    console.log(i);
}

What's missing?

i++;

So:

i = 1


1 <= 5 → true
print 1


i is STILL 1


1 <= 5 → true
print 1


...

It never stops.

That's an infinite loop.

Always make sure your loop has a way to eventually reach its stopping condition.

16. forEach() — related but different

Arrays also have:

const employees = ["John", "David", "Sam"];


employees.forEach(function (employee) {
    console.log(employee);
});

Output:

John
David
Sam

forEach() is an array method, not a loop statement like for.

Important difference:

You cannot use normal:

break;
continue;

inside a forEach() callback the way you do inside a for/for...of loop.

We'll keep array methods separate because you already covered them deeply in Arrays — Master.

Interview Questions 🔥
What is a loop?

A loop repeatedly executes code while its looping condition/rules allow it.

while vs do...while?

while checks the condition before executing. do...while executes once first and then checks the condition.

break vs continue?

break exits the loop completely. continue skips the current iteration and continues with the next one.

for...of vs for...in?

for...of gives values from an iterable. for...in gives enumerable property keys.

🧠 Final Loops Cheat Sheet
for
→ general counter/index-based looping


while
→ repeat while condition is true


do...while
→ execute at least once


break
→ exit entire loop


continue
→ skip current iteration


for...of
→ VALUES from iterable
→ commonly arrays


for...in
→ PROPERTY KEYS
→ commonly used with objects


nested loop
→ loop inside loop


infinite loop
→ stopping condition is never reached
Most important formula
FOR LOOP


Initialize
    ↓
Condition?
    ↓ YES
Run code
    ↓
Update
    ↓
Condition again


    ↓ NO


STOP


# >>>>>>>>

6.7 — FUNCTIONS

A function is a reusable block of code that performs a task.

Instead of repeating:

console.log("Hello John");
console.log("Welcome to the application");

for every employee, we can create a function.

function greet() {
    console.log("Hello John");
    console.log("Welcome to the application");
}


greet();

Output:

Hello John
Welcome to the application
1. Function Declaration

Basic syntax:

function greet() {
    console.log("Hello");
}

Here:

function → keyword
greet    → function name
()       → parameters go here
{}       → function body

Creating the function does not execute it.

function greet() {
    console.log("Hello");
}

To execute it:

greet();

This is called calling/invoking the function.

2. Parameters and Arguments 🔥

Consider:

function greet(name) {
    console.log("Hello " + name);
}


greet("John");

Output:

Hello John

Here:

function greet(name)
               ↑
           parameter

And:

greet("John");
       ↑
    argument
Easy formula
PARAMETER
→ variable written when defining the function


ARGUMENT
→ actual value passed when calling the function

Example:

function add(a, b) {
    console.log(a + b);
}


add(10, 20);

Here:

a, b     → parameters


10, 20   → arguments

During execution:

a = 10
b = 20

Therefore:

a + b
↓
10 + 20
↓
30
3. Multiple function calls

The same function can be reused:

function greet(name) {
    console.log("Hello " + name);
}


greet("John");
greet("David");
greet("Sam");

Output:

Hello John
Hello David
Hello Sam

That's one major reason functions exist:

Write logic once and reuse it.

4. return 🔥🔥

This is extremely important.

Consider:

function add(a, b) {
    return a + b;
}


const result = add(10, 20);


console.log(result);

Output:

30

Flow:

add(10, 20)
     ↓
a = 10
b = 20
     ↓
a + b
     ↓
30
     ↓
return 30
     ↓
result = 30

Think:

argument goes IN
      ↓
   function
      ↓
result comes OUT
5. console.log() vs return

This is important.

Using console.log
function add(a, b) {
    console.log(a + b);
}


const result = add(10, 20);

It prints:

30

But:

console.log(result);

prints:

undefined

Why?

Because the function didn't return anything.

Using return
function add(a, b) {
    return a + b;
}


const result = add(10, 20);

Now:

result = 30

So remember:

console.log()
→ displays something


return
→ sends a value back from the function
6. return stops the function 🔥

Consider:

function test() {


    console.log("A");


    return;


    console.log("B");
}


test();

Output:

A

B never executes.

Why?

console.log("A")
       ↓
return
       ↓
FUNCTION ENDS

Anything after that return in the same execution path is unreachable.

7. Early Return

This is useful in real applications.

function checkAge(age) {


    if (age < 18) {
        return "Not allowed";
    }


    return "Allowed";
}

Now:

console.log(checkAge(15));

Output:

Not allowed

Because:

15 < 18
↓
true
↓
return "Not allowed"
↓
function ends

For:

console.log(checkAge(25));

Output:

Allowed

Early returns can help avoid unnecessary nested conditions.

8. Default Parameters

Suppose:

function greet(name) {
    console.log("Hello " + name);
}


greet();

Output:

Hello undefined

Because no argument was provided.

We can provide a default:

function greet(name = "Guest") {
    console.log("Hello " + name);
}

Now:

greet();

Output:

Hello Guest

But:

greet("John");

Output:

Hello John

So:

argument provided
→ use argument


argument missing / undefined
→ use default value
9. Rest Parameters ...

Suppose we don't know how many arguments will be passed.

function total(...numbers) {
    console.log(numbers);
}


total(10, 20, 30);

Output:

[10, 20, 30]

The rest parameter:

...numbers

collects remaining arguments into an array.

So:

10, 20, 30
     ↓
...numbers
     ↓
[10, 20, 30]

Example:

function total(...numbers) {
    return numbers.reduce(function (sum, number) {
        return sum + number;
    }, 0);
}


console.log(total(10, 20, 30));

Output:

60
10. Function Declaration vs Function Expression 🔥
Function Declaration
function greet() {
    console.log("Hello");
}
Function Expression
const greet = function () {
    console.log("Hello");
};

Here:

function () {
    console.log("Hello");
}

is a function without its own written name in that expression.

We store it in:

const greet

Then:

greet();

executes it.

11. Arrow Function

Modern JavaScript also provides arrow functions.

Normal function:

function add(a, b) {
    return a + b;
}

Arrow version:

const add = (a, b) => {
    return a + b;
};

Short version:

const add = (a, b) => a + b;

All can produce:

add(10, 20);

→

30

We'll treat arrow functions more deeply in Functions — Deep Practice, especially because their this behavior is different.

12. Callback Function

You already know the basic concept.

function greet(callback) {
    console.log("Hello");


    callback();
}


function finish() {
    console.log("Finished");
}


greet(finish);

Remember:

finish
→ pass the function


finish()
→ execute the function

Here:

greet(finish);

passes finish.

Inside:

callback();

executes it.

We'll revisit async callbacks separately because that section is currently still undone.

13. Higher-Order Function

A higher-order function is a function that:

takes another function as an argument, or
returns another function.

Example:

function greet(callback) {
    callback();
}

greet receives another function:

greet(finish);

Therefore greet is a higher-order function.

And finish is the callback.

Easy relationship:

greet(finish)
  ↑      ↑
  │      └── Callback
  │
  └── Higher-order function
14. Pure Function

A pure function gives the same output for the same input and does not modify outside state.

function add(a, b) {
    return a + b;
}

Every time:

add(10, 20);

returns:

30

It doesn't change anything outside itself.

That's a simple pure function.

15. Impure Function

Example:

let total = 0;


function add(value) {
    total = total + value;
}

The function changes:

total

which exists outside the function.

Therefore it has a side effect and is not pure.

Don't interpret this as "impure functions are always bad." Real applications need side effects—API calls, DOM updates, logging, database writes, etc.

16. Recursion

Recursion means:

A function calls itself.

Example:

function countdown(number) {


    if (number === 0) {
        return;
    }


    console.log(number);


    countdown(number - 1);
}


countdown(3);

Flow:

countdown(3)
↓
print 3


countdown(2)
↓
print 2


countdown(1)
↓
print 1


countdown(0)
↓
return
↓
STOP

Output:

3
2
1

The stopping condition:

if (number === 0) {
    return;
}

is called the base case.

Without a proper base case, recursion can continue until the call stack limit is exceeded.

17. Functions are values in JavaScript 🔥

This is an important JavaScript concept.

You can store a function:

const greet = function () {
    console.log("Hello");
};

Pass a function:

someFunction(greet);

Return a function:

function createFunction() {


    return function () {
        console.log("Hello");
    };


}

This is why JavaScript supports things like:

Callbacks
Higher-order functions
map()
filter()
Promises
Event handlers
Interview Questions
What is a function?

A function is a reusable block of code designed to perform a task.

Parameter vs argument?

A parameter is defined in the function definition. An argument is the actual value supplied when calling the function.

return vs console.log()?

console.log() displays a value. return sends a value back to the caller and ends that function execution path.

What is a callback?

A callback is a function passed to another function so that the receiving function can call it.

What is a higher-order function?

A function that takes another function as an argument or returns a function.

What is recursion?

Recursion is when a function calls itself until a base condition stops it.

🧠 Final Functions Cheat Sheet
function greet() {}
→ function declaration


greet()
→ call / invoke function


function greet(name)
               ↑
             parameter


greet("John")
       ↑
    argument


return
→ send result back
→ ends current function execution


default parameter
→ name = "Guest"


rest parameter
→ ...values
→ collects arguments into array


function expression
→ const greet = function () {}


arrow function
→ const greet = () => {}


callback
→ function passed to another function


higher-order function
→ accepts or returns a function


pure function
→ same input → same output
→ no side effects


recursion
→ function calls itself


base case
→ stops recursion

# >>>

6.8 — ARRAYS

An array stores multiple values in one variable.

Instead of:

const employee1 = "John";
const employee2 = "David";
const employee3 = "Sam";

we can write:

const employees = ["John", "David", "Sam"];

Think:

employees
   ↓
["John", "David", "Sam"]
1. Creating an Array

Most common:

const fruits = ["Apple", "Mango", "Orange"];

An array can contain different data types:

const values = [
    "John",
    30,
    true,
    null
];

It can even contain objects:

const employees = [
    { id: 1, name: "John" },
    { id: 2, name: "David" }
];
2. Array Index 🔥

Array positions start from 0, not 1.

const fruits = ["Apple", "Mango", "Orange"];

Think:

Value      Index


Apple   →    0
Mango   →    1
Orange  →    2

Access a value:

console.log(fruits[0]);

Output:

Apple

Similarly:

console.log(fruits[1]); // Mango
console.log(fruits[2]); // Orange
3. Accessing an Invalid Index
const fruits = ["Apple", "Mango"];


console.log(fruits[10]);

Output:

undefined

JavaScript doesn't throw an error just because that array index doesn't exist.

4. Changing an Array Value
const fruits = ["Apple", "Mango", "Orange"];


fruits[1] = "Banana";


console.log(fruits);

Output:

["Apple", "Banana", "Orange"]

You might ask:

But fruits is const. How can we change it?

Because we are not assigning a new array to fruits.

This is allowed:

fruits[1] = "Banana";

This is not:

fruits = ["Grapes"];

because const prevents reassignment of the variable itself.

5. length
const fruits = ["Apple", "Mango", "Orange"];


console.log(fruits.length);

Output:

3

Remember:

length = number of elements

But last index is:

length - 1

So:

const fruits = ["Apple", "Mango", "Orange"];


console.log(fruits[fruits.length - 1]);

Output:

Orange

Because:

length = 3


3 - 1 = 2


fruits[2]
→ Orange
6. push() 🔥

Adds an element to the end.

const fruits = ["Apple", "Mango"];


fruits.push("Orange");


console.log(fruits);

Output:

["Apple", "Mango", "Orange"]

Formula:

push()
→ ADD at END
7. pop()

Removes the last element.

const fruits = ["Apple", "Mango", "Orange"];


const removedFruit = fruits.pop();


console.log(fruits);
console.log(removedFruit);

Output:

["Apple", "Mango"]


Orange

Formula:

pop()
→ REMOVE from END
8. unshift()

Adds an element to the beginning.

const fruits = ["Mango", "Orange"];


fruits.unshift("Apple");


console.log(fruits);

Output:

["Apple", "Mango", "Orange"]

Formula:

unshift()
→ ADD at START
9. shift()

Removes the first element.

const fruits = ["Apple", "Mango", "Orange"];


const removedFruit = fruits.shift();


console.log(fruits);

Output:

["Mango", "Orange"]

Formula:

shift()
→ REMOVE from START
Easy memory trick
START                  END


unshift() → ADD        push() → ADD


shift()   → REMOVE     pop()  → REMOVE
10. includes()

Checks whether an array contains a value.

const fruits = ["Apple", "Mango", "Orange"];


console.log(fruits.includes("Mango"));

Output:

true

But:

console.log(fruits.includes("Banana"));

Output:

false
11. indexOf()

Finds the index of a value.

const fruits = ["Apple", "Mango", "Orange"];


console.log(fruits.indexOf("Mango"));

Output:

1

If the value doesn't exist:

console.log(fruits.indexOf("Banana"));

Output:

-1

So:

Found
→ index


Not found
→ -1
12. slice() 🔥

slice() extracts part of an array without changing the original array.

const numbers = [10, 20, 30, 40, 50];


const result = numbers.slice(1, 4);


console.log(result);

Output:

[20, 30, 40]

Formula:

array.slice(start, end);

Important:

start → included
end   → excluded

So:

slice(1, 4)

takes:

index 1 → 20 ✅
index 2 → 30 ✅
index 3 → 40 ✅
index 4 → STOP ❌

Original array remains:

[10, 20, 30, 40, 50]
13. splice() 🔥

splice() can remove, add or replace elements.

Most importantly:

splice() changes the original array.

Example:

const numbers = [10, 20, 30, 40, 50];


numbers.splice(1, 2);


console.log(numbers);

Output:

[10, 40, 50]

What does:

splice(1, 2);

mean?

1 → start at index 1


2 → remove 2 elements

So it removes:

20
30
14. slice() vs splice() 🔥🔥

Very common interview question.

slice()
→ gets part of array
→ does NOT modify original array




splice()
→ add/remove/replace elements
→ DOES modify original array

Memory trick:

splice changes the source array.

15. concat()

Combines arrays.

const frontend = ["React", "Angular"];
const backend = ["Node", "Express"];


const technologies = frontend.concat(backend);


console.log(technologies);

Output:

["React", "Angular", "Node", "Express"]

Original arrays are not changed.

Modern code also commonly uses spread:

const technologies = [...frontend, ...backend];
16. Spread with Arrays ... 🔥

Spread means:

Take values out / expand them.

const numbers = [10, 20, 30];


console.log(...numbers);

Conceptually:

[10, 20, 30]
      ↓ spread


10, 20, 30

That's why this works:

Math.max(...numbers);

It becomes conceptually:

Math.max(10, 20, 30);

We already used this when learning apply().

17. Copy Array with Spread
const original = [10, 20, 30];


const copy = [...original];

Now:

original → [10, 20, 30]


copy     → [10, 20, 30]

They are different array objects.

console.log(original === copy);

Output:

false

However, spread creates a shallow copy.

We'll cover that properly under Objects.

18. Destructuring Arrays

Suppose:

const employees = ["John", "David", "Sam"];

Instead of:

const first = employees[0];
const second = employees[1];

we can write:

const [first, second] = employees;

Now:

first  → John
second → David
19. Skipping values during destructuring
const numbers = [10, 20, 30];


const [first, , third] = numbers;

Now:

first → 10
third → 30

The second value was skipped.

20. Rest with Array Destructuring
const numbers = [10, 20, 30, 40];


const [first, ...remaining] = numbers;

Result:

first
→ 10


remaining
→ [20, 30, 40]

Here:

...remaining

is rest.

It collects remaining values.

21. Spread vs Rest

Both use:

...

But the job is different.

Spread
const copy = [...numbers];

Think:

SPREAD
→ EXPAND values
Rest
const [first, ...remaining] = numbers;

Think:

REST
→ COLLECT remaining values

Easy:

Spread → EXPAND


Rest   → COLLECT
22. Loop through an Array

Traditional:

const fruits = ["Apple", "Mango", "Orange"];


for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}

Modern simple iteration:

for (const fruit of fruits) {
    console.log(fruit);
}

Output:

Apple
Mango
Orange
23. forEach()
const fruits = ["Apple", "Mango", "Orange"];


fruits.forEach(function (fruit) {
    console.log(fruit);
});

The function passed to forEach() is a:

callback

Conceptually:

Apple  → callback("Apple")
Mango  → callback("Mango")
Orange → callback("Orange")
24. Important Array Methods

You've already covered these separately in Arrays — Master:

map()
filter()
reduce()
find()
findIndex()
some()
every()
sort()
flat()
flatMap()

So we don't need to duplicate their full notes here.

But remember the purpose:

map()
→ transform every element


filter()
→ keep matching elements


reduce()
→ reduce array to one accumulated result


find()
→ first matching value


findIndex()
→ index of first matching value


some()
→ does at least ONE match?


every()
→ do ALL match?


sort()
→ sort elements


flat()
→ flatten nested arrays


flatMap()
→ map + flatten one level
25. Array.isArray() 🔥

Because:

typeof [];

returns:

"object"

So don't use typeof to check whether something is an array.

Use:

Array.isArray([10, 20]);

Output:

true

And:

Array.isArray("Hello");

Output:

false

Interview point:

typeof []            → "object"


Array.isArray([])    → true
26. Arrays are Objects 🔥

This is important for senior understanding.

typeof [];

returns:

"object"

Arrays are specialized objects in JavaScript.

You access their elements using numeric-like indexes:

numbers[0];
numbers[1];

And they have array-specific behavior/methods such as:

length
push()
pop()
map()
filter()
27. Arrays are Reference Types 🔥🔥

Consider:

const a = [10, 20];


const b = a;

Now:

a ──────┐
        ↓
     [10, 20]
        ↑
b ──────┘

Both variables refer to the same array object.

So:

b.push(30);

Now:

console.log(a);

Output:

[10, 20, 30]

Because a and b refer to the same array.

This connects directly to our references / shallow copy / deep copy topics.

28. Comparing Arrays 🔥

Consider:

const a = [1, 2];
const b = [1, 2];


console.log(a === b);

Output:

false

Why?

They are two different array objects.

Conceptually:

a → [1, 2]   object #1


b → [1, 2]   object #2

Even though their contents look identical, their references are different.

But:

const a = [1, 2];
const b = a;


console.log(a === b);

Output:

true

because both refer to the same array object.

29. Mutating vs Non-Mutating Methods 🔥

Some methods modify the original array.

Examples:

push()
pop()
shift()
unshift()
splice()
sort()
reverse()

These are mutating methods.

Others return new values/arrays without directly changing the original array:

slice()
concat()
map()
filter()

These are generally non-mutating.

This distinction is especially important when working with React state.

Interview Questions
What is an array?

An array is an ordered collection used to store multiple values.

What index does an array start from?

0.

push() vs pop()?

push() adds to the end. pop() removes from the end.

shift() vs unshift()?

shift() removes from the beginning. unshift() adds to the beginning.

slice() vs splice()?

slice() returns part of an array without modifying the original. splice() can add/remove/replace elements and modifies the original array.

How do you check if a value is an array?
Array.isArray(value);
Why does [1,2] === [1,2] return false?

Because they are two different array objects with different references.

Are arrays objects?

Yes. Arrays are specialized objects in JavaScript.

🧠 Final Arrays Cheat Sheet
ARRAY
→ ordered collection
→ index starts at 0


length
→ number of elements


push()
→ add END


pop()
→ remove END


unshift()
→ add START


shift()
→ remove START


includes()
→ value exists?


indexOf()
→ find index


slice()
→ copy/extract section
→ original unchanged


splice()
→ add/remove/replace
→ original changed


concat()
→ combine arrays


...array
→ spread / expand


[first, ...rest]
→ destructuring + rest


for...of
→ iterate values


Array.isArray()
→ check for array


Arrays
→ reference types

And remember this:

MUTATING
push / pop / shift / unshift
splice / sort / reverse


NON-MUTATING
slice / concat / map / filter

# >>>


6.9 — OBJECTS

An object stores related data together using key-value pairs.

Imagine we want to store information about an employee.

Instead of:

const name = "John";
const age = 30;
const role = "Developer";

we can group the information:

const employee = {
    name: "John",
    age: 30,
    role: "Developer"
};

Think:

employee
   ↓


{
    name: "John",
    age: 30,
    role: "Developer"
}

Here:

name  → key/property
John  → value


age   → key/property
30    → value


role  → key/property
Developer → value
1. Creating an Object

The most common way is an object literal:

const user = {
    name: "John",
    age: 30,
    isActive: true
};

An object can contain different types of values:

const user = {
    name: "John",
    age: 30,
    isActive: true,
    salary: null
};

So an object is useful for representing something with multiple related properties.

2. Accessing Object Properties 🔥

There are two common ways.

Dot notation
const user = {
    name: "John",
    age: 30
};


console.log(user.name);

Output:

John

Similarly:

console.log(user.age);

Output:

30

Think:

user.name
   ↓
"John"
3. Bracket Notation

We can also write:

console.log(user["name"]);

Output:

John

So:

user.name

and:

user["name"]

can access the same property.

4. When Bracket Notation Is Important 🔥🔥

Suppose the property name is stored inside a variable:

const user = {
    name: "John",
    age: 30
};


const key = "name";

If we write:

console.log(user[key]);

Output:

John

Why?

key
 ↓
"name"


user[key]
 ↓


user["name"]
 ↓


"John"

But:

user.key

means:

Look for a property literally called "key"

So:

console.log(user.key);

Output:

undefined

This difference is very important.

Easy formula
user.name
→ property name is known directly




user[key]
→ property name comes from a variable/expression
5. Property Names With Spaces

Consider:

const user = {
    "full name": "John Smith"
};

This will not work:

user.full name

Instead:

console.log(user["full name"]);

Output:

John Smith

Another reason bracket notation exists.

6. Changing Object Properties

Objects are mutable.

const user = {
    name: "John",
    age: 30
};


user.age = 31;


console.log(user);

Output:

{
    name: "John",
    age: 31
}

We changed:

age


30
 ↓
31
7. But user Is const — How Can We Change It? 🔥

This confuses many beginners.

const user = {
    name: "John"
};


user.name = "David";

This is allowed.

But:

user = {
    name: "David"
};

is not allowed.

Why?

Because const prevents reassignment of the variable.

It does not automatically make the object immutable.

Think:

const user
    │
    ↓
 OBJECT
 { name: "John" }

We can modify the object:

user.name = "David";

But we cannot make user refer to a completely different object:

user = {};

❌ TypeError

🔥 Interview point:

const prevents reassignment of the binding. It does not prevent mutation of the referenced object.

8. Adding New Properties

JavaScript objects are dynamic.

const user = {
    name: "John"
};


user.age = 30;


console.log(user);

Output:

{
    name: "John",
    age: 30
}

The age property didn't exist originally.

JavaScript added it.

We can also use bracket notation:

user["role"] = "Developer";

Now:

console.log(user);

Output:

{
    name: "John",
    age: 30,
    role: "Developer"
}
9. Deleting Properties

Use:

delete

Example:

const user = {
    name: "John",
    age: 30,
    role: "Developer"
};


delete user.age;


console.log(user);

Output:

{
    name: "John",
    role: "Developer"
}

So:

delete object.property

removes that property from the object.

10. Accessing a Missing Property

Suppose:

const user = {
    name: "John"
};

Now:

console.log(user.age);

Output:

undefined

JavaScript normally doesn't throw an error just because a property doesn't exist.

It returns:

undefined
11. Checking Whether a Property Exists 🔥

One option is the in operator.

const user = {
    name: "John",
    age: 30
};


console.log("name" in user);

Output:

true

And:

console.log("salary" in user);

Output:

false

Syntax:

"property" in object
12. Why undefined Isn't Always Enough

Consider:

const user = {
    name: "John",
    age: undefined
};

Now:

console.log(user.age);

Output:

undefined

But:

console.log(user.salary);

also gives:

undefined

There is a difference:

age
→ property exists
→ value is undefined




salary
→ property doesn't exist

Check:

console.log("age" in user);     // true
console.log("salary" in user);  // false

🔥 Good interview distinction.

13. Object Methods

An object can contain functions.

const user = {
    name: "John",


    greet: function () {
        console.log("Hello");
    }
};

Now:

user.greet();

Output:

Hello

A function stored as an object property is commonly called a method.

Think:

user
 ├── name  → "John"
 └── greet → function

Call it:

user.greet();
14. Method Shorthand

Instead of:

const user = {
    greet: function () {
        console.log("Hello");
    }
};

modern JavaScript allows:

const user = {
    greet() {
        console.log("Hello");
    }
};

Then:

user.greet();

Output:

Hello

Both create a method.

15. Using this Inside an Object 🔥🔥

Consider:

const user = {
    name: "John",


    greet() {
        console.log("Hello " + this.name);
    }
};


user.greet();

Output:

Hello John

Here:

user.greet()

causes this inside that regular method call to refer to user.

So:

this.name

becomes:

user.name

which is:

John

We'll study this deeply under JavaScript Internals, because its value depends on how a function is called.

For now understand the basic object-method example.

16. Nested Objects 🔥

Objects can contain other objects.

const employee = {
    name: "John",


    address: {
        city: "Mumbai",
        state: "Maharashtra"
    }
};

Think:

employee
│
├── name → John
│
└── address
      │
      ├── city  → Mumbai
      └── state → Maharashtra

Access city:

console.log(employee.address.city);

Output:

Mumbai

Flow:

employee
   ↓
address
   ↓
city
   ↓
Mumbai
17. Optional Chaining With Objects 🔥🔥

Suppose API data looks like:

const employee = {
    name: "John"
};

If we do:

console.log(employee.address.city);

we get an error because address is undefined, and JavaScript then tries to read .city from it.

Use optional chaining:

console.log(employee.address?.city);

Output:

undefined

Instead of crashing at that access.

Very common with API responses:

const city = employee.address?.city ?? "Not provided";

If address/city is unavailable:

Not provided
18. Objects Inside Arrays

Extremely common in MERN applications.

const users = [
    {
        id: 1,
        name: "John"
    },
    {
        id: 2,
        name: "David"
    }
];

Think:

users
 │
 ├── index 0
 │     ├── id   → 1
 │     └── name → John
 │
 └── index 1
       ├── id   → 2
       └── name → David

Access:

console.log(users[0].name);

Output:

John

Because:

users[0]
   ↓
{ id: 1, name: "John" }


       ↓ .name


"John"

🔥 You'll see this structure constantly with API/database data.

19. Looping Through an Object

We already saw for...in.

const user = {
    name: "John",
    age: 30,
    role: "Developer"
};


for (const key in user) {
    console.log(key);
}

Output:

name
age
role

To get values:

for (const key in user) {
    console.log(user[key]);
}

Output:

John
30
Developer

Why:

key = "name"


user[key]
    ↓
user["name"]
    ↓
"John"
20. Computed Property Names

Suppose:

const property = "salary";

We can create an object like:

const employee = {
    name: "John",
    [property]: 50000
};

Now:

console.log(employee);

Output:

{
    name: "John",
    salary: 50000
}

Because:

[property]
    ↓
"salary"

This is called a computed property name.

21. Property Shorthand

Suppose:

const name = "John";
const age = 30;

We could write:

const user = {
    name: name,
    age: age
};

But when the variable name and property name are the same, JavaScript allows:

const user = {
    name,
    age
};

Result:

{
    name: "John",
    age: 30
}

Very common in MERN code.

For example:

const email = "john@test.com";
const password = "12345";


const data = {
    email,
    password
};
22. Objects Are Compared by Reference 🔥🔥

Consider:

const user1 = {
    name: "John"
};


const user2 = {
    name: "John"
};


console.log(user1 === user2);

Output:

false

This surprises people.

The contents look identical:

user1 → { name: "John" }


user2 → { name: "John" }

But they are two different objects.

Think:

user1 ───→ Object A


user2 ───→ Object B

Therefore:

user1 === user2

is:

false
23. Same Object Reference

Now:

const user1 = {
    name: "John"
};


const user2 = user1;

Think:

          ┌───────────────┐
user1 ───→│ {name:"John"} │
          └───────────────┘
                 ↑
user2 ───────────┘

Both variables refer to the same object.

So:

console.log(user1 === user2);

Output:

true

And:

user2.name = "David";

Now:

console.log(user1.name);

Output:

David

Because both variables refer to the same object.

🔥 This becomes extremely important when we study:

References
Shallow copying
Deep copying
Spread
React state

We'll master those in OBJECTS — MASTER.

24. Object.keys()

Suppose:

const user = {
    name: "John",
    age: 30,
    role: "Developer"
};

We can get the object's own enumerable string keys:

const keys = Object.keys(user);


console.log(keys);

Output:

["name", "age", "role"]

Think:

Object.keys()
→ keys
25. Object.values()
const values = Object.values(user);


console.log(values);

Output:

["John", 30, "Developer"]

Think:

Object.values()
→ values
26. Object.entries()
const entries = Object.entries(user);


console.log(entries);

Output:

[
    ["name", "John"],
    ["age", 30],
    ["role", "Developer"]
]

Each property becomes:

[key, value]

So remember:

Object.keys()
→ ["name", "age"]




Object.values()
→ ["John", 30]




Object.entries()
→ [
     ["name", "John"],
     ["age", 30]
   ]

We'll practice these much more under OBJECTS — MASTER.

27. Object Destructuring — Basic Introduction 🔥

Suppose:

const user = {
    name: "John",
    age: 30
};

Normally:

const name = user.name;
const age = user.age;

Destructuring lets us write:

const { name, age } = user;

Now:

console.log(name); // John
console.log(age);  // 30

Think:

user
 ↓


{
    name: "John",
    age: 30
}


      ↓ destructure


name = "John"
age  = 30

We will cover destructuring deeply under OBJECTS — MASTER.

28. Spread — Basic Introduction

Consider:

const user = {
    name: "John",
    age: 30
};

We can create another object:

const updatedUser = {
    ...user,
    role: "Developer"
};

Result:

{
    name: "John",
    age: 30,
    role: "Developer"
}

Here:

...user

spreads the object's own enumerable properties into the new object.

Again, this is only the foundation.

Later we'll study:

Spread
References
Shallow copies
Nested objects
Deep copies

together.

29. Common Interview Traps 🔥
Trap 1
const a = {};
const b = {};


console.log(a === b);

Output:

false

Different objects.

Trap 2
const a = {};
const b = a;


console.log(a === b);

Output:

true

Same object reference.

Trap 3
const user = {
    name: "John"
};


user.name = "David";

Allowed even though user is const.

Trap 4
const user = {
    age: undefined
};


console.log(user.age);     // undefined
console.log(user.salary);  // undefined

But:

console.log("age" in user);     // true
console.log("salary" in user);  // false
Trap 5
const key = "name";


const user = {
    name: "John"
};


console.log(user.key);

Output:

undefined

But:

console.log(user[key]);

Output:

John
Interview Questions 🔥
What is an object in JavaScript?

An object is a collection of properties where data is stored as key-value pairs.

Dot notation vs bracket notation?

Dot notation directly accesses a named property. Bracket notation can use a string or an expression, so it's useful when the property name is dynamic.

Can we modify an object declared with const?

Yes. const prevents reassignment of the variable, but the object's properties can still be changed unless something else prevents mutation.

What happens when we access a missing property?

JavaScript normally returns undefined.

What is an object method?

A method is a function stored as a property of an object.

Why is {} === {} false?

Because they are two different object references. Object equality with === compares identity/reference, not whether their contents look the same.

What does Object.keys() return?

An array containing the object's own enumerable string property keys.

What does Object.values() return?

An array containing the corresponding property values.

What does Object.entries() return?

An array of [key, value] pairs.

🧠 Final Objects Cheat Sheet
OBJECT


"name" in user
→ true




METHOD


const user = {
    greet() {
        console.log("Hello");
    }
};




NESTED OBJECT


user.address.city




OPTIONAL CHAINING


user.address?.city




OBJECT KEYS


Object.keys(user)
→ array of keys




OBJECT VALUES


Object.values(user)
→ array of values




OBJECT ENTRIES


Object.entries(user)
→ array of [key, value]




DESTRUCTURING


const { name, age } = user;




SPREAD


const copy = {
    ...user
};




OBJECT COMPARISON


{} === {}
→ false




SAME REFERENCE


const a = {};
const b = a;


a === b
→ true




const + object


const prevents reassignment
NOT object mutation

# >>>>

6.10 — STRINGS

JavaScript strings are used to represent text.

In MERN applications, strings are everywhere:

const name = "John";
const email = "john@gmail.com";
const password = "hello123";
const role = "admin";
const search = "laptop";
const message = "Login successful";

You will use strings for:

User names
Emails
Passwords
Search text
Form inputs
API data
URLs
Messages
Roles
Tokens
Database fields

So Strings are one of the most practically important JavaScript data types.

1. Creating a String

Strings can be created using:

const firstName = "John";
const city = 'Mumbai';
const message = `Hello World`;

JavaScript supports:

" "   Double quotes
' '   Single quotes
` `   Backticks

All three can represent strings.

console.log(typeof firstName);

Output:

string
2. Strings Are Primitive Values 🔥

A normal JavaScript string is a primitive.

const name = "John";


console.log(typeof name);

Output:

string

Remember:

"John"
   ↓
primitive string

Don't confuse this with:

new String("John");

That creates a String object and is normally unnecessary.

console.log(typeof new String("John"));

Output:

object

Prefer:

const name = "John";
3. String Length

Use:

.length

Example:

const name = "JavaScript";


console.log(name.length);

Output:

10

.length is a property, not a function.

Correct:

name.length

Wrong:

name.length()

🔥 This is a common beginner mistake.

4. Accessing Characters

Strings use zero-based indexes.

const name = "React";


console.log(name[0]);
console.log(name[1]);
console.log(name[2]);

Output:

R
e
a

Conceptually:

R  e  a  c  t
0  1  2  3  4

You can also use:

name.charAt(0);

But modern JavaScript commonly uses:

name[0];
5. Strings Are Immutable 🔥🔥

This is an important concept.

Suppose:

let name = "John";


name[0] = "R";


console.log(name);

Output:

John

It does not become:

Rohn

Why?

Strings are immutable.

That means the existing string's characters cannot be changed directly.

You need to create a new string:

let name = "John";


name = "R" + name.slice(1);


console.log(name);

Output:

Rohn

Remember:

String
  ↓
immutable
  ↓
operations produce new strings
6. toUpperCase() / toLowerCase()

Very common methods.

const name = "JavaScript";


console.log(name.toUpperCase());

Output:

JAVASCRIPT

And:

console.log(name.toLowerCase());

Output:

javascript

Practical example:

const role = "ADMIN";


if (role.toLowerCase() === "admin") {
    console.log("Admin user");
}

🔥 Useful for case-insensitive comparisons.

7. trim() 🔥🔥

Form inputs often contain accidental spaces.

Example:

const username = "   John   ";


console.log(username.trim());

Output:

John

There are also:

trimStart();
trimEnd();

Example:

const value = "   hello   ";


console.log(value.trimStart());
console.log(value.trimEnd());

🔥 MERN example:

const email = req.body.email.trim();

Or frontend:

const searchText = input.trim();
8. includes() 🔥

Checks whether a string contains some text.

const message = "JavaScript is powerful";


console.log(message.includes("JavaScript"));

Output:

true

Example:

console.log(message.includes("React"));

Output:

false

Useful for simple searching:

const product = "Apple iPhone 16";


console.log(product.includes("iPhone"));
9. startsWith() / endsWith()

Check the beginning or end of a string.

const url = "https://example.com";


console.log(url.startsWith("https"));

Output:

true

Example:

const file = "profile.jpg";


console.log(file.endsWith(".jpg"));

Output:

true

Practical uses:

URL validation
File extension checking
Prefix checking
Simple input validation
10. indexOf() 🔥

Finds the position of text.

const text = "Hello JavaScript";


console.log(text.indexOf("JavaScript"));

Output:

6

If the value isn't found:

console.log(text.indexOf("React"));

Output:

-1

Remember:

found
↓
index


not found
↓
-1
11. lastIndexOf()

Returns the last occurrence.

const text = "hello world hello";


console.log(text.lastIndexOf("hello"));

Output:

12

Compare:

text.indexOf("hello");

returns:

0

while:

text.lastIndexOf("hello");

returns:

12
12. slice() 🔥🔥

One of the most important string methods.

const text = "JavaScript";


console.log(text.slice(0, 4));

Output:

Java

Important:

start index → included
end index   → excluded

So:

text.slice(0, 4)

takes:

J a v a
0 1 2 3

Index 4 is not included.

You can omit the end:

text.slice(4);

Output:

Script
13. Negative Index with slice()

slice() supports negative indexes.

const text = "JavaScript";


console.log(text.slice(-6));

Output:

Script

Negative indexes count from the end.

This is useful for getting the last few characters.

Example:

const card = "1234567812345678";


const lastFour = card.slice(-4);


console.log(lastFour);

Output:

5678
14. substring()

Another method for extracting part of a string:

const text = "JavaScript";


console.log(text.substring(0, 4));

Output:

Java

For most application code, slice() is usually easier to standardize on because it also supports negative indexes.

Example:

text.slice(-6);

works naturally.

15. replace() 🔥

Replaces matching text.

const message = "Hello John";


const result = message.replace("John", "David");


console.log(result);

Output:

Hello David

Important:

replace() normally replaces the first matching occurrence.

const text = "cat cat cat";


console.log(text.replace("cat", "dog"));

Output:

dog cat cat
16. replaceAll()

To replace every occurrence:

const text = "cat cat cat";


const result = text.replaceAll("cat", "dog");


console.log(result);

Output:

dog dog dog

Very useful for text processing.

17. split() 🔥🔥

split() converts a:

String → Array

Example:

const technologies = "React,Node,MongoDB";


const result = technologies.split(",");


console.log(result);

Output:

["React", "Node", "MongoDB"]

This is extremely useful.

Example API/form value:

const tags = "javascript,react,node";

Convert it:

const tagArray = tags.split(",");

Result:

["javascript", "react", "node"]
18. Array join() — Reverse of split()

If:

const technologies = ["React", "Node", "MongoDB"];

You can create a string using:

const result = technologies.join(", ");

Result:

React, Node, MongoDB

Think:

split()


String
  ↓
Array

while:

join()


Array
  ↓
String

join() itself is an Array method, but you will frequently use it together with strings.

19. concat()

Strings can be combined using:

const firstName = "John";
const lastName = "Doe";


const fullName = firstName.concat(" ", lastName);


console.log(fullName);

Output:

John Doe

But in modern JavaScript, you'll more commonly see:

const fullName = firstName + " " + lastName;

or template literals.

20. Template Literals 🔥🔥🔥

Very important in modern JavaScript, React and Node.

Use backticks:

``

Example:

const name = "John";


const message = `Hello ${name}`;


console.log(message);

Output:

Hello John

${} allows JavaScript expressions inside strings.

Example:

const price = 100;
const quantity = 3;


const message =
    `Total price: ₹${price * quantity}`;


console.log(message);

Output:

Total price: ₹300
21. Multiline Strings

Template literals can span multiple lines.

const message = `
Hello John,


Your order has been confirmed.


Thank you.
`;

This is much cleaner than manually joining many strings.

22. String Concatenation vs Template Literals

Old/common approach:

const name = "John";
const age = 30;


const message =
    "My name is " + name + " and I am " + age;

Modern approach:

const message =
    `My name is ${name} and I am ${age}`;

Prefer template literals when interpolation makes the code clearer.

23. String Comparison 🔥

Strings can be compared using:

===

Example:

const role = "admin";


console.log(role === "admin");

Output:

true

But string comparison is case-sensitive.

console.log("Admin" === "admin");

Output:

false

For case-insensitive comparison:

const role = "ADMIN";


console.log(
    role.toLowerCase() === "admin"
);

Output:

true
24. String Conversion 🔥

Convert another value to a string:

const age = 30;


const value = String(age);


console.log(value);
console.log(typeof value);

Output:

30
string

You may also see:

age.toString();

But String(value) is useful because it can safely handle values such as null and undefined by converting them to text, whereas calling .toString() directly on null or undefined throws an error.

25. repeat()

Repeats a string.

const text = "Hi ";


console.log(text.repeat(3));

Output:

Hi Hi Hi

Not a daily MERN method, but useful to recognize.

26. padStart() / padEnd()

Adds characters until a desired length is reached.

const number = "5";


console.log(number.padStart(2, "0"));

Output:

05

Useful for formatting.

Example:

const hours = "8";


const formatted =
    hours.padStart(2, "0");


console.log(formatted);

Output:

08
27. Practical MERN Example — Search 🔥🔥

Imagine products:

const products = [
    "Apple iPhone",
    "Samsung Galaxy",
    "Google Pixel"
];

User searches:

const search = "pixel";

A common approach:

const result = products.filter(product =>
    product
        .toLowerCase()
        .includes(search.toLowerCase())
);


console.log(result);

Result:

["Google Pixel"]

This pattern appears constantly in frontend search/filter features.

28. Practical MERN Example — Cleaning Form Input

Suppose the user enters:

const email = "   JOHN@GMAIL.COM   ";

Normalize it:

const normalizedEmail =
    email.trim().toLowerCase();


console.log(normalizedEmail);

Output:

john@gmail.com

Think:

User Input
    ↓
trim()
    ↓
toLowerCase()
    ↓
normalized value

This is a very common application pattern.

29. Practical Example — Username Initials

Suppose:

const name = "John Doe";

We want:

JD

One solution:

const initials = name
    .split(" ")
    .map(word => word[0])
    .join("");


console.log(initials);

Output:

JD

Don't worry if the chained code looks advanced yet.

You will understand it completely when we master arrays.

30. Practical Example — Masking Data

Suppose:

const phone = "9876543210";

Show only the final four digits:

const masked =
    "******" + phone.slice(-4);


console.log(masked);

Output:

******3210

This combines:

concatenation
+
slice()
31. Strings + APIs 🔥

An API might return:

const user = {
    name: "John Doe",
    email: "john@gmail.com",
    role: "admin"
};

These values are strings.

console.log(typeof user.name);
console.log(typeof user.email);
console.log(typeof user.role);

Output:

string
string
string

You will constantly process API strings using:

trim()
toLowerCase()
includes()
slice()
split()
replace()
32. Method Chaining 🔥🔥

String methods can be chained.

Example:

const email = "   JOHN@GMAIL.COM   ";


const result =
    email
        .trim()
        .toLowerCase();


console.log(result);

Output:

john@gmail.com

Conceptually:

"   JOHN@GMAIL.COM   "
          ↓
        trim()
          ↓
"JOHN@GMAIL.COM"
          ↓
    toLowerCase()
          ↓
"john@gmail.com"
33. Common Interview Traps 🔥🔥
Trap 1 — Strings Are Immutable
let name = "John";


name[0] = "R";


console.log(name);

Output:

John
Trap 2 — length Is Not a Function

Correct:

name.length;

Wrong:

name.length();
Trap 3 — Index Starts From 0
const text = "React";


console.log(text[0]);

Output:

R
Trap 4 — includes() Is Case-Sensitive
console.log(
    "JavaScript".includes("javascript")
);

Output:

false

A normalized comparison:

"JavaScript"
    .toLowerCase()
    .includes("javascript");

returns:

true
Trap 5 — slice End Is Excluded
const text = "JavaScript";


console.log(text.slice(0, 4));

Output:

Java

Index 4 isn't included.

Trap 6 — split() Returns an Array
const value = "React,Node";


const result = value.split(",");


console.log(typeof result);

result is an Array object.

Better check:

console.log(Array.isArray(result));

Output:

true
Trap 7 — replace() vs replaceAll()
"cat cat".replace("cat", "dog");

Result:

dog cat

Whereas:

"cat cat".replaceAll("cat", "dog");

Result:

dog dog
34. Interview Questions 🔥
What is a string in JavaScript?

A string is a primitive data type used to represent textual data.

Are JavaScript strings mutable?

No.

Strings are immutable.

How do you get the length of a string?
text.length
How do you remove spaces from both ends?
text.trim()
How do you check whether text exists inside a string?
text.includes("value")
What does indexOf() return when the value isn't found?
-1
What does split() do?

It converts a string into an array based on a separator.

"a,b,c".split(",");

Result:

["a", "b", "c"]
What is the difference between slice() and substring()?

Both extract part of a string, but slice() supports negative indexes, making it convenient for extracting from the end.

What are template literals?

Strings written using backticks that support interpolation and multiline text.

`Hello ${name}`
Are string comparisons case-sensitive?

Yes.

"Admin" === "admin"

is:

false
Does trim() modify the original string?

No.

Because strings are immutable, it returns a new string.

🧠 Final Strings Cheat Sheet
CREATE
const name = "John";
const city = 'Mumbai';
const message = `Hello`;
TYPE
typeof name;
// "string"
LENGTH
text.length;
CHARACTER
text[0];
UPPERCASE
text.toUpperCase();
LOWERCASE
text.toLowerCase();
REMOVE OUTER SPACES
text.trim();
SEARCH
text.includes("JS");
POSITION
text.indexOf("JS");
START / END CHECK
text.startsWith("https");
text.endsWith(".jpg");
EXTRACT
text.slice(0, 5);
text.slice(-4);
REPLACE FIRST
text.replace("old", "new");
REPLACE ALL
text.replaceAll("old", "new");
STRING → ARRAY
text.split(",");
ARRAY → STRING
array.join(",");
INTERPOLATION
`Hello ${name}`
CONVERT TO STRING
String(value);
CASE-INSENSITIVE COMPARISON
a.toLowerCase() === b.toLowerCase();
CLEAN FORM INPUT
const email =
    input.trim().toLowerCase();
🔥 Methods you should remember
length


toUpperCase()
toLowerCase()
trim()


includes()
startsWith()
endsWith()


indexOf()
lastIndexOf()


slice()
substring()


replace()
replaceAll()


split()


repeat()
padStart()
padEnd()


Template literals

# >>>>>>>>

6.11 — DATES

JavaScript provides the Date object for working with dates and times.

In MERN applications, dates are common for:

createdAt
updatedAt
dateOfBirth
orderDate
appointmentDate
loginTime
expiryDate

For example, MongoDB/Mongoose commonly stores timestamps such as:

createdAt
updatedAt

So you need a solid practical understanding of JavaScript dates, but we don't need to go unnecessarily deep here.

1. Creating a Date

Use:

new Date()

Example:

const now = new Date();


console.log(now);

You will get the current date and time.

Conceptually:

new Date()
    ↓
current date + current time
2. Creating a Specific Date

We can provide a date string:

const date = new Date("2026-08-16");


console.log(date);

This creates a Date object representing that date.

Another example:

const birthday = new Date("1995-05-20");

🔥 In MERN applications, you'll frequently receive date strings from APIs/databases and convert or format them.

3. Date Is an Object 🔥
const date = new Date();


console.log(typeof date);

Output:

object

So:

new Date()
    ↓
Date object

It is not a primitive string.

4. Getting Date Parts

Suppose:

const date = new Date();

JavaScript provides methods for extracting parts of the date.

Year
date.getFullYear();

Example:

console.log(date.getFullYear());

Could output:

2026
Month
date.getMonth();

🔥 Important:

Months are zero-based.

January   → 0
February  → 1
March     → 2
...
December  → 11

So:

const date = new Date("2026-08-16T12:00:00");


console.log(date.getMonth());

Output:

7

August is month index 7.

This is a classic interview trap.

5. Getting Day of Month

Use:

getDate()

Example:

const date = new Date("2026-08-16T12:00:00");


console.log(date.getDate());

Output:

16

Remember:

getDate()
→ day of MONTH
6. Getting Day of Week

Use:

getDay()

Example:

const date = new Date("2026-08-16T12:00:00");


console.log(date.getDay());

Output:

0

Because:

Sunday    → 0
Monday    → 1
Tuesday   → 2
Wednesday → 3
Thursday  → 4
Friday    → 5
Saturday  → 6

🔥 Very common confusion:

getDate()
→ date of month


getDay()
→ day of week
7. Getting Time
const date = new Date();


console.log(date.getHours());
console.log(date.getMinutes());
console.log(date.getSeconds());

Methods:

getHours()
getMinutes()
getSeconds()
getMilliseconds()
8. Date Timestamp 🔥🔥

Internally, JavaScript dates are based on a timestamp representing milliseconds relative to:

January 1, 1970 UTC

Use:

Date.now()

Example:

const timestamp = Date.now();


console.log(timestamp);

You will get a number similar to:

1786838400000

The exact value depends on the current time.

🔥 Important:

Date.now()
→ number
→ milliseconds
9. getTime()

A Date object can also return its timestamp.

const date = new Date();


console.log(date.getTime());

Think:

Date object
    ↓
.getTime()
    ↓
timestamp in milliseconds

So:

Date.now();

and:

new Date().getTime();

both give a current timestamp in milliseconds.

10. Comparing Dates 🔥

A very practical requirement:

Is this date before another date?
Is a token expired?
Which order is newer?

Example:

const date1 = new Date("2026-01-01");
const date2 = new Date("2026-05-01");


console.log(date1.getTime() < date2.getTime());

Output:

true

Because January occurs before May.

You can also compare Date objects relationally:

console.log(date1 < date2);

Output:

true
11. Difference Between Two Dates 🔥🔥

Dates can be used to calculate elapsed time.

const start = new Date("2026-08-10");
const end = new Date("2026-08-15");


const difference = end - start;


console.log(difference);

The result is milliseconds.

To convert milliseconds to days:

const days =
    difference / (1000 * 60 * 60 * 24);


console.log(days);

Output:

5

Remember:

1000 milliseconds = 1 second


60 seconds = 1 minute


60 minutes = 1 hour


24 hours = 1 day

Therefore:

1000 * 60 * 60 * 24

represents milliseconds in a 24-hour day.

For real calendar-day calculations across daylight-saving changes, date libraries or UTC-based logic can be safer.

12. ISO Date Format 🔥🔥

Extremely important for APIs.

Use:

toISOString()

Example:

const date = new Date();


console.log(date.toISOString());

Result looks like:

2026-08-15T19:08:00.000Z

Structure:

2026-08-15 T 19:08:00.000 Z
│          │              │
date       separator      UTC

The Z indicates UTC.

🔥 You will see ISO date strings constantly in:

REST APIs
MongoDB
Mongoose
JSON responses
Frontend/backend communication
13. Formatting Dates for Users

API dates often aren't ideal for displaying directly.

Suppose:

const date = new Date("2026-08-16");

You can use:

date.toLocaleDateString();

to get a localized date representation.

You can also specify a locale:

date.toLocaleDateString("en-IN");

For more control:

const formatted = date.toLocaleDateString("en-IN", {
    day: "2-digit",
    month: "short",
    year: "numeric"
});


console.log(formatted);

This can produce something like:

16 Aug 2026

🔥 Useful in React UIs.

14. Date and JSON / APIs 🔥

Imagine an API response:

const user = {
    name: "John",
    createdAt: "2026-08-16T10:30:00.000Z"
};

Here:

user.createdAt

is a string.

Check:

console.log(typeof user.createdAt);

Output:

string

If you want Date methods:

const createdDate = new Date(user.createdAt);

Now:

createdDate.getFullYear();
createdDate.toLocaleDateString();

can be used.

🔥 Very important MERN distinction:

API JSON
   ↓
ISO date string
   ↓
new Date(dateString)
   ↓
Date object
15. MongoDB / Mongoose Connection 🔥

Later, you will commonly create Mongoose schemas using:

{
    timestamps: true
}

Mongoose then manages fields such as:

createdAt
updatedAt

When these travel through JSON to your React frontend, you commonly work with their serialized date representation.

Example React-side processing:

const formattedDate = new Date(
    employee.createdAt
).toLocaleDateString("en-IN");

This is why understanding Dates matters for MERN.

16. UTC vs Local Time 🔥🔥

This is the most important conceptual date issue for application developers.

There are two ideas:

UTC
vs
Local Time

An ISO string such as:

2026-08-16T10:00:00.000Z

uses UTC because of:

Z

But:

date.getHours();

uses the runtime's local time zone.

There are corresponding UTC methods:

date.getUTCHours();
date.getUTCDate();
date.getUTCMonth();
date.getUTCFullYear();

You don't need to memorize every UTC method now.

Understand the principle:

Backend/database
      ↓
often store/communicate an absolute time
      ↓
Frontend
      ↓
display it in the appropriate user timezone

🔥 Timezone mistakes cause real production bugs.

17. Invalid Dates

Bad input can produce an invalid Date.

const date = new Date("hello");


console.log(date);

Result:

Invalid Date

A common check is:

const date = new Date(value);


if (Number.isNaN(date.getTime())) {
    console.log("Invalid date");
}

Why?

An invalid date's:

date.getTime()

returns:

NaN
18. Common Interview Traps 🔥
Trap 1 — Month Starts From 0
const date = new Date("2026-08-16T12:00:00");


console.log(date.getMonth());

Output:

7

Not:

8
Trap 2 — getDate() vs getDay()
getDate()
→ day of month


getDay()
→ day of week

Example:

const date = new Date("2026-08-16T12:00:00");


console.log(date.getDate());
console.log(date.getDay());

Output:

16
0
Trap 3 — Date.now() Doesn't Return a Date Object
console.log(typeof Date.now());

Output:

number

Because:

Date.now()
→ timestamp
Trap 4 — API Date Can Be a String
const createdAt =
    "2026-08-16T10:00:00.000Z";


console.log(typeof createdAt);

Output:

string

Convert it:

const date = new Date(createdAt);
Trap 5 — Timezones Matter
UTC time
≠ necessarily user's displayed local time

Never assume that the hour visible in an ISO UTC string is the same hour the user should see.

Interview Questions 🔥
What is Date in JavaScript?

Date is a built-in object used to represent and work with dates and times.

What does Date.now() return?

The current timestamp in milliseconds since the Unix epoch.

What is the difference between getDate() and getDay()?
getDate()
→ day of month


getDay()
→ day of week
What does getMonth() return?

A zero-based month number:

0 → January
...
11 → December
What is an ISO date string?

A standardized date/time representation commonly used when exchanging date values between systems.

Example:

2026-08-16T10:30:00.000Z
How do you convert an API date string into a Date object?
const date = new Date(apiDate);
How do you compare two dates?

A clear approach is comparing their timestamps:

date1.getTime() < date2.getTime();
Why are timezones important?

Because backend timestamps and the user's local displayed time may use different timezones. Incorrect conversion can display the wrong date or time.

🧠 Final Dates Cheat Sheet
CREATE


const date = new Date();




CURRENT TIMESTAMP


Date.now();




YEAR


date.getFullYear();




MONTH


date.getMonth();


0 → January
11 → December




DAY OF MONTH


date.getDate();




DAY OF WEEK


date.getDay();


0 → Sunday
6 → Saturday




TIME


date.getHours();
date.getMinutes();
date.getSeconds();




TIMESTAMP


date.getTime();




ISO / API FORMAT


date.toISOString();




DISPLAY


date.toLocaleDateString();




API STRING → DATE


const date = new Date(apiDate);




COMPARE


date1.getTime() < date2.getTime();




INVALID DATE


Number.isNaN(date.getTime());




IMPORTANT


UTC ≠ Local Time


# >>>

6.12 — SETS

A Set is a JavaScript collection that stores unique values.

The most important rule:

SET
↓
NO DUPLICATE VALUES

Example:

const numbers = new Set([10, 20, 20, 30, 30]);


console.log(numbers);

Conceptually:

Set {
    10,
    20,
    30
}

The duplicate 20 and 30 are removed.

🔥 Sets are especially useful for:

Removing duplicates
Checking whether a value exists
Tracking unique IDs/items
Coding/interview problems
1. Creating a Set

Use:

new Set()

Example:

const numbers = new Set();


console.log(numbers);

This creates an empty Set.

We can also create one with values:

const numbers = new Set([10, 20, 30]);

Think:

Array
[10, 20, 30]


     ↓ new Set()


Set
{10, 20, 30}
2. Set Stores Unique Values 🔥🔥

Consider:

const numbers = new Set([
    10,
    20,
    20,
    30,
    30
]);


console.log(numbers);

The Set contains:

10
20
30

Duplicates are ignored.

Another example:

const names = new Set([
    "John",
    "David",
    "John"
]);

Result conceptually:

John
David
3. add()

Use add() to add a value.

const numbers = new Set();


numbers.add(10);
numbers.add(20);
numbers.add(30);


console.log(numbers);

Conceptually:

Set {
    10,
    20,
    30
}

If we add the same value again:

numbers.add(20);

Nothing new is added.

Before:


10
20
30


Add 20 again


↓


10
20
30
4. has() 🔥

has() checks whether a value exists in the Set.

const numbers = new Set([10, 20, 30]);


console.log(numbers.has(20));

Output:

true

And:

console.log(numbers.has(100));

Output:

false

Remember:

set.has(value)


↓


true / false

🔥 This is one of the most important Set operations.

5. delete()

Removes a value from the Set.

const numbers = new Set([10, 20, 30]);


numbers.delete(20);


console.log(numbers);

Result:

10
30

delete() also returns a boolean indicating whether the value existed.

console.log(numbers.delete(10));

Output:

true
6. size 🔥

Arrays use:

array.length

Sets use:

set.size

Example:

const numbers = new Set([10, 20, 30]);


console.log(numbers.size);

Output:

3

🔥 Interview trap:

Array
→ length


Set
→ size

Not:

numbers.length;
7. clear()

clear() removes everything.

const numbers = new Set([10, 20, 30]);


numbers.clear();


console.log(numbers.size);

Output:

0

Think:

{10, 20, 30}


   ↓ clear()


{}
8. Removing Duplicates From an Array 🔥🔥

This is the most common practical Set pattern.

Suppose:

const numbers = [
    10,
    20,
    20,
    30,
    30,
    40
];

Convert it into a Set:

const unique = new Set(numbers);

Now duplicates are gone.

But if we want an array again:

const result = [...new Set(numbers)];


console.log(result);

Output:

[10, 20, 30, 40]

Flow:

ARRAY


[10, 20, 20, 30, 30, 40]


          ↓


       new Set()


          ↓


{10, 20, 30, 40}


          ↓


        [...]


          ↓


[10, 20, 30, 40]

🔥🔥 Memorize the pattern:

const unique = [...new Set(array)];

You will see this in coding interviews and application code.

9. Looping Through a Set

Sets are iterable.

We can use:

for...of

Example:

const numbers = new Set([10, 20, 30]);


for (const number of numbers) {
    console.log(number);
}

Output:

10
20
30
10. forEach() With Set

We can also use:

const numbers = new Set([10, 20, 30]);


numbers.forEach((number) => {
    console.log(number);
});

Output:

10
20
30
11. Convert Set → Array 🔥

Using spread:

const numbers = new Set([10, 20, 30]);


const array = [...numbers];


console.log(array);

Output:

[10, 20, 30]

Another option is:

Array.from(numbers);

But for your current JavaScript/MERN preparation:

[...numbers]

is the important pattern.

12. Set With Strings
const roles = new Set();


roles.add("admin");
roles.add("user");
roles.add("admin");


console.log(roles);

Conceptually:

admin
user

Only unique values remain.

Checking:

console.log(roles.has("admin"));

Output:

true
13. Set Is Case-Sensitive
const names = new Set();


names.add("John");
names.add("john");


console.log(names.size);

Output:

2

Because:

"John"

and:

"john"

are different strings.

14. Objects Inside Sets 🔥

This is an important interview concept.

Consider:

const user1 = {
    name: "John"
};


const user2 = {
    name: "John"
};


const users = new Set([
    user1,
    user2
]);


console.log(users.size);

Output:

2

Why?

Because objects are compared by reference.

Remember from Objects:

user1 ──→ Object A


user2 ──→ Object B

Even though their contents look identical, they are different objects.

But:

const user1 = {
    name: "John"
};


const user2 = user1;


const users = new Set([
    user1,
    user2
]);


console.log(users.size);

Output:

1

Because both variables reference the same object.

🔥 This connects directly to your Object reference concepts.

15. Practical MERN Example

Suppose an API gives us:

const employees = [
    {
        id: 1,
        department: "IT"
    },
    {
        id: 2,
        department: "HR"
    },
    {
        id: 3,
        department: "IT"
    },
    {
        id: 4,
        department: "Finance"
    }
];

We want unique departments.

First extract departments:

const departments = employees.map(
    employee => employee.department
);

Result:

[
    "IT",
    "HR",
    "IT",
    "Finance"
]

Then:

const uniqueDepartments = [
    ...new Set(departments)
];


console.log(uniqueDepartments);

Output:

["IT", "HR", "Finance"]

🔥 This is a very realistic React/API-data pattern.

Later, after mastering map(), this kind of transformation should become automatic.

16. Set vs Array 🔥

The important difference:

ARRAY


Can contain duplicates
Has indexes
Uses length
Good for ordered lists/data processing




SET


Stores unique values
No normal index access like set[0]
Uses size
Excellent for uniqueness/membership checks

Example:

const numbers = new Set([10, 20, 30]);


console.log(numbers[0]);

Output:

undefined

You don't normally access Sets using array indexes.

17. Why Use Set Instead of Array? 🔥

Suppose you repeatedly need to ask:

Does this ID already exist?

With a Set:

const selectedIds = new Set([
    101,
    102,
    103
]);


if (selectedIds.has(102)) {
    console.log("Already selected");
}

This expresses the intent clearly:

Set
↓
unique membership collection

For frequent membership checks, Sets are also generally more appropriate than repeatedly searching an array.

18. Common Interview Traps 🔥
Trap 1 — Set Removes Duplicates
const set = new Set([
    1,
    1,
    2,
    2,
    3
]);


console.log(set.size);

Output:

3
Trap 2 — Set Uses size
const set = new Set([1, 2, 3]);


console.log(set.size);

Output:

3

Not:

set.length;
Trap 3 — No Array Index Access
const set = new Set([
    "A",
    "B",
    "C"
]);


console.log(set[0]);

Output:

undefined
Trap 4 — Objects Are Compared by Reference
const set = new Set([
    { name: "John" },
    { name: "John" }
]);


console.log(set.size);

Output:

2

Two different object references.

Trap 5 — add() Doesn't Create Duplicates
const set = new Set();


set.add(10);
set.add(10);
set.add(10);


console.log(set.size);

Output:

1
Interview Questions 🔥
What is a Set in JavaScript?

A Set is a collection that stores unique values.

How do you create a Set?
const set = new Set();

Or:

const set = new Set([1, 2, 3]);
How do you add a value?
set.add(value);
How do you check whether a value exists?
set.has(value);

It returns:

true / false
How do you remove a value?
set.delete(value);
How do you remove everything?
set.clear();
How do you get the number of values?
set.size;
How do you remove duplicates from an array?
const unique = [...new Set(array)];
Can a Set contain objects?

Yes.

But objects are compared by reference, so two separate objects with identical properties are still different Set values.

Set vs Array?

Use an Array for normal ordered list processing and indexed access. Use a Set when uniqueness and membership checks are the main requirement.

🧠 Final Set Cheat Sheet
CREATE


const set = new Set();




WITH VALUES


const set = new Set([10, 20, 30]);




ADD


set.add(10);




CHECK


set.has(10);


→ true / false




DELETE


set.delete(10);




DELETE EVERYTHING


set.clear();




SIZE


set.size;




LOOP


for (const value of set) {
    console.log(value);
}




SET → ARRAY


const array = [...set];




REMOVE ARRAY DUPLICATES 🔥


const unique = [...new Set(array)];




IMPORTANT


Set stores unique values.




ARRAY


array.length




SET


set.size




OBJECTS IN SET


Compared by reference.

# >>

13 — MAP

A Map is a JavaScript collection that stores data as key-value pairs.

KEY → VALUE

Example:

const employee = new Map([
    ["name", "John"],
    ["age", 30]
]);

Think:

"name" → "John"
"age"  → 30

🔥 For MERN, the main thing to understand is:

Map
 ↓
Store a value using a key
 ↓
Retrieve that value using the key
1. Creating a Map

Create an empty Map:

const employee = new Map();

Or create it with values:

const employee = new Map([
    ["name", "John"],
    ["age", 30]
]);

The format is:

[
    [key, value],
    [key, value]
]
2. set() 🔥

set() adds a key-value pair.

const employee = new Map();


employee.set("name", "John");
employee.set("age", 30);

Syntax:

map.set(key, value);

Think:

"name" → "John"
"age"  → 30
3. get() 🔥

get() gets the value stored for a key.

const employee = new Map();


employee.set("name", "John");


console.log(employee.get("name"));

Output:

John

Remember:

set()
→ store


get()
→ retrieve

If the key doesn't exist:

employee.get("salary");

returns:

undefined
4. has() 🔥

has() checks whether a key exists.

const employee = new Map([
    ["name", "John"],
    ["age", 30]
]);


console.log(employee.has("name"));

Output:

true

And:

console.log(employee.has("salary"));

Output:

false

Remember:

map.has(key);

returns:

true / false
5. Map Keys Are Unique 🔥

A Map cannot have the same key multiple times.

const employee = new Map();


employee.set("name", "John");
employee.set("name", "David");

The second set() updates the existing value.

Result:

"name" → "David"

So:

Same key again
      ↓
Update value

It does not create another "name" entry.

6. delete()

delete() removes an entry using its key.

const employee = new Map([
    ["name", "John"],
    ["age", 30]
]);


employee.delete("age");

Now:

"name" → "John"

Syntax:

map.delete(key);
7. size 🔥

Map uses size to tell us how many entries it contains.

const employee = new Map([
    ["name", "John"],
    ["age", 30]
]);


console.log(employee.size);

Output:

2

Remember:

Array → length


Set   → size


Map   → size
8. clear()

clear() removes everything.

const employee = new Map([
    ["name", "John"],
    ["age", 30]
]);


employee.clear();


console.log(employee.size);

Output:

0
9. Looping Through a Map

A Map can be looped using for...of.

const employee = new Map([
    ["name", "John"],
    ["age", 30]
]);


for (const [key, value] of employee) {
    console.log(key, value);
}

Output:

name John
age 30

For now, just remember:

for (const [key, value] of map) {
    // use key and value
}

No need to go deeper into Map iteration yet.

10. Map vs Set 🔥

This is important.

Set

Stores unique values.

const ids = new Set([101, 102, 103]);

Think:

101
102
103
Map

Stores key-value pairs.

const employees = new Map([
    [101, "John"],
    [102, "David"]
]);

Think:

101 → John
102 → David

Easy memory:

SET
 ↓
Unique values




MAP
 ↓
Key → Value
11. Map vs Object

Both can represent key-value information.

Normal object:

const employee = {
    id: 101,
    name: "John"
};

Map:

const employees = new Map();


employees.set(101, "John");
employees.set(102, "David");

For your current MERN preparation:

Object
→ normal structured application data


Map
→ key-value collection / lookup

That's enough for now.

12. Simple MERN Use

Imagine you want:

Employee ID → Employee


101 → John
102 → David
103 → Sam

A Map naturally represents this:

const employees = new Map();


employees.set(101, "John");
employees.set(102, "David");
employees.set(103, "Sam");

Then:

employees.get(102);

gives:

David

That's the main practical idea you need.

Interview Questions 🔥
What is a Map?

A Map is a JavaScript collection that stores key-value pairs.

How do you add or update a value?
map.set(key, value);
How do you retrieve a value?
map.get(key);
How do you check whether a key exists?
map.has(key);
How do you delete a value?
map.delete(key);
How do you remove everything?
map.clear();
How do you get the number of entries?
map.size;
What happens if the same key is added again?

The existing value for that key is updated.

Set vs Map?
Set → unique values


Map → key-value pairs
🧠 Final Map Cheat Sheet
CREATE


const map = new Map();
ADD / UPDATE


map.set(key, value);
GET


map.get(key);
CHECK


map.has(key);


→ true / false
DELETE


map.delete(key);
DELETE EVERYTHING


map.clear();
SIZE


map.size;
LOOP


for (const [key, value] of map) {
    console.log(key, value);
}
🔥 Remember
MAP
 ↓
KEY → VALUE
Same key again
 ↓
Value gets updated
Set → unique values
Map → key-value pairs


# >>>>>>>>>>>


FUNCTIONS — DEEP PRACTICE

Functions are one of the most important JavaScript concepts for MERN.

Think:

INPUT
  ↓
FUNCTION
  ↓
LOGIC
  ↓
OUTPUT

For your fast-track preparation, focus on these 7 function concepts.

1. Function Declaration 🔥

A function declaration creates a named function using the function keyword.

function greet(name) {
    return "Hello " + name;
}


console.log(greet("John"));

Output:

Hello John

Syntax:

function functionName(parameters) {
    // logic
    return value;
}
Important Point

Function declarations are hoisted, so this works:

greet();


function greet() {
    console.log("Hello");
}
Remember
function keyword
      ↓
function name
      ↓
parameters
      ↓
function body
      ↓
return
2. Function Expression 🔥

A function can also be stored inside a variable.

const greet = function (name) {
    return "Hello " + name;
};


console.log(greet("John"));

This is called a function expression.

Think:

Function
   ↓
stored inside
   ↓
variable
Declaration vs Expression

Function declaration:

function greet() {
    console.log("Hello");
}

Function expression:

const greet = function () {
    console.log("Hello");
};

🔥 Important interview difference:

Function Declaration
→ can generally be called before declaration




Function Expression
→ cannot be called before its initialization
3. Arrow Function 🔥🔥

Arrow functions provide a shorter way to write functions.

Normal function:

function add(a, b) {
    return a + b;
}

Arrow function:

const add = (a, b) => {
    return a + b;
};

If there is only one expression:

const add = (a, b) => a + b;

The result is automatically returned.

One Parameter
const greet = name => {
    return "Hello " + name;
};

Parentheses can be omitted for one simple parameter.

No Parameters
const greet = () => {
    console.log("Hello");
};
React Relevance 🔥

You will see arrow functions constantly in React:

const handleClick = () => {
    console.log("Clicked");
};

and:

users.map(user => user.name);

🔥 For now remember:

Arrow Function
     ↓
Short function syntax
     ↓
Extremely common in React

The deeper this behavior of arrow functions can wait until the dedicated this section.

4. Callback Function 🔥🔥🔥

A callback is a function passed to another function.

Example:

function greet(name) {
    console.log("Hello " + name);
}


function processUser(callback) {
    callback("John");
}


processUser(greet);

Here:

greet
  ↓
passed into
  ↓
processUser()

So greet is the callback.

You'll see this constantly:

numbers.map(number => number * 2);

Here:

number => number * 2

is a callback.

MERN Relevance 🔥

Callbacks appear everywhere:

array.map(() => {});
array.filter(() => {});
button.addEventListener("click", () => {});

Later you'll also encounter them in asynchronous code.

Remember
Callback
   ↓
A function passed to
another function
5. Higher-Order Function 🔥🔥

A higher-order function is a function that:

takes another function as an argument


OR


returns another function

Example:

function processUser(callback) {
    callback();
}

processUser receives another function.

Therefore:

processUser
     ↓
Higher-Order Function

While the function passed to it is the:

Callback

Easy relationship:

processUser(greet)
     ↓       ↓
    HOF   Callback

Common JavaScript examples:

map()
filter()
reduce()

These are higher-order functions because they receive callback functions.

6. Pure Function 🔥

A pure function:

Gives the same output for the same input.
Does not modify outside data.

Example:

function add(a, b) {
    return a + b;
}

Calling:

add(10, 20);

always produces:

30

It doesn't change anything outside the function.

Therefore it is pure.

Impure Function

Consider:

let total = 0;


function add(value) {
    total = total + value;
}

The function modifies:

total

which exists outside the function.

Therefore it has a side effect and is impure.

Think:

PURE


Input
 ↓
Function
 ↓
Output


No outside modification

vs:

IMPURE


Function
   ↓
Changes something outside
React Relevance 🔥

Pure-function thinking is important in React because you generally want predictable transformations and should avoid unexpected mutation/side effects during rendering.

You don't need deeper functional-programming theory right now.

7. Recursion 🔥

Recursion means a function calls itself.

Example:

function countdown(number) {


    if (number === 0) {
        return;
    }


    console.log(number);


    countdown(number - 1);
}


countdown(3);

Conceptually:

countdown(3)
    ↓
countdown(2)
    ↓
countdown(1)
    ↓
countdown(0)
    ↓
STOP

The condition that stops recursion is called the:

Base Case

Here:

if (number === 0) {
    return;
}

is the base case.

🔥 Without a proper stopping condition, recursion can continue until JavaScript throws a stack overflow error.

MERN Relevance

You won't use recursion every day in normal React/Express code.

But you should understand it because it appears in:

Nested data
Trees
Comments/replies
Folder structures
Menus
DSA/interviews

Don't go deeper into recursion algorithms now. We'll handle that during coding/DSA practice.

🔥 Callback vs Higher-Order Function

This is a common source of confusion.

function greet() {
    console.log("Hello");
}


function execute(callback) {
    callback();
}


execute(greet);

Here:

execute
   ↓
Higher-Order Function




greet
   ↓
Callback Function

Easy memory:

Function RECEIVING another function
        ↓
Higher-Order Function




Function being PASSED
        ↓
Callback
🔥 Declaration vs Expression vs Arrow
Declaration
function add(a, b) {
    return a + b;
}
Expression
const add = function (a, b) {
    return a + b;
};
Arrow
const add = (a, b) => {
    return a + b;
};

Short arrow:

const add = (a, b) => a + b;

Conceptually:

Declaration
→ traditional named declaration


Expression
→ function stored in variable


Arrow
→ shorter function syntax
→ extremely common in React
Interview Questions 🔥
What is a function declaration?

A named function created using the function keyword.

function greet() {}
What is a function expression?

A function created as part of an expression and commonly assigned to a variable.

const greet = function () {};
What is an arrow function?

A shorter syntax for writing functions.

const greet = () => {};
What is a callback?

A function passed to another function.

What is a higher-order function?

A function that takes another function as an argument or returns a function.

Callback vs higher-order function?
Passed function
→ Callback


Function receiving/returning function
→ Higher-Order Function
What is a pure function?

A function that gives the same output for the same input and doesn't cause side effects.

What is recursion?

When a function calls itself until a stopping condition is reached.

What is a base case?

The condition that stops recursion.

🧠 Final Functions Cheat Sheet
FUNCTION DECLARATION


function add(a, b) {
    return a + b;
}
FUNCTION EXPRESSION


const add = function (a, b) {
    return a + b;
};
ARROW FUNCTION


const add = (a, b) => a + b;
CALLBACK


Function passed to another function.
HIGHER-ORDER FUNCTION


Takes a function
OR
returns a function.
PURE FUNCTION


Same input → same output
No outside modification.
RECURSION


Function calls itself.
Must have a stopping/base condition.
🔥 Final Memory
Declaration
→ function greet() {}




Expression
→ const greet = function() {}




Arrow
→ const greet = () => {}




Callback
→ function PASSED




Higher-Order Function
→ function RECEIVING / RETURNING function




Pure
→ predictable + no side effects




Recursion - (usecases - comments , folder structure in windows etc.  )
→ function calls itself

# >>>>>>>

ARRAYS — MASTER

These methods are high-priority JavaScript concepts, so we should understand what each one does without going unnecessarily deep.

The easiest way to organize them is:

map()       → TRANSFORM
filter()    → FILTER
reduce()    → COMBINE
find()      → FIND ONE VALUE
findIndex() → FIND ONE INDEX
some()      → ANY?
every()     → ALL?
sort()      → SORT
flat()      → FLATTEN
flatMap()   → MAP + FLATTEN
1. map() 🔥🔥🔥

map() creates a new array by transforming every element of an existing array.

Example:

const numbers = [1, 2, 3];


const doubled = numbers.map(number => number * 2);


console.log(doubled);

Output:

[2, 4, 6]

Think:

[1, 2, 3]


    ↓ map()


1 → 2
2 → 4
3 → 6


    ↓


[2, 4, 6]
Important Rule

map() returns a new array.

const result = array.map(item => {
    return /* transformed item */;
});

The original array is not changed by map() itself.

Common Object Example
const users = [
    { id: 1, name: "John" },
    { id: 2, name: "David" }
];


const names = users.map(user => user.name);

Result:

["John", "David"]

🔥 Remember:

map()
  ↓
One input element
  ↓
One transformed element
  ↓
New array
2. filter() 🔥🔥🔥

filter() creates a new array containing only elements that pass a condition.

const numbers = [10, 20, 30, 40];


const result = numbers.filter(number => number > 20);

Result:

[30, 40]

Think:

10 → false ❌
20 → false ❌
30 → true  ✅
40 → true  ✅


        ↓


    [30, 40]

Syntax:

const result = array.filter(item => condition);

The callback should produce:

true  → keep item
false → remove item

filter() returns a new array.

🔥 Remember:

filter()
   ↓
KEEP matching items
3. reduce() 🔥🔥🔥

reduce() combines an array into a single accumulated result.

Example:

const numbers = [10, 20, 30];


const total = numbers.reduce(
    (accumulator, number) => accumulator + number,
    0
);

Output:

60

Think:

Starting value = 0


0 + 10
   ↓
10


10 + 20
   ↓
30


30 + 30
   ↓
60

The two important parts are:

(accumulator, currentValue)

and:

0

The 0 is the initial accumulator value.

General syntax:

array.reduce((accumulator, currentValue) => {
    return updatedAccumulator;
}, initialValue);

🔥 Easy memory:

map()     → array → array


filter()  → array → array


reduce()  → array → accumulated result

For now, understand the accumulator idea. More complicated reduce() patterns can come during coding practice.

4. find() 🔥🔥

find() returns the first element that matches a condition.

const users = [
    { id: 1, name: "John" },
    { id: 2, name: "David" },
    { id: 3, name: "Sam" }
];


const user = users.find(user => user.id === 2);

Result:

{
    id: 2,
    name: "David"
}

If nothing matches:

users.find(user => user.id === 100);

returns:

undefined

🔥 Important:

find()
 ↓
FIRST matching element
5. findIndex()

findIndex() works similarly to find(), but returns the index.

const numbers = [10, 20, 30, 40];


const index = numbers.findIndex(
    number => number === 30
);

Output:

2

Because:

INDEX    VALUE


0   →     10
1   →     20
2   →     30  ← found
3   →     40

If nothing matches:

-1

Remember:

find()
→ element




findIndex()
→ index
6. some() 🔥🔥

some() asks:

Does at least ONE element satisfy this condition?

It returns a boolean.

const numbers = [10, 20, 30];


const result = numbers.some(
    number => number > 25
);

Output:

true

Because 30 satisfies the condition.

Think:

10 ❌
20 ❌
30 ✅


At least ONE? → YES


true

If nobody matches:

false

🔥 Remember:

some()
 ↓
ANY?
 ↓
true / false
7. every() 🔥

every() asks:

Do ALL elements satisfy this condition?

const numbers = [10, 20, 30];


const result = numbers.every(
    number => number > 5
);

Output:

true

Because:

10 > 5 ✅
20 > 5 ✅
30 > 5 ✅


ALL passed


→ true

But:

numbers.every(number => number > 15);

returns:

false

because 10 failed.

some() vs every()
some()
→ AT LEAST ONE




every()
→ ALL

Both return:

true / false
8. sort() 🔥🔥

sort() sorts an array.

For numbers, use a compare function.

Ascending:

const numbers = [30, 10, 40, 20];


numbers.sort((a, b) => a - b);

Result:

[10, 20, 30, 40]

Descending:

numbers.sort((a, b) => b - a);

Result:

[40, 30, 20, 10]

🔥 Memorize:

a - b
→ ascending




b - a
→ descending
Important Trap 🔥

sort() mutates the original array.

const numbers = [30, 10, 20];


numbers.sort((a, b) => a - b);

numbers itself has now changed.

If you want to keep the original:

const sorted = [...numbers].sort((a, b) => a - b);
9. flat()

flat() flattens nested arrays.

const numbers = [
    1,
    2,
    [3, 4]
];


const result = numbers.flat();

Result:

[1, 2, 3, 4]

Think:

[1, 2, [3, 4]]


       ↓ flat()


[1, 2, 3, 4]

By default it flattens one level.

Example:

const numbers = [
    1,
    [2, [3, 4]]
];


console.log(numbers.flat());

Result:

[1, 2, [3, 4]]

You can specify depth:

numbers.flat(2);

Result:

[1, 2, 3, 4]

That's enough for flat().

10. flatMap()

flatMap() basically performs:

map()
  +
flat(1)

Example:

const numbers = [1, 2, 3];


const result = numbers.flatMap(
    number => [number, number * 2]
);

Result:

[1, 2, 2, 4, 3, 6]

Conceptually:

map


1 → [1, 2]
2 → [2, 4]
3 → [3, 6]


       ↓


[
 [1, 2],
 [2, 4],
 [3, 6]
]


       ↓ flatten


[1, 2, 2, 4, 3, 6]

For now:

flatMap()
    ↓
map()
 +
flat(1)

No need to go deeper.

🔥 Most Important Differences
map() vs filter()
map()
→ TRANSFORM elements




filter()
→ SELECT elements
find() vs filter()
find()
→ first matching ELEMENT
→ returns element / undefined




filter()
→ ALL matching elements
→ returns array
some() vs every()
some()
→ does ANY element pass?




every()
→ do ALL elements pass?
map() vs reduce()
map()
→ transforms each element
→ returns array




reduce()
→ accumulates values
→ returns accumulated result
Interview Questions 🔥

What does map() do?
Transforms each array element and returns a new array.

What does filter() do?
Returns a new array containing elements that satisfy a condition.

What does reduce() do?
Processes the array into an accumulated result.

find() vs filter()?

find()
→ first match


filter()
→ all matches

What does findIndex() return if nothing matches?

-1

What does find() return if nothing matches?

undefined

some() vs every()?

some()  → any?


every() → all?

Does sort() modify the original array?

Yes. 🔥

🧠 Final Array Methods Cheat Sheet
METHOD          THINK


map()           TRANSFORM
                  ↓
               new array




filter()        SELECT
                  ↓
               new array




reduce()        ACCUMULATE
                  ↓
               result




find()          FIRST MATCH
                  ↓
               element / undefined




findIndex()     FIRST MATCH
                  ↓
               index / -1




some()          ANY?
                  ↓
               true / false




every()         ALL?
                  ↓
               true / false




sort()          SORT
                  ↓
               ⚠️ mutates original




flat()          FLATTEN
                  ↓
               nested → flat




flatMap()       MAP + FLAT(1)
🔥 The six you should be strongest in
map()
filter()
reduce()
find()
some()
every()

sort() is also important, especially remembering the numeric comparator and mutation behavior. flat() and flatMap() only need the basic understanding above for now.


# >>>>


OBJECTS — MASTER

For fast-track JavaScript, these are the object concepts you need to understand well:

References       → objects share memory references
Shallow Copy     → copy only the first level
Deep Copy        → independent nested copy
Destructuring    → extract properties
Spread           → copy / merge
Rest             → collect remaining properties
Object.keys()    → get keys
Object.values()  → get values
Object.entries() → get key-value pairs
1. References 🔥🔥🔥

Objects are reference types.

Consider:

const user1 = {
    name: "John"
};


const user2 = user1;

This does not create a new object.

Think:

user1 ──┐
        ├──→ { name: "John" }
user2 ──┘

Both variables point to the same object.

So:

user2.name = "David";


console.log(user1.name);

Output:

David

Because the original object was modified.

Important
const user2 = user1;

means:

Copy reference ❗


NOT


Copy object

This concept is extremely important before understanding shallow and deep copies.

2. Shallow Copy 🔥🔥🔥

A shallow copy creates a new outer object, but nested objects can still be shared.

The common way:

const user = {
    name: "John",
    age: 30
};


const copy = {
    ...user
};

Now:

user ──→ Object A


copy ──→ Object B

They are different outer objects.

So:

copy.name = "David";

doesn't change:

user.name;
But nested objects matter 🔥
const user = {
    name: "John",
    address: {
        city: "Mumbai"
    }
};


const copy = {
    ...user
};

The outer objects are different, but the nested address object is shared.

Conceptually:

user ──→ Object A ──┐
                    ├──→ address object
copy ──→ Object B ──┘

Therefore:

copy.address.city = "Delhi";


console.log(user.address.city);

Output:

Delhi

🔥 Remember:

Shallow Copy
     ↓
First level copied
     ↓
Nested references may remain shared
3. Deep Copy 🔥🔥

A deep copy creates an independent copy, including nested data.

Modern JavaScript provides:

structuredClone()

Example:

const user = {
    name: "John",
    address: {
        city: "Mumbai"
    }
};


const copy = structuredClone(user);

Now:

copy.address.city = "Delhi";

The original remains:

console.log(user.address.city);

Output:

Mumbai

Think:

SHALLOW COPY


Outer object     → new
Nested object    → may be shared




DEEP COPY


Outer object     → new
Nested object    → new

For now, that's enough. Deep-cloning limitations and special cases can wait.

4. Destructuring 🔥🔥🔥

Destructuring lets us extract object properties into variables.

Instead of:

const user = {
    name: "John",
    age: 30
};


const name = user.name;
const age = user.age;

We can write:

const { name, age } = user;

Now:

console.log(name);
console.log(age);

Output:

John
30

Think:

{
    name: "John",
    age: 30
}


       ↓ destructure


name = "John"
age = 30
Rename while destructuring
const { name: userName } = user;

Now:

console.log(userName);

Output:

John
Default value
const { role = "user" } = user;

If role is missing:

role = "user"

🔥 Destructuring is a concept you should be very comfortable with.

5. Spread 🔥🔥🔥

Object spread uses:

...

It is commonly used to copy or combine objects.

Copy
const user = {
    name: "John",
    age: 30
};


const copy = {
    ...user
};
Add a property
const updatedUser = {
    ...user,
    role: "admin"
};

Result:

{
    name: "John",
    age: 30,
    role: "admin"
}
Update a property
const updatedUser = {
    ...user,
    name: "David"
};

Result:

{
    name: "David",
    age: 30
}

🔥 Order matters.

{
    ...user,
    name: "David"
}

The later name overwrites the earlier name.

Remember:

Spread
  ↓
EXPAND properties
6. Rest 🔥🔥

Rest also uses:

...

But its purpose is different.

const user = {
    id: 101,
    name: "John",
    age: 30
};


const { id, ...remaining } = user;

Now:

id

contains:

101

and:

remaining

contains:

{
    name: "John",
    age: 30
}

Think:

REST
 ↓
Collect what remains
Spread vs Rest 🔥

Same syntax:

...

Different purpose:

Spread
→ EXPAND




Rest
→ COLLECT remaining values
7. Object.keys() 🔥

Object.keys() returns an array containing the object's keys.

const user = {
    name: "John",
    age: 30,
    role: "admin"
};


const keys = Object.keys(user);

Result:

["name", "age", "role"]

Remember:

Object.keys(object)
        ↓
Array of keys
8. Object.values()

Object.values() returns an array containing the object's values.

const user = {
    name: "John",
    age: 30
};


const values = Object.values(user);

Result:

["John", 30]

Remember:

Object.values(object)
          ↓
Array of values
9. Object.entries() 🔥🔥

Object.entries() returns key-value pairs.

const user = {
    name: "John",
    age: 30
};


const entries = Object.entries(user);

Result:

[
    ["name", "John"],
    ["age", 30]
]

Think:

Object


name → John
age  → 30


       ↓ Object.entries()


[
    ["name", "John"],
    ["age", 30]
]

This also makes object iteration convenient:

for (const [key, value] of Object.entries(user)) {
    console.log(key, value);
}
🔥 Shallow Copy vs Deep Copy

Very important interview distinction:

SHALLOW COPY


Original
   │
   ├── primitive properties → copied
   │
   └── nested object ───────┐
                            ↓
Copy ─────────────────→ same nested reference

Versus:

DEEP COPY


Original
   ↓
Independent nested structure




Copy
   ↓
Independent nested structure

Simple interview answer:

A shallow copy creates a new outer object but nested references may still be shared. A deep copy creates independent nested data as well.

🔥 Spread vs Rest
Spread
const copy = {
    ...user
};

Think:

SPREAD
→ expand
Rest
const { id, ...remaining } = user;

Think:

REST
→ collect remaining

Same ..., meaning depends on where it is used.

Interview Questions 🔥
Are objects copied by value or reference?

Object variables hold references. Assigning one object variable to another makes both refer to the same object.

What is a shallow copy?

A new outer object where nested references may still be shared.

How can you make a shallow object copy?
const copy = { ...original };
What is a deep copy?

A copy where nested data is also independently copied.

What does destructuring do?

Extracts object properties into variables.

const { name, age } = user;
Spread vs rest?
Spread → expand


Rest → collect remaining
What does Object.keys() return?

An array of keys.

What does Object.values() return?

An array of values.

What does Object.entries() return?

An array of [key, value] pairs.

🧠 Final Objects Cheat Sheet
REFERENCE


const user2 = user1;


→ Same object
SHALLOW COPY


const copy = { ...user };


→ New outer object
→ Nested references may be shared
DEEP COPY


const copy = structuredClone(user);


→ Independent nested copy
DESTRUCTURING


const { name, age } = user;
SPREAD


const updated = {
    ...user,
    age: 31
};


→ Expand / copy
REST


const { id, ...remaining } = user;


→ Collect remaining properties
OBJECT METHODS


Object.keys(user)
→ ["name", "age"]


Object.values(user)
→ ["John", 30]


Object.entries(user)
→ [
     ["name", "John"],
     ["age", 30]
   ]
🔥 Final Memory
References       → SAME object


Shallow Copy     → NEW outer object


Deep Copy        → INDEPENDENT nested copy


Destructuring    → EXTRACT


Spread           → EXPAND


Rest             → COLLECT


Object.keys()    → KEYS


Object.values()  → VALUES


Object.entries() → KEY + VALUE


#### 7. JavaScript Internals


— Execution Context 🔥🔥🔥

An execution context is the environment JavaScript creates to run code.

Simple way to remember:

JavaScript needs to execute some code
              ↓
Creates an Execution Context
              ↓
Runs that code
1. Global Execution Context

When a JavaScript program starts, JavaScript creates the Global Execution Context.

Example:

const name = "John";
const age = 30;


function greet() {
    console.log("Hello");
}

Conceptually:

JavaScript file starts
        ↓
Global Execution Context created
        ↓
name
age
greet

There is generally one global execution context for the script.

2. Function Execution Context 🔥

Every time a function is called, JavaScript creates a new function execution context for that call.

function add(a, b) {
    const result = a + b;
    return result;
}


add(10, 20);

Think:

Global Execution Context
          │
          │ add(10, 20)
          ↓
Function Execution Context
          ↓
a = 10
b = 20
result = 30

When the function finishes, that execution context is no longer needed for normal execution.

3. Every Function Call Gets Its Own Context 🔥

Consider:

function greet(name) {
    const message = "Hello " + name;
    console.log(message);
}


greet("John");
greet("David");

First call:

greet("John")
      ↓
Execution Context #1


name = "John"
message = "Hello John"

After it finishes:

Execution Context #1
        ↓
      finished

Second call:

greet("David")
      ↓
Execution Context #2


name = "David"
message = "Hello David"

So even though the same function is called twice, each call gets its own execution context.

4. Creation and Execution 🔥

At interview level, know that an execution context is commonly explained in two phases:

Execution Context
       │
       ├── Creation Phase
       │
       └── Execution Phase
Creation Phase

JavaScript prepares the environment before executing the statements.

This is where concepts such as:

Hoisting
Function declarations
var
let / const

become relevant.

We won't go deeper here, because Hoisting and TDZ are separate upcoming topics.

Execution Phase

JavaScript executes the statements.

Example:

const a = 10;
const b = 20;


const result = a + b;

Conceptually:

Creation
   ↓
Prepare environment


Execution
   ↓
a = 10
b = 20
result = 30

That's enough depth for now.

5. Execution Context and Functions

Consider:

const x = 10;


function calculate() {
    const y = 20;
    return x + y;
}


calculate();

Conceptually:

GLOBAL EXECUTION CONTEXT


x = 10
calculate = function


          │
          │ calculate()
          ↓


FUNCTION EXECUTION CONTEXT


y = 20
return x + y

This gives us the foundation for understanding the next concepts:

Execution Context
      ↓
Call Stack
      ↓
Scope
      ↓
Closures

Don't try to learn all of those together yet.

Interview Questions 🔥
What is an execution context?

An execution context is the environment in which JavaScript code is executed.

What is the Global Execution Context?

It is the execution context created when JavaScript starts executing the script.

What happens when a function is called?

A new function execution context is created for that function call.

If the same function is called twice?

Each call gets its own function execution context.

What are the two commonly discussed phases?
Creation Phase
Execution Phase
🧠 Cheat Sheet
JavaScript starts
      ↓
Global Execution Context
Function called
      ↓
Function Execution Context
function add(a, b) {
    const result = a + b;
    return result;
}


add(10, 20);


        ↓


Function Execution Context


a = 10
b = 20
result = 30
🔥 Final Memory
EXECUTION CONTEXT
       ↓
Environment where JS code executes




Program starts
       ↓
Global Execution Context




Function called
       ↓
New Function Execution Context




Same function called again
       ↓
Another Function Execution Context


# >>

2 — CALL STACK 🔥🔥🔥

The Call Stack keeps track of which functions are currently running.

Simple memory:

Function called
     ↓
Added to Call Stack


Function finishes
     ↓
Removed from Call Stack

It works like a stack of plates:

Last added
   ↓
First removed

This is called:

LIFO


Last In
First Out
1. Basic Example
function greet() {
    console.log("Hello");
}


greet();

When the program starts:

CALL STACK


┌──────────────┐
│ Global       │
└──────────────┘

When:

greet();

is called:

CALL STACK


┌──────────────┐
│ greet()      │ ← running
├──────────────┤
│ Global       │
└──────────────┘

When greet() finishes:

┌──────────────┐
│ Global       │
└──────────────┘

So:

CALL → PUSH


FINISH → POP
2. Multiple Function Calls 🔥

Consider:

function first() {
    second();
}


function second() {
    console.log("Hello");
}


first();

Execution starts:

Global

Then first():

┌──────────────┐
│ first()      │
├──────────────┤
│ Global       │
└──────────────┘

Inside first(), we call:

second();

Now:

┌──────────────┐
│ second()     │ ← running
├──────────────┤
│ first()      │
├──────────────┤
│ Global       │
└──────────────┘

second() finishes first:

┌──────────────┐
│ first()      │
├──────────────┤
│ Global       │
└──────────────┘

Then first() finishes:

┌──────────────┐
│ Global       │
└──────────────┘

🔥 That's LIFO:

second() entered last
        ↓
second() leaves first
3. Execution Context vs Call Stack 🔥

Don't confuse these two.

Execution Context

The environment created to execute code.

Call Stack

The structure JavaScript uses to keep track of active execution contexts/function calls.

Think:

Function called
      ↓
Execution Context created
      ↓
Added to Call Stack
      ↓
Function executes
      ↓
Function finishes
      ↓
Removed from Call Stack
4. Stack Overflow 🔥

Consider recursion with no stopping condition:

function test() {
    test();
}


test();

What happens?

test()
  ↓
test()
  ↓
test()
  ↓
test()
  ↓
...

The Call Stack keeps growing:

┌──────────────┐
│ test()       │
├──────────────┤
│ test()       │
├──────────────┤
│ test()       │
├──────────────┤
│ test()       │
├──────────────┤
│ Global       │
└──────────────┘

Eventually the stack reaches its limit.

JavaScript throws an error such as:

Maximum call stack size exceeded

This is commonly called a stack overflow.

Interview Questions 🔥
What is the Call Stack?

The Call Stack is the mechanism JavaScript uses to keep track of active function calls.

What principle does the Call Stack follow?
LIFO
↓
Last In, First Out
What happens when a function is called?

Its execution is added/pushed onto the Call Stack.

What happens when the function finishes?

It is removed/popped from the Call Stack.

What is a stack overflow?

It happens when too many function calls accumulate on the Call Stack, commonly because of uncontrolled recursion.

Execution Context vs Call Stack?
Execution Context
→ environment where code executes


Call Stack
→ keeps track of active execution contexts/function calls
🧠 Call Stack Cheat Sheet
CALL STACK
    ↓
Tracks function execution
Function called
    ↓
PUSH


Function finished
    ↓
POP
LIFO
 ↓
Last In
First Out

Example:

first()
   ↓
second()


CALL STACK


┌──────────────┐
│ second()     │ ← removed first
├──────────────┤
│ first()      │
├──────────────┤
│ Global       │
└──────────────┘


# >>>

3 — SCOPE 🔥🔥🔥

Scope determines where a variable can be accessed in your code.

Simple memory:

SCOPE
  ↓
Where can I use this variable?

JavaScript mainly has:

1. Global Scope
2. Function Scope
3. Block Scope
1. Global Scope

A variable declared outside functions and blocks is in the global scope.

const name = "John";


function greet() {
    console.log(name);
}


greet();

Output:

John

name can be accessed inside greet() because it was declared outside the function.

Think:

GLOBAL


const name = "John";


        ↓ accessible


function greet() {
    console.log(name);
}
Remember
Global variable
      ↓
Accessible from inner scopes
(subject to normal JS rules)
2. Function Scope 🔥

Variables declared inside a function belong to that function's scope.

function greet() {
    const message = "Hello";


    console.log(message);
}


greet();

This works.

But:

function greet() {
    const message = "Hello";
}


console.log(message);

This causes:

ReferenceError

Because message only exists inside greet().

Think:

function greet() {


    const message = "Hello";
          ↓
    available here


}


console.log(message);
            ❌
3. Block Scope 🔥🔥

A block is code inside { }.

For example:

if (true) {
    const message = "Hello";


    console.log(message);
}

Inside the block:

message ✅

Outside:

console.log(message);
message ❌

let and const are block-scoped.

if (true) {
    let age = 30;
    const name = "John";
}

Neither can normally be accessed outside that block.

4. var vs let / const Scope 🔥

Important interview concept.

let and const:

Block Scoped

But var:

Function Scoped

Example:

if (true) {
    var age = 30;
}


console.log(age);

Output:

30

But:

if (true) {
    let age = 30;
}


console.log(age);

causes:

ReferenceError

So remember:

var
 ↓
Function scoped




let / const
 ↓
Block scoped

This is another reason modern JavaScript normally prefers const and let.

5. Inner Scope Can Access Outer Scope 🔥
const company = "ABC";


function showEmployee() {
    const name = "John";


    console.log(company);
    console.log(name);
}

Inside the function:

company ✅
name    ✅

But outside:

console.log(name);
❌

Think:

OUTER SCOPE


company
   │
   ▼
INNER SCOPE


name

The inner scope can access the outer variable.

But the outer scope cannot directly access variables created only inside the inner scope.

This leads directly into Lexical Scope and Scope Chain, which we'll cover separately.

Interview Questions 🔥
What is scope?

Scope determines where variables can be accessed in JavaScript.

What are the important types of scope?
Global Scope
Function Scope
Block Scope
Are let and const block-scoped?

Yes.

Is var block-scoped?

No. var is function-scoped.

Can a variable declared inside a function be accessed outside it?

Normally, no.

🧠 Scope Cheat Sheet
GLOBAL SCOPE


const name = "John";


→ available to inner scopes
FUNCTION SCOPE


function test() {
    const x = 10;
}


x only exists inside test()
BLOCK SCOPE


if (true) {
    const x = 10;
}


x only exists inside the block
🔥 Final Memory
SCOPE
 ↓
Where can this variable be accessed?




Global
→ outer/global area




Function
→ inside function




Block
→ inside { }




var
→ function scoped




let / const
→ block scoped

# >>>

4 — LEXICAL SCOPE 🔥🔥

Lexical scope means a function can access variables based on where the function is written in the code.

Simple memory:

LEXICAL SCOPE
      ↓
Where the function is WRITTEN
determines what it can access
1. Basic Example
const name = "John";


function greet() {
    console.log(name);
}


greet();

Output:

John

Why?

greet() was written inside the scope where name is available.

Global Scope


name = "John"


    ↓ accessible


greet()
2. Nested Functions 🔥

Consider:

function outer() {


    const name = "John";


    function inner() {
        console.log(name);
    }


    inner();
}


outer();

Output:

John

inner() can access name from its outer scope.

Think:

outer()
│
├── name = "John"
│
└── inner()
       │
       └── can access name ✅

This is lexical scope.

3. Outer Cannot Access Inner Variables

Now:

function outer() {


    function inner() {
        const message = "Hello";
    }


    console.log(message);
}

This doesn't work.

Why?

message belongs to the inner function.

outer()
│
├── message ❌
│
└── inner()
       │
       └── message = "Hello" ✅

Easy rule:

Inner scope
    ↓
Can look OUTWARD ✅




Outer scope
    ↓
Cannot look INTO inner scope ❌
4. Where Defined, Not Where Called 🔥🔥

This is the important lexical-scope idea.

const name = "Global";


function showName() {
    console.log(name);
}


function test() {
    const name = "Local";


    showName();
}


test();

What prints?

Global

Not:

Local

Why?

showName() was defined in the global scope.

showName() defined here
        ↓
Global Scope
        ↓
name = "Global"

It doesn't suddenly use test()'s variables just because it was called from test().

🔥 Remember:

Lexical Scope


Where function is DEFINED
        ↓
determines its surrounding scope


NOT


Where function is CALLED
Scope vs Lexical Scope

Don't overcomplicate this.

Scope
Where can a variable be accessed?
Lexical Scope
How is that accessibility determined?


        ↓


By where functions/blocks
are written in the code.
Interview Questions 🔥
What is lexical scope?

Lexical scope means variable accessibility is determined by where functions and blocks are written in the source code.

Can an inner function access variables from its outer function?

Yes.

function outer() {
    const x = 10;


    function inner() {
        console.log(x);
    }
}
Can the outer function access variables declared only inside the inner function?

No.

Does lexical scope depend on where a function is called?

No. It depends on where the function is defined.

🧠 Lexical Scope Cheat Sheet
function outer() {


    const x = 10;


    function inner() {
        console.log(x);
    }
}
outer()
│
├── x = 10
│
└── inner()
       │
       └── x ✅
🔥 Final Memory
LEXICAL SCOPE
      ↓
Function looks at where
it was DEFINED
      ↓
Inner can access outer
      ↓
Outer cannot access inner

That's enough for Lexical Scope ✅.

# >>>

4 — LEXICAL SCOPE 🔥🔥

Lexical scope means a function can access variables based on where the function is written in the code.

Simple memory:

LEXICAL SCOPE
      ↓
Where the function is WRITTEN
determines what it can access
1. Basic Example
const name = "John";


function greet() {
    console.log(name);
}


greet();

Output:

John

Why?

greet() was written inside the scope where name is available.

Global Scope


name = "John"


    ↓ accessible


greet()
2. Nested Functions 🔥

Consider:

function outer() {


    const name = "John";


    function inner() {
        console.log(name);
    }


    inner();
}


outer();

Output:

John

inner() can access name from its outer scope.

Think:

outer()
│
├── name = "John"
│
└── inner()
       │
       └── can access name ✅

This is lexical scope.

3. Outer Cannot Access Inner Variables

Now:

function outer() {


    function inner() {
        const message = "Hello";
    }


    console.log(message);
}

This doesn't work.

Why?

message belongs to the inner function.

outer()
│
├── message ❌
│
└── inner()
       │
       └── message = "Hello" ✅

Easy rule:

Inner scope
    ↓
Can look OUTWARD ✅




Outer scope
    ↓
Cannot look INTO inner scope ❌
4. Where Defined, Not Where Called 🔥🔥

This is the important lexical-scope idea.

const name = "Global";


function showName() {
    console.log(name);
}


function test() {
    const name = "Local";


    showName();
}


test();

What prints?

Global

Not:

Local

Why?

showName() was defined in the global scope.

showName() defined here
        ↓
Global Scope
        ↓
name = "Global"

It doesn't suddenly use test()'s variables just because it was called from test().

🔥 Remember:

Lexical Scope


Where function is DEFINED
        ↓
determines its surrounding scope


NOT


Where function is CALLED
Scope vs Lexical Scope

Don't overcomplicate this.

Scope
Where can a variable be accessed?
Lexical Scope
How is that accessibility determined?


        ↓


By where functions/blocks
are written in the code.
Interview Questions 🔥
What is lexical scope?

Lexical scope means variable accessibility is determined by where functions and blocks are written in the source code.

Can an inner function access variables from its outer function?

Yes.

function outer() {
    const x = 10;


    function inner() {
        console.log(x);
    }
}
Can the outer function access variables declared only inside the inner function?

No.

Does lexical scope depend on where a function is called?

No. It depends on where the function is defined.

🧠 Lexical Scope Cheat Sheet
function outer() {


    const x = 10;


    function inner() {
        console.log(x);
    }
}
outer()
│
├── x = 10
│
└── inner()
       │
       └── x ✅
🔥 Final Memory
LEXICAL SCOPE
      ↓
Function looks at where
it was DEFINED
      ↓
Inner can access outer
      ↓
Outer cannot access inner

That's enough for Lexical Scope ✅.

Next: 5 — Scope Chain.

# >>

5 — SCOPE CHAIN 🔥🔥🔥

The scope chain is the process JavaScript uses to search for a variable through outer scopes.

Simple memory:

Variable needed
      ↓
Check current scope
      ↓
Not found?
      ↓
Check outer scope
      ↓
Keep going outward
      ↓
Until global scope
1. Basic Example
const company = "Google";


function employee() {
    const name = "John";


    console.log(name);
    console.log(company);
}


employee();

Inside employee():

name
 ↓
Found in current scope ✅




company
 ↓
Not in current scope
 ↓
Check outer scope
 ↓
Found ✅

This outward search is the scope chain.

2. Multiple Levels 🔥
const a = 10;


function outer() {
    const b = 20;


    function inner() {
        const c = 30;


        console.log(a);
        console.log(b);
        console.log(c);
    }


    inner();
}


outer();

Inside inner():

Looking for c


inner scope
   ↓
Found ✅




Looking for b


inner scope
   ↓
Not found
   ↓
outer scope
   ↓
Found ✅




Looking for a


inner scope
   ↓
Not found
   ↓
outer scope
   ↓
Not found
   ↓
global scope
   ↓
Found ✅

So the chain looks like:

INNER
  ↓
OUTER
  ↓
GLOBAL
3. JavaScript Uses the Nearest Variable 🔥

Consider:

const name = "Global";


function outer() {
    const name = "Outer";


    function inner() {
        console.log(name);
    }


    inner();
}


outer();

Output:

Outer

Why?

JavaScript starts from the nearest scope:

inner()
   ↓
Does inner have name?
NO
   ↓
Check outer
   ↓
name = "Outer" ✅


STOP SEARCHING

It doesn't continue to the global name.

🔥 Rule:

First matching variable found
          ↓
        USED
4. What If JavaScript Cannot Find It?
function greet() {
    console.log(username);
}


greet();

JavaScript searches:

greet scope
    ↓
Not found
    ↓
Global scope
    ↓
Not found
    ↓
ReferenceError ❌

So if the variable cannot be found anywhere in the accessible scope chain:

ReferenceError
5. Lexical Scope vs Scope Chain 🔥

These two are closely related, but don't confuse them.

Lexical Scope

Determines which outer scopes are available based on where the code/function is defined.

Lexical Scope
      ↓
Which scopes can I access?
Scope Chain

JavaScript searches through those scopes for a variable.

Scope Chain
     ↓
Where should I search for the variable?

Easy memory:

LEXICAL SCOPE
→ defines the relationship




SCOPE CHAIN
→ follows that relationship to search
Interview Questions 🔥
What is the scope chain?

The scope chain is the mechanism JavaScript uses to search for variables from the current scope toward outer scopes.

In which direction does JavaScript search?
Current Scope
     ↓
Outer Scope
     ↓
Global Scope
What happens when JavaScript finds the variable?

It uses the first/nearest matching variable and stops searching.

What happens if the variable isn't found?

JavaScript throws a ReferenceError when that unresolved identifier is evaluated.

Scope vs scope chain?
Scope
→ where variables are accessible


Scope Chain
→ how JavaScript searches through accessible scopes
🧠 Scope Chain Cheat Sheet
VARIABLE NEEDED
      ↓
Current Scope
      ↓
Not found?
      ↓
Outer Scope
      ↓
Not found?
      ↓
Keep moving outward
      ↓
Global Scope
      ↓
Still not found?
      ↓
ReferenceError
🔥 Final Memory
Lexical Scope
→ determines the scope relationships


Scope Chain
→ searches through those scopes


Search direction:
INNER → OUTER → GLOBAL


Nearest match wins.


# >>>

6 — HOISTING 🔥🔥🔥

Hoisting means JavaScript processes declarations before executing the code.

Simple memory:

HOISTING
   ↓
JavaScript knows about certain
declarations before their line
is executed

⚠️ Don't think JavaScript physically moves your code to the top. That's just a common way of visualizing it.

1. Function Declaration Hoisting 🔥

This works:

greet();


function greet() {
    console.log("Hello");
}

Output:

Hello

Even though greet() is called before its declaration.

Why?

The function declaration is available before normal code execution reaches that line.

Think:

greet();


function greet() { ... }


       ↓


Function declaration
is already available

🔥 Remember:

Function Declaration
        ↓
Fully hoisted
2. var Hoisting 🔥🔥

Consider:

console.log(age);


var age = 30;

Output:

undefined

Not:

ReferenceError

Why?

Conceptually, think of it like:

var age;


console.log(age);


age = 30;

So:

Declaration
var age
   ↓
Hoisted




Assignment
age = 30
   ↓
Not hoisted

🔥 Very important:

var declaration → hoisted


var value/assignment → not hoisted
3. let and const 🔥🔥🔥

Consider:

console.log(age);


let age = 30;

This gives:

ReferenceError

Same with:

console.log(name);


const name = "John";
ReferenceError

let and const are also associated with hoisting, but they cannot be accessed before initialization.

That area is called:

Temporal Dead Zone
        ↓
TDZ

TDZ is our next topic, so we won't go deeper into it here.

For now:

var
→ before assignment: undefined




let / const
→ cannot access before initialization
4. Function Expression Hoisting

Consider:

greet();


const greet = function () {
    console.log("Hello");
};

This fails.

Why?

greet is a const variable.

So it follows the rules of:

const
 ↓
Cannot access before initialization

The fact that a function is assigned to it doesn't make it behave like a function declaration.

5. Arrow Function Hoisting

Same idea:

greet();


const greet = () => {
    console.log("Hello");
};

This also fails.

Because:

greet
 ↓
const variable
 ↓
Cannot access before initialization

So don't confuse:

function greet() {}

with:

const greet = () => {};

Their behavior before declaration/initialization is different.

🔥 Most Important Comparison
FUNCTION DECLARATION


greet();


function greet() {}


        ✅ Works
VAR


console.log(age);


var age = 30;


        ↓
undefined
LET


console.log(age);


let age = 30;


        ↓
ReferenceError
CONST


console.log(age);


const age = 30;


        ↓
ReferenceError
FUNCTION EXPRESSION / ARROW WITH CONST


greet();


const greet = () => {};


        ↓
ReferenceError
Interview Questions 🔥
What is hoisting?

Hoisting is JavaScript's behavior of processing declarations before normal code execution.

Does JavaScript physically move code to the top?

No. That's only a mental model used to explain hoisting.

What happens when var is accessed before assignment?
undefined
Can let and const be accessed before initialization?

No. It results in a ReferenceError.

Are function declarations hoisted?

Yes. Function declarations are available before their declaration line executes.

Function declaration vs arrow function?
function greet() {}
→ can be called before declaration




const greet = () => {};
→ cannot be called before initialization
🧠 Hoisting Cheat Sheet
HOISTING
   │
   ├── function declaration
   │        ↓
   │     available early ✅
   │
   ├── var
   │        ↓
   │     undefined before assignment
   │
   ├── let
   │        ↓
   │     ReferenceError before initialization
   │
   └── const
            ↓
        ReferenceError before initialization
🔥 Final Memory
function declaration
→ fully available




var
→ declaration hoisted
→ initialized as undefined




let / const
→ cannot access before initialization




Function expression / arrow
→ behavior depends on the variable
  holding the function


# >>>>>>>

7 — TDZ (Temporal Dead Zone) 🔥🔥

TDZ means the period where a let or const variable exists but cannot be accessed yet.

TDZ stands for:

Temporal Dead Zone

Simple memory:

Scope starts
    ↓
TDZ
    ↓
Variable declaration / initialization
    ↓
Can access variable ✅
1. Basic Example
console.log(age);


let age = 30;

Output:

ReferenceError

Why?

Because age is being accessed before initialization.

Conceptually:

Start of scope
     ↓
     │
     │  TDZ ❌
     │
console.log(age);
     │
     ↓
let age = 30;
     ↓
TDZ ENDS
     ↓
age can be used ✅
2. TDZ With const

Same rule applies to const.

console.log(name);


const name = "John";

Result:

ReferenceError

After initialization:

const name = "John";


console.log(name);

Output:

John

So:

let   → TDZ
const → TDZ
3. TDZ Starts From Beginning of Scope 🔥

Consider:

{
    console.log(age);


    let age = 30;
}

The block starts here:

{
│
│ ← TDZ for age
│
console.log(age); ❌
│
let age = 30;
│
│ ← age available ✅
}

So TDZ is related to the variable's scope, not simply the entire JavaScript file.

4. var Does Not Behave This Way 🔥

Compare:

var
console.log(age);


var age = 30;

Output:

undefined
let
console.log(age);


let age = 30;

Output:

ReferenceError
const
console.log(age);


const age = 30;

Output:

ReferenceError

This is the main interview distinction.

var
 ↓
Hoisted + initialized with undefined




let / const
 ↓
Hoisted
 ↓
TDZ
 ↓
Cannot access before initialization
5. Why TDZ Exists

TDZ helps prevent using variables before they have been initialized.

For example:

console.log(total);


let total = 100;

Instead of silently giving:

undefined

JavaScript throws an error.

This makes accidental early access easier to detect.

Hoisting vs TDZ 🔥🔥

These concepts are closely connected.

Hoisting
Declarations are processed before
normal code execution.
TDZ
let / const cannot be accessed
before initialization.

So don't say:

"let and const are not hoisted." ❌

Better interview answer:

"let and const are hoisted, but they remain in the Temporal Dead Zone until initialization."

Interview Questions 🔥
What is TDZ?

TDZ stands for Temporal Dead Zone. It is the period where a let or const variable cannot be accessed before initialization.

Which variables have TDZ?
let
const
What happens if you access them during TDZ?
ReferenceError
Does var have the same TDZ behavior?

No.

var before assignment
→ undefined
Are let and const hoisted?

Yes, but they cannot be accessed before initialization because of TDZ.

🧠 TDZ Cheat Sheet
let / const


Start of Scope
      ↓
┌─────────────┐
│     TDZ     │
│             │
│ Access ❌   │
└─────────────┘
      ↓
Declaration / Initialization
      ↓
Access ✅
🔥 Final Memory
var


Before assignment
→ undefined




let / const


Before initialization
→ ReferenceError
→ TDZ
TDZ
 ↓
Starts when scope begins
 ↓
Ends when variable is initialized

# >>

8 — CLOSURES 🔥🔥🔥

A closure happens when an inner function remembers and can access variables from its outer function even after the outer function has finished executing.

Simple memory:

CLOSURE
   ↓
Inner function
   ↓
REMEMBERS outer variables
1. Basic Example
function outer() {
    const name = "John";


    function inner() {
        console.log(name);
    }


    return inner;
}


const greet = outer();


greet();

Output:

John

The interesting part is:

const greet = outer();

outer() has already finished.

But later:

greet();

can still access:

name

Why?

Because inner() remembers the lexical environment where it was created.

That is a closure.

2. Understand the Flow 🔥

Start:

const greet = outer();

outer() runs:

outer()
   ↓
name = "John"
   ↓
inner() created
   ↓
inner returned
   ↓
outer() finishes

Normally you might think:

outer finished
     ↓
name is gone

But inner() still needs name.

So:

inner()
   ↓
remembers
   ↓
name = "John"

Then:

greet();

prints:

John

🔥 That's the core closure concept.

3. Closure With Changing Data

Closures can also remember changing values.

function createCounter() {
    let count = 0;


    return function () {
        count++;
        return count;
    };
}


const counter = createCounter();

Now:

counter(); // 1
counter(); // 2
counter(); // 3

Think:

createCounter()
      ↓
count = 0
      ↓
returns function
      ↓
function remembers count

Each call updates the same remembered count:

count = 0


counter()
   ↓
count = 1


counter()
   ↓
count = 2


counter()
   ↓
count = 3
4. Closures Come From Lexical Scope 🔥

Earlier we learned:

Lexical Scope
     ↓
Inner function can access
outer variables

Closure builds directly on that.

LEXICAL SCOPE


Inner function can access outer variable


             +


Outer function finishes


             +


Inner function still remembers it


             ↓


          CLOSURE

So lexical scope and closures are strongly connected.

5. Each Closure Can Have Its Own Data 🔥
function createCounter() {
    let count = 0;


    return function () {
        count++;
        return count;
    };
}


const counter1 = createCounter();
const counter2 = createCounter();

Now:

counter1(); // 1
counter1(); // 2


counter2(); // 1

Why isn't counter2() returning 3?

Because each call to createCounter() creates its own environment.

Conceptually:

counter1
   ↓
count = 2




counter2
   ↓
count = 1

They are separate closures.

6. Why Closures Are Useful

At concept level, remember these common purposes:

Closures
   ↓
Preserve data between function calls
Keep data private
Create function factories
Callbacks/event handlers

You don't need to study advanced closure patterns yet.

Closure vs Scope 🔥

Don't confuse them.

Scope

Determines:

Where can I access this variable?
Closure

Means:

A function remembers variables
from its lexical outer scope
even when used later.
Interview Questions 🔥
What is a closure?

A closure is when a function remembers and can access variables from its lexical outer scope even after the outer function has finished executing.

Are closures related to lexical scope?

Yes. Closures work because functions retain access to their lexical environment.

What can closures be used for?

Common uses include preserving state, data privacy, callbacks, and function factories.

Does each closure have its own state?

It can. Separate calls to an outer function can create separate lexical environments.

🧠 Closure Cheat Sheet
function outer() {


    const value = 10;


    return function inner() {
        console.log(value);
    };
}


const fn = outer();


fn();

Conceptually:

outer()
   ↓
value = 10
   ↓
inner created
   ↓
inner returned
   ↓
outer finishes




Later...


inner()
   ↓
still remembers
   ↓
value = 10


🔥 CLOSURE
Final Memory
CLOSURE
   ↓
Function remembers
its lexical outer variables
   ↓
even when called later

The most important sentence to remember for an interview:

A closure is a function that retains access to variables from its lexical scope even after the outer function has finished executing.

A classic interview question is:

for (var i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 1000);
}

What is the output?

3
3
3

But with let:

for (let i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 1000);
}

Output:

0
1
2
Why does var print 3 3 3? 🔥

var is function-scoped, not block-scoped.

Conceptually, all callbacks refer to the same i:

One shared i


i = 0
 ↓
i = 1
 ↓
i = 2
 ↓
i = 3 → loop stops


Later callbacks execute:


callback → i → 3
callback → i → 3
callback → i → 3

Therefore:

3
3
3

The callback doesn't store a frozen copy of i's value. It closes over the variable.

Why does let print 0 1 2? 🔥🔥

let is block-scoped, and in a for loop JavaScript creates a separate binding of i for each iteration.

Conceptually:

Iteration 1
i = 0
callback remembers → 0


Iteration 2
i = 1
callback remembers → 1


Iteration 3
i = 2
callback remembers → 2

Later:

callback 1 → 0
callback 2 → 1
callback 3 → 2

Therefore:

0
1
2
🧠 Interview Answer

If they ask:

Why does var print 3,3,3 but let prints 0,1,2?

Say:

var is function-scoped, so all callbacks share the same i. By the time they execute, the loop has finished and i is 3. With let, the for loop creates a separate binding for each iteration, so each callback remembers its own value.

This question combines three concepts:

var vs let
    +
Block Scope
    +
Closures

🔥 Definitely keep this one for interviews.

Now Closures ✅ complete, including this important interview case.


# >>>>>>>>


9 — this 🔥🔥🔥

this is a special JavaScript keyword that refers to a value determined by how a function is called.

Simple memory:

this
 ↓
Usually depends on
HOW the function is called

Don't think:

this = where function was written ❌

That idea belongs more to lexical scope.

1. this Inside an Object Method 🔥
const user = {
    name: "John",


    greet: function () {
        console.log(this.name);
    }
};


user.greet();

Output:

John

Here:

user.greet()
 ↑

greet() is called through user.

So inside greet():

this === user

Therefore:

this.name

means:

user.name

which is:

John

Think:

user.greet()
  ↓
this → user
  ↓
this.name
  ↓
"John"
2. this Depends on How the Function Is Called 🔥🔥

Consider:

function showName() {
    console.log(this.name);
}


const user = {
    name: "John",
    showName: showName
};


user.showName();

Inside the call:

user.showName();

this refers to user.

The important rule:

For normal functions, this is determined mainly by the call site.

3. Losing this 🔥🔥🔥

Very common interview concept.

const user = {
    name: "John",


    greet: function () {
        console.log(this.name);
    }
};


const greet = user.greet;


greet();

Don't assume:

this → user ❌

The function is now called as:

greet();

not:

user.greet();

So it no longer gets user as this.

Think:

user.greet()
     ↓
this → user ✅




const greet = user.greet;


greet()
  ↓
No user object at call site
  ↓
this is NOT user

The exact standalone value can differ by environment/mode, so the important interview point is simply:

The method lost its object context.

4. Arrow Functions and this 🔥🔥🔥

Arrow functions behave differently.

They do not create their own this.

Instead, they use this from their surrounding lexical environment.

const user = {
    name: "John",


    greet: function () {


        const showName = () => {
            console.log(this.name);
        };


        showName();
    }
};


user.greet();

Output:

John

Why?

user.greet()
    ↓
normal function
    ↓
this → user


        ↓


arrow function
    ↓
uses surrounding this
    ↓
this → user

🔥 Remember:

Normal Function
→ has call-based this




Arrow Function
→ no own this
→ uses surrounding this
5. Arrow Function as Object Method ⚠️

This is an important trap.

const user = {
    name: "John",


    greet: () => {
        console.log(this.name);
    }
};


user.greet();

Don't expect:

John ❌

The arrow function doesn't get user as its own this.

So for a method that needs the object through this, a normal method/function is usually appropriate:

const user = {
    name: "John",


    greet() {
        console.log(this.name);
    }
};

Now:

user.greet();

Output:

John
6. this in Constructor Functions

When a normal function is called using new:

function User(name) {
    this.name = name;
}


const user = new User("John");

this refers to the newly created object.

Conceptually:

new User("John")
      ↓
new object created
      ↓
this → new object
      ↓
this.name = "John"

We'll see a similar idea again when we reach Classes.

7. The Main this Rules 🔥

For your current interview level, remember these:

1. Object method


user.greet()


this → user
2. Detached method


const greet = user.greet;
greet();


this → no longer user
3. Arrow function


() => {}


No own this
Uses surrounding this
4. Constructor with new


new User()


this → newly created object

That's enough before call(), apply() and bind().

Interview Questions 🔥
What is this in JavaScript?

this is a special value whose value, for normal functions, generally depends on how the function is called.

What is this inside an object method?

For:

user.greet();

inside a normal greet method:

this → user
Do arrow functions have their own this?

No.

They use this from their surrounding lexical environment.

Why can a method lose this?
const greet = user.greet;


greet();

Because it is no longer being called through user.

Should an arrow function be used as an object method when you need this to refer to that object?

Usually no, because arrow functions don't create their own this.

🧠 this Cheat Sheet
NORMAL METHOD


user.greet()
     ↓
this → user
DETACHED METHOD


const greet = user.greet;


greet()
 ↓
this is no longer user
ARROW FUNCTION


() => {}
   ↓
No own this
   ↓
Uses surrounding this
CONSTRUCTOR


new User()
   ↓
this → new object
🔥 Final Memory
Normal function
      ↓
this mainly depends on
HOW IT IS CALLED




Arrow function
      ↓
NO OWN this
      ↓
uses surrounding this


# >>>

10 — call(), apply(), bind() 🔥🔥🔥

All three are used to control what this refers to when calling a normal function.

Simple memory:

call()
apply()
bind()
   ↓
Control `this`

The biggest difference:

call()   → calls NOW
apply()  → calls NOW
bind()   → returns a NEW function to call LATER
1. Why Do We Need Them?

Suppose:

const user = {
    name: "John"
};


function greet() {
    console.log(this.name);
}

greet() is not a method of user.

But we want:

this → user

We can use:

greet.call(user);

Output:

John

Think:

greet.call(user)
           ↓
      this = user
2. call() 🔥🔥

call() calls a function immediately and lets us specify this.

const user = {
    name: "John"
};


function greet() {
    console.log(this.name);
}


greet.call(user);

Output:

John
Passing Arguments
const user = {
    name: "John"
};


function greet(city, country) {
    console.log(
        this.name,
        city,
        country
    );
}


greet.call(user, "Mumbai", "India");

Output:

John Mumbai India

Syntax:

functionName.call(
    thisValue,
    arg1,
    arg2
);

🔥 Remember:

call()
  ↓
Arguments passed separately
3. apply() 🔥

apply() is almost the same as call().

The difference is how arguments are passed.

const user = {
    name: "John"
};


function greet(city, country) {
    console.log(
        this.name,
        city,
        country
    );
}


greet.apply(user, ["Mumbai", "India"]);

Output:

John Mumbai India

Notice:

["Mumbai", "India"]

The arguments are passed as an array / array-like collection.

4. call() vs apply() 🔥🔥
call()
greet.call(
    user,
    "Mumbai",
    "India"
);

Arguments:

"Mumbai", "India"
apply()
greet.apply(
    user,
    ["Mumbai", "India"]
);

Arguments:

["Mumbai", "India"]

Easy memory:

CALL
 ↓
Comma-separated arguments




APPLY
 ↓
Arguments as array

Both execute the function immediately.

5. bind() 🔥🔥🔥

bind() is different.

It doesn't immediately execute the function.

Instead, it returns a new function with this fixed.

const user = {
    name: "John"
};


function greet() {
    console.log(this.name);
}


const boundGreet = greet.bind(user);

Nothing has executed yet.

Now:

boundGreet();

Output:

John

Think:

greet.bind(user)
       ↓
Create new function
       ↓
this fixed to user
       ↓
boundGreet
       ↓
call later
6. bind() Solves Lost this 🔥

Earlier we saw:

const user = {
    name: "John",


    greet() {
        console.log(this.name);
    }
};


const greet = user.greet;

Calling:

greet();

loses the user context.

We can fix it:

const greet = user.greet.bind(user);


greet();

Output:

John

Because:

bind(user)
    ↓
this permanently bound
to user for that new function
7. Most Important Difference 🔥🔥🔥

This is the part to remember for interviews.

              call()      apply()      bind()


this control    ✅           ✅           ✅


Executes now    ✅           ✅           ❌


Returns
bound function  ❌           ❌           ✅


Arguments       separate     array       separate
Interview Questions 🔥
What does call() do?

It calls a function immediately with a specified this value.

greet.call(user, "Mumbai");
What does apply() do?

Same basic purpose as call(), but arguments are supplied as an array/array-like object.

greet.apply(user, ["Mumbai"]);
What does bind() do?

It returns a new function with this bound to the provided value.

const fn = greet.bind(user);
Does bind() execute immediately?

No.

call() vs apply()?
call()
→ arguments separately


apply()
→ arguments in array
call() vs bind()?
call()
→ execute now


bind()
→ create function for later
🧠 Final Cheat Sheet
// CALL
greet.call(user, "Mumbai", "India");
// APPLY
greet.apply(user, ["Mumbai", "India"]);
// BIND
const fn = greet.bind(user);


fn();
🔥 Final Memory
call()
→ CALL NOW
→ arguments separately




apply()
→ CALL NOW
→ arguments as array




bind()
→ CALL LATER
→ returns new function

And all three revolve around:

        this
         ↓
call / apply / bind
         ↓
Control function context



# >>>

11 — PROTOTYPE 🔥🔥🔥

A prototype is an object that another JavaScript object can use to access shared properties and methods.

Simple memory:

Object
  ↓
Doesn't have property/method?
  ↓
Check its prototype

This is the core idea. Don't make prototypes more complicated than this yet.

1. Basic Idea

Consider:

const user = {
    name: "John"
};

You can do:

console.log(user.toString());

But we never created:

toString()

inside user.

So where did it come from?

Conceptually:

user
 │
 ├── name: "John"
 │
 └── prototype
        ↓
   Object.prototype
        ↓
     toString()

JavaScript gets toString() through the object's prototype.

2. Objects Can Inherit From Other Objects 🔥

Example:

const person = {
    greet() {
        console.log("Hello");
    }
};


const user = Object.create(person);


user.name = "John";

user itself has:

name → "John"

But greet() comes from person.

So:

user.greet();

Output:

Hello

Conceptually:

user
 │
 ├── name
 │
 └── prototype
        ↓
      person
        ↓
      greet()

This is prototypal inheritance.

3. Property Lookup 🔥🔥

Suppose:

user.greet();

JavaScript looks for greet:

user
 ↓
Does user have greet?


NO
 ↓
Check prototype
 ↓
person
 ↓
greet found ✅

This lookup behavior is one of the most important things to understand about prototypes.

4. Constructor Functions and prototype 🔥

Consider:

function User(name) {
    this.name = name;
}

We can put a shared method on:

User.prototype

Example:

User.prototype.greet = function () {
    console.log("Hello " + this.name);
};

Now:

const user1 = new User("John");
const user2 = new User("David");


user1.greet();
user2.greet();

Output:

Hello John
Hello David

Conceptually:

user1 ──┐
        │
        ├──→ User.prototype
        │        ↓
user2 ──┘      greet()

Both objects can use the same shared greet() method.

5. Why Put Methods on the Prototype? 🔥

Compare this:

function User(name) {
    this.name = name;


    this.greet = function () {
        console.log("Hello");
    };
}

Every new object gets its own greet function.

Conceptually:

user1 → greet()
user2 → greet()
user3 → greet()

With a prototype:

function User(name) {
    this.name = name;
}


User.prototype.greet = function () {
    console.log("Hello");
};

The method can be shared:

user1 ──┐
user2 ──┼──→ User.prototype → greet()
user3 ──┘

That's the important idea.

6. __proto__ vs prototype 🔥

This often confuses people.

For now, remember:

prototype
→ property found on constructor functions/classes
→ used for methods shared by instances

Whereas an object's internal prototype is the object it inherits from.

You may see:

user.__proto__

in interview examples or browser consoles, but __proto__ is a legacy accessor. In normal modern code, don't build your application around it.

The important relationship is:

function User() {}


const user = new User();

Conceptually:

user
 ↓ prototype link
User.prototype
7. Prototype Is About Sharing + Inheritance

The main purpose to understand:

PROTOTYPE
    │
    ├── Shared properties/methods
    │
    └── Inheritance

Example:

User.prototype.greet = function () {
    return "Hello";
};

All User instances can access greet() through their prototype relationship.

Interview Questions 🔥
What is a prototype?

A prototype is an object that another object can inherit/access properties and methods from.

What happens if JavaScript cannot find a property directly on an object?

It checks the object's prototype.

What is prototypal inheritance?

It is JavaScript's mechanism where objects can inherit properties and methods through prototypes.

Why put methods on Constructor.prototype?

So instances can share the method instead of creating a separate method function for every instance.

Does every object contain its prototype methods directly?

No. It can access them through its prototype relationship.

🧠 Prototype Cheat Sheet
OBJECT


user
 ↓
Own properties
 ↓
name

If something isn't found:

user
 ↓
Not found?
 ↓
Prototype
 ↓
Search there

Constructor example:

function User(name) {
    this.name = name;
}


User.prototype.greet = function () {
    return "Hello " + this.name;
};


const user = new User("John");

Conceptually:

user
 │
 ├── name = "John"
 │
 └── prototype link
          ↓
     User.prototype
          ↓
        greet()
🔥 Final Memory
Prototype
    ↓
Object used for
shared/inherited behavior




Property not found on object
    ↓
Check prototype




Constructor.prototype
    ↓
Shared methods for instances


# >>>

12 — PROTOTYPE CHAIN 🔥🔥🔥

A prototype chain is the chain JavaScript follows when searching for a property or method that is not directly available on an object.

Simple memory:

Object
  ↓
Not found?
  ↓
Prototype
  ↓
Not found?
  ↓
Prototype's prototype
  ↓
Keep searching
  ↓
null → STOP
1. Basic Example
const user = {
    name: "John"
};


console.log(user.toString());

user doesn't directly contain:

toString()

So JavaScript searches:

user
 ↓
toString() here?


NO
 ↓
Object.prototype
 ↓
toString() found ✅

That's the prototype chain.

2. Multiple Levels 🔥

Consider:

const person = {
    greet() {
        console.log("Hello");
    }
};


const employee = Object.create(person);


employee.name = "John";

Now:

employee.greet();

JavaScript searches:

employee
   ↓
greet() ?


Not found
   ↓
person
   ↓
greet() found ✅

But suppose we use:

employee.toString();

The search continues further:

employee
    ↓
person
    ↓
Object.prototype
    ↓
toString() ✅

That's a chain:

employee
    ↓
person
    ↓
Object.prototype
    ↓
null
3. JavaScript Stops at the First Match 🔥

Consider:

const person = {
    name: "Parent"
};


const user = Object.create(person);


user.name = "John";


console.log(user.name);

Output:

John

JavaScript searches:

user
 ↓
name found ✅


STOP

It doesn't continue looking for name on person.

🔥 Remember:

Nearest property wins.
4. What If Nothing Is Found?
const user = {
    name: "John"
};


console.log(user.salary);

JavaScript searches:

user
 ↓
salary? ❌
 ↓
Object.prototype
 ↓
salary? ❌
 ↓
null
 ↓
STOP

Result:

undefined

So:

Property not found anywhere
          ↓
       undefined
5. Prototype vs Prototype Chain 🔥

This is the important interview distinction.

Prototype

An object another object can inherit properties/methods from.

user
 ↓
prototype
Prototype Chain

The complete sequence JavaScript searches.

user
 ↓
User.prototype
 ↓
Object.prototype
 ↓
null

Easy memory:

Prototype
→ ONE prototype relationship




Prototype Chain
→ FULL lookup chain
6. Constructor Example

From the previous topic:

function User(name) {
    this.name = name;
}


User.prototype.greet = function () {
    return "Hello";
};


const user = new User("John");

Conceptually:

user
 │
 ├── name
 │
 ↓
User.prototype
 │
 ├── greet()
 │
 ↓
Object.prototype
 │
 ├── toString()
 │
 └── other common methods
 │
 ↓
null

This complete path is the prototype chain.

Interview Questions 🔥
What is the prototype chain?

The prototype chain is the sequence JavaScript follows to search for properties and methods through an object's prototypes.

What happens when JavaScript finds the property?

It uses the first matching property and stops searching.

Where does the prototype chain end?
null
What happens if the property isn't found anywhere?

Accessing it returns:

undefined
Prototype vs prototype chain?
Prototype
→ object used for inheritance




Prototype Chain
→ chain of prototypes JavaScript searches
🧠 Prototype Chain Cheat Sheet
PROPERTY REQUESTED
       ↓
Current Object
       ↓
Not found?
       ↓
Prototype
       ↓
Not found?
       ↓
Next Prototype
       ↓
...
       ↓
Object.prototype
       ↓
null
       ↓
STOP
🔥 Final Memory
PROTOTYPE CHAIN
      ↓
JavaScript property lookup path




Example:


user
 ↓
User.prototype
 ↓
Object.prototype
 ↓
null
Found?
→ use nearest match




Not found anywhere?
→ undefined


# >>

13 — CLASSES 🔥🔥🔥

A class is a cleaner syntax for creating multiple objects with the same structure and behavior.

Simple memory:

CLASS
  ↓
Blueprint
  ↓
Create Objects

Example:

class User {
    constructor(name) {
        this.name = name;
    }


    greet() {
        console.log("Hello " + this.name);
    }
}

Create objects:

const user1 = new User("John");
const user2 = new User("David");


user1.greet();
user2.greet();

Output:

Hello John
Hello David
1. class Syntax

Basic structure:

class User {


    constructor(name) {
        this.name = name;
    }


    greet() {
        console.log(this.name);
    }
}

Think:

class User
   │
   ├── constructor()
   │
   └── methods
2. constructor() 🔥🔥

The constructor() runs automatically when we create an object using new.

class User {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}

Now:

const user = new User("John", 30);

Conceptually:

new User("John", 30)
        ↓
constructor runs
        ↓
this.name = "John"
this.age  = 30
        ↓
object created

Result:

user


{
    name: "John",
    age: 30
}
3. this Inside Classes 🔥

Remember our previous this topic.

class User {
    constructor(name) {
        this.name = name;
    }
}

When:

const user = new User("John");

this refers to the new object being created.

new User()
    ↓
this
    ↓
new object

So:

this.name = name;

stores name on that object.

4. Class Methods 🔥🔥

Methods are written directly inside the class.

class User {
    constructor(name) {
        this.name = name;
    }


    greet() {
        console.log("Hello " + this.name);
    }
}

Then:

const user = new User("John");


user.greet();

Output:

Hello John

Notice we don't write:

greet: function() {} // ❌

Inside a class, simply:

greet() {
    // code
}
5. Creating Multiple Objects

One class can create many objects.

class User {
    constructor(name) {
        this.name = name;
    }


    greet() {
        console.log(this.name);
    }
}


const user1 = new User("John");
const user2 = new User("David");
const user3 = new User("Sam");

Each object has its own data:

user1 → John
user2 → David
user3 → Sam

but they share the class methods through the prototype.

6. Classes and Prototypes 🔥🔥🔥

This is the most important internal concept.

Classes do not replace JavaScript's prototype system.

Classes are built on top of prototypes.

When we write:

class User {
    greet() {
        console.log("Hello");
    }
}

Conceptually:

User
 ↓
User.prototype
 ↓
greet()

Instances:

const user1 = new User();
const user2 = new User();

use:

user1 ──┐
        ├──→ User.prototype → greet()
user2 ──┘

So the concepts you just learned connect:

Class
  ↓
Prototype
  ↓
Prototype Chain

🔥 Interview point:

JavaScript classes are primarily syntactic sugar over JavaScript's prototype-based inheritance system.

7. Inheritance — extends 🔥🔥

A class can inherit from another class using:

extends

Example:

class Person {
    greet() {
        console.log("Hello");
    }
}


class Employee extends Person {
    work() {
        console.log("Working");
    }
}

Now:

const employee = new Employee();


employee.greet();
employee.work();

Both work.

Think:

Person
   ↓
 greet()


   ↑ inherits


Employee
   ↓
 work()
8. super() 🔥

When a child class has its own constructor, super() calls the parent constructor.

class Person {
    constructor(name) {
        this.name = name;
    }
}


class Employee extends Person {
    constructor(name, role) {
        super(name);


        this.role = role;
    }
}

Now:

const employee =
    new Employee("John", "Developer");

Flow:

Employee constructor
       ↓
super(name)
       ↓
Person constructor
       ↓
this.name = "John"
       ↓
Back to Employee
       ↓
this.role = "Developer"

Result:

name → John
role → Developer

🔥 In a derived class constructor, you must call super() before using this.

Interview Questions 🔥
What is a class?

A class is a syntax used to create objects with shared structure and behavior.

What does constructor() do?

It runs when an object is created with new and initializes the object.

What does new do here?
const user = new User("John");

It creates a new instance and runs the class constructor.

What does extends do?

It allows one class to inherit from another class.

What does super() do?

It calls the parent class constructor.

Are JavaScript classes completely separate from prototypes?

No.

JavaScript classes are built on top of the prototype system.

🧠 Classes Cheat Sheet
class User {


    constructor(name) {
        this.name = name;
    }


    greet() {
        console.log(this.name);
    }
}

Create:

const user = new User("John");

Inheritance:

class Employee extends User {
}

Parent constructor:

super(name);
🔥 Final Memory
class
→ blueprint-style syntax




constructor()
→ initialize object




new
→ create instance




this
→ current instance




extends
→ inherit




super()
→ parent constructor




Class methods
→ shared through prototype

# >>>>>>

#### 8. Async JavaScript




8.1 — Synchronous vs Asynchronous 🔥

First understand these two words.

1. Synchronous

Synchronous code runs one task at a time, in sequence.

console.log("A");
console.log("B");
console.log("C");

Output:

A
B
C

Execution:

A
↓
finish


B
↓
finish


C
↓
finish

Simple memory:

Finish the current synchronous work before moving to the next synchronous work.

2. What if One Task Is Slow?

Imagine:

console.log("Start");


heavyTask();


console.log("End");

Suppose heavyTask() takes 5 seconds of synchronous CPU work.

Execution:

Start
  ↓
heavyTask()
  ↓
████████████████
   5 seconds
████████████████
  ↓
End

console.log("End") cannot execute until heavyTask() finishes.

This is called:

Blocking 🔥
Long synchronous work
        ↓
JavaScript is busy
        ↓
Following JS waits

This distinction matters: synchronous does not automatically mean slow. Most synchronous code finishes extremely quickly.

The problem is long-running synchronous work.

3. Why Do We Need Async?

Many operations involve waiting.

For example:

API/network request
Timer
User interaction
Some file/I/O operations

Suppose you request data from a server and it takes 3 seconds.

We don't want the mental model to be:

Request data
     ↓
JavaScript does nothing useful
     ↓
wait...
wait...
wait...
     ↓
response arrives
     ↓
continue

Instead, asynchronous mechanisms allow the waiting operation to be handled without blocking all subsequent JavaScript.

4. Asynchronous JavaScript 🔥🔥

Example:

console.log("Start");


setTimeout(() => {
    console.log("Timer finished");
}, 2000);


console.log("End");

Output:

Start
End
Timer finished

Notice that JavaScript did not wait two seconds before executing:

console.log("End");
5. What Happened?

First:

console.log("Start");

prints:

Start

Then JavaScript encounters:

setTimeout(..., 2000);

The timer is arranged by the runtime environment.

JavaScript can continue executing:

console.log("End");

So conceptually:

JavaScript


console.log("Start")
        ↓
      Start


setTimeout(...)
        ↓
Timer gets handled by runtime


        ↓


JavaScript continues


        ↓


console.log("End")
        ↓
       End




...later...


Timer callback
      ↓
Timer finished

For now, that's enough.

Soon we'll replace the vague phrase "handled by runtime" with the actual pieces:

Call Stack
Web APIs
Event Loop
Task Queue
Microtask Queue
6. Async Does NOT Mean JavaScript Runs Everything in Parallel 🔥

This is an important interview distinction.

Don't say:

"Async means JavaScript executes multiple JavaScript functions at the same time."

That's misleading.

For the browser main thread, JavaScript execution is generally:

One piece of JS
      ↓
then another
      ↓
then another

The environment around JavaScript helps handle asynchronous operations and schedules JavaScript work to run later.

That's why JavaScript can provide non-blocking asynchronous behavior without simply executing all JavaScript simultaneously.

7. Classic Interview Example 🔥🔥🔥

What is the output?

console.log("A");


setTimeout(() => {
    console.log("B");
}, 0);


console.log("C");

You might think:

A
B
C

because:

setTimeout(..., 0)

has zero delay.

But the output is:

A
C
B

Why?

Because 0 does not mean:

Execute the callback immediately

It means roughly:

The timer has no requested delay,
but its callback still has to be
scheduled to run later.

The currently executing synchronous JavaScript finishes first.

So:

A
↓
schedule timer callback
↓
C
↓
current synchronous work finishes
↓
B can run later

The exact reason will become obvious once we learn the Event Loop and Task Queue.

Don't memorize it blindly yet.

8. Common Async Operations

You'll encounter asynchronous behavior with:

Timers
setTimeout(...)
Promises
promise.then(...)
Network requests
fetch(...)
Async/Await
async function loadData() {
    const result = await something();
}

We'll learn all of these separately.

9. Synchronous vs Asynchronous
SYNCHRONOUS


Task A
  ↓
finish
  ↓
Task B
  ↓
finish
  ↓
Task C

Compared with:

ASYNCHRONOUS


Start async operation
        ↓
JavaScript can continue
        ↓
other synchronous work
        ↓
async result becomes ready
        ↓
handle result later
10. Blocking vs Non-Blocking 🔥

These terms often appear in interviews.

Blocking
Current operation
       ↓
takes time
       ↓
following JS cannot execute
Non-blocking asynchronous behavior
Start operation
       ↓
waiting handled asynchronously
       ↓
JavaScript continues
       ↓
result handled later
Interview Questions 🔥
What is synchronous JavaScript?

Synchronous code executes sequentially, with the current synchronous operation completing before subsequent synchronous work proceeds.

What is asynchronous JavaScript?

Asynchronous programming allows certain operations to be started without blocking JavaScript while waiting for their completion, with the result handled later.

What is blocking code?

Code that prevents subsequent JavaScript from executing until the current operation finishes.

Does asynchronous mean JavaScript executes everything simultaneously?

No. Async behavior relies on the JavaScript runtime and scheduling mechanisms to handle work and execute callbacks/jobs at the appropriate time.

Does setTimeout(fn, 0) run immediately?

No. The callback is scheduled for later and cannot interrupt the currently executing synchronous JavaScript.

🧠 Quick Revision
SYNC


Do work
   ↓
finish
   ↓
next work
ASYNC


Start operation
      ↓
don't block while waiting
      ↓
continue other work
      ↓
handle result later

And remember:

Synchronous ≠ automatically bad


Long synchronous work
        ↓
can block JavaScript
setTimeout(fn, 0)
        ↓
NOT immediate
        ↓
callback runs later
One sentence to remember

Synchronous code finishes the current work before moving forward; asynchronous mechanisms allow waiting work to be handled later without unnecessarily blocking subsequent JavaScript.


# >>>>>>


8.2 — How JavaScript Handles Async Work 🔥🔥🔥

Before separately learning Call Stack, Web APIs, Event Loop, Task Queue, and Microtask Queue, understand how they fit together.

This is the big picture.

1. JavaScript Executes One Thing at a Time

On the normal JavaScript main thread:

JavaScript
    ↓
Executes one piece of JS
    ↓
Then the next
    ↓
Then the next

This is why JavaScript is commonly described as single-threaded in this context.

But then we can write:

console.log("Start");


setTimeout(() => {
    console.log("Timer");
}, 2000);


console.log("End");

and get:

Start
End
Timer

So the obvious question is:

If JavaScript executes one thing at a time, who is handling that timer?

That's where the runtime environment comes in.

2. JavaScript Engine ≠ Entire Runtime 🔥

JavaScript doesn't work completely alone.

In a browser, you can think of the environment like this:

┌─────────────────────────────────────────────┐
│                  BROWSER                    │
│                                             │
│   JavaScript Engine                         │
│   ┌─────────────────┐                       │
│   │   Call Stack    │                       │
│   └─────────────────┘                       │
│                                             │
│   Browser APIs / Web APIs                   │
│   ┌─────────────────┐                       │
│   │ Timers          │                       │
│   │ Network         │                       │
│   │ DOM events      │                       │
│   └─────────────────┘                       │
│                                             │
│   Queues                                    │
│   ┌─────────────────┐                       │
│   │ Microtask Queue │                       │
│   │ Task Queue      │                       │
│   └─────────────────┘                       │
│                                             │
│              Event Loop                     │
└─────────────────────────────────────────────┘

These pieces cooperate to give JavaScript asynchronous behavior.

3. The Five Pieces You Need to Know

Don't learn every detail yet. Just understand each one's job.

① Call Stack

Where JavaScript functions execute.

CALL STACK
    ↓
"What's running right now?"
② Web APIs

Browser-provided features that can handle certain operations outside the JavaScript call stack.

Examples:

setTimeout()
DOM events
network-related browser functionality

Think:

WEB APIs
   ↓
"Runtime handles the waiting/work"
③ Task Queue

Callbacks from certain async sources can wait here until JavaScript can execute them.

A timer callback is a classic example.

TASK QUEUE
    ↓
"Callbacks waiting for their turn"

You may also hear:

Callback Queue
Macrotask Queue
Task Queue

For our current learning, we'll primarily call it the Task Queue.

④ Microtask Queue 🔥

Promises use an important higher-priority scheduling mechanism.

For example:

Promise.resolve().then(() => {
    console.log("Promise");
});

The .then() reaction is scheduled as a microtask.

MICROTASK QUEUE
       ↓
Promise-related jobs
       ↓
High priority before the next task

We'll study this properly later.

⑤ Event Loop 🔥🔥🔥

The Event Loop coordinates when queued work gets an opportunity to execute.

Simple mental model:

Is current JavaScript finished?
          ↓
        YES
          ↓
Run pending microtasks
          ↓
Then the runtime can move
to the next task when appropriate

Don't memorize more than that yet.

4. Put Them Together 🔥🔥🔥

Consider:

console.log("A");


setTimeout(() => {
    console.log("B");
}, 1000);


console.log("C");

Let's follow the big picture.

Step 1 — "A"
console.log("A");

JavaScript executes it.

OUTPUT


A
Step 2 — Timer

JavaScript reaches:

setTimeout(() => {
    console.log("B");
}, 1000);

The browser's timer mechanism handles the waiting.

Conceptually:

JavaScript
    ↓
setTimeout(...)
    ↓
Browser timer
    ↓
wait ~1000ms

JavaScript doesn't sit on the call stack doing nothing for that second.

Step 3 — Continue Synchronous Code

JavaScript continues:

console.log("C");

Output:

A
C
Step 4 — Timer Becomes Ready

After the timer requirement is satisfied, its callback can be queued as a task.

Task Queue


┌────────────────────┐
│ () => log("B")     │
└────────────────────┘

Important:

The timer finishing does not mean its callback instantly interrupts JavaScript.

It must wait for its opportunity to execute.

Step 5 — Current JS Finishes

Once the current synchronous work is finished, the runtime can eventually execute the queued timer callback.

() => {
    console.log("B");
}

Final output:

A
C
B
5. Full Mental Picture
             JAVASCRIPT
                 │
                 ▼
          ┌────────────┐
          │ CALL STACK │
          └────────────┘
                 │
                 │ async operation
                 ▼
          ┌────────────┐
          │  WEB APIs  │
          └────────────┘
                 │
          operation ready
                 │
        ┌────────┴────────┐
        ▼                 ▼
┌────────────────┐  ┌────────────┐
│ Microtask Queue│  │ Task Queue │
└────────────────┘  └────────────┘
        │                 │
        └────────┬────────┘
                 │
            EVENT LOOP
                 │
                 ▼
          ┌────────────┐
          │ CALL STACK │
          └────────────┘

This diagram is the backbone of Async JavaScript.

6. Don't Make This Common Mistake 🔥

Don't think:

setTimeout()
     ↓
callback goes directly
to Call Stack ❌

A better model is:

setTimeout()
     ↓
Timer handled by runtime
     ↓
Timer becomes eligible
     ↓
Callback queued as a task
     ↓
Gets opportunity to execute later
7. What About Promises?

Promises take a slightly different scheduling path.

Promise.resolve().then(() => {
    console.log("Promise");
});

Conceptually:

Promise reaction ready
       ↓
Microtask Queue
       ↓
Runs before the next Task Queue task

That's why this:

setTimeout(() => {
    console.log("Timer");
}, 0);


Promise.resolve().then(() => {
    console.log("Promise");
});

normally gives:

Promise
Timer

Even though the timer delay is 0.

⚠️ Don't memorize this as a random rule.

When we reach Microtask Queue, you'll understand exactly why.

8. Browser vs Node.js

One useful distinction:

Web APIs is mainly browser terminology.

Node.js has its own runtime facilities for asynchronous work.

So don't say:

"JavaScript itself provides all Web APIs."

Better:

The JavaScript runtime/environment provides asynchronous capabilities around the JavaScript engine.

For now, we're building the browser mental model because it's the easiest way to understand the fundamentals.

Interview Questions 🔥
If JavaScript is single-threaded, how can it handle asynchronous operations?

JavaScript executes JavaScript code on its main thread one piece at a time, while the runtime environment provides mechanisms for handling asynchronous operations and scheduling their callbacks/jobs for later execution.

Does setTimeout() itself block the Call Stack for two seconds?

No.

The runtime handles the timer, allowing JavaScript to continue executing other code.

Does a timer callback execute immediately when its delay expires?

No.

It becomes eligible to be scheduled and must wait until JavaScript can execute it.

What coordinates queued asynchronous work?

The Event Loop is part of the runtime's scheduling mechanism.

Where do Promise callbacks go?

Promise reactions such as .then() are scheduled as microtasks.

🧠 Quick Revision
CALL STACK
→ Executes JavaScript


WEB APIs / RUNTIME
→ Handle async facilities/waiting


TASK QUEUE
→ Holds tasks such as ready timer callbacks


MICROTASK QUEUE
→ Holds microtasks such as Promise reactions


EVENT LOOP
→ Coordinates when queued work can execute
The flow to remember
JS executes
    ↓
Async operation starts
    ↓
Runtime handles waiting
    ↓
Result/callback becomes ready
    ↓
Relevant queue
    ↓
Scheduling rules
    ↓
JavaScript executes it



# >>>>>


8.3 — EVENT LOOP 🔥🔥🔥

The Event Loop is one of the most important concepts in Async JavaScript.

But the basic idea is actually simple:

The Event Loop helps decide when queued asynchronous JavaScript can execute.

1. First Remember — JavaScript Executes One Thing at a Time

On the normal JavaScript main thread:

JavaScript code
      ↓
Call Stack
      ↓
Execute current work
      ↓
Finish it
      ↓
Execute next work

JavaScript doesn't normally execute two JavaScript functions on the main thread at exactly the same time.

For example:

console.log("A");
console.log("B");
console.log("C");

Output:

A
B
C

Simple.

But async code introduces another problem.

2. The Async Problem

Consider:

console.log("A");


setTimeout(() => {
    console.log("B");
}, 1000);


console.log("C");

Output:

A
C
B

We already know why at a high level:

A
↓
timer starts
↓
C
↓
timer callback runs later
↓
B

But who decides when B is allowed to execute?

That's where the Event Loop becomes important.

3. Pieces Involved

For our browser mental model:

                JavaScript
                    │
                    ▼
              ┌───────────┐
              │Call Stack │
              └───────────┘
                    │
                    │ async operation
                    ▼
              ┌───────────┐
              │ Web APIs  │
              └───────────┘
                    │
                    │ ready
                    ▼
        ┌─────────────────────────┐
        │                         │
        ▼                         ▼
┌────────────────┐        ┌─────────────┐
│Microtask Queue │        │ Task Queue  │
└────────────────┘        └─────────────┘
        │                         │
        └────────────┬────────────┘
                     │
                     ▼
                EVENT LOOP
                     │
                     ▼
               ┌───────────┐
               │Call Stack │
               └───────────┘

You already saw this overall architecture in the previous topic.

Now we're focusing specifically on:

EVENT LOOP
4. What Does the Event Loop Do?

Think of the Event Loop as continuously coordinating:

Is the current JavaScript work finished?
              ↓
             YES
              ↓
Handle pending microtasks
              ↓
Then move to the next task
when appropriate

For now, the most important rule is:

Current synchronous JavaScript
        ↓
must finish first

Queued async callbacks don't simply interrupt currently running JavaScript.

5. Simple Timer Example
console.log("Start");


setTimeout(() => {
    console.log("Timer");
}, 0);


console.log("End");

What is the output?

Not:

Start
Timer
End

The output is:

Start
End
Timer

Let's understand exactly what happens.

6. Step-by-Step Execution 🔥🔥🔥
Step 1 — Start

JavaScript executes:

console.log("Start");

Output:

Start
Step 2 — setTimeout

JavaScript reaches:

setTimeout(() => {
    console.log("Timer");
}, 0);

The timer is handled by the browser/runtime.

Conceptually:

Call Stack
    ↓
setTimeout(...)
    ↓
Browser timer mechanism

JavaScript doesn't wait there.

Step 3 — Continue JavaScript

JavaScript continues:

console.log("End");

Output is now:

Start
End
Step 4 — Timer Callback Becomes Ready

After the timer requirement is satisfied, the callback becomes eligible to run and is queued as a task.

TASK QUEUE


┌──────────────────────────┐
│ () => console.log("Timer")│
└──────────────────────────┘

Important:

0ms delay
   ≠
execute immediately

The callback still has to wait for JavaScript to get an opportunity to execute it.

Step 5 — Current JavaScript Finishes

The current synchronous work is finished.

Conceptually:

CALL STACK
┌──────────────┐
│              │
└──────────────┘


EMPTY

Now queued work can get an opportunity to execute.

Step 6 — Timer Callback Executes

The timer callback can eventually execute:

() => {
    console.log("Timer");
}

Final output:

Start
End
Timer
7. Important Rule 🔥🔥🔥

A common beginner explanation is:

Event Loop checks:


Is Call Stack empty?
        ↓
      YES
        ↓
Take callback from queue
        ↓
Execute it

This is a useful starting mental model.

But because we also have Microtask Queue and Task Queue, the more accurate model is:

Current JavaScript finishes
          ↓
Process microtasks
          ↓
Next task gets its opportunity
          ↓
Process microtasks again
          ↓
Next task
          ↓
...

We'll understand this much better when we reach Microtask Queue.

8. Event Loop + Promise 🔥🔥🔥

Now consider:

console.log("A");


setTimeout(() => {
    console.log("B");
}, 0);


Promise.resolve().then(() => {
    console.log("C");
});


console.log("D");

Output:

A
D
C
B

Why?

Let's classify everything.

Synchronous
console.log("A");
console.log("D");
Promise reaction
() => {
    console.log("C");
}

goes to the:

Microtask Queue
Timer callback
() => {
    console.log("B");
}

becomes a:

Task

So conceptually:

Current synchronous code
        ↓
A
D
        ↓
current JS finishes
        ↓
Microtask Queue
        ↓
C
        ↓
Task Queue
        ↓
B

Therefore:

A
D
C
B

Your previous section already established that Promise reactions such as .then() are scheduled as microtasks and run before the next timer task.

9. Very Important Priority 🔥🔥🔥

At this stage, remember:

Current synchronous JavaScript
            ↓
       Microtasks
            ↓
       Next Task

Or shorter:

SYNC
 ↓
MICROTASKS
 ↓
NEXT TASK

For our upcoming examples:

Promise .then()
     ↓
Microtask


setTimeout callback
     ↓
Task

Therefore:

setTimeout(() => console.log("Timer"), 0);


Promise.resolve().then(() => console.log("Promise"));

normally outputs:

Promise
Timer

Not:

Timer
Promise
10. Does the Event Loop Execute JavaScript?

Be careful with the wording.

Don't imagine:

Event Loop
    ↓
executes your function itself

A better mental model:

Event Loop / runtime scheduling
            ↓
coordinates when queued work
gets an opportunity to execute
            ↓
JavaScript executes that work

So a good interview explanation is:

The Event Loop coordinates the execution of queued asynchronous work when the current JavaScript execution allows it.

11. setTimeout(0) Interview Trap 🔥🔥🔥
console.log(1);


setTimeout(() => {
    console.log(2);
}, 0);


console.log(3);

Output:

1
3
2

Why doesn't 2 come immediately?

Because:

0ms
 ↓
minimum requested timer delay satisfied
 ↓
callback becomes eligible
 ↓
queued for later execution

It does not mean:

0ms
 ↓
interrupt JavaScript immediately ❌

This matches the rule from your previous notes: a timer callback becoming ready does not mean it can instantly interrupt the currently executing JavaScript.

12. Long Synchronous Code Can Delay Async Code 🔥

Consider:

setTimeout(() => {
    console.log("Timer");
}, 1000);


// imagine expensive synchronous work here
for (let i = 0; i < 5_000_000_000; i++) {
    // heavy work
}

Suppose that synchronous work takes several seconds.

The timer callback cannot force its way into currently executing JavaScript just because 1 second has passed.

Conceptually:

Timer ready
    ↓
callback waiting
    ↓


Call Stack still busy
    ↓
WAIT


Call Stack becomes available
    ↓
callback eventually executes

This gives us an extremely important rule:

A timer delay tells us when the callback may become eligible; it does not guarantee the exact time the callback will execute.

13. Event Loop Does NOT Make Heavy JavaScript Non-Blocking

Suppose:

console.log("Start");


function heavyWork() {
    for (let i = 0; i < 5_000_000_000; i++) {}
}


heavyWork();


console.log("End");

heavyWork() is synchronous.

Therefore:

Start
 ↓
heavyWork()
 ↓
Call Stack busy
 ↓
other JS waits
 ↓
heavyWork finishes
 ↓
End

The Event Loop cannot magically make normal synchronous JavaScript asynchronous.

14. Browser Responsiveness 🔥

The Event Loop concept also helps explain why long-running synchronous JavaScript can make a webpage feel frozen.

For example:

function hugeCalculation() {
    // very expensive synchronous calculation
}


hugeCalculation();

While the main thread is occupied:

Main thread busy
      ↓
other JavaScript waits
      ↓
UI work may be delayed
      ↓
page can feel frozen

This is one reason we avoid unnecessarily heavy synchronous work on the browser's main thread.

15. Event Loop Is NOT a Queue

Another common confusion:

Event Loop = Queue ❌

No.

We have queues such as:

Microtask Queue
Task Queue

The Event Loop is part of the coordination/scheduling mechanism.

Think:

QUEUE
→ waiting work




EVENT LOOP
→ coordinates when that work can proceed
16. Event Loop Is NOT Part of JavaScript Language Itself

Another useful interview distinction.

Don't say:

JavaScript language itself contains the Event Loop.

Better:

The Event Loop is provided by the JavaScript runtime environment.

For example:

Browser
   ↓
has its event-loop/runtime model


Node.js
   ↓
has its own event-loop/runtime model

The details differ.

For now, we're learning the browser-oriented mental model.

17. Classic Interview Question 🔥🔥🔥

What is the output?

console.log("Start");


setTimeout(() => {
    console.log("Timer");
}, 0);


Promise.resolve().then(() => {
    console.log("Promise");
});


console.log("End");

Break it down.

Synchronous
Start
End
Microtask
Promise
Task
Timer

Therefore:

Start
End
Promise
Timer

Mental flow:

Start
  ↓
schedule timer task
  ↓
schedule Promise microtask
  ↓
End
  ↓
current synchronous work finishes
  ↓
Promise microtask
  ↓
Timer task
18. One More Output Question
console.log("1");


setTimeout(() => {
    console.log("2");
}, 0);


Promise.resolve().then(() => {
    console.log("3");
});


Promise.resolve().then(() => {
    console.log("4");
});


console.log("5");

First synchronous:

1
5

Microtasks were queued in order:

3
4

Then timer:

2

Final output:

1
5
3
4
2

Don't worry about mastering every output puzzle yet.

Your syllabus has a dedicated section later for:

Async execution-order questions
Timer + Promise output questions
Microtask vs Task Queue questions

We'll practice them properly there.

19. Common Mistakes 🔥
Mistake 1
setTimeout(fn, 0)
= execute immediately ❌

Correct:

schedule callback for later
Mistake 2
Event Loop executes multiple JS functions
at the same time ❌

No.

The browser/runtime provides asynchronous facilities, while normal main-thread JavaScript execution is still one piece at a time.

Mistake 3
Timer finished
= callback executes immediately ❌

Better:

Timer ready
    ↓
callback becomes eligible / queued
    ↓
wait for execution opportunity
Mistake 4
Task Queue always runs before
Microtask Queue ❌

Pending microtasks are processed before moving to the next task.

20. Interview Questions 🔥🔥🔥
What is the Event Loop?

The Event Loop is part of the JavaScript runtime's scheduling mechanism that coordinates when queued asynchronous work can execute.

Why do we need the Event Loop?

Because asynchronous callbacks/jobs can become ready while JavaScript is doing other work. The runtime needs scheduling rules to determine when that queued work gets an opportunity to execute.

Does setTimeout(fn, 0) execute immediately?

No. It schedules the callback for later. The callback cannot interrupt the currently executing synchronous JavaScript.

Which has higher priority: Promise or setTimeout?

Promise reactions are microtasks, and pending microtasks are processed before moving to the next timer task.

Does the Event Loop make JavaScript multi-threaded?

No. The Event Loop coordinates scheduling. It doesn't mean normal JavaScript on the main thread suddenly executes multiple functions simultaneously.

Can synchronous code delay a timer?

Yes. If JavaScript is busy executing synchronous code, a ready timer callback must wait for an opportunity to execute.

🧠 Quick Revision
EVENT LOOP


Coordinates when queued
asynchronous JavaScript
gets an opportunity to execute

Basic flow:

JavaScript executes
        ↓
Async operation starts
        ↓
Runtime handles waiting
        ↓
Callback/job becomes ready
        ↓
Relevant queue
        ↓
Current JS finishes
        ↓
Microtasks
        ↓
Next task
        ↓
JavaScript executes it

Remember:

SYNC
 ↓
MICROTASKS
 ↓
NEXT TASK

And:

setTimeout(fn, 0)
        ↓
NOT immediate
        ↓
callback executes later

Most important sentence:

The Event Loop coordinates when queued asynchronous work gets an opportunity to execute; current synchronous JavaScript finishes first, pending microtasks are processed, and then the runtime can move to the next task.

# >>

8.4 — CALL STACK

The Call Stack is where JavaScript keeps track of which function is currently running and what should run next.

Think:

CALL STACK
    ↓
Current JavaScript execution
1. Why is it called a “Stack”?

Because it follows:

LIFO

Meaning:

Last In
First Out

Imagine a stack of plates:

Plate 3   ← removed first
Plate 2
Plate 1

The last plate placed on top is the first one removed.

The Call Stack works similarly.

2. Simple Example
function greet() {
    console.log("Hello");
}


greet();

When JavaScript calls:

greet();

the function is placed on the Call Stack.

Conceptually:

CALL STACK


┌─────────────┐
│   greet()   │
└─────────────┘

Then:

console.log("Hello");

runs.

After greet() finishes:

CALL STACK


┌─────────────┐
│             │
└─────────────┘

The function is removed from the stack.

3. Functions are PUSHED and POPPED

You may hear these words in interviews.

When a function starts:

PUSH

When it finishes:

POP

So:

function called
      ↓
PUSH onto stack
      ↓
execute
      ↓
function finishes
      ↓
POP from stack
4. Nested Function Example 🔥

Consider:

function one() {
    two();
}


function two() {
    three();
}


function three() {
    console.log("Hello");
}


one();

Let's see the exact flow.

Step 1 — one()
one();

Call Stack:

┌─────────────┐
│    one()    │
└─────────────┘

Inside one():

two();
Step 2 — two()

two() is placed on top.

┌─────────────┐
│    two()    │
├─────────────┤
│    one()    │
└─────────────┘

Now two() executes.

Inside it:

three();
Step 3 — three()
┌─────────────┐
│   three()   │
├─────────────┤
│    two()    │
├─────────────┤
│    one()    │
└─────────────┘

Inside:

console.log("Hello");

Output:

Hello
5. Functions Finish in Reverse Order

three() finishes first.

┌─────────────┐
│    two()    │
├─────────────┤
│    one()    │
└─────────────┘

Then two() finishes:

┌─────────────┐
│    one()    │
└─────────────┘

Then one() finishes:

EMPTY STACK

So execution happened:

one()
 ↓
two()
 ↓
three()

But completion happened:

three()
 ↓
two()
 ↓
one()

That's LIFO.

6. Full Visual Flow 🔥

Code:

function one() {
    two();
}


function two() {
    three();
}


function three() {
    console.log("Done");
}


one();

Flow:

one()
 ↓ PUSH


STACK
one

Then:

two()
 ↓ PUSH


STACK
two
one

Then:

three()
 ↓ PUSH


STACK
three
two
one

Then:

three finishes
 ↓ POP


STACK
two
one

Then:

two finishes
 ↓ POP


STACK
one

Then:

one finishes
 ↓ POP


STACK
EMPTY
7. What About Normal Global Code?

Consider:

console.log("A");


function greet() {
    console.log("B");
}


greet();


console.log("C");

Output:

A
B
C

Conceptually, the script itself starts execution first.

Then:

greet()

gets pushed onto the stack.

After greet() finishes, JavaScript continues with:

console.log("C");

So:

Global/script execution
        ↓
greet()
        ↓
greet finishes
        ↓
continue script
8. Call Stack and Synchronous Code

This explains normal synchronous execution.

Example:

function calculate() {
    return 10 + 20;
}


const result = calculate();


console.log(result);

JavaScript cannot continue past the function call until the function completes.

Flow:

calculate()
    ↓
Call Stack
    ↓
return 30
    ↓
function removed
    ↓
result = 30
    ↓
continue

This is synchronous behavior.

9. Call Stack and Async JavaScript 🔥🔥🔥

Now consider:

console.log("Start");


setTimeout(() => {
    console.log("Timer");
}, 1000);


console.log("End");

A very important point:

The timer callback does not stay on the Call Stack for 1 second.

Don't imagine:

CALL STACK


setTimeout callback
     ↓
wait 1 second ❌

That would block JavaScript.

Instead:

setTimeout(...)
      ↓
runtime handles timer
      ↓
JavaScript continues

Your previous section described this runtime handoff and queue-based scheduling model.

10. What Happens to setTimeout()?

Consider:

setTimeout(() => {
    console.log("Hello");
}, 2000);

Conceptually:

Call Stack
    ↓
setTimeout()
    ↓
browser timer mechanism starts
    ↓
setTimeout() call itself finishes
    ↓
Call Stack continues

Later:

timer completes
    ↓
callback becomes ready
    ↓
Task Queue

Then when scheduling rules allow it:

callback
   ↓
Call Stack
   ↓
execute
11. Important Distinction 🔥

There are two different things here:

setTimeout(callback, 1000);
setTimeout()

The function call itself executes now.

callback

The callback executes later.

Think:

setTimeout()
   ↓
runs now


callback
   ↓
scheduled for later

Don't treat them as the same thing.

12. Call Stack + Event Loop

Now the connection should become clearer.

                CALL STACK
                    │
                    │ async operation
                    ▼
              Runtime / Web APIs
                    │
                    ▼
                  Queue
                    │
                    ▼
               EVENT LOOP
                    │
                    ▼
                CALL STACK

The Event Loop is concerned with when queued work gets an opportunity to return to JavaScript execution.

And JavaScript functions execute through the Call Stack.

13. Why Must the Stack Become Available?

Suppose:

setTimeout(() => {
    console.log("Timer");
}, 0);


function heavyWork() {
    for (let i = 0; i < 5_000_000_000; i++) {}
}


heavyWork();

Even though the timer has 0 delay:

heavyWork()
     ↓
Call Stack busy

The timer callback cannot simply interrupt it.

Conceptually:

Task Queue


Timer callback waiting

But:

Call Stack


heavyWork()

So the callback waits.

When heavyWork() finishes:

Call Stack becomes available

Then queued work can eventually execute.

14. Call Stack Blocking 🔥🔥

This is why long synchronous functions can freeze an application.

Example:

function calculateEverything() {
    for (let i = 0; i < 10_000_000_000; i++) {
        // huge calculation
    }
}


calculateEverything();

While it runs:

CALL STACK


calculateEverything()

Other JavaScript cannot run on that same main thread.

So:

Long synchronous function
          ↓
Call Stack occupied
          ↓
other JS waits
          ↓
UI can feel frozen
15. Stack Overflow 🔥

The Call Stack has limited space.

Consider recursion:

function test() {
    test();
}


test();

There is no stopping condition.

So:

test()
 ↓
test()
 ↓
test()
 ↓
test()
 ↓
test()
 ↓
...

The Call Stack keeps growing.

Conceptually:

┌─────────────┐
│   test()    │
├─────────────┤
│   test()    │
├─────────────┤
│   test()    │
├─────────────┤
│   test()    │
├─────────────┤
│   ...       │
└─────────────┘

Eventually JavaScript throws an error similar to:

Maximum call stack size exceeded

This is commonly called:

Stack Overflow
16. Proper Recursion

Compare with:

function countdown(number) {
    if (number === 0) {
        return;
    }


    console.log(number);


    countdown(number - 1);
}


countdown(3);

Stack grows temporarily:

countdown(3)
countdown(2)
countdown(1)

Then:

countdown(0)
   ↓
return

Functions start finishing and are removed.

So the stack can unwind.

17. Call Stack Errors Help Debugging

You've probably seen errors like:

at functionThree (...)
at functionTwo (...)
at functionOne (...)

This is related to the call stack.

Example:

function one() {
    two();
}


function two() {
    three();
}


function three() {
    throw new Error("Something went wrong");
}


one();

The error stack can show roughly:

three
two
one

This tells us the chain of function calls that led to the error.

Very useful for debugging.

18. Call Stack vs Task Queue 🔥

Don't confuse these.

CALL STACK
→ code executing NOW
TASK QUEUE
→ task callbacks waiting to execute

Example:

setTimeout(() => {
    console.log("Timer");
}, 0);

Before callback executes:

TASK QUEUE


Timer callback

When it gets an execution opportunity:

CALL STACK


Timer callback

Then it runs.

19. Call Stack vs Microtask Queue

Similarly:

Promise.resolve().then(() => {
    console.log("Promise");
});

The .then() reaction is scheduled as a microtask.

Before execution:

MICROTASK QUEUE


Promise callback

When it gets its turn:

CALL STACK


Promise callback

Then JavaScript executes it.

So queues hold waiting work.

The Call Stack contains currently executing JavaScript.

20. Very Important Mental Model 🔥🔥🔥

Remember this:

CALL STACK
→ What is running NOW?




WEB APIs / RUNTIME
→ What is being handled outside
  normal JS execution?




QUEUES
→ What is ready and waiting?




EVENT LOOP
→ When can waiting JS run?

This is the backbone of Async JavaScript.

21. Interview Question

What is the output?

function first() {
    console.log("First");


    second();


    console.log("First End");
}


function second() {
    console.log("Second");
}


first();

Let's trace it.

Start:

first()

Output:

First

Then:

second()

Stack:

second
first

Output:

Second

second() finishes.

Back to first():

console.log("First End");

Final output:

First
Second
First End

Important:

Calling second() does not permanently end first().

JavaScript remembers where first() paused using the execution stack/context.

22. Another Interview Example 🔥
console.log("A");


function one() {
    console.log("B");


    two();


    console.log("C");
}


function two() {
    console.log("D");
}


one();


console.log("E");

Output:

A
B
D
C
E

Execution:

A


one()
 ↓
B


two()
 ↓
D


two finishes
 ↓
back to one()


C


one finishes
 ↓
back to global code


E
23. Common Mistakes
Mistake 1
Call Stack stores all asynchronous work ❌

No.

Queued async callbacks/jobs wait elsewhere before they execute.

Mistake 2
setTimeout callback waits inside Call Stack ❌

No.

The runtime handles the timer and the callback is scheduled later.

Mistake 3
Event Loop executes functions directly ❌

Better mental model:

Event Loop
→ scheduling/coordination


Call Stack
→ JavaScript execution
Mistake 4
Call Stack = Task Queue ❌

Completely different roles.

Interview Questions 🔥
What is the Call Stack?

The Call Stack is the mechanism JavaScript uses to keep track of currently executing function calls.

What does LIFO mean?

Last In, First Out. The last function pushed onto the stack finishes and is removed before the function beneath it continues.

What happens when a function is called?

A new execution frame is pushed onto the Call Stack. When the function finishes, that frame is removed.

Does an async callback wait inside the Call Stack?

No. Async work is handled by the runtime and its callback/job waits through the relevant scheduling mechanism until it can execute.

What is stack overflow?

It happens when too many function calls build up on the Call Stack, commonly because of uncontrolled recursion.

Can long synchronous code block async callbacks?

Yes. If the Call Stack is busy running synchronous JavaScript, queued callbacks must wait.

🧠 Quick Revision
CALL STACK


Where JavaScript
is executing NOW

Remember:

Function called
     ↓
PUSH
     ↓
execute
     ↓
finish
     ↓
POP

And:

LIFO


Last In
First Out

Async relationship:

Async callback ready
       ↓
Queue
       ↓
wait
       ↓
gets execution opportunity
       ↓
Call Stack
       ↓
execute

Most important distinction:

CALL STACK
→ executing now


QUEUE
→ ready but waiting


EVENT LOOP
→ coordinates scheduling

Most important sentence:

The Call Stack tracks currently executing JavaScript function calls using a Last In, First Out structure; functions are pushed when called and popped when they finish.


# >>>

8.5 — WEB APIs

JavaScript itself cannot directly perform many operations such as:

Running timers
Making network requests
Listening for user clicks
Accessing browser storage
Reading the user's location

These capabilities are provided by the browser/runtime environment.

In browser JavaScript, many of these capabilities are exposed through Web APIs.

Think:

JavaScript
    ↓
asks the browser
    ↓
WEB APIs
    ↓
browser handles the work
1. What are Web APIs?

Web APIs are features provided by the browser that JavaScript can use to interact with the browser and perform operations outside normal JavaScript execution.

Examples:

setTimeout()
fetch()
DOM APIs
addEventListener()
localStorage
Geolocation API

Important:

JavaScript language
        ≠
Browser Web APIs

The browser provides these capabilities.

2. Why Do We Need Web APIs?

JavaScript executes code using the:

CALL STACK

Suppose JavaScript had to sit and wait for a timer:

setTimeout(() => {
    console.log("Hello");
}, 5000);

If JavaScript itself blocked for 5 seconds:

CALL STACK


wait...
wait...
wait...
wait...
wait...

nothing else could execute during that wait.

Instead, the browser handles the timer.

JavaScript
    ↓
setTimeout()
    ↓
Browser timer mechanism
    ↓
JavaScript continues

This allows JavaScript to continue executing other code.

3. Browser + JavaScript Runtime

A simplified browser environment looks like:

┌───────────────────────────────┐
│            BROWSER            │
│                               │
│   ┌───────────────────────┐   │
│   │   JavaScript Engine   │   │
│   │                       │   │
│   │      Call Stack       │   │
│   └───────────────────────┘   │
│                               │
│   ┌───────────────────────┐   │
│   │       Web APIs        │   │
│   │                       │   │
│   │ Timers                │   │
│   │ Network               │   │
│   │ DOM / Events          │   │
│   │ Geolocation           │   │
│   │ Storage               │   │
│   └───────────────────────┘   │
│                               │
└───────────────────────────────┘

So there are different responsibilities:

JavaScript Engine
      ↓
executes JavaScript




Browser / Web APIs
      ↓
provide browser capabilities
4. setTimeout() Example 🔥

Consider:

console.log("Start");


setTimeout(() => {
    console.log("Timer");
}, 2000);


console.log("End");

Output:

Start
End
Timer

Why?

Let's trace it.

Step 1 — Start
console.log("Start");

Runs normally.

Output:

Start
Step 2 — setTimeout()

JavaScript reaches:

setTimeout(() => {
    console.log("Timer");
}, 2000);

The timer capability is provided by the runtime/browser.

Conceptually:

Call Stack
    ↓
setTimeout()
    ↓
Browser timer mechanism

The browser starts tracking the timer.

JavaScript does not sit there waiting for 2 seconds.

Step 3 — JavaScript Continues

Next:

console.log("End");

runs immediately.

Output becomes:

Start
End

Meanwhile:

Browser


Timer
 ↓
counting...
Step 4 — Timer Completes

After approximately 2 seconds:

Timer finishes
      ↓
callback becomes eligible
      ↓
Task Queue

The callback does not necessarily execute immediately.

It waits until scheduling allows it to execute.

Eventually:

callback
   ↓
Call Stack
   ↓
console.log("Timer")

Final output:

Start
End
Timer
5. Web APIs Don't Mean Everything Is Async

This distinction is important.

Web APIs include many browser capabilities.

Some operations are asynchronous:

Timers
Network requests
User events

But the term Web API does not automatically mean:

Web API = async ❌

Web APIs are browser-provided interfaces.

Some of them participate heavily in asynchronous behavior.

6. Common Web APIs

Some important browser APIs you will encounter:

Timers
├── setTimeout()
└── setInterval()


Network
└── fetch()


DOM
├── document.querySelector()
├── document.createElement()
└── DOM manipulation


Events
└── addEventListener()


Storage
├── localStorage
└── sessionStorage


Location
└── Geolocation API

You don't need to memorize every Web API.

Understand the concept.

7. fetch() and Web APIs 🔥🔥

Consider:

fetch("https://example.com/users");

A network request can take:

100 ms
500 ms
2 seconds
10 seconds

JavaScript should not freeze while waiting.

Conceptually:

JavaScript
    ↓
fetch()
    ↓
Browser networking capability
    ↓
HTTP request happening

Meanwhile JavaScript can continue executing.

Later, when the operation progresses/completes, Promise-related reactions can be scheduled.

We will understand this properly when we study:

Promises
Microtask Queue
fetch()

For now remember:

JavaScript initiates the operation, while the runtime/browser performs the network work.

8. Event Listeners and Web APIs

Consider:

button.addEventListener("click", () => {
    console.log("Clicked");
});

JavaScript does not keep this callback running on the Call Stack while waiting for the user.

Imagine if it did:

CALL STACK


waiting for click...
waiting...
waiting...

That would make no sense.

Instead:

JavaScript
    ↓
register click handler
    ↓
Browser handles event monitoring

Later:

User clicks
    ↓
callback becomes ready
    ↓
scheduled for JavaScript execution

The callback eventually executes through the:

CALL STACK
9. Web APIs + Call Stack 🔥

The relationship is important.

CALL STACK
    ↓
JavaScript starts operation
    ↓
WEB API / RUNTIME
    ↓
operation handled outside
normal JS execution

JavaScript continues doing other work.

Later:

operation completes / event occurs
        ↓
callback/job becomes ready
        ↓
appropriate queue
        ↓
scheduling
        ↓
CALL STACK
        ↓
JavaScript executes callback

This is one of the core ideas behind asynchronous JavaScript.

10. Web APIs + Event Loop

Now connect everything you have learned:

        JAVASCRIPT ENGINE


          CALL STACK
              │
              │ starts async work
              ▼
        WEB APIs / RUNTIME
              │
              │ work becomes ready
              ▼
            QUEUE
              │
              ▼
         EVENT LOOP
              │
              ▼
          CALL STACK

Important:

The Web API does not directly interrupt currently running JavaScript.

Queued work has to wait for its execution opportunity.

11. Example With Blocking Code 🔥🔥

Consider:

setTimeout(() => {
    console.log("Timer");
}, 0);


for (let i = 0; i < 5_000_000_000; i++) {}


console.log("Done");

You might think:

0 ms timer
    ↓
Timer immediately

But no.

The timer mechanism can complete, but JavaScript is still busy running the loop.

CALL STACK


huge loop

Meanwhile:

TASK QUEUE


timer callback

The callback cannot interrupt the current synchronous code.

So:

huge loop finishes
      ↓
console.log("Done")
      ↓
current synchronous work finishes
      ↓
timer callback gets its turn

Output:

Done
Timer
12. Web APIs Don't Execute Your JavaScript Callback

A useful mental distinction:

The browser may handle:

Timer counting
Network communication
Event detection

But when your JavaScript callback actually executes:

() => {
    console.log("Hello");
}

it executes as JavaScript through the:

CALL STACK

So think:

Web API
→ handles external/runtime operation


Call Stack
→ executes JavaScript
13. Browser vs Node.js 🔥

Web APIs is mainly browser terminology.

Browser:

JavaScript Engine
+
Browser APIs

Node.js also provides runtime capabilities for things such as:

Timers
File system
Networking

But Node.js is not a browser.

So don't memorize:

All async JavaScript
    =
Web APIs

A better general mental model is:

JavaScript
    ↓
Runtime environment
    ↓
runtime handles external/async work

In browsers, many of those capabilities are exposed as Web APIs.

14. JavaScript Engine vs Web APIs 🔥🔥

This is a common interview distinction.

JavaScript Engine

Responsible for executing JavaScript.

Examples:

V8
SpiderMonkey
JavaScriptCore

Think:

JavaScript Engine
      ↓
runs JS code
Web APIs

Provided by the browser.

Examples:

DOM
Timers
Fetch
Events
Geolocation
Storage

Think:

Browser
   ↓
provides additional capabilities
15. setTimeout() Is Not Part of Core JavaScript 🔥

This surprises many beginners.

You use:

setTimeout(...)

inside JavaScript code.

But setTimeout() is not defined by the ECMAScript language itself.

It is provided by the runtime environment.

Similarly, in browsers:

document
window
fetch
localStorage

are environment-provided APIs.

JavaScript can use them because the browser exposes them.

16. What Happens When Async Work Finishes?

This is important because it connects directly to your next topics.

Suppose a timer finishes.

The callback doesn't simply jump into the Call Stack.

Instead, conceptually:

Timer completes
      ↓
callback becomes ready
      ↓
Task Queue
      ↓
wait for execution opportunity
      ↓
Call Stack

But not every async operation uses the same queue.

For example:

Timer callback
      ↓
Task Queue




Promise reaction
      ↓
Microtask Queue

This distinction is extremely important.

That's exactly why your next topics are:

Microtask Queue 🔥🔥🔥
Task Queue      🔥🔥
17. Important Mental Model 🔥🔥🔥

Remember these four responsibilities:

CALL STACK
→ executes JavaScript




WEB APIs / RUNTIME
→ handle browser/runtime capabilities




QUEUES
→ hold ready JavaScript work




EVENT LOOP
→ coordinates when queued work can run

Together:

JavaScript code
      ↓
Call Stack
      ↓
starts async operation
      ↓
Web API / Runtime
      ↓
operation becomes ready
      ↓
Queue
      ↓
Event Loop scheduling
      ↓
Call Stack
      ↓
callback executes
18. Common Mistakes
Mistake 1
Web APIs are part of the JavaScript language ❌

No.

They are provided by the environment, such as the browser.

Mistake 2
Web API callback executes inside the Web API ❌

The runtime handles the external operation.

Your JavaScript callback eventually executes through normal JavaScript execution.

Mistake 3
setTimeout waits inside the Call Stack ❌

No.

The runtime handles the timer.

Mistake 4
When a timer finishes, its callback immediately interrupts JavaScript ❌

No.

It becomes ready and waits for an execution opportunity.

Mistake 5
Every Web API is asynchronous ❌

No.

Web API simply means an API exposed by the browser.

Interview Questions 🔥
What are Web APIs?

Web APIs are browser-provided interfaces that JavaScript can use for capabilities such as timers, networking, DOM manipulation, events, storage, and geolocation.

Are Web APIs part of JavaScript?

No. JavaScript is defined by ECMAScript, while Web APIs are provided by environments such as web browsers.

Is setTimeout() part of JavaScript?

Not part of the core ECMAScript language. It is provided by the runtime environment.

What happens when JavaScript calls setTimeout()?

The runtime handles the timer. JavaScript continues executing, and when the timer completes, its callback becomes eligible to be scheduled for later execution.

Where does a timer callback wait?

Once ready, it is scheduled through the task queue.

Does a Web API execute JavaScript callbacks?

The runtime handles the underlying operation, but the JavaScript callback itself eventually executes as JavaScript through the Call Stack.

What is the relationship between Web APIs and the Event Loop?

Runtime/Web APIs handle operations outside normal JavaScript execution. When JavaScript work becomes ready, it is queued, and the Event Loop participates in scheduling that work for execution.

🧠 Quick Revision
WEB APIs


Browser-provided
capabilities that
JavaScript can use

Examples:

setTimeout()
fetch()
DOM
Events
Storage
Geolocation

Main flow:

CALL STACK
    ↓
start async operation
    ↓
WEB API / RUNTIME
    ↓
operation completes
    ↓
QUEUE
    ↓
EVENT LOOP
    ↓
CALL STACK
    ↓
execute callback

Remember:

JavaScript Engine
→ executes JavaScript


Web APIs / Runtime
→ provide external capabilities


Queues
→ ready work waiting


Event Loop
→ scheduling coordination


Call Stack
→ JavaScript executing now

Most important sentence:

Web APIs are capabilities provided by the browser environment that JavaScript can use for operations such as timers, networking and events; when asynchronous work becomes ready, its JavaScript callback/job is scheduled to eventually execute through the Call Stack.

# >>

8.5 — WEB APIs □□
The Call Stack executes JavaScript, but JavaScript cannot efficiently sit there waiting for things like:

Timer
API response User click Network request

The browser provides Web APIs to handle these operations outside the normal JavaScript execution stack.
The easiest way to understand Web APIs is:

JavaScript finds async operation
↓
registers / hands it to Web API
↓
Web API handles the waiting
↓
JavaScript continues executing


1.	What are Web APIs?
Web APIs are capabilities provided by the browser that JavaScript can use for things such as timers, network requests, DOM events, storage and geolocation.
Examples:

setTimeout() setInterval() fetch() addEventListener() DOM APIs
localStorage Geolocation

Important:

JavaScript language
≠ Web APIs

Web APIs are provided by the browser/runtime environment.


2.	Why Do We Need Web APIs?
Suppose we have:
console.log("Start"); setTimeout(() => {
console.log("Timer");
}, 2000);
 
console.log("End");

If JavaScript itself waited for 2 seconds:

CALL STACK

setTimeout()
↓
wait 2 seconds...
↓ wait...
↓ wait...

the Call Stack would be blocked.
JavaScript couldn't continue executing other code. Instead, the browser handles the timer.
JavaScript
↓
finds setTimeout()
↓
registers timer with browser timer API
↓
browser tracks the timer
↓
JavaScript continues


3.	Understand setTimeout() Properly □□□
This is the mental model you should remember.

setTimeout(() => { console.log("Hello");
}, 2000);

JavaScript reaches:

setTimeout(...)

Think step by step:

1.	JavaScript reaches setTimeout()

↓

2.	setTimeout() is called on the Call Stack

↓

3.	Timer + callback are registered with the browser's timer mechanism

↓

4.	Browser starts tracking 2000 ms
 
↓

5.	setTimeout() call finishes

↓

6.	JavaScript continues executing

So you can think:

That is a good learning mental model.


4.	What Does "Register" Mean? □
You'll hear this word frequently. Suppose:
setTimeout(callback, 2000);

Register simply means:

Tell the browser:

"Track this timer.

After the required delay,
this callback should become ready to run."

Conceptually:

JavaScript
↓
setTimeout(callback, 2000)
↓ REGISTER
↓
Browser Timer API

┌─────────────────────────┐
│ Timer: 2000 ms	│
│ Callback: callback	│
└─────────────────────────┘

JavaScript doesn't wait there.


5.	Full setTimeout() Flow □□□
Consider:

console.log("Start");
 
setTimeout(() => { console.log("Timer");
}, 2000);

console.log("End");

Let's follow the complete journey.

Step 1
console.log("Start");

Call Stack:

CALL STACK

console.log("Start")

Output:

Start

Then it is removed.

Step 2
JavaScript finds:

setTimeout(() => { console.log("Timer");
}, 2000);

Think:

CALL STACK

setTimeout(...)

The timer and callback are registered with the browser timer mechanism.

CALL STACK
↓ setTimeout()
↓
WEB API / TIMER ENVIRONMENT

Timer: 2000 ms
Callback: () => console.log("Timer")

The browser starts tracking the timer.

Step 3
The setTimeout() call itself finishes. The Call Stack does not contain:
wait 2 seconds □
 
Instead:

WEB API

Timer counting...

2000 ms

Meanwhile JavaScript continues.

Step 4
JavaScript reaches:

console.log("End");

Output:

Start End

Meanwhile the timer is still being tracked by the browser.

Step 5
After the delay has elapsed:

WEB API / TIMER

2000 ms completed
↓
Timer callback becomes eligible

Now an important thing happens.
The callback does not jump directly into the Call Stack. It is queued as a task.
Timer completes
↓
Task Queue

┌──────────────────────┐
│ Timer callback	│
└──────────────────────┘

Step 6
When the runtime's scheduling rules allow that task to run:

Task Queue
↓
Timer callback
↓
CALL STACK
↓ console.log("Timer")
 
Output:

Timer

Final output:

Start End Timer


6.	The Complete Timer Mental Model □□□
Memorize this flow:

JavaScript finds setTimeout()
↓
setTimeout() executes
↓
register timer + callback
↓
Browser Timer API
↓
timer runs / delay elapses
↓
callback becomes ready
↓ TASK QUEUE
↓
wait for execution opportunity
↓ CALL STACK
↓
callback executes

Short version:

setTimeout
↓
Web API Timer
↓
Task Queue
↓
Call Stack


7.	Important — Timer Doesn't Move to Web API
Be precise with the wording. Don't think:
setTimeout function itself
moves permanently into Web API □

A better model:

JavaScript calls setTimeout()
 
↓
browser timer facility is asked to track the timer/callback
↓
setTimeout() call returns
↓
JavaScript continues

So:

setTimeout()
→ called now

Timer
→ tracked by runtime

Callback
→ executed later

These are different things.


8.	What Does 0ms Mean? □□
Consider:

setTimeout(() => { console.log("Timer");
}, 0);

console.log("Hello");

You might think:

0 milliseconds
↓
run immediately □

No.
0 means the timer does not intentionally wait for a longer requested delay before becoming eligible.
The callback still has to be scheduled.

setTimeout(callback, 0)
↓
Timer mechanism
↓
callback becomes eligible
↓ Task Queue
↓
wait
↓
Call Stack

Therefore:
 
Hello Timer


9.	Web APIs + fetch() □□
Timers are not the only example. Consider:
fetch("/users");

A network request may take:

100 ms
500 ms
2 seconds
10 seconds

JavaScript should not sit on the Call Stack waiting. Conceptually:
JavaScript reaches fetch()
↓
browser networking capability handles the request
↓
JavaScript continues
↓
network operation progresses

Later, Promise settlement causes the relevant Promise reactions to be scheduled. We will understand that fully under:
Promises Microtask Queue fetch()


10.	Web APIs + Events
Consider:

button.addEventListener("click", () => { console.log("Clicked");
});

JavaScript registers an event handler. Think:
JavaScript
↓ addEventListener()
↓
register callback
 
↓
Browser event system

Now JavaScript doesn't sit there doing:

waiting... waiting...
waiting for click... □

The browser monitors the event. Later:
User clicks button
↓
Browser detects click
↓
callback becomes eligible
↓
scheduled as a task
↓
event callback executes


11.	What Does "Callback Becomes Ready" Mean?
Suppose:

setTimeout(callback, 2000);

After the timer delay has elapsed:

callback does NOT automatically execute

Instead:

Timer delay elapsed
↓
callback becomes eligible
↓
Task Queue

It is now essentially saying:

"I'm ready to execute, but I need my turn."

This distinction becomes extremely important when learning:

Task Queue Microtask Queue Event Loop


12.	Web APIs + Call Stack
Now connect them.
 
CALL STACK
↓
JavaScript calls async API
↓
WEB API / RUNTIME
↓
underlying operation happens

Meanwhile:

CALL STACK
↓
JavaScript continues

Later:

operation ready
↓ Queue
↓ scheduling
↓
Call Stack
↓
JavaScript callback executes


13.	Web APIs + Event Loop □□□
Your complete async model is becoming:

CALL STACK
│
│ starts operation
▼
WEB APIs / RUNTIME
│
│ work ready
▼ QUEUE
│
▼
EVENT LOOP
│
▼
CALL STACK

Simplified:

Call Stack
↓ Web API
↓ Queue
↓
Event Loop scheduling
↓
Call Stack
 
14.	Web API Does Not Interrupt JavaScript
Suppose:

setTimeout(() => { console.log("Timer");
}, 0);

function heavyWork() {
for (let i = 0; i < 5_000_000_000; i++) {}
}

heavyWork();

The timer can become ready while:

CALL STACK

heavyWork()

is still executing.
The callback cannot say:

STOP heavyWork()

I need to execute now □

Instead:

TASK QUEUE

Timer callback

waits.
After current JavaScript finishes, the timer task can eventually get an execution opportunity.


15.	Blocking □□
This explains blocking. Suppose:
function heavyWork() {
for (let i = 0; i < 10_000_000_000; i++) {}
}

heavyWork();

While it runs:

CALL STACK

heavyWork()

The Call Stack is occupied.
 
Therefore:

Long synchronous code
↓
Call Stack occupied
↓
other JavaScript waits
↓
timers/callbacks delayed
↓
UI may freeze

This is called:

BLOCKING


16.	What Is Starvation? □□
You specifically need this term because it becomes important with queues.
Starvation means some work keeps waiting because other higher-priority work continuously gets processed before it.
Think of a queue at a counter.

Person A waiting Person B waiting Person C waiting

But every time Person A is about to get a chance:

Higher-priority person arrives
↓ goes first

Again:

another higher-priority person
↓ goes first

Again:

another one
↓ goes first

The original person keeps waiting. That is the basic idea of:
STARVATION


17.	Starvation in Async JavaScript □□□
 
This becomes particularly important with:

Microtask Queue
vs Task Queue

Remember the simplified priority:

Current synchronous code
↓ MICROTASKS
↓ Next TASK

Before taking the next task, the runtime performs a microtask checkpoint and drains available microtasks.
Now imagine:

MICROTASK QUEUE

Promise callback Promise callback Promise callback Promise callback
...

And those microtasks keep creating more microtasks. Meanwhile:
TASK QUEUE

setTimeout callback

is waiting.
Conceptually:

Microtask
↓
creates Microtask
↓
creates Microtask
↓
creates Microtask
↓
keeps continuing Meanwhile...
Timer task
↓ waiting... waiting... waiting...

The normal task can be delayed for a long time. This is commonly described as:
 
MICROTASK STARVATION


18.	Simple Starvation Example
Consider:

function repeat() { Promise.resolve().then(repeat);
}
repeat(); setTimeout(() => {
console.log("Timer");
}, 0);

Conceptually:

Promise microtask
↓ repeat()
↓
creates another microtask
↓ repeat()
↓
creates another microtask
↓
...

The Microtask Queue keeps receiving more work. Meanwhile:
TASK QUEUE

Timer callback

may keep waiting.
So:

We'll understand this much better in Microtask Queue.


19.	Blocking vs Starvation □□□
Do not confuse them.

Blocking
One long-running synchronous operation
↓
 
Call Stack stays busy
↓
other JavaScript cannot execute

Example:

while (true) {
// never finishes
}

Think:

BLOCKING
=
Call Stack is occupied

Starvation
Work is ready
↓
but other work repeatedly gets priority
↓
it keeps waiting

Think:

STARVATION
=
Ready to run,
but not getting a turn

Easy distinction:

BLOCKING

"Stack is busy."


STARVATION

"I am waiting for my turn,
but other work keeps going first."


20.	Browser vs Node.js □
Web APIs is mainly browser terminology. Browser:
JavaScript Engine
+
Browser APIs

Node.js also provides runtime capabilities such as:

Timers Networking
 
File system

But Node.js is not a browser. So don't memorize:
Async JavaScript
=
Web APIs □

Better:

JavaScript
↓
Runtime Environment
↓
runtime provides capabilities

Browser:

Browser APIs / Web APIs

Node.js:

Node runtime APIs


21.	JavaScript Engine vs Web APIs □
JavaScript Engine
Executes JavaScript.
Examples:

V8
SpiderMonkey JavaScriptCore

Think:

JavaScript Engine
↓ EXECUTES JS

Web APIs
Provided by the browser. Examples:
Timers Fetch DOM
Events Storage Geolocation
 
Think:

Web APIs
↓
PROVIDE BROWSER CAPABILITIES


22.	setTimeout() Is Not Core JavaScript □
You write:

setTimeout(callback, 1000);

inside JavaScript.
But setTimeout() is not part of the core ECMAScript language. It is provided by the runtime environment.
Similarly, browsers provide:

window document fetch localStorage setTimeout

JavaScript can use them because the browser exposes them.


23.	Important Queue Preview □□□
Not every asynchronous operation is scheduled in the same way. For now remember:
setTimeout callback
↓ Task Queue

While:

Promise reaction
↓ Microtask Queue

And the simplified priority is:

Synchronous code
↓ Microtasks
↓ Next Task

This is why:

setTimeout(() => { console.log("Timer");
 
}, 0);

Promise.resolve().then(() => { console.log("Promise");
});

normally gives:

Promise Timer

We'll cover this properly in the next sections.


24.	Complete Mental Model □□□
Put everything together:

JavaScript code
↓
CALL STACK
↓
finds async API
↓
registers operation/callback
↓
WEB API / RUNTIME
↓
runtime handles waiting/work
↓
operation becomes ready
↓
appropriate QUEUE
↓
wait for execution opportunity
↓
CALL STACK
↓
callback executes

For a timer specifically:

setTimeout()
↓
register timer + callback
↓
Browser Timer API
↓
delay elapses
↓
Task Queue
↓ wait
↓
Call Stack
↓
callback executes
 
25.	Common Mistakes
Mistake 1
Web APIs are JavaScript itself □

No.
They are provided by the browser/runtime.

Mistake 2
setTimeout waits on the Call Stack □

No.
The runtime tracks the timer.

Mistake 3
0ms means execute immediately □

No.
The callback still needs to be scheduled.

Mistake 4
Timer callback directly jumps from Web API to Call Stack □

No.
It becomes eligible and is scheduled as a task.

Mistake 5
Web API executes my JavaScript callback while other JS is running □

No.
Your callback eventually executes as JavaScript through the Call Stack.

Mistake 6
Blocking = Starvation □

No.

Blocking
→ Call Stack occupied

Starvation
→ work keeps waiting because other work keeps getting priority
 
Interview Questions □
What are Web APIs?
Web APIs are capabilities provided by the browser that JavaScript can use for operations such as timers, networking, DOM events, storage and geolocation.

What happens when JavaScript reaches setTimeout()?
JavaScript calls setTimeout(), which registers the timer and callback with the runtime's timer mechanism. The call returns and JavaScript continues executing.

Does the callback stay on the Call Stack while the timer runs?
No. The runtime tracks the timer outside normal JavaScript execution.

What happens after the timer delay finishes?
The callback becomes eligible and is queued as a task. It executes later when scheduling allows it to run.

Does setTimeout(callback, 0) execute immediately?
No. 0 does not mean immediate execution. The callback still goes through task scheduling.

What is blocking?
Blocking occurs when long-running synchronous JavaScript occupies the Call Stack and prevents other JavaScript from executing.

What is starvation?
Starvation occurs when ready work keeps waiting because other work repeatedly receives execution priority.

What is microtask starvation?
It occurs when microtasks continuously create more microtasks, potentially delaying normal tasks such as timer callbacks.

Blocking vs starvation?
Blocking
→ Call Stack is busy.

Starvation
→ Work is ready but keeps failing to get its turn.


□ Quick Revision
Web API:
 
Browser-provided capability

Examples:

Timers Fetch DOM
Events Storage Geolocation

Timer:

JavaScript finds setTimeout()
↓
setTimeout() executes
↓
REGISTER timer + callback
↓
Browser Timer API
↓
delay elapses
↓
TASK QUEUE
↓
wait for turn
↓ CALL STACK
↓
callback executes

Remember register:

REGISTER

=
Tell the runtime:

"Track this operation
and associate this callback with it."

Remember blocking:

BLOCKING

Call Stack busy
↓
other JavaScript waits

Remember starvation:

STARVATION

Work is ready
↓
keeps waiting
↓
other work keeps getting priority
 
And the most important distinction:

CALL STACK
→ executing JavaScript NOW

WEB API / RUNTIME
→ handling external/runtime work

QUEUE
→ ready JavaScript work waiting

EVENT LOOP / SCHEDULER
→ coordinates execution opportunities

Most important sentence:

Progress
FOUNDATION
├── Sync vs Async	□ DONE
├── How JavaScript handles async work □ DONE
├── Event Loop	□ DONE
├── Call Stack	□ DONE
├── Web APIs	□ DONE
├── Microtask Queue	□ □□□ ← NEXT
└── Task Queue	□ □□



# >>>

OUNDATION — 6. Microtask Queue 🔥🔥🔥

You already know:

Sync vs Async                     ✅
How JavaScript handles async work ✅
Event Loop                        ✅
Call Stack                        ✅
Web APIs                          ✅

Now we connect all of those with the Microtask Queue.

1. What is the Microtask Queue?

The Microtask Queue is a special queue where JavaScript keeps high-priority asynchronous callbacks that are ready to execute.

Most importantly for MERN development:

Promise callbacks
.then()
.catch()
.finally()


async / await

use the Microtask Queue.

Simple definition

Microtask Queue = a high-priority waiting queue for Promise-related asynchronous work.

2. Why do we need it?

Suppose JavaScript has:

console.log("Start");


Promise.resolve().then(() => {
  console.log("Promise");
});


console.log("End");

Output:

Start
End
Promise

Why didn't "Promise" print immediately?

Because the callback inside .then() doesn't execute directly.

It waits in the Microtask Queue.

3. Step-by-step execution

Consider:

console.log("Start");


Promise.resolve().then(() => {
  console.log("Promise");
});


console.log("End");
Step 1
console.log("Start");

goes to the Call Stack and executes.

Start
Step 2

JavaScript reaches:

Promise.resolve().then(() => {
  console.log("Promise");
});

The Promise is already resolved.

But the .then() callback still does not execute immediately.

Its callback is placed in:

Microtask Queue

Now:

Microtask Queue
┌────────────────────────┐
│ () => console.log(...) │
└────────────────────────┘
Step 3

JavaScript continues with:

console.log("End");

Output becomes:

Start
End
Step 4

The synchronous code is finished.

The Call Stack becomes empty.

Call Stack
┌───────────────┐
│     EMPTY     │
└───────────────┘

Now the Event Loop checks the Microtask Queue.

It finds:

() => {
  console.log("Promise");
}

The callback moves:

Microtask Queue
       ↓
Call Stack

and executes.

Final output:

Start
End
Promise
4. Complete flow

Remember this flow:

JavaScript Code
       │
       ▼
   Call Stack
       │
       │ synchronous code executes
       ▼
Promise becomes ready
       │
       ▼
 Microtask Queue
       │
       │ Call Stack becomes empty
       ▼
   Event Loop
       │
       ▼
   Call Stack
       │
       ▼
    Execute
5. What goes into the Microtask Queue?

For your MERN learning, remember these:

Microtask Queue
│
├── Promise .then()
├── Promise .catch()
├── Promise .finally()
├── async / await continuation
└── queueMicrotask()

The most important ones right now are:

.then()
.catch()
.finally()

You will understand async/await when we reach that section.

6. Microtask Queue has higher priority 🔥🔥🔥

This is the most important rule.

There are two queues you are going to learn:

Microtask Queue
Task Queue

The Microtask Queue has higher priority.

Microtask Queue
      ↓
HIGH PRIORITY




Task Queue
      ↓
LOWER PRIORITY

For example:

console.log("Start");


setTimeout(() => {
  console.log("Timer");
}, 0);


Promise.resolve().then(() => {
  console.log("Promise");
});


console.log("End");

What is the output?

Start
End
Promise
Timer
7. Why Promise runs before setTimeout()

Let's trace it.

First:

console.log("Start");

Output:

Start

Then JavaScript finds:

setTimeout(() => {
  console.log("Timer");
}, 0);

The timer is registered with the Web APIs timer environment.

When the timer finishes, its callback becomes ready for the Task Queue.

Task Queue
┌──────────────┐
│ Timer        │
└──────────────┘

Then JavaScript finds:

Promise.resolve().then(() => {
  console.log("Promise");
});

The Promise is resolved, so its .then() callback is scheduled in the:

Microtask Queue
┌──────────────┐
│ Promise      │
└──────────────┘

Then:

console.log("End");

executes.

At this point:

Call Stack
EMPTY




Microtask Queue
┌──────────────┐
│ Promise      │
└──────────────┘




Task Queue
┌──────────────┐
│ Timer        │
└──────────────┘

The Event Loop gives priority to:

Microtask Queue

So:

Promise

runs first.

Then:

Timer

runs.

Therefore:

Start
End
Promise
Timer
8. Important Rule 🔥🔥🔥

Remember:

1. Execute synchronous code


2. Call Stack becomes empty


3. Execute ALL available Microtasks


4. Then take the next Task


5. Repeat

So conceptually:

Synchronous Code
       ↓
Microtask Queue
       ↓
Task Queue

NOT:

Synchronous Code
       ↓
Task Queue
       ↓
Microtask Queue
9. Does the Event Loop execute only one microtask?

No.

This is important.

The Event Loop processes all currently queued microtasks before moving to the next task.

Example:

setTimeout(() => {
  console.log("Timer");
}, 0);


Promise.resolve().then(() => {
  console.log("Promise 1");
});


Promise.resolve().then(() => {
  console.log("Promise 2");
});


Promise.resolve().then(() => {
  console.log("Promise 3");
});

Output:

Promise 1
Promise 2
Promise 3
Timer

The Task Queue had:

Timer

But JavaScript first drained the Microtask Queue:

Promise 1
Promise 2
Promise 3

Only after that did it execute the timer.

10. Microtasks follow queue order

Microtasks generally execute in the order they are queued.

Promise.resolve().then(() => {
  console.log("A");
});


Promise.resolve().then(() => {
  console.log("B");
});


Promise.resolve().then(() => {
  console.log("C");
});

Output:

A
B
C

Think:

Microtask Queue


FRONT
  ↓
┌─────────────┐
│ A           │
├─────────────┤
│ B           │
├─────────────┤
│ C           │
└─────────────┘
11. Can a microtask create another microtask?

Yes. 🔥

Example:

Promise.resolve().then(() => {
  console.log("A");


  Promise.resolve().then(() => {
    console.log("B");
  });
});


setTimeout(() => {
  console.log("Timer");
}, 0);

Output:

A
B
Timer

When A executes, it creates another Promise microtask:

Microtask Queue
      ↓
B

JavaScript still doesn't move to the Task Queue.

It continues draining the Microtask Queue.

Therefore B runs before Timer.

12. Microtask Starvation 🔥

Now the term you specifically need to know:

What is starvation?

Starvation means some work keeps waiting because other higher-priority work continuously gets executed before it.

For example, imagine microtasks continuously create more microtasks.

function repeat() {
  Promise.resolve().then(repeat);
}


repeat();


setTimeout(() => {
  console.log("Timer");
}, 0);

The microtask keeps creating another microtask:

Microtask
   ↓
creates another Microtask
   ↓
creates another Microtask
   ↓
creates another Microtask
   ↓
...

JavaScript keeps trying to drain the Microtask Queue.

The timer may keep waiting.

Microtask Queue
      ↓
      ↓
      ↓
 keeps receiving work




Task Queue
┌─────────────┐
│ Timer       │  ← keeps waiting
└─────────────┘

This situation is called:

Microtask Starvation

Easy definition

Microtask starvation happens when microtasks continuously generate more microtasks, preventing the Event Loop from getting a chance to process normal tasks.

This can delay things such as:

Timers
User events
Rendering
Other tasks

You don't need deeper browser internals for MERN right now.

13. MERN example

Later you will frequently write:

fetch("/api/users")
  .then((response) => response.json())
  .then((data) => {
    console.log(data);
  });

Promise callbacks such as:

.then(...)

run as microtasks once the relevant Promise is settled.

Similarly:

async function getUsers() {
  const response = await fetch("/api/users");
  const data = await response.json();


  console.log(data);
}

The code that continues after an await is scheduled through the Promise/microtask mechanism once the awaited Promise settles.

That is why understanding the Microtask Queue becomes very useful when debugging async MERN applications.

14. Interview Rule 🔥🔥🔥

If you get an output question containing:

console.log()
setTimeout()
Promise.then()

first classify them:

console.log()       → synchronous


Promise.then()      → Microtask Queue


setTimeout()        → Task Queue

Then use:

Synchronous
    ↓
Microtasks
    ↓
Tasks

Example:

console.log("1");


setTimeout(() => console.log("2"), 0);


Promise.resolve().then(() => console.log("3"));


console.log("4");

Output:

1
4
3
2
15. Final Mental Model

Keep this in your head:

              JavaScript
                  │
                  ▼
             Call Stack
                  │
        synchronous work
                  │
                  ▼
          Call Stack Empty
                  │
                  ▼
             Event Loop
              /       \
             /         \
            ▼           ▼
   Microtask Queue    Task Queue
   HIGH PRIORITY      LOWER PRIORITY
        │                  │
        │                  │
 Promise callbacks      Timers etc.
        │                  │
        └──────► first ◄────┘

The key rule is:

When the current JavaScript job finishes, JavaScript drains the Microtask Queue before moving to the next Task Queue task.

What you need to remember
Microtask Queue
│
├── Stores high-priority async callbacks
│
├── Promise callbacks use it
│   ├── .then()
│   ├── .catch()
│   └── .finally()
│
├── async/await continuation uses microtasks
│
├── Runs after current synchronous code
│
├── Has priority over the Task Queue
│
├── All queued microtasks are drained before
│   moving to the next task
│
└── Too many continuously generated microtasks
    can cause Microtask Starvation


# >>>

8. ASYNC JAVASCRIPT
FOUNDATION — 7. Task Queue 🔥🔥

You already know that asynchronous callbacks don't simply jump into the Call Stack.

The Task Queue is another place where callbacks wait until JavaScript is ready to execute them.

1. What is the Task Queue?

The Task Queue is a queue that holds callbacks from certain asynchronous operations after those operations are ready to run.

It is also commonly called:

Task Queue
     =
Macrotask Queue
     =
Callback Queue

Task Queue = a waiting queue for callbacks such as setTimeout() callbacks.

For your current MERN learning, think mainly about:

setTimeout()
setInterval()
DOM events
2. Basic Example
console.log("Start");


setTimeout(() => {
  console.log("Timer");
}, 2000);


console.log("End");

Output:

Start
End


// after around 2 seconds


Timer

Let's understand exactly what happens.

3. Step-by-Step Execution 🔥
Step 1 — console.log("Start")
console.log("Start");

goes to the Call Stack and executes.

Output:

Start
Step 2 — JavaScript finds setTimeout()
setTimeout(() => {
  console.log("Timer");
}, 2000);

JavaScript does not sit there waiting for 2 seconds.

The timer is registered with the browser's Web APIs timer environment.

Conceptually:

Call Stack
     │
     │ setTimeout found
     ▼
Web APIs
┌───────────────────────┐
│ Timer: 2000ms         │
│ Callback: Timer       │
└───────────────────────┘

JavaScript continues executing the remaining code.

4. JavaScript continues

Next:

console.log("End");

executes immediately.

Output:

Start
End

Meanwhile, the timer is running outside the JavaScript Call Stack.

JavaScript             Web APIs


Call Stack             Timer
   │                    2000ms
   │                      ↓
continues              counting
5. What happens when the timer finishes?

After approximately 2000ms, the callback:

() => {
  console.log("Timer");
}

does not directly enter the Call Stack.

This is very important.

It is placed into the:

Task Queue

So:

Web APIs
   │
   │ Timer completes
   ▼
Task Queue
┌──────────────────────┐
│ Timer callback       │
└──────────────────────┘

Now it is ready to execute, but it still has to wait.

6. When does the callback execute?

The Event Loop checks whether JavaScript can execute the next task.

Conceptually:

Task Queue
    │
    │ Event Loop
    ▼
Call Stack
    │
    ▼
Execute

But remember the rule from the previous topic:

Microtasks have priority before the next task is taken.

So the simplified order is:

Current synchronous code
        ↓
Microtask Queue
        ↓
Next Task
7. setTimeout(..., 0) 🔥🔥

This is extremely important.

Look at:

console.log("A");


setTimeout(() => {
  console.log("B");
}, 0);


console.log("C");

What is the output?

A
C
B

Not:

A
B
C
8. Why doesn't 0ms execute immediately?

Because:

setTimeout(callback, 0);

does not mean:

Execute the callback immediately.

It means roughly:

After the timer is eligible, schedule the callback to run as a task when JavaScript gets the opportunity.

So:

setTimeout(callback, 0)
          │
          ▼
     Timer handling
          │
          ▼
      Task Queue
          │
          │ wait
          ▼
      Event Loop
          │
          ▼
      Call Stack

The current synchronous code must finish first.

Therefore:

console.log("A");


setTimeout(() => {
  console.log("B");
}, 0);


console.log("C");

becomes:

A
C
B
9. Timer delay is NOT guaranteed execution time 🔥🔥

Suppose:

setTimeout(() => {
  console.log("Hello");
}, 2000);

A common misunderstanding is:

"Hello" will execute exactly after 2 seconds.

That's not guaranteed.

2000ms is better understood as a minimum delay before the callback can become eligible to run.

After that, the callback may still have to wait for JavaScript to become available.

10. Example — Call Stack is busy
console.log("Start");


setTimeout(() => {
  console.log("Timer");
}, 1000);


const start = Date.now();


while (Date.now() - start < 5000) {
  // blocking JavaScript for around 5 seconds
}


console.log("End");

The timer becomes eligible long before the blocking loop ends.

But the callback cannot interrupt the currently running JavaScript.

Conceptually:

Timer eligible
     │
     ▼
Task Queue
┌─────────────┐
│ Timer       │
└─────────────┘
     │
     │ waiting...
     │
     │ Call Stack still busy
     ▼

Only after the synchronous work finishes can JavaScript eventually run the timer callback.

So:

Timer delay tells us when the callback may become eligible, not the exact moment it will execute.

11. Task Queue vs Microtask Queue 🔥🔥🔥

This distinction is extremely important.

Consider:

console.log("Start");


setTimeout(() => {
  console.log("Timer");
}, 0);


Promise.resolve().then(() => {
  console.log("Promise");
});


console.log("End");

First classify everything:

console.log("Start")  → Synchronous


setTimeout() callback → Task


Promise.then()        → Microtask


console.log("End")    → Synchronous

Now apply:

Synchronous Code
       ↓
Microtasks
       ↓
Next Task

Output:

Start
End
Promise
Timer
12. Visual Execution 🔥

During execution:

CALL STACK
────────────────
console.log("Start")
console.log("End")




MICROTASK QUEUE
────────────────
Promise callback




TASK QUEUE
────────────────
Timer callback

After synchronous code finishes:

Call Stack
   EMPTY
     │
     ▼


Microtask Queue
┌───────────────┐
│ Promise       │
└───────────────┘
     │
     ▼
   Execute

Then:

Task Queue
┌───────────────┐
│ Timer         │
└───────────────┘
     │
     ▼
   Execute

Therefore:

Start
End
Promise
Timer
13. What commonly creates Tasks?

For your current level, remember:

Task Queue
│
├── setTimeout() callback
├── setInterval() callback
├── DOM event callbacks
│   ├── click
│   ├── input
│   └── key events
│
└── other browser task sources

Don't try to memorize every browser task source.

For MERN + interviews, the most important one right now is:

setTimeout()
14. DOM Event Example

Suppose:

button.addEventListener("click", () => {
  console.log("Clicked");
});

When this code executes, JavaScript is registering an event listener.

It does not execute the callback.

Later:

User clicks button
       ↓
Browser detects click
       ↓
Click task becomes eligible
       ↓
JavaScript gets an opportunity to run it
       ↓
Callback executes

So:

() => {
  console.log("Clicked");
}

runs later when the click event is processed.

15. setInterval()

Example:

setInterval(() => {
  console.log("Hello");
}, 1000);

This requests repeated timer callbacks.

Conceptually:

Timer
  ↓
callback becomes eligible
  ↓
Task Queue
  ↓
Execute


Timer
  ↓
callback becomes eligible
  ↓
Task Queue
  ↓
Execute


...

Again, 1000ms does not guarantee the callback executes at an exact perfect 1-second boundary if JavaScript is busy.

16. Very Important Execution Rule 🔥🔥🔥

For output questions, use this mental model:

1. Run current synchronous JavaScript


                ↓


2. Call Stack becomes empty


                ↓


3. Drain Microtask Queue


                ↓


4. Run the next eligible Task


                ↓


5. Drain Microtask Queue again


                ↓


6. Run the next Task


                ↓


              Repeat

Notice something important:

Microtasks are checked between tasks.

17. Important Example 🔥🔥🔥
setTimeout(() => {
  console.log("Timer 1");


  Promise.resolve().then(() => {
    console.log("Promise");
  });
}, 0);


setTimeout(() => {
  console.log("Timer 2");
}, 0);

You might think:

Timer 1
Timer 2
Promise

But that's wrong.

The output is:

Timer 1
Promise
Timer 2

Why?

First task:

Timer 1

executes.

While executing, it creates a microtask:

Microtask Queue


Promise

Before JavaScript takes the next task, it drains the Microtask Queue.

So:

Timer 1
   ↓
Promise
   ↓
Timer 2

This rule is extremely useful for interview output questions.

18. Task Queue is FIFO — Simplified Mental Model

For the normal examples you're learning, think of the queue as:

First In, First Out

Example:

setTimeout(() => {
  console.log("A");
}, 0);


setTimeout(() => {
  console.log("B");
}, 0);


setTimeout(() => {
  console.log("C");
}, 0);

Typical output:

A
B
C

Mental model:

FRONT
  ↓


┌─────────────┐
│ A           │
├─────────────┤
│ B           │
├─────────────┤
│ C           │
└─────────────┘

For now, this is enough. Browser scheduling has more details, but you do not need that depth for MERN.

19. Microtask Queue vs Task Queue
Microtask Queue	Task Queue
Higher priority	Lower priority
Promise callbacks	Timer callbacks
.then()	setTimeout()
.catch()	setInterval()
.finally()	DOM events
await continuation	Event tasks
Drained before next task	Tasks run one at a time

The biggest thing to remember:

Promise.then()
      ↓
Microtask Queue




setTimeout()
      ↓
Task Queue
20. Interview Example 🔥🔥🔥

What is the output?

console.log("1");


setTimeout(() => {
  console.log("2");
}, 0);


Promise.resolve().then(() => {
  console.log("3");
});


Promise.resolve().then(() => {
  console.log("4");
});


setTimeout(() => {
  console.log("5");
}, 0);


console.log("6");
Step 1 — synchronous
1
6
Step 2 — microtasks
3
4
Step 3 — tasks
2
5

Final output:

1
6
3
4
2
5
21. Final Mental Model 🔥🔥🔥

Keep this entire Async Foundation picture in your head:

                  JavaScript
                      │
                      ▼
                 Call Stack
                      │
              synchronous code
                      │
                      ▼
                Stack Empty
                      │
                      ▼
                 Event Loop
                      │
          ┌───────────┴───────────┐
          │                       │
          ▼                       ▼
 Microtask Queue             Task Queue
 HIGH PRIORITY               LOWER PRIORITY
          │                       │
     Promise.then()           setTimeout()
     Promise.catch()          setInterval()
     Promise.finally()        DOM events
     await continuation
          │                       │
          └──────────┬────────────┘
                     │
                     ▼
                 Call Stack
                     │
                     ▼
                   Run
22. What You Need to Remember
Task Queue
│
├── Holds callbacks ready to run as tasks
│
├── setTimeout() callbacks
│
├── setInterval() callbacks
│
├── DOM event callbacks
│
├── Callback doesn't directly enter Call Stack
│
├── setTimeout(..., 0) does NOT mean immediate execution
│
├── Timer delay is not guaranteed execution time
│
├── Microtasks have priority before the next task
│
└── Simplified execution order:
       Current Sync Code
              ↓
       All Microtasks
              ↓
         Next Task
              ↓
       All Microtasks
              ↓
         Next Task

# >>>
















