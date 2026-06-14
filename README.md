# YStack

## Overview

YStack is a personal collection of Claude agent skills. Skills cover any purpose or
audience — productivity tools, SWE utilities, games, and more. Each skill is small and
lightweight, designed to improve AI output quality without burning unnecessary tokens.

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

### SWE

- **[swe-pattern-rec-game](./skills/swe-pattern-rec-game/)** &mdash; An interactive algorithm pattern recognition game for SWE interview prep. 10 problems per session — identify the pattern, explain why, write the first line(s). One hint per problem, full session report at the end.
