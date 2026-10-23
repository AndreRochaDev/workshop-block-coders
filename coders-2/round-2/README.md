# Round 2: same app, with context

This time Copilot gets three things it didn't have in Round 1:

- an **instructions file** that says *how* we build: `.github/copilot-instructions.md`
- **skills** that say *how* we work: Matt Pocock's `grill-me`, `grilling` and `tdd`
- a **spec** you write together, which says *what* we build: `SPEC.md`

## 1. Set up (10 min)

1. Create an empty folder called `timesheet-2` and open it in VS Code.
2. Open a **new chat**, pick **Agent**, and pick the same model as on the presenter's screen.
3. Send this:

   ```text
   Download these two files into this folder:
   - https://raw.githubusercontent.com/AndreRochaDev/workshop-block-coders/main/coders-2/requirements.md as requirements.md
   - https://raw.githubusercontent.com/AndreRochaDev/workshop-block-coders/main/coders-2/round-2/.github/copilot-instructions.md as .github/copilot-instructions.md

   Then install the grill-me, grilling and tdd skills from https://github.com/mattpocock/skills
   into .github/skills/. Copy each skill's whole folder. Install nothing else, and don't
   leave a clone of the repo behind. List the files you created.
   ```

4. Open `.github/skills/grilling/SKILL.md` and read it. A skill is a text file of instructions that Copilot follows with your permissions, so always read the skills you install.
5. Type `/` in the chat box. You should see `grill-me` and `tdd` in the list.

**If the skills didn't install,** send this instead:

```text
Copy the grill-me, grilling and tdd folders from
https://github.com/AndreRochaDev/workshop-block-coders/tree/main/coders-2/round-2/skills-fallback
into .github/skills/, with every file in each folder.
```

Or run Matt's own installer in the terminal:

```sh
npx skills@latest add mattpocock/skills --skill grill-me --skill grilling --skill tdd -a github-copilot --copy -y
```

## 2. Grill (15 min)

Open a **new chat**, pick **Agent**, and send:

```text
/grill-me #file:requirements.md Ask me at most 10 questions in total. Start with the ones that change how many hours someone owes.
```

If Copilot just answers instead of asking you questions, send the same message again with `/grilling` in place of `/grill-me`.

Answer every question. Reply "yes" to accept Copilot's recommendation, or give your own answer. These are your decisions: there is no right answer, only a written one.

When it's done, send:

```text
Write what we agreed into SPEC.md: each rule with one worked example, the pay cycle dates, and what's out of scope. No code.
```

Read `SPEC.md` and fix anything that isn't what you meant.

## 3. Plan and build (35 min)

1. Open a **new chat**, pick **Plan**, and send:

   ```text
   #file:SPEC.md Plan the implementation, following .github/copilot-instructions.md.
   ```

2. Read the plan and answer its questions. Spend a minute comparing it with your neighbour's.
3. In the same chat, switch the dropdown to **Agent** and send:

   ```text
   /tdd Implement this plan, one rule at a time.
   ```

   If Copilot doesn't seem to know about the plan, go back to **Plan**, press **Start implementation**, and then send `/tdd Continue, one rule at a time.`

4. When the tests pass, run `node server.js` in the terminal and open http://localhost:3000.

## When time is up

The same two questions as Round 1:

- How many hours did the pay cycle that just closed require?
- How does your app work that out? Point to the line in `SPEC.md` and the test that checks it.

---

The skills in `skills-fallback/` are copied unchanged from [mattpocock/skills](https://github.com/mattpocock/skills) at commit `5beb35b`, under his MIT licence (`skills-fallback/LICENSE`).
