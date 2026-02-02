# NTM PUBLICATIE OVERZICHT - PROJECT GUIDE

Welcome to the NTM Publicatie Overzicht project. This guide explains the project structure and where to find relevant documentation.

## PROJECT OVERVIEW

This is a monorepo containing:
- **Frontend**: `/ntm-frontend/` - The main web application for publication management
- **Backend Services**: Multiple specialized backend services (ntm-backend, ntm-tracker-backend)
- **Design System**: `/ndw-design/` - Shared NTM design system

## WHICH PART ARE YOU WORKING ON?

### 🎨 FRONTEND DEVELOPMENT

If you're working on the **ntm-frontend** application:

1. **Start here**: [./frontend/NTM-Publicatie-overzicht.md](./frontend/NTM-Publicatie-overzicht.md)
   - Frontend-specific rules and conventions
   - Working directory and git repository configuration
   - Design system usage
   - Language rules and UI text guidelines
   - Translation system setup

### 🔧 BACKEND API REFERENCE

If you need to understand the **backend APIs** used by this frontend:

> Backend documentation coming soon. For now, refer to the backend repositories directly:
> - `ntm-backend` - Primary backend service
> - `ntm-tracker-backend` - Tracker service

### 🎨 DESIGN SYSTEM REFERENCE

The frontend uses the **NTM design system** located in `/ndw-design/libs/ntm`:
- Reference only - understand available components, styles, and patterns
- Never edit design system code
- If you need new components or modifications → Contact the design system team

## ⚠️ CRITICAL RULE: EDIT ONLY THE FRONTEND

**ALL code edits happen in `/ntm-frontend/` ONLY.**

Backend services and the design system are **reference only** - never edit them. If you need changes to:
- Backend APIs → Contact the backend team
- Design system components → Contact the design system team

Use them to understand data structures, API contracts, and available components.

## GENERAL RULES

- **Language**: English for code and commits, Dutch for UI text
- **Translation System**: ngx-translate (`@ngx-translate/core`) - All UI text must use translation keys, not hardcoded strings

## MONOREPO STRUCTURE

```
NTM-Publicatie-overzicht/
├── ntm-frontend/                   ← EDIT ONLY THIS
├── ntm-backend/                    ← Reference only
├── ntm-tracker-backend/            ← Reference only
├── ndw-design/                     ← Reference only
└── .clinerules/
    └── projects/NTM-Publicatie-overzicht/
        ├── README.md               ← You are here
        └── frontend/
            └── NTM-Publicatie-overzicht.md
```
