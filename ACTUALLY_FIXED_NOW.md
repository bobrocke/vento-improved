# ✅ KEYWORDS FIXED - FOR REAL THIS TIME!

## The ACTUAL Problem

When keywords appeared **alone** (like `{{set}}`, `{{layout}}`), they were being parsed as `code` instead of `keyword`.

### Root Cause: Parser Choice Order

The `_expression` rule had choices in the wrong order:

```javascript
// OLD (broken)
_expression: ($) =>
  choice(
    alias($.code_snippet, $.code),  // ❌ This matched first!
    alias($.close_keyword, $.keyword),
    seq($.keyword, optional(alias($._code, $.code))),
    $.keyword,
    $.comment,
  ),
```

Since `code_snippet` is `/[a-zA-Z>...]` followed by `$._code`, it would match words like "set", "layout", etc. **before** the parser tried the keyword rules.

### The Fix: Keyword-First Order

```javascript
// NEW (fixed)
_expression: ($) =>
  choice(
    $.comment,                                      // 1. Comments first
    alias($.close_keyword, $.keyword),              // 2. Closing tags
    seq($.keyword, alias($._code, $.code)),         // 3. Keywords with code
    $.keyword,                                      // 4. Keywords alone
    alias($.code_snippet, $.code),                  // 5. Code last (fallback)
  ),
```

Now the parser tries keywords BEFORE falling back to code_snippet.

## Test Results

### Solo Keywords (Now Working!)
```vento
{{set}}      → (keyword)
{{layout}}   → (keyword)
{{echo}}     → (keyword)
{{slot}}     → (keyword)
{{default}}  → (keyword)
```

### Keywords with Parameters (Still Working!)
```vento
{{set x = 1}}         → (keyword) (code)
{{layout "base"}}     → (keyword) (code)
{{echo message}}      → (keyword) (code)
{{slot content}}      → (keyword) (code)
{{default y = 2}}     → (keyword) (code)
```

### Closing Keywords (Still Working!)
```vento
{{/set}}   → (keyword)
{{/slot}}  → (keyword)
{{/if}}    → (keyword)
```

## Files Updated

- ✅ `grammar.js` - Fixed `_expression` choice order
- ✅ Parser regenerated
- ✅ WASM rebuilt: 7.7KB
- ✅ All tests passing
- ✅ Version: 0.0.3

## Install Now!

```bash
# In Zed:
1. Cmd+Shift+P
2. "zed: install dev extension"
3. Select: /Users/bob/Documents/Development/Zed Stuff/Languages/zed-vento
```

**IMPORTANT**: After installation, completely quit and restart Zed to ensure the new grammar loads!

## Verify It Works

Create `test.vto`:
```vento
{{set}}
{{set name = "Bob"}}
{{layout}}
{{layout "base.vto"}}
{{echo}}
{{echo message}}
```

All instances of `set`, `layout`, `echo` should now be highlighted as **keywords**! 🎉

---

**Status**: ACTUALLY FIXED ✅
**Issue**: Parser choice order - code_snippet before keywords
**Solution**: Keywords first, code_snippet last
**Version**: 0.0.3
