# React JavaScript Setup Guide

A professional, production-ready baseline for building modern React applications with:

- React
- JavaScript
- Vite
- ESLint
- React Hooks ESLint
- React Refresh ESLint
- JSX accessibility linting
- Prettier
- Git
- npm
- VS Code

This guide intentionally uses **JavaScript**, not TypeScript.

---

# 0. Prerequisites

Install:

- Node.js
- npm
- Git
- VS Code

---

## Install Node.js

Download Node.js from the official website:

https://nodejs.org/en/download

Check the installation:

```bash
node -v
npm -v
```

Vite currently requires Node.js `20.19+` or `22.12+` or newer supported versions. Always use a currently supported Node.js LTS release for development.

---

## Install Git

Download Git for Windows:

https://git-scm.com/install/windows

Check the installation:

```bash
git --version
```

---

# 1. Create the React + JavaScript Project

Create a React project using Vite:

```bash
npm create vite@latest react-project-name -- --template react --eslint
```

The important parts are:

```text
react
```

for the JavaScript React template, and:

```text
--eslint
```

to explicitly select ESLint.

Current Vite React templates can use either Oxlint or ESLint. The `--eslint` option explicitly selects ESLint.

> If your installed `create-vite` version uses an interactive prompt instead, select **React → JavaScript → ESLint**.

---

# 2. Move Into the Project

```bash
cd react-project-name
```

---

# 3. Install Dependencies

If dependencies were not installed automatically:

```bash
npm install
```

Check the project:

```bash
npm run dev
```

Vite will start the development server.

Usually the application will be available at:

```text
http://localhost:5173
```

---

# 4. Initial Project Structure

A Vite React JavaScript project will look approximately like:

```text
react-project-name/
│
├── public/
│
├── src/
│   ├── assets/
│   ├── App.css
│   ├── App.jsx
│   ├── index.css
│   └── main.jsx
│
├── .gitignore
├── eslint.config.js
├── index.html
├── package-lock.json
├── package.json
├── README.md
└── vite.config.js
```

The exact generated files can change as Vite evolves.

---

# 5. Understand the Project Architecture

The basic execution flow is:

```text
index.html
     │
     ▼
src/main.jsx
     │
     ▼
React createRoot()
     │
     ▼
<App />
     │
     ▼
React Components
     │
     ▼
Browser DOM
```

Vite provides:

```text
Development server
HMR
JavaScript transformation
Asset handling
Production bundling
```

React provides:

```text
Components
State
Props
Events
Rendering
Hooks
```

ESLint provides:

```text
Code-quality checks
React Hooks checks
React Refresh checks
Potential bug detection
```

Prettier provides:

```text
Consistent code formatting
```

---

# 6. Configure `main.jsx`

A production-oriented entry point can use an explicit root-element check:

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";

import App from "./App";
import "./index.css";

const rootElement = document.getElementById("root");

if (!rootElement) {
  throw new Error('Root element with id "root" was not found.');
}

createRoot(rootElement).render(
  <StrictMode>
    <App />
  </StrictMode>,
);
```

## Why use `StrictMode`?

React recommends wrapping newly created applications in `StrictMode`.

It enables additional development-only checks for problems such as:

- impure rendering
- missing Effect cleanup
- missing ref cleanup
- deprecated APIs

These checks do not affect the production build.

---

# 7. ESLint

Current ESLint uses the modern **flat configuration** system.

The configuration file is:

```text
eslint.config.js
```

ESLint's flat configuration supports:

```text
eslint.config.js
eslint.config.mjs
eslint.config.cjs
```

and the configuration belongs in the project root.

The Vite React JavaScript template already creates an ESLint configuration when ESLint is selected.

The current Vite configuration includes:

- `@eslint/js`
- `eslint`
- `eslint-plugin-react-hooks`
- `eslint-plugin-react-refresh`
- `globals`

for the JavaScript React template.

---

# 8. Install Additional ESLint Dependencies

For a stronger production setup, install JSX accessibility linting:

```bash
npm install --save-dev eslint-plugin-jsx-a11y
```

This adds accessibility-oriented linting for JSX.

---

# 9. Recommended ESLint Configuration

Use a flat configuration similar to:

```js
import js from "@eslint/js";
import globals from "globals";
import jsxA11y from "eslint-plugin-jsx-a11y";
import reactHooks from "eslint-plugin-react-hooks";
import reactRefresh from "eslint-plugin-react-refresh";
import { defineConfig, globalIgnores } from "eslint/config";

export default defineConfig([
  globalIgnores(["dist/", "coverage/", "node_modules/"]),

  {
    files: ["**/*.{js,jsx}"],

    extends: [
      js.configs.recommended,
      reactHooks.configs.flat.recommended,
      reactRefresh.configs.vite,
      jsxA11y.flatConfigs.recommended,
    ],

    languageOptions: {
      globals: globals.browser,

      parserOptions: {
        ecmaFeatures: {
          jsx: true,
        },
      },
    },
  },
]);
```

### What each configuration provides

```text
js.configs.recommended
        ↓
JavaScript quality rules

reactHooks.configs.flat.recommended
        ↓
React Hooks rules

reactRefresh.configs.vite
        ↓
React Fast Refresh compatibility

jsxA11y.flatConfigs.recommended
        ↓
JSX accessibility rules
```

The official React Hooks plugin provides a flat `recommended` configuration for enforcing the Rules of React and related best practices.

---

# 10. Run ESLint

Run ESLint:

```bash
npm run lint
```

Or directly:

```bash
npx eslint .
```

---

# 11. Automatically Fix ESLint Problems

Some ESLint problems can be automatically fixed:

```bash
npx eslint . --fix
```

Add an npm script:

```json
"lint:fix": "eslint . --fix"
```

Then:

```bash
npm run lint:fix
```

---

# 12. Install Prettier

Install Prettier locally:

```bash
npm install --save-dev --save-exact prettier
```

Using `--save-exact` keeps the project on the exact Prettier version specified in `package.json`.

---

# 13. Install `eslint-config-prettier`

Install:

```bash
npm install --save-dev eslint-config-prettier
```

This disables ESLint rules that conflict with Prettier.

Import it:

```js
import eslintConfigPrettier from "eslint-config-prettier";
```

Then put it **last** in the ESLint configuration:

```js
extends: [
  js.configs.recommended,
  reactHooks.configs.flat.recommended,
  reactRefresh.configs.vite,
  jsxA11y.flatConfigs.recommended,
  eslintConfigPrettier,
],
```

The principle is:

```text
ESLint
  ↓
Code quality

Prettier
  ↓
Code formatting

eslint-config-prettier
  ↓
Prevents conflicting ESLint formatting rules
```

---

# 14. Create `.prettierrc`

Create:

```text
.prettierrc
```

Recommended configuration:

```json
{
  "semi": true,
  "singleQuote": false,
  "trailingComma": "all",
  "tabWidth": 2,
  "printWidth": 80
}
```

### Explanation

| Option          | Purpose                                      |
| --------------- | -------------------------------------------- |
| `semi`          | Adds semicolons                              |
| `singleQuote`   | Uses double quotes                           |
| `trailingComma` | Adds trailing commas where supported         |
| `tabWidth`      | Uses 2 spaces                                |
| `printWidth`    | Targets approximately 80 characters per line |

These are style decisions. The important production practice is to choose one consistent configuration and let Prettier enforce it.

---

# 15. Create `.prettierignore`

Create:

```text
.prettierignore
```

Add:

```text
node_modules/
dist/
coverage/
```

Generated files and dependencies should not be formatted.

---

# 16. Configure npm Scripts

A professional baseline:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "lint": "eslint .",
    "lint:fix": "eslint . --fix",
    "format": "prettier . --write",
    "format:check": "prettier . --check",
    "check": "npm run lint && npm run format:check",
    "preview": "vite preview"
  }
}
```

---

# 17. Understand the npm Scripts

## Development

```bash
npm run dev
```

Starts the Vite development server with HMR.

---

## Production Build

```bash
npm run build
```

Runs:

```bash
vite build
```

and creates:

```text
dist/
```

Vite's production build generates the optimized application assets for deployment.

---

## Lint

```bash
npm run lint
```

Checks JavaScript and JSX for ESLint violations.

---

## Fix ESLint

```bash
npm run lint:fix
```

Automatically fixes problems that ESLint can safely fix.

---

## Format

```bash
npm run format
```

Runs Prettier and formats the project.

---

## Check Formatting

```bash
npm run format:check
```

Checks whether files are correctly formatted without modifying them.

This is especially useful in CI.

---

## Run All Static Checks

```bash
npm run check
```

Runs:

```text
ESLint
   ↓
Prettier check
```

---

## Preview Production Build

```bash
npm run preview
```

This serves the production build locally so you can inspect it.

> `vite preview` is for local previewing and is not intended to be your production server.

---

# 18. Environment Variables

Vite supports environment files such as:

```text
.env
.env.local
.env.development
.env.development.local
.env.production
.env.production.local
```

For a frontend application, a good local-development choice is:

```text
.env.local
```

Example:

```env
VITE_API_BASE_URL=https://api.example.com
```

Use it:

```js
const apiBaseUrl = import.meta.env.VITE_API_BASE_URL;
```

---

# 19. Critical Environment Variable Security Rule

**Never assume a Vite environment variable is secret.**

Variables beginning with:

```text
VITE_
```

are exposed to client-side application code.

Therefore, do **not** put these into frontend environment variables:

```text
❌ Database passwords
❌ Private API keys
❌ Secret tokens
❌ Service-account credentials
❌ Private encryption keys
```

For example:

```env
VITE_SECRET_API_KEY=super-secret-value
```

is **not actually secret** once included in the frontend application.

Private credentials belong on a backend/server/serverless function.

---

# 20. Environment Variable Example

Create:

```text
.env.example
```

Example:

```env
VITE_API_BASE_URL=
```

Create your local file:

```text
.env.local
```

Example:

```env
VITE_API_BASE_URL=https://api.example.com
```

Commit:

```text
.env.example
```

Do not commit:

```text
.env.local
```

---

# 21. Configure `.gitignore`

A professional `.gitignore` should contain at least:

```gitignore
node_modules/
dist/
coverage/

.env.local
.env.*.local
```

You may also ignore local editor/OS files:

```gitignore
.DS_Store
Thumbs.db
```

---

# 22. Git Initialization

If Git was not initialized:

```bash
git init
```

Check:

```bash
git status
```

Stage files:

```bash
git add .
```

Create the initial commit:

```bash
git commit -m "chore: initialize React JavaScript project"
```

---

# 23. Recommended `package.json`

A professional project can have:

```json
{
  "name": "react-project-name",
  "private": true,
  "version": "0.0.0",
  "type": "module",

  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "lint": "eslint .",
    "lint:fix": "eslint . --fix",
    "format": "prettier . --write",
    "format:check": "prettier . --check",
    "check": "npm run lint && npm run format:check",
    "preview": "vite preview"
  }
}
```

The current Vite React template also uses `"type": "module"` and the `dev`, `build`, and `preview` scripts.

---

# 24. Why `"type": "module"`?

```json
"type": "module"
```

tells Node.js to treat `.js` files in the package as ECMAScript modules.

This allows modern syntax such as:

```js
import something from "./something.js";
```

and:

```js
export default something;
```

Vite's current React template uses ESM configuration.

---

# 25. React Compiler

The current Vite scaffolder also provides a React Compiler template:

```bash
npm create vite@latest react-project-name -- --template react-compiler
```

However, **do not enable the React Compiler simply because it is available**.

The standard React Vite template does not enable it by default because of development/build performance considerations.

For a learning project or normal production application, start with:

```text
React
+
Vite
+
ESLint
+
Prettier
```

Add React Compiler deliberately when you understand:

- what problem it solves
- its current limitations
- compiler configuration
- build/dev performance
- compatibility with your application

---

# 26. Accessibility Linting

For professional frontend development, accessibility should be part of development rather than an afterthought.

The JSX accessibility ESLint plugin can catch issues involving common HTML/ARIA usage.

For example, problematic JSX such as:

```jsx
<div onClick={handleClick}>Click me</div>
```

may indicate a semantic/accessibility problem.

Prefer appropriate semantic HTML:

```jsx
<button type="button" onClick={handleClick}>
  Click me
</button>
```

Linting is not a replacement for accessibility testing, but it provides an important automated safety net.

---

# 27. ESLint vs Prettier

Do not give both tools the same responsibility.

## ESLint

Use ESLint for:

```text
Code-quality problems
Potential bugs
Bad patterns
React Hooks rules
React Refresh rules
Accessibility rules
```

## Prettier

Use Prettier for:

```text
Formatting
Indentation
Quotes
Semicolons
Line wrapping
Trailing commas
```

Think of the architecture as:

```text
ESLint
   │
   └── "Is this code healthy?"

Prettier
   │
   └── "Is this code consistently formatted?"
```

---

# 28. No TypeScript Type-Check

Because this is a **JavaScript project**, do not add:

```bash
tsc
```

or:

```bash
npm run type-check
```

There is no TypeScript compiler in this setup.

Your static validation is:

```text
ESLint
   +
Prettier
   +
Production build
```

For JavaScript, ESLint is the primary static code-quality tool in this setup.

---

# 29. Production Build Validation

Before deployment:

```bash
npm run lint
```

Then:

```bash
npm run format:check
```

Then:

```bash
npm run build
```

Recommended:

```text
npm run lint
       ↓
npm run format:check
       ↓
npm run build
```

If all three pass:

```text
✓ ESLint
✓ Prettier
✓ Production build
```

the project has passed the basic local quality gate.

---

# 30. Recommended One-Command Check

Use:

```json
"check": "npm run lint && npm run format:check && npm run build"
```

Then:

```bash
npm run check
```

This performs:

```text
ESLint
   ↓
Prettier
   ↓
Vite production build
```

This is a useful pre-push/CI quality gate.

---

# 31. CI/Production Dependency Installation

When installing dependencies in CI or deployment environments and a `package-lock.json` exists, prefer:

```bash
npm ci
```

instead of:

```bash
npm install
```

`npm ci` is designed for clean, reproducible dependency installation from the lockfile.

Then run:

```bash
npm run check
```

---

# 32. Recommended Development Workflow

During development:

```bash
npm run dev
```

While working, periodically run:

```bash
npm run lint
```

and:

```bash
npm run format
```

Before committing:

```bash
npm run check
```

Before deployment:

```bash
npm run build
```

---

# 33. Complete Development Workflow

```text
Write code
    │
    ▼
npm run lint
    │
    ▼
Fix code-quality problems
    │
    ▼
npm run format
    │
    ▼
npm run format:check
    │
    ▼
npm run build
    │
    ▼
Test production build
    │
    ▼
npm run preview
    │
    ▼
git status
    │
    ▼
git add .
    │
    ▼
git commit
    │
    ▼
git push
```

---

# 34. Final Project Structure

A professional React JavaScript project can look like:

```text
react-project-name/
│
├── public/
│
├── src/
│   ├── assets/
│   ├── App.css
│   ├── App.jsx
│   ├── index.css
│   └── main.jsx
│
├── .env.example
├── .env.local
├── .gitignore
├── .prettierignore
├── .prettierrc
│
├── eslint.config.js
├── index.html
│
├── package-lock.json
├── package.json
│
└── vite.config.js
```

As the application grows, you can introduce a feature-oriented structure such as:

```text
src/
├── assets/
├── components/
├── features/
├── hooks/
├── layouts/
├── pages/
├── services/
├── utils/
├── App.jsx
├── index.css
└── main.jsx
```

Do not create every directory on day one. Introduce architectural boundaries when the application actually needs them.

---

# 35. Tool Responsibilities

## React

Responsible for:

```text
Components
Rendering
State
Props
Events
Hooks
UI
```

## JavaScript

Responsible for:

```text
Application logic
Data manipulation
Functions
Objects
Async operations
Browser interaction
```

## Vite

Responsible for:

```text
Development server
HMR
Asset processing
Production bundling
```

## ESLint

Responsible for:

```text
Code quality
Potential bugs
React Hooks
React Refresh
Accessibility
```

## Prettier

Responsible for:

```text
Code formatting
Consistent style
Whitespace
Line wrapping
```

## Git

Responsible for:

```text
Version control
History
Branches
Collaboration
```

---

# 36. Important Production Rules

## Rule 1 — Keep Development Tools Local

Install project tools as `devDependencies`.

For example:

```bash
npm install -D eslint prettier eslint-config-prettier
```

Do not depend on globally installed ESLint or Prettier versions for the project.

---

## Rule 2 — Don't Commit `node_modules`

Use:

```gitignore
node_modules/
```

Dependencies can be restored with:

```bash
npm ci
```

---

## Rule 3 — Don't Commit Build Output

Normally:

```gitignore
dist/
```

The production output can be regenerated:

```bash
npm run build
```

---

## Rule 4 — Don't Commit Secrets

Never commit:

```text
.env.local
.env.*.local
```

and never expose private credentials through:

```text
VITE_*
```

---

## Rule 5 — Use Semantic HTML

Prefer:

```html
<button>
  <nav>
    <main>
      <header>
        <footer>
          <form>
            <label></label>
          </form>
        </footer>
      </header>
    </main>
  </nav>
</button>
```

over using generic `<div>` elements for everything.

This improves accessibility, maintainability, and browser semantics.

---

## Rule 6 — Keep `StrictMode` Enabled

Use:

```jsx
<StrictMode>
  <App />
</StrictMode>
```

React recommends this for newly created applications.

Do not remove it simply because development renders/effects behave differently.

Understand why Strict Mode does those checks instead.

---

## Rule 7 — Don't Add Libraries Without a Reason

A production project does not need dozens of dependencies immediately.

Start with:

```text
React
Vite
ESLint
Prettier
```

Then add libraries when the application's requirements justify them.

---

# 37. Quick Command Reference

| Purpose                                  | Command                                                      |
| ---------------------------------------- | ------------------------------------------------------------ |
| Create React + JavaScript + Vite project | `npm create vite@latest my-app -- --template react --eslint` |
| Enter project                            | `cd my-app`                                                  |
| Install dependencies                     | `npm install`                                                |
| Start development server                 | `npm run dev`                                                |
| Run ESLint                               | `npm run lint`                                               |
| Fix ESLint issues                        | `npm run lint:fix`                                           |
| Format project                           | `npm run format`                                             |
| Check formatting                         | `npm run format:check`                                       |
| Run all checks                           | `npm run check`                                              |
| Production build                         | `npm run build`                                              |
| Preview production build                 | `npm run preview`                                            |
| Install dependencies in CI               | `npm ci`                                                     |
| Initialize Git                           | `git init`                                                   |
| Check Git status                         | `git status`                                                 |
| Stage files                              | `git add .`                                                  |
| Commit                                   | `git commit -m "message"`                                    |

---

# 38. Final Recommended Stack

```text
React
  +
JavaScript
  +
Vite
  +
ESLint
  ├── JavaScript recommended rules
  ├── React Hooks
  ├── React Refresh
  └── JSX Accessibility
  +
Prettier
  +
Git
  +
npm
```

The resulting quality pipeline is:

```text
                 React Application
                        │
                        ▼
                    JavaScript
                        │
             ┌──────────┴──────────┐
             ▼                     ▼
          ESLint                Prettier
             │                     │
       Code quality            Formatting
             │                     │
             └──────────┬──────────┘
                        ▼
                    Vite Build
                        │
                        ▼
                      dist/
                        │
                        ▼
                    Deployment
```

---

# Final Pre-Push Checklist

Run:

```bash
npm ci
```

Then:

```bash
npm run lint
```

Then:

```bash
npm run format:check
```

Then:

```bash
npm run build
```

Or simply:

```bash
npm run check
```

if `check` includes all three.

Final result:

```text
✓ Dependencies installed
✓ ESLint passed
✓ Prettier passed
✓ Production build passed
✓ Ready for Git commit
```

---

# Complete Setup Flow

```text
Install Node.js
       │
       ▼
Install Git
       │
       ▼
Create React + JavaScript + Vite project
       │
       ▼
Select ESLint
       │
       ▼
npm install
       │
       ▼
Configure main.jsx
       │
       ▼
Enable React StrictMode
       │
       ▼
Configure ESLint
       │
       ├── JavaScript rules
       ├── React Hooks
       ├── React Refresh
       └── JSX Accessibility
       │
       ▼
Install Prettier
       │
       ▼
Install eslint-config-prettier
       │
       ▼
Configure .prettierrc
       │
       ▼
Configure .prettierignore
       │
       ▼
Configure .env.local
       │
       ▼
Configure .gitignore
       │
       ▼
Configure npm scripts
       │
       ▼
npm run lint
       │
       ▼
npm run format:check
       │
       ▼
npm run build
       │
       ▼
npm run preview
       │
       ▼
Git commit
       │
       ▼
Git push
```

This is the recommended **React + JavaScript + Vite baseline** before adding application-specific technologies such as React Router, state-management libraries, API clients, testing frameworks, Tailwind CSS, component libraries, or backend integration.
