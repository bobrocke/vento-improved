# Installation Guide for Zed-Vento Extension

## Quick Start

1. **Clone or download this repository** to your local machine

2. **Open Zed** and access the command palette:
   - macOS: `Cmd+Shift+P`
   - Linux/Windows: `Ctrl+Shift+P`

3. **Run the installation command**:
   - Type "zed: install dev extension"
   - Press Enter

4. **Select the extension directory**:
   - Navigate to and select the `zed-vento` folder you cloned

5. **Verify installation**:
   - Open any `.vto` or `.vento` file
   - You should see syntax highlighting automatically applied

## Testing the Extension

Open the example file at `examples/front-matter-example.vto` to see:
- YAML syntax highlighting in the front matter section
- HTML syntax highlighting in the template body
- JavaScript syntax highlighting in code expressions
- Proper highlighting of Vento template tags

## Troubleshooting

### Extension not loading

1. Make sure you selected the correct folder (the one containing `extension.toml`)
2. Restart Zed after installation
3. Check Zed's logs for any error messages

### Syntax highlighting not working

1. Verify the file has `.vto` or `.vento` extension
2. Try closing and reopening the file
3. Reinstall the extension following the steps above

### Front matter not highlighting

1. Make sure the front matter starts at the very beginning of the file
2. Verify the YAML is between `---` delimiters:
   ```
   ---
   key: value
   ---
   ```
3. Check that there's a newline after the closing `---`

## Uninstalling

To remove the extension:
1. Open Zed's extensions panel
2. Find "VentoRJR" in the list
3. Click the uninstall button

## Development

If you want to modify the extension:

1. Edit the grammar in `grammars/vento/grammar.js`
2. Update the external scanner in `grammars/vento/src/scanner.c` if needed
3. Regenerate the parser:
   ```bash
   cd grammars/vento
   tree-sitter generate
   tree-sitter test
   tree-sitter build --wasm
   cp tree-sitter-vento.wasm ../vento.wasm
   ```
4. Reload the extension in Zed

## Support

For issues or questions:
- Check the [README](README.md) for documentation
- Review the [CHANGELOG](CHANGELOG.md) for recent updates
- Open an issue on the repository
