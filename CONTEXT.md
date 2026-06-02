# CONTEXT.md — YStack Agent Skills
_Last updated: 2026-06-01_
_Version: 1_

## Project Purpose
YStack is a collection of agent skills for non-technical Claude users. Skills are installed on the local Claude desktop app or used in Claude Cowork. The goal is to improve AI output quality and add QOL for users who don't write code.

## Repository Structure
```
agent-skills/
├── skills/
│   ├── agent-picker/
│   │   └── SKILL.md
│   └── xlsx-cowork/
│       └── SKILL.md
├── CONTEXT.md       ← this file
├── CLAUDE.md        ← instructions for Claude Code
└── README.md        ← user-facing overview
```

## Skills

### agent-picker
| Field | Detail |
|-------|--------|
| Purpose | Help non-technical users pick the right Claude model |
| Entry trigger | User asks which Claude to use, or describes a task and wants guidance |
| Phases | Grilling → (optional) web research → recommendation |
| Output | Plain-language model recommendation with next step |
| Session end | Hard close — no follow-up questions |

### xlsx-cowork
| Field | Detail |
|-------|--------|
| Purpose | Full end-to-end Excel editing workflow in Claude Cowork |
| Entry trigger | User uploads .xlsx/.xls with intent to edit; or uploads Excel + CONTEXT.md for a return session |
| Phases | Understand → Compress → Plan → Sanity check → Subagent execute → Review |
| Persistent artifact | CONTEXT.md — document knowledge base, versioned, updated by subagent each session |
| Ephemeral artifact | PLAN.md — created fresh each session, discarded after |
| Key constraint | Orchestrator never modifies the Excel file; subagent handles all writes |

## Design Conventions

### One question at a time
All YStack skills ask one question at a time during grilling. Never ask multiple questions in a single message.

### Token efficiency
Skills are designed to be lightweight. Subagents receive only the minimum context needed — no conversation history. Documents (CONTEXT.md, PLAN.md) are made self-contained so history doesn't need to be passed.

### Consolidated workflows
Prefer a single skill with multiple phases over splitting into separate skills. The old `mod-xslx` approach (4 separate skills) was replaced by `xlsx-cowork` (one skill, one entry point) for this reason.

### Plain language for non-technical users
Skills avoid jargon. Model names become plain descriptions. Technical terms are translated into outcomes.

## Changelog
| Version | Date | Changes |
|---------|------|---------|
| 1 | 2026-06-01 | Initial — created from collaborative understanding session |
