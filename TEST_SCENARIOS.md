# CODEOWNERS Test Scenarios

This document describes specific test scenarios that tools should validate.

## Scenario 1: Basic Pattern Matching

**Files to test:**
- `app.js` should match `*.js` → @js-team
- `main.ts` should match `*.ts` → @typescript-team
- `script.py` should match `*.py` → @python-team

**Expected behavior:** Each file should be owned by the corresponding team based on extension.

## Scenario 2: Root vs Non-Root Pattern Distinction

**Files to test:**
- `/README.md` (root) should match `/README.md` → @project-leads
- `GUIDE.md` (root) should match `*.md` → @docs-team
- `CHANGELOG` (root) should match `CHANGELOG` → @release-team
- `src/CHANGELOG` should also match `CHANGELOG` → @release-team
- `/LICENSE` (root only) should match `/LICENSE` → @legal-team

**Expected behavior:** 
- Root-specific patterns (with leading `/`) only match in repository root
- Non-root patterns match at any depth

## Scenario 3: Pattern Precedence (Most Specific Wins)

**Files to test:**
- `database.config.js` matches `*.config.js` → @config-team
- `src/app.config.js` matches both `*.config.js` AND `/src/app.config.js`
  - Should use the more specific pattern: `/src/app.config.js` → @app-team

**Expected behavior:** When multiple patterns match, the last (most specific) pattern takes precedence.

## Scenario 4: Nested Directory Ownership

**Files to test:**
- `src/index.js` matches `/src/` → @src-team
- `src/components/Header.js` matches both `/src/` AND `/src/components/`
  - Should use: `/src/components/` → @frontend-team
- `src/components/buttons/PrimaryButton.js` matches:
  - `/src/` → @src-team
  - `/src/components/` → @frontend-team  
  - `/src/components/buttons/` → @ui-team
  - Should use the most specific: @ui-team

**Expected behavior:** More deeply nested directory patterns override parent patterns.

## Scenario 5: Wildcard Patterns

**Files to test:**
- `src/utils.test.js` matches:
  - `*.js` → @js-team
  - `/src/` → @src-team
  - `/src/**/*.test.js` → @qa-team
  - Should use: @qa-team (most specific)

**Expected behavior:** The `**` wildcard matches any number of subdirectories.

## Scenario 6: Multiple Owners

**Files to test:**
- `config/database.yml` matches `/config/` → @devops-team @security-team

**Expected behavior:** File should show both teams as owners.

## Scenario 7: Special Characters in Paths

**Files to test:**
- `files-with-spaces/my file.txt` → @special-files-team
- `files.with.dots/config.file.json` → @special-files-team
- `files-with-dashes/my-component.js` → @special-files-team
- `[special]/file.txt` → @special-team (requires escaped pattern `/\[special\]/`)

**Expected behavior:** Tools should handle spaces, dots, dashes, and special characters correctly.

## Scenario 8: Documentation Pattern Hierarchy

**Files to test:**
- `GUIDE.md` (root) matches `*.md` → @docs-team
- `docs/README.md` matches both:
  - `*.md` → @docs-team
  - `/docs/**/*.md` → @docs-specialists
  - Should use: @docs-specialists
- `docs/guides/installation.md` matches same patterns, should also be @docs-specialists

**Expected behavior:** Documentation in the `/docs/` directory has specialized owners.

## Scenario 9: Build Artifacts

**Files to test:**
- `dist/bundle.min.js` matches:
  - `*.js` → @js-team
  - `*.min.js` → @build-team
  - `/dist/` → @build-team
  - Should use: @build-team
- `dist/bundle.js.map` matches:
  - `*.map` → @build-team
  - `/dist/` → @build-team
  - Should use: @build-team

**Expected behavior:** Build outputs are owned by build team regardless of file type.

## Scenario 10: Default Catch-All

**Files to test:**
- Any file not matching a more specific pattern should match `*` → @global-owner

**Expected behavior:** The wildcard `*` pattern provides a default owner for all files.

## Testing Methodology

Tools should:

1. Parse the `.github/CODEOWNERS` file
2. For each file in the repository, determine the matching pattern(s)
3. Apply precedence rules (last match wins, more specific beats general)
4. Verify the correct owner(s) are assigned
5. Handle edge cases like special characters, spaces, and deeply nested paths
6. Support multiple owners per file
7. Distinguish between root and non-root patterns

## Common Pitfalls to Test

- **Pattern Order**: CODEOWNERS rules are evaluated bottom-up (last match wins)
- **Trailing Slashes**: `/dir/` matches all files in directory; `/dir` matches the directory itself
- **Glob Patterns**: `*` matches within a directory; `**` matches across directories
- **Escaped Characters**: Special characters like `[`, `]`, `*`, `?` may need escaping
- **Case Sensitivity**: Patterns are typically case-sensitive
- **Whitespace**: Owners are whitespace-separated; patterns end at first whitespace
