# GRG FRONTEND - DEVELOPMENT GUIDE

> **Project Overview**: See [../README.md](../README.md) for general project structure and backend API references.

## WORKING DIRECTORY

Primary codebase: `traffic-sign-frontend/`
Git repository location: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-frontend`

ALL git operations MUST execute from: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-frontend`
Root folder `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden` is NOT a git repository

ALL code edits MUST be in: `/traffic-sign-frontend/` subdirectory only

## DESIGN SYSTEM

Use GRG design system from `/ndw-design/`
- Most common components available (cards, tabs, buttons, forms, etc.)
- Design system components have associated models
- NEVER hardcode strings that exist in models
- Default to design system components when creating UI elements
- Assume components exist unless told otherwise

## LANGUAGE RULES

Code and comments: English
Commit messages: English
UI text displayed to users: Dutch

Translation system: NO i18n/translation system
UI text: Hardcoded Dutch strings (no translation keys)

