# Changelog

All notable changes to the Zed-Vento extension will be documented in this file.

## [0.0.2] - 2024-02-01

### Fixed
- **Keyword Recognition**: Fixed `set`, `layout`, `echo`, `slot`, and `default` to be properly recognized as keywords
- Simplified keyword regex pattern from complex alternation to clean keyword list
- Added `else` keyword support
- Updated close_keyword pattern to include all closing tags

### Added
- Test cases for all Vento keywords in corpus

### Changed
- WASM size reduced from 9.6KB to 7.7KB due to simplified regex

## [0.0.1] - 2024-02-01

### Added
- YAML front matter support with proper syntax highlighting
- Front matter content is now properly parsed as a separate node (`front_matter_content`)
- YAML language injection for front matter blocks
- External scanner for accurate front matter parsing
- Support for both empty and non-empty front matter blocks
- Example files demonstrating front matter usage

### Changed
- Updated tree-sitter-vento grammar to use external scanner for front matter
- Improved front matter delimiter matching
- Updated injection queries to target `front_matter_content` instead of `front_matter`
- Enhanced highlights query with front matter delimiter styling

### Fixed
- Front matter now correctly stops at closing `---` delimiter
- Front matter no longer consumes content outside its boundaries
- Empty front matter blocks (`---\n---`) now parse correctly
- External scanner properly included in build configuration
- Header include style changed to use quotes instead of angle brackets
- Cargo.toml syntax error (trailing comma) fixed
- All compilation errors resolved

## [0.0.0-devel.1] - Previous

### Added
- Initial Vento language support
- Basic syntax highlighting
- HTML and JavaScript injections
- Auto-closing brackets
