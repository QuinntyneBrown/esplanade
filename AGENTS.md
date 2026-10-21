# esplanade

## Purpose

This repo is a fullstack web application. It's a headless Cms for a micro content site. The first content site will be used on is for a Co-op for older women. .NET backend, angular (workspace) frontend

## Technology

- Use .NET for the API.
- Use MediatR, pinned to **12.5.0**. Do not upgrade. 12.5.0 is the last release
  under plain Apache-2.0; from 13.0.0 MediatR is commercially licensed, free only
  under a registered Community tier that lapses above $5M USD annual revenue.
- Use Microsoft.Extensions libraries and patterns: dependency injection, Options,
  and Configuration.
- Use Angular for the web client.

## Architecture and Design

- Implement requirements radically simply: the least code that satisfies the
  acceptance criteria, and nothing more. Simple in design, never reduced in scope.
- Apply SOLID principles throughout the codebase.
- Organize features and behaviors into vertical slices.
- Keep back-end code in `backend/`, with source in `src` and tests in `tests`.
- A command-line tool is another project under `backend/src`, not a second root.

## Backend

- Use Clean Architecture. Dependencies point inward. `Domain` references nothing.
- Keep controllers thin: bind, dispatch through MediatR, return. No logic in a
  controller.
- Commands, queries, handlers, and validators live in `Application`.
- One file per type. Every class, interface, record, and enum gets its own file,
  named for the type it holds.
- Folders and namespaces agree. Controllers live in a `Controllers` folder and are
  namespaced `Esplanade.Api.Controllers`.

## Frontend

- `frontend/` is an Angular workspace: the `api`, `components`, and `domain`
  libraries and the application project are siblings under `frontend/projects/`.
- Prefer signals over RxJS, and hold state in signals. Reach for RxJS only for
  genuine streams and events.
- No single-file components. Template, styles, and class each live in their own file.

### Where a component belongs

Placement follows what a component knows, and it is not negotiable.

- `components`: presentational only - buttons, cards, pills. Takes an input, emits
  an output, injects no application service, imports no other project. An Angular
  primitive like `Router` is fine; an `api` contract is not. That leaf position is
  what lets the library publish to npm.
- `domain`: components that inject an `api` contract through its token and render
  what it returns.
- The application project: routed page components, composing the other two and
  owning routing, guards, and dialogs.
- Dependencies run one way, application to `domain` to `api`. A presentational
  component that turns out to need a service moves to `domain`.

## Testing Approach

Use acceptance test-driven development (ATDD): begin with a failing acceptance
test, link it to explicit criteria written using the Given-When-Then format,
implement until it passes, and keep criteria, tests, and implementation aligned.

Back end: integration tests against the API. Front end: Playwright, using the
Page Object Model - one page object per screen, owning the selectors and the
interactions. Tests state intent; page objects know the DOM. Never put a
selector in a test.

### Never write architecture tests

Never add a test that asserts the shape of the codebase rather than its behavior:
no structure, layout, or naming tests; no banned-API scans; no traceability tests
that parse the specifications. Those constraints belong to the compiler, the
formatter, and review. A test suite exists to prove behavior.

## Folder Structure

```text
esplanade/
|-- backend/
|   |-- src/
|   `-- tests/
|-- frontend/
|   `-- projects/
|       |-- esplanade/
|       |-- api/
|       |-- components/
|       `-- domain/
|-- design-system/
|-- e2e/
|   |-- page-objects/
|   `-- specs/
`-- docs/
    `-- specs/
```
