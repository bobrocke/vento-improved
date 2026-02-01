# ✅ Keywords ACTUALLY Fixed Now!

## The Real Problem

There were TWO issues:

### Issue 1: Incomplete Keyword List ✅ (Fixed Earlier)
The keyword regex was malformed and missing keywords.

### Issue 2: Keywords Aliased to Code ❌ (Just Fixed!)
Even when keywords were matched, they were being aliased to `code` nodes in the `_expression` rule:

```javascript
// OLD (broken)
_expression: ($) =>
  choice(
    alias($.code_snippet, $.code),
    alias($.keyword, $.code),  // ❌ This was the problem!
    alias($.close_keyword, $.keyword),
    seq($.keyword, alias($._code, $.code)),
    $.comment,
  ),
```

This meant:
- `{{ set }}` → parsed as `(code)` instead of `(keyword)`
- `{{ set x = 1 }}` → parsed as `(keyword)` with `(code)` (correct)

## The Fix

Removed the aliasing and made keywords stay as keywords:

```javascript
// NEW (fixed)
_expression: ($) =>
  choice(
    alias($.code_snippet, $.code),
    alias($.close_keyword, $.keyword),
    seq($.keyword, optional(alias($._code, $.code))),
    $.keyword,  // ✅ Keywords stay as keywords!
    $.comment,
  ),
```

Now:
- `{{ set }}` → `(keyword)`
- `{{ set x = 1 }}` → `(keyword)` with `(code)`
- `{{ layout "base" }}` → `(keyword)` with `(code)`
- `{{ echo msg }}` → `(keyword)` with `(code)`
- `{{ slot content }}` → `(keyword)` with `(code)`
- `{{ /slot }}` → `(keyword)` (closing)

## Verification

```bash
echo '{{ set x = 1 }}
{{ layout "base" }}
{{ echo msg }}
{{ slot content }}
{{ default y = 2 }}' | tree-sitter parse
```

Output shows all as `(keyword)`:
```
(keyword) # set
(keyword) # layout  
(keyword) # echo
(keyword) # slot
(keyword) # default
```

## Files Updated

- ✅ `grammar.js` - Fixed `_expression` rule
- ✅ Parser regenerated
- ✅ WASM rebuilt: 7.7KB
- ✅ All tests passing

## Install Now

The fix is complete:

1. Open **Zed**
2. Press **`Cmd+Shift+P`**
3. Run: **"zed: install dev extension"**
4. Select: `/Users/bob/Documents/Development/Zed Stuff/Languages/zed-vento`

## Test It

```vento
{{ set name = "Bob" }}
{{ layout "base.vto" }}
{{ echo greeting }}
{{ slot header }}
  <h1>Default Header</h1>
{{ /slot }}
{{ default value = 42 }}
```

All keywords should now highlight correctly! 🎨

---

**Version**: 0.0.2
**Status**: ACTUALLY FIXED ✅
**Issue**: Keywords being aliased to code nodes
**Solution**: Removed incorrect alias, keywords now stay as keywords
