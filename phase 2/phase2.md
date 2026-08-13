PHASE 2 — FRONTEND FOUNDATION

Topic 3 — HTML
Basic Document Structure

We already understand the semantic elements. Now we're covering the basic skeleton that every normal HTML document starts from.

Your practice notes already showed this skeleton, but we hadn't properly learned what each part means.

Here's the structure:

<!DOCTYPE html>

<html lang="en">

<head>
  <meta charset="UTF-8" />
  <title>WorkForge</title>
</head>

<body>
  <h1>Welcome to WorkForge</h1>
</body>

</html>

Don't memorize it blindly. Understand it from outside → inside:

<!DOCTYPE html>

<html>
│
├── <head>
│      Information ABOUT the page
│
└── <body>
       Content SHOWN on the page

</html>
1. <!DOCTYPE html>

You'll almost always see this as the first line:

<!DOCTYPE html>
What does it mean?

It tells the browser:

Treat this document as modern HTML (HTML5).

Think:

Browser opens file
       ↓
Sees
<!DOCTYPE html>
       ↓
Use modern HTML standards
       ↓
Read the page normally
Is DOCTYPE an HTML element?

No.

This:

<header></header>

is an element.

But:

<!DOCTYPE html>

is a declaration.

It tells the browser what kind of HTML document it should expect/how to render it in standards mode.

Where does it go?

At the very top:

<!DOCTYPE html>

<html>
...
</html>
Interview answer

What is <!DOCTYPE html>?

It declares the document as HTML5 and helps browsers render the page using standards mode.

Remember
DOCTYPE
   ↓
"Browser, this is modern HTML."
2. <html>

Next:

<html>
</html>

This is the root element of the HTML document.

Root simply means:

The main/outermost element containing the document.

Think of a box:

<html>
│
│   Everything about the HTML document
│
└──
</html>

Normally:

<html>
  <head>
  </head>

  <body>
  </body>
</html>

So:

HTML
│
├── HEAD
│
└── BODY
What is lang="en"?

You'll normally see:

<html lang="en">

instead of simply:

<html>

lang is an attribute.

lang
 ↓
language

And:

en
 ↓
English

So:

<html lang="en">

means:

The primary language of this page is English.

This helps browsers and accessibility technologies such as screen readers understand the document language.

For now, remember:

<html lang="en">

html
→ root element

lang="en"
→ page language is English
3. <head>

Now we have:

<head>
</head>

This is where beginners often get confused.

head does NOT mean:

The visible header at the top of my website.

These are completely different:

<head>

vs

<header>
<head>

Contains information/configuration about the document.

<header>

Contains visible introductory/header content.

Example:

<head>
  <title>WorkForge</title>
</head>

and:

<body>

  <header>
    <h1>WorkForge</h1>
  </header>

</body>

Think:

<head>
    ↓
ABOUT / CONFIGURATION OF PAGE


<header>
    ↓
VISIBLE HEADER CONTENT

⭐ Very important distinction.

4. What Goes Inside <head>?

You'll commonly see:

<head>

  <meta charset="UTF-8" />

  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  />

  <title>WorkForge</title>

</head>

Don't worry about learning all the meta tags now.

We have a dedicated SEO / Meta Basics section later.

For now understand the idea:

HEAD

├── Page title
├── Character information
├── Metadata
├── CSS links
└── Other page configuration

Most of this isn't ordinary visible page content.

5. <title>

Inside head:

<title>WorkForge</title>

This doesn't normally create a heading inside your page.

Instead, look at the browser tab:

┌──────────────────────────┐
│ WorkForge ×              │
└──────────────────────────┘
       ↑
     title

Compare:

<title>WorkForge</title>

→ browser/document title

while:

<h1>WorkForge</h1>

→ visible heading inside the webpage.

Another important distinction:

<title>
→ browser/document title

<h1>
→ visible page heading
6. <body>

Now the most straightforward part:

<body>
</body>

The body contains the actual document content displayed as the webpage.

Example:

<body>

  <h1>Employee Dashboard</h1>

  <p>Welcome to WorkForge.</p>

  <button>Add Employee</button>

</body>

The user can see these elements on the page.

Later our semantic elements go inside the body:

<body>

  <header>
  </header>

  <nav>
  </nav>

  <aside>
  </aside>

  <main>
  </main>

  <footer>
  </footer>

</body>

Now our previous Semantic HTML lesson connects perfectly.

7. <head> vs <body> ⭐

This is important.

             HTML
              │
       ┌──────┴──────┐
       │             │
     HEAD           BODY
       │             │
       ▼             ▼
 Information       Page
 ABOUT page        CONTENT

Example:

<head>
  <title>Employees</title>
</head>

<body>
  <h1>Employees</h1>
  <p>Manage employees here.</p>
</body>
Easy memory
HEAD
→ Information/configuration ABOUT the document

BODY
→ Actual document content
8. <head> vs <header> ⭐

This is an interview/basic concept worth getting right.

<head>
    ↓
Document information/configuration
    ↓
Not your visible page header


<header>
    ↓
Semantic element
    ↓
Introductory/header content
    ↓
Lives inside <body>

Example:

<!DOCTYPE html>

<html lang="en">

<head>
  <title>WorkForge</title>
</head>

<body>

  <header>
    <h1>WorkForge</h1>
  </header>

</body>

</html>

Hierarchy:

HTML
│
├── HEAD
│   └── title
│
└── BODY
    │
    └── HEADER
        └── h1

Don't confuse:

HEAD ≠ HEADER
9. Complete Structure With What We Already Learned

Now everything starts connecting:

<!DOCTYPE html>

<html lang="en">

<head>
  <meta charset="UTF-8" />
  <title>WorkForge Dashboard</title>
</head>

<body>

  <header>
    <h1>WorkForge</h1>
  </header>

  <nav>
    <a href="#">Dashboard</a>
    <a href="#">Employees</a>
  </nav>

  <aside>
    <h2>Quick Links</h2>
  </aside>

  <main>

    <section>
      <h2>Employee Statistics</h2>
      <p>Total Employees: 120</p>
    </section>

  </main>

  <footer>
    <p>Copyright 2026 WorkForge</p>
  </footer>

</body>

</html>

Look at the structure rather than individual tags:

DOCTYPE
│
└── HTML
    │
    ├── HEAD
    │   ├── meta
    │   └── title
    │
    └── BODY
        │
        ├── HEADER
        ├── NAV
        ├── ASIDE
        ├── MAIN
        │    └── SECTION
        │
        └── FOOTER

That's the big picture.




Quick Revision
<!DOCTYPE html>
→ Declares modern HTML / HTML5


<html>
→ Root element of the document


<html lang="en">
→ Document's primary language is English


<head>
→ Information/configuration about the document


<title>
→ Document/browser-tab title


<body>
→ Actual webpage content


<header>
→ Semantic introductory/header content
  inside the body
Most important structure
<!DOCTYPE html>

<html>

    <head>
        ABOUT THE DOCUMENT
    </head>

    <body>
        DOCUMENT CONTENT
    </body>

</html>





Topic 3 — HTML
3.1 Semantic HTML
What are we learning?

Before React, CSS or Tailwind, a browser page needs structure.

Think about building a house:

HTML       → Structure
CSS        → Appearance
JavaScript → Behaviour
React      → Component-based UI/application logic

Right now, forget styling.

Our goal is to look at a page and know:

"This is the header. This is navigation. This is the main content. These things belong together. This is supporting content. This is the footer."

That's where semantic HTML comes in.

1. What is Semantic HTML?

Semantic means meaningful.

Semantic HTML means:

Using an HTML element whose name describes the purpose of the content inside it.

For example:

<div>WorkForge</div>
<div>Home Employees Projects</div>
<div>Employee Management</div>
<div>Copyright 2026</div>

Technically, this can work.

But everything is a generic <div>.

Compare it with:

<header>
  <h1>WorkForge</h1>
</header>

<nav>
  Home Employees Projects
</nav>

<main>
  Employee Management
</main>

<footer>
  Copyright 2026
</footer>

Now the structure has meaning.

<header> → header/introductory content
<nav>    → navigation
<main>   → main content
<footer> → footer

That's the fundamental idea.

2. The Main Elements You Need

Don't memorize dozens of semantic tags.

For our professional MERN work, understand these first:

<header>       → Header / introductory content

<nav>          → Major navigation

<main>         → Main content of the page

<section>      → Group of related content

<article>      → Independent piece of content

<aside>        → Supporting / secondary content

<footer>       → Footer information

<div>          → Generic block container

<span>         → Generic inline container

The first seven provide semantic meaning.

div and span are generic containers.

3. Skeleton of a Typical Page

This is the picture you should keep in your head:

┌───────────────────────────────────────────────┐
│                   HEADER                      │
│              WorkForge / Logo                 │
├───────────────────────────────────────────────┤
│                    NAV                        │
│      Dashboard | Employees | Projects         │
├──────────────┬────────────────────────────────┤
│              │                                │
│    ASIDE     │              MAIN              │
│              │                                │
│ Quick Links  │      SECTION                   │
│ Reports      │      Employee Statistics       │
│ Settings     │                                │
│              │      SECTION                   │
│              │      Available Jobs            │
│              │          │                     │
│              │          ├── ARTICLE           │
│              │          └── ARTICLE           │
│              │                                │
├──────────────┴────────────────────────────────┤
│                   FOOTER                      │
│          Copyright / Privacy / Terms          │
└───────────────────────────────────────────────┘

HTML hierarchy:

<body>
│
├── <header>
│
├── <nav>
│
├── <aside>
│
├── <main>
│    │
│    ├── <section>
│    │
│    └── <section>
│          │
│          ├── <article>
│          └── <article>
│
└── <footer>

This is an example structure, not a rule that every website must have all of these.

4. <header>

header represents introductory/header content.

Example:

<header>
  <h1>WorkForge</h1>
  <p>Project Management Platform</p>
</header>

Think:

HEADER
   ↓
Who/what is this page or area about?

It can contain things like:

Logo
Website name
Heading
Introductory information
Important

header doesn't mean:

Browser, put this visually at the top.

HTML provides meaning.

CSS controls positioning and appearance.

5. <nav>

nav represents major navigation links.

<nav>
  <a href="/">Dashboard</a>
  <a href="/employees">Employees</a>
  <a href="/projects">Projects</a>
</nav>

Think:

NAV
 ↓
Where can the user navigate?
6. <main>

main contains the dominant content of the current page.

For an Employees page:

<main>
  <h1>Employees</h1>

  <p>Manage company employees.</p>
</main>

Think:

Website
   ↓
Current Page
   ↓
Most important page content
   ↓
MAIN

A normal document should have one primary main region.

7. <section>

You specifically wanted this clearer.

Remember:

SECTION = GROUP / TOPIC

A section groups content that belongs to the same subject.

For example:

<section>
  <h2>Employee Statistics</h2>

  <p>Total Employees: 120</p>
  <p>Active Employees: 105</p>
</section>

Everything is about:

Employee Statistics

Another section:

<section>
  <h2>Available Jobs</h2>

  ...
</section>

So:

MAIN

├── SECTION
│     Employee Statistics
│
├── SECTION
│     Available Jobs
│
└── SECTION
      Recent Activity
8. <article>

Remember:

ARTICLE = ONE INDEPENDENT ITEM

Suppose we have:

AVAILABLE JOBS                  ← SECTION

├── React Developer            ← ARTICLE
├── Node.js Developer          ← ARTICLE
└── UI Developer               ← ARTICLE

HTML:

<section>
  <h2>Available Jobs</h2>

  <article>
    <h3>React Developer</h3>
    <p>Location: Bangalore</p>
    <p>Experience: 5+ years</p>
    <button>Apply</button>
  </article>

  <article>
    <h3>Node.js Developer</h3>
    <p>Location: Hyderabad</p>
    <p>Experience: 4+ years</p>
    <button>Apply</button>
  </article>
</section>
What does independent/self-contained mean?

Take this:

React Developer
Location: Bangalore
Experience: 5+ years
Apply

and put it somewhere else.

You can still understand:

"This is a React Developer job posting."

It makes sense by itself.

That's what self-contained means.

Common examples:

Job posting      → article
Blog post        → article
News story       → article
Forum post       → article
Product review   → article

But don't use article just because something looks visually like a card.

9. <section> vs <article> ⭐

This is your memory trick:

SECTION
   ↓
GROUP / TOPIC


ARTICLE
   ↓
ONE INDEPENDENT ITEM

Example:

LATEST NEWS                       ← SECTION

├── News Story A                  ← ARTICLE
├── News Story B                  ← ARTICLE
└── News Story C                  ← ARTICLE

Ask:

Am I grouping related content?

<section>

Ask:

Can this individual content make sense on its own?

<article>
10. <aside>

Remember:

ASIDE = SUPPORTING CONTENT

Suppose Employees are the main thing you're viewing.

┌────────────────┬───────────────────────────┐
│                │                           │
│     ASIDE      │           MAIN            │
│                │                           │
│ Quick Links    │ Employees                 │
│                │                           │
│ Reports        │ Amar                      │
│ Settings       │ Rahul                     │
│ Help           │ Priya                     │
│                │                           │
└────────────────┴───────────────────────────┘

Supporting                    Important
content                       page content

HTML:

<aside>
  <h2>Quick Links</h2>

  <a href="/reports">Reports</a>
  <a href="/settings">Settings</a>
</aside>

Don't memorize:

aside = left sidebar ❌

Memorize:

aside = supporting/secondary content ✅

CSS can eventually put it on the left, right, etc.

11. <footer>

Footer-related information:

<footer>
  <p>Copyright 2026 WorkForge</p>
</footer>

Common examples:

Copyright
Privacy
Terms
Contact
Related links
12. Why Do We Still Need <div>?

Semantic HTML doesn't mean:

Never use div.

div is simply a generic block container.

Example:

<div class="employee-actions">
  <button>Edit</button>
  <button>Delete</button>
</div>

We're grouping the buttons.

There isn't necessarily a better semantic element for that grouping.

Think:

Does the container have a meaningful
semantic purpose?

           ↓

       YES     NO
        │       │
        ▼       ▼
 semantic     <div>
 element
13. What is <span>?

span is a generic inline container.

Example:

<p>
  Employee Status:
  <span>Active</span>
</p>

Later we could style only:

Active

without affecting the entire paragraph.

For now:

div  → generic block/group container

span → generic inline/text-level container

We'll understand block/inline behavior more deeply in CSS.

14. Why Do We Care About Semantic HTML?

Three practical reasons:

Semantic HTML
      │
      ├── Easier code to understand/maintain
      │
      ├── Better structure for accessibility
      │
      └── Helps search engines understand content

We'll properly cover accessibility and SEO later in Topic 3.


🛠 NOW THE PRACTICAL — DO THIS

We're going to build a tiny HTML-only WorkForge Dashboard skeleton.

No React.

No CSS.

No Tailwind.

The purpose is purely to prove:

You can structure a page correctly.

Step 1 — Go to your project

Your project root is:

D:\Fullstack-MERN\MERN-master-project

Your current project already contains:

MERN-master-project/
│
├── client/
├── server/
└── ...

Don't put this exercise inside client/src.

At the root, create:

html-practice/

So:

MERN-master-project/
│
├── client/
├── server/
│
└── html-practice/

Inside that create:

semantic.html

Final:

MERN-master-project/
│
├── client/
├── server/
│
└── html-practice/
    └── semantic.html
Step 2 — Put the basic HTML skeleton inside

Type this yourself:

<!DOCTYPE html>

<html lang="en">

<head>
  <meta charset="UTF-8" />

  <title>WorkForge Dashboard</title>
</head>

<body>

</body>

</html>
Understand the skeleton
<!DOCTYPE html>
      ↓
We're using modern HTML


<html>
      ↓
Entire HTML document


<head>
      ↓
Information/configuration about document


<body>
      ↓
Visible page content

We'll properly learn head, meta tags and SEO later.

Step 3 — Add the Header

Inside <body>:

<header>
  <h1>WorkForge</h1>

  <p>Project Management Platform</p>
</header>

Now mentally say:

This is introductory/header content, so I used header.

Step 4 — Add Navigation

Below the header:

<nav>
  <a href="#">Dashboard</a>
  <a href="#">Employees</a>
  <a href="#">Projects</a>
</nav>

Why nav?

Because these are major navigation links.

We'll properly learn <a> later.

Step 5 — Add Aside

Below nav:

<aside>
  <h2>Quick Links</h2>

  <a href="#">Reports</a>
  <a href="#">Settings</a>
</aside>

Before moving on, tell yourself:

aside
=
supporting content
Step 6 — Add Main
<main>

</main>

Everything important about this dashboard page will go inside this.

Step 7 — Add Your First Section

Inside main:

<section>
  <h2>Employee Statistics</h2>

  <p>Total Employees: 120</p>
  <p>Active Employees: 105</p>
</section>

Ask:

Why section?

Because:

Employee Statistics

is one meaningful group/topic.

Step 8 — Add Another Section

Still inside main:

<section>
  <h2>Available Jobs</h2>
</section>

Why another section?

Because:

Employee Statistics

and:

Available Jobs

are different groups/topics.

Step 9 — Add Articles

Inside the Available Jobs section:

<article>
  <h3>React Developer</h3>

  <p>Location: Bangalore</p>
  <p>Experience: 5+ years</p>

  <button>Apply</button>
</article>

Then:

<article>
  <h3>Node.js Developer</h3>

  <p>Location: Hyderabad</p>
  <p>Experience: 4+ years</p>

  <button>Apply</button>
</article>

Why article?

Each job is one independent job posting.

So:

Available Jobs                  SECTION

    React Developer             ARTICLE

    Node.js Developer           ARTICLE
Step 10 — Footer

After main:

<footer>
  <p>Copyright 2026 WorkForge</p>
</footer>
Your Complete Practical Code

Your final semantic.html should be:

<!DOCTYPE html>

<html lang="en">

<head>
  <meta charset="UTF-8" />

  <title>WorkForge Dashboard</title>
</head>

<body>

  <header>
    <h1>WorkForge</h1>
    <p>Project Management Platform</p>
  </header>


  <nav>
    <a href="#">Dashboard</a>
    <a href="#">Employees</a>
    <a href="#">Projects</a>
  </nav>


  <aside>
    <h2>Quick Links</h2>

    <a href="#">Reports</a>
    <a href="#">Settings</a>
  </aside>


  <main>

    <section>
      <h2>Employee Statistics</h2>

      <p>Total Employees: 120</p>
      <p>Active Employees: 105</p>
    </section>


    <section>
      <h2>Available Jobs</h2>

      <article>
        <h3>React Developer</h3>

        <p>Location: Bangalore</p>
        <p>Experience: 5+ years</p>

        <button>Apply</button>
      </article>


      <article>
        <h3>Node.js Developer</h3>

        <p>Location: Hyderabad</p>
        <p>Experience: 4+ years</p>

        <button>Apply</button>
      </article>

    </section>

  </main>


  <footer>
    <p>Copyright 2026 WorkForge</p>
  </footer>

</body>

</html>
Step 11 — Run It

You don't need:

npm run dev

for this simple standalone HTML exercise.

In VS Code, locate:

html-practice/semantic.html

Open it in your browser. If you use a Live Server extension, that's also fine.

Don't worry when it looks plain and ugly.

That's correct.

We're currently learning:

HTML
 ↓
STRUCTURE

NOT

HTML
 ↓
DESIGN

CSS comes in Topic 4.

Step 12 — Inspect It

In Chrome/Edge:

Right Click
    ↓
Inspect
    ↓
Elements

Look at the DOM structure.

You should see something conceptually like:

body
│
├── header
│
├── nav
│
├── aside
│
├── main
│   │
│   ├── section
│   │
│   └── section
│       ├── article
│       └── article
│
└── footer

This is valuable because you're seeing how your HTML becomes the browser's document structure.



Topic 3 — HTML
Text & Content 🔄

Semantic HTML is complete. Now we learn the basic elements used to put text and lists inside that structure.

Text & Content

├── h1–h6        ← CURRENT
├── p
├── strong
├── em
├── br
├── hr
├── ul
├── ol
└── li
1. Headings — <h1> to <h6>

HTML provides 6 heading levels:

<h1>Main Heading</h1>
<h2>Section Heading</h2>
<h3>Subsection Heading</h3>
<h4>Smaller Subsection</h4>
<h5>Lower-level Heading</h5>
<h6>Lowest-level Heading</h6>

The important thing is not their visual size.

They represent the hierarchy/structure of the content.

Think of a book:

Employee Management                 ← h1

    Employee Statistics             ← h2

        Active Employees            ← h3

        Inactive Employees          ← h3

    Available Jobs                  ← h2

        React Developer             ← h3

        Node.js Developer           ← h3

HTML:

<h1>Employee Management</h1>

<section>
  <h2>Employee Statistics</h2>

  <h3>Active Employees</h3>
  <h3>Inactive Employees</h3>
</section>

<section>
  <h2>Available Jobs</h2>

  <article>
    <h3>React Developer</h3>
  </article>

  <article>
    <h3>Node.js Developer</h3>
  </article>
</section>
Easy rule
h1
│
├── h2
│   ├── h3
│   └── h3
│
└── h2
    ├── h3
    └── h3

Think:

Heading levels describe content hierarchy.

Don't choose <h3> simply because you like how small it looks. CSS controls appearance later.

2. Paragraph — <p>

p means paragraph.

<p>Welcome to the WorkForge employee management system.</p>

Another example:

<h2>About WorkForge</h2>

<p>
  WorkForge helps organizations manage employees and projects.
</p>

Think:

<h2> → Heading

<p>  → Normal paragraph/content
3. <strong>

strong indicates content with strong importance.

<p>
  Employee Status:
  <strong>Active</strong>
</p>

Browser default styling usually makes it bold:

Employee Status: Active

But remember:

<strong>
     ↓
Strong importance

It's semantic meaning, not merely "make text bold."

4. <em>

em means emphasis.

<p>
  Please submit the form <em>before Friday</em>.
</p>

Browsers normally display em in italics.

But again:

<em>
 ↓
Emphasis

not simply:

make text italic

CSS handles purely visual styling.

5. <br>

br means line break.

<p>
  Amarnath Mishra<br>
  UI Developer<br>
  Bangalore
</p>

Output conceptually:

Amarnath Mishra
UI Developer
Bangalore

br does not need a closing tag:

<br>

or:

<br />
Don't misuse it

Don't write:

<p>Hello</p>
<br>
<br>
<br>
<p>Employees</p>

just to create visual spacing.

Later CSS will handle spacing.

Use <br> when a line break is actually part of the content.

6. <hr>

hr represents a thematic break/change between sections of content and is commonly displayed as a horizontal line.

<h2>Active Employees</h2>

<p>105 employees are currently active.</p>

<hr>

<h2>Inactive Employees</h2>

<p>15 employees are currently inactive.</p>

You might see:

Active Employees

105 employees are currently active.

────────────────────────────────

Inactive Employees

15 employees are currently inactive.

Again, don't think only:

hr = draw a line

Its semantic purpose is a thematic break.

7. Lists

HTML has two main types of lists you'll regularly use.

Unordered list — <ul>

Use when order doesn't matter.

<ul>
  <li>React</li>
  <li>TypeScript</li>
  <li>Node.js</li>
</ul>

Browser:

• React
• TypeScript
• Node.js

ul means:

Unordered List

Ordered list — <ol>

Use when order matters.

<ol>
  <li>Create employee</li>
  <li>Validate employee data</li>
  <li>Save employee</li>
</ol>

Browser:

1. Create employee
2. Validate employee data
3. Save employee

ol means:

Ordered List

8. <li>

li means:

List Item

It goes inside ul or ol.

<ul>
  <li>Dashboard</li>
  <li>Employees</li>
  <li>Projects</li>
</ul>

Think:

UL / OL
   │
   ├── LI
   ├── LI
   └── LI
ul vs ol

Very easy:

Does order/sequence matter?

        │
   ┌────┴────┐
   │         │
   NO       YES
   │         │
   ▼         ▼
  <ul>      <ol>

   │         │
   └────┬────┘
        ▼
      <li>

For example:

Skills

React
Node
MongoDB

No particular sequence is required.

→ ul

Registration steps

1. Enter details
2. Verify email
3. Create account

Sequence matters.

→ ol


🛠 Practical

Continue using:

MERN-master-project/
└── html-practice/
    └── semantic.html

Inside your existing <main>, add this:

<section>

  <h2>Employee Profile</h2>

  <h3>About</h3>

  <p>
    Rahul Sharma is a frontend developer working
    on the WorkForge application.
  </p>

  <p>
    Status: <strong>Active</strong>
  </p>

  <p>
    Current project: <em>Employee Management System</em>
  </p>


  <h3>Technical Skills</h3>

  <ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
    <li>React</li>
  </ul>


  <h3>Onboarding Steps</h3>

  <ol>
    <li>Complete employee registration</li>
    <li>Verify employee details</li>
    <li>Assign project</li>
    <li>Activate employee account</li>
  </ol>

</section>

Now look at what you just built:

Employee Profile                   ← h2

    About                          ← h3

    Rahul Sharma...               ← p

    Status: Active                ← p + strong

    Current project...            ← p + em


    Technical Skills              ← h3

       • HTML                     ← ul + li
       • CSS
       • JavaScript
       • React


    Onboarding Steps              ← h3

       1. Registration            ← ol + li
       2. Verify
       3. Assign project
       4. Activate

This also reinforces the Semantic HTML you just completed because the content sits inside a meaningful <section>.

Quick revision
<h1> - <h6>
→ Heading hierarchy

<p>
→ Paragraph

<strong>
→ Strong importance

<em>
→ Emphasis

<br>
→ Line break

<hr>
→ Thematic break

<ul>
→ Unordered list
→ Order doesn't matter

<ol>
→ Ordered list
→ Order matters

<li>
→ List item


Topic 3 — HTML
Links 🔄 CURRENT

Assuming you've completed the Text & Content practical, our progress is:

├── Semantic HTML              ✅
├── Text & Content             ✅
├── Links                      🔄 CURRENT
├── Images                     ⏳
├── Forms                      ⏳
├── Labels & Inputs            ⏳
├── Buttons                    ⏳
├── Native HTML Validation     ⏳
├── Tables                     ⏳
├── Accessibility Basics       ⏳
├── ARIA Basics                ⏳
└── SEO / Meta Basics          ⏳
1. What is a Link?

A link allows the user to navigate from one location to another.

HTML uses the anchor element:

<a>...</a>

Example:

<a href="employees.html">Employees</a>

Think:

<a href="employees.html">Employees</a>
 │          │              │
 │          │              └── What user sees/clicks
 │          │
 │          └── Where to go
 │
 └── Anchor element

href means Hypertext Reference.

2. Basic Link
<a href="employees.html">View Employees</a>

The user sees:

View Employees

When clicked:

View Employees
      ↓
employees.html
3. Internal Links

An internal link navigates to another page/resource in your own website/application.

Imagine:

html-practice/
│
├── index.html
├── employees.html
└── contact.html

Inside index.html:

<a href="employees.html">Employees</a>
<a href="contact.html">Contact</a>

These are internal links.

Your Website
     │
     ├── Home
     ├── Employees
     └── Contact
4. External Links

An external link takes the user to another website.

For example:

<a href="https://www.google.com">
  Google
</a>

Concept:

Your website
     ↓
    Link
     ↓
Another website

So:

Internal link
→ another location/page in your application

External link
→ another website/domain
5. Relative vs Absolute Paths ⭐

This is important.

Relative path
<a href="employees.html">Employees</a>

The location is relative to your current project/file.

Another example:

<a href="pages/employees.html">Employees</a>

Think:

Current location
      ↓
Find another file relative to me
Absolute URL
<a href="https://www.google.com">Google</a>

It provides the complete address:

https://
www.google.com

Easy distinction:

RELATIVE

employees.html
pages/employees.html
../index.html

Used relative to current file/location.


ABSOLUTE

https://www.google.com

Complete URL.

You'll encounter this concept constantly in frontend development.

6. target

Suppose you have:

<a href="https://www.google.com">
  Google
</a>

Normally, clicking it navigates in the current browser tab.

You can use:

<a
  href="https://www.google.com"
  target="_blank"
>
  Google
</a>

target="_blank" generally opens the destination in a new tab/window browsing context.

Remember:

target="_blank"
       ↓
Open in new tab/window
7. rel Basics

For links opened with _blank, you'll often encounter:

<a
  href="https://example.com"
  target="_blank"
  rel="noopener noreferrer"
>
  External Website
</a>

For your current level, don't overcomplicate this.

Know:

rel
 ↓
Describes the relationship between
the current page and linked resource.

noopener prevents the newly opened page from getting access to the original page through window.opener in browsers where that relationship exists.

noreferrer tells the browser not to send referrer information to the destination.

Modern browsers already provide important protections for _blank, but you'll still commonly see these values in code.

8. Link to a Section on the Same Page

Links don't have to open another page.

Suppose:

<a href="#contact">Go to Contact</a>

Later:

<section id="contact">
  <h2>Contact Us</h2>
</section>

The connection is:

href="#contact"
       │
       ▼
id="contact"

Clicking the link moves to that section.

This is called a fragment/anchor link.

9. Email Link

HTML supports:

<a href="mailto:hr@example.com">
  Email HR
</a>

Clicking it can open the user's configured email application.

mailto:
   ↓
Email address
10. Telephone Link

You can also write:

<a href="tel:+919876543210">
  Call HR
</a>

Especially useful on mobile devices.

tel:
 ↓
Telephone number
11. Accessible Link Text

Avoid vague links like:

<a href="employees.html">Click here</a>

If someone only hears:

"Click here"

they don't know what it does.

Better:

<a href="employees.html">
  View Employees
</a>

Now the link explains its destination/action.

❌ Click here

✅ View Employees
✅ View employee details
✅ Read project documentation

We'll go deeper into this under Accessibility Basics.

12. Link vs Button — Small Introduction

This becomes important later.

Use a link when the user is navigating somewhere:

<a href="employees.html">
  View Employees
</a>

Use a button when the user is performing an action:

<button>
  Save Employee
</button>

Memory:

LINK
→ GO somewhere

BUTTON
→ DO something

We'll properly cover this in the Buttons section.

🛠 Practical

Continue in your:

html-practice/
└── semantic.html

Add a navigation section:

<nav>
  <a href="#dashboard">Dashboard</a>
  <a href="#employees">Employees</a>
  <a href="#projects">Projects</a>
  <a href="#contact">Contact</a>
</nav>

Then inside your <main>:

<main>

  <section id="dashboard">
    <h2>Dashboard</h2>
    <p>Welcome to the WorkForge dashboard.</p>
  </section>


  <section id="employees">
    <h2>Employees</h2>
    <p>Manage company employees here.</p>
  </section>


  <section id="projects">
    <h2>Projects</h2>
    <p>View company projects here.</p>
  </section>


  <section id="contact">
    <h2>Contact</h2>

    <p>
      Email:
      <a href="mailto:hr@example.com">
        hr@example.com
      </a>
    </p>

    <p>
      Phone:
      <a href="tel:+919876543210">
        +91 98765 43210
      </a>
    </p>
  </section>

</main>

Now run it in your browser.

Click:

Dashboard
Employees
Projects
Contact

Notice how:

<a href="#employees">

connects to:

<section id="employees">

That's the important practical concept.

🧪 Small Independent Task

Without copying another solution, add this below your page:

Company Resources

• Visit MDN Documentation
• Contact Support

Requirements:

Visit MDN Documentation
→ external link
→ open in new tab

Contact Support
→ email link

You decide the correct href, target, and rel.

Quick Revision
<a>
→ Anchor/link element

href
→ Destination

Internal link
→ Navigate within your website/application

External link
→ Navigate to another website

Relative path
→ Location relative to current file

Absolute URL
→ Complete URL

target="_blank"
→ Open in new tab/window

rel
→ Relationship/security/privacy information

#section
→ Navigate within same page

mailto:
→ Email link

tel:
→ Telephone link

Good link text
→ Clearly describes destination

LINK
→ GO somewhere

BUTTON
→ DO something


Topic 3 — HTML
Images 🔄 CURRENT

Assuming you completed the Links practical:

├── Semantic HTML              ✅
├── Text & Content             ✅
├── Links                      ✅
├── Images                     🔄 CURRENT
├── Forms                      ⏳
├── Labels & Inputs            ⏳
├── Buttons                    ⏳
├── Native HTML Validation     ⏳
├── Tables                     ⏳
├── Accessibility Basics       ⏳
├── ARIA Basics                ⏳
└── SEO / Meta Basics          ⏳

For Images, we need to understand:

├── <img>
├── src
├── alt
├── width / height basics
├── Relative image paths
├── Decorative vs informative images
├── <figure>
└── <figcaption>
1. <img> — Image Element

HTML uses <img> to display an image.

<img src="employee.jpg" alt="Employee profile" />

Unlike elements such as:

<p></p>
<section></section>

img doesn't wrap content and doesn't have a closing </img> tag.

So:

<img src="employee.jpg" alt="Employee profile" />

Think:

<img>
  │
  ├── src → WHERE is the image?
  │
  └── alt → WHAT does the image represent?
2. src

src means source.

It tells the browser where the image is located.

Suppose your files are:

html-practice/
│
├── semantic.html
│
└── employee.jpg

You can write:

<img src="employee.jpg" alt="Employee profile" />

Browser:

semantic.html
      │
      └── Find employee.jpg
              ↓
         Display image
3. Relative Image Paths ⭐

Suppose we organize things better:

html-practice/
│
├── semantic.html
│
└── images/
    ├── employee.jpg
    └── company-logo.png

From semantic.html, the employee image is inside images.

Therefore:

<img
  src="images/employee.jpg"
  alt="Employee profile"
/>

Think:

semantic.html
     │
     └── images/
           │
           └── employee.jpg

This is a relative path, just like what we learned with links.

4. alt ⭐ VERY IMPORTANT

alt means alternative text.

Example:

<img
  src="images/employee.jpg"
  alt="Rahul Sharma, frontend developer"
/>

The alt text describes the meaningful information conveyed by the image when the image can't be seen, including for users of screen readers.

Think:

IMAGE
  ↓
Can user see/use image?
  │
  ├── YES → Image
  │
  └── NO  → Alternative text can convey its meaning

This becomes especially important when we study Accessibility.

5. Bad vs Good alt

Suppose the image shows an employee named Rahul Sharma.

Poor:

<img
  src="images/rahul.jpg"
  alt="image"
/>

Also usually poor:

<img
  src="images/rahul.jpg"
  alt="photo"
/>

Better:

<img
  src="images/rahul.jpg"
  alt="Rahul Sharma, frontend developer"
/>

The text should communicate the image's purpose/context, not mechanically describe every visual detail.

6. Decorative Images

Here's an important exception.

Suppose an image exists only for decoration and communicates no useful information.

Then you can use an empty alt:

<img
  src="images/decoration.png"
  alt=""
/>

Why?

Because accessibility tools don't need to announce meaningless decoration.

So remember:

INFORMATIVE IMAGE
        ↓
Meaningful alt text

<img src="employee.jpg"
     alt="Rahul Sharma, frontend developer">


DECORATIVE IMAGE
        ↓
Empty alt

<img src="decoration.png"
     alt="">

Don't omit alt just because you don't know what to write.

7. Width and Height

You can provide intrinsic dimensions:

<img
  src="images/employee.jpg"
  alt="Rahul Sharma, frontend developer"
  width="300"
  height="300"
/>

Conceptually:

width="300"
→ 300 CSS pixels as the image's intrinsic display width

height="300"
→ 300 CSS pixels as the image's intrinsic display height

Later, CSS will handle responsive image sizing and visual design.

For now, understand the attributes.

Providing appropriate width and height can also help the browser reserve space before the image finishes loading, reducing layout movement.

8. <figure>

Sometimes an image and its caption belong together.

Example:

<figure>

  <img
    src="images/employee.jpg"
    alt="Rahul Sharma, frontend developer"
    width="300"
  />

  <figcaption>
    Rahul Sharma — Frontend Developer
  </figcaption>

</figure>

figure groups self-contained media/content with an optional caption.

Think:

FIGURE
│
├── IMAGE
│
└── CAPTION
9. <figcaption>

figcaption provides the caption for the figure.

Example:

<figure>

  <img
    src="images/dashboard.png"
    alt="WorkForge employee dashboard"
  />

  <figcaption>
    WorkForge Employee Dashboard
  </figcaption>

</figure>

Conceptually:

┌───────────────────────────────┐
│                               │
│        DASHBOARD IMAGE        │
│                               │
└───────────────────────────────┘

 WorkForge Employee Dashboard
            ↑
       figcaption
10. alt vs figcaption ⭐

Don't confuse these.

alt
 ↓
Text alternative for the IMAGE


figcaption
 ↓
Visible caption associated with the FIGURE

Example:

<figure>

  <img
    src="images/rahul.jpg"
    alt="Rahul Sharma presenting the WorkForge dashboard"
  />

  <figcaption>
    WorkForge product demonstration — August 2026
  </figcaption>

</figure>

They can provide different information.

The caption is visible.

The alt describes/conveys the image's relevant meaning.

11. Image Inside a Link

Images can also be clickable.

For example, a company logo that navigates home:

<a href="index.html">

  <img
    src="images/logo.png"
    alt="WorkForge home"
  />

</a>

Now:

IMAGE
  ↓
inside
  ↓
LINK
  ↓
click
  ↓
Home

Notice how our HTML topics start connecting.

12. Practical Folder Structure

Inside your existing practice folder, create:

MERN-master-project/
│
└── html-practice/
    │
    ├── semantic.html
    │
    └── images/
        └── employee.jpg

Use any normal practice image and rename it:

employee.jpg

Make sure the actual file format is JPEG. Don't simply rename a .png file's extension.

🛠 Practical — Employee Profile

Inside your existing <main>, add:

<section>

  <h2>Employee Profile</h2>

  <figure>

    <img
      src="images/employee.jpg"
      alt="Employee profile photo"
      width="250"
      height="250"
    />

    <figcaption>
      Rahul Sharma — Frontend Developer
    </figcaption>

  </figure>

  <p>
    Rahul works on the WorkForge frontend application.
  </p>

</section>

Run the page.

You should see roughly:

Employee Profile

┌──────────────────────────┐
│                          │
│      EMPLOYEE IMAGE      │
│                          │
└──────────────────────────┘

Rahul Sharma — Frontend Developer

Rahul works on the WorkForge
frontend application.
🧪 Important Experiment

Temporarily change:

src="images/employee.jpg"

to:

src="images/not-found.jpg"

Refresh the browser.

The image can't load.

Now inspect what happens and notice why:

alt="Employee profile photo"

matters.

Then change the src back.

This is an easy way to understand alt instead of merely memorizing its definition.

🛠 Independent Challenge

Create another section yourself:

Company Office

[office image]

WorkForge Development Center

Requirements:

<section>
     ↓
<figure>
     ↓
<img>
     ↓
<figcaption>

You must decide:

correct src
useful alt
width/height
appropriate caption

Don't copy my Employee Profile example blindly—build this one yourself.

Quick Revision
<img>
→ Displays an image

src
→ Image location/source

alt
→ Text alternative conveying image meaning/purpose

Informative image
→ Meaningful alt

Decorative image
→ alt=""

width / height
→ Image dimensions/intrinsic sizing information

Relative path
→ images/employee.jpg

<figure>
→ Groups media/content with its caption

<figcaption>
→ Caption for figure
Most important interview points

What does alt do?
It provides a text alternative for an image, especially important for accessibility and when an image can't be displayed.

alt vs figcaption?

alt
→ alternative text for image

figcaption
→ visible caption for figure

Informative vs decorative image?

Informative → meaningful alt

Decorative → alt=""

Once you've completed the practical and independent challenge, Images = ✅.



Topic 3 — HTML
Forms — Restart From Zero 🔄

We'll restart Forms completely, in easy language, and build it gradually. Don't worry about inputs, validation, React, or MongoDB yet.

Our position:

Semantic HTML              ✅
Text & Content             ✅
Links                      ✅
Images                     ✅

Forms                      🔄 CURRENT
├── What is a form?
├── <form>
├── action
├── method
├── GET vs POST
├── name
├── fieldset
├── legend
└── Form submission basics

Labels & Inputs            ⏳ NEXT
Buttons                    ⏳
Validation                 ⏳
Tables                     ⏳
Accessibility              ⏳
ARIA                       ⏳
SEO / Meta                 ⏳
1. What is a Form?

A form is used to collect information from a user.

Think about a login page:

        LOGIN

Email
[___________________]

Password
[___________________]

       [ Login ]

There are three basic things happening:

USER
 │
 ├── enters email
 │
 ├── enters password
 │
 └── clicks Login
          ↓
       FORM DATA
          ↓
       submitted

Other examples of forms:

Login
Registration
Contact Us
Create Employee
Edit Employee
Checkout
Search

In our MERN project, imagine:

CREATE EMPLOYEE

Name:        [____________]

Email:       [____________]

Department:  [____________]

             [ Create Employee ]

The purpose of this form is to collect employee information.

2. <form>

HTML provides the <form> element:

<form>

</form>

Think of <form> as a container.

Everything related to that particular form normally goes inside it.

For example:

<form>

  <label>Name</label>
  <input type="text">

  <label>Email</label>
  <input type="email">

  <button type="submit">
    Create Employee
  </button>

</form>

Don't focus on label, input, or button yet.

Just look at the structure:

FORM
│
├── Name field
├── Email field
│
└── Submit button

So:

<form> groups controls used to collect and submit user data.

3. What Happens to the Data?

Suppose I enter:

Name
[ Rahul ]

Email
[ rahul@gmail.com ]

[ Create Employee ]

The browser now has something conceptually like:

name  → Rahul

email → rahul@gmail.com

When I submit the form:

User
 ↓
fills form

Name = Rahul
Email = rahul@gmail.com
 ↓
clicks Create Employee
 ↓
FORM SUBMISSION
 ↓
Server

And in a full-stack application, that can eventually become:

HTML / React Form
       ↓
Employee data
       ↓
HTTP Request
       ↓
Express API
       ↓
Controller
       ↓
Mongoose
       ↓
MongoDB

You've already seen the later part of this flow in our MERN project. Now we're learning where the data starts.

4. action — WHERE?

Now suppose we want the form to send data somewhere.

HTML gives us action:

<form action="/api/employees">

</form>

action answers one question:

WHERE should this form be submitted?

Here:

action="/api/employees"
              ↓
        WHERE TO SEND

So remember:

ACTION = WHERE

For example:

<form action="/login">

means:

Submit to
    ↓
/login

And:

<form action="/api/employees">

means:

Submit to
       ↓
/api/employees
5. method — HOW?

We know where.

Now we need to say how.

That's what method does.

<form
  action="/api/employees"
  method="post"
>

Read this as English:

Submit this form to /api/employees using POST.

Easy:

<form
   │
   ├── action → WHERE?
   │
   └── method → HOW?

This is the easiest way to remember them.

6. GET vs POST

You've already encountered GET and POST in our API work, but let's understand them specifically from the perspective of an HTML form.

GET

Example:

<form action="/search" method="get">

Imagine:

Search Employee

[ Rahul ]

[ Search ]

After submission, you may get a URL like:

/search?name=Rahul

Notice:

name=Rahul

is in the URL.

GET is commonly appropriate for:

Search
Filters
Sorting
Retrieving something
POST

Now imagine:

CREATE EMPLOYEE

Name:  Rahul
Email: rahul@gmail.com

[ Create Employee ]

We don't want to represent creation as a search URL like:

/api/employees?name=Rahul&email=...

Instead:

<form
  action="/api/employees"
  method="post"
>

The submitted form data goes in the request body.

Conceptually:

POST /api/employees

Request Body
      ↓
name = Rahul
email = rahul@gmail.com

POST is commonly used for operations such as:

Registration
Login
Create Employee
Submit Contact Form
Create Order
GET vs POST — Easy Memory
              FORM
                │
        ┌───────┴───────┐
        │               │
       GET             POST
        │               │
        ▼               ▼
 Search / Read       Submit data
 Filters             Create/process
        │               │
        ▼               ▼
Values generally     Values generally
in URL query         in request body

For our Employee creation:

Create Employee
      ↓
    POST
7. name — Very Important

Look at:

<input
  type="text"
  name="employeeName"
>

Suppose the user enters:

Rahul

The browser needs to know:

What field does Rahul belong to?

That's where name becomes important.

name="employeeName"
          ↓
      FIELD NAME

User enters
   Rahul
     ↓
FIELD VALUE

Together:

employeeName = Rahul

Another input:

<input
  type="email"
  name="email"
>

User enters:

rahul@gmail.com

Now:

employeeName = Rahul

email = rahul@gmail.com

Think of name as the key/field name used during native form submission.

8. Connect It With JSON

This is only to help you understand the concept.

Form fields:

employeeName = Rahul
email        = rahul@gmail.com

A similar data structure represented as JSON could look like:

{
  "employeeName": "Rahul",
  "email": "rahul@gmail.com"
}

So conceptually:

name attribute
     ↓
FIELD / KEY


entered value
     ↓
VALUE

But remember: a normal HTML form does not automatically send JSON.

We'll deal with JSON/API submission later when React handles the form.

9. <fieldset>

Suppose our registration form becomes bigger:

REGISTRATION

Personal Information
---------------------
Name
Email
Phone


Account Information
--------------------
Username
Password

There are clearly two groups of related information.

HTML provides:

<fieldset>

</fieldset>

Its job is to:

Group related form controls.

Example:

<fieldset>

  <label>Name</label>
  <input type="text">

  <label>Email</label>
  <input type="email">

</fieldset>

Think:

FIELDSET
│
├── Name
├── Email
└── Phone
10. <legend>

Now we have a group.

But what is that group called?

Use:

<legend>
  Personal Information
</legend>

Complete:

<fieldset>

  <legend>
    Personal Information
  </legend>

  <label>Name</label>
  <input type="text">

  <label>Email</label>
  <input type="email">

</fieldset>

Think:

┌── Personal Information ──────────┐
│                                  │
│ Name     [_______________]       │
│                                  │
│ Email    [_______________]       │
│                                  │
└──────────────────────────────────┘

So:

fieldset
   ↓
GROUP


legend
   ↓
NAME/CAPTION OF GROUP
11. Put Everything Together

Now look at this:

<form
  action="/api/employees"
  method="post"
>

  <fieldset>

    <legend>
      Employee Information
    </legend>

    <label for="employeeName">
      Employee Name
    </label>

    <input
      id="employeeName"
      type="text"
      name="employeeName"
    >

    <br><br>

    <label for="employeeEmail">
      Email
    </label>

    <input
      id="employeeEmail"
      type="email"
      name="email"
    >

    <br><br>

    <button type="submit">
      Create Employee
    </button>

  </fieldset>

</form>

Don't memorize it.

Read it:

FORM
│
├── action="/api/employees"
│      ↓
│    WHERE?
│
├── method="post"
│      ↓
│     HOW?
│
└── FIELDSET
       ↓
   Related controls
       │
       ├── LEGEND
       │     ↓
       │   Group name
       │
       ├── Employee Name
       │
       ├── Email
       │
       └── Submit
12. Form Submission Flow ⭐

This is the main thing I want you to understand from this topic:

USER
 │
 ▼
Fills Employee Form

Name  = Rahul
Email = rahul@gmail.com
 │
 ▼
Clicks Create Employee
 │
 ▼
<form>
 │
 ├── action
 │      ↓
 │   WHERE?
 │
 └── method
        ↓
      HOW?
 │
 ▼
HTTP REQUEST
 │
 ▼
BACKEND

Later with React:

FORM
 ↓
React onSubmit
 ↓
TypeScript / JavaScript
 ↓
API request
 ↓
Express
 ↓
MongoDB

That second flow will become extremely important later.

🛠 Our Practical

Inside your existing:

html-practice/
└── semantic.html

add this inside <main>:

<section>

  <h2>Create Employee</h2>

  <form
    action="/api/employees"
    method="post"
  >

    <fieldset>

      <legend>
        Employee Information
      </legend>

      <label for="employeeName">
        Employee Name
      </label>

      <input
        id="employeeName"
        type="text"
        name="employeeName"
      >

      <br><br>

      <label for="employeeEmail">
        Email
      </label>

      <input
        id="employeeEmail"
        type="email"
        name="email"
      >

      <br><br>

      <button type="submit">
        Create Employee
      </button>

    </fieldset>

  </form>

</section>

Open it in your browser and enter any employee name and email.

Don't worry about making the POST actually work yet. This exercise is about understanding the HTML form.

Before we mark Forms complete, you should be able to answer these seven things:
<form>      → What is it?

action      → WHERE?

method      → HOW?

GET         → When/why?

POST        → When/why?

fieldset    → What does it group?

legend      → What does it describe?




Topic 3 — HTML
Labels & Inputs 🔄 CURRENT

Forms = ✅ COMPLETE.

Now we learn the actual controls users interact with.

├── Semantic HTML                    ✅
├── Text & Content                   ✅
├── Links                            ✅
├── Images                           ✅
├── Forms                            ✅
│
├── Labels & Inputs                  🔄 CURRENT
│   ├── label
│   ├── for / id
│   ├── name / value
│   ├── placeholder
│   ├── text
│   ├── email
│   ├── password
│   ├── number
│   ├── tel
│   ├── date
│   ├── checkbox
│   ├── radio
│   ├── file
│   ├── textarea
│   ├── select / option
│   └── disabled / readonly
│
├── Buttons                          ⏳
├── Native HTML Validation           ⏳
├── Tables                           ⏳
├── Accessibility Basics            ⏳
├── ARIA Basics                      ⏳
└── SEO / Meta Basics               ⏳
1. What is <input>?

An <input> is a control where the user can enter or choose data.

For example:

<input type="text">

Browser:

Employee Name

[________________________]

Different type values create different kinds of controls:

type="text"       → normal text
type="email"      → email
type="password"   → password
type="number"     → number
type="tel"        → phone number
type="date"       → date
type="checkbox"   → checkbox
type="radio"      → radio button
type="file"       → file picker

Think:

<input>
   │
   └── type
        ↓
   What kind of input?
2. <label> ⭐

Suppose we have:

<label>Employee Name</label>

<input type="text">

The label tells the user what information the input expects.

Employee Name       ← LABEL

[_______________]   ← INPUT

Another:

<label>Email Address</label>

<input type="email">

So:

LABEL
  ↓
What should I enter?


INPUT
  ↓
Where do I enter it?

But there is a better way to connect them.

3. for and id ⭐ VERY IMPORTANT

Look carefully:

<label for="employeeName">
  Employee Name
</label>

<input
  id="employeeName"
  type="text"
>

Notice:

for="employeeName"

          ↕ MATCH

id="employeeName"

The label's for identifies the input with that id.

Think:

LABEL
for="employeeName"
       │
       │ connects to
       ▼
INPUT
id="employeeName"
Why does this matter?

Click the words:

Employee Name

The browser should focus the associated input.

That's useful for both usability and accessibility.

Rule
<label for="email">

<input id="email">

The values should match.

4. id vs name ⭐

This is something beginners often confuse.

Look:

<input
  id="employeeName"
  name="employeeName"
  type="text"
>

They happen to have the same value here, but they have different jobs.

id

Identifies an element in the HTML document.

Here it also connects the input with:

<label for="employeeName">

So:

id
↓
IDENTIFIES THE ELEMENT
name

Identifies the field when its value participates in form submission.

name
↓
FORM FIELD NAME / KEY

Example:

<input
  id="employeeName"
  name="name"
  type="text"
>

These don't have to match.

The label connects using:

for="employeeName"
        ↓
id="employeeName"

The submitted field is:

name="name"
     ↓
name = Rahul
Easy memory
for ↔ id
      ↓
Label connection


name ↔ entered value
       ↓
Form data

This distinction is very important.

5. value

Suppose:

<input
  type="text"
  name="employeeName"
  value="Rahul"
>

When the page loads, the input already contains:

[ Rahul ]

value represents the input's current/default value in this HTML example.

Conceptually:

name="employeeName"
        ↓
       KEY

value="Rahul"
        ↓
      VALUE

So:

employeeName = Rahul
6. placeholder

Now compare:

<input
  type="text"
  placeholder="Enter employee name"
>

Browser:

[ Enter employee name        ]

The placeholder is a hint inside the input.

Once the user starts typing:

[ Rahul                      ]

the placeholder disappears.

value vs placeholder
value="Rahul"

[ Rahul                 ]

→ actual input value


placeholder="Enter your name"

[ Enter your name       ]

→ temporary hint

And importantly:

A placeholder should not replace a proper <label>.

Good:

<label for="name">
  Employee Name
</label>

<input
  id="name"
  name="name"
  type="text"
  placeholder="Enter employee name"
>
7. type="text"

The most basic input:

<input type="text">

Use it for ordinary single-line text.

Examples:

Name
City
Department
Job title
Username

Employee example:

<label for="employeeName">
  Employee Name
</label>

<input
  id="employeeName"
  name="employeeName"
  type="text"
  placeholder="Enter employee name"
>
8. type="email"

For email:

<input type="email">

Example:

<label for="email">
  Email
</label>

<input
  id="email"
  name="email"
  type="email"
  placeholder="name@example.com"
>

Why not simply use text?

Because email communicates that this field expects an email address and enables browser behaviors such as built-in email-format validation.

We'll study validation properly later.

9. type="password"
<input type="password">

Browser:

Password

[ ••••••••••• ]

The characters are visually obscured.

Example:

<label for="password">
  Password
</label>

<input
  id="password"
  name="password"
  type="password"
>

Important:

password input
      ≠
password encryption

It mainly masks the characters visually. Security still requires things such as HTTPS and proper backend password handling.

10. type="number"

For numeric input:

<input type="number">

Example:

<label for="experience">
  Years of Experience
</label>

<input
  id="experience"
  name="experience"
  type="number"
>

Good for quantities/numeric values such as:

Experience → 8
Quantity   → 5
Age        → 33

But don't use number simply because something contains digits.

For example, a phone number isn't something you mathematically calculate.

That's why we have tel.

11. type="tel"
<input type="tel">

Example:

<label for="phone">
  Phone Number
</label>

<input
  id="phone"
  name="phone"
  type="tel"
  placeholder="+91 98765 43210"
>

Useful for telephone numbers.

On mobile devices, browsers may provide a more suitable keyboard.

12. type="date"
<input type="date">

Example:

<label for="joiningDate">
  Joining Date
</label>

<input
  id="joiningDate"
  name="joiningDate"
  type="date"
>

The browser usually provides a date-selection interface.

Conceptually:

Joining Date

[ 13/08/2026 📅 ]

The exact appearance depends on the browser/device.

13. Checkbox

A checkbox represents an independent yes/no or multiple-selection choice.

<input
  type="checkbox"
  id="remote"
  name="remote"
>

<label for="remote">
  Remote Employee
</label>

Browser:

☐ Remote Employee

Click:

☑ Remote Employee

Another example:

Skills

☑ React
☑ TypeScript
☐ Angular

Multiple checkboxes can be selected.

Think:

CHECKBOX
   ↓
Zero, one, or multiple choices
depending on the form
14. Radio Buttons ⭐

Radio buttons are useful when the user should choose one option from a group.

Example:

<input
  type="radio"
  id="fullTime"
  name="employmentType"
  value="full-time"
>

<label for="fullTime">
  Full Time
</label>


<input
  type="radio"
  id="contract"
  name="employmentType"
  value="contract"
>

<label for="contract">
  Contract
</label>

Notice something very important:

name="employmentType"

name="employmentType"

Both have the same name.

That groups them.

Browser:

Employment Type

○ Full Time
○ Contract

Choose one:

● Full Time
○ Contract

Choose Contract:

○ Full Time
● Contract
Checkbox vs Radio
CHECKBOX

☑ React
☑ Node
☑ MongoDB

Multiple selections possible


RADIO

○ Full Time
● Contract

One selection from the group
15. value becomes especially clear with radio

Look:

<input
  type="radio"
  name="employmentType"
  value="full-time"
>

If selected, conceptually:

employmentType = full-time

Other radio:

<input
  type="radio"
  name="employmentType"
  value="contract"
>

If selected:

employmentType = contract

Now name and value should make much more sense:

name
 ↓
employmentType


value
 ↓
full-time

Together:

employmentType = full-time
16. File Input

For choosing a file:

<input type="file">

Example:

<label for="resume">
  Upload Resume
</label>

<input
  id="resume"
  name="resume"
  type="file"
>

Browser generally displays something like:

Upload Resume

[ Choose File ] No file chosen

Later, file uploading involves additional frontend/backend handling.

For HTML right now, just understand the control.

17. <textarea>

Suppose we need:

Employee Description

[                              ]
[                              ]
[                              ]
[                              ]

A normal text input is single-line.

For larger multi-line text, use:

<textarea></textarea>

Example:

<label for="bio">
  Employee Bio
</label>

<textarea
  id="bio"
  name="bio"
></textarea>

Useful for:

Comments
Messages
Descriptions
Feedback
Bio
Address
Input vs textarea
<input type="text">
       ↓
Single-line text


<textarea>
       ↓
Multi-line text
18. <select> and <option>

Suppose an employee needs a department:

Department

[ Engineering ▼ ]

Use:

<select>

with:

<option>

Example:

<label for="department">
  Department
</label>

<select
  id="department"
  name="department"
>

  <option value="engineering">
    Engineering
  </option>

  <option value="finance">
    Finance
  </option>

  <option value="hr">
    Human Resources
  </option>

</select>

Think:

SELECT
  │
  ├── OPTION → Engineering
  ├── OPTION → Finance
  └── OPTION → Human Resources

If Engineering is selected:

department = engineering

because:

name="department"

and:

value="engineering"
19. disabled

Example:

<input
  type="text"
  value="Employee ID 101"
  disabled
>

A disabled control generally:

cannot be edited,
cannot be focused normally,
and is not included in native form submission.

Think:

disabled
   ↓
Control unavailable
20. readonly

Example:

<input
  type="text"
  name="employeeCode"
  value="EMP101"
  readonly
>

The user can see the value but cannot edit it.

Unlike a disabled input, a readonly input can still participate in native form submission.

disabled vs readonly
DISABLED

Cannot edit
Usually cannot focus
NOT submitted


READONLY

Cannot edit
Can generally focus/select
IS submitted

This distinction is worth remembering.

🛠 Practical — Build Our Employee Form

Now upgrade the employee form you've already created.

<section>

  <h2>Create Employee</h2>

  <form action="/api/employees" method="post">

    <fieldset>

      <legend>Employee Information</legend>

      <label for="employeeName">
        Employee Name
      </label>

      <input
        id="employeeName"
        name="employeeName"
        type="text"
        placeholder="Enter employee name"
      >

      <br><br>


      <label for="employeeEmail">
        Email
      </label>

      <input
        id="employeeEmail"
        name="email"
        type="email"
        placeholder="Enter email address"
      >

      <br><br>


      <label for="password">
        Password
      </label>

      <input
        id="password"
        name="password"
        type="password"
      >

      <br><br>


      <label for="experience">
        Experience
      </label>

      <input
        id="experience"
        name="experience"
        type="number"
      >

      <br><br>


      <label for="phone">
        Phone
      </label>

      <input
        id="phone"
        name="phone"
        type="tel"
      >

      <br><br>


      <label for="joiningDate">
        Joining Date
      </label>

      <input
        id="joiningDate"
        name="joiningDate"
        type="date"
      >

      <br><br>


      <label for="department">
        Department
      </label>

      <select
        id="department"
        name="department"
      >
        <option value="engineering">
          Engineering
        </option>

        <option value="finance">
          Finance
        </option>

        <option value="hr">
          Human Resources
        </option>
      </select>

      <br><br>


      <p>Employment Type</p>

      <input
        id="fullTime"
        name="employmentType"
        type="radio"
        value="full-time"
      >

      <label for="fullTime">
        Full Time
      </label>


      <input
        id="contract"
        name="employmentType"
        type="radio"
        value="contract"
      >

      <label for="contract">
        Contract
      </label>

      <br><br>


      <input
        id="remote"
        name="remote"
        type="checkbox"
      >

      <label for="remote">
        Remote Employee
      </label>

      <br><br>


      <label for="resume">
        Resume
      </label>

      <input
        id="resume"
        name="resume"
        type="file"
      >

      <br><br>


      <label for="bio">
        Employee Bio
      </label>

      <br>

      <textarea
        id="bio"
        name="bio"
      ></textarea>

      <br><br>


      <button type="submit">
        Create Employee
      </button>

    </fieldset>

  </form>

</section>

Don't worry about how ugly this looks. CSS comes later. Right now we're building the correct HTML.

⭐ The 4 Attributes You Must Not Confuse

This is the key takeaway from this section:

<label for="employeeName">
                    │
                    │ connects to
                    ▼
<input id="employeeName"
       name="name"
       value="Rahul">

Read it as:

for
 ↓
Which input does this label belong to?


id
 ↓
Identity of that HTML element


name
 ↓
Field/key used for form submission


value
 ↓
Value associated with that field

Or even shorter:

for ↔ id
Label connection

name ↔ value
Form data

That's the part I want you to understand particularly well before we move forward.





