# YStack

## Overview

YStack is a collection of agent skills for Claude users who aren't software
engineers. Each skill is designed to be small and lightweight — saving your
tokens rather than burning them, while getting you better results.

If you're hitting usage limits every session, start with `agent-picker` to
make sure you aren't running expensive flagship models for simple tasks.

## Installation

Run this command in your terminal to install all YStack skills:

```sh
npx skills add onetruemint/agent-skills
```

Once installed, your Claude agent will automatically trigger the right skill
based on what you're working on — no setup required beyond installation.

## Skills

### Excel

- **[xlsx-cowork](./skills/xlsx-cowork/)** &mdash; A full collaborative workflow for editing Excel files in Claude Cowork. Guides you through understanding your document, planning changes together, and executing them via a focused subagent — so you stay in control without burning through context.

### Quality of Life

- **[agent-picker](./skills/agent-picker/)** &mdash; Not sure which Claude model to use? This skill asks you a few questions about your task and recommends the best fit in plain language — no jargon, no guessing.
