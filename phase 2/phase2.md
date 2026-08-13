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


