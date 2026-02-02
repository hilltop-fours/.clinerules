# Clinerules Repository

Shared configuration and guidelines for Claude Code CLI across multiple projects.

## Structure

```
.clinerules/
├── claude.md           # Main entry point (project-specific, rename per project)
├── angular.md          # Angular-specific guidelines
├── git-commits.md      # Git commit message standards
├── projects/           # Project-specific rules
│   ├── GRG-Wegkenmerken-verkeersborden.md
│   └── NTM-Publicatie-overzicht.md
└── README.md          # This file
```

## Usage

1. Clone this repository into your project root as `.clinerules`:
   ```bash
   git clone https://github.com/hilltop-fours/clinerules.git .clinerules
   ```

2. Rename `claude.md` to match your project name (or keep it as `claude.md`)

3. Create a project-specific file in `projects/` directory with:
   - Working directory information
   - Git repository locations
   - Project-specific conventions
   - Technology stack details

4. Reference shared rules from your main `claude.md` file

## Updating Rules

- Update shared rules (angular.md, git-commits.md, etc.) in this repository
- Pull changes into all projects using `git pull` from the `.clinerules` directory
- Project-specific rules stay in each project's `projects/` file

## Benefits

- **Consistency**: Same guidelines across all projects
- **Easy updates**: Change once, pull everywhere
- **Clear context**: Claude knows exactly where to work
- **Reusable patterns**: Standards stay consistent
