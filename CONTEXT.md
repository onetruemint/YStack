# CONTEXT.md — YStack Agent Skills

_Last updated: 2026-06-13_
_Version: 2_

## Project Purpose

YStack is a personal collection of Claude agent skills. Skills cover any purpose or audience — productivity tools for non-technical users, SWE utilities, games, and anything else. There is no common theme requirement. Skills install on the local Claude desktop app or run in Claude Cowork.

## Repository Structure

```markdown
agent-skills/
├── skills/
│   ├── agent-picker/
│   │   └── SKILL.md
│   ├── swe-pattern-rec-game/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── question-bank.md
│   └── xlsx-cowork/
│       └── SKILL.md
├── CONTEXT.md       ← this file
├── CLAUDE.md        ← instructions for Claude Code
└── README.md        ← user-facing overview
```

Skills are stored flat under `skills/`. If skills with a strong common theme accumulate, group them into a category subdirectory (e.g. `skills/swe/`, `skills/productivity/`).

## Skills

### agent-picker

| Field | Detail |
|-------|--------|
| Audience | Non-technical users |
| Purpose | Help users pick the right Claude model |
| Entry trigger | User asks which Claude to use, or describes a task and wants guidance |
| Phases | Grilling → (optional) web research → recommendation |
| Output | Plain-language model recommendation with next step |
| Session end | Hard close — no follow-up questions |

### xlsx-cowork

| Field | Detail |
|-------|--------|
| Audience | Non-technical users |
| Purpose | Full end-to-end Excel editing workflow in Claude Cowork |
| Entry trigger | User uploads .xlsx/.xls with intent to edit; or uploads Excel + CONTEXT.md for a return session |
| Phases | Understand → Compress → Plan → Sanity check → Subagent execute → Review |
| Persistent artifact | CONTEXT.md — document knowledge base, versioned, updated by subagent each session |
| Ephemeral artifact | PLAN.md — created fresh each session, discarded after |
| Key constraint | Orchestrator never modifies the Excel file; subagent handles all writes |

### swe-pattern-rec-game

| Field | Detail |
|-------|--------|
| Audience | Software engineers (interview prep) |
| Purpose | Interactive algorithm pattern recognition game for SWE interview prep |
| Entry trigger | User mentions interview prep, algorithm practice, LeetCode, "quiz me on patterns", etc. |
| Phases | Session setup → 10 problems (one at a time) → end-of-session report |
| Question bank | `references/question-bank.md` — read before selecting problems each session |
| Output | Per-problem grading with one hint allowed; full session report with strengths, revisit topics, habits |

## Design Conventions

### One question at a time

Skills that use a grilling or setup phase ask one question at a time. Never ask multiple questions in a single message during a grilling session.

### Token efficiency

Skills are designed to be lightweight. Subagents receive only the minimum context needed — no conversation history. Documents (CONTEXT.md, PLAN.md) are made self-contained so history doesn't need to be passed.

### Consolidated workflows

Prefer a single skill with multiple phases over splitting into separate skills. The old `mod-xslx` approach (4 separate skills) was replaced by `xlsx-cowork` (one skill, one entry point) for this reason.

### Audience varies by skill

Each skill defines its own audience and tone. Non-technical skills avoid jargon; SWE-facing skills can assume developer context. Check the individual SKILL.md for tone guidance.

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 2 | 2026-06-13 | Broadened repo purpose to all-encompassing personal collection; added swe-pattern-rec-game |
| 1 | 2026-06-01 | Initial — created from collaborative understanding session |
