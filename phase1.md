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
