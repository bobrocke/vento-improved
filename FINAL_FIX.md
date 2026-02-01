# ✅ KEYWORDS FIXED WITH PRECEDENCE

## The Real Problem

Even with the correct choice order, the parser was still matching `{{set}}` as `code` instead of `keyword` because tree-sitter couldn't decide between:
- `seq($.keyword, alias($._code, $.code))` - keyword with code
- `$.keyword` - keyword alone
- `alias($.code_snippet, $.code)` - code snippet

Without explicit precedence, tree-sitter was choosing the wrong alternative.

## The Solution: Explicit Precedence

Added precedence values to guide the parser:

```javascript
_expression: ($) =>
  choice(
    $.comment,
    alias($.close_keyword, $.keyword),
    prec(2, seq($.keyword, alias($._code, $.code))),  // Try keyword+code first
    prec(3, $.keyword),                                // Then keyword alone (highest)
    prec(1, alias($.code_snippet, $.code)),           // Code last (lowest)
  ),
```

Higher precedence = preferred choice when ambiguous.

## Test Results ✅

### Solo Keywords
```vento
{{set}}      → (keyword) ✅
{{layout}}   → (keyword) ✅
{{echo}}     → (keyword) ✅
{{slot}}     → (keyword) ✅
{{default}}  → (keyword) ✅
```

### Keywords with Code
```vento
{{set x = 1}}         → (keyword) (code) ✅
{{layout "base"}}     → (keyword) (code) ✅
{{echo message}}      → (keyword) (code) ✅
```

### Closing Keywords
```vento
{{/set}}   → (keyword) ✅
{{/slot}}  → (keyword) ✅
```

## What Changed

### Files Updated
- ✅ `grammar.js` - Added precedence to `_expression` choices
- ✅ Parser regenerated
- ✅ WASM rebuilt: 7.7KB
- ✅ All tests passing

### Version
- Updated to **0.0.3**

## Installation

```bash
# In Zed:
1. Cmd+Shift+P
2. "zed: install dev extension"
3. Select: /Users/bob/Documents/Development/Zed Stuff/Languages/zed-vento
4. **QUIT AND RESTART ZED** (important!)
```

## Expected Result in Zed

Create `test.vto`:
```vento
{{set}}
{{layout}}
{{echo}}
{{slot}}
{{default}}
```

All should now be highlighted as **keywords** (not variables)! 🎉

---

**Status**: FIXED WITH PRECEDENCE ✅
**Root Cause**: Parser ambiguity without precedence
**Solution**: Added prec(3) to solo keywords, prec(2) to keyword+code, prec(1) to code
**Version**: 0.0.3
**WASM**: 7.7KB
