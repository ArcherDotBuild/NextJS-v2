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