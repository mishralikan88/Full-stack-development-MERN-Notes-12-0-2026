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