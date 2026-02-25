---
name: aria-apg-patterns
description: Accessibility patterns reference based on W3C WAI-ARIA Authoring Practices Guide for implementing accessible web UI components.
---

# ARIA APG Patterns

> Accessibility patterns reference based on W3C WAI-ARIA Authoring Practices Guide
>
> Source: https://www.w3.org/WAI/ARIA/apg/patterns/

This document provides ARIA pattern guidelines for implementing accessible web UI components.

---

## Quick Reference

| Pattern                       | Use Case                        | Required Roles               |
| ----------------------------- | ------------------------------- | ---------------------------- |
| [Accordion](#accordion)       | Expandable/collapsible sections | `button`, `region`           |
| [Alert](#alert)               | Important message (non-modal)   | `alert`                      |
| [Alert Dialog](#alert-dialog) | Important message (modal)       | `alertdialog`                |
| [Breadcrumb](#breadcrumb)     | Hierarchical navigation         | `navigation`                 |
| [Button](#button)             | Action trigger                  | `button`                     |
| [Carousel](#carousel)         | Slide content                   | `region`, `group`            |
| [Checkbox](#checkbox)         | Check selection                 | `checkbox`                   |
| [Combobox](#combobox)         | Autocomplete input              | `combobox`                   |
| [Dialog](#dialog-modal)       | Modal dialog                    | `dialog`                     |
| [Disclosure](#disclosure)     | Show/hide content               | `button`                     |
| [Feed](#feed)                 | Infinite scroll content         | `feed`, `article`            |
| [Grid](#grid)                 | Interactive tabular data        | `grid`, `row`, `gridcell`    |
| [Link](#link)                 | Resource reference              | `link`                       |
| [Listbox](#listbox)           | Option list selection           | `listbox`, `option`          |
| [Menu](#menu-and-menubar)     | Action/function selection       | `menu`, `menuitem`           |
| [Menu Button](#menu-button)   | Menu trigger button             | `button`                     |
| [Meter](#meter)               | Value within range display      | `meter`                      |
| [Radio Group](#radio-group)   | Single selection                | `radiogroup`, `radio`        |
| [Slider](#slider)             | Range value selection           | `slider`                     |
| [Spinbutton](#spinbutton)     | Discrete value input            | `spinbutton`                 |
| [Switch](#switch)             | On/off toggle                   | `switch`                     |
| [Table](#table)               | Static table                    | `table`, `row`, `cell`       |
| [Tabs](#tabs)                 | Tab panel switching             | `tablist`, `tab`, `tabpanel` |
| [Tooltip](#tooltip)           | Supplementary info popup        | `tooltip`                    |
| [Tree View](#tree-view)       | Hierarchical list               | `tree`, `treeitem`           |

---

## Accordion

Vertically stacked set of interactive headings. Each section can be expanded or collapsed.

### Keyboard Interaction

| Key               | Action                                |
| ----------------- | ------------------------------------- |
| `Enter` / `Space` | Toggle panel expand/collapse          |
| `Tab`             | Move to next focusable element        |
| `↓` / `↑`         | (Optional) Move focus between headers |
| `Home` / `End`    | (Optional) Move to first/last header  |

### ARIA Attributes

```html
<!-- Header button -->
<h3>
  <button aria-expanded="true|false" aria-controls="panel-id">Section Title</button>
</h3>

<!-- Panel -->
<div id="panel-id" role="region" aria-labelledby="header-id">Content</div>
```

### Checklist

- [ ] Header contains `button` role
- [ ] Manage `aria-expanded` state
- [ ] Connect panel with `aria-controls`
- [ ] Panel has `role="region"` (when 6 or fewer panels expanded)
- [ ] Panel has `aria-labelledby` referencing header

---

## Alert

Displays important message without interrupting user's current task. **Does not move focus**.

### ARIA Attributes

```html
<div role="alert">Important message content</div>
```

### Checklist

- [ ] Set `role="alert"`
- [ ] Dynamically insert into DOM for screen reader announcement
- [ ] No focus movement (use Alert Dialog if focus needed)
- [ ] Avoid auto-dismiss (WCAG 2.2.3 compliance)
- [ ] Minimize frequent alerts

---

## Alert Dialog

**Modal** dialog that interrupts user to deliver important message.

### Keyboard Interaction

| Key         | Action                    |
| ----------- | ------------------------- |
| `Tab`       | Cycle focus within dialog |
| `Shift+Tab` | Reverse cycle focus       |
| `Escape`    | Close dialog              |

### ARIA Attributes

```html
<div role="alertdialog" aria-modal="true" aria-labelledby="title-id" aria-describedby="desc-id">
  <h2 id="title-id">Title</h2>
  <p id="desc-id">Description</p>
  <button>Confirm</button>
</div>
```

### Checklist

- [ ] Set `role="alertdialog"`
- [ ] Set `aria-modal="true"`
- [ ] Connect title with `aria-labelledby`
- [ ] Move focus to internal element on open
- [ ] Return focus to trigger element on close
- [ ] Implement focus trap

---

## Breadcrumb

Displays parent page links of current page in hierarchical order.

### ARIA Attributes

```html
<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/products">Products</a></li>
    <li><a href="/products/shoes" aria-current="page">Shoes</a></li>
  </ol>
</nav>
```

### Checklist

- [ ] Use `<nav>` or `role="navigation"`
- [ ] Set `aria-label="Breadcrumb"`
- [ ] Set `aria-current="page"` on current page
- [ ] Handle separators via CSS (not read by screen readers)

---

## Button

Widget that triggers actions such as form submission, opening dialogs, etc.

### Button Types

1. **Standard Button**: Default command button
2. **Toggle Button**: Express pressed/unpressed state with `aria-pressed`
3. **Menu Button**: Set `aria-haspopup="menu"`

### Keyboard Interaction

| Key     | Action          |
| ------- | --------------- |
| `Enter` | Activate button |
| `Space` | Activate button |

### ARIA Attributes

```html
<!-- Standard button -->
<button>Save</button>

<!-- Custom button -->
<div role="button" tabindex="0" aria-label="Save">Save</div>

<!-- Toggle button -->
<button aria-pressed="false">Mute</button>

<!-- Disabled button -->
<button aria-disabled="true">Submit</button>
```

### Checklist

- [ ] Prefer native `<button>` element
- [ ] Custom elements: `role="button"` + `tabindex="0"`
- [ ] Handle both `Enter` and `Space`
- [ ] Toggle buttons: manage `aria-pressed` state
- [ ] Disabled: use `aria-disabled="true"` (instead of disabled attribute)
- [ ] Dialog-opening buttons: add ellipsis "Save..." convention

---

## Carousel

Content presentation that displays slides sequentially.

### Keyboard Interaction

| Key         | Action                                        |
| ----------- | --------------------------------------------- |
| `Tab`       | Move between interactive elements in carousel |
| Focus entry | Stop auto-rotation                            |

### ARIA Attributes

```html
<div role="region" aria-roledescription="carousel" aria-label="Featured Products">
  <!-- Rotation controls -->
  <button aria-label="Pause slide rotation">Pause</button>
  <button aria-label="Previous slide">Previous</button>
  <button aria-label="Next slide">Next</button>

  <!-- Slide container -->
  <div aria-live="polite">
    <div role="group" aria-roledescription="slide" aria-label="1 of 5">Slide content</div>
  </div>
</div>
```

### Checklist

- [ ] Container: `aria-roledescription="carousel"`
- [ ] Each slide: `aria-roledescription="slide"`
- [ ] Provide slide position info (e.g., "1 of 5")
- [ ] Auto-rotation requires stop/resume button
- [ ] Stop auto-rotation on focus entry
- [ ] Stop auto-rotation on mouse hover
- [ ] Set `aria-live` (auto-rotation: `off`, manual: `polite`)

---

## Checkbox

Supports checked/unchecked or partial checked states.

### Keyboard Interaction

| Key     | Action             |
| ------- | ------------------ |
| `Space` | Toggle check state |

### ARIA Attributes

```html
<!-- Native checkbox -->
<input type="checkbox" id="agree" />
<label for="agree">I agree</label>

<!-- Custom checkbox -->
<div role="checkbox" tabindex="0" aria-checked="false" aria-labelledby="label-id"></div>
<span id="label-id">I agree</span>

<!-- Tri-state (mixed) -->
<div role="checkbox" aria-checked="mixed"></div>
```

### Checklist

- [ ] Prefer native `<input type="checkbox">`
- [ ] Custom: `role="checkbox"` + `tabindex="0"`
- [ ] Manage `aria-checked` state (`true`, `false`, `mixed`)
- [ ] Handle `Space` key for toggle
- [ ] Group with `role="group"` + `aria-labelledby`

---

## Combobox

Input widget with autocomplete functionality.

### Keyboard Interaction (Popup Closed)

| Key      | Action                           |
| -------- | -------------------------------- |
| `↓`      | Open popup, move to first option |
| `↑`      | Open popup, move to last option  |
| `Escape` | Close popup                      |
| `Enter`  | Accept autocomplete suggestion   |

### Keyboard Interaction (Popup Open)

| Key            | Action                       |
| -------------- | ---------------------------- |
| `↓` / `↑`      | Move between options         |
| `Enter`        | Select option, close popup   |
| `Escape`       | Close popup, return to input |
| `Home` / `End` | Move to first/last option    |

### ARIA Attributes

```html
<label for="city">City</label>
<input
  type="text"
  id="city"
  role="combobox"
  aria-expanded="false"
  aria-controls="city-listbox"
  aria-autocomplete="list"
  aria-activedescendant="option-1"
/>
<ul id="city-listbox" role="listbox">
  <li id="option-1" role="option" aria-selected="true">Seoul</li>
  <li id="option-2" role="option">Busan</li>
</ul>
```

### Checklist

- [ ] Input element: `role="combobox"`
- [ ] Manage `aria-expanded` state
- [ ] Connect popup with `aria-controls`
- [ ] Set `aria-autocomplete` (`none`, `list`, `both`)
- [ ] Reference current option with `aria-activedescendant`
- [ ] Popup: appropriate role (`listbox`, `grid`, `tree`, `dialog`)
- [ ] DOM focus stays on combobox

---

## Dialog (Modal)

Modal window overlaid on main window. Interaction with content outside dialog is blocked.

### Keyboard Interaction

| Key         | Action                                 |
| ----------- | -------------------------------------- |
| `Tab`       | Cycle focus within dialog (last→first) |
| `Shift+Tab` | Reverse cycle (first→last)             |
| `Escape`    | Close dialog                           |

### Initial Focus Placement Rules

1. **Structured content** (lists, tables): Add `tabindex="-1"` to start point
2. **Large content**: Focus on title or first paragraph
3. **Destructive actions** (delete, etc.): Focus on safest button (Cancel)

### ARIA Attributes

```html
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title" aria-describedby="dialog-desc">
  <h2 id="dialog-title">Delete File</h2>
  <p id="dialog-desc">Are you sure you want to delete this file?</p>
  <button>Delete</button>
  <button>Cancel</button>
</div>
```

### Checklist

- [ ] Set `role="dialog"`
- [ ] Set `aria-modal="true"`
- [ ] Connect title with `aria-labelledby`
- [ ] Connect description with `aria-describedby` (optional)
- [ ] Move focus to internal element on open
- [ ] Return focus to trigger element on close
- [ ] **Implement focus trap (required)**
- [ ] Set `aria-hidden="true"` or `inert` on background content

---

## Disclosure

Widget that collapses or expands content.

### Keyboard Interaction

| Key     | Action                   |
| ------- | ------------------------ |
| `Enter` | Toggle content show/hide |
| `Space` | Toggle content show/hide |

### ARIA Attributes

```html
<button aria-expanded="false" aria-controls="content-id">Show more</button>
<div id="content-id" hidden>Hidden content</div>
```

### Checklist

- [ ] Toggle element has `button` role
- [ ] Manage `aria-expanded` state
- [ ] Connect content with `aria-controls` (optional)
- [ ] Icons: right arrow (▶) when collapsed, down arrow (▼) when expanded

---

## Feed

Section that automatically loads new content as user scrolls.

### Keyboard Interaction

| Key         | Action                         |
| ----------- | ------------------------------ |
| `Page Down` | Move focus to next article     |
| `Page Up`   | Move focus to previous article |
| `Ctrl+End`  | Move to element after feed     |
| `Ctrl+Home` | Move to element before feed    |

### ARIA Attributes

```html
<div role="feed" aria-labelledby="feed-title" aria-busy="false">
  <h2 id="feed-title">Latest News</h2>

  <article aria-posinset="1" aria-setsize="-1" aria-labelledby="article-1-title" aria-describedby="article-1-content">
    <h3 id="article-1-title">Article Title</h3>
    <p id="article-1-content">Article content</p>
  </article>
</div>
```

### Checklist

- [ ] Container: `role="feed"`
- [ ] Each article: `role="article"` or `<article>`
- [ ] `aria-posinset` for article position
- [ ] `aria-setsize` for total articles (-1 if unknown)
- [ ] Set `aria-busy="true"` during DOM update
- [ ] **Must set `aria-busy="false"` when complete**

---

## Grid

Interactive tabular data navigable with arrow keys.

### Keyboard Interaction

| Key                      | Action                          |
| ------------------------ | ------------------------------- |
| `→` / `←` / `↑` / `↓`    | Move focus between cells        |
| `Home` / `End`           | Move to first/last cell in row  |
| `Ctrl+Home` / `Ctrl+End` | Move to first/last cell in grid |
| `Page Up` / `Page Down`  | Scroll by row                   |
| `Enter` / `F2`           | Enter cell edit mode            |
| `Escape`                 | Restore grid navigation         |

### ARIA Attributes

```html
<div role="grid" aria-labelledby="grid-title">
  <div role="row">
    <div role="columnheader">Name</div>
    <div role="columnheader">Age</div>
  </div>
  <div role="row">
    <div role="gridcell" tabindex="0">John Doe</div>
    <div role="gridcell">30</div>
  </div>
</div>
```

### Checklist

- [ ] Container: `role="grid"`
- [ ] Rows: `role="row"`
- [ ] Headers: `columnheader` / `rowheader` roles
- [ ] Data cells: `role="gridcell"`
- [ ] Manage `tabindex` on focusable cells
- [ ] `aria-selected` for selection state
- [ ] `aria-sort` for sort state

---

## Link

Interactive reference to a resource.

### Keyboard Interaction

| Key     | Action       |
| ------- | ------------ |
| `Enter` | Execute link |

### ARIA Attributes

```html
<!-- Native link (recommended) -->
<a href="/page">Go to page</a>

<!-- Custom link -->
<span role="link" tabindex="0" onclick="navigate()" onkeydown="handleKeyDown(event)"> Go to page </span>
```

### Checklist

- [ ] **Strongly prefer native `<a>` element**
- [ ] Custom: `role="link"` + `tabindex="0"`
- [ ] Handle `Enter` key (required)
- [ ] Implement same behavior as click

---

## Listbox

List where user can select one or more options.

### Keyboard Interaction (Single Select)

| Key            | Action                                       |
| -------------- | -------------------------------------------- |
| `↓` / `↑`      | Move focus                                   |
| `Home` / `End` | Move to first/last option                    |
| Type character | Search for item starting with that character |

### Keyboard Interaction (Multi Select)

| Key                   | Action                          |
| --------------------- | ------------------------------- |
| `Space`               | Toggle focused option selection |
| `Shift+↓` / `Shift+↑` | Move focus + toggle selection   |
| `Ctrl+A`              | Select/deselect all             |

### ARIA Attributes

```html
<div role="listbox" aria-labelledby="label-id" aria-multiselectable="true">
  <div role="option" aria-selected="true">Option 1</div>
  <div role="option" aria-selected="false">Option 2</div>
</div>
```

### Checklist

- [ ] Container: `role="listbox"`
- [ ] Each option: `role="option"`
- [ ] `aria-selected` for selection state
- [ ] Multi-select: `aria-multiselectable="true"`
- [ ] Horizontal layout: `aria-orientation="horizontal"`

---

## Menu and Menubar

Widget offering list of actions or functions.

### Keyboard Interaction

| Key               | Action                                          |
| ----------------- | ----------------------------------------------- |
| `Enter` / `Space` | Activate item                                   |
| `↓` / `↑`         | Move in vertical menu                           |
| `→` / `←`         | Move in horizontal menubar / Open/close submenu |
| `Escape`          | Close menu                                      |
| `Home` / `End`    | Move to first/last item                         |
| Type character    | Move to matching label item                     |

### ARIA Attributes

```html
<div role="menubar" aria-label="Edit">
  <div role="menuitem" aria-haspopup="menu" aria-expanded="false">
    File
    <div role="menu">
      <div role="menuitem">New File</div>
      <div role="menuitem">Open</div>
      <div role="menuitemcheckbox" aria-checked="true">Auto Save</div>
    </div>
  </div>
</div>
```

### Checklist

- [ ] Container: `role="menu"` or `role="menubar"`
- [ ] Items: `menuitem` / `menuitemcheckbox` / `menuitemradio` roles
- [ ] Submenu presence: `aria-haspopup="menu"`
- [ ] Manage submenu open state with `aria-expanded`
- [ ] Checkbox/radio items: manage `aria-checked`
- [ ] Disabled items: `aria-disabled="true"`

---

## Menu Button

Button that opens a menu.

### Keyboard Interaction

| Key               | Action                                 |
| ----------------- | -------------------------------------- |
| `Enter` / `Space` | Open menu, focus first item            |
| `↓`               | (Optional) Open menu, focus first item |
| `↑`               | (Optional) Open menu, focus last item  |

### ARIA Attributes

```html
<button aria-haspopup="menu" aria-expanded="false" aria-controls="menu-id">Options</button>
<div id="menu-id" role="menu">
  <div role="menuitem">Item 1</div>
  <div role="menuitem">Item 2</div>
</div>
```

### Checklist

- [ ] Button: `aria-haspopup="menu"` or `"true"`
- [ ] Manage `aria-expanded` state
- [ ] Connect menu with `aria-controls` (optional)
- [ ] Menu: `role="menu"`

---

## Meter

Graphical display of numeric value within a defined range.

> Warning: Use `progressbar` role for progress indication

### ARIA Attributes

```html
<div role="meter" aria-valuenow="75" aria-valuemin="0" aria-valuemax="100" aria-label="Battery level">75%</div>

<!-- With additional description -->
<div
  role="meter"
  aria-valuenow="50"
  aria-valuemin="0"
  aria-valuemax="100"
  aria-valuetext="50% (6 hours) remaining"
  aria-label="Battery"
></div>
```

### Checklist

- [ ] Set `role="meter"`
- [ ] `aria-valuenow` current value
- [ ] `aria-valuemin` minimum value
- [ ] `aria-valuemax` maximum value
- [ ] `aria-valuetext` user-friendly value (optional)
- [ ] `aria-label` or `aria-labelledby` for label

---

## Radio Group

Set of radio buttons where only one can be selected at a time.

### Keyboard Interaction

| Key       | Action                                        |
| --------- | --------------------------------------------- |
| `Tab`     | Enter group (selected button or first button) |
| `Space`   | Select current focused button                 |
| `→` / `↓` | Move to next button + select (cycles)         |
| `←` / `↑` | Move to previous button + select (cycles)     |

### ARIA Attributes

```html
<div role="radiogroup" aria-labelledby="group-label">
  <span id="group-label">Shipping Method</span>

  <div role="radio" aria-checked="true" tabindex="0">Standard Shipping</div>
  <div role="radio" aria-checked="false" tabindex="-1">Express Shipping</div>
</div>
```

### Checklist

- [ ] Container: `role="radiogroup"`
- [ ] Each button: `role="radio"`
- [ ] Manage `aria-checked` state
- [ ] Selected button: `tabindex="0"`, others: `tabindex="-1"`
- [ ] Arrow key selection moves focus too

---

## Slider

Input widget for selecting value within a given range.

### Keyboard Interaction

| Key         | Action                            |
| ----------- | --------------------------------- |
| `→` / `↑`   | Increase value                    |
| `←` / `↓`   | Decrease value                    |
| `Home`      | Set to minimum                    |
| `End`       | Set to maximum                    |
| `Page Up`   | (Optional) Increase by large step |
| `Page Down` | (Optional) Decrease by large step |

### ARIA Attributes

```html
<div role="slider" tabindex="0" aria-valuenow="50" aria-valuemin="0" aria-valuemax="100" aria-label="Volume"></div>

<!-- With user-friendly value -->
<div
  role="slider"
  aria-valuenow="1"
  aria-valuemin="0"
  aria-valuemax="6"
  aria-valuetext="Monday"
  aria-label="Day selection"
></div>
```

### Checklist

- [ ] Set `role="slider"`
- [ ] Set `tabindex="0"`
- [ ] `aria-valuenow` current value
- [ ] `aria-valuemin` minimum value
- [ ] `aria-valuemax` maximum value
- [ ] `aria-valuetext` user-friendly value (optional)
- [ ] Vertical slider: `aria-orientation="vertical"`

---

## Spinbutton

Input widget for selecting value from discrete value range.

### Keyboard Interaction

| Key         | Action                            |
| ----------- | --------------------------------- |
| `↑`         | Increase value                    |
| `↓`         | Decrease value                    |
| `Home`      | Set to minimum                    |
| `End`       | Set to maximum                    |
| `Page Up`   | (Optional) Increase by large step |
| `Page Down` | (Optional) Decrease by large step |

### ARIA Attributes

```html
<div role="spinbutton" tabindex="0" aria-valuenow="5" aria-valuemin="0" aria-valuemax="59" aria-label="Minutes">
  <button aria-label="Increase">+</button>
  <span>05</span>
  <button aria-label="Decrease">-</button>
</div>
```

### Checklist

- [ ] Set `role="spinbutton"`
- [ ] `aria-valuenow` current value
- [ ] `aria-valuemin` minimum value
- [ ] `aria-valuemax` maximum value
- [ ] `aria-valuetext` user-friendly value (optional)
- [ ] Out of range: `aria-invalid="true"`

---

## Switch

Widget for selecting between on/off states.

### Keyboard Interaction

| Key     | Action                  |
| ------- | ----------------------- |
| `Space` | Toggle state            |
| `Enter` | (Optional) Toggle state |

### ARIA Attributes

```html
<button role="switch" aria-checked="false">Notifications</button>

<!-- HTML checkbox based -->
<input type="checkbox" role="switch" aria-checked="true" />
```

### Checklist

- [ ] Set `role="switch"`
- [ ] Manage `aria-checked` state (`true` / `false`)
- [ ] **Label should NOT change on state change**
- [ ] Group with `role="group"` + `aria-labelledby`

---

## Table

Static table structure (no interactive features).

> For interactive tables, use [Grid](#grid) pattern

### ARIA Attributes

```html
<table role="table" aria-labelledby="table-title">
  <caption id="table-title">
    User List
  </caption>
  <thead>
    <tr role="row">
      <th role="columnheader" aria-sort="ascending">Name</th>
      <th role="columnheader">Email</th>
    </tr>
  </thead>
  <tbody>
    <tr role="row">
      <td role="cell">John Doe</td>
      <td role="cell">john@example.com</td>
    </tr>
  </tbody>
</table>
```

### Checklist

- [ ] **Prefer native `<table>` element**
- [ ] Custom: `role="table"` + `row` + `cell` structure
- [ ] Headers: `columnheader` / `rowheader` roles
- [ ] `aria-label` or `aria-labelledby` for table label
- [ ] Sorted columns: set `aria-sort`

---

## Tabs

Layered content sections displaying one panel at a time.

### Keyboard Interaction

| Key               | Action                             |
| ----------------- | ---------------------------------- |
| `Tab`             | Enter tab list (active tab)        |
| `→` / `←`         | Move to next/previous tab (cycles) |
| `Space` / `Enter` | (Manual activation) Activate tab   |
| `Home` / `End`    | Move to first/last tab             |
| `Delete`          | (Optional) Delete tab              |

### ARIA Attributes

```html
<div role="tablist" aria-label="Content tabs">
  <button role="tab" aria-selected="true" aria-controls="panel-1" tabindex="0">Tab 1</button>
  <button role="tab" aria-selected="false" aria-controls="panel-2" tabindex="-1">Tab 2</button>
</div>

<div id="panel-1" role="tabpanel" aria-labelledby="tab-1" tabindex="0">Panel 1 content</div>
<div id="panel-2" role="tabpanel" aria-labelledby="tab-2" hidden>Panel 2 content</div>
```

### Checklist

- [ ] Tab container: `role="tablist"`
- [ ] Each tab: `role="tab"`
- [ ] Content area: `role="tabpanel"`
- [ ] Manage `aria-selected` state
- [ ] Connect tab-panel with `aria-controls`
- [ ] Active tab: `tabindex="0"`, others: `tabindex="-1"`
- [ ] Vertical tabs: `aria-orientation="vertical"`

---

## Tooltip

Popup displaying information when element receives focus or hover.

> Warning: This pattern is currently under W3C development

### Keyboard Interaction

| Key      | Action        |
| -------- | ------------- |
| `Escape` | Close tooltip |

### ARIA Attributes

```html
<button aria-describedby="tooltip-1">Save</button>
<div id="tooltip-1" role="tooltip">Save your changes (Ctrl+S)</div>
```

### Checklist

- [ ] Tooltip: `role="tooltip"`
- [ ] Trigger element: `aria-describedby` referencing tooltip
- [ ] Focus stays on trigger element
- [ ] Display after slight delay
- [ ] Close with `Escape` key
- [ ] Auto-close on focus loss
- [ ] Stay open while cursor on trigger/tooltip

---

## Tree View

Widget presenting hierarchical list.

### Keyboard Interaction

| Key            | Action                                    |
| -------------- | ----------------------------------------- |
| `Enter`        | Activate node (default action)            |
| `→`            | Open closed node / Move to child          |
| `←`            | Close open node / Move to parent          |
| `↓` / `↑`      | Move to next/previous focusable node      |
| `Home` / `End` | Move to first/last node                   |
| Type character | Move to node starting with that character |

### Keyboard Interaction (Multi Select)

| Key                   | Action                        |
| --------------------- | ----------------------------- |
| `Space`               | Toggle focused node selection |
| `Shift+↓` / `Shift+↑` | Toggle selection while moving |
| `Ctrl+A`              | Select/deselect all           |

### ARIA Attributes

```html
<ul role="tree" aria-labelledby="tree-label" aria-multiselectable="false">
  <li role="treeitem" aria-expanded="true" aria-selected="false">
    Folder 1
    <ul role="group">
      <li role="treeitem" aria-selected="true">File 1</li>
      <li role="treeitem" aria-selected="false">File 2</li>
    </ul>
  </li>
</ul>
```

### Checklist

- [ ] Container: `role="tree"`
- [ ] Each node: `role="treeitem"`
- [ ] Child container: `role="group"`
- [ ] Manage `aria-expanded` state (open/close)
- [ ] `aria-selected` for selection state
- [ ] Multi-select: `aria-multiselectable="true"`
- [ ] `aria-level`, `aria-setsize`, `aria-posinset` for hierarchy info
- [ ] Visually distinguish focus and selection states

---

## Common Patterns

### Focus Management

```typescript
// Focus trap (for modal dialogs)
function trapFocus(element: HTMLElement) {
  const focusableElements = element.querySelectorAll(
    'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
  );
  const first = focusableElements[0] as HTMLElement;
  const last = focusableElements[focusableElements.length - 1] as HTMLElement;

  element.addEventListener('keydown', (e) => {
    if (e.key !== 'Tab') return;

    if (e.shiftKey && document.activeElement === first) {
      e.preventDefault();
      last.focus();
    } else if (!e.shiftKey && document.activeElement === last) {
      e.preventDefault();
      first.focus();
    }
  });
}
```

### Roving Tabindex

Used in tab lists, radio groups, etc.:

```typescript
// Only active element has tabindex="0", others have tabindex="-1"
function setRovingTabindex(container: HTMLElement, activeSelector: string) {
  const items = container.querySelectorAll('[role="tab"], [role="radio"]');
  items.forEach((item) => {
    (item as HTMLElement).tabIndex = item.matches(activeSelector) ? 0 : -1;
  });
}
```

### Live Regions

Announce dynamic content changes:

```html
<!-- Immediate announcement (urgent) -->
<div aria-live="assertive" aria-atomic="true">Error: This field is required.</div>

<!-- Next opportunity announcement (normal) -->
<div aria-live="polite">3 search results found</div>
```

---

## References

- [WAI-ARIA Authoring Practices 1.2](https://www.w3.org/WAI/ARIA/apg/)
- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [MDN ARIA Documentation](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)
