# Excel Workflow Skills
A 4-step system for editing Excel files with Claude — without burning through context or losing track of changes.

---

## What's in the box

| File | What it does |
|------|-------------|
| `xlsx-understand.skill` | Phase 1 — Claude reads and learns your document |
| `xlsx-compress.skill` | Phase 2 — Claude exports that knowledge to a text file |
| `xlsx-plan.skill` | Phase 3 — Claude plans your changes (no document needed) |
| `xlsx-execute.skill` | Phase 4 — Claude makes the changes and updates the record |

---

## Installation

1. Open [claude.ai](https://claude.ai) and go to **Settings → Skills**
2. Click **Install skill**
3. Upload all four `.skill` files, one at a time
4. Done — Claude will now recognize and use these skills automatically

---

## How to use it

You'll run **4 sessions**, each in a fresh chat. Here's exactly what to do in each one.

---

### Session 1 — Understand
**Upload:** your Excel file  
**Say:** `"I need to work on this Excel file"` *(Claude will take it from there)*

Claude will read the document and ask questions until you've both fully understood it. Answer honestly — the more context you give, the better everything else works.

**End of session:** Claude tells you to move on. Do it.

---

### Session 2 — Compress
**Upload:** your Excel file + any notes from Session 1  
**Say:** `"Create the CONTEXT.md file for this document"`

Claude produces a `CONTEXT.md` — a plain-text summary of everything it learned. **Save this file.** It travels with your Excel file from here on.

---

### Session 3 — Plan
**Upload:** `CONTEXT.md` only *(leave the Excel file out)*  
**Say:** `"Here's what I want to change: [describe your changes]"`

Claude builds a `PLAN.md` with numbered steps. **Read it before you approve it.** This is your chance to catch misunderstandings before anything gets touched.

---

### Session 4 — Execute
**Upload:** your Excel file + `CONTEXT.md` + `PLAN.md`  
**Say:** `"Execute the plan"`

Claude works through each step one at a time, confirms as it goes, and updates `CONTEXT.md` with a changelog entry when finished.

---

## Subsequent changes

**Skip Sessions 1 and 2.** Just run Sessions 3 and 4 again with the updated `CONTEXT.md`.

Only redo Sessions 1–2 if the document's structure changes significantly (new sheets, major restructuring, etc.).

---

## Files to keep track of

```
📁 Your project folder
├── your-file.xlsx        ← the document
├── CONTEXT.md            ← always keep this up to date
└── PLAN.md               ← generated fresh each time, safe to overwrite
```

---

## Tips

- **Never skip the plan review.** Session 3 exists so you can catch problems before the document is touched.
- **Keep CONTEXT.md with the Excel file** — treat it as part of the deliverable.
- **If something goes wrong in Session 4**, Claude will stop and ask. Don't ask it to push through errors.
- The whole point of this system is that Claude never has to re-read your document from scratch — `CONTEXT.md` handles that.
