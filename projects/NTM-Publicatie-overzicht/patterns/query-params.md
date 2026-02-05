# NTM Query Parameters Pattern

## Overview

This document defines how query parameters are used and handled in the NTM project for routing, filtering, pagination, and feature flags.

---

## Quick Reference

| Use Case | Example | Notes |
|----------|---------|-------|
| Feature-specific data | `?publicationId=123` | Used to pre-fill forms or deep link to specific items |
| Navigation state | `?tab=details` | Preserve user's UI state across navigation |
| Filtering & search | `?status=approved&type=dataset` | Apply filters to listings |
| Pagination | `?page=2&pageSize=20` | Handle large result sets |
| Features & A/B testing | `?useNewUI=true` | Enable/disable features |

---

## Common Patterns

### Feature-Specific Deep Linking
Load specific data based on query parameter:

```typescript
// Example: Load a specific publication in the contact form
publicationId$ = this.route.queryParams.pipe(
  map((params) => params['publicationId'] as string | undefined),
  filter((id): id is string => Boolean(id))
);

// Usage in component
combineLatest({ publicationId: publicationId$, user: user$ })
  .subscribe(({ publicationId, user }) => {
    // Use publicationId to pre-fill form fields
  });
```

### Navigation State Preservation
Maintain state across page navigations:

```typescript
// Save current tab in URL
navigateTo(tabName: string) {
  this.router.navigate(['/details'], {
    queryParams: { tab: tabName },
    queryParamsHandling: 'merge'  // Preserve other params
  });
}

// Restore tab from URL
activeTab$ = this.route.queryParams.pipe(
  map((params) => params['tab'] || 'overview')
);
```

### Pagination
Handle large result sets with page and page size:

```typescript
// Route params: ?page=2&pageSize=50
queryParams$ = this.route.queryParams.pipe(
  map((params) => ({
    page: parseInt(params['page'] || '1'),
    pageSize: parseInt(params['pageSize'] || '20')
  }))
);

// Trigger API call with pagination
data$ = queryParams$.pipe(
  switchMap(({ page, pageSize }) =>
    this.api.getItems({ page, pageSize })
  )
);
```

### Filtering & Search
Apply multiple filters to listings:

```typescript
// Route: ?status=approved&type=dataset&organization=123
filters$ = this.route.queryParams.pipe(
  map((params) => ({
    status: params['status'],
    type: params['type'],
    organization: params['organization']
  }))
);

// API call with filters
results$ = filters$.pipe(
  switchMap((filters) => this.api.search(filters))
);
```

---

## Best Practices

### 1. Always Type Query Parameters
```typescript
// ✅ GOOD: Type-safe
const publicationId$ = this.route.queryParams.pipe(
  map((params) => params['publicationId'] as string | undefined),
  filter((id): id is string => Boolean(id))
);

// ❌ AVOID: Untyped
const publicationId$ = this.route.queryParams.pipe(
  map((params) => params['publicationId'])
);
```

### 2. Validate & Filter
```typescript
// ✅ GOOD: Filter out invalid values
filter((id): id is string => Boolean(id))

// ❌ AVOID: Pass undefined through
map((params) => params['publicationId'])  // May be undefined
```

### 3. Provide Defaults
```typescript
// ✅ GOOD: Explicit default
const pageSize = parseInt(params['pageSize'] || '20')

// ❌ AVOID: No fallback
const pageSize = parseInt(params['pageSize'])  // NaN if missing
```

### 4. Preserve Params on Navigation
```typescript
// ✅ GOOD: Keep existing params
this.router.navigate(['/new-page'], {
  queryParams: { newParam: value },
  queryParamsHandling: 'merge'
});

// ❌ AVOID: Lose existing params
this.router.navigate(['/new-page'], {
  queryParams: { newParam: value }
});
```

### 5. Use Snake Case for Multi-Word Params
```typescript
// ✅ GOOD: snake_case
?publication_id=123
?page_size=50
?is_draft=true

// ❌ AVOID: camelCase
?publicationId=123
?pageSize=50
?isDraft=true
```

---

## Security Considerations

### 1. Validate Input Types
```typescript
// ✅ GOOD: Parse and validate
const page = parseInt(params['page'] || '1');
if (isNaN(page) || page < 1) {
  page = 1;  // Use safe default
}

// ❌ AVOID: Trust user input
const page = params['page'];  // Could be "abc" or malicious
```

### 2. Sanitize Before Using in Display
```typescript
// ✅ GOOD: Sanitize before HTML interpolation
const searchTerm = this.sanitizer.sanitize(SecurityContext.HTML, params['search']);

// ❌ AVOID: Use directly in templates
{{ searchTerm }}  <!-- Could execute XSS -->
```

### 3. Validate Against Expected Values
```typescript
// ✅ GOOD: Only allow expected values
const allowedStatuses = ['draft', 'published', 'rejected'];
const status = allowedStatuses.includes(params['status'])
  ? params['status']
  : 'published';

// ❌ AVOID: Accept any value
const status = params['status'];  // Could be anything
```

---

## Testing Query Parameters

### Unit Testing
```typescript
// Mock ActivatedRoute with queryParams
const mockRoute = {
  queryParams: of({ publicationId: '123', tab: 'details' })
};

// Provide in test
TestBed.configureTestingModule({
  providers: [
    { provide: ActivatedRoute, useValue: mockRoute }
  ]
});
```

### Integration Testing
```typescript
// Test navigation with params
it('should load publication data from query param', () => {
  router.navigate(['/contact'], {
    queryParams: { publicationId: '123' }
  });

  expect(component.publicationId).toBe('123');
  expect(component.form.get('body')?.value).toContain('publication-url');
});
```

---

## Related Documentation
- [Angular ActivatedRoute](https://angular.io/guide/router#router-api)
- [Query Parameters in Angular](https://angular.io/guide/router#query-parameters)
