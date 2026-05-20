# Project Setup Guide for any AI reading this

version = 1.0

## React + Vite

### Structure overview
```
monorepo-name/
    backend/
        src/
            lambdas/
    frontend/
        public/
            assets/
            icons/
        src/
            App.jsx
            main.jsx
            index.css
            customScroll.css
        .gitignore
        eslint.config.js
        index.html
        package.json
        vite.config.js
    package.json
    pnpm-workspace.yaml
    pnpm-lock.yaml
```

### Setup pnpm

1) Run in terminal:

```bash
corepack enable pnpm
```

Before continuing, check for the latest official stable version.
If the latest official version was released more than a day ago, use:

```bash
corepack use pnpm@latest-11
```

Otherwise, pin the previous stable version (that was uploaded more than a day ago):

```bash
corepack use pnpm@11.X.X
```

Notes:
- Version 11 is an example, try to use the most up-to-date versions
- In the generated package.json, remove the ```+sha512...``` part if generated. Leave the version clean

### Setup root folder

1) Edit package.json, it should look like this:

```json
{
    "name": "$monorepo-name",
    "version": "1.0.0",
    "type": "module",
    "private": true,
    "packageManager": "pnpm@11.1.2",
    "scripts": {
        "dev:frontend": "pnpm --filter frontend dev"
    }
}
```

2) Create pnpm-workspace.yaml, it should look like this:

```yaml
# Folders that have dependencies and need to generate node_modules
packages:
  - frontend
  - backend/src/lambdas/*

minimumReleaseAge: 10080 # Update/Import a package if more than this time has passed since it was released (1 week)
minimumReleaseAgeIgnoreMissingTime: false # This makes pnpm fail if registry metadata does not include publish time info

savePrefix: "" # This makes pnpm save exact package versions (remove ^)
engineStrict: true # This makes pnpm refuse to install packages that say they are not compatible with your Node.js version
strictPeerDependencies: true # This makes installs fail when there are missing or invalid peer dependencies
autoInstallPeers: false # Don’t magically add non-optional peer dependencies for me; make me declare what I actually use
strictDepBuilds: true # This makes installation fail if dependencies have unreviewed build scripts/postinstall scripts
failIfNoMatch: true # Ensures that pnpm fails if no packages/folders match the provided workspace filters

# Some packages need to install/run scripts (possible supply-chain attack). Those approved with `pnpm approve-builds` are added here.
allowBuilds:


```

3) Remove node_modules folder

### Setup frontend

1) Run in terminal:

```bash
pnpm create vite frontend --template react
```

2) Remove this files in the frontend folder:
- README.md
- public/*
- node_modules
- src/*

3) Edit .gitignore, it should look like this:

```gitignore
# macOS
.DS_Store
.DS_Store?
.AppleDouble
.LSOverride
._*
.Spotlight-V100
.Trashes

# Windows
Thumbs.db
ehthumbs.db
Desktop.ini
$RECYCLE.BIN/

# Visual Studio Code
.vscode/
.vscode-react/
.vscode-react-snippets/
.vscode-react-devtools/
.history/
*.vsix

# Logs
.log
logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*
lerna-debug.log*

# Env
.env
.env.*
!.env.example

# Dependencies
node_modules/
/.pnp
.pnp.js

# Pnpm
.pnpm
.pnpm-store
pnpm-lock.yaml

# AWS
.aws-sam/

# Testing
/coverage

# React + Vite
dist/
build/
.vite/
.vitepress/cache/
.idea/
*.suo
*.ntvs*
*.njsproj
*.sln
*.sw?

# Graphify
graphify-out/manifest.json
graphify-out/cost.json
graphify-out/cache/
graphify-out/.graphify_*
```

4) Edit index.html, it should look like this:

```html
<!doctype html>
<html lang="en" class="custom-scroll">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />

        <!-- Website -->
        <title>Title</title>
        <meta name="description" content="" />
        <meta name="keywords" content="" />
        <meta name="robots" content="index,follow" />
        <meta name="referrer" content="no-referrer-when-downgrade" />
        <meta name="application-name" content="" />
        <meta name="theme-color" content="#ffffff" />
        <meta name="color-scheme" content="only light" />
        <meta name="supported-color-schemes" content="light" />
        <meta name="format-detection" content="telephone=no" />
    </head>
    <body class="custom-scroll">
        <noscript>This application requires JavaScript to function correctly.</noscript>
        <div id="root"></div>
        <script type="module" src="/src/main.jsx"></script>
    </body>
</html>
```

5) Edit package.json, it should look like this (Make sure to remove the caret ^ in all dependencies):

```json
{
    "name": "frontend",
    "version": "1.0.0",
    "type": "module",
    "private": true,
    "scripts": {
        "dev": "vite --host --mode dev",
        "build": "vite build",
        "lint": "eslint .",
        "preview": "vite preview"
    },
    "dependencies": {
        "react": "XX.X.X",
        "react-dom": "XX.X.X"
    },
    "devDependencies": {
        "@eslint/js": "XX.X.X",
    }
}
```

6) Create src/main.jsx, it should look like this:

```jsx
import "./index.css";
import "./customScroll.css"
import { createRoot } from "react-dom/client";
import { App } from "./App.jsx";

createRoot(document.getElementById("root")).render(
    <App />
)
```

7) Create src/index.css, it should look like this:

```css
* {
    padding: 0;
    margin: 0;

    overscroll-behavior: none;

    box-sizing: border-box;

    text-decoration: none;
    list-style: none;
    line-height: 1.5;
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

:root {
    --white: white;
    --black: black;
}

html {
    font-size: 62.5%;
    background-color: var(--white); /* Chrome paints the scrollbar gutter using background-color. */
}

.app {
    width: 100%;
    min-height: 100dvh;

    display: flex;
    flex-flow: column nowrap;
    justify-content: flex-start;
    align-items: center;
    gap: 2rem;
    background: var(--white);
}

a {
    color: inherit;
    text-decoration: none;
}

input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
    appearance: none;
    -webkit-appearance: none;
}
```

8) Create src/App.jsx, it should look like this:

```jsx
import { useEffect, useState } from "react";

export const App = () => {
    return (
        <>
            <div className="app">
                <h1>Hello World</h1>
            </div>
        </>
    );
}
```

9) Create src/customScroll.css, it should look like this:

```css
.custom-scroll {
    /* Size */
    --scrollbar-size: 8px;
    --scrollbar-radius: 999px;
    --scrollbar-thumb-border: 2px;

    /* Colors */
    /* --scrollbar-thumb-color and --scrollbar-thumb-hover-color should have the same color */
    --scrollbar-track-color: var(--white, rgba(0, 0, 0, 0.25)); /* Color del fondo */
    --scrollbar-thumb-color: var(--black, rgba(140, 140, 140, 0.55)); /* Color de la barra cuando el mouse no está tocando la pagina */
    --scrollbar-thumb-hover-color: var(--black, rgba(160, 160, 160, 0.75)); /* Color de la barra cuando el mouse está tocando la pagina */
    --scrollbar-thumb-active-color: var(--black, rgba(180, 180, 180, 0.9)); /* Color cuando se clickea la barra */
    --scrollbar-corner-color: var(--scrollbar-track-color);

    /* Firefox */
    scrollbar-width: thin;
    scrollbar-color: var(--scrollbar-thumb-color) var(--scrollbar-track-color);

    /* Prevent layout shift when scrollbars appear (supported in most modern browsers) */
    scrollbar-gutter: stable;

    /* Let high-contrast/forced-colors modes do the right thing */
    forced-color-adjust: auto;
}

/* Firefox: increase contrast on hover/active */
.custom-scroll:hover {
    scrollbar-color: var(--scrollbar-thumb-hover-color) var(--scrollbar-track-color);
}

.custom-scroll:active {
    scrollbar-color: var(--scrollbar-thumb-active-color) var(--scrollbar-track-color);
}

/* WebKit (Chrome, Safari, Edge) */
.custom-scroll::-webkit-scrollbar {
    width: var(--scrollbar-size);
    height: var(--scrollbar-size);
}

.custom-scroll::-webkit-scrollbar-track {
    border-radius: var(--scrollbar-radius);
    background: var(--scrollbar-track-color);
}

.custom-scroll::-webkit-scrollbar-thumb {
    border-radius: var(--scrollbar-radius);
    background-color: var(--scrollbar-thumb-color);
    border: var(--scrollbar-thumb-border) solid var(--scrollbar-track-color);
    background-clip: padding-box;
}

.custom-scroll::-webkit-scrollbar-thumb:hover {
    background-color: var(--scrollbar-thumb-hover-color);
}

.custom-scroll::-webkit-scrollbar-thumb:active {
    background-color: var(--scrollbar-thumb-active-color);
}

.custom-scroll::-webkit-scrollbar-corner {
    background: var(--scrollbar-corner-color);
}

@media (forced-colors: active) {
    .custom-scroll {
        scrollbar-width: auto;
        scrollbar-color: auto;
    }
}
```

10) Run in terminal:

```bash
mkdir -p frontend/public/assets frontend/public/icons 
```

### Setup backend

1) Run in terminal:

```bash
mkdir -p backend/src/lambdas
```

### Finish

1) Run in terminal:

```bash
pnpm install
```

Do not run ```pnpm approve-builds``` after, the user must do it
