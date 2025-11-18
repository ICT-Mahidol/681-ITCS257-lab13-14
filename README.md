# Faculty of Information and Communication Technology <br/> ITCS257 Frontend Application Development <br/> Advanced React with Next JS
**Lab 10-11 Recall:**
The financial management system comprises three components: invoice, quotation, and receipt. Previously, the invoice management subsystem was built using HTML, CSS, and TypeScript. In this lab, you will implement the quotation management system using React, Tailwind CSS, React Hooks, and shadcn/ui.

***Lab13-14***: Next.js Lab – Refactoring the Quotation Management System

> In the previous lab, you implemented a Quotation Management System using React, Vite, Tailwind CSS, React Hooks, and shadcn/ui

In this lab, you will refactor the same Quotation Management System to use Next.js and take advantage of its built-in features such as:

- App Router & file-based routing

- Shared layouts and metadata

- Server Components & Client Components

- Data fetching in server components / route handlers

- API route proxying & environment variables

- Loading and error UI

## Objectives

After completing this lab, students will be able to:

- Implement a React application with multiple pages and components
- Connect frontend components to APIs using React Hooks
- Create custom hooks for specialized functionality
- Utilize shadcn/ui and Tailwind CSS for responsive user interfaces
- Implement form validation and data filtering features

After completing this lab, you will be able to:
- Set up a Next.js project with App Router and file-based routing.
- Use layouts, nested routes, and metadata to structure a multi-page app.
- Integrate with existing backend APIs using:
    - Server Components (async components, fetch on the server), and/or
    - Route Handlers (app/api/.../route.ts) as a proxy layer.
    - Implement authentication flow (login, logout, protecting quotation pages) using Next.js navigation and state.
- Reuse Tailwind CSS, shadcn/ui, and React Hooks inside a Next.js app.
- Implement form validation, search & filtering, and show loading/error states using Next.js patterns.

## You already have a working Quotation Management System implemented with React + Vite.
Your task now is to migrate that system into a Next.js project while keeping the core features but leveraging Next.js-specific capabilities.

You should still support the same business functions:
- Home Page
- Login Page
- Quotation Page
- Create quotations
    - List quotations
    - Search and filter by:
        - Company name
        - Client email
        - Date range (start and end creation dates)
    - Validate all fields in the quotation creation form
- About Us Page (card with first name, last name, email, avatar)
- Logout (clear tokens / session)

## Project Setup Next.js
### Task 1 – Create a Next.js App

1.  Create a new Next.js project (App Router):
```
npx create-next-app@latest quotation-next --typescript --eslint
```
2. Confirm that you are using the app/ directory (App Router).
3. Install the required dependencies:
    - Tailwind CSS (follow official Next.js + Tailwind setup); read 4.
    - shadcn/ui (install and configure as in the previous lab)
4. Configure Tailwind to work with the app/ directory.
    - Note that if you use default setup, Tailwind CSS would come with the nextjs project by default
5. Set up a base layout in app/layout.tsx including:
    - Common ```<html>``` and ```<body>``` structure
    - A top navigation bar with links to:
    - ```/``` (Home)
    - ```/login```
    - ```/quotations```
    - ```/about```
    - ```/logout``` (or a logout button)

### Task 2 - Routing & Layout Structure
Use ```file-based``` routing and App Router features.

#### Task 2.1 - Create the following routes and components:

- ```app/page.tsx``` → Home Page

- ```app/login/page.tsx``` → Login Page

- ```app/quotations/page.tsx``` → Quotation List & Search

- ```app/quotations/new/page.tsx``` → Create Quotation Page (optional: modal or separate page)

- ```app/about/page.tsx``` → About Us Page

You may organize nested routes such as:
```
app/
  layout.tsx
  page.tsx                 // Home
  login/
    page.tsx
  quotations/
    page.tsx               // list + search
    new/
      page.tsx             // create quotation
  about/
    page.tsx
```

#### Task 2.2 – Shared Layout & Metadata
Use app/layout.tsx for shared layout (navigation, fonts, theme).

Add metadata (title, description) using:
```
export const metadata = {
  title: 'Quotation Management System Lab13-14',
  description: 'Lab10-11 upgraded with Next.js  version of the quotation management system',
}
```

### Task 3 - Data Fetching & API Integration

You must still connect to the provided backend APIs, but now via ```Next.js``` patterns.

#### Task 3.1 – Environment Variables
Add an ```.env.local``` file containing:
```
NEXT_PUBLIC_API_BASE_URL=<your-backend-url>
```
- Use this variable in API calls instead of hardcoding the base URL.
- Consult the documentations: https://nextjs.org/docs/pages/guides/environment-variables

#### Task 3.2 – Route Handler as API Proxy (Recommended)
Create route handlers under ```app/api/``` to proxy calls to the existing backend APIs:

Examples:
- ```app/api/login/route.ts``` – forward login requests to the external login API.

- ```app/api/quotations/route.ts``` – handle:
- ```GET``` for listing & search
- ```POST``` for creating quotations

This lets your frontend call relative URLs like ```/api/quotations``` instead of external URLs.
> Must do! This is a checkpoint for grading with LAs

> In the future, you may choose to call external APIs directly from the client or server components,
but using route handlers is recommended to centralize logic and hide secrets.

#### Task 3.3 Server Components vs Client Components
- Implement quotation listing ```(/quotations)``` as a Server Component (default in App Router):
    - Use ```async function QuotationsPage()``` and call ```await fetch(...)``` on the server side.
    - Render the initial list of quotations on the server.
- Implement interactive parts (filters, search form, create form) as Client Components:
    - Mark them with ```"use client"``` at the top.
    - Accept data from the parent server component via props or fetch on the client if needed.
    - Ref: https://nextjs.org/docs/app/getting-started/server-and-client-components

### Task 4 - Pages & Requirements

#### Task 4.1 Home Page (/)
- Introduce the system & explain this is ```the Next.js``` version of your Quotation Management System.
- Provide buttons/links to:
    - Go to Login

    - View Quotations

    - View About Us

#### Task 4.2 Login Page (/login)

Implement a login form that:
- Submits to your ```/api/login``` route handler (or directly to the backend).
- Stores tokens / session information in:
    - Cookie, or
    - localStorage 
> Explain LAs about trade-offs in your reflection (CheckPoint)

#### Task 4.3 Quotation Page (/quotations)
This page must include:
1. Quotation List

- Display quotations in a table or cards using shadcn/ui components.

2. Search & Filtering

- Filter by:

    - Company name

    - Client email

    - Date range (start & end creation dates)

    - Bind search inputs to query parameters (optional: use ```useSearchParams()```).
    - Note: https://nextjs.org/docs/app/api-reference/functions/use-search-params

- Call your ```/api/quotations``` route with query parameters.

3. Loading & Error States

- Show a loading UI (preferably using loading.tsx in ```app/quotations/```).

- Show an error state (optionally using ```error.tsx``` in ```app/quotations/```).

4. Navigation to Create Page

- A button ```“Create Quotation”``` linking to ```/quotations/new```

#### Task 4.4 Create Quotation Page (/quotations/new)
- Implement a quotation form using ```shadcn/ui``` components:

    - All fields must have validation (required fields, email format, numeric fields, etc.).
-On submit:

    - Call ```/api/quotations``` with ```POST```.

    - Show ```success``` or ```error``` message.

    - Redirect back to ```/quotations``` upon success.

#### Task 4.5 About Us Page (/about)
- Show implementer information in card format:

    - First name, last name

    - Email

    - Avatar (image)
    
- Use shadcn/ui Card components.

- This page can be a Server Component or Client Component.

#### Task 4.6 Logout Function (```/logout``` or ```Logout button```)
- Clear any stored token/session (cookie or localStorage).
- Redirect user to /login or /
- Make sure protected pages (like /quotations) cannot be accessed without login.

> LA's checkpoint here, After this it is optional but a practical example for your portfolio

#### Task 4.7 Authentication & Route Protection (++ but Recommended)
##### Task 4.7.1 – Simple Protection
At minimum:

- If no token is found, ```/quotations``` and ```/quotations/new``` should redirect to ```/login```.
- You can implement this logic:

    - In the ```Server Component``` (checking cookies), or

    - In a Client Component wrapper using ```useEffect```.

##### Task 4.7.2 – Middleware (Advanced Option)
Optionally, use ```middleware.ts``` to protect certain routes (```/quotations```, ```/quotations/new```) by checking cookies and redirecting unauthenticated users.

#### Task 4.8 Authentication & Route Protection (++ but Recommended)
Demonstrate React Hooks usage within the Next.js app:

- Use at least three built-in hooks:
> e.g., useState, useEffect, useMemo, useRouter, useSearchParams, etc.

- Implement at least one custom hook, for example:
```useAuth()``` – handle login state (get/set token, check authenticated).

```useQuotationSearch()``` – manage search filters and query building.

```useApi()``` – generic API caller with loading & error state.

Make sure the hook is reusable and used in more than one place if possible.

#### Task 4.9 UI & Design Expectations
- Use Tailwind CSS and shadcn/ui to match or closely follow the UI from your previous lab.
- The layout and styling should feel consistent:
    - Responsive layout
    - Proper spacing, typography, and colors

Make use of: shadcn/ui buttons, inputs, cards, dialogs, etc.
- Tailwind utility classes for layout and responsiveness.

#### Task 4.10 Extra Tasks (Optional Challenges)

If you finish early or want extra practice, try:

- Update / Delete quotations:

    - Add edit and delete functionalities using additional route handlers:

        - ```PUT``` ```/api/quotations/:id```

        - ```DELETE``` ```/api/quotations/:id```

- Pagination or Infinite Scroll:
    - Implement pagination controls for the quotation list.
- Better UX
    - Show toast notifications on success/error.
    - Confirm dialog before deleting quotations.


## Submission

1. **Include a Generative AI usage declaration and reflection** at the beginning of your code file. Clearly state if AI tools were used and briefly reflect on your work.
2. **Push your code** to the provided GitHub Classroom repository for this assignment. Make sure all your code is committed and pushed before the submission deadline.
3. Submit the lab by the end of the next class session to the LAs. Late submissions may not be accepted.

## AI Usage Declaration and Reflection

Students must add an AI Declaration and a Reflection of Today's Learning to their MYREADME.md file.

A reflection is not a summary of what you did or what the AI generated.
Instead, it is a personal explanation of your learning process.

- If you used AI, focus on how AI impacted your learning or understanding of the code.
- If you did not use AI, focus on your learning, tools, and experience from the lab.

Here are examples:

### Example 1 – No AI Used

```tsx
/*
AI Declaration:
No Generative AI tools were used for this lab.
All code was written manually by the student.

Reflection:
[ Your Reflection goes here
Today’s lab helped me learn [key takeaway].
I practiced ...
]
*/

```

### Example 2 – AI Used for Reference

```tsx
/*
AI Declaration:
I used ChatGPT only to clarify HTML semantic tags.
No code was directly copied without modification.

Reflection:
[ Write 1–2 sentences reflecting on your learning or how AI impacted your understanding]
*/

```

### Example 3 – AI Assisted in Debugging

```tsx
/*
AI Declaration:
I used ChatGPT to help debug the table structure in my invoice layout.
I wrote all the other code, and I understand the entire implementation.

Reflection:
[ Write 1–2 sentences reflecting on your learning or how AI impacted your understanding ]
*/

```
