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


