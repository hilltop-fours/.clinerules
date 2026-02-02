# INSTRUCTIONS FOR CLAUDE CODE

## META: How to Write These Files
These configuration files are written FOR CLAUDE CODE TO READ, not for humans.
- Use explicit, unambiguous language that leaves no room for interpretation
- State requirements as direct commands/rules, not suggestions
- Be specific about file paths, working directories, and command execution locations
- Write assuming the reader (Claude) might be Haiku 4.5 which needs maximum clarity
- Avoid human-friendly explanations - focus on machine-parseable rules

## PROJECT IDENTIFICATION - EXECUTE FIRST

Read your current working directory path. Match it against these patterns:

IF path contains `/GRG-Wegkenmerken-verkeersborden`:
  - Project: GRG Wegkenmerken Verkeersborden
  - READ FILE: projects/GRG-Wegkenmerken-verkeersborden.md
  - GIT COMMAND EXECUTION DIRECTORY: /Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-frontend
  - ALL git commands (checkout, add, commit, push, status, branch, etc.) MUST execute from the directory above
  - NEVER run git commands from /Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden (this is NOT a git repository)

IF path contains `/NTM-Publicatie-overzicht`:
  - Project: NTM Publicatie Overzicht
  - READ FILE: projects/NTM-Publicatie-overzicht.md
  - GIT COMMAND EXECUTION DIRECTORY: /Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-frontend
  - ALL git commands (checkout, add, commit, push, status, branch, etc.) MUST execute from the directory above
  - NEVER run git commands from /Users/daniel/Developer/NTM-Publicatie-overzicht (this is NOT a git repository)

## EXECUTION ORDER

1. Identify project using rules above
2. Read the project-specific config file specified above (MANDATORY - contains all project rules)
3. Read shared rule files:
   - angular.md (Angular coding standards)
   - git-commits.md (commit message format)
4. Apply all rules from steps 2-3 to every action taken in this session

## CRITICAL RULES

- The project-specific file contains ALL context needed for the project
- Git operations MUST be executed from the subdirectory specified above, NOT from project root
- Project root directories are NOT git repositories and git commands will fail there
- When in doubt about any rule, refer back to the project-specific config file
