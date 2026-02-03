# GRG TRAFFIC SIGN FRONTEND - CODING RULES

> **Project Context**: Read `../project-instructions.md` for repository locations and backend API mapping

## FILE TYPE RESTRICTIONS

This project uses ONLY:
- TypeScript: `.ts` files
- HTML templates: `.html` files
- SCSS styles: `.scss` files

When using Glob tool, use pattern: `**/*.{ts,html,scss}`

NEVER search for or work with: `.js`, `.jsx`, `.tsx`, `.css`, `.less`, `.vue` files

---

## DESIGN SYSTEM

**Location**: `/ndw-design/` (reference only, never edit)

**Usage Rules**:
- Most common components available: cards, tabs, buttons, forms, modals, drawers, inputs, selects, etc.
- Design system components have associated models/interfaces
- NEVER hardcode strings/values that exist in component models
- Default to design system components when creating UI elements
- Assume components exist unless explicitly told otherwise
- If unsure about a component, search design system first before creating custom components

---

## LANGUAGE RULES

**Code & Comments**: English only
- Variable names: English
- Function names: English
- Class names: English
- Comments: English

**Commit Messages**: English only
- Follow rules in `.clinerules/global/git-instructions.md`

**UI Text**: Dutch only
- All user-facing text must be in Dutch

**CRITICAL: NO TRANSLATION SYSTEM**
- GRG does NOT use ngx-translate or any i18n framework
- There are NO translation files (no `nl.json`, no `en.json`)
- UI text is hardcoded as Dutch strings directly in templates

**Example**:
```typescript
// ✅ CORRECT for GRG - hardcoded Dutch
<h1>Verkeersborden</h1>
<button>Opslaan</button>

// ❌ WRONG for GRG - do NOT use translation keys
<h1>{{ 'TRAFFIC_SIGNS.TITLE' | translate }}</h1>
```

---

## WORKING DIRECTORY

**Primary codebase**: `/traffic-sign-frontend/src/`

**Edit locations**:
- ✅ `/traffic-sign-frontend/src/**` - All source code
- ✅ `/.clinerules/**` - Documentation only

**Never edit**:
- ❌ Backend repos
- ❌ Design system (`/ndw-design/`)
- ❌ `node_modules/`
- ❌ `dist/`, `build/` (build artifacts)

**Configuration files** (read-only unless explicitly asked to edit):
- Can read: `package.json`, `angular.json`, `tsconfig.json`, etc.
- Only edit if user explicitly requests: "update package.json", "add dependency X"
