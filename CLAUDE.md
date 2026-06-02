# YStack — Agent Skills Repository

## Project Overview
This repo contains Claude agent skills for non-technical users. Skills install on the local Claude desktop app or run in Claude Cowork. See `CONTEXT.md` for full project context.

## Skill File Structure
Each skill lives in its own subdirectory under `skills/` and contains a single `SKILL.md`:

```
skills/
└── skill-name/
    └── SKILL.md    ← frontmatter (name, description) + skill content
```

The frontmatter `description` field is what the AI reads to decide when to trigger the skill — write it carefully.

## YStack Conventions

### One question at a time
All grilling sessions ask one question at a time. Never produce a list of questions. This is a hard rule across all YStack skills.

### Token efficiency
- Skills should be small and lightweight
- Subagents receive only the files they need — no conversation history
- Make plan/context documents self-contained so nothing extra needs to be passed

### Consolidated workflows
Prefer one skill with phases over multiple skills the user has to invoke separately.

### Non-technical audience
- No jargon in user-facing content
- Translate technical terms into outcomes
- Assume the user is not a developer

## When Adding a New Skill
1. Create `skills/<skill-name>/SKILL.md`
2. Write a precise `description:` in the frontmatter — this controls when the skill triggers
3. Update `README.md` with a one-line entry under the appropriate category
4. Update `CONTEXT.md`: add a row to the Skills table and increment the version + changelog
