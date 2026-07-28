```markdown
# Claude-test Development Patterns

> Auto-generated skill from repository analysis

## Overview
This skill teaches you the core development conventions and patterns used in the `Claude-test` TypeScript repository. You'll learn how to structure files, write imports and exports, and follow the project's testing approach. While no specific frameworks or automated workflows are detected, this skill ensures consistency and clarity in your contributions.

## Coding Conventions

### File Naming
- Use **camelCase** for file names.
  - Example: `userProfile.ts`, `dataFetcher.test.ts`

### Import Style
- Use **relative imports** for modules within the project.
  - Example:
    ```typescript
    import { fetchData } from './dataFetcher';
    ```

### Export Style
- Use **named exports** for all modules.
  - Example:
    ```typescript
    // In userProfile.ts
    export function getUserProfile(id: string) { ... }
    ```

### Commit Messages
- Freeform style, no enforced prefixes.
- Average message length: ~63 characters.
  - Example:  
    ```
    Fix bug in user profile loading logic
    ```

## Workflows

_No automated workflows detected in this repository._

## Testing Patterns

- **Test files** use the pattern: `*.test.*` (e.g., `userProfile.test.ts`).
- **Testing framework** is unknown; follow existing patterns or consult the team.
- Example test file structure:
  ```typescript
  import { getUserProfile } from './userProfile';

  describe('getUserProfile', () => {
    it('returns correct user data', () => {
      // test implementation
    });
  });
  ```

## Commands

| Command      | Purpose                                      |
|--------------|----------------------------------------------|
| /add-test    | Create a new test file for a module          |
| /list-tests  | List all test files in the project           |
| /lint        | Run linting on the codebase (if configured)  |
| /format      | Format code according to project conventions |

```