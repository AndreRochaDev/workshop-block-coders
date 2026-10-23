# Timesheet Check

`SPEC.md` says what the app does; until it exists, `requirements.md` does. This file says how we build it. When the spec leaves a question open, ask me, then add my answer to `SPEC.md`.

## Stack

Zero dependencies: plain HTML, CSS and JavaScript ES modules, run with Node.js 22 or newer and its built-in modules. There is no build step, so the project runs the moment it's copied to any laptop. Ask me before adding a package.

- `node server.js` serves the app on http://localhost:3000
- `node --test` runs the tests

## Layout

This is the whole project. Ask me before adding a file.

| File | Holds |
|---|---|
| `src/timesheet.js` | Every business rule |
| `src/mock-data.js` | The mock team and their logged hours, generated from a fixed seed so every refresh shows the same data |
| `public/index.html`, `public/app.js`, `public/styles.css` | The screen: one page, plain DOM code |
| `server.js` | A static file server for `public/` and `src/` |
| `test/timesheet.test.js` | The tests |

## `src/timesheet.js` is a deep module

It is the one place that knows the rules: working days, holidays, pay cycles, expected hours, status. It exports a small interface of a few functions and hides everything else behind it. The screen calls that interface and displays what it returns.

- Pure functions: data in, result out. "Today" arrives as a parameter.
- Holiday dates live in the code as a list.
- Dates are `YYYY-MM-DD` strings. Parse them in UTC to get the weekday: `` new Date(`${day}T00:00:00Z`).getUTCDay() ``. Dates built in local time turn into the previous day when converted back to strings while Portugal is on summer time.

## Tests

Test-first, through the exported functions of `src/timesheet.js`: that interface is the agreed seam. Expected values are literal numbers taken from the worked examples in `SPEC.md`. Use `node:test` and `node:assert/strict`. Run `node --test` after every change and show me the result.
