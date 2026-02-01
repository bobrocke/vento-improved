# Keyword Recognition Fix

## Issue
The following Vento keywords were being parsed as regular code instead of keywords:
- `set`
- `layout`
- `echo`
- `slot`
- `default`

## Root Cause
The keyword regex pattern was malformed:
```javascript
// Old (broken)
keyword: () => /[a-z>][a-zA-Z]*? |if|from|of|for|include|set|import|export|layout|function/,
```

This pattern had two issues:
1. The first part `[a-z>][a-zA-Z]*? ` (with space) was matching any lowercase word followed by a space
2. It would match before reaching the specific keyword alternatives

## Fix Applied
Replaced with a clean list of all Vento keywords:
```javascript
// New (fixed)
keyword: () => /if|else|for|from|of|include|set|import|export|layout|function|echo|slot|default/,
```

Also updated closing keywords:
```javascript
close_keyword: () => /\/(if|for|include|set|import|export|layout|function|echo|slot|default)/,
```

## Keywords Now Recognized

### Control Flow
- `if` / `/if`
- `else`
- `for` / `/for`
- `from`
- `of`

### Template Operations
- `set` / `/set` - Variable assignment
- `layout` - Template layout
- `echo` - Output expression
- `slot` / `/slot` - Content slots
- `default` - Default values

### Module System
- `include`
- `import`
- `export`
- `function`

## Testing

```vento
{{ set title = "Test" }}
{{ layout "base.vto" }}
{{ echo message }}
{{ slot content }}
  Default content
{{ /slot }}
{{ default value }}
```

All keywords are now correctly identified as `(keyword)` nodes instead of being treated as regular code.

## Files Updated
- `grammar.js` - Fixed keyword regex
- `corpus/main.txt` - Added test cases for new keywords
- Regenerated parser and WASM

## WASM Size
- Previous: 9.6KB
- Current: 7.7KB (simplified regex made it smaller!)

---

**Status**: Fixed ✅
**Version**: 0.0.1
**Last Updated**: February 1, 2024
