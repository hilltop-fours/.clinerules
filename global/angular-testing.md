# Angular Testing Guide

A practical reference for writing meaningful, maintainable tests in Angular projects. Focus on **what to test** (not just how), based on official Angular documentation and community best practices.

---

## Core Testing Philosophy

### Quality Over Quantity

- Write **meaningful tests that verify critical behavior**, not just "should create" tests
- High coverage numbers are meaningless without test quality
- A few well-written tests catching real bugs > many tests just checking if code runs
- **Tests should serve as living documentation** of how your code should behave

### Test Like a User

- Structure tests to resemble how users actually interact with the application
- Test component-template interactions, user actions, and DOM state changes
- Avoid testing internal implementation details
- **Principle**: The more your tests resemble actual usage, the more confidence they provide

### Respect Encapsulation

- Test **public methods primarily**; private methods are tested indirectly
- If private method logic is very complex, consider extracting it into a separate testable unit
- Avoid accessing private members via bracket notation `component['private']()` — this breaks encapsulation

### Mocking External Dependencies

- **Always mock HTTP calls** in unit tests using `HttpTestingController`
- Mock other services using Jasmine `spyOn`
- Mock the Router and ActivatedRoute for navigation testing
- Isolation = faster tests, reliable tests, and proper unit testing

---

## What to Test by Artifact Type

### Pipes (Simplest)

**Testing approach**: Direct unit testing — no DOM required

**What to test**:
- The `transform()` method with various inputs
- Edge cases and special values
- Error handling

**Example**:
```typescript
describe('UppercasePipe', () => {
  let pipe: UppercasePipe;

  beforeEach(() => {
    pipe = new UppercasePipe();
  });

  it('should transform input to uppercase', () => {
    expect(pipe.transform('hello')).toBe('HELLO');
  });

  it('should handle empty string', () => {
    expect(pipe.transform('')).toBe('');
  });
});
```

**Why pipes are easiest**: A pipe is just a class with a pure function — no Angular machinery, no DOM, no change detection.

---

### Directives (Moderate)

**Testing approach**: Apply directive to a test host component, verify DOM changes

**What to test**:
- **Attribute directives**: Verify the directive applies classes, styles, or attributes correctly
- **Structural directives**: Verify elements are added/removed from DOM based on conditions
- Directive input properties
- Directive output events

**Example** (attribute directive):
```typescript
describe('HighlightDirective', () => {
  let component: TestHostComponent;
  let fixture: ComponentFixture<TestHostComponent>;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [HighlightDirective],
      declarations: [TestHostComponent],
    }).compileComponents();

    fixture = TestBed.createComponent(TestHostComponent);
    component = fixture.componentInstance;
  });

  it('should highlight element on hover', () => {
    const element = fixture.debugElement.query(By.directive(HighlightDirective));
    expect(element.nativeElement.style.backgroundColor).toBe('');

    element.triggerEventHandler('mouseenter', null);
    expect(element.nativeElement.style.backgroundColor).toBe('yellow');
  });
});

@Component({
  template: `<div appHighlight></div>`,
  standalone: true,
  imports: [HighlightDirective],
})
class TestHostComponent {}
```

---

### Services (Moderate)

**Testing approach**: Unit test in isolation with mocked dependencies

**What to test**:
- Public methods and their return values
- State management (if applicable)
- Side effects and observable streams
- Error handling and edge cases
- How the service interacts with dependencies (mocked)

**Example**:
```typescript
describe('UserService', () => {
  let service: UserService;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      providers: [UserService, provideHttpClient(), provideHttpClientTesting()],
    });

    service = TestBed.inject(UserService);
    httpMock = TestBed.inject(HttpTestingController);
  });

  afterEach(() => {
    httpMock.verify(); // Verify no outstanding HTTP requests
  });

  it('should fetch users from API', () => {
    const mockUsers = [{ id: 1, name: 'John' }];

    service.getUsers().subscribe((users) => {
      expect(users).toEqual(mockUsers);
    });

    const req = httpMock.expectOne('/api/users');
    expect(req.request.method).toBe('GET');
    req.flush(mockUsers);
  });

  it('should handle API errors gracefully', () => {
    service.getUsers().subscribe(
      () => fail('should have failed with 404 error'),
      (error) => {
        expect(error.status).toBe(404);
      }
    );

    const req = httpMock.expectOne('/api/users');
    req.flush('Not Found', { status: 404, statusText: 'Not Found' });
  });
});
```

**Key patterns**:
- Use `HttpTestingController` to intercept and mock HTTP requests
- Call `httpMock.verify()` in `afterEach` to ensure no unexpected requests
- Test error scenarios explicitly

---

### Components (Most Complex)

**Testing approach**: Full TestBed setup with component fixture, template interaction

**What to test**:
- **Initialization**: Form creation, initial state, pre-filled values
- **User interactions**: Click handlers, form input, keyboard events
- **Template bindings**: Verify component state displays in DOM correctly
- **Conditional rendering**: Elements appear/disappear based on component state
- **Component communication**: @Input properties, @Output events
- **Error handling**: Form validation, error messages, error states
- **HTTP/async operations**: Mocking API calls, loading states, error states

**What NOT to test**:
- Generic "should create" tests (they add no value)
- Internal implementation details like private methods
- Framework behavior (Angular's change detection, etc.)
- Third-party library internals

**Example** (form component):
```typescript
describe('ContactFormComponent', () => {
  let component: ContactFormComponent;
  let fixture: ComponentFixture<ContactFormComponent>;
  let contactService: jasmine.SpyObj<ContactService>;

  beforeEach(async () => {
    const contactServiceSpy = jasmine.createSpyObj('ContactService', ['submitForm']);

    await TestBed.configureTestingModule({
      imports: [ContactFormComponent, ReactiveFormsModule],
      providers: [{ provide: ContactService, useValue: contactServiceSpy }],
    }).compileComponents();

    contactService = TestBed.inject(ContactService) as jasmine.SpyObj<ContactService>;
    fixture = TestBed.createComponent(ContactFormComponent);
    component = fixture.componentInstance;
    fixture.detectChanges();
  });

  it('should initialize form with empty fields', () => {
    expect(component.form.get('email')?.value).toBe('');
    expect(component.form.get('message')?.value).toBe('');
  });

  it('should mark form as invalid when required fields are empty', () => {
    expect(component.form.valid).toBeFalse();
  });

  it('should display validation error when email is invalid', () => {
    const emailControl = component.form.get('email');
    emailControl?.setValue('invalid-email');
    emailControl?.markAsTouched();
    fixture.detectChanges();

    const errorElement = fixture.debugElement.query(By.css('[data-testid="email-error"]'));
    expect(errorElement.nativeElement.textContent).toContain('Invalid email');
  });

  it('should submit form when valid and button is clicked', () => {
    component.form.patchValue({
      email: 'test@example.com',
      message: 'Hello',
    });
    contactService.submitForm.and.returnValue(of({ success: true }));

    const submitButton = fixture.debugElement.query(By.css('button[type="submit"]'));
    submitButton.nativeElement.click();

    expect(contactService.submitForm).toHaveBeenCalledWith({
      email: 'test@example.com',
      message: 'Hello',
    });
  });

  it('should show success message after form submission', fakeAsync(() => {
    component.form.patchValue({
      email: 'test@example.com',
      message: 'Hello',
    });
    contactService.submitForm.and.returnValue(of({ success: true }));

    const submitButton = fixture.debugElement.query(By.css('button[type="submit"]'));
    submitButton.nativeElement.click();

    tick();
    fixture.detectChanges();

    const successMessage = fixture.debugElement.query(By.css('[data-testid="success-message"]'));
    expect(successMessage.nativeElement.textContent).toContain('Message sent');
  }));

  it('should display error message when submission fails', fakeAsync(() => {
    component.form.patchValue({
      email: 'test@example.com',
      message: 'Hello',
    });
    contactService.submitForm.and.returnValue(
      throwError(() => new Error('Server error'))
    );

    const submitButton = fixture.debugElement.query(By.css('button[type="submit"]'));
    submitButton.nativeElement.click();

    tick();
    fixture.detectChanges();

    const errorMessage = fixture.debugElement.query(By.css('[data-testid="error-message"]'));
    expect(errorMessage.nativeElement.textContent).toContain('Failed to send');
  }));
});
```

**Key patterns**:
- Create `TestBed` with component and mocked dependencies
- Use `fixture.detectChanges()` to trigger Angular change detection
- Use `By.css()` to query template elements
- Use `fakeAsync` and `tick()` for async operations
- Use `jasmine.createSpyObj()` to create mock services
- Test user interactions like clicking buttons and filling forms

---

## Testing Reactive Forms (Recommended)

Reactive forms are much easier to test because you can manipulate the form model directly without DOM interaction.

### Form Initialization and Validation

```typescript
describe('LoginFormComponent', () => {
  let component: LoginFormComponent;
  let fixture: ComponentFixture<LoginFormComponent>;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [LoginFormComponent, ReactiveFormsModule],
    }).compileComponents();

    fixture = TestBed.createComponent(LoginFormComponent);
    component = fixture.componentInstance;
    fixture.detectChanges();
  });

  it('should initialize form with required validators', () => {
    const emailControl = component.form.get('email');
    expect(emailControl?.hasError('required')).toBeTrue();
  });

  it('should mark control as invalid when validation fails', () => {
    const emailControl = component.form.get('email');
    emailControl?.setValue('invalid-email');
    expect(emailControl?.hasError('email')).toBeTrue();
  });

  it('should mark control as valid when input is correct', () => {
    const emailControl = component.form.get('email');
    emailControl?.setValue('test@example.com');
    expect(emailControl?.valid).toBeTrue();
  });
});
```

### Custom Form Validators

```typescript
describe('PasswordValidator', () => {
  it('should validate password strength', () => {
    const control = new FormControl('weak');
    expect(PasswordValidator.strong(control)).toEqual({ weakPassword: true });

    const strongControl = new FormControl('StrongPass123!');
    expect(PasswordValidator.strong(strongControl)).toBeNull();
  });

  it('should validate password match', () => {
    const group = new FormGroup({
      password: new FormControl('Password123!'),
      confirmPassword: new FormControl('Password123!'),
    });

    expect(PasswordValidator.match(group)).toBeNull();

    group.get('confirmPassword')?.setValue('DifferentPass123!');
    expect(PasswordValidator.match(group)).toEqual({ passwordMismatch: true });
  });
});
```

**Key advantage**: No DOM interaction needed — you can set values and check validity programmatically.

---

## Testing HTTP Integration

Always mock HTTP calls using `HttpTestingController`.

### Basic HTTP Mocking

```typescript
describe('UserRepository', () => {
  let repository: UserRepository;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      providers: [UserRepository, provideHttpClient(), provideHttpClientTesting()],
    });

    repository = TestBed.inject(UserRepository);
    httpMock = TestBed.inject(HttpTestingController);
  });

  afterEach(() => {
    httpMock.verify(); // Ensure no outstanding requests
  });

  it('should send POST request with correct data', () => {
    const userData = { name: 'John', email: 'john@example.com' };

    repository.create(userData).subscribe();

    const req = httpMock.expectOne('/api/users');
    expect(req.request.method).toBe('POST');
    expect(req.request.body).toEqual(userData);

    req.flush({ id: 1, ...userData });
  });

  it('should handle server errors', () => {
    repository.create({}).subscribe(
      () => fail('should have failed'),
      (error) => {
        expect(error.status).toBe(500);
      }
    );

    const req = httpMock.expectOne('/api/users');
    req.flush('Server error', { status: 500, statusText: 'Internal Server Error' });
  });

  it('should not make request when service is destroyed', () => {
    // This assumes your service cleans up subscriptions on destroy
    const request$ = repository.create({});
    const subscription = request$.subscribe();
    subscription.unsubscribe();

    httpMock.expectNone('/api/users');
  });
});
```

### HTTP Error Handling

```typescript
it('should retry on network failure', fakeAsync(() => {
  let attempts = 0;
  const errorHandler = jasmine.createSpy('errorHandler');

  repository
    .create({})
    .pipe(retry({ count: 3, delay: 100 }))
    .subscribe(() => {}, errorHandler);

  // First attempt fails
  let req = httpMock.expectOne('/api/users');
  req.error(new ErrorEvent('Network error'));

  tick(100);

  // Second attempt fails
  req = httpMock.expectOne('/api/users');
  req.error(new ErrorEvent('Network error'));

  tick(100);

  // Third attempt succeeds
  req = httpMock.expectOne('/api/users');
  req.flush({ id: 1 });

  tick();

  expect(errorHandler).not.toHaveBeenCalled();
}));
```

---

## Testing Accessibility (WCAG)

Test that your components meet WCAG standards.

```typescript
describe('FormComponent - Accessibility', () => {
  let component: FormComponent;
  let fixture: ComponentFixture<FormComponent>;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [FormComponent, ReactiveFormsModule],
    }).compileComponents();

    fixture = TestBed.createComponent(FormComponent);
    component = fixture.componentInstance;
    fixture.detectChanges();
  });

  it('should have aria-required on required fields', () => {
    const requiredInputs = fixture.debugElement.queryAll(
      By.css('input[aria-required="true"]')
    );
    expect(requiredInputs.length).toBeGreaterThan(0);
  });

  it('should display aria-invalid when field is invalid', () => {
    const emailControl = component.form.get('email');
    emailControl?.setValue('invalid');
    emailControl?.markAsTouched();
    fixture.detectChanges();

    const emailInput = fixture.debugElement.query(By.css('input[formControlName="email"]'));
    expect(emailInput.nativeElement.getAttribute('aria-invalid')).toBe('true');
  });

  it('should associate error messages with aria-describedby', () => {
    const emailInput = fixture.debugElement.query(By.css('input[formControlName="email"]'));
    const describedById = emailInput.nativeElement.getAttribute('aria-describedby');

    const errorElement = fixture.debugElement.query(By.css(`#${describedById}`));
    expect(errorElement.nativeElement.textContent).toContain('Invalid email');
  });

  it('should announce validation errors to screen readers', () => {
    const liveRegion = fixture.debugElement.query(By.css('[aria-live="polite"]'));
    expect(liveRegion).toBeTruthy();
  });

  it('should have proper label associations', () => {
    const labels = fixture.debugElement.queryAll(By.css('label'));
    labels.forEach((label) => {
      const forAttr = label.nativeElement.getAttribute('for');
      const associatedInput = fixture.debugElement.query(By.css(`#${forAttr}`));
      expect(associatedInput).toBeTruthy();
    });
  });
});
```

---

## Testing Code Coverage

### Coverage Targets

- **Baseline**: 75-80% code coverage (statements, branches, functions, lines)
- **Strategy**: Maintain or improve coverage with each new feature
- **Philosophy**: Coverage is a metric, not a goal — focus on test quality

### Measuring Coverage

```bash
# Run tests with coverage report
ng test --code-coverage

# Coverage thresholds in karma.conf.js
coverageReporter: {
  dir: 'coverage/',
  subdir: '.',
  reporters: [{ type: 'html' }, { type: 'text-summary' }],
  check: {
    global: {
      statements: 75,
      branches: 75,
      functions: 75,
      lines: 75,
    },
  },
}
```

### Interpreting Coverage Reports

- **Statements**: How many individual statements are executed
- **Branches**: How many conditional branches (`if`, `else`, ternary) are taken
- **Functions**: How many functions are called
- **Lines**: How many lines of code are executed

**Important**: 100% coverage doesn't mean all bugs are caught. It just means all code paths are executed. Bad tests can achieve high coverage.

---

## Testing Anti-Patterns (What NOT to Do)

### ❌ Don't: Test Internal Implementation

```typescript
// BAD: Testing private method implementation
it('should initialize validator correctly', () => {
  component['createValidator'](); // ❌ Breaks encapsulation
  expect(component['validator']).toBeDefined(); // ❌ Testing private state
});

// GOOD: Test public behavior
it('should have validation enabled when form is displayed', () => {
  fixture.detectChanges();
  expect(component.form.valid).toBeFalse(); // ✅ Test public API
});
```

### ❌ Don't: Test Framework Behavior

```typescript
// BAD: Testing Angular's change detection
it('should run change detection', () => {
  fixture.detectChanges();
  expect(fixture.componentInstance).toBeTruthy(); // ❌ Pointless
});

// GOOD: Test component behavior
it('should display updated value after input', () => {
  component.name = 'John';
  fixture.detectChanges();
  expect(fixture.debugElement.query(By.css('h1')).nativeElement.textContent)
    .toContain('John'); // ✅ Test real behavior
});
```

### ❌ Don't: Write Tests That Are Too Specific

```typescript
// BAD: Over-specific test that breaks on refactoring
it('should call updateFormControl with email value', () => {
  spyOn(component, 'updateFormControl'); // ❌ Testing implementation
  component.form.get('email')?.setValue('test@example.com');
  expect(component.updateFormControl).toHaveBeenCalledWith('email', 'test@example.com');
});

// GOOD: Test the user-visible behavior
it('should display entered email in form', () => {
  const input = fixture.debugElement.query(By.css('input[formControlName="email"]'));
  input.nativeElement.value = 'test@example.com';
  input.nativeElement.dispatchEvent(new Event('input'));
  fixture.detectChanges();
  expect(component.form.get('email')?.value).toBe('test@example.com'); // ✅ Test behavior
});
```

### ❌ Don't: Test Third-Party Libraries

```typescript
// BAD: Testing Angular/RxJS internals
it('should unsubscribe from observable on destroy', () => {
  const spy = spyOn(component['subscription'], 'unsubscribe');
  component.ngOnDestroy();
  expect(spy).toHaveBeenCalled(); // ❌ Testing library behavior
});

// GOOD: Trust the library and test your integration
it('should load data when component initializes', () => {
  // Your code calls the observable, library handles subscription
  expect(component.data).toBeDefined();
});
```

---

## Best Practices Checklist

### Before Writing a Test

- [ ] Is this testing user-visible behavior or internal implementation?
- [ ] Would this test fail if the code had a real bug?
- [ ] Can I simplify this test to make it more readable?
- [ ] Am I testing one thing per test, or multiple things?

### After Writing a Test

- [ ] Does the test name clearly describe what it tests?
- [ ] Can someone unfamiliar with the code understand what this test does?
- [ ] Is the test isolated (no dependencies on other tests)?
- [ ] Does it mock external dependencies (HTTP, services, etc.)?
- [ ] Does it clean up after itself (e.g., `httpMock.verify()`)?

### Test Organization

```typescript
describe('ComponentName', () => {
  let component: ComponentName;
  let fixture: ComponentFixture<ComponentName>;

  beforeEach(async () => {
    // Setup common to all tests
  });

  describe('initialization', () => {
    it('should create component', () => {});
    it('should load initial data', () => {});
  });

  describe('user interaction', () => {
    it('should update form when user types', () => {});
    it('should submit form when button clicked', () => {});
  });

  describe('error handling', () => {
    it('should display error when API fails', () => {});
  });

  describe('accessibility', () => {
    it('should have aria-required on required fields', () => {});
  });
});
```

---

## Official Angular Testing Documentation

For more detailed information, refer to the official Angular guides:

- [Testing Overview](https://angular.dev/guide/testing)
- [Component Testing Basics](https://angular.dev/guide/testing/components-basics)
- [Component Testing Scenarios](https://angular.dev/guide/testing/components-scenarios)
- [Testing Services](https://angular.dev/guide/testing/services)
- [Testing HTTP](https://angular.dev/guide/http/testing)
- [Testing Pipes](https://angular.dev/guide/testing/pipes)
- [Testing Code Coverage](https://angular.dev/guide/testing/code-coverage)

---

## Quick Reference: Testing by Artifact Type

| Artifact | Complexity | Setup Required | Key Tool | Example |
|---|---|---|---|---|
| Pipe | ⭐ Lowest | Minimal | Jasmine | `expect(pipe.transform(input)).toBe(expected)` |
| Service | ⭐⭐ Low | TestBed + mocks | HttpTestingController | Mock HTTP, test methods |
| Directive | ⭐⭐ Low | TestBed + host component | By.directive() | Apply to test host, verify DOM |
| Component | ⭐⭐⭐ High | Full TestBed setup | fixture.debugElement | Test template interaction, user input |

---

## Summary

Write tests that:
- **Verify real user behavior**, not implementation details
- **Fail when bugs are introduced**, not when code is refactored
- **Serve as documentation** of how your code should work
- **Are isolated and fast**, with mocked external dependencies
- **Are readable and maintainable**, with clear test names and structure

Focus on **quality over quantity**. A few well-written tests that catch real bugs are worth more than many tests that just check if code compiles.
