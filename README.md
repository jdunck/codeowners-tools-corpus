# codeowners-tools-corpus
A repo to facilitate testing codeowner-related tools

## Overview

This repository contains a comprehensive test corpus for validating CODEOWNERS tooling. It includes various files and directory structures that test different edge cases and patterns commonly found in CODEOWNERS files.

## Test Cases Included

### 1. Basic File Extension Patterns
- `*.js` → @js-team @web-team (multiple owners)
- `*.ts` → @typescript-team
- `*.py` → @python-team
- `*.md` → @docs-team @technical-writers (multiple owners)

**Test Files:**
- `app.js` - JavaScript file
- `main.ts` - TypeScript file
- `script.py` - Python file

### 2. Root-Specific vs Non-Root Patterns
- `/README.md` → @project-leads (only matches root)
- `/LICENSE` → @legal-team (only matches root)
- `CHANGELOG` → @release-team (matches anywhere)

**Test Files:**
- `README.md` - In root
- `LICENSE` - In root
- `CHANGELOG` - In root
- `src/CHANGELOG` - In subdirectory

### 3. Directory Ownership
- `/src/` → @src-team
- `/docs/` → @docs-team
- `/tests/` → @qa-team

**Test Files:**
- `src/index.js`
- `docs/README.md`
- `tests/unit/parser.test.js`

### 4. Nested Directory Patterns
- `/src/components/` → @frontend-team
- `/src/components/buttons/` → @ui-team (more specific)

**Test Files:**
- `src/components/Header.js`
- `src/components/buttons/PrimaryButton.js`

### 5. Pattern Precedence (More Specific Wins)
- `*.config.js` → @config-team (general)
- `/src/app.config.js` → @app-team (specific)

**Test Files:**
- `database.config.js` - Matches general pattern
- `src/app.config.js` - Matches specific pattern

### 6. Wildcard Patterns with Paths
- `/src/**/*.test.js` → @qa-team

**Test Files:**
- `src/utils.test.js`

### 7. Multiple Owners
- `/config/` → @devops-team @security-team

**Test Files:**
- `config/database.yml`

### 8. Build and Generated Files
- `/dist/` → @build-team
- `*.min.js` → @build-team
- `*.map` → @build-team

**Test Files:**
- `dist/bundle.min.js`
- `dist/bundle.js.map`

### 9. Special Characters in Paths
- `/files-with-spaces/` → @special-files-team
- `/files.with.dots/` → @special-files-team
- `/files-with-dashes/` → @special-files-team
- `/\[special\]/` → @special-team (escaped brackets)

**Test Files:**
- `files-with-spaces/my file.txt`
- `files.with.dots/config.file.json`
- `files-with-dashes/my-component.js`
- `[special]/file.txt`

### 10. Deeply Nested Paths
- `/src/deep/nested/path/components/` → @nested-team

**Test Files:**
- `src/deep/nested/path/components/Widget.js`

### 11. Documentation Specificity
- `*.md` → @docs-team @technical-writers (general)
- `/docs/**/*.md` → @docs-specialists (more specific)

**Test Files:**
- `docs/README.md` - In docs directory
- `docs/guides/installation.md` - Nested in docs

### 12. Vendor/Dependencies
- `/vendor/` → @dependencies-team

**Test Files:**
- `vendor/library.js`

### 13. Default Owner
- `*` → @global-owner (catches everything not matched by more specific patterns)

## Using This Corpus

Tools can use this repository to test:

1. **Pattern Matching**: Verify correct owner assignment for each file
2. **Precedence Rules**: Ensure more specific patterns override general ones
3. **Special Characters**: Handle spaces, dots, dashes, brackets in paths
4. **Wildcards**: Support `*`, `**`, and other glob patterns
5. **Multiple Owners**: Parse and handle multiple owners per pattern
6. **Root vs Non-Root**: Distinguish between `/pattern` and `pattern`
7. **Edge Cases**: Test deeply nested paths, generated files, etc.

## Expected Ownership Map

See `.github/CODEOWNERS` for the complete pattern definitions and expected ownership for each file.
