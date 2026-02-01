# Build Fixes Applied

## Problem
The Zed extension was failing to compile with error: "Failed to install dev extension: failed to compile grammar 'vento'"

## Root Cause
The grammar uses an external scanner (`src/scanner.c`) for parsing front matter content, but the build configuration files did not include it in the compilation process.

## Files Fixed

### 1. binding.gyp
**Issue**: Missing `src/scanner.c` in the sources list

**Fix**:
```gyp
"sources": [
  "bindings/node/binding.cc",
  "src/parser.c",
  "src/scanner.c"  // Added this line
],
```

### 2. bindings/rust/build.rs
**Issue**: Scanner not included in Rust compilation

**Fix**:
```rust
// Include the external scanner
let scanner_path = src_dir.join("scanner.c");
c_config.file(&scanner_path);
println!("cargo:rerun-if-changed={}", scanner_path.to_str().unwrap());
```

### 3. bindings/node/binding.cc
**Issue**: Incorrect function names referencing old grammar

**Fix**:
- Changed `tree_sitter_embedded_template()` to `tree_sitter_vento()`
- Changed name from `"embedded_template"` to `"vento"`
- Updated NODE_MODULE name to `tree_sitter_vento_binding`

### 4. src/scanner.c
**Issue**: Non-static function causing potential naming conflicts

**Fix**:
```c
// Changed from: void skip_whitespace(TSLexer *lexer)
static void skip_whitespace(TSLexer *lexer)
```

## Verification

All changes have been:
✅ Applied to both repositories:
   - `/Users/bob/Documents/Development/Zed Stuff/Tree-Sitters/tree-sitter-vento`
   - `/Users/bob/Documents/Development/Zed Stuff/Languages/zed-vento`

✅ Tested with `tree-sitter test` - All tests pass
✅ WASM rebuilt successfully - Size optimized to 9.6K
✅ No compilation warnings

## Installation

Now you can install the extension in Zed:

1. Open Zed
2. Press `Cmd+Shift+P` (macOS) or `Ctrl+Shift+P` (Linux/Windows)
3. Run: "zed: install dev extension"
4. Select: `/Users/bob/Documents/Development/Zed Stuff/Languages/zed-vento`

The extension should now compile and load successfully! 🎉

## Additional Fix - Header Include Style

### 5. src/scanner.c (Include Path)
**Issue**: Using angled brackets for tree-sitter header caused Zed compilation to fail

**Error**: 
```
'tree_sitter/parser.h' file not found with <angled> include; use "quotes" instead
```

**Fix**:
```c
// Changed from: #include <tree_sitter/parser.h>
#include "tree_sitter/parser.h"
```

This uses relative path resolution which works better in Zed's compilation environment.

## Final Verification
✅ All tests pass
✅ WASM builds successfully (9.6K)
✅ Scanner compiles with correct include style
✅ Parser correctly identifies front matter
