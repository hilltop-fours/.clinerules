# NTM FRONTEND - DEVELOPMENT GUIDE

> **Project Overview**: See [../README.md](../README.md) for general project structure and backend API references.

## WORKING DIRECTORY

Primary codebase: `ntm-frontend/`
Git repository location: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-frontend`

ALL git operations MUST execute from: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-frontend`
Root folder `/Users/daniel/Developer/NTM-Publicatie-overzicht` is NOT a git repository

ALL code edits MUST be in: `/ntm-frontend/` subdirectory only

## DESIGN SYSTEM

Use NTM design system from `/ndw-design/libs/ntm`
- Most common components available (cards, tabs, buttons, forms, etc.)
- Design system components have associated models
- NEVER hardcode strings that exist in models
- Default to design system components when creating UI elements
- Assume components exist unless told otherwise

## LANGUAGE RULES

Code and comments: English
Commit messages: English
UI text displayed to users: Dutch

Translation system: ngx-translate (`@ngx-translate/core`)
UI text: Place in translation files, NOT hardcoded strings
Use translation keys for all user-facing text
