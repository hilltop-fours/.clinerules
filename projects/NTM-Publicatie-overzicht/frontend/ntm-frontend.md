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

---

## PROJECT-SPECIFIC PATTERNS

### Filter Components and Query Params

**Pattern**: Filter components MUST subscribe to `route.queryParams` and reactively update their state when query params change.

**Why**: This allows parent components (like `*-filters-aside`) to clear filters by simply updating the URL params. The filter components will automatically sync their UI state. This avoids tight coupling where parents need to manually call `deselectAll()` on child components.

**CORRECT Pattern** (reactive):
```typescript
@UntilDestroy()
@Component({
  // ...
  providers: [{ provide: BaseFilterDirective, useExisting: MyFilterComponent }],
})
export class MyFilterComponent extends BaseFilterDirective {
  constructor() {
    super();
    this.queryParamName = QUERY_PARAMS.MY_FILTER;

    // Subscribe to query param changes and update options reactively
    this.route.queryParams.pipe(untilDestroyed(this)).subscribe((params) => {
      const activeFilters = params[this.queryParamName]?.split(',') || [];
      this.#updateOptions(activeFilters);
    });
  }

  #updateOptions(activeFilters: string[]): void {
    this.options.set(
      FILTER_VALUES.map((value) => ({
        labelPath: `TRANSLATIONS.${value}`,
        value: value,
        checked: activeFilters.includes(value),
      }))
    );
    this.cdr.detectChanges();
  }
}
```

**INCORRECT Pattern** (one-time read):
```typescript
// ❌ DON'T DO THIS - Only reads params once on init, doesn't react to changes
export class MyFilterComponent extends BaseFilterDirective implements OnInit {
  ngOnInit(): void {
    const activeFilters = this.getActiveFiltersFromQueryParams(); // snapshot only!
    // ... populate options
  }
}
```

**Parent Component** (clearFilters):
```typescript
// ✅ CORRECT - Just update URL, child filters will react automatically
clearFilters() {
  this.#filtersService.updateUrlParams({
    [QUERY_PARAMS.MY_FILTER]: null,
  });
}

// ❌ INCORRECT - Manual coupling to child components
clearFilters() {
  this.#filtersService.updateUrlParams({ [QUERY_PARAMS.MY_FILTER]: null });
  this.filterComponents().forEach((f) => f.deselectAll()); // Don't do this!
}
```

**Reference Examples**:
- Reactive: `data-import-filter-date.component.ts`, `search-input.component.ts`
- Reactive: `publication-review-filter-status.component.ts` (updated)

**Note**: Some older filter components in the codebase still use the incorrect pattern. When modifying these, consider refactoring to the reactive pattern.
