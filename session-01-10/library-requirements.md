# Library Loans — Requirements

You will build this API and its web UI with GitHub Copilot in VS Code, one iteration at a time.
For each iteration, give Copilot the relevant section below and let it **plan** first. Only let it write code once you've read the plan.

> **This sheet says what the software must do, not how to arrange it.** Project names, folder layout, file names and class names are yours to choose. Whatever you decide, write it into `.github/copilot-instructions.md` so Copilot stops re-deciding — and expect your repo to look different from your neighbour's. That's normal.

---

## Tech stack (fixed)

- .NET 10, a minimal API
- EF Core with PostgreSQL, running via `docker compose`
- Integration tests with xUnit and Testcontainers. Tests must not share the `docker compose` database.
- Web UI: React 19 + TypeScript on Vite
- Your own Git repository on GitHub. One branch per iteration. Commit after every green test run.

## Domain

| Entity | Fields |
|---|---|
| **Book** | id, ISBN (13 digits, unique), title, author, published year, total copies (≥ 1) |
| **Member** | id, full name, email (unique), joined-on date |
| **Loan** | id, book, member, borrowed-on date, due-on date, returned-on date (optional) |

A loan is **active** while it has no returned-on date. A book's **available copies** = total copies − its active loans.

## Response conventions (all iterations)

| Situation | Response |
|---|---|
| Invalid input (missing field, bad format) | `400`, listing the errors per field |
| Resource not found | `404` |
| Business rule violated | `409`, with a human-readable `detail` |
| Created | `201` + a `Location` header |

Dates travel over the wire as `yyyy-MM-dd` strings.

---

## Iteration 0 — Design the contract, then build it (Block 2)

**Step 1.** In an empty folder, save this sheet into your repo so you can point Copilot at it:

```sh
curl -O https://raw.githubusercontent.com/AndreRochaDev/workshop-block-coders/main/session-01-10/library-requirements.md
```

**Step 2 — design the HTTP contract.** Nothing below tells you the paths, the verbs or the payloads: that's your design work. With Copilot in **Ask** mode, work out the resources, the request and response bodies, and the status code for every success and every refusal in this sheet. Disagree with at least one of its choices — it will fold immediately, which is exactly why the judgement has to be yours. Save the result as `api-contract.md` and commit it.

We put a few contracts on screen afterwards and compare them. They will differ, and that's the point.

**Step 3 — decide the architecture.** Ask Copilot for **two** ways to structure the project, with trade-offs and a recommendation. Pick one yourself, then write that decision into `.github/copilot-instructions.md`.

**Step 4 — build it, exactly as your contract says:**

- Books: create, list, get one
- Members: create, list, get one
- Validation: ISBN exactly 13 digits · title and author required, max 200 characters · email required and valid · duplicate ISBN or email → `409`
- Create the initial database migration and **read what it generated** before applying it
- At least one passing integration test per endpoint
- **Done when:** the database starts, the tests are green, the app runs, and you have written a first `.github/copilot-instructions.md` based on what Copilot got wrong

## Iteration 1 — Borrow and return (Block 3)

- **Borrow** a book for a member. Borrowed-on is today; due-on is 14 days later.
  - Rejected with `409` when: no copy is available · the member already has **3** active loans · the member already has an active loan of that same book · the member has any overdue loan
- **Return** a loan: sets returned-on to today. Returning an already-returned loan → `409`.
- **List a member's loans**, active first, then most recently borrowed first.
- **Write the test for each rule first.**

## Iteration 2 — The web UI (Block 3)

A React 19 + TypeScript app created with Vite. One page is enough.

- **Books:** every book with its title, author and how many copies are available right now.
- **Borrow:** pick a member, press Borrow on a book, see the list update.
- **Member loans:** the selected member's loans with their due dates. Highlight any loan that is past due.
- **Errors:** when the API answers `409`, show its `detail` to the user. Don't swallow it.

Rules:

- Call the API with bare `/api/...` paths and let the Vite dev server proxy them. Don't hardcode the API origin, and don't add CORS to the API.
- Send and read dates as `yyyy-MM-dd` strings. Don't put `Date` objects on the wire.
- **Done when:** the dev server serves the page, borrowing works end to end against your own API, and a rejected borrow shows a readable message.

## Stretch — if you finish early

**A. Overdue loans endpoint**

- List overdue loans as of a date, defaulting to today. Overdue = active **and** due before that date.
- An unparseable date → `400`. Each item includes the book title, the member name, the due date and how many days overdue. Most overdue first.
- Then move the UI's highlight over to this endpoint instead of working it out in the browser.

**B. Membership tiers**

- Members are `Standard` or `Premium`, defaulting to `Standard`.
- Active loan limit: Standard **3**, Premium **5**. Loan period: Standard **14** days, Premium **28** days.
- Creating a member accepts an optional tier, case-insensitive. An unknown value → `400`.
- Provide a migration that sets existing members to `Standard`.
- Show the tier in the UI, next to the member.

---

## Documentation and review (Block 4)

Once the loans API is green:

- Add doc comments to your loan handlers explaining each `409` rule, then **read them against the code**. A doc comment that lies is worse than none.
- Draft the pull request description from the diff.
- Run a security review in a **fresh chat**, never the one that wrote the code.

---

## Out of scope (do **not** build)

Authentication · fines or payments · reservations and holds · loan renewals · pagination · deleting books or members · email notifications
