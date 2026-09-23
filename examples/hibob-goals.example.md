# 📎 Example: Hibob Goals (Local Only)

This is an example of the personal `context/hibob-goals.local.md` file that tracks the goals and key results (KRs) your leader set in Hibob for the current cycle.

> **This file is git-ignored.** Copy it to `context/hibob-goals.local.md` (or create your own from scratch) — it will never be committed, just like `context/additional-context.local.md` and the generated `reports/` folder. See `.gitignore`: `context/*.local.md`.

There's no Hibob API access in this project, so this file is the manual bridge between what's actually in Bob and what the report/brag skills can use as evidence. Update it whenever a goal or KR changes in Bob.

---

## How to add a goal

Copy this template for each goal your leader set:

```markdown
### Goal N — [Goal title, as written in Bob]

- **Status:** [Validated (matches Bob) / Draft — proposed KRs below, not yet entered in Bob]
- **Description:** Goal description
- **Key Results:**
  1. [KR text, as written in Bob or proposed]
  2. [KR text]
```

**Two things worth knowing when writing entries:**

- `Status` **matters.** `Validated (matches Bob)` means you've checked this goal/KRs against Bob's UI and they match exactly — these count as reliable evidence. `Draft` means the KRs were proposed (by you or with AI help) but haven't been entered into Bob yet — treat these as a to-do, not settled evidence, until validated.
- **Keep the goal/KR wording close to Bob's**, even if it's in a different language than the rest of the file (goals are often bilingual) — this file is meant to mirror Bob, not rewrite it.

---



## Sample entry



### Goal 1 — Enhance collaboration with product teams

- **Status:** Draft — proposed KRs below, not yet entered in Bob
- **Key Results:**
  1. Establish a recurring sync with the product team owning your top documentation area to track upcoming feature work before it hits your backlog.
  2. Get an explicit sign-off from the product owner before finalizing at least one large documentation deliverable this cycle.
- **Linked competencies:** `ecosystem_collaboration` (recurring cross-team sync); `expectation_management` (sign-off loop before finalizing)

---



## 🔗 Next Steps

- **Copy this file:** `cp examples/hibob-goals.example.md context/hibob-goals.local.md`
- **Fill in your actual goals/KRs from Bob**, marking `Draft` ones you still need to enter there.
- **See the related evidence file:** [additional-context.example.md](additional-context.example.md)

