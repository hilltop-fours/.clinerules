# NTM FRONTEND - CODING RULES

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

**Location**: `/ndw-design/libs/ntm` (reference only, never edit)

**Usage Rules**:
- Most common components available: cards, tabs, buttons, forms, modals, drawers, inputs, selects, etc.
- Design system components have associated models/interfaces
- NEVER hardcode strings/values that exist in component models
- Default to design system components when creating UI elements
- Assume components exist unless explicitly told otherwise
- If unsure about a component, search design system first before creating custom components

**Component Import Pattern**:
```typescript
import { ComponentName } from '@shared/components/component-name';
```

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
- Use ngx-translate (`@ngx-translate/core`) translation system
- NEVER hardcode Dutch strings in components
- Always use translation keys: `{{ 'TRANSLATION.KEY' | translate }}`

**Translation System**: ngx-translate (`@ngx-translate/core`)

**Translation Files**:
- `src/i18n/nl.json` - Dutch (primary language)
- `src/i18n/en.json` - English (language toggle for non-Dutch users)

**Rules**:
- ALL new translation keys MUST be added to BOTH files
- Dutch values: user-facing text in Dutch
- English values: same text translated to English
- Keep keys synchronized between both files
- Use descriptive key names with proper namespacing

**Example**:
```typescript
// ❌ BAD - Hardcoded Dutch
<h1>Publicaties</h1>
<button>Opslaan</button>

// ✅ GOOD - Translation keys
<h1>{{ 'PUBLICATIONS.TITLE' | translate }}</h1>
<button>{{ 'GENERAL.SAVE' | translate }}</button>
```

**Translation File Example**:
```json
// nl.json
{
  "PUBLICATIONS": {
    "TITLE": "Publicaties",
    "SAVE": "Opslaan"
  }
}

// en.json
{
  "PUBLICATIONS": {
    "TITLE": "Publications",
    "SAVE": "Save"
  }
}
```

---

## WORKING DIRECTORY

**Primary codebase**: `/ntm-frontend/src/`

**Edit locations**:
- ✅ `/ntm-frontend/src/**` - All source code
- ✅ `/.clinerules/**` - Documentation only

**Never edit**:
- ❌ Backend repos
- ❌ Design system
- ❌ `node_modules/`
- ❌ `dist/`, `build/` (build artifacts)

**Configuration files** (read-only unless explicitly asked to edit):
- Can read: `package.json`, `angular.json`, `tsconfig.json`, etc.
- Only edit if user explicitly requests: "update package.json", "add dependency X"
