# GRG WEGKENMERKEN VERKEERSBORDEN - PROJECT CONFIG

## WORKING DIRECTORY - CRITICAL

Primary codebase: `traffic-sign-frontend/`
Git repository location: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-frontend`

ALL git operations MUST execute from: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-frontend`
Root folder `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden` is NOT a git repository

ALL code edits MUST be in: `/traffic-sign-frontend/` subdirectory only

## PROJECT STRUCTURE

Monorepo containing:
- Frontend: `/traffic-sign-frontend/` - ONLY directory for edits
- Backend projects: Multiple services (traffic-sign-backend, traffic-sign-area-backend, traffic-sign-wkd-backend, etc.)
  - READ ONLY - reference for API contracts, data formats, expected payloads
  - NEVER edit backend code
- Design system: `/ndw-design/`
  - Contains GRG and NTM design systems
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
**[GRG-Wegkenmerken-verkeersborden Backend API Reference](../backend/GRG-Wegkenmerken-verkeersborden.md)**

This includes comprehensive information about all backend services, endpoints, DTOs, and authentication/authorization.
