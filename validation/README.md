# Validation Rules

This folder contains code validation and style rules that can be applied systematically during code generation and review.

## Files in this folder

### `angular-style.md`
Code style preferences and conventions for Angular development:
- Ternary operators for simple conditionals
- Nullish coalescing for default values
- Readonly for immutable values
- Class and style bindings over ngClass/ngStyle directives
- Event handler naming conventions
- Private field syntax (#)

### `angular-class-structure.md`
Class organization and member ordering guidelines:
- Class member ordering (dependencies → static → public → protected → private)
- Organization of properties and methods
- Constructor and lifecycle hook placement

### `sonarqube-rules.md`
All SonarQube rules actively enforced in the project:
- typescript:S4202 — No `any` type
- typescript:S109 — No magic numbers
- typescript:S134 — Control flow nesting depth
- typescript:S1192 — Duplicated string literals
- css:S4666 — Duplicate CSS selectors
- typescript:S106 — Console statements
- typescript:S1128 — Unused imports
- typescript:S1066 — Merge nested if statements
- typescript:S121 — Control structures with braces
- typescript:S1117 — Variable declaration duplicates
- Web:S6811 — aria-required placement
- Web:S6853 — Form label associations

## Usage

These rules are meant to be:
1. **Referenced during code generation** - Include relevant rules in AI prompts
2. **Applied in validation passes** - Check generated code against these rules
3. **Used in code reviews** - Reference specific rules when discussing patterns

## Relationship to `angular-instructions.md`

The main `angular-instructions.md` file contains core architecture and patterns. This `validation/` folder contains rules that are more "fixable" or stylistic in nature - rules that can be systematically validated or corrected during code generation and review.
