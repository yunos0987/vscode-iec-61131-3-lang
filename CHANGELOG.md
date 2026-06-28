# Changelog

All notable changes to the "iec-61131-3" extension will be documented in this file.

## [0.1.0] - 2026-06-27

### Added

- Syntax highlighting for IEC 61131-3
  - Control flow keywords (if, for, while, case, repeat, etc.)
  - POU declarations (function, function_block, program, method, class, etc.)
  - Variable section keywords (var, var_input, var_output, var_in_out, etc.)
  - Storage modifiers (retain, constant, at, public, private, etc.)
  - Data types (bool, int, dint, real, lreal, string, time, etc.)
  - Typed literals (e.g. `dint#123`, `byte#16#FF`, `bool#trie`)
  - Duration literals (e.g. `t#1h30m`, `time#500ms`)
  - Date/time literals (e.g. `d#2024-01-01`, `tod#12:00:00`)
  - Numeric literals (decimal, hex `16#`, binary `2#`, octal `8#`)
  - Direct addresses (e.g. `%IX0.0`, `%QW1`, `%MD2`)
  - Operators (`:=`, `?=`, `and`, `or`, `not`, `mod`, `xor`, etc.)
  - Line comments (`//`) and block comments (`(*..*)`, `/*..*/`)
  - Pragmas (`{...}`)
  - Single-quoted and double-quoted strings with `$`-escape sequences
- Language configuration
  - Bracket matching for `()` and `[]`
  - Auto-closing pairs for `()`, `[]`, `(**)`, `/**/`, `''`, `""`
  - Indentation rules for ST block keywords
- File association for `.iec`, `.st`, and `.iecst` extensions
