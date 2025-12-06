# DecentSampler Developer's Guide - AI Agent Instructions

## Project Overview

This is a **Sphinx documentation project** for the DecentSampler format—an XML-based format for creating sample libraries. The docs explain how to write `.dspreset` files (XML files that define sample mappings, UI elements, effects, MIDI routing, etc.).

**Key Architecture:**
- `docs/source/` contains Markdown files (MyST parser) that document XML elements and attributes
- Sphinx builds HTML/PDF/EPUB outputs from these sources
- Documentation is hosted on ReadTheDocs (configured via `.readthedocs.yaml`)
- Primary audience: Sample library creators learning the DecentSampler XML format

## Essential Workflows

### Building Documentation

**Command:** Run the build task from `docs/` directory:
```bash
cd docs && make html
```
Or use VS Code task: "make html" (defined in workspace tasks)

**Output location:** `docs/build/html/`

**Live preview:** Open `docs/build/html/index.html` in browser after building

### Documentation Structure

```
docs/source/
├── index.rst           # Main TOC (reStructuredText format)
├── introduction.md     # Format overview, <DecentSampler> element
├── the-groups-element.md       # <groups>, <group>, <sample> (core mapping)
├── the-ui-element.md           # UI controls, buttons, knobs, tabs
├── the-effects-element.md      # Audio effects chain
├── the-midi-element.md         # MIDI CC mappings
├── the-modulators-element.md   # LFOs, envelopes
├── the-tags-element.md         # Sample tagging system
├── the-buses-element.md        # Audio routing
├── the-noteSequences-element.md # Step sequencers
├── appendix-*.md               # Reference materials
└── topic-*.md                  # How-to guides
```

**File naming convention:** Use `the-{element-name}-element.md` for element reference docs, `topic-{feature}.md` for tutorials

## Project-Specific Patterns

### XML Documentation Style

When documenting XML elements/attributes:

1. **Use escaped HTML entities in headings:** `&lt;DecentSampler&gt;` not `<DecentSampler>`
   - MyST parser renders these correctly in output
   - Example: `The &lt;ui&gt; element` (see `the-ui-element.md`)

2. **Attribute tables:** Use markdown tables with this structure:
   ```markdown
   | Attribute | Required/Optional | Description | Default |
   ```

3. **Code examples:** Use fenced code blocks with `xml` language tag:
   ````markdown
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <DecentSampler minVersion="1.0.0">
     <!-- Example content -->
   </DecentSampler>
   ```
   ````

4. **Nested Markdown code blocks:** Use 4 backticks for outer fence when showing markdown examples (see `appendix-c-boilerplate-dspreset-file.md`)

### Sphinx Configuration

**Key settings in `docs/source/conf.py`:**
- Uses `myst_parser` extension for Markdown support
- Theme: `sphinx_rtd_theme` (ReadTheDocs style)
- Custom CSS: `_static/css/custom.css`
- Version tracking: Update `release` and `version` when documenting new features

**Dependencies (docs/requirements.txt):**
```
sphinx==7.1.2
sphinx-rtd-theme==1.3.0rc1
myst_parser==2.0.0
```

### Content Organization Philosophy

**Two doc types:**
1. **Element references** (`the-*-element.md`): Comprehensive attribute tables, technical specs
2. **Tutorials** (`topic-*.md`): Practical examples (legato, drum muting, keyswitches, etc.)

**Critical cross-references:**
- `useful-topics.md` aggregates all tutorials with `toctree` directives
- Appendix C (`appendix-c-boilerplate-dspreset-file.md`) provides full working template

## Common Tasks

### Adding New XML Element Documentation

1. Create `docs/source/the-{element}-element.md`
2. Add entry to `docs/source/index.rst` TOC
3. Follow this structure:
   ```markdown
   The &lt;element&gt; element
   ===========================
   
   [Description of purpose]
   
   ## Attributes
   
   | Attribute | Required | Description | Default |
   
   ## Example
   
   ```xml
   [Working code example]
   ```
   ```

### Adding Tutorial/How-To

1. Create `docs/source/topic-{feature}.md`
2. Add to relevant section in `useful-topics.md` using `toctree` directive
3. Include practical code examples and use cases

### Updating for New DecentSampler Version

1. Update `minVersion` examples if new features require specific version
2. Update `docs/source/conf.py` version numbers
3. Document new attributes/elements following established patterns

## ReadTheDocs Integration

Builds triggered by git pushes. Configuration in `.readthedocs.yaml`:
- Python 3.8 environment
- Installs from `docs/requirements.txt`
- Generates HTML, PDF, EPUB formats
- Fails on Sphinx warnings (`fail_on_warning: true`)

## File References

**Never edit build outputs** in `docs/build/` or `docs/_build/` (ignored by git)

**Static assets:** Place images in `docs/source/_static/images/`, CSS in `docs/source/_static/css/`

**Boilerplate reference:** `appendix-c-boilerplate-dspreset-file.md` contains canonical starter template

## Important Notes

- **The DecentSampler format is XML-based**, not a programming language—docs focus on declarative configuration
- **Element hierarchy matters**: Most elements nest under `<DecentSampler>` root
- **Sample library workflow**: Users write `.dspreset` XML → DecentSampler plugin loads it → plays samples
- **Cross-platform**: Documentation serves Mac/Windows/iOS users, so avoid platform-specific assumptions
