# IEC 61131-3 Language Extension for VS Code

## Project Overview

A pure grammar extension for Visual Studio Code that provides syntax highlighting
and language configuration for IEC 61131-3 files.

- **No TypeScript / JavaScript runtime.** The extension consists solely of JSON files.
- **Supported file extensions:** `.iec`, `.st`, `.iecst`
- **Language ID:** `iec-61131-3`
- **Publisher:** `graviness.com`

## Rules

### Language

All source code, JSON comments, and documentation must be written in **English**.
`README.md` is also English.

### Grammar file (`syntaxes/iec-61131-3.tmLanguage.json`)

- **Scope**: This extension targets **IEC 61131-3** as a whole, not Structured Text (ST) alone.
  Never describe the extension as "Structured Text" in documentation, descriptions, or
  display names. Use "IEC 61131-3" as the language identity.
- Format: TextMate grammar (JSON). Do **not** convert to YAML or PLIST.
- IEC 61131-3 keywords are **case-insensitive**. Use the inline flag `(?i:...)` in
  every keyword regex (e.g., `(?i:if|end_if)`). Do not rely on the `ignoreCase`
  flag at the grammar root level.
- Escape character in strings and pragmas is **`$`** (not `\`).
  - Single-quoted string escapes: `$l`, `$L`, `$n`, `$N`, `$p`, `$P`, `$r`, `$R`,
    `$t`, `$T`, `$"`, `$'`, `$$`, and two-hex-digit form `$XX`.
  - Double-quoted string escapes: same set but four-hex-digit form `$XXXX`.
  - Pragma (`{...}`) escapes: `${`, `$}`, `$$` only.
- **`this` and `_`** are classified as `variable.language` scope
  (analogous to C++ `this`), NOT as keywords.
- **`using`** belongs to `keyword.other.object.iec-61131-3` (`keyword-object` rule).
  It must **not** appear in `keyword-operator`.
- Scope naming convention: `<type>.<subtype>.<detail>.iec-61131-3`
  (e.g., `keyword.control.iec-61131-3`, `variable.language.this.iec-61131-3`).

### Language configuration (`language-configuration.json`)

- `{}` braces are used for **pragmas** and must **not** be added to
  `brackets` or `autoClosingPairs`.
- Block comment delimiters: `(*` / `*)` (primary) and `/*` / `*/` (secondary).
- Line comment: `//`.

### Packaging (`.vscodeignore`)

- Any directory intended for development use only (e.g., `_ws/`) must be listed
  in `.vscodeignore` with a glob pattern (`_ws/**`).
- `.git/info/exclude` is **ignored by vsce**. Use `.vscodeignore` exclusively
  to control what is packaged.
- The `"license"` field in `package.json` must be an SPDX identifier (e.g., `"MIT"`).
  Using `"SEE LICENSE IN LICENSE.md"` causes a vsce WARNING when the actual file
  is `LICENSE` (no extension).

## Building

This extension has no compilation step. All files are static JSON.

```sh
# Preview what will be included in the package (run before packaging)
npx @vscode/vsce ls

# Create the .vsix package
npx @vscode/vsce package
```

Verify the output: zero WARNINGs, `_ws/` not included, `language-configuration.json`
present at the root level inside the archive.

## Testing and Validation

1. Open the repository in VS Code.
2. Press **F5** to launch the Extension Development Host.
3. Open any `.iec`, `.st`, or `.iecst` file and confirm syntax highlighting applies.
4. Test comment toggle (**Ctrl+/**), bracket auto-close, and indentation on `IF`/`END_IF` blocks.

Automated test suite: none (grammar-only extension).

## Publishing

```sh
npx @vscode/vsce publish
```

Requires a Personal Access Token (PAT) with the **Marketplace (manage)** scope.
Set the token via `vsce login <publisher>` or the `VSCE_PAT` environment variable.
The publisher ID is defined in `package.json` under `"publisher"`.

## Project Structure and Key Files

```
vscode-iec-61131-3-lang/
├── package.json                      # Extension manifest (contributes.languages + grammars)
├── language-configuration.json       # Bracket matching, auto-close pairs, indentation rules
├── syntaxes/
│   └── iec-61131-3.tmLanguage.json  # TextMate grammar (syntax highlighting rules)
├── README.md                         # User-facing documentation (English)
├── CHANGELOG.md                      # Version history
├── LICENSE                           # MIT license
├── .vscodeignore                     # Files excluded from the .vsix package
├── .github/
│   └── copilot-instructions.md      # This file — AI coding rules for the project
└── _ws/                              # Development workspace (git-excluded, vsce-excluded)
```
