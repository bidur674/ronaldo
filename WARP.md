# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Overview

This repository is a static multi-page HTML site for a Cristiano Ronaldo fanpage. There is no build system, package manager configuration, or automated test suite. The site consists of:

- `Index.html`: main landing page with high-level introduction and navigation links.
- `about.html`: early life and background section with external links (FIFA, UEFA).
- `career.html`: career statistics and achievements section.
- `Contact.html`: simple contact form and alternate contact details.
- Image assets (`img*.jpg`, `*.jpeg`, `*.webp`) used by the above pages.
- `.hintrc`: configuration for the `webhint` HTML linter.

All pages are directly loadable by a browser or any static HTTP server.

## Common Commands

There is no project-local package manager config (no `package.json`, etc.), but there is a `.hintrc` for [webhint](https://webhint.io/). The following commands assume that Node.js and `npx` are available in the environment.

### Lint HTML with webhint

Use the existing `.hintrc` (which extends the `development` preset and customizes some accessibility hints):

```bash path=null start=null
npx hint . --config .hintrc
```

This will lint all HTML files in the repository using the rules defined in `.hintrc`, including:

- Extends the `development` preset.
- Adjusted `axe/structure` and `axe/text-alternatives` behaviour (list and image-alt checks are turned off).
- `no-inline-styles` is disabled.

If `hint` is installed globally instead of via `npx`, you can run:

```bash path=null start=null
hint . --config .hintrc
```

### Run the site locally (static server)

There is no prescribed dev server in this repo. Any static HTTP server that serves the repository root will work. One simple option, if Node.js is available, is:

```bash path=null start=null
npx serve .
```

Then open the reported URL (typically `http://localhost:3000`) and navigate to `Index.html`.

You can also open `Index.html` directly in a browser via the file system if a server is not required.

### Tests

There is no automated test framework configured in this repository and no standard test command. If you introduce tests (e.g., using a browser testing or HTML validation tool), document the commands here to keep future Warp agents aligned.

## High-Level Architecture and Structure

This codebase is intentionally simple and structured as a set of interlinked static HTML documents:

- **Page-to-page navigation**: Each page (`Index.html`, `about.html`, `career.html`, `Contact.html`) links to the others via `<a>` tags and unordered lists, forming a small, manually maintained navigation system. There is no shared layout template, so navigation markup is duplicated across pages.
- **Content responsibilities**:
  - `Index.html` acts as the home page, introducing the site and linking to other sections. It displays a gallery of key images.
  - `about.html` focuses on early life and background, including narrative text, supporting images, and links out to external authoritative sources (FIFA, UEFA).
  - `career.html` summarizes major career milestones, statistics, and trophies, along with supporting images.
  - `Contact.html` contains a basic HTML `<form>` (name, subject, email, phone, message) and additional contact information, plus navigation back to the other pages.
- **Assets**: All images are stored at the repository root and are referenced directly via relative paths from each HTML file.
- **No client-side scripting or stylesheets**: There is no JavaScript and no external CSS files. Styling, where present, is handled via basic HTML attributes, and inline styles are permitted (the `.hintrc` file explicitly disables the `no-inline-styles` rule).

## Notes for Future Changes

- When adding new pages, follow the existing pattern of cross-linking via `<a>` elements so that navigation remains consistent.
- If you introduce JavaScript, CSS, or a build pipeline (e.g., bundlers, preprocessors), add the relevant tooling files (`package.json`, build scripts) and update the **Common Commands** section with concrete build, lint, and test commands.
- If you extend `webhint` usage (for example, to add stricter accessibility rules), update `.hintrc` and keep the lint commands above in sync with any new configuration options.