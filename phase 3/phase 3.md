Phase 3 — JavaScript
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




