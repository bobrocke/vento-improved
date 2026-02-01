# Test Instructions

## Create this test file in Zed

Save as `test.vto`:

```vento
{{ set name = "Bob" }}
{{ layout "base.vto" }}
{{ echo message }}
{{ slot content }}
{{ default value = 42 }}

{{ if condition }}
  content
{{ /if }}
```

## What You Should See

After installing the extension (v0.0.2):

- `set`, `layout`, `echo`, `slot`, `default`, `if` should be highlighted as **keywords** (same color as other language keywords)
- `name = "Bob"`, `"base.vto"`, `message`, etc. should be highlighted as **code** (JavaScript syntax)

## Debugging Steps

1. **Verify extension is installed**: Check Zed's extensions panel
2. **Reload Zed**: Completely quit and restart Zed
3. **Check file extension**: Make sure file ends in `.vto` or `.vento`
4. **Check language mode**: Bottom right of Zed should show "Vento"

## Parse Test

To verify the grammar is working:

```bash
cd "/Users/bob/Documents/Development/Zed Stuff/Languages/zed-vento/grammars/vento"
echo '{{ set x = 1 }}' | tree-sitter parse
```

Should show:
```
(template
  (tag
    (keyword)  ← "set" should be here
    (code)))   ← "x = 1" should be here
```

## If Still Not Working

Please specify exactly what you see:
- Is `set` highlighted as code/variable?
- Is `set` not highlighted at all?
- Is the whole `{{ set x = 1 }}` one color?
- Screenshot would be helpful!
