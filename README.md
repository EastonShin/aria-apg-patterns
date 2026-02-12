# ARIA APG Patterns

> WAI-ARIA Authoring Practices Guide patterns for Claude Code

[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://claude.ai/code)
[![W3C](https://img.shields.io/badge/W3C-ARIA%20APG-green)](https://www.w3.org/WAI/ARIA/apg/patterns/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A comprehensive ARIA pattern reference for building accessible web components. This skill provides Claude Code with detailed implementation guidelines based on W3C WAI-ARIA Authoring Practices Guide.

## Installation

### Project-level (recommended)

```bash
# Add to current project
mkdir -p .claude/commands
curl -o .claude/commands/aria-apg-patterns.md \
  https://raw.githubusercontent.com/EastonShin/aria-apg-patterns/main/SKILL.md
```

### Global (all projects)

```bash
# Add to all Claude Code projects
mkdir -p ~/.claude/commands
curl -o ~/.claude/commands/aria-apg-patterns.md \
  https://raw.githubusercontent.com/EastonShin/aria-apg-patterns/main/SKILL.md
```

## Usage

Ask Claude directly:

```text
"Create an accessible tabs component following ARIA patterns"
"How do I implement keyboard navigation for a combobox?"
"What ARIA attributes do I need for a modal dialog?"
```

## What's Included

### 26 ARIA Patterns

| Category       | Patterns                                                                      |
| -------------- | ----------------------------------------------------------------------------- |
| **Navigation** | Breadcrumb, Link, Menu, Menubar, Tabs, Tree View                              |
| **Input**      | Button, Checkbox, Combobox, Listbox, Radio Group, Slider, Spinbutton, Switch  |
| **Feedback**   | Alert, Alert Dialog, Dialog, Meter, Tooltip                                   |
| **Layout**     | Accordion, Carousel, Disclosure, Feed, Grid, Table                            |

### Each Pattern Contains

- **Keyboard Interaction** - Complete key mappings for navigation and activation
- **ARIA Attributes** - Required roles, states, and properties with code examples
- **Checklist** - Implementation verification points

### Common Utilities

- Focus trap implementation
- Roving tabindex pattern
- Live regions for announcements

## Example

When you ask Claude to create an accessible dialog:

```tsx
// Claude will generate code following the Dialog pattern:
<div
  role="dialog"
  aria-modal="true"
  aria-labelledby="dialog-title"
  aria-describedby="dialog-desc"
>
  <h2 id="dialog-title">Confirm Action</h2>
  <p id="dialog-desc">Are you sure you want to proceed?</p>
  <button>Confirm</button>
  <button>Cancel</button>
</div>
```

With proper:

- Focus trap on open/close
- `Escape` key to close
- Focus return to trigger element

## Why Use This Skill?

| Without                     | With aria-apg-patterns   |
| --------------------------- | ------------------------ |
| Generic ARIA attributes     | W3C-compliant patterns   |
| Missing keyboard support    | Complete key mappings    |
| Inconsistent implementation | Standardized approach    |
| Manual research needed      | Instant reference        |

## Comparison with Other Skills

| Feature | aria-apg-patterns | [web-interface-guidelines](https://github.com/vercel-labs/web-interface-guidelines) |
| --- | --- | --- |
| **Focus** | ARIA accessibility patterns | General UI/UX best practices |
| **Scope** | 26 component patterns in depth | Broad coverage (a11y, animation, forms) |
| **Keyboard specs** | Complete key mappings per pattern | General guidelines |
| **Code examples** | HTML/ARIA snippets for each pattern | Conceptual rules |
| **Use case** | Building accessible components | Code review & quality checks |

**Recommendation**: Use both for comprehensive coverage.

## Source

Based on [W3C WAI-ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/patterns/)

## Contributing

1. Fork the repository
2. Edit `SKILL.md`
3. Submit a pull request

## License

MIT License - See [LICENSE](LICENSE) for details.

---

**Keywords**: accessibility, a11y, aria, wai-aria, wcag, web accessibility, claude code, skill, patterns, keyboard navigation, screen reader
