# BER BEREIKBAARHEIDSKAART - PROJECT CONFIG

## WORKING DIRECTORY - CRITICAL

Primary codebase: `accessibility-map-frontend/`
Git repository location: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/accessibility-map-frontend`

ALL git operations MUST execute from: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/accessibility-map-frontend`
Root folder `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden` is NOT a git repository

ALL code edits MUST be in: `/accessibility-map-frontend/` subdirectory only

## PROJECT STRUCTURE

Monorepo containing:
- Frontend: `/accessibility-map-frontend/` - ONLY directory for edits
- Backend projects: Multiple services (see BACKEND MAPPING section)
  - READ ONLY - reference for API contracts, data formats, expected payloads
  - NEVER edit backend code
- Design system: `/ndw-design/`
  - Contains GRG design system (shared with GRG Wegkenmerken project)
  - USE: GRG design system for this frontend project
  - READ ONLY - reference for components and styling
  - NEVER edit design system code

Backend and design system usage:
- Use backend projects to understand API behavior and data structures
- Use design system to understand available components and their usage

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

## BACKEND MAPPING

For detailed API documentation for the backends used by this frontend, see:
**[BER-Bereikbaarheidskaart Backend API Reference](../backend/BER-Bereikbaarheidskaart.md)**

This includes information about:
- traffic-sign-backend
- traffic-sign-wkd-backend
- traffic-sign-area-backend
- traffic-sign-feedback-backend
- External API dependencies
- API communication patterns
