# Godot Docs LLM Converter (DEPRECATED)

**THIS PROJECT IS NOW OBSOLETE, USE THE [mcp-server](https://github.com/Zei33/godot-mcp) INSTEAD. IT IS SIGNIFICANTLY IMPROVED AND MORE PRACTICAL FOR TOKEN USAGE!**

A complete TypeScript solution that automatically converts Godot documentation from RST format to clean, LLM-friendly Markdown. Designed specifically for use as context rules in Cursor and other AI-powered development tools.

## 🌟 Key Features

- 🚀 **Fully Automated**: Git clone/pull, conversion, and cleanup in one command
- 📊 **Smart Table Reconstruction**: Automatically rebuilds Properties and Methods tables
- 🔄 **GitHub Flavored Markdown**: Clean, readable output optimized for LLM consumption
- 🗣️ **Language Filtering**: Extract GDScript-only, C#-only, or both language examples
- ⚡ **Concurrent Processing**: Configurable workers for optimal performance
- 🧹 **Aggressive Cleanup**: Removes RST/Sphinx markup, HTML, and browser artifacts
- 📁 **Smart File Discovery**: Processes only relevant documentation files
- ⚙️ **Highly Configurable**: JSON-based configuration with sensible defaults

## 🏗️ Table Reconstruction

One of the major features is intelligent table reconstruction. When pandoc fails to convert RST tables properly, the converter automatically:

- **Extracts Properties**: Creates clean tables from property descriptions
- **Extracts Methods**: Builds method signature tables from detailed descriptions
- **Preserves Signatures**: Maintains proper typing and parameter information
- **Clean Formatting**: Removes markup artifacts while preserving structure

### Before (Empty Sections)
```markdown
## Properties

## Methods
```

### After (Populated Tables)
```markdown
## Properties

| Type | Property | Default |
|------|----------|---------|
| Vector2 | **global_position** | Vector2(0, 0) |
| float | **rotation** | 0.0 |

## Methods

| Return Type | Method |
|-------------|--------|
| void | **apply_scale**(ratio: Vector2) |
| Transform2D | **get_global_transform**() |
```

## 🛠️ Prerequisites

- [Node.js](https://nodejs.org/) (v16 or higher)
- [Git](https://git-scm.com/) (for automatic repository management)
- [Pandoc](https://pandoc.org/) (for RST to Markdown conversion)
- pnpm or npm

## 📦 Installation

1. **Clone and setup**:
```bash
git clone <your-repo-url>
cd godot-docs-llm
pnpm install  # or npm install
```

2. **Verify dependencies**:
```bash
git --version
pandoc --version
```

## 🚀 Quick Start

Run with default settings (recommended):
```bash
pnpm start
```

This will:
- Show compatibility warnings
- Clone/update the Godot docs repository
- Convert 1000+ RST files to clean Markdown
- Generate `llms.md` with complete class reference

## ⚙️ Configuration

### Default Configuration

The converter comes with optimal defaults:

```json
{
  "excludedDirectories": ["about", "community", "contributing", "tutorials"],
  "godotDocsPath": "./godot-docs",
  "outputFile": "./llms.md",
  "concurrency": 5,
  "language": "both",
  "git": {
    "enabled": true,
    "repository": "https://github.com/godotengine/godot-docs.git",
    "branch": "master",
    "autoUpdate": true
  }
}
```

### Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `excludedDirectories` | `string[]` | `["about", "community", "contributing", "tutorials"]` | Directories to skip during conversion |
| `godotDocsPath` | `string` | `"./godot-docs"` | Local path for the Godot docs repository |
| `outputFile` | `string` | `"./llms.md"` | Output file for the combined Markdown |
| `concurrency` | `number` | `5` | Number of files to process simultaneously |
| `language` | `string` | `"both"` | Language filter: `"gdscript"`, `"csharp"`, or `"both"` |
| `git.enabled` | `boolean` | `true` | Enable automatic git operations |
| `git.repository` | `string` | Official Godot docs URL | Git repository to clone/pull from |
| `git.branch` | `string` | `"master"` | Branch to checkout and track |
| `git.autoUpdate` | `boolean` | `true` | Automatically pull updates if repo exists |

### Pre-configured Examples

Use these configurations for specific workflows:

#### GDScript Only
```bash
cp config-gdscript.json converter.config.json
pnpm start
```
Generates: `godot-gdscript-llm.md` (6.6MB, GDScript-only examples)

#### C# Only
```bash
cp config-csharp.json converter.config.json
pnpm start
```
Generates: `godot-csharp-llm.md` (6.6MB, C#-only examples)

#### Comprehensive Documentation (Larger Size)
For maximum context including tutorials and general information:
```json
{
  "excludedDirectories": ["community", "contributing"],
  "outputFile": "./godot-complete-llm.md",
  "concurrency": 25,
  "language": "both"
}
```
**Trade-offs**:
- ✅ **More comprehensive**: Includes tutorials, getting started guides, and about pages
- ✅ **Better context**: General concepts and workflow explanations
- ❌ **Larger size**: ~8-10MB output (increased token usage)
- ❌ **More noise**: Some content may be less relevant for code completion

#### Class Reference Only (Smaller Size)
For focused API documentation:
```json
{
  "excludedDirectories": ["about", "community", "contributing", "tutorials", "getting_started"],
  "outputFile": "./godot-classes-only-llm.md",
  "concurrency": 25,
  "language": "both"
}
```
**Trade-offs**:
- ✅ **Focused content**: Only class reference and API docs
- ✅ **Smaller size**: ~4-5MB output (reduced token usage)
- ❌ **Less context**: Missing workflow and conceptual information

#### Custom Configuration
Create your own `converter.config.json`:
```json
{
  "excludedDirectories": ["about", "community"],
  "outputFile": "./godot-classes-only.md",
  "concurrency": 10,
  "language": "gdscript",
  "git": {
    "branch": "4.2-stable",
    "autoUpdate": false
  }
}
```

## 🎯 Git Integration

### Automatic Repository Management

The converter automatically handles the Godot docs repository:

**First Run**: Clones the repository
```
📥 Cloning Godot docs repository...
   Repository: https://github.com/godotengine/godot-docs.git
   Branch: master
   Destination: ./godot-docs
✅ Repository cloned successfully
📍 Using commit: 75fd92db (2025-05-28)
```

**Subsequent Runs**: Updates existing repository
```
📦 Godot docs repository found at ./godot-docs
🔄 Updating repository (branch: master)...
✅ Repository updated successfully
📍 Using commit: 75fd92db (2025-05-28)
```

### Git Configuration Options

- **`git.enabled: false`**: Skip git operations, use existing files
- **`git.branch: "4.2-stable"`**: Use a specific stable branch
- **`git.autoUpdate: false`**: Don't pull updates on subsequent runs
- **`git.repository: "<url>"`**: Use a fork or different repository

### ⚠️ Compatibility Warning

```
⚠️  COMPATIBILITY WARNING:
   This parser is designed for Godot docs master branch as of May 30, 2025.
   Using different branches or older commits may result in parsing issues.
   For best results, use the default master branch.
```

## 🏃‍♂️ Performance & Optimization

### Concurrent Processing

The converter processes multiple files simultaneously:

- **Default (5 workers)**: Balanced for most systems
- **Low concurrency (1-3)**: Better for limited RAM/storage
- **High concurrency (8-16)**: Faster on powerful systems

### Performance Metrics

Typical performance on modern hardware:
- **Files processed**: ~1,039 RST files
- **Processing time**: 1-2 minutes
- **Output size**: ~6MB (clean, compressed)
- **Memory usage**: <500MB peak

### File Size & Token Implications

Different exclusion strategies affect output size and LLM token usage:

| Configuration | Output Size | Est. Tokens* | Best For |
|---------------|-------------|--------------|----------|
| **Classes only** | ~4-5MB | ~800K-1M | API reference, autocompletion |
| **Default** (excludes tutorials) | ~6MB | ~1.2M | Balanced development context |
| **Learning focused** (+tutorials) | ~8MB | ~1.6M | Learning, understanding patterns |
| **Complete** (+about +tutorials) | ~10MB | ~2M | Maximum context, research |

*Token estimates based on typical markdown compression ratios

**Choosing the right size**:
- **Cursor**: Consider your context window limits (e.g., Claude Sonnet 200K tokens)
- **Development**: Smaller is often better for focused code completion
- **Learning**: Larger provides more examples and explanations

### Optimization Tips

```json
{
  "concurrency": 8,              // Increase for faster processing
  "excludedDirectories": [       // Exclude more to reduce processing
    "about", "community", "contributing", 
    "tutorials", "getting_started"
  ],
  "git": {
    "autoUpdate": false          // Skip updates for faster subsequent runs
  }
}
```

## 📂 File Processing Rules

### ✅ Included Files
- RST files in subdirectories of `godot-docs/`
- Files without `.` or `_` prefixes
- Files not in excluded directories
- Class reference documentation
- API documentation

### ❌ Excluded Files
- Root-level RST files
- Hidden files (starting with `.`)
- Template files (starting with `_`)
- Tutorial and community content (by default)
- Image files and assets

### 📁 Directory Content Overview

Understanding what each directory contains helps you customize `excludedDirectories`:

| Directory | Content | Size Impact | Usefulness for LLMs |
|-----------|---------|-------------|-------------------|
| `classes/` | **Class reference** - API docs, methods, properties | High | ⭐⭐⭐⭐⭐ Essential |
| `getting_started/` | **Beginner tutorials** - first projects, basic concepts | Medium | ⭐⭐⭐⭐ Very helpful for workflow |
| `tutorials/` | **Advanced tutorials** - specific techniques, best practices | High | ⭐⭐⭐ Good for context & patterns |
| `about/` | **General information** - engine philosophy, release notes | Low | ⭐⭐ Background knowledge |
| `community/` | **Community guidelines** - asset library, contribution | Low | ⭐ Rarely relevant for coding |
| `contributing/` | **Development docs** - engine development, building | Low | ⭐ Only for engine contributors |

**Recommendation by use case**:

- **Code completion & API reference**: Include only `classes/` + `getting_started/`
- **Learning & workflows**: Include `classes/` + `getting_started/` + `tutorials/`
- **Complete context**: Include everything except `community/` + `contributing/`

### Example Structure Processing
```
godot-docs/
├── index.rst                    # ❌ Root level
├── _templates/                  # ❌ Starts with _
├── about/                       # ❌ In excludedDirectories
├── classes/
│   ├── class_node.rst          # ✅ Class documentation
│   └── class_scene.rst         # ✅ Class documentation
└── getting_started/             # ✅ Included (unless excluded)
    └── introduction.rst        # ✅ Getting started guide
```

## 🧹 Cleanup Features

### Aggressive Markup Removal
- RST/Sphinx directives and roles
- HTML divs and browser artifacts
- CSS classes and style annotations
- Broken table markup from failed conversions
- Image references and media content

### Content Preservation
- **Programming Identifiers**: Preserves `camelCase`, `PascalCase`, and `snake_case`
- **Method Signatures**: Maintains `method_name()` format
- **Type Information**: Keeps class names and type hints
- **Code Examples**: Preserves formatted code blocks

### Language Filtering
Smart extraction of language-specific content:

**GDScript Example**:
```gdscript
var astar = AStar2D.new()
astar.add_point(1, Vector2(1, 0))
```

**C# Example**:
```csharp
var astar = new AStar2D();
astar.AddPoint(1, new Vector2(1, 0));
```

## 📊 Output Quality

### Before Conversion (Raw RST)
```rst
:class:`Vector2<class_Vector2>` **global_position** = ``Vector2(0, 0)`` :ref:`🔗<class_Node2D_property_global_position>`

    Global position of the node. This is equivalent to ``position + get_parent().global_position``.
```

### After Conversion (Clean Markdown)
```markdown
**Vector2 global_position** = `Vector2(0, 0)`

Global position of the node. This is equivalent to `position + get_parent().global_position`.
```

## 🚨 Troubleshooting

### Common Issues

**Git not found**:
```bash
# Install Git
# macOS: brew install git
# Ubuntu: sudo apt install git
# Windows: Download from git-scm.com
```

**Pandoc not found**:
```bash
# Install Pandoc
# macOS: brew install pandoc
# Ubuntu: sudo apt install pandoc
# Windows: Download from pandoc.org
```

**Memory issues**:
```json
{
  "concurrency": 1,  // Reduce concurrent processing
  "excludedDirectories": ["about", "community", "contributing", "tutorials", "getting_started"]
}
```

**Git authentication issues**:
```json
{
  "git": {
    "enabled": false  // Disable git, use existing docs
  }
}
```

### Debug Mode

For troubleshooting, check the generated output:
```bash
# Check if git worked
ls -la godot-docs/

# Check file count
find godot-docs/classes -name "*.rst" | wc -l

# Check output
head -50 llms.md
```

## 📈 Development

### Build and Run
```bash
pnpm run build  # Compile TypeScript
pnpm start      # Run converter
pnpm run dev    # Build + Run
```

### Architecture

```
src/index.ts
├── Git Management (clone/pull)
├── File Discovery (RST finder)
├── Language Filtering (tabs parser)
├── Pandoc Conversion (RST → GFM)
├── Cleanup Pipeline (markup removal)
├── Table Reconstruction (properties/methods)
└── File Combination (single output)
```

## 📄 License

This project is open source. The generated documentation content belongs to the Godot project and is used under their documentation license.

## 🤝 Contributing

Contributions welcome! Areas for improvement:
- Additional markup cleanup patterns
- Better table reconstruction logic
- Support for other documentation formats
- Performance optimizations

## 📞 Support

- **Issues**: Report bugs or request features
- **Docs**: Complete examples in config files
- **Performance**: Adjust concurrency for your system
- **Compatibility**: Stick to master branch for best results

---

**Generated output ready for Cursor, Claude, or any LLM! 🎉** 
