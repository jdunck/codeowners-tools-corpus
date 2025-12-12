# Commit History for Testing

This document describes the commit history in this test corpus and what each commit tests.

## Commit Series Overview

The repository contains a series of commits designed to test how CODEOWNERS tools handle various scenarios over time:

### Commit 1: Initial Corpus Creation
**Commit**: `a35f805 - Add comprehensive CODEOWNERS test corpus with edge cases`

**What it tests**:
- Initial CODEOWNERS file with multiple pattern types
- 27 test files covering various edge cases
- Directory structure with nested paths
- Files with special characters in names

**Files added**:
- `.github/CODEOWNERS` - The main CODEOWNERS file
- Multiple test files across various directories
- Documentation (README.md updated)

### Commit 2: Documentation and Configuration
**Commit**: `7f35e64 - Add test scenarios documentation and config files`

**What it tests**:
- How tools handle documentation changes
- Configuration files (.gitignore, .editorconfig)
- Detailed test scenario documentation

**Files added**:
- `TEST_SCENARIOS.md` - Detailed testing methodology
- `.gitignore` - Tests ignored file handling
- `.editorconfig` - Tests dot-file handling

### Commit 3: Ownership Rule Changes
**Commit**: `a1752a1 - Update ownership rules and add new file to test change tracking`

**What it tests**:
- Modification of existing CODEOWNERS patterns
- Addition of multiple owners to patterns
- New files added in later commits

**Changes**:
- Modified `*.js` pattern to add `@web-team`
- Modified `*.md` pattern to add `@technical-writers`
- Added `new-feature.js` file

**Purpose**: Tests how tools track ownership changes over time.

### Commit 4: File Deletion
**Commit**: `d410aa2 - Remove GUIDE.md to test file deletion tracking`

**What it tests**:
- How tools handle deleted files
- Tracking ownership of files that no longer exist
- Historical ownership queries

**Changes**:
- Deleted `GUIDE.md`

**Purpose**: Tests file deletion tracking and historical analysis.

### Commit 5: File Rename
**Commit**: `6b24f01 - Rename src/index.js to src/main.js to test file rename tracking`

**What it tests**:
- How tools handle file renames/moves
- Tracking ownership across renames
- Git rename detection (100% similarity)

**Changes**:
- Renamed `src/index.js` → `src/main.js`

**Purpose**: Tests rename tracking and ownership continuity.

### Commit 6: Email-Based Ownership
**Commit**: `cce4789 - Add email-based ownership pattern and experimental directory`

**What it tests**:
- Email addresses as owner identifiers (alternative to @username)
- New directory and pattern additions
- Comments and documentation in CODEOWNERS

**Changes**:
- Added email pattern: `experiment-team@example.com`
- Added `/experimental/` directory
- Enhanced CODEOWNERS comments

**Purpose**: Tests alternative owner formats.

### Commit 7: Documentation Fixes
**Commit**: `6ff8579 - Update documentation to reflect actual multiple owners in patterns`

**What it tests**:
- Documentation consistency
- Clarification of precedence rules

**Changes**:
- Updated README.md to match actual CODEOWNERS patterns
- Clarified precedence rule comments

**Purpose**: Ensures documentation accuracy for test validation.

## Testing with this History

Tools can use this commit history to test:

1. **Initial State Analysis**: Parse the complete corpus at HEAD
2. **Historical Queries**: 
   - "Who owned GUIDE.md before it was deleted?"
   - "When did ownership of *.js files change?"
   - "What files were owned by @js-team in commit a35f805?"
3. **Change Tracking**:
   - Detect when ownership patterns change
   - Track files added/removed/renamed
   - Monitor ownership evolution
4. **Diff Analysis**:
   - Compare ownership between any two commits
   - Identify files with ownership changes
   - Track pattern modifications

## Commands for Testing

```bash
# View ownership at different points in time
git checkout a35f805  # Initial corpus
git checkout a1752a1  # After ownership changes
git checkout HEAD     # Current state

# View file history
git log --follow -- src/index.js  # Follows through rename to src/main.js
git log -- GUIDE.md               # Shows deletion

# View CODEOWNERS changes
git log -p -- .github/CODEOWNERS

# View ownership at specific commit
git show a35f805:.github/CODEOWNERS
```

## Expected Tool Behavior

A robust CODEOWNERS tool should be able to:

1. Parse CODEOWNERS at any commit
2. Answer "Who owns file X at commit Y?"
3. Track ownership changes across commits
4. Handle renamed files correctly
5. Support both @username and email formats
6. Apply precedence rules correctly
7. Handle special characters in paths
8. Distinguish root vs non-root patterns
9. Support multiple owners per pattern
10. Ignore comments and blank lines in CODEOWNERS
