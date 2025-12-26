---
title: "Bootstrap 5 - Quick Reference Map"
type: doc-post
framework: Bootstrap
tags:
  - bootstrap
  - frontend
  - css
  - cheatsheet
  - quick-reference
created: 2025-12-26
---


## 🔹 1.1 Framework Philosophy & Setup

- **Philosophy & Use Cases** : mobile-first, when-to-use, Bootstrap vs custom CSS
    
- **Reboot & Normalization** : cross-browser reset, opinionated defaults
    
- **Installation**
    
    - **CDN** : `bootstrap.min.css`, `bootstrap.bundle.min.js`, `@popperjs/core`
        
    - **Package Managers** : npm, yarn, pnpm
        
    - **Sources** : `scss/`, `dist/` (compiled)
        
- **File Structure** : `dist/`, `scss/`, `js/`
    
- **Customization Entry Points** : Sass variables, maps, Utility API
    
- **Theming & Extending** : custom themes, overrides
    
- **Browser Support** : modern browsers, no IE
    
- **Polyfills & Fallbacks** : minimal, graceful degradation
    
- **JavaScript Dependencies** : Popper.js, bundle vs separate
    
- **Builds** : ESM modules, UMD
    
- **RTL Support** : `rtl.css`, logical properties
    



## 🔹 [[1.2 Grid System & Responsive Layouts]]

- **Containers** : `.container`, `.container-fluid`, `.container-{bp}`
    
- **Breakpoints** : `xs`, `sm`, `md`, `lg`, `xl`, `xxl`
    
- **Rows** : `.row`
    
- **Columns** : `.col`, `.col-auto`, `.col-*`, `.col-{bp}-*`
    
- **12-Column Grid** : auto layout, fixed spans
    
- **Gutters** : `.g-*`, `.gx-*`, `.gy-*`, `.g-0`
    
- **Ordering** : `.order-*`, `.order-{bp}-*`
    
- **Offsets** : `.offset-*`, `.offset-{bp}-*`
    
- **Row Columns** : `.row-cols-*`, `.row-cols-auto`
    
- **Vertical Alignment** : `.align-items-*`, `.align-self-*`, `.align-content-*`
    
- **Horizontal Alignment** : `.justify-content-*`, `.mx-auto`
    
- **Nesting** : `.row` → `.col-*` inside columns
    
- **Flex Integration** : grid + flex utilities
    



## 🔹 1.3 Typography & Content Styling

- **Headings** : `h1–h6`, `.display-1…6`
    
- **Paragraphs** : `<p>`, `.lead`
    
- **Font Size** : `.fs-1…6`
    
- **Font Weight** : `.fw-light`, `.fw-normal`, `.fw-bold`
    
- **Line Height** : `.lh-1`, `.lh-sm`, `.lh-base`, `.lh-lg`
    
- **Inline Text** : `<strong>`, `<em>`, `<u>`, `<s>`, `<mark>`, `<small>`, `<abbr>`
    
- **Text Alignment** : `.text-start`, `.text-center`, `.text-end`, `.text-{bp}-*`
    
- **Text Transform** : `.text-lowercase`, `.text-uppercase`, `.text-capitalize`
    
- **Lists** : `<ul>`, `<ol>`, `.list-unstyled`, `.list-inline`
    
- **Description Lists** : `<dl>`, `<dt>`, `<dd>`
    
- **List Groups** : `.list-group`, `.list-group-item`
    
- **Blockquotes** : `.blockquote`, `.blockquote-footer`
    
- **Figures** : `<figure>`, `<figcaption>`
    
- **Code** : `<code>`, `<pre>`
    
- **Tables** : `.table`, `.table-striped`, `.table-hover`, `.table-bordered`, `.table-responsive`
    



## 🔹 1.4 Color System & Utilities

- **Theme Colors** : `primary`, `secondary`, `success`, `info`, `warning`, `danger`, `light`, `dark`
    
- **Backgrounds** : `.bg-*`, `.bg-gradient`, `.bg-opacity-*`
    
- **Text Colors** : `.text-*`, `.text-opacity-*`
    
- **Combined** : `.text-bg-*`
    
- **Links** : `.link-*`, contextual links
    
- **Borders** : `.border`, `.border-*`, `.border-{side}-*`, width, style
    
- **Opacity** : `.opacity-*`
    
- **Accessibility** : WCAG contrast, accessible pairs
    
- **Color Modes** : light, dark
    



## 🔹 1.5 Spacing & Sizing Utilities

- **Margin** : `.m-*`, `.mx-*`, `.my-*`, `.ms-*`, `.me-*`, `.mt-*`, `.mb-*`, `.m-*-n`
    
- **Padding** : `.p-*`, `.px-*`, `.py-*`, `.ps-*`, `.pe-*`, `.pt-*`, `.pb-*`
    
- **Scale** : `0–5`, `auto`
    
- **Responsive Variants** : `{bp}-*` prefixes
    
- **Auto Centering** : `.mx-auto`
    
- **Width** : `.w-*`, `.mw-100`, `.vw-100`
    
- **Height** : `.h-*`, `.mh-100`, `.vh-100`
    
- **Min/Max** : `min-vw-100`, `min-vh-100`
    
- **Gaps** : `.gap-*`, `.row-gap-*`, `.column-gap-*`
    
- **Gap vs Gutter** : flex/grid gap vs grid gutters
    



## 🔹 1.6 Components — Structure & Behavior

- **Navigation** : `.navbar*`, `.nav`, `.nav-tabs`, `.breadcrumb`, `.pagination`
    
- **Content Containers** : `.card`, `.accordion`, `.list-group`, jumbotron alternatives
    
- **Forms** : `.form-control`, `.input-group`, `.form-check`, `.form-range`, `.form-floating`, `.is-valid/.is-invalid`
    
- **Feedback** : `.alert`, `.toast`, `.progress`, `.spinner-*`
    
- **Interactive**
    
    - **Buttons** : `.btn`, `.btn-*`, `.btn-group`
        
    - **Dropdown** : `.dropdown*`
        
    - **Collapse** : `.collapse`
        
    - **Carousel** : `.carousel*`
        
    - **Modal** : `.modal*`
        
    - **Offcanvas** : `.offcanvas*`
        
    - **Tooltip** : `.tooltip`
        
    - **Popover** : `.popover`
        
    - **Scrollspy** : `data-bs-spy`
        
- **Visual** : `.badge`, `.rounded-pill`, `.btn-close`, `.placeholder*`
    
- **Variants** : colors, sizes, outlines
    
- **Responsiveness** : responsive behaviors
    
- **Accessibility** : ARIA roles, keyboard support
    
- **JavaScript APIs** : `data-bs-*`, `new bootstrap.*()`
    



## 🔹 1.7 Utility Classes & Helpers

- **Display** : `.d-*`, `.d-{bp}-*`
    
- **Flexbox** : `.d-flex`, `.flex-*`, `.flex-{dir}`, `.flex-wrap`, `.flex-grow-*`, `.flex-shrink-*`
    
- **Float** : `.float-start`, `.float-end`, `.float-{bp}-*`
    
- **Position** : `.position-*`, `.top-0`, `.bottom-0`, `.start-0`, `.end-0`, `.translate-middle`, `.z-*`
    
- **Borders & Radius** : `.border*`, `.rounded*`, `.shadow*`
    
- **Visibility** : `.visible`, `.invisible`, `.opacity-*`
    
- **Overflow** : `.overflow-*`
    
- **Interaction** : `.user-select-*`, `.pe-none`, `.pointer-events-*`, `.cursor-*`
    
- **Text Helpers** : `.text-truncate`, `.text-break`, `.font-monospace`
    
- **Vertical Align** : `.align-baseline`, `.align-top`, `.align-middle`, `.align-bottom`
    
- **Ratio / Embed** : `.ratio-*`
    
- **Flow Helpers** : clearfix alternatives, `.vstack`, `.hstack`