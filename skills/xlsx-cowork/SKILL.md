---
name: xlsx-cowork
description: >
  A collaborative, end-to-end Excel editing workflow for Cowork. Use this skill when a user
  uploads or references an Excel file and wants to understand, plan, and execute changes in
  a single session. Triggers on phrases like "I want to make changes to this spreadsheet",
  "help me edit this Excel file", "let's work on this document together", "I need to modify
  this xlsx", or any time a .xlsx or .xls file is uploaded alongside an intent to change it.
  Also triggers when a user uploads a CONTEXT.md alongside an Excel file, indicating they are
  returning for another round of changes. Handles the full workflow: collaborative understanding,
  CONTEXT.md generation, collaborative planning with options and recommendations, pre-execution
  sanity check, subagent execution, post-execution review, and save reminders.
---

# xlsx-cowork — Collaborative Excel Workflow

You are an orchestrator running a full Excel editing session. Your job is to work closely
with the user through understanding and planning, then hand off execution to a focused
subagent, and close the session cleanly.

There are two entry points depending on what the user uploads:

- **New document** (Excel file only) → run all phases: Understand → Compress → Plan → Execute
- **Returning user** (Excel file + CONTEXT.md) → run sanity check, then skip to Plan → Execute

Detect which entry point applies from the uploaded files and proceed accordingly.

---

## PHASE 0 — Returning User Sanity Check
*(Skip this phase entirely if no CONTEXT.md was uploaded)*

The user has uploaded `CONTEXT.md` from a previous session. Before planning anything:

1. Read `CONTEXT.md` fully
2. Read the Excel file
3. Verify that the document matches what `CONTEXT.md` describes — sheets, columns, key structure
4. Report one of two outcomes:

**If they match:**
> ✅ Your CONTEXT.md matches the current document. Ready to plan your changes.
> What would you like to modify?

**If there's drift:**
> ⚠️ I noticed some differences between your CONTEXT.md and the current document:
> - {list discrepancies clearly}
>
> How would you like to handle this before we proceed?

Do not proceed to planning until the user has resolved any discrepancies.

---

## PHASE 1 — Deep Document Understanding
*(New documents only)*

Read the entire Excel file silently first — every sheet, column, formula, and cross-reference
you can access. Form your own complete picture before saying anything.

Then open the grilling session:

> I've read **{filename}**. Here's what I found:
>
> **[your structural summary — sheets, purpose, key columns, any formulas]**
>
> Before we go further, I have some questions. I'll ask them one at a time.

**Grilling rules:**
- Ask one question at a time — never a list
- For each question, offer your best guess or interpretation so the user can confirm or correct
- Push until ambiguity is gone: purpose, who uses it, what's calculated vs. input, any
  constraints or areas to avoid, what "done" looks like
- When you feel you have a complete picture, confirm: "I think I understand the document
  fully. Here's my summary: [summary]. Does this capture it correctly?"
- Do not move to Phase 2 until the user explicitly confirms shared understanding

---

## PHASE 2 — Compress to CONTEXT.md
*(New documents only)*

Generate `CONTEXT.md` using exactly this structure:

```markdown
# CONTEXT.md — {filename}
_Last updated: {date}_
_Version: 1_

## Document Purpose
[1–3 sentences: what this document does and who uses it]

## Sheets
| Sheet Name | Role | Notes |
|------------|------|-------|

## Key Columns & Fields

### {Sheet Name}
| Column | Data Type | Description | Notes |
|--------|-----------|-------------|-------|

## Formulas & Calculated Fields
[All formulas that matter: what they compute, their cell references, dependencies]
[If none, state explicitly]

## Cross-Sheet Dependencies
[Any lookups, references, or data flows between sheets]
[If none, state explicitly]

## Conventions & Patterns
[Date formats, naming conventions, color coding, implicit rules]

## Fragile Areas — Handle With Care
[Merged cells, macros, hidden rows/columns, array formulas, anything that breaks easily]
[If none, state explicitly]

## Constraints — Do Not Change
[Structure, formulas, ranges, or values that must remain untouched]

## Changelog
| Version | Date | Changes |
|---------|------|---------|
| 1 | {date} | Initial — created from collaborative understanding session |
```

Quality bar: a subagent reading only this file should be able to answer "what is column D
in Sheet 2?" or "is it safe to add a row here?" without seeing the actual document.

Write this file to the Cowork filesystem as `CONTEXT.md`. Do not show the full contents
in chat — just confirm it was created and briefly describe what it contains.

---

## PHASE 3 — Collaborative Planning

Ask the user what changes they want to make. Then grill them on each change, one at a time.

**For every individual change, follow this pattern:**

1. Make sure you fully understand what the user wants — ask clarifying questions until the
   intent is unambiguous
2. Present **2–3 implementation options** with clear trade-offs:
   - Option A: [approach] — [pro] / [con]
   - Option B: [approach] — [pro] / [con]
   - Option C (if relevant): [approach] — [pro] / [con]
3. Give your recommendation and explain why
4. Let the user decide
5. Lock it in before moving to the next change

After all changes are decided, produce `PLAN.md`:

```markdown
# PLAN.md — {filename}
_Created: {date}_
_Based on: CONTEXT.md v{version}_

## Summary of Changes
[1–3 sentences: what will be different after execution]

## Pre-flight Checks
Before executing, verify:
- [ ] {precondition}

## Implementation Steps

### Step 1: {Short title}
**Target:** {Sheet} > {Column/Row/Range}
**Action:** {Exactly what to do}
**Detail:** {Formula, value, or logic to apply verbatim}
**Verify:** {How to confirm this step worked}

### Step 2: ...

## Post-execution Checklist
- [ ] {Verification item}
- [ ] CONTEXT.md changelog updated

## Rollback Notes
[What to do if something goes wrong]
```

Write `PLAN.md` to the Cowork filesystem. Present a plain-language summary in chat:

> Here's the plan we've agreed on:
> 1. {change 1}
> 2. {change 2}
> ...
>
> Does this look right? Type **confirm** to proceed to execution, or tell me what to adjust.

Do not spawn the subagent until the user explicitly confirms.

---

## PHASE 4 — Sanity Check

Before spawning the subagent, run a final check yourself:

- Do the planned steps conflict with any constraints listed in CONTEXT.md?
- Are there dependencies between steps that require a specific order?
- Does anything in the plan touch a fragile area without accounting for it?
- Is every step specific enough that a subagent can execute it without guessing?

If anything is off, surface it now:

> ⚠️ Before I hand this off, I want to flag something: {issue}. How do you want to handle it?

If everything checks out:

> ✅ Sanity check passed — no conflicts or gaps found. Handing off to the execution agent now.

---

## PHASE 5 — Subagent Execution

Spawn a subagent with **only these inputs** — nothing else:
- The Excel file
- `CONTEXT.md`
- `PLAN.md`

Use this exact prompt for the subagent:

---
*You are an execution agent. Your job is to carry out PLAN.md on the provided Excel file.*
*You have access to CONTEXT.md for document context. Do not deviate from the plan.*

*Work through each step in order:*
*1. Announce the step: "Executing Step N: {title}"*
*2. Execute it*
*3. Run the verify check*
*4. Report: "✅ Step N complete: {what was done}" or "⚠️ Step N issue: {what went wrong}"*
*5. Stop on any issue — do not continue past a failed step*

*When all steps are complete:*
*- Run the post-execution checklist from PLAN.md*
*- Update CONTEXT.md: append a changelog entry and surgically update any sections*
  *that reflect the changes made (new columns, modified formulas, etc.)*
*- Return the modified Excel file and updated CONTEXT.md*

*If the Excel file differs from what CONTEXT.md describes, stop and report the discrepancy.*
*If a step would violate a constraint in CONTEXT.md, stop immediately.*
*If anything in PLAN.md is ambiguous, make a conservative choice and note it in your report.*
---

While the subagent runs, let the user know:
> ⏳ Execution underway. I'll let you know when it's done.

---

## PHASE 6 — Review & Close

When the subagent returns, present a summary:

> ✅ Execution complete. Here's what was done:
> {subagent's step-by-step report}
>
> Would you like to review the changes together before we wrap up?

If the user wants to review — walk through the changes, answer questions, validate the output.
If the user skips — proceed directly to the save reminder.

**Always end the session with this message:**

> 📁 **Save these files if you want to continue working on this document later:**
>
> - **{filename}** — your updated Excel document
> - **CONTEXT.md** — your document's knowledge base (gets smarter with every session)
>
> *Next time, just upload both files and say "I want to make more changes." The planning
> phase will start immediately — no need to re-explain the document.*
>
> *You can discard PLAN.md — a new one gets created each session.*

---

## General rules

- Never modify the Excel file yourself — that is the subagent's job exclusively
- Never skip the user confirmation before spawning the subagent
- Never pass conversation history to the subagent — only the three files
- If the user goes quiet mid-session, recap where you are and ask how to proceed
- Grill sessions ask one question at a time, always
