# Pre-Installation Checklist ✅

Before installing in Zed, verify:

## Files Present
- [x] `grammars/vento.wasm` exists (9.6KB)
- [x] `grammars/vento/src/scanner.c` exists
- [x] `grammars/vento/src/parser.c` exists
- [x] `languages/vento/highlights.scm` exists
- [x] `languages/vento/injections.scm` exists
- [x] `languages/vento/config.toml` exists
- [x] `extension.toml` exists

## Build Configuration
- [x] `binding.gyp` includes `src/scanner.c`
- [x] `bindings/rust/build.rs` includes scanner
- [x] `Cargo.toml` has no syntax errors
- [x] `bindings/node/binding.cc` uses `tree_sitter_vento()`

## Code Quality
- [x] `scanner.c` uses `"tree_sitter/parser.h"` (quotes)
- [x] `skip_whitespace` is static
- [x] All tests pass
- [x] Cargo builds successfully
- [x] WASM builds successfully

## Try These Commands

```bash
cd /Users/bob/Documents/Development/Zed\ Stuff/Languages/zed-vento/grammars/vento

# Should pass
tree-sitter test

# Should compile
cargo build

# Should generate WASM
tree-sitter build --wasm
```

All passing? ✅ **Ready to install in Zed!**

## If Still Failing

If Zed still shows compilation errors, try:

1. **Clean build**:
   ```bash
   cd grammars/vento
   cargo clean
   rm -rf target
   tree-sitter generate
   tree-sitter build --wasm
   cp tree-sitter-vento.wasm ../vento.wasm
   ```

2. **Check Zed logs**: Look for specific error messages

3. **Try without dev mode**: Sometimes Zed's dev extension mode has stricter requirements

4. **Verify Zed version**: Make sure you're using a recent version of Zed
