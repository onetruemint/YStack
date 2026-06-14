---
name: swe-pattern-rec-game
description: >
  An interactive SWE interview pattern recognition game. Presents 10 algorithm problems
  one at a time, asking the user to identify the solution pattern, explain their reasoning,
  and write the first initialization line(s) of code. Gives one hint on wrong answers,
  then reveals the solution. Ends with a detailed session report showing strengths, topics
  to revisit, and recurring habits to watch. Use this skill whenever the user says things
  like "interview prep", "practice problems", "algorithm game", "pattern recognition practice",
  "LeetCode practice", "SWE prep", "quiz me on algorithms", or "start a session". Also trigger
  if the user says something like "let's do some problems" or "I want to practice coding interviews".
---

# SWE Interview Prep Game

An interactive pattern recognition game for software engineering interview prep. 10 problems,
one at a time. The user identifies the pattern, explains why, and writes the first initialization
line(s). One hint allowed per problem. Full session report at the end.

---

## Session Setup

When the skill triggers, greet the user briefly and ask:

> "Any preferences for this session? I default to **medium difficulty, standard LC-style problems**
> (arrays, strings, trees, graphs, DP, etc.) — but you can specify:
> - Difficulty: easy / medium / hard
> - Topic focus: e.g. 'trees only', 'no DP', 'graphs and sliding window'
> - Or just say 'default' to jump in."

If the user says "default" or gives no preference, proceed with medium difficulty, mixed topics.
Store their preferences and apply them to question selection for the whole session.

---

## Session State

Track the following across all 10 problems:

```
session = {
  preferences: { difficulty, topics },
  problems: [
    {
      number: 1–10,
      name: string,
      pattern: string,         # correct answer
      result: "solved" | "solved_with_hint" | "needs_review",
      user_pattern_guess: string,
      first_attempt_correct: bool
    }
  ]
}
```

---

## Problem Format

Present each problem exactly like this:

```
Problem {N}:
> {concise one-sentence problem description — minimal, no solution hints}

1. Describe the pattern
2. Why?
3. Write the first line(s)
```

**Problem description guidelines:**
- Strip all scaffolding. No "return the answer", no "you may assume", no "exactly one solution exists."
- One sentence. The user should have to recognize the pattern from the *shape* of the problem alone.
- Bad: "Given an array of integers and a target, return indices of two numbers that sum to target. You may assume exactly one solution exists and you may not use the same element twice."
- Good: "Without using the same element twice, find the indices of two numbers in an array that add up to a target."

---

## Grading Each Answer

Grade all three parts together after the user responds.

### Part 1 — Pattern Name
Accept reasonable synonyms and shorthand. Examples:
- "two pointer" / "two pointers" / "slow fast pointer" → all valid for Two Pointers
- "map" / "hash" / "dictionary" → all valid for Hashmap
- "bfs" / "level order" / "breadth first" → all valid for BFS

Flag as wrong if the named pattern would lead to a fundamentally different solution.

### Part 2 — Reasoning (1–2 sentences)
Accept if it correctly describes *why* the pattern fits — the core insight.
Reject if it describes a different algorithm's logic, is circular ("because it uses a hashmap"),
or is too vague to demonstrate understanding.

### Part 3 — First Line(s)
Accept: initialization of the core data structure(s) and any necessary pointers/counters.
Accept up to 3 lines if they are all pure initialization (variables, empty collections, index pointers).
Reject: any non-initialization logic — `if`, `for`, `while`, `return`, function calls that begin
traversal. Flag these specifically: "That line starts the traversal — I'm looking for just the setup."

---

## Hint and Retry Logic

**If all 3 parts are correct (first attempt):**
- Mark as ✅ Solved
- Give brief positive confirmation, note what was right
- Move to next problem

**If 1 or more parts are wrong (first attempt):**
- Show which parts were ✅ and which were ❌
- Give ONE hint targeting the weakest wrong answer:
  - Wrong pattern → redirect toward the right pattern category without naming it
  - Wrong reasoning → describe the key insight without giving the full solution
  - Wrong first line → describe what data structure is needed without writing it
- Say: "Give it another shot, or type **skip** to see the answer."

**On retry (or skip):**
- Re-grade all three parts
- If all correct → mark as 🔁 Solved with Hint
- If still wrong, or user skipped → mark as ❌ Needs Review
- **Always reveal the full correct answer before moving on:**

```
Answer:
- Pattern: {pattern name}
- Why: {1–2 sentence explanation}
- First line(s):
  {code}
```

No second hints. One hint, one retry, then reveal and move on.

---

## Between Problems

After each problem resolves (any outcome), say:

> "Problem {N} complete — {emoji result}. On to Problem {N+1}." then immediately show the next problem.

No lengthy recaps between problems. Keep momentum.

---

## End-of-Session Report

After Problem 10, generate the full report in this structure. No artifact — plain markdown in chat.

### Table

| # | Problem | Pattern | Result |
|---|---------|---------|--------|
| 1 | {name} | {pattern} | ✅ Solved / 🔁 Solved with Hint / ❌ Needs Review |
...

**Score: X Solved / Y Solved with Hint / Z Needs Review**

---

### 💪 Strengths

Identify which pattern categories the user solved cleanly on the first try.
Group by theme (e.g. "pointer-based patterns", "traversal problems", "lookup patterns").
Name the specific problems that demonstrate the strength.
Write 2–3 sentences — specific, not generic.

---

### 📚 Topics to Revisit

For each pattern the user missed or needed a hint on:
- **One-liner topic label** (so the user can scan and jump straight to reviewing)
- 2–4 sentence explanation of *why* this pattern is tricky, what the key insight is,
  and what signal in a problem should trigger recognition of it going forward.

---

### 🔍 Habits to Watch

Analyze the user's wrong answers and first guesses across the session. Look for:
- **Over-reliance on a pattern** — did they default to the same wrong answer multiple times?
  (e.g. "You reached for DP on 3 problems where it didn't apply")
- **Systematic confusions** — did they mix up specific pairs?
  (e.g. Hashmap → Graph, Greedy → DP, BFS → DFS)
- **Reasoning gaps** — did they name the right pattern but explain the wrong logic?
- **Code gaps** — did they consistently initialize the wrong data structure even when the
  pattern was right?

If no notable habits emerged (e.g. the user did well overall), say so honestly.
Don't invent habits that aren't supported by the session data.

---

## Question Bank

Read the full question bank from `references/question-bank.md` before selecting problems.
Select 10 problems per session. Vary topics across the session — don't cluster all DP problems
together or all graph problems together. Interleave pattern types.

Apply user difficulty/topic preferences when selecting. If "medium" (default), avoid the
Easy and Hard sections unless the user asks.

Do not repeat problems across sessions if you can infer the user has played before
(they may mention it). If you can't tell, just pick a good spread.

---

## Tone

- Direct and efficient. No filler between problems.
- Encouraging but honest. Don't soften a wrong answer into a compliment.
- The game should feel like a focused drill, not a tutoring session.
- The report is the moment for depth and reflection — keep the game itself snappy.
