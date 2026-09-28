# Strict English LiveCode Project

Bidirectional transpiler between LiveCode and Strict English with VSCode integration.

## Project Locations

This project exists in two locations that should be kept in sync:

1. **PRIMARY (MBP):** `/Users/thawsoe/Documents/LiveCode/strict-english-livecode/`
2. **BACKUP (Big Mac):** `/Volumes/Pi/LiveCode/strict-english-livecode/`

**⚠️ IMPORTANT:** The MBP folder is the PRIMARY working directory. New work should be done here unless Big Mac is specifically needed. Use `hermes-sync push` to keep both in sync.

## Project Structure

```
strict-english-livecode/
├── logs/                           # Self-contained project chat logs (se-YYYY-MM-DD.md)
├── docs/                            # Project documentation
│   ├── architecture.md             # System architecture
│   ├── REFERENCES.md                 # Project references with search anchors
│   └── KEYWORD_INDEX.md              # Concept linking keywords
├── src/                             # Source code
│   ├── extension.ts                  # VS Code extension (transpiler engine)
│   │   └── RULES: Rules 1-5 transpile SE → LC
│   ├── livecode/                   # LiveCode source files
│   ├── strict-english/             # Transpiler source
│   └── server/                     # Transpiler microservice
├── scripts/                         # Helper scripts
│   ├── simple-import.js            # Populate Supabase dictionary (defines `answer` command)
│   └── update-livecode-syntax.js   # Fix livecode_syntax field in database
├── test/                            # Test files
│   ├── test-answer-grammar.js      # Tests SE definitions and LC outputs
│   └── test-answer-template.js     # Template validation tests
├── configs/                         # Configuration files
└── data/                            # Data files (grammar rules, etc.)
```

## Key Files for Answer Command Definition

**Multiple definitions exist - change requires updates in multiple places:**

| File | Purpose | Definition Location |
|------|---------|---------------------|
| `scripts/simple-import.js` | Database source | Lines 36-45: `answerDef` object with SE/LC syntax |
| `src/extension.ts` | Transpiler rules | Rules 1-5 for SE→LC conversion |
| `test-answer-grammar.js` | Test cases | Lines 11-29: 4 example pairs |
| `scripts/update-livecode-syntax.js` | Database fix | Updates livecode_syntax column |
| Supabase `dictionary` table | Runtime data | Single source of truth for website |

**⚠️ CRITICAL:** To change the `answer` command definition, update ALL the above locations!

## ANSWER Command: All Definitions

### Quick Reference
- **SE Syntax:** `answer <prompt> [using icon <iconType>] [with <buttons>] [titled <windowTitle>] [as sheet]`
- **LC Syntax:** `answer [<iconType>] <prompt> [with <button1> [or <buttons>]] [titled <windowTitle>] [as sheet]`

### Definition Sources
1. **Database** (Supabase) - Lines shown on https://thaw-soe.github.io/strict-english-dictionary/
2. **Import Script** `scripts/simple-import.js` - Lines 36-45
3. **Transpiler** `src/extension.ts` - Rules 1-5
4. **Tests** `test-answer-grammar.js` - Lines 9-29

### 4 Examples (from LiveCode Dictionary)
| # | Strict English | LiveCode |
|---|---------------|----------|
| 1 | `answer "Go ahead?" with buttons "Yes", "No", and "Maybe"` | `answer "Go ahead?" with "Yes" or "No" or "Maybe"` |
| 2 | `answer with information icon filesProcessed and "files were exported."` | `answer information filesProcessed && "files were exported."` |
| 3 | `answer "Greetings!" with buttons "Log In", "Cancel" with window titled "Login"` | `answer "Greetings!" with "Log In" or "Cancel" titled "Login"` |
| 4 | `answer my current prompt with buttons "OK", "Cancel" as sheet` | `answer the currPrompt of me with "OK" or "Cancel" as sheet` |

## Self-Contained Documentation

This project uses a self-contained documentation system:

- **Logs:** `logs/se-YYYY-MM-DD.md` - Daily project chat logs
- **References:** `docs/REFERENCES.md` - Project references with search anchors
- **Keywords:** `docs/KEYWORD_INDEX.md` - Concept linking keywords

## File Naming Convention

**NO SPACES in file/folder names** (critical for shell/URL compatibility):
- ✅ `my-stack.livecode`
- ✅ `my_stack.livecode`
- ❌ `my stack.livecode`

## Quick Start

1. Open project from PRIMARY location: `/Users/thawsoe/Documents/LiveCode/strict-english-livecode/`
2. Open in VSCode with recommended extensions
3. Review `docs/REFERENCES.md` for project references
4. Check `docs/KEYWORD_INDEX.md` for key concepts
5. To update dictionary: `node scripts/simple-import.js`

## Two-Folder Synchronization Protocol

When both machines are available:
- Work should be done in the PRIMARY (MBP) folder
- Use `hermes-sync push` to sync MBP → Big Mac
- Use `hermes-sync pull` to sync Big Mac → MBP (before starting work)
- If conflicts occur (different file creation dates), review both versions and merge as needed

## LiveCode Path Commands

```livecode
the filename of this stack  -- Get full path
the defaultFolder           -- Current directory
open stack "My Stack"       -- Open by name
```

## Resources

- **Dictionary Website:** https://thaw-soe.github.io/strict-english-dictionary/
- **GitHub:** https://github.com/Thaw-Soe/strict-english-dictionary
- **Backend:** Supabase

## Development

```bash
# Set up project (primary location)
cd /Users/thawsoe/Documents/LiveCode/strict-english-livecode
code .

# Update database with current definitions
node scripts/simple-import.js

# Verify livecode_syntax field (if null)
node scripts/update-livecode-syntax.js

# Build scripts
scripts/build.sh
scripts/test.sh

# Sync to Big Mac backup
hermes-sync push
```

---

## ⚠️ SINGLE SOURCE OF TRUTH

**For the `answer` command, update these files in order:**

1. `scripts/simple-import.js` (line 36-45: `answerDef` object)
2. `src/extension.ts` (Rules 1-5 transpilation logic)
3. `test-answer-grammar.js` (lines 9-29: test examples)
4. Run `node scripts/simple-import.js` to update database
5. Verify on https://thaw-soe.github.io/strict-english-dictionary/

**Database record ID:** 8374
**Last verified:** 2026-09-27

---

*Search Anchor: [Ref SE-readme-updated] Strict English LiveCode Project*