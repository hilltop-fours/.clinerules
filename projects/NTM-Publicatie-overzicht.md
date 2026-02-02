# NTM PUBLICATIE OVERZICHT - PROJECT CONFIG

## WORKING DIRECTORY - CRITICAL

Primary codebase: `ntm-frontend/`
Git repository location: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-frontend`

ALL git operations MUST execute from: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-frontend`
Root folder `/Users/daniel/Developer/NTM-Publicatie-overzicht` is NOT a git repository

ALL code edits MUST be in: `/ntm-frontend/` subdirectory only

## PROJECT STRUCTURE

Monorepo containing:
- Frontend: `/ntm-frontend/` - ONLY directory for edits
- Backend projects:
  - `ntm-backend` - Primary backend service
  - `ntm-tracker-backend` - Tracker service
  - READ ONLY - reference for API contracts, data formats, expected payloads
  - NEVER edit backend code
- Design system: `/ndw-design/`
  - Contains NTM design system in `/ndw-design/libs/ntm`
  - USE: NTM design system for this frontend project
  - READ ONLY - reference for components and styling
  - NEVER edit design system code

Backend and design system usage:
- Use backend projects to understand API behavior and data structures
- Use design system to understand available components and their usage

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
