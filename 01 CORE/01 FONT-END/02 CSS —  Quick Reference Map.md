---
title: "CSS 3 - Quick Reference Map"
type: doc-post
framework: CSS
tags:
  - css
  - frontend
  - styling
  - cheatsheet
  - quick-reference
created: 2025-12-26
---
## 🔹 1.1 CSS Fundamentals & Setup

- **Syntax & Rules** : selectors `{ property: value; }`
    
- **Selectors**
    
    - Type, Class, ID
        
    - Attribute `[attr=value]`
        
    - Pseudo-classes `:hover`, `:focus`, `:nth-child()`
        
    - Pseudo-elements `::before`, `::after`
        
- **Combinators** : descendant , child `>`, adjacent `+`, sibling `~`
    
- **Including CSS** : inline, internal `<style>`, external `.css`
    
- **Linking Stylesheets** : `<link rel="stylesheet">`
    
- **Cascade & Inheritance** : origin, order, inherited props
    
- **Specificity** : inline > ID > class > type
    
- **Importance** : `!important`
    
- **Source Order** : later wins
    
- **Comments** : `/* ... */`
    
- **Units** : `px`, `em`, `rem`, `%`, `vw/vh`, `fr`
    
- **Values & Data Types** : lengths, colors, keywords, functions
    
- **DevTools** : inspect, computed, box model
    
- **Reset / Normalize** : reset.css, normalize.css
    



## 🔹 1.2 Box Model & Layout Basics

- **Box Model** : content → padding → border → margin
    
- **Box Sizing** : `content-box`, `border-box`
    
- **Margin Collapse** : vertical margins merge
    
- **Display** : `block`, `inline`, `inline-block`, `none`
    
- **Positioning** : `static`, `relative`, `absolute`, `fixed`, `sticky`
    
- **Offsets & Stack** : `top/right/bottom/left`, `z-index`
    
- **Floats** : `float`, `clear`, clearfix
    
- **Overflow** : `visible`, `hidden`, `scroll`, `auto`
    
- **Sizing** : `width/height`, `min-*`, `max-*`
    



## 🔹 1.3 Typography & Text Styling

- **Font Properties** : `font-family`, `font-size`, `font-weight`, `font-style`, `font-variant`
    
- **Web Fonts** : `@font-face`, Google Fonts
    
- **Line & Spacing** : `line-height`, `letter-spacing`, `word-spacing`
    
- **Text Alignment** : `text-align`, `vertical-align`
    
- **Text Transform** : `uppercase`, `lowercase`, `capitalize`
    
- **Decorations** : `text-decoration`, `text-shadow`
    
- **Indentation** : `text-indent`
    
- **Color & Opacity** : `color`, `opacity`
    
- **Whitespace** : `white-space`, `text-wrap`
    
- **Lists** : `list-style-type`, `position`, `image`
    
- **Quotes & Counters** : `quotes`, `counter-*`
    



## 🔹 1.4 Colors, Backgrounds & Borders

- **Color Models** : named, `#hex`, `rgb/a`, `hsl/a`
    
- **Backgrounds**
    
    - `background-color`
        
    - `background-image`
        
    - `repeat`, `attachment`, `position`, `size`
        
    - `clip`, `origin`
        
- **Gradients** : `linear`, `radial`, `conic`
    
- **Borders** : `border-width`, `style`, `color`, `radius`, `image`
    
- **Outline** : `outline-*`
    
- **Shadows** : `box-shadow`
    
- **Opacity** : alpha channels, `opacity`
    



## 🔹 1.5 Flexbox Layout

- **Flex Container** : `display: flex | inline-flex`
    
- **Direction & Wrap** : `flex-direction`, `flex-wrap`, `flex-flow`
    
- **Main Axis Align** : `justify-content`
    
- **Cross Axis Align** : `align-items`, `align-content`
    
- **Flex Items**
    
    - `flex-grow`
        
    - `flex-shrink`
        
    - `flex-basis`
        
    - shorthand `flex`
        
- **Item Control** : `order`, `align-self`
    
- **Responsive Flex** : wrapping, media queries
    
- **Patterns** : centering, navbars, holy grail
    



## 🔹 1.6 Grid Layout

- **Grid Container** : `display: grid | inline-grid`
    
- **Templates** : `grid-template-columns/rows/areas`
    
- **Gaps** : `row-gap`, `column-gap`, `gap`
    
- **Alignment** : `justify-items`, `align-items`, `place-items`
    
- **Content Align** : `justify-content`, `align-content`
    
- **Auto Placement** : `grid-auto-flow`, `dense`
    
- **Tracks** : explicit vs implicit
    
- **Functions** : `minmax()`, `repeat()`, `auto-fit`, `auto-fill`
    
- **Subgrid** : emerging support
    
- **Responsive Grids** : fluid tracks
    
- **Patterns** : magazine, dashboard
    



## 🔹 1.7 Responsive Design & Media Queries

- **Strategies** : mobile-first, desktop-first
    
- **Viewport** : `<meta name="viewport">`
    
- **Breakpoints** : fluid vs fixed
    
- **Media Queries** : `@media screen`, `print`, `(min/max-width)`
    
- **Logical Properties** : `margin-inline`, `padding-block`, `inset-*`
    
- **Container Queries** : `@container`
    
- **Responsive Images** : `srcset`, `sizes`, `<picture>`
    
- **Feature Queries** : `@supports`
    



## 🔹 1.8 Transitions, Animations & Transforms

- **Transitions** : `property`, `duration`, `timing-function`, `delay`
    
- **Transforms 2D/3D** : `translate`, `scale`, `rotate`, `skew`
    
- **3D Control** : `perspective`, `backface-visibility`
    
- **Keyframes** : `@keyframes`
    
- **Animation Props** : `name`, `duration`, `timing`, `delay`, `iteration-count`, `direction`, `fill-mode`
    
- **Play State** : `animation-play-state`
    
- **Performance** : compositor-friendly props
    



## 🔹 1.9 Advanced Selectors, Pseudo-elements & Functions

- **Advanced Selectors** : `:nth-child()`, `:nth-of-type()`, `:has()`, `:is()`, `:where()`
    
- **Pseudo-elements** : `::before`, `::after`, `::first-line`, `::first-letter`
    
- **Custom Properties** : `--var`, `var()`
    
- **Math Functions** : `calc()`, `clamp()`, `min()`, `max()`
    
- **Value Functions** : `attr()`, `counter()`, `env()`
    
- **Color Functions** : modern color funcs
    
- **Filters** : `filter`, `backdrop-filter`
    
- **Clipping & Masking** : `clip-path`, `mask`
    



## 🔹 1.10 Modern & Emerging Features

- **Cascade Layers** : `@layer`
    
- **Nesting** : native CSS nesting (emerging)
    
- **Modern Color Spaces** : `oklch()`, `lab()`
    
- **Scroll Behavior** : scroll snap, `scroll-behavior: smooth`
    
- **Aspect Ratio** : `aspect-ratio`
    
- **Form Accent** : `accent-color`
    
- **View Transitions** : page/state transitions
    
- **Masonry Layout** : emerging grids
    
- **Subgrid & Containers** : deep layouts
    
- **Accessibility Media** : `prefers-reduced-motion`, `color-scheme`
    



If you want, next I can:

- Add **`[[Wiki Links]]`** for each major section (e.g., `[[Flexbox Layout]]`).
    
- Break this into **linked sub-notes** per chapter.
    
- Or generate a **one-page CSS cheatsheet** distilled from this map.