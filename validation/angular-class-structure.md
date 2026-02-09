# Angular Class Structure Validation Rules

Guidelines for organizing class members in Angular components and services.

## Class member ordering - RECOMMENDED

**Key principle:** Injected dependencies always first (Angular-specific), then public→protected→private for everything else (TypeScript standard).

**Rationale:**
- Dependencies first makes Angular DI immediately visible
- Public→protected→private follows TypeScript-ESLint defaults
- Public members define the class API and should be seen before implementation details

**Recommended order:**

1. **Injected dependencies** (public → protected → private):
   - Public: `readonly router = inject(Router)`
   - Protected: `protected readonly cdr = inject(ChangeDetectorRef)` (rare)
   - Private: `readonly #service = inject(Service)`

2. **Static fields** (public → protected → private)

3. **Public instance fields:**
   - Public readonly constants/enums: `readonly MyEnum = MyEnum`
   - Public ViewChild/ContentChild signals: `element = viewChild<ElementRef>('ref')`
   - Public Input signals: `myInput = input<Type>()`
   - Public Output signals: `myOutput = output<Type>()`
   - Public computed signals: `myComputed = computed(() => ...)`
   - Public variables: `myVar = initialValue`

4. **Protected instance fields:** (same subcategories as public, rare)

5. **Private instance fields:**
   - Private readonly constants: `readonly #constant = value`
   - Private ViewChild/ContentChild signals: `#element = viewChild<ElementRef>('ref')`
   - Private computed signals: `#myComputed = computed(() => ...)`
   - Private variables: `#myVar = value`

6. **Constructor** (`constructor() { super(); }`)

7. **Lifecycle hooks** (`ngOnInit()`, `ngOnDestroy()`, etc.)

8. **Getters/setters** (public → protected → private)

9. **Methods** (public → protected → private):
   - Public methods: `myMethod() { }`
   - Protected methods: `myMethod() { }` (rare)
   - Private methods: `#myMethod() { }`

**Notes:**
- This is a recommended convention for consistency
- Apply when writing new code or refactoring existing code
- Protected members are rare in Angular components
