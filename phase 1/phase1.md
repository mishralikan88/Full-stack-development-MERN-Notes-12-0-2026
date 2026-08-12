#### PHASE 1 — FULL-STACK BASICS & PROJECT SETUP


### Topic 1 — How a Web Application Works


Don't memorize a large definition. By the end of this topic, you only need to clearly understand:

When a user clicks something in a MERN application, how does data travel from the screen to MongoDB and back to the screen?

1. What is a Web Application?

A web application is software that users access through a browser.

Examples include Gmail, Amazon, LinkedIn, Jira, banking applications, admin dashboards, booking systems, and ecommerce applications.

For our MERN development, think of a web application as 3 major parts:

┌─────────────────────┐
│      FRONTEND       │
│                     │
│ React + TypeScript  │
│ Tailwind / CSS      │
└──────────┬──────────┘
           │
           │ HTTP / API
           ▼
┌─────────────────────┐
│       BACKEND       │
│                     │
│ Node.js + Express   │
│ TypeScript          │
└──────────┬──────────┘
           │
           │ Query / Save
           ▼
┌─────────────────────┐
│      DATABASE       │
│                     │
│      MongoDB        │
└─────────────────────┘

These three pieces are the foundation of our full-stack work.

2. What is Frontend?

The frontend is the part of the application the user interacts with in the browser.

Our frontend stack will primarily be:

HTML
CSS / Tailwind
JavaScript
TypeScript
React

Suppose we're building an Employee Management System.

The user sees:

--------------------------------
       Employee Management
--------------------------------

Name:        [ Rahul          ]

Email:       [ rahul@test.com ]

Department:  [ Development    ]

             [ Add Employee ]
--------------------------------

React is responsible for things such as:

displaying this form
capturing what the user types
showing validation errors
calling the backend
showing loading
displaying success/error messages
updating the employee list
3. What is Backend?

The backend runs on the server, not inside the user's browser.

Our backend stack:

Node.js
   +
Express
   +
TypeScript

Suppose React wants to create this employee:

{
  "name": "Rahul",
  "email": "rahul@test.com",
  "department": "Development"
}

React sends that information to our backend.

For example:

POST /api/employees

The backend receives it.

Later we'll build an architecture like:

Route
  ↓
Middleware
  ↓
Controller
  ↓
Service
  ↓
Repository / Model
  ↓
MongoDB

Don't memorize this architecture yet.

We'll actually build it.

4. What is a Database?

A database stores application data permanently.

We'll primarily use:

MongoDB

For example, MongoDB could contain:

{
  "_id": "emp123",
  "name": "Rahul",
  "email": "rahul@test.com",
  "department": "Development",
  "status": "active"
}

Why don't we simply keep this inside React?

Because React's application state is not our permanent database.

If the user closes the browser, comes back tomorrow, and logs in again, their employees still need to exist.

That's why we need persistent storage.

5. How Do Frontend and Backend Communicate?

Through HTTP requests and responses.

Imagine React saying:

Backend, please give me all employees.

React sends:

GET /api/employees

Backend responds:

[
  {
    "name": "Rahul",
    "department": "Development"
  },
  {
    "name": "Priya",
    "department": "HR"
  }
]

React receives this data and displays it.

6. What is an API?

For now, use this simple understanding:

An API provides a defined way for the frontend to communicate with the backend.

For our employee application:

GET    /api/employees
POST   /api/employees
GET    /api/employees/:id
PATCH  /api/employees/:id
DELETE /api/employees/:id

These allow the frontend to perform different operations.

7. GET

We normally use GET when we want to retrieve data.

Example:

GET /api/employees

Meaning:

Give me the employees.

Flow:

React
  │
  │ GET /api/employees
  ▼
Express
  │
  ▼
MongoDB
  │
  │ Employees
  ▼
Express
  │
  │ JSON
  ▼
React
8. POST

We normally use POST when creating something.

POST /api/employees

Request data:

{
  "name": "Rahul",
  "email": "rahul@test.com"
}

Meaning:

Create this employee.

9. PATCH

Suppose Rahul moves from Development to Management.

PATCH /api/employees/123

Request:

{
  "department": "Management"
}

Meaning:

Update part of employee 123.

10. DELETE

To delete employee 123:

DELETE /api/employees/123

Meaning:

Delete this employee.

So you'll constantly encounter:

CREATE     → POST

READ       → GET

UPDATE     → PATCH / PUT

DELETE     → DELETE

This is commonly called CRUD.

And you will write CRUD functionality many times during our projects.

11. The Complete Full-Stack Flow

Now here's the important part.

Admin enters:

Name: Rahul
Email: rahul@test.com

[ Add Employee ]
Step 1 — React

React has the form values.

name  = Rahul
email = rahul@test.com

Admin clicks:

Add Employee

Step 2 — React sends an HTTP request

Conceptually:

POST /api/employees

with:

{
  "name": "Rahul",
  "email": "rahul@test.com"
}
Step 3 — Express receives it

Our backend receives the request.

Eventually you'll write code similar to:

router.post("/employees", createEmployee);

Don't worry about the syntax yet.

Step 4 — Backend validates it

The backend checks things such as:

Is name provided?

Is email valid?

Does this email already exist?

Does this user have permission?

Never assume the frontend validation alone is enough.

Step 5 — MongoDB stores it

MongoDB creates something similar to:

{
  "_id": "abc123",
  "name": "Rahul",
  "email": "rahul@test.com"
}
Step 6 — Backend responds

The server might return:

201 Created

and:

{
  "success": true,
  "data": {
    "_id": "abc123",
    "name": "Rahul",
    "email": "rahul@test.com"
  }
}
Step 7 — React receives the response

React can now:

update the employee list
clear the form
show a success message

User sees:

Employee created successfully.

---------------------------
Rahul
rahul@test.com
---------------------------
12. Visualize the Complete Journey

This is the mental model I want you to develop:

                    USER
                      │
                      │ clicks
                      ▼
              ┌───────────────┐
              │     REACT     │
              │   FRONTEND    │
              └───────┬───────┘
                      │
                      │ HTTP REQUEST
                      │ POST /api/employees
                      ▼
              ┌───────────────┐
              │    EXPRESS    │
              │    BACKEND    │
              └───────┬───────┘
                      │
                 Validate
                      │
                Business Logic
                      │
                      ▼
              ┌───────────────┐
              │    MONGODB    │
              │   DATABASE    │
              └───────┬───────┘
                      │
                   Saved
                      │
                      ▼
              ┌───────────────┐
              │    EXPRESS    │
              └───────┬───────┘
                      │
                      │ HTTP RESPONSE
                      │ JSON
                      ▼
              ┌───────────────┐
              │     REACT     │
              └───────┬───────┘
                      │
                 Update UI
                      │
                      ▼
                    USER

This flow will appear again and again throughout your MERN career.

13. Where Does TypeScript Fit?

TypeScript isn't another separate layer.

We'll use it on both sides.

FRONTEND
React
  +
TypeScript

      ↕ API

BACKEND
Node
  +
Express
  +
TypeScript

      ↕

MongoDB

For example, an employee type might eventually look like:

interface Employee {
  name: string;
  email: string;
  department: string;
}

We'll properly learn this during TypeScript.

14. Where Does Tailwind Fit?

Tailwind is primarily concerned with styling the frontend.

React
 │
 ├── TypeScript → logic/types
 │
 └── Tailwind → styling

It doesn't talk to MongoDB.

It doesn't create backend APIs.

15. What Does Node.js Do?

Simple definition for now:

Node.js lets us execute JavaScript/TypeScript-based server-side applications.

Later we'll understand the Node runtime, event loop, async I/O, streams, processes, etc.

Right now, that's enough.

16. What Does Express Do?

Express helps us build our HTTP server/API on Node.

For example:

GET /employees

POST /employees

PATCH /employees/:id

DELETE /employees/:id

Express makes defining and handling these routes straightforward.

17. What Does MongoDB Do?

MongoDB stores the data.

So keep this simple association:

React
"What should the user see/do?"

Express + Node
"What should happen on the server?"

MongoDB
"What data should persist?"
18. Practical Thinking — Employee Application

A client says:

Build an Employee Management System.

Don't immediately start coding.

Break it down.

Frontend
Login
Dashboard
Employee List
Add Employee
Edit Employee
Employee Details
Backend
Authentication

Employee APIs

Validation

Permissions

Error handling
Database

Potential collections:

users

employees

Later this could become much larger.

19. One Feature Becomes Three Tasks

Client says:

“Admin should be able to create employees.”

As a full-stack developer, your brain should convert that into:

FEATURE
Create Employee
      │
      ├──────── FRONTEND
      │
      │   Form
      │   Validation
      │   Submit
      │   Loading
      │   Error
      │   Success
      │
      ├──────── BACKEND
      │
      │   POST API
      │   Validation
      │   Business Logic
      │   Error Handling
      │
      └──────── DATABASE
              Employee Schema
              Save Employee

This is one of the most important habits we'll develop.

A client gives you business language.

You convert it into technical tasks.

20. Tiny Interview Notes

For this topic, these are enough:

Frontend: Client-side part of an application responsible for user interaction and presentation.

Backend: Server-side part responsible for APIs, business logic, security and data operations.

Database: Persistent storage for application data.

API: Defined interface through which systems communicate.

HTTP request: A message a client sends to a server.

HTTP response: The server's reply.

CRUD: Create, Read, Update, Delete.

Don't memorize paragraphs.

Understand the flow.


 
Topic 1 — Remaining Web Fundamentals (Take Topic 2 references)

1. Client vs Server

This is simple:

Client = the application/device requesting something.

Server = the application receiving the request, processing it, and returning something.

In our project:

CLIENT                              SERVER

React Browser App                   Express App
localhost:5173                      localhost:5000

      GET /api/employees
            ────────────────►

                              Express processes
                              the request

            ◄────────────────
             JSON Response

So in our MERN application:

React/Browser = Client
Express/Node  = Server
MongoDB       = Database
Important distinction

Frontend and client are closely related, but they're not definitions of exactly the same thing.

A client is anything making a request.

For example, when we tested:

GET http://localhost:5000/api/employees

using Postman:

Postman = Client
Express = Server

When React calls it:

React/Browser = Client
Express       = Server

Interview answer: Client sends requests; server receives requests, performs processing/business logic, communicates with resources such as databases, and returns responses.

2. Browser

Chrome, Edge, Firefox etc. aren't just displaying HTML.

In our application, the browser:

Browser
│
├── Loads HTML
├── Loads CSS
├── Executes JavaScript
├── Runs our React application
├── Handles user interaction
├── Sends HTTP requests
├── Receives HTTP responses
├── Enforces browser security such as CORS
├── Stores cookies
├── Provides localStorage
└── Provides sessionStorage

When you open:

http://localhost:5173

the browser loads your frontend.

Then React can execute:

fetch("http://localhost:5000/api/employees")

The browser sends that HTTP request.

3. DNS Basics

Imagine you visit:

example.com

Servers communicate using IP addresses.

DNS essentially helps translate:

example.com

      ↓ DNS

93.x.x.x

      ↓

Server

Think of DNS as the internet's name-to-address system.

You remember:

google.com

instead of remembering its server IP address.

Simplified flow
User enters:
example.com
      ↓
DNS lookup
      ↓
Find IP address
      ↓
Connect to server
      ↓
Send HTTP request

You don't need deep DNS administration at this stage.

4. HTTP vs HTTPS

HTTP = Hypertext Transfer Protocol.

It defines how clients and servers exchange HTTP messages.

Our local project uses:

http://localhost:5173

http://localhost:5000
HTTPS

HTTPS is HTTP protected using TLS.

HTTP
Client ─────────────── Server
      data exchange

HTTPS
Client ═══════════════ Server
       TLS protected

For production websites, HTTPS is the standard expectation.

Remember:

HTTP  → communication protocol

HTTPS → HTTP over a TLS-protected connection

HTTPS helps protect data in transit.

5. JSON — Proper Understanding


1. What is JSON?

JSON = JavaScript Object Notation

JSON is simply a text format used to exchange data.

In our MERN project:

React
  ↓
 JSON
  ↓
Express

and

Express
  ↓
 JSON
  ↓
React

For example, employee data can be represented as JSON:

{
  "name": "Amar",
  "email": "amar@test.com",
  "department": "Engineering",
  "role": "Developer"
}

So remember:

Frontend and backend commonly exchange API data using JSON.

2. JavaScript Object vs JSON

This is the most important distinction.

Inside React, suppose we have:

const employee = {
  name: "Amar",
  role: "Developer"
};

This is a:

JavaScript Object

It exists inside our JavaScript application.

JSON representation looks like:

{
  "name": "Amar",
  "role": "Developer"
}

JSON is a text-based representation of data.

Think:

JavaScript Object
       ↓
  JSON.stringify()
       ↓
    JSON text

Example:

const employee = {
  name: "Amar",
  role: "Developer"
};

const jsonData = JSON.stringify(employee);

jsonData represents:

'{"name":"Amar","role":"Developer"}'
3. Why Do We Need JSON.stringify()?

This is exactly what we used in our project:

body: JSON.stringify(form)

Suppose React currently has:

form = {
  name: "Amar",
  email: "amar@test.com"
};

That's a JavaScript object.

Before sending it as our JSON request body, we do:

JSON.stringify(form)

So:

React

JavaScript Object
{
  name: "Amar",
  email: "amar@test.com"
}

        ↓

JSON.stringify()

        ↓

JSON text

'{"name":"Amar","email":"amar@test.com"}'

        ↓

HTTP POST

        ↓

Express
Easy memory

Sending JSON:

Object
   ↓
JSON.stringify()
   ↓
JSON text
4. Why Did We Write Content-Type?

Our React code was:

fetch("http://localhost:5000/api/employees", {
  method: "POST",

  headers: {
    "Content-Type": "application/json"
  },

  body: JSON.stringify(form)
});

This:

body: JSON.stringify(form)

sends JSON.

And this:

"Content-Type": "application/json"

tells the backend:

The body I'm sending contains JSON.

Think:

Content-Type
     ↓
"What type of data am I sending?"

application/json
     ↓
"I'm sending JSON."
5. What Does Express Do With JSON?

Now the JSON reaches our backend.

We already added:

app.use(express.json());

express.json() tells Express:

If a request contains JSON, parse that JSON for me.

So:

React
│
│ JavaScript Object
│
▼
JSON.stringify()
│
│ JSON
│
▼
HTTP POST
│
▼
Express
│
▼
express.json()
│
▼
req.body

Now Express can access:

req.body

For example:

console.log(req.body);

could give:

{
  name: "Amar",
  email: "amar@test.com"
}

And:

req.body.name

gives:

Amar
6. Then We Save It to MongoDB

Our backend has:

const employee = await Employee.create(req.body);

Since:

req.body

contains:

{
  name: "Amar",
  email: "amar@test.com",
  department: "Engineering",
  role: "Developer"
}

Mongoose can save that employee to MongoDB.

So our complete sending flow is:

React Form
     ↓

JavaScript Object
     ↓

JSON.stringify()
     ↓

JSON
     ↓

POST /api/employees
     ↓

Express
     ↓

express.json()
     ↓

req.body
     ↓

Employee.create(req.body)
     ↓

MongoDB

This is exactly what you've already built.

7. Now Understand the Reverse Direction

So far:

React → Express

But Express also needs to send data back:

Express → React

Suppose MongoDB has:

Amar
Rahul
Priya

Express gets them:

const employees = await Employee.find();

Then we send:

res.status(200).json({
  success: true,
  data: employees
});

Notice:

.json(...)

Express sends a JSON response.

So:

MongoDB
   ↓
Employee.find()
   ↓
Express
   ↓
res.json()
   ↓
JSON Response
   ↓
React
8. How Does React Read the JSON?

Our React code:

const response = await fetch(
  "http://localhost:5000/api/employees"
);

At this point:

response

represents the HTTP response.

Then we do:

const result = await response.json();

This reads/parses the JSON response body.

So:

Express
   ↓

JSON Response
   ↓

React fetch()
   ↓

response
   ↓

response.json()
   ↓

JavaScript value
   ↓

result

Then:

result.data

contains our employees.

9. The Two Things That Look Similar

This is where you should be careful.

When React SENDS data

We use:

JSON.stringify(form)

Meaning:

JavaScript value
      ↓
JSON.stringify()
      ↓
JSON text
When React RECEIVES JSON

We use:

await response.json()

Meaning:

JSON response
      ↓
response.json()
      ↓
JavaScript value
Easy memory trick
SEND
────

JavaScript
    ↓
JSON.stringify()
    ↓
JSON


RECEIVE
───────

JSON
    ↓
response.json()
    ↓
JavaScript

⭐ This is the part I want you to remember.

10. Where Does express.json() Fit?

Now add the backend piece:

                REACT SENDS

JavaScript Object
       ↓
JSON.stringify()
       ↓
JSON
       ↓
HTTP
       ↓


              EXPRESS RECEIVES

JSON
       ↓
express.json()
       ↓
req.body
       ↓
JavaScript Object

So there are three different JSON-related things we've used:

Code	Purpose
JSON.stringify(data)	Convert JS value → JSON text
express.json()	Parse incoming JSON request body
response.json()	Parse JSON response body in frontend

That's the key table.

11. Full Example From OUR Project

User enters:

Name: Amar
Email: amar@test.com
Role: Developer

React state:

{
  name: "Amar",
  email: "amar@test.com",
  role: "Developer"
}

Then:

JSON.stringify(form)

↓

JSON request body:

{
  "name": "Amar",
  "email": "amar@test.com",
  "role": "Developer"
}

↓

Express:

express.json()

↓

Now:

req.body

contains the parsed employee data.

↓

Mongoose:

Employee.create(req.body)

↓

MongoDB saves it.

⭐ Final Diagram

Learn this diagram instead of memorizing definitions:

                     CREATE EMPLOYEE


USER
 │
 ▼
React Form
 │
 ▼
JavaScript Object
 │
 │
 │ JSON.stringify()
 ▼
JSON
 │
 │ HTTP POST
 ▼
Express
 │
 │ express.json()
 ▼
req.body
 │
 ▼
Employee.create()
 │
 ▼
MongoDB
 │
 │
 │ Employee.find()
 ▼
Express
 │
 │ res.json()
 ▼
JSON Response
 │
 ▼
React fetch()
 │
 │ response.json()
 ▼
JavaScript Data
 │
 ▼
setEmployees()
 │
 ▼
React UI
 │
 ▼
USER
Your 4-line revision
JSON = text format for exchanging data.

JSON.stringify()
→ JavaScript value → JSON text

express.json()
→ Incoming JSON body → req.body

response.json()
→ JSON response body → JavaScript value


6. REST API

REST = Representational State Transfer.

For our practical learning, focus on resources + HTTP methods + sensible URLs.

Bad style:

/getAllEmployees
/createNewEmployee
/deleteEmployee

A REST-style resource design would be:

GET     /api/employees
POST    /api/employees
GET     /api/employees/:id
PATCH   /api/employees/:id
DELETE  /api/employees/:id

Notice:

employees

is the resource.

The HTTP method communicates the operation.

GET       → Read
POST      → Create
PATCH     → Partially update
DELETE    → Delete

This becomes very important when we build proper APIs later.

7. HTTP Status Codes

You don't need to memorize hundreds.

For MERN interviews, start with these:

2xx — SUCCESS

200 OK
→ Successful request

201 Created
→ Resource created


4xx — CLIENT-SIDE REQUEST PROBLEMS

400 Bad Request
→ Invalid request/data

401 Unauthorized
→ Authentication required/invalid

403 Forbidden
→ Authenticated but not permitted

404 Not Found
→ Resource/route not found

409 Conflict
→ Request conflicts with current state
   Example: duplicate email


5xx — SERVER PROBLEMS

500 Internal Server Error
→ Unexpected server-side failure

In our project:

POST employee successful
        ↓
201 Created

GET employees successful
        ↓
200 OK
8. HTTP Headers

Headers provide metadata about an HTTP request or response.

You already used one:

headers: {
  "Content-Type": "application/json"
}

That tells the server:

The request body is JSON.

Later, authentication commonly uses something like:

Authorization: Bearer <token>

Think:

HTTP Request
│
├── Method
├── URL
├── Headers
└── Body

Example:

POST /api/employees

Headers:
Content-Type: application/json

Body:
{
  "name": "Amar"
}
9. Route Params

Suppose we want one specific employee.

Instead of:

/api/employees

we use:

/api/employees/123

Express route:

app.get("/api/employees/:id", ...)

Here:

:id

is a route parameter.

If request URL is:

/api/employees/123

then:

req.params.id

is:

"123"

Practical use:

GET    /employees/:id
PATCH  /employees/:id
DELETE /employees/:id

Use route params when identifying a particular resource.

10. Query Params

Query parameters usually modify/filter the request.

Example:

/api/employees?status=active

Here:

status=active

is a query parameter.

Express:

req.query.status

Examples:

/api/employees?department=engineering

/api/employees?page=2

/api/employees?sort=name

/api/employees?status=active&page=2

Common uses:

Filtering
Searching
Sorting
Pagination
Route vs Query

Remember this:

/employees/123
           ↑
       Route param
       WHICH employee?


/employees?status=active
           ↑
       Query param
       HOW to filter/list?
11. Cookies

A cookie is small data associated with a website that the browser stores and can send with matching HTTP requests according to cookie rules.

A server can send:

Set-Cookie: ...

The browser stores it.

Then relevant future requests can include that cookie.

Server
   ↓
Set-Cookie
   ↓
Browser stores cookie
   ↓
Later HTTP request
   ↓
Cookie can be sent
   ↓
Server

Cookies are commonly involved in:

Sessions
Authentication
Preferences

Security attributes such as HttpOnly, Secure, and SameSite become important for authentication. We'll cover them properly there.

12. localStorage

Browser-provided storage.

JavaScript can do:

localStorage.setItem("theme", "dark");

Read:

localStorage.getItem("theme");

Remove:

localStorage.removeItem("theme");

Data generally persists across:

Refresh ✅
Tab close/reopen ✅
Browser restart ✅

until it is cleared/removed, subject to browser behavior.

Good example:

UI preferences

Don't treat localStorage as a secure place for sensitive secrets.

13. sessionStorage

Similar API:

sessionStorage.setItem("step", "2");

Read:

sessionStorage.getItem("step");

The major practical difference:

sessionStorage
      ↓
Scoped to a browser tab/session
      ↓
Typically cleared when that tab/session ends

Useful for temporary tab-specific state.

14. Cookie vs localStorage vs sessionStorage

This is interview-important:

	Cookie	localStorage	sessionStorage
Browser storage	✅	✅	✅
JS can access	Often*	✅	✅
Automatically included in applicable HTTP requests	✅	❌	❌
Persists after tab closes	Depends	✅	❌
Typical use	Sessions/auth/preferences	Persistent client state	Temporary tab state

*An HttpOnly cookie cannot be read by JavaScript, which is an important security feature.

The easiest memory trick:

Cookie
→ Can participate automatically in HTTP requests

localStorage
→ Persistent browser-side JS storage

sessionStorage
→ Temporary tab/session storage
15. CORS Basics

You have already used CORS practically in Topic 2.

Our frontend:

http://localhost:5173

Backend:

http://localhost:5000

These are different origins because the ports differ.

The browser applies the same-origin policy and CORS rules to cross-origin requests.

Our Express configuration:

app.use(
  cors({
    origin: "http://localhost:5173",
  })
);

allows that frontend origin under the configured policy.

Remember this distinction:

React
  ↓
Browser
  ↓
CORS rules checked
  ↓
Express response permissions

CORS is primarily a browser-enforced cross-origin access mechanism. It is not a replacement for authentication or authorization.

One diagram to connect everything
User enters website URL
          ↓
        Browser
          ↓
      DNS lookup
          ↓
     Server address
          ↓
     HTTPS Request
          ↓
        Headers
          ↓
    Express REST API
          ↓
 Route / Query Parameters
          ↓
     Business Logic
          ↓
       Mongoose
          ↓
       MongoDB
          ↓
     JSON Response
          ↓
    HTTP Status Code
          ↓
       Browser
          ↓
        React
          ↓
          UI

Browser may also use:
├── Cookies
├── localStorage
└── sessionStorage

Cross-origin browser request?
          ↓
       CORS rules




### Topic 2: Development Setup

Topic 2: Development Setup
🎯 Goal of Topic 2

By the end, your computer will have:

mern-master-project/
│
├── client/                     ← FRONTEND
│   ├── src/
│   │   ├── App.tsx
│   │   └── main.tsx
│   ├── package.json
│   └── ...
│
├── server/                     ← BACKEND
│   ├── src/
│   │   └── index.ts
│   ├── package.json
│   ├── tsconfig.json
│   └── ...
│
└── .gitignore

And eventually:

React
localhost:5173
     ↓
Express API
localhost:5000
     ↓
MongoDB

We'll do it one step at a time.

2.1 — Create the Main Project Folder
Step 1 — Decide where the project will live

For example, on Windows you could use:

D:\Projects

Open that folder in File Explorer.

You do not need to use D: specifically. You can use any development folder you prefer.

For this lesson, I'll assume:

D:\Projects
Step 2 — Open terminal there

Inside File Explorer:

Right-click inside D:\Projects → Open in Terminal

Or open PowerShell and run:

cd D:\Projects

Your terminal should now show something similar to:

PS D:\Projects>
Important

Whenever I give you a command from now on, I'll show you WHERE to run it.

2.2 — Create Our Main Folder
📍 Run from:
D:\Projects

Run:

mkdir mern-master-project

Then enter it:

cd mern-master-project

Now your terminal should show:

PS D:\Projects\mern-master-project>

Your computer now contains:

D:\Projects
│
└── mern-master-project
2.3 — Open the Project in VS Code
📍 You should currently be inside:
D:\Projects\mern-master-project

Run:

code .

The . means:

Open the current folder in VS Code.

VS Code should open with:

mern-master-project

as the root folder.

From this point onward

When I say:

Create something in the ROOT

I mean:

mern-master-project/

Not inside client.

Not inside server.

2.4 — Initialize Git

Open the VS Code terminal:

Terminal → New Terminal

Check the terminal path.

It should say:

PS D:\Projects\mern-master-project>
📍 Run from ROOT:
mern-master-project/

Run:

git init

You should see something similar to:

Initialized empty Git repository...

Git is now tracking this project.

We'll learn Git properly while working on the project.

2.5 — Create .gitignore
📍 Create this file HERE:
mern-master-project/
│
└── .gitignore        ← CREATE HERE

In VS Code:

Right-click:

mern-master-project

Select:

New File

Name it exactly:

.gitignore

Put this inside:

node_modules/
.env
dist/
Why?

Very briefly:

node_modules → thousands of installed dependency files
.env         → secrets/configuration
dist         → generated production build

We don't want these committed normally.

2.6 — Create the React + TypeScript Frontend

Now we're creating:

client/

But do not manually create client.

Vite will create it for us.

📍 Terminal must be at ROOT:
PS D:\Projects\mern-master-project>

Check before running anything.

Now run:

npm create vite@latest client -- --template react-ts

This tells Vite:

Create project
      ↓
Folder name = client
      ↓
Framework = React
      ↓
Language = TypeScript

After completion, VS Code should show:

mern-master-project/
│
├── client/
│   ├── public/
│   ├── src/
│   ├── package.json
│   ├── tsconfig.json
│   └── ...
│
└── .gitignore
2.7 — Install Frontend Dependencies

Now we need to go inside client.

📍 Current location:
mern-master-project/

Run:

cd client

Your terminal should become something like:

PS D:\Projects\mern-master-project\client>
📍 Now run INSIDE client:
npm install

This installs the frontend dependencies.

You'll notice:

client/
└── node_modules/

appears.

2.8 — Start the Frontend
📍 Stay inside:
mern-master-project/client

Run:

npm run dev

You should see something similar to:

VITE ready

Local: http://localhost:5173/

Open the local address shown by Vite in your browser.

You should see the default Vite + React screen.

✅ Checkpoint 1

At this point:

React              ✅
TypeScript         ✅
Vite               ✅
Frontend running   ✅
2.9 — Clean the Default React Screen

Now go to VS Code.

📍 Open this exact file:
mern-master-project
└── client
    └── src
        └── App.tsx       ← OPEN THIS

Delete its existing contents.

Replace them with:

function App() {
  return (
    <div>
      <h1>MERN Master Project</h1>
      <p>Frontend is running successfully.</p>
    </div>
  );
}

export default App;

Save:

Ctrl + S

Go back to the browser.

You should see:

MERN Master Project

Frontend is running successfully.

Great.

Your frontend is now running.

2.10 — Create the Backend Folder

Keep your frontend running.

Don't stop that terminal.

Open a second VS Code terminal:

Terminal → New Terminal

The new terminal may open at:

PS D:\Projects\mern-master-project>

If it doesn't, navigate to your root folder.

📍 We need to be at ROOT:
mern-master-project/

Now create:

mkdir server

Your project becomes:

mern-master-project/
│
├── client/                  ← FRONTEND
│
├── server/                  ← BACKEND
│
└── .gitignore
2.11 — Enter the Backend
📍 From ROOT run:
cd server

Terminal should now show:

PS D:\Projects\mern-master-project\server>

Everything in the next few steps happens inside server.

2.12 — Initialize the Backend Node Project
📍 Run inside:
mern-master-project/server

Run:

npm init -y

Now you'll see:

server/
└── package.json
What happened?

package.json describes our Node project.

We'll understand it properly later while using it.

2.13 — Install Express
📍 Still inside:
mern-master-project/server

Run:

npm install express

Express is the framework we'll use to create our backend APIs.

2.14 — Install TypeScript Development Tools
📍 Still inside server:
npm install -D typescript tsx @types/node @types/express

Very short meaning:

typescript
    → TypeScript compiler

tsx
    → lets us conveniently run TypeScript during development

@types/node
    → Node.js TypeScript definitions

@types/express
    → Express TypeScript definitions

Don't memorize these.

You'll naturally understand them through use.

2.15 — Create TypeScript Configuration
📍 Still inside:
mern-master-project/server

Run:

npx tsc --init

Now:

server/
│
├── package.json
│
└── tsconfig.json       ← CREATED

tsconfig.json controls how TypeScript behaves.

We are not deep-diving into tsconfig now. That's part of our TypeScript phase.

2.16 — Create Backend src Folder

Now use the VS Code Explorer.

Right-click:

server

Choose:

New Folder

Name:

src

You should have:

server/
│
├── src/
├── package.json
└── tsconfig.json
2.17 — Create Backend Entry File

Right-click:

server/src

Select:

New File

Create:

index.ts

Exact location:

mern-master-project/
│
└── server/
    └── src/
        └── index.ts       ← CREATE HERE

This is important.

Don't create index.ts directly inside server.

It belongs inside:

server/src/
2.18 — Write Your First Express Server
📍 Open:
server/src/index.ts

Add:

import express from "express";

const app = express();

const PORT = 5000;

app.get("/api/health", (req, res) => {
  res.json({
    success: true,
    message: "Backend is running successfully",
  });
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});

Don't worry about understanding every line yet.

Here's the quick picture:

express()
   ↓
Create backend application

app.get(...)
   ↓
Create GET API

/api/health
   ↓
API address

res.json(...)
   ↓
Send JSON response

app.listen(5000)
   ↓
Start backend on port 5000
2.19 — Configure Backend Development Command

Now open:

📍
server/package.json

You'll find a section similar to:

"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1"
}

Replace only the scripts section with:

"scripts": {
  "dev": "tsx watch src/index.ts"
}

Don't replace the entire package.json.

Only modify scripts.

2.20 — Start Backend

Go back to your server terminal.

📍 Make sure terminal shows:
PS D:\Projects\mern-master-project\server>

Run:

npm run dev

Expected terminal output:

Server running on port 5000

Now open your browser and visit:

http://localhost:5000/api/health

You should see:

{
  "success": true,
  "message": "Backend is running successfully"
}
✅ Checkpoint 2

Now:

Frontend
React + TypeScript
localhost:5173
        ✅

Backend
Node + Express + TypeScript
localhost:5000
        ✅
📁 Your Exact Project Structure Now

Compare your VS Code Explorer with this:

mern-master-project/
│
├── client/
│   │
│   ├── node_modules/
│   ├── public/
│   │
│   ├── src/
│   │   ├── App.tsx
│   │   ├── main.tsx
│   │   └── ...
│   │
│   ├── package.json
│   ├── package-lock.json
│   ├── tsconfig.json
│   └── vite.config.ts
│
├── server/
│   │
│   ├── node_modules/
│   │
│   ├── src/
│   │   └── index.ts
│   │
│   ├── package.json
│   ├── package-lock.json
│   └── tsconfig.json
│
└── .gitignore

Some additional Vite-generated files are normal.

🧠 What Have We Actually Done?

You now have two separate applications.

Frontend application

Located at:

mern-master-project/client

Runs on:

localhost:5173

Technology:

React
TypeScript
Vite
Backend application

Located at:

mern-master-project/server

Runs on:

localhost:5000

Technology:

Node.js
Express
TypeScript

Right now:

React              Express
5173                 5000

  ○                   ○

They are running,
but we haven't connected them yet.

That's intentional.


Topic 2.20 — Connect React to Express

Now we connect your frontend and backend for the first time.

Goal

We want this flow:

React :5173
   ↓
GET /api/health
   ↓
Express :5000
   ↓
JSON response
   ↓
React displays:
Backend Connected Successfully
Step 1 — Keep backend running

Open one VS Code terminal.

Run from:

D:\Fullstack-MERN\MERN-master-project\server

Command:

npm run dev

You should see:

Server running on port 5000

Now test this in the browser:

http://localhost:5000/api/health

Expected response:

{
  "success": true,
  "message": "Backend is running successfully"
}

If this works, backend is ready.

Step 2 — Keep frontend running

Open a second VS Code terminal.

Run from:

D:\Fullstack-MERN\MERN-master-project\client

Command:

npm run dev

You should see:

http://localhost:5173

So now you have:

Terminal 1
server/
npm run dev
→ localhost:5000

Terminal 2
client/
npm run dev
→ localhost:5173
Step 3 — Modify React App.tsx

Open this exact file:

D:\Fullstack-MERN\MERN-master-project
└── client
    └── src
        └── App.tsx

Replace the current code with:

import { useEffect, useState } from "react";

function App() {
  const [message, setMessage] = useState("Checking backend...");

  useEffect(() => {
    const checkBackend = async () => {
      try {
        const response = await fetch("http://localhost:5000/api/health");

        const data = await response.json();

        setMessage(data.message);
      } catch (error) {
        setMessage("Backend connection failed");
      }
    };

    checkBackend();
  }, []);

  return (
    <div>
      <h1>MERN Master Project</h1>
      <p>{message}</p>
    </div>
  );
}

export default App;

Save the file.

What this code is doing

Don't worry about mastering React yet.

For now, understand only the flow.

This:

const [message, setMessage] = useState("Checking backend...");

creates a value called:

message

Initially:

Checking backend...

This:

useEffect(() => {

means:

Run some code after this component loads.

We'll properly learn useEffect later.

This is the important full-stack line:

fetch("http://localhost:5000/api/health")

React is sending an HTTP request to your Express backend.

Conceptually:

React
   ↓
GET http://localhost:5000/api/health
   ↓
Express

Because fetch() uses GET by default.

Then:

const response = await fetch(...)

The server sends back an HTTP response.

Then:

const data = await response.json();

converts the JSON response into a JavaScript object.

Backend sends:

{
  "success": true,
  "message": "Backend is running successfully"
}

So data becomes roughly:

{
  success: true,
  message: "Backend is running successfully"
}

Then:

setMessage(data.message);

changes the React state.

So the screen should change from:

Checking backend...

to:

Backend is running successfully
Step 4 — Open frontend

Go to:

http://localhost:5173

You may expect:

MERN Master Project

Backend is running successfully

But there is a good chance you will instead see:

Backend connection failed

and the browser console may show a CORS error.

That is actually useful.

Why CORS can happen

Your frontend is running at:

http://localhost:5173

Backend:

http://localhost:5000

These are different origins because the ports are different.

Browser sees:

5173 ≠ 5000

So the backend must explicitly allow the frontend to access it.

This brings us naturally to:

Topic 2.21 — CORS
Step 5 — Install CORS package

Go to your backend terminal.

Make sure you're here:

D:\Fullstack-MERN\MERN-master-project\server

Run:

npm install cors

Because we're using TypeScript, also install its type definitions:

npm install -D @types/cors
Step 6 — Update backend index.ts

Open:

D:\Fullstack-MERN\MERN-master-project
└── server
    └── src
        └── index.ts

Change it to:

import express from "express";
import cors from "cors";

const app = express();

const PORT = 5000;

app.use(
  cors({
    origin: "http://localhost:5173",
  })
);

app.get("/api/health", (req, res) => {
  res.json({
    success: true,
    message: "Backend is running successfully",
  });
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});

Because you're running:

tsx watch src/index.ts

the backend should restart automatically when you save.

What does this mean?

This code:

app.use(
  cors({
    origin: "http://localhost:5173",
  })
);

means:

Allow requests coming from our React frontend running on port 5173.

So now:

React :5173
    │
    │ allowed by CORS
    ▼
Express :5000
Step 7 — Test again

Refresh:

http://localhost:5173

You should now see:

MERN Master Project

Backend is running successfully

If you see that, congratulations:

Your React frontend has successfully called your Express backend.

This is your first real frontend ↔ backend integration.

The actual flow you just built
Browser
   ↓
React App
   ↓
useEffect
   ↓
fetch()
   ↓
GET http://localhost:5000/api/health
   ↓
CORS check
   ↓
Express route
   ↓
res.json()
   ↓
HTTP response
   ↓
response.json()
   ↓
setMessage()
   ↓
React re-renders
   ↓
Browser shows backend message

This is much more important than memorizing a definition of fetch().

One small improvement

Right now we're hardcoding:

"http://localhost:5000/api/health"

That's okay for this lesson.

But in a real application, we generally don't want API URLs scattered throughout the source code.

Very soon we'll change this into an environment variable like:

VITE_API_URL=http://localhost:5000

Then:

fetch(`${import.meta.env.VITE_API_URL}/api/health`)

That's Topic 2.22 — Environment Variables.





Topic 2.22 — Environment Variables

We're continuing exactly where we stopped.

You currently have:

React :5173
     │
     │ GET /api/health
     ▼
Express :5000
     │
     ▼
Backend is running successfully

There is one problem with our frontend code.

We wrote:

fetch("http://localhost:5000/api/health");

http://localhost:5000 is hardcoded.

In a real project, the backend address changes between environments:

LOCAL
http://localhost:5000

STAGING
https://staging-api.example.com

PRODUCTION
https://api.example.com

We don't want to search through our React code and manually replace URLs.

That's where environment variables come in.

2.22.1 — Frontend Environment Variable
Step 1 — Create .env

📍 Create it inside client, NOT inside src.

Exact location:

MERN-master-project/
│
├── client/
│   ├── src/
│   ├── .env             ← CREATE HERE
│   ├── package.json
│   └── vite.config.ts
│
└── server/

In VS Code:

Right-click client → New File → .env

Put this inside:

VITE_API_URL=http://localhost:5000
Why VITE_?

Our frontend uses Vite.

Vite exposes client environment variables prefixed with:

VITE_

So:

VITE_API_URL=http://localhost:5000

can be accessed from React.

Step 2 — Modify App.tsx

📍 Open:

MERN-master-project/
└── client/
    └── src/
        └── App.tsx

Currently you have something like:

const response = await fetch(
  "http://localhost:5000/api/health"
);

Change only that part to:

const response = await fetch(
  `${import.meta.env.VITE_API_URL}/api/health`
);

Your complete file should now look like:

import { useEffect, useState } from "react";

function App() {
  const [message, setMessage] = useState("Checking backend...");

  useEffect(() => {
    const checkBackend = async () => {
      try {
        const response = await fetch(
          `${import.meta.env.VITE_API_URL}/api/health`
        );

        const data = await response.json();

        setMessage(data.message);
      } catch (error) {
        setMessage("Backend connection failed");
      }
    };

    checkBackend();
  }, []);

  return (
    <div>
      <h1>MERN Master Project</h1>
      <p>{message}</p>
    </div>
  );
}

export default App;
Step 3 — Restart Vite

This is important.

Environment-variable changes often require restarting the development server.

Go to your client terminal:

D:\Fullstack-MERN\MERN-master-project\client

Stop it:

Ctrl + C

Then start again:

npm run dev

Open your frontend again.

You should still see:

MERN Master Project

Backend is running successfully

So nothing appears different to the user.

But internally we've improved the application:

Before
fetch("http://localhost:5000/api/health");
After
fetch(`${import.meta.env.VITE_API_URL}/api/health`);

and configuration lives separately:

VITE_API_URL=http://localhost:5000
⚠️ Very Important Security Concept

You might think:

".env means secret."

Not necessarily.

Anything exposed to frontend JavaScript can ultimately be inspected by the user.

Therefore, don't put secrets such as these in a Vite client environment variable:

❌ Database passwords
❌ JWT signing secrets
❌ Private API keys
❌ Payment secret keys

For example:

VITE_API_URL=http://localhost:5000

is fine because the API URL isn't a secret.

But something like:

VITE_DATABASE_PASSWORD=mySecretPassword

would be wrong.

We'll handle backend secrets separately.

2.22.2 — Backend Environment Variables

We also currently have this in:

server/src/index.ts
const PORT = 5000;

Let's move configuration outside our source code.

Step 4 — Install dotenv

📍 Open the server terminal.

Make sure you're here:

D:\Fullstack-MERN\MERN-master-project\server

Run:

npm install dotenv
Step 5 — Create Server .env

Create:

MERN-master-project/
│
├── client/
│   └── .env
│
└── server/
    ├── src/
    ├── .env             ← CREATE HERE
    ├── package.json
    └── tsconfig.json

Inside:

PORT=5000

Soon this same file will also contain our MongoDB connection configuration.

Step 6 — Load the environment variables

Open:

server/src/index.ts

Add:

import "dotenv/config";

at the top.

Your file becomes:

import "dotenv/config";
import express from "express";
import cors from "cors";

const app = express();

const PORT = process.env.PORT || 5000;

app.use(
  cors({
    origin: "http://localhost:5173",
  })
);

app.get("/api/health", (req, res) => {
  res.json({
    success: true,
    message: "Backend is running successfully",
  });
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
What's process.env.PORT?

Node makes environment variables available through:

process.env

We created:

PORT=5000

So:

process.env.PORT

returns approximately:

"5000"

And:

const PORT = process.env.PORT || 5000;

means:

Is PORT configured?
      │
   YES│       NO
      ▼        ▼
use PORT     use 5000

We'll improve configuration handling later when we build the production architecture.

2.22.3 — Verify .env Is Ignored by Git

This is extremely important.

Your root .gitignore should already contain:

node_modules/
.env
dist/

Because .gitignore patterns apply recursively here, your:

client/.env
server/.env

should be ignored.

Let's verify.

📍 Go to the ROOT terminal:

D:\Fullstack-MERN\MERN-master-project

Run:

git status

You should NOT see:

client/.env
server/.env

in the changes.

If either .env appears in git status, do not commit yet.

2.22.4 — But How Does Another Developer Know What Variables Are Needed?

Great production habit:

We don't commit:

.env

But we do commit:

.env.example

Create:

client/.env.example

with:

VITE_API_URL=http://localhost:5000

And create:

server/.env.example

with:

PORT=5000

Later our server example will become something like:

PORT=5000
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret

Notice:

variable names are shown, real secrets are not.

🧠 Quick Revision

You only need to remember this:

SOURCE CODE
     │
     │ reads
     ▼
ENVIRONMENT VARIABLES
Frontend — Vite
VITE_API_URL=http://localhost:5000

Read using:

import.meta.env.VITE_API_URL
Backend — Node
PORT=5000

Read using:

process.env.PORT
Git
.env            ❌ DON'T COMMIT

.env.example    ✅ COMMIT

That's enough theory.



Topic 2.22 — MongoDB + Mongoose Setup
Goal

By the end of this section:

React :5173
      ↓
Express :5000
      ↓
Mongoose
      ↓
MongoDB
      ↓
✅ Database Connected

For now, we're only establishing the database connection. We will create the Employee model and CRUD immediately afterward.

2.22.1 — MongoDB vs Mongoose

Keep this very simple.

MongoDB = our database.

It stores documents such as:

{
  "_id": "68abc...",
  "name": "Rahul",
  "email": "rahul@test.com",
  "department": "Development"
}

Mongoose = a Node.js library that makes working with MongoDB easier.

Our backend flow will become:

Express
   ↓
Mongoose
   ↓
MongoDB

You'll learn schemas, models, validation, queries, indexes, aggregation, etc. properly in the MongoDB phase.

Right now, we only need to connect.

2.22.2 — Which MongoDB Are We Going to Use?

For this master learning project, I recommend MongoDB Atlas.

That means MongoDB runs in the cloud instead of requiring the database server to run on your Windows machine.

We'll have:

YOUR COMPUTER

React
   ↓
Express
   ↓
Internet
   ↓
MongoDB Atlas
   ↓
Database

You can still use MongoDB Compass to visually inspect the database.

2.22.3 — Create MongoDB Atlas Account/Project

Go to MongoDB Atlas.

Sign in/create an account.

Then create a project.

You can name it:

MERN Fullstack Mastery

Then create a database deployment using the available free option suitable for learning.

Atlas's UI can change, so don't worry if the exact button wording differs slightly.

2.22.4 — Create Database User

Atlas will require database credentials.

Create a database user.

For example:

Username:
mern_user

Password:
<generate a strong password>

⚠️ Do not send me your actual password.

And don't put it directly inside index.ts.

It will eventually live in:

server/.env

which Git ignores.

2.22.5 — Configure Network Access

Atlas also controls which IP addresses may connect to your database.

For development, add your current IP address using Atlas's network-access setup.

Atlas also offers broader access options, but allowing only the necessary IPs is the better habit.

2.22.6 — Get Your Connection String

In Atlas, find the option to Connect your database and choose the application/driver connection option.

You should get something resembling:

mongodb+srv://mern_user:<password>@cluster-name.mongodb.net/

Again:

Do not paste your real connection string into this chat, because it contains database credentials.

2.22.7 — Add Database Name

Let's call our learning database:

mern_master

Your URI conceptually becomes:

mongodb+srv://USERNAME:PASSWORD@CLUSTER/mern_master

MongoDB can create this database when we first store data, so don't be concerned if you can't see mern_master in Atlas yet.

2.22.8 — Put URI in Backend .env

📍 Open exactly:

D:\Fullstack-MERN\MERN-master-project
└── server
    └── .env

You currently have:

PORT=5000

Add:

PORT=5000
MONGODB_URI=YOUR_REAL_CONNECTION_STRING

For example structurally:

PORT=5000
MONGODB_URI=mongodb+srv://USERNAME:PASSWORD@CLUSTER/mern_master

Use your actual Atlas connection string, not my placeholder.

Important

If your password contains characters such as:

@
:
/
?
#
%

they may need URL encoding in a connection URI.

Using Atlas's generated connection details carefully helps avoid this issue.

2.22.9 — Update .env.example

📍 Open:

server/.env.example

Change it to:

PORT=5000
MONGODB_URI=your_mongodb_connection_string

Notice the difference:

server/.env

REAL credentials
❌ Git ignored


server/.env.example

Placeholder
✅ Git committed
2.22.10 — Install Mongoose

Open your server terminal.

📍 Make sure you're here:

PS D:\Fullstack-MERN\MERN-master-project\server>

Run:

npm install mongoose

Don't install:

@types/mongoose

Modern Mongoose already provides its TypeScript definitions.

2.22.11 — Don't Put Everything in index.ts

We could write:

mongoose.connect(...)

directly inside index.ts.

It would work.

But we're learning how to structure a real project.

Let's introduce our first backend organization.

Inside:

server/src

create a folder:

config

Then create:

db.ts

Exact structure:

MERN-master-project/
└── server/
    └── src/
        ├── config/
        │   └── db.ts          ← CREATE
        │
        └── index.ts
2.22.12 — Write Database Connection Function

📍 Open:

server/src/config/db.ts

Add:

import mongoose from "mongoose";

export const connectDatabase = async (): Promise<void> => {
  try {
    const mongoUri = process.env.MONGODB_URI;

    if (!mongoUri) {
      throw new Error("MONGODB_URI is not defined");
    }

    await mongoose.connect(mongoUri);

    console.log("MongoDB connected successfully");
  } catch (error) {
    console.error("MongoDB connection failed:", error);

    process.exit(1);
  }
};

Let's understand this before using it.

2.22.13 — process.env.MONGODB_URI

Remember our:

server/.env

contains:

MONGODB_URI=...

Node can access it using:

process.env.MONGODB_URI

So:

const mongoUri = process.env.MONGODB_URI;

takes the MongoDB connection string from the environment.

2.22.14 — Why Check !mongoUri?

We wrote:

if (!mongoUri) {
  throw new Error("MONGODB_URI is not defined");
}

Suppose somebody clones your project but forgets to create:

server/.env

Instead of getting a confusing database error later, our application clearly says:

MONGODB_URI is not defined

This is called failing fast.

2.22.15 — Actual Database Connection

This is the important line:

await mongoose.connect(mongoUri);

Conceptually:

Node application
      ↓
mongoose.connect()
      ↓
Connection URI
      ↓
MongoDB Atlas

Because connecting is asynchronous, we use:

await

We'll deeply understand async/await during JavaScript.

2.22.16 — Why process.exit(1)?

If our application cannot connect to its required database, we don't want to pretend everything is healthy.

process.exit(1);

terminates the Node process with an error status.

For now:

0 → normal/successful termination

1 → failure

That's enough.

2.22.17 — Connect Database When Server Starts

Now open:

server/src/index.ts

Change it to:

import "dotenv/config";
import express from "express";
import cors from "cors";

import { connectDatabase } from "./config/db.js";

const app = express();

const PORT = process.env.PORT || 5000;

app.use(
  cors({
    origin: "http://localhost:5173",
  })
);

app.use(express.json());

app.get("/api/health", (req, res) => {
  res.json({
    success: true,
    message: "Backend is running successfully",
  });
});

const startServer = async () => {
  await connectDatabase();

  app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
  });
};

startServer();

There are two important changes.

First:

import { connectDatabase } from "./config/db.js";

We're importing our database connection function.

Second:

const startServer = async () => {
  await connectDatabase();

  app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
  });
};

We're deliberately connecting to MongoDB before starting the HTTP server.

So startup becomes:

Start application
       ↓
Load environment variables
       ↓
Connect MongoDB
       ↓
Connection successful?
       ↓
      YES
       ↓
Start Express
       ↓
Listen on port 5000

If MongoDB fails:

MongoDB connection
       ↓
      FAIL
       ↓
process.exit(1)
       ↓
Server doesn't start

That's a much cleaner startup behavior.

One New Line You Saw

We also added:

app.use(express.json());

This middleware allows Express to parse JSON request bodies.

Very soon React will send:

{
  "name": "Rahul",
  "email": "rahul@test.com"
}

Without JSON-body parsing, our API wouldn't conveniently get that object through req.body.

We'll understand middleware properly in the Express phase.

2.22.18 — Run the Backend

📍 Terminal:

D:\Fullstack-MERN\MERN-master-project\server

Run:

npm run dev

If everything is correct, you should see:

MongoDB connected successfully
Server running on port 5000

🎯 That is our target output.

Your architecture has now become:

              FRONTEND
         React + TypeScript
               :5173
                 │
                 │ HTTP
                 ▼
              BACKEND
      Node + Express + TypeScript
               :5000
                 │
                 │ Mongoose
                 ▼
              DATABASE
            MongoDB Atlas

We officially have all major pieces of MERN connected.

Don't commit yet

We're going to make this more meaningful before committing.

Our next section is:

Topic 2.23 — First Employee Model + MongoDB Save

We'll create:

server/src/
│
├── config/
│   └── db.ts
│
├── models/
│   └── employee.model.ts
│
└── index.ts

Then your first real database flow:

POST /api/employees
        ↓
Express receives JSON
        ↓
Create Employee
        ↓
Mongoose
        ↓
MongoDB Atlas
        ↓
201 Created

After that we'll open Atlas/Compass and actually see the employee document you created.


Topic 2.23 — First Employee Model

Now we're going to store real data in MongoDB for the first time.

We currently have:

React
  ↓
Express
  ↓
Mongoose
  ↓
MongoDB Atlas

Now we'll build:

POST /api/employees
        ↓
Express
        ↓
Employee Model
        ↓
Mongoose
        ↓
MongoDB
        ↓
Employee saved
        ↓
201 Created

For this first implementation, we'll keep the architecture deliberately simple. Later, during the Express phase, we'll refactor into routes → controllers → services → models.

2.23.1 — What is a Mongoose Schema and Model?

Very short explanation.

Suppose an employee should contain:

Name
Email
Department
Role
Status

A Schema describes what an employee document should look like.

Conceptually:

Employee
│
├── name        → string, required
├── email       → string, required
├── department  → string, required
├── role        → string, required
└── status      → string

A Model gives our Node application a way to work with those employee documents.

For example, later we'll be able to do:

Employee.create(...)

and:

Employee.find()

Think:

Schema
   ↓
"What does Employee data look like?"

Model
   ↓
"Let me create/read/update/delete Employees."

MongoDB
   ↓
"Store the actual Employee documents."

That's enough theory.

2.23.2 — Create models Folder

Go to VS Code.

Inside:

server/src

create a new folder:

models
Exact location
MERN-master-project/
│
└── server/
    └── src/
        │
        ├── config/
        │   └── db.ts
        │
        ├── models/                 ← CREATE
        │
        └── index.ts
2.23.3 — Create Employee Model File

Right-click:

server/src/models

Create:

employee.model.ts

Now:

server/
└── src/
    ├── config/
    │   └── db.ts
    │
    ├── models/
    │   └── employee.model.ts      ← HERE
    │
    └── index.ts
2.23.4 — Create Employee Type

📍 Open:

server/src/models/employee.model.ts

Add:

import mongoose, { Schema } from "mongoose";

interface IEmployee {
  name: string;
  email: string;
  department: string;
  role: string;
  status: "active" | "inactive";
}

Don't worry about mastering this TypeScript syntax now.

We will cover it deeply in the TypeScript phase.

For now understand:

interface IEmployee

describes the TypeScript shape of an employee.

For example, this is valid:

{
  name: "Rahul",
  email: "rahul@test.com",
  department: "Engineering",
  role: "Developer",
  status: "active"
}

But:

status: "sleeping"

doesn't match our intended TypeScript type.

Because we allowed only:

"active" | "inactive"
2.23.5 — Create Mongoose Schema

Below the interface add:

const employeeSchema = new Schema<IEmployee>(
  {
    name: {
      type: String,
      required: true,
      trim: true,
    },

    email: {
      type: String,
      required: true,
      unique: true,
      lowercase: true,
      trim: true,
    },

    department: {
      type: String,
      required: true,
      trim: true,
    },

    role: {
      type: String,
      required: true,
      trim: true,
    },

    status: {
      type: String,
      enum: ["active", "inactive"],
      default: "active",
    },
  },
  {
    timestamps: true,
  }
);

Let's understand the important parts.

type: String
name: {
  type: String
}

means Mongo/Mongoose expects:

name → String

For example:

Rahul
Priya
Amar
required: true
required: true

means this field must be provided.

So:

{
  "name": "Rahul"
}

has a name.

But if a required field is missing, Mongoose validation can reject the document.

trim: true

Suppose someone sends:

"    Rahul    "

trim removes leading/trailing whitespace, resulting in:

"Rahul"
unique: true

For email:

unique: true

helps establish a unique index so duplicate email values are not allowed by MongoDB once the index exists.

We'll later learn an important interview point:

unique: true is not the same thing as a normal Mongoose validator.

We'll handle duplicate-key errors properly when we build production APIs.

lowercase: true

Suppose the frontend sends:

RAHUL@TEST.COM

Mongoose can store:

rahul@test.com

This helps normalize email values.

enum

We wrote:

enum: ["active", "inactive"]

Therefore we expect:

active

or:

inactive

rather than arbitrary status values.

default
default: "active"

means if we don't send:

{
  "status": "active"
}

Mongoose will use active by default.

timestamps

We wrote:

{
  timestamps: true
}

Mongoose automatically manages:

createdAt
updatedAt

So we don't need to manually create them.

2.23.6 — Create Employee Model

At the bottom of the same file add:

export const Employee = mongoose.model<IEmployee>(
  "Employee",
  employeeSchema
);

Your complete file should now be:

import mongoose, { Schema } from "mongoose";

interface IEmployee {
  name: string;
  email: string;
  department: string;
  role: string;
  status: "active" | "inactive";
}

const employeeSchema = new Schema<IEmployee>(
  {
    name: {
      type: String,
      required: true,
      trim: true,
    },

    email: {
      type: String,
      required: true,
      unique: true,
      lowercase: true,
      trim: true,
    },

    department: {
      type: String,
      required: true,
      trim: true,
    },

    role: {
      type: String,
      required: true,
      trim: true,
    },

    status: {
      type: String,
      enum: ["active", "inactive"],
      default: "active",
    },
  },
  {
    timestamps: true,
  }
);

export const Employee = mongoose.model<IEmployee>(
  "Employee",
  employeeSchema
);
🧠 What Have We Created?

We haven't created an employee yet.

We've created the definition/model that our application will use.

IEmployee
     │
     │ TypeScript
     ▼
Developer-side type safety


employeeSchema
     │
     │ Mongoose
     ▼
Database document structure/configuration


Employee Model
     │
     ▼
Employee.create()
Employee.find()
Employee.findById()
...
2.23.7 — Create Our First POST API

For this first learning example, we'll put the API directly in index.ts.

📍 Open:

server/src/index.ts

First import our Employee model:

import { Employee } from "./models/employee.model.js";

Place it with your other imports.

Now, below the health route and before startServer, add:

app.post("/api/employees", async (req, res) => {
  try {
    const employee = await Employee.create(req.body);

    res.status(201).json({
      success: true,
      message: "Employee created successfully",
      data: employee,
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      message: "Failed to create employee",
    });
  }
});
2.23.8 — Understand the Flow

When someone sends:

POST /api/employees

with:

{
  "name": "Rahul Sharma",
  "email": "rahul@test.com",
  "department": "Engineering",
  "role": "Developer"
}

Express gives us the request body through:

req.body

because earlier we added:

app.use(express.json());

Then:

Employee.create(req.body);

means:

Create an Employee document using this request data and save it to MongoDB.

The flow is:

POST /api/employees
        ↓
Express
        ↓
req.body
        ↓
Employee.create()
        ↓
Mongoose Schema
        ↓
MongoDB
        ↓
Document created
2.23.9 — Why 201?

We wrote:

res.status(201)

201 Created means:

The request successfully created a new resource.

So:

200 → Successful request

201 → Resource successfully created

You'll learn status codes properly later.

2.23.10 — Test with Postman

We're deliberately testing the backend before building the React form.

This is a very useful development habit:

Build API
   ↓
Test API independently
   ↓
Confirm backend works
   ↓
Connect frontend

Open Postman.

Create a new request.

Method:

POST

URL:

http://localhost:5000/api/employees

Choose:

Body → raw → JSON

Enter:

{
  "name": "Rahul Sharma",
  "email": "rahul@test.com",
  "department": "Engineering",
  "role": "Developer"
}

Click Send.

Expected Response

You should receive something similar to:

{
  "success": true,
  "message": "Employee created successfully",
  "data": {
    "name": "Rahul Sharma",
    "email": "rahul@test.com",
    "department": "Engineering",
    "role": "Developer",
    "status": "active",
    "_id": "...",
    "createdAt": "...",
    "updatedAt": "...",
    "__v": 0
  }
}

Notice something important.

We did not send:

{
  "status": "active"
}

But MongoDB contains it because our schema has:

default: "active"

We also didn't send:

_id
createdAt
updatedAt

Those were generated for us.

2.23.11 — Check MongoDB Atlas

Now go back to MongoDB Atlas and open your database/browse collections view.

After the first successful insert, you should find something conceptually like:

mern_master
     │
     └── employees
             │
             └── Rahul Sharma

Why employees?

Our model is:

mongoose.model("Employee", ...)

Mongoose normally maps that model to a pluralized collection name:

Employee
   ↓
employees

Inside it, you should see your actual document.

🎯 This is an important milestone.

You have now manually sent:

Postman
   ↓
HTTP POST
   ↓
Express
   ↓
Mongoose
   ↓
MongoDB Atlas

and persisted real data.

⚠️ One Important Thing

Our API is not production quality yet.

For example:

Employee.create(req.body)

blindly passing the entire request body isn't how I want our eventual production architecture to remain.

And:

catch {
   return 500
}

for every error is too simplistic.

We're intentionally doing this first so you understand the flow.

Later we'll introduce:

Route
  ↓
Validation
  ↓
Controller
  ↓
Service
  ↓
Model / Repository
  ↓
Database

with proper:

400 validation errors
409 duplicate conflicts
404 not found
500 unexpected server errors

First understand the machine. Then engineer it properly.

Topic 2 — GET Employees from MongoDB

We continue from your existing project. Do not create a new project or new model.

Right now CREATE works:

POST /api/employees
        ↓
Express
        ↓
Employee.create()
        ↓
MongoDB
        ↓
✅ Employee saved

Now we'll implement READ:

GET /api/employees
        ↓
Express
        ↓
Employee.find()
        ↓
MongoDB
        ↓
Employees returned
Step 1 — Open the exact backend file

Open:

D:\Fullstack-MERN\MERN-master-project
└── server
    └── src
        └── index.ts        ← OPEN THIS

You should already have your Employee import near the top:

import { Employee } from "./models/employee.model.js";

Do not import it again.

Step 2 — Add the GET Employees API

Find your existing POST route:

app.post("/api/employees", async (req, res) => {

After the complete POST route, add:

app.get("/api/employees", async (req, res) => {
  try {
    const employees = await Employee.find();

    res.status(200).json({
      success: true,
      message: "Employees fetched successfully",
      data: employees,
    });
  } catch (error) {
    console.error("Get employees error:", error);

    res.status(500).json({
      success: false,
      message: "Failed to fetch employees",
    });
  }
});

So conceptually your index.ts now contains:

Imports

↓

Express setup

↓

Middleware

↓

GET /api/health

↓

POST /api/employees      ← CREATE

↓

GET /api/employees       ← READ (NEW)

↓

startServer()

↓

MongoDB connection

↓

Express starts
Step 3 — Understand the important line

This is today's most important line:

const employees = await Employee.find();

Remember our Mongoose model:

Employee

It represents our employee collection.

.find() means:

Find the employee documents.

Because we haven't supplied any filter:

Employee.find()

means roughly:

Give me all employees.

Think of it like:

Employee Model
      ↓
    .find()
      ↓
MongoDB employees collection
      ↓
Employee 1
Employee 2
Employee 3
...

Later you'll write things like:

Employee.find({ status: "active" });

which means:

Give me only active employees.

But don't jump there yet.

Step 4 — Why await?

Database operations take time.

Node asks MongoDB:

"Give me the employees."

MongoDB needs to perform the query and return the result.

Therefore:

const employees = await Employee.find();

means conceptually:

Send database query
       ↓
Wait for result
       ↓
Receive employees
       ↓
Store them in employees

We'll learn async/await properly in JavaScript. For now, understand its role in this flow.

Step 5 — Send the response

After MongoDB returns the employees, we send:

res.status(200).json({
  success: true,
  message: "Employees fetched successfully",
  data: employees,
});

200 means:

200 OK
→ Request succeeded

And:

data: employees

contains the documents MongoDB returned.

Step 6 — Make sure backend is running

Open your backend terminal.

📍 Exact location:

D:\Fullstack-MERN\MERN-master-project\server

If necessary:

cd D:\Fullstack-MERN\MERN-master-project\server

Run:

npm run dev

You should see:

MongoDB connected successfully
Server running on port 5000
Step 7 — Test GET in Postman

Open Postman.

Create/request:

Method:
GET

URL:
http://localhost:5000/api/employees

For GET, you do not need to provide a request body.

Click Send.

Step 8 — Expected Result

Because you previously saved an employee, you should receive something similar to:

{
  "success": true,
  "message": "Employees fetched successfully",
  "data": [
    {
      "_id": "your-generated-id",
      "name": "Rahul Sharma",
      "email": "rahul@test.com",
      "department": "Engineering",
      "role": "Developer",
      "status": "active",
      "createdAt": "2026-08-12T...",
      "updatedAt": "2026-08-12T...",
      "__v": 0
    }
  ]
}

Notice:

data: [
       ...
      ]

It's an array.

Why?

Because:

Employee.find()

can return:

0 employees
1 employee
100 employees

Therefore, the result is a collection/array.

If MongoDB contains no employees, you would normally get:

{
  "success": true,
  "message": "Employees fetched successfully",
  "data": []
}

That's not an error. The query succeeded; there simply weren't any matching documents.

Step 9 — Understand POST vs GET

You have now implemented two CRUD operations:

             EMPLOYEE API

CREATE                         READ
   │                             │
   │ POST                        │ GET
   ▼                             ▼
/api/employees              /api/employees
   │                             │
   ▼                             ▼
Employee.create()           Employee.find()
   │                             │
   ▼                             ▼
MongoDB                     MongoDB
   │                             │
   ▼                             ▼
Save employee               Get employees
   │                             │
   ▼                             ▼
201 Created                 200 OK

This distinction is extremely important.

POST
Employee.create(req.body);

Writes data.

GET
Employee.find();

Reads data.


Topic 2 — Show Employees in React

Now that this works:

GET /api/employees
        ↓
Express
        ↓
Employee.find()
        ↓
MongoDB
        ↓
JSON response

we will complete the other half:

MongoDB
   ↓
Express
   ↓
GET /api/employees
   ↓
React
   ↓
Display employee list
Step 1 — Open the exact frontend file

Open:

D:\Fullstack-MERN\MERN-master-project
└── client
    └── src
        └── App.tsx

Replace your current App.tsx with this:

import { useEffect, useState } from "react";

interface Employee {
  _id: string;
  name: string;
  email: string;
  department: string;
  role: string;
  status: "active" | "inactive";
}

function App() {
  const [employees, setEmployees] = useState<Employee[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState("");

  useEffect(() => {
    const fetchEmployees = async () => {
      try {
        const response = await fetch(
          `${import.meta.env.VITE_API_URL}/api/employees`
        );

        if (!response.ok) {
          throw new Error("Failed to fetch employees");
        }

        const result = await response.json();

        setEmployees(result.data);
      } catch (error) {
        console.error("Fetch employees error:", error);

        setError("Could not load employees");
      } finally {
        setLoading(false);
      }
    };

    fetchEmployees();
  }, []);

  return (
    <div>
      <h1>Employee Management</h1>

      {loading && <p>Loading employees...</p>}

      {error && <p>{error}</p>}

      {!loading && !error && employees.length === 0 && (
        <p>No employees found.</p>
      )}

      {!loading && !error && employees.length > 0 && (
        <div>
          {employees.map((employee) => (
            <div key={employee._id}>
              <h3>{employee.name}</h3>

              <p>Email: {employee.email}</p>

              <p>Department: {employee.department}</p>

              <p>Role: {employee.role}</p>

              <p>Status: {employee.status}</p>

              <hr />
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

export default App;
Step 2 — Make sure both applications are running
Backend terminal

Exact location:

D:\Fullstack-MERN\MERN-master-project\server

Run:

npm run dev

You should see:

MongoDB connected successfully
Server running on port 5000
Frontend terminal

Exact location:

D:\Fullstack-MERN\MERN-master-project\client

Run:

npm run dev

Open:

http://localhost:5173
Step 3 — Expected Result

If MongoDB currently contains:

{
  "name": "Rahul Sharma",
  "email": "rahul@test.com",
  "department": "Engineering",
  "role": "Developer",
  "status": "active"
}

your React page should show something like:

Employee Management

Rahul Sharma

Email: rahul@test.com
Department: Engineering
Role: Developer
Status: active

That means your data successfully travelled:

MongoDB
   ↓
Mongoose
   ↓
Express
   ↓
HTTP Response
   ↓
fetch()
   ↓
React State
   ↓
Browser UI
Now understand the code

We are not learning React deeply yet, so only understand what is necessary for this flow.

Employee interface
interface Employee {
  _id: string;
  name: string;
  email: string;
  department: string;
  role: string;
  status: "active" | "inactive";
}

This tells TypeScript:

This is what an Employee object should look like.

Later we'll move types like this out of App.tsx into proper files.

For now it's fine here.

employees state
const [employees, setEmployees] = useState<Employee[]>([]);

Initial value:

[]

Meaning:

We don't have employee data yet.

After the API call succeeds:

setEmployees(result.data);

React state might become:

[
  {
    _id: "123",
    name: "Rahul Sharma",
    email: "rahul@test.com",
    department: "Engineering",
    role: "Developer",
    status: "active"
  }
]
loading
const [loading, setLoading] = useState(true);

While React is waiting for the backend:

Loading employees...

After the request finishes:

setLoading(false);

This happens inside:

finally {
  setLoading(false);
}

finally runs whether the request succeeds or fails.

Why check response.ok?

We wrote:

if (!response.ok) {
  throw new Error("Failed to fetch employees");
}

This is important.

fetch() does not automatically throw an error just because the server returns something like:

404
500

So we inspect:

response.ok

It is generally true for successful 2xx HTTP responses.

If not:

throw new Error(...)

Then execution moves to:

catch
result.data

Our backend returns:

{
  "success": true,
  "message": "Employees fetched successfully",
  "data": [
    ...
  ]
}

So:

const result = await response.json();

gives us the whole response object.

Then:

result.data

is the employee array.

So:

setEmployees(result.data);

puts that employee array into React state.

map()

This:

employees.map((employee) => (

means:

For every employee in the array, create some UI.

If we had three employees:

Rahul
Priya
Arjun

React creates three employee blocks.

We'll learn map() deeply in JavaScript before relying on it heavily.

Why key={employee._id}?
<div key={employee._id}>

React needs a stable identity when rendering lists.

MongoDB already gives every employee:

_id

so that's a good unique key for this list.

We'll cover React keys properly in the React phase.

Your full READ flow

You have now built:

PAGE LOADS
    ↓
useEffect
    ↓
fetchEmployees()
    ↓
GET /api/employees
    ↓
Express
    ↓
Employee.find()
    ↓
MongoDB
    ↓
Employees
    ↓
Express JSON response
    ↓
response.json()
    ↓
setEmployees()
    ↓
React re-renders
    ↓
Employee list appears

That's a real full-stack read operation.

But we're not finished yet

Right now you've done CREATE using Postman and READ using React.

We still need the frontend to create the employee too.

Otherwise our flow is:

Postman
  ↓
POST
  ↓
MongoDB

MongoDB
  ↓
GET
  ↓
React

We want:

React Form
    ↓
POST
    ↓
Express
    ↓
MongoDB
    ↓
GET
    ↓
React
    ↓
Updated List

That will be our first complete browser-driven MERN feature.


Topic 2 — Complete React → API → MongoDB Flow

Now we finish the first browser-driven full-stack feature.

Right now:

POST employee        ✅ via Postman
GET employees        ✅ via React

Now we want:

React Form
   ↓
POST /api/employees
   ↓
Express
   ↓
MongoDB
   ↓
Employee saved
   ↓
React reloads employee list
Step 1 — Open the exact frontend file

Open:

D:\Fullstack-MERN\MERN-master-project
└── client
    └── src
        └── App.tsx

Replace the file with this:

import { useEffect, useState } from "react";

interface Employee {
  _id: string;
  name: string;
  email: string;
  department: string;
  role: string;
  status: "active" | "inactive";
}

interface EmployeeForm {
  name: string;
  email: string;
  department: string;
  role: string;
}

function App() {
  const [employees, setEmployees] = useState<Employee[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState("");

  const [form, setForm] = useState<EmployeeForm>({
    name: "",
    email: "",
    department: "",
    role: "",
  });

  const fetchEmployees = async () => {
    try {
      setLoading(true);
      setError("");

      const response = await fetch(
        `${import.meta.env.VITE_API_URL}/api/employees`
      );

      if (!response.ok) {
        throw new Error("Failed to fetch employees");
      }

      const result = await response.json();

      setEmployees(result.data);
    } catch (error) {
      console.error("Fetch employees error:", error);
      setError("Could not load employees");
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchEmployees();
  }, []);

  const handleChange = (
    event: React.ChangeEvent<HTMLInputElement>
  ) => {
    const { name, value } = event.target;

    setForm((previousForm) => ({
      ...previousForm,
      [name]: value,
    }));
  };

  const handleSubmit = async (
    event: React.FormEvent<HTMLFormElement>
  ) => {
    event.preventDefault();

    try {
      setError("");

      const response = await fetch(
        `${import.meta.env.VITE_API_URL}/api/employees`,
        {
          method: "POST",

          headers: {
            "Content-Type": "application/json",
          },

          body: JSON.stringify(form),
        }
      );

      if (!response.ok) {
        throw new Error("Failed to create employee");
      }

      setForm({
        name: "",
        email: "",
        department: "",
        role: "",
      });

      await fetchEmployees();
    } catch (error) {
      console.error("Create employee error:", error);
      setError("Could not create employee");
    }
  };

  return (
    <div>
      <h1>Employee Management</h1>

      <h2>Add Employee</h2>

      <form onSubmit={handleSubmit}>
        <div>
          <label>Name</label>

          <input
            type="text"
            name="name"
            value={form.name}
            onChange={handleChange}
          />
        </div>

        <div>
          <label>Email</label>

          <input
            type="email"
            name="email"
            value={form.email}
            onChange={handleChange}
          />
        </div>

        <div>
          <label>Department</label>

          <input
            type="text"
            name="department"
            value={form.department}
            onChange={handleChange}
          />
        </div>

        <div>
          <label>Role</label>

          <input
            type="text"
            name="role"
            value={form.role}
            onChange={handleChange}
          />
        </div>

        <button type="submit">
          Add Employee
        </button>
      </form>

      <hr />

      <h2>Employees</h2>

      {loading && <p>Loading employees...</p>}

      {error && <p>{error}</p>}

      {!loading && !error && employees.length === 0 && (
        <p>No employees found.</p>
      )}

      {!loading && employees.length > 0 && (
        <div>
          {employees.map((employee) => (
            <div key={employee._id}>
              <h3>{employee.name}</h3>

              <p>Email: {employee.email}</p>
              <p>Department: {employee.department}</p>
              <p>Role: {employee.role}</p>
              <p>Status: {employee.status}</p>

              <hr />
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

export default App;
Step 2 — Understand the new form state

We created:

const [form, setForm] = useState<EmployeeForm>({
  name: "",
  email: "",
  department: "",
  role: "",
});

This means React stores the form data as:

{
  name: "",
  email: "",
  department: "",
  role: ""
}

When you type:

Name: Amar
Email: amar@test.com
Department: Engineering
Role: Developer

React state becomes:

{
  name: "Amar",
  email: "amar@test.com",
  department: "Engineering",
  role: "Developer"
}
Step 3 — handleChange

We use one function for all four inputs:

const handleChange = (
  event: React.ChangeEvent<HTMLInputElement>
) => {
  const { name, value } = event.target;

  setForm((previousForm) => ({
    ...previousForm,
    [name]: value,
  }));
};

Don't worry about mastering this yet.

The important idea is:

Input changed
    ↓
Find input's name
    ↓
Find its value
    ↓
Update the matching field in React state

For example:

name="department"
value="Engineering"

updates:

form.department
Step 4 — Form submission

We have:

<form onSubmit={handleSubmit}>

When the user clicks:

Add Employee

React runs:

handleSubmit

First:

event.preventDefault();

prevents the normal browser form submission/page refresh.

Step 5 — The POST request

This is the most important part:

const response = await fetch(
  `${import.meta.env.VITE_API_URL}/api/employees`,
  {
    method: "POST",

    headers: {
      "Content-Type": "application/json",
    },

    body: JSON.stringify(form),
  }
);

Let's break it down.

URL
`${import.meta.env.VITE_API_URL}/api/employees`

becomes:

http://localhost:5000/api/employees
HTTP method
method: "POST"

means:

Create a new employee.

Header
headers: {
  "Content-Type": "application/json"
}

means:

The request body I'm sending contains JSON.

This is your first practical use of an HTTP header.

We'll study headers properly later.

Request body

React currently has:

form

which is a JavaScript object.

We convert it to JSON text using:

JSON.stringify(form)

So:

{
  name: "Amar",
  email: "amar@test.com"
}

becomes JSON data sent over HTTP.

Step 6 — Backend receives it

Your Express backend already has:

app.use(express.json());

So Express parses the JSON request and gives you:

req.body

Then your POST route does:

Employee.create(req.body);

So the flow is:

React form state
      ↓
JSON.stringify()
      ↓
HTTP POST
      ↓
Express
      ↓
express.json()
      ↓
req.body
      ↓
Employee.create()
      ↓
MongoDB
Step 7 — Clear the form

After successful creation:

setForm({
  name: "",
  email: "",
  department: "",
  role: "",
});

This resets the form inputs.

Step 8 — Refresh employees

Then:

await fetchEmployees();

means:

Employee was created. Now fetch the latest employees again.

So:

POST employee
     ↓
MongoDB saves it
     ↓
GET employees
     ↓
Latest list returned
     ↓
React updates

This is why the new employee appears without manually refreshing the browser.

Step 9 — Run everything
Backend terminal

From:

D:\Fullstack-MERN\MERN-master-project\server

run:

npm run dev

Expected:

MongoDB connected successfully
Server running on port 5000
Frontend terminal

From:

D:\Fullstack-MERN\MERN-master-project\client

run:

npm run dev

Open:

http://localhost:5173
Step 10 — Test the complete feature

Fill:

Name:
Amar Mishra

Email:
amar@test.com

Department:
Engineering

Role:
Developer

Click:

Add Employee

Expected:

Employee Management

Add Employee
...

Employees

Rahul Sharma
...

Amar Mishra
Email: amar@test.com
Department: Engineering
Role: Developer
Status: active

Then check MongoDB Atlas.

You should also see the new employee document there.

What you have now built

This is your first real browser-driven MERN feature:

USER
  ↓
React Form
  ↓
React State
  ↓
POST /api/employees
  ↓
Express
  ↓
Mongoose
  ↓
MongoDB
  ↓
Employee Saved
  ↓
GET /api/employees
  ↓
Express
  ↓
MongoDB
  ↓
JSON Response
  ↓
React State
  ↓
Employee List
  ↓
USER

This is the exact full-stack flow we discussed in Topic 1.

Now you've actually built it.


Topic 2 — Quick Revision
1. Project structure
MERN-master-project/
│
├── client/                 ← React + TypeScript frontend
│   ├── src/
│   ├── .env
│   └── package.json
│
├── server/                 ← Node + Express + TypeScript backend
│   ├── src/
│   │   ├── config/
│   │   │   └── db.ts
│   │   ├── models/
│   │   │   └── employee.model.ts
│   │   └── index.ts
│   ├── .env
│   └── package.json
│
└── .gitignore
2. Frontend

Our frontend runs using:

React
+
TypeScript
+
Vite

Development URL:

http://localhost:5173

React handles:

Form
↓
User input
↓
State
↓
API call
↓
Response
↓
Update UI
3. Backend

Our backend uses:

Node.js
+
Express
+
TypeScript

Development URL:

http://localhost:5000

We currently have:

GET  /api/health

POST /api/employees

GET  /api/employees
4. MongoDB

We use:

MongoDB Atlas

and Node communicates with MongoDB through:

Mongoose

Connection flow:

Express
   ↓
Mongoose
   ↓
MongoDB Atlas
5. Environment Variables

Frontend:

VITE_API_URL=http://localhost:5000

React reads it with:

import.meta.env.VITE_API_URL

Backend:

PORT=5000
MONGODB_URI=...

Node reads them using:

process.env.PORT
process.env.MONGODB_URI

Remember:

.env             ❌ Don't commit
.env.example     ✅ Commit
6. CORS

Frontend:

localhost:5173

Backend:

localhost:5000

Different ports mean different origins.

So Express allows our frontend through CORS:

app.use(
  cors({
    origin: "http://localhost:5173",
  })
);

For now remember:

CORS controls which origins can access your backend from a browser.

7. JSON Middleware

We added:

app.use(express.json());

Why?

React sends:

{
  "name": "Amar",
  "email": "amar@test.com"
}

Express converts the JSON request body so we can access:

req.body

Flow:

JSON Request
   ↓
express.json()
   ↓
req.body
8. Employee Schema

Our Mongoose schema defines employee data such as:

Employee

├── name
├── email
├── department
├── role
├── status
├── createdAt
└── updatedAt

Schema:

Defines how employee data should be structured/configured.

Model:

Allows us to work with employee documents.

Examples:

Employee.create(...)

and:

Employee.find()
9. CREATE Flow

User fills:

Name
Email
Department
Role

Then:

React Form
   ↓
React State
   ↓
JSON.stringify()
   ↓
POST /api/employees
   ↓
Express
   ↓
req.body
   ↓
Employee.create()
   ↓
Mongoose
   ↓
MongoDB

Important backend line:

const employee = await Employee.create(req.body);

Successful creation returns:

201 Created
10. READ Flow

React sends:

GET /api/employees

Backend runs:

const employees = await Employee.find();

Then:

MongoDB
   ↓
Mongoose
   ↓
Express
   ↓
JSON
   ↓
React

Successful GET:

200 OK
11. Complete Full-Stack Flow

This is the most important revision diagram for Topic 2:

                    USER
                     │
                     ▼
                 React Form
                     │
                     ▼
                React State
                     │
                     │ POST
                     ▼
               Express API
                     │
                     ▼
                Mongoose
                     │
                     ▼
                 MongoDB
                     │
                 SAVED ✅
                     │
                     ▼
                GET Request
                     │
                     ▼
               Express API
                     │
                     ▼
                Mongoose
                     │
                     ▼
                 MongoDB
                     │
                     ▼
               JSON Response
                     │
                     ▼
                 React State
                     │
                     ▼
                Employee List
                     │
                     ▼
                    USER

That is your first complete MERN feature.

12. Git Workflow We Used
Make changes
     ↓
git status
     ↓
git add .
     ↓
git commit
     ↓
git push
     ↓
GitHub

You should always check:

git status

before committing.

And secrets such as:

.env
MongoDB password
JWT secret

must never be pushed.

13. Commands Worth Remembering

Frontend:

cd client
npm run dev

Backend:

cd server
npm run dev

Git:

git status
git add .
git commit -m "message"
git push
14. Tiny Interview Notes

What is Vite?
Frontend development/build tooling used for our React application.

What is Express?
A Node.js framework used to create HTTP servers and APIs.

What is Mongoose?
A library used by our Node application to model and interact with MongoDB.

What is CORS?
A browser security mechanism that controls cross-origin requests.

Why environment variables?
To separate environment-specific configuration and secrets from application code.

Why express.json()?
To parse JSON request bodies.

Difference between Employee.create() and Employee.find()?

create() → write data

find()   → read data

Why 201 for POST?

201 = resource successfully created

Why 200 for GET?

200 = request completed successfully