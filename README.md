# Intro to NextJS v2

## FrontEnd Masters

#### Teacher: Scott Moss

#### Course website:

https://intro-nextjs-v2-instructions.vercel.app

#### Repos:

- Course repo: https://github.com/btholt/complete-intro-to-react-v8
- Course examples repo: https://github.com/btholt/citr-v8-project

## Section 01: Introduction

## 1. Introduction

#### Things we will be covering

- Creating pages and using the app and pages directories
- Using server and client components together
- Fetching data from server and client compoents
- Configuration
- Styling
- and more...

## 2. NextJS Origins and Q&A

#### Origins

NextJS is a framework built on top of React. You might be asking, Why is there a framework built on top of another framework? Well, as amazing as React is for helping us build applications, it's intentionally missing some things. Things like routing, styling, tooling, and SSR, to name a few. The community has stepped in to create awesome packages for us to build our own frameworks on top of React.

After years of building apps with React, leading opinions and conventions start to form in the community. You can think if NextJS as a framework that incorporates these ideas. The need to install anything on the framework level is almost nonexistent with Next.js. It's also a full-stack framework, allowing us to build out server-side logic and APIs.

#### When should you use NextJS?

NextJS is very flexible, with its many different rendering modes, making it ideal for many scenarios. I love to use Next.js for any React-based site/app I am working on for the web. You wouldn't use NextJS to make a component library or package, as it's designed to help you build applications.

## Section 02: Setup

## 3. Creating a NextJS App

**folder 01**

#### <u>Using create-next-app</u>

The fastest way to get started with NextJS is to use create-next-app. It's very simple, just run:

`npx create-next-app@13.1 --experimental-app`
`npx create-next-app@latest --experimental-app`

**Note: Make sure you pin the NextJS version to 13.1**

This will install the node_modules you need, and setup a basic app with example code for you to get started. I recommend using this approach when starting a NextJS app.

#### <u>Using create-next-app</u>

If you can't or prefer not to use create-next-app, its still very simple to get started. You only have to install a few modules:

`npm i next@latest react@latest react-dom@latest eslint-config-next@latest`

You can then create some scripts in your package.json:

```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "next lint"
}
```

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    appDir: true,
  },
}

module.exports = nextConfig
```

At this point, you have everything you need to get started, anything else from create-next-app is either optional or not necessary (like the example code it creates).

`npm run dev` to run the NextJS project

## 4. File-Based Routing & Params

#### <u>File based routing</u>

NextJS uses file-based routing. This means, instead of configuring a router yourself like you would with react-router or express for an API, you follow a set of folder and file naming conventions and NextJS will configure the router for you. It's really magical.

#### <u>App directory</u>

NextJS 13 introduces the app directory that expects you to follow file based routing. Previously, there was the pages directory (which still works with v13). They both can handle file based routing but in different ways. We'll mostly focus on the app directory and talk about the differences.

All pages must go in the app directory. To create routes, we create a folder with the path of the route, and then a page.{tsx, jsx, js}. For the index route, we don't need a folder, because the URL for the index page doesn't have a path. We can just create the page file.

```javascript
export default function IndexPage() {
  return <div>home</div>
}
```

For a route that has a named path, we can create a folder. For the contact page, we can create a folder called contact. Then in that folder, we can create the page.js file for the route. So you should have this ./app/contact/page.js which will create a route for /contact.

#### <u>Dynamic routes</u>

If your route path has a dynamic segment, we can define that as well. For our blog, the url would look like this /blog/:title. And our file structure will look similar: ./app/blog/[title]/page.js. We can access the route param inside our page component.

#### <u>Grouping</u>

You can group pages together without affecting the URL structure. You might want this to:

- not spam the url with extra segments
- opt into and out of certain layouts
- create multiple root layouts (one for the marketing pages, one for the dashboard, etc.)
  You can do this with the () syntax in the file structure:

  - app
  - (marketing)
    - layout.tsx
    - blog
      - [title]
        - page.tsx
  - (dashboard)
    - layout.tsx
    - home - page.tsx

  Above, we created two groups, marketing and dashboard. Each group has their own root layout, so we don't need a single root layout here. Also, the URL structure is not affected. The path for the dashboard home is /home and not /dashboard/home. And the route for a blog post would be /blog/learn-to-code and not /marketing/blog/learn-to-code. The group folders are ignored by the router. This means you have to be aware of potential route conflicts. If the marketing group had a home folder with a page, it would have a conflicting route with the dashboard home page.

#### <u>Routes for our app</u>

- / - home
- /blog - blog home
- /blog/:title - blog post
- /contact - contact page

## 5. Route Grouping

## Section 03: Rendering

## 6. Layout Components

#### <u>Head component</u>

Our app needs a head component in the app directory. This will hold the meta and title tags for our application.

```javascript
// ./app/head.tsx
export default function Head() {
  return (
    <>
      <title></title>
      <meta name='viewport' content='width=device-width, initial-scale=1' />
    </>
  )
}
```

#### <u>Layouts</u>

Layouts are components that wrap our pages. We can use them when we want to keep certain UI elements on page across routes. Things like a navigation bar, footer, layout, etc. We need to create a root layout. You must have a root layout when using the app directory.

```javascript
// ./app/layout.tsx
export default function RootLayout({ children }) {
  return (
    <html lang='en'>
      <head />
      <body>{children}</body>
    </html>
  )
}
```

#### <u>Nested Layouts</u>

It's required to have a root layout, but we can also have multiple nested layouts that render inside each other. You simply have to create a layout file in the route folder. By default, the layouts will nest.

## 7. Navigation

NextJS does so much behind the scene when it comes to handling navigation for our pages. From the docs:

**How Navigation Works**

- A route transition is initiated using or calling router.push().
- The router updates the URL in the browser’s address bar.
- The router avoids unnecessary work by re-using segments that haven't changed (e.g. shared layouts) from the client-side cache. This is also referred to as partial rendering.
- If the conditions of soft navigation are met, the router fetches the new segment from the cache rather than the server. If not, the router performs a hard navigation and fetches the Server Component payload from the server.
- If created, loading UI is shown from the server while the payload is being fetched.
- The router uses the cached or fresh payload to render the new segments on the client.

#### <u>Using the Link component</u>

In place of an anchor tag, we can use the Link component from next/link to handle click interactions that translate to page navigations. It's easier than ever to use.

```javascript
import Link from 'next/link'

export default function Page() {
  return (
    <div>
      <Link href='/blog'>Blog</Link>
    </div>
  )
}
```

For dynamic routes, we can just interpolate.

```javascript
import Link from 'next/link'

export default function Blog() {
  return (
    <div>
      <Link href={`/blog/${post.title}`}>Post</Link>
    </div>
  )
}
```

#### <u>Programatic usage</u>

If you need to navigate between pages programmatically instead of when a user clicks. First, you must understand that this only works in client components, we'll get there later. NextJS provides a hook we can use that creates a router object for that has helpful navigation methods on it.

```javascript
'use client'

import { useRouter } from 'next/navigation'

export default function Page() {
  const router = useRouter()

  return (
    <button type='button' onClick={() => router.push('/blog')}>
      Blog
    </button>
  )
}
```
