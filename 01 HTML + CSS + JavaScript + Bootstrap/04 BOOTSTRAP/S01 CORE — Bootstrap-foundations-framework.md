#### **1.1 Framework Philosophy & Setup**

**1.1.1 Framework Philosophy & Use Cases —** Purpose of Bootstrap as a component-based framework; when to choose Bootstrap over custom CSS; rapid prototyping versus production systems.

**1.1.2 Mobile-First Design Strategy —** Designing from small screens upward; responsive breakpoints; progressive enhancement as the core layout philosophy.

**1.1.3 Bootstrap vs Custom CSS —** Trade-offs between speed and control; consistency vs flexibility; criteria for extending or overriding Bootstrap styles.

**1.1.4 Installation via CDN —** Fast setup using hosted CSS/JS; ideal for demos and prototypes; zero build tooling requirement.

**1.1.5 Installation via npm/yarn —** Package-based setup for modern workflows; version control; integration with bundlers and build tools.

**1.1.6 Sass Source Compilation —** Using Bootstrap’s Sass for deep customization; variable overrides; building tailored CSS bundles.

**1.1.7 Compiled vs Source Assets —** Choosing prebuilt files for simplicity versus source files for customization and optimization.

**1.1.8 File Structure & Architecture —** Understanding CSS, JS, and Sass directories; compiled vs source layouts; locating customization entry points.

**1.1.9 Customization Entry Points —** Overriding variables, maps, and utilities; selective imports; building brand-specific themes.

**1.1.10 Browser Support Scope —** Supported browsers and versions; alignment with evergreen browser policies; constraints of legacy support.

**1.1.11 Polyfills & Fallbacks —** Handling missing features in older browsers; JS and CSS fallbacks; ensuring baseline functionality.

**1.1.12 Cross-Browser Testing —** Testing layouts and components across browsers and devices; tools and strategies for consistency.

**1.1.13 Graceful Degradation —** Ensuring usable experiences when features are unsupported; prioritizing core content and interactions.

**1.1.14 JavaScript Dependencies Overview —** Role of Bootstrap’s JS layer; dependency management; when JS is required for components.

**1.1.15 Popper.js Integration —** Positioning engine for tooltips, popovers, and dropdowns; why it is required and how it is bundled.

**1.1.16 Bundle vs Separate JS Files —** Using `bootstrap.bundle.js` for simplicity versus modular imports for optimized builds.

**1.1.17 Module vs UMD Builds —** ES module builds for modern bundlers; UMD for legacy/global usage; choosing based on environment.

---
#### **1.2 Grid System & Responsive Layouts**

**1.2.1 Containers & Layout Boundaries —** Using `.container`, `.container-fluid`, and responsive variants; controlling max-widths and horizontal padding to define layout limits.

**1.2.2 Breakpoints & Grid Tiers —** xs–xxl breakpoints; mobile-first tier system; applying responsive classes to scale layouts across devices.

**1.2.3 12-Column Grid System —** Core grid model based on 12 columns; building rows and columns; understanding column math and spans.

**1.2.4 Auto & Fixed Columns —** Auto-layout columns that share space versus fixed-width `.col-*` spans for precise control.

**1.2.5 Responsive Column Spans —** Using `.col-{breakpoint}-*` to change column widths at different screen sizes.

**1.2.6 Gutters & Spacing Control —** Managing inter-column gaps with `.g-*`, `.gx-*`, `.gy-*`, and removing gutters with `.g-0`.

**1.2.7 Column Ordering —** Reordering visual flow with `.order-*` utilities without changing source HTML.

**1.2.8 Column Offsetting —** Creating horizontal offsets using `.offset-*` to shift columns within a row.

**1.2.9 Row Columns Utility —** Defining equal-width layouts with `.row-cols-*` for quick grid patterns.

**1.2.10 Nested Grids —** Placing rows inside columns to build complex, multi-level layouts.

**1.2.11 Vertical Alignment —** Aligning columns vertically using `.align-items-*` and `.align-self-*` within flex-based rows.

**1.2.12 Horizontal Distribution —** Controlling horizontal spacing with `.justify-content-*` utilities.

**1.2.13 Centering Techniques —** Centering columns horizontally and vertically using grid and flex utilities.

**1.2.14 Flex Integration with Grid —** Leveraging flexbox behavior built into `.row` and `.col` for advanced alignment and layout control.

---
#### **1.3 Typography & Content Styling**

**1.3.1 Headings & Display Text —** Using standard headings and `.display-*` classes to establish visual hierarchy and emphasis.

**1.3.2 Typographic Hierarchy —** Structuring content with consistent font sizes, weights, and spacing for readability.

**1.3.3 Paragraphs & Lead Text —** Styling body text with paragraphs and highlighting key intros using `.lead`.

**1.3.4 Font Size & Line Height —** Managing readability through responsive font sizing and line-height utilities.

**1.3.5 Inline Text Utilities —** Applying bold, italic, underline, strikethrough, mark, and small text styles inline.

**1.3.6 Semantic vs Presentational Styling —** Choosing semantic HTML elements versus purely visual utility classes.

**1.3.7 Text Alignment —** Aligning text with `.text-start`, `.text-center`, `.text-end`, including responsive variants.

**1.3.8 Text Transformation —** Controlling casing with `.text-lowercase`, `.text-uppercase`, and `.text-capitalize`.

**1.3.9 Lists & List Styles —** Creating unstyled, inline, and definition lists for structured content.

**1.3.10 List Groups —** Using list group components for interactive or styled collections of items.

**1.3.11 Blockquotes —** Presenting quoted text using `.blockquote` with proper spacing and emphasis.

**1.3.12 Citations & Footer Attribution —** Adding sources and authors using footer elements within blockquotes.

---
#### **1.4 Color System & Utilities**

**1.4.1 Theme & Semantic Colors —** Using primary–dark theme colors to convey meaning and maintain visual consistency.

**1.4.2 Contextual Color Usage —** Applying colors appropriately for states such as success, danger, warning, and info.

**1.4.3 Background Utilities —** Setting backgrounds with `.bg-*`, including gradients and opacity controls.

**1.4.4 Text–Background Pairs —** Ensuring readable combinations with `.text-bg-*` utilities.

**1.4.5 Text Colors & Opacity —** Controlling text emphasis using `.text-*` and opacity helpers.

**1.4.6 Emphasis & Muted Text —** Highlighting or de-emphasizing content with emphasis and muted utilities.

**1.4.7 Link Color Styling —** Managing link colors to match themes while preserving accessibility.

**1.4.8 Border Colors & Sides —** Applying semantic border colors and side-specific border utilities.

**1.4.9 Border Width & Style —** Adjusting border thickness and styles for visual separation.

**1.4.10 WCAG Contrast Ratios —** Understanding contrast requirements for readable and accessible UI.

**1.4.11 Accessibility Testing —** Using tools and helpers to validate color accessibility.

---
#### **1.5 Spacing & Sizing Utilities**

**1.5.1 Margin & Padding Utilities —** Using `.m-*` and `.p-*` classes to control outer and inner spacing.

**1.5.2 Directional Spacing —** Applying side-specific spacing with `t`, `b`, `s`, `e`, `x`, and `y` modifiers.

**1.5.3 Spacing Scale (0–5) —** Understanding Bootstrap’s spacing scale and how it maps to rem values.

**1.5.4 Negative Margins —** Pulling elements closer using negative margin utilities where layouts require overlap.

**1.5.5 Auto Spacing & Centering —** Using `.m-auto` and related utilities for horizontal or vertical centering.

**1.5.6 Responsive Spacing —** Adjusting spacing per breakpoint with responsive spacing classes.

**1.5.7 Width Utilities —** Controlling element width with percentage, auto, and max-width helpers.

**1.5.8 Height Utilities —** Managing height with percentage, viewport, and auto height classes.

**1.5.9 Viewport Sizing —** Using `vw`/`vh`-based utilities for full-screen or relative sizing.

**1.5.10 Gap Utilities —** Applying `.gap-*` for flex and grid item spacing.

**1.5.11 Row & Column Gaps —** Using row and column gap utilities for two-dimensional spacing control.

**1.5.12 Gap vs Gutter Distinction —** Understanding when to use flex/grid gaps versus grid gutters.

---
#### **1.6 Components — Structure & Behavior**

**1.6.1 Navigation Components —** Using Navbar, Nav/Tabs, Breadcrumb, and Pagination for site navigation patterns.

**1.6.2 Content Containers —** Structuring content with Cards, Accordion, List Group, Modal, and Offcanvas.

**1.6.3 Buttons & Button Groups —** Triggering actions and grouping controls with consistent styles and states.

**1.6.4 Form Controls & Inputs —** Building forms with inputs, selects, checks, radios, and ranges.

**1.6.5 Input Groups —** Combining inputs with text, icons, or buttons for compact data entry.

**1.6.6 Floating Labels —** Creating modern form labels that float based on input state.

**1.6.7 Form Validation —** Applying validation states, feedback messages, and accessibility patterns.

**1.6.8 Feedback Components —** Informing users with Alerts, Toasts, Progress bars, and Spinners.

**1.6.9 Interactive Components —** Adding behavior with Dropdown, Collapse, Carousel, Scrollspy, Modal toggles.

**1.6.10 Tooltips & Popovers —** Providing contextual hints using JS-powered overlays.

**1.6.11 Visual Utility Components —** Enhancing UI with Badges, Placeholders, and the Close button.

**1.6.12 Component Variants —** Styling components with size, color, and contextual variants.

**1.6.13 Responsive Components —** Adapting components across breakpoints for mobile-first layouts.

**1.6.14 Accessibility in Components —** Ensuring ARIA roles, keyboard support, and screen-reader usability.

**1.6.15 JavaScript Behavior —** Understanding data attributes, JS APIs, events, and lifecycle of components.

---
#### **1.7 Utility Classes & Helpers**

**1.7.1 Display Utilities —** Controlling element display with `.d-*` classes across breakpoints.

**1.7.2 Flexbox Utilities —** Building flexible layouts using flex direction, wrap, alignment, and order helpers.

**1.7.3 Positioning Utilities —** Applying static to sticky positioning, offsets, and z-index controls.

**1.7.4 Border Utilities —** Managing borders with side, color, width, and rounding helpers.

**1.7.5 Shadow Utilities —** Adding depth with predefined box-shadow utilities.

**1.7.6 Visibility & Opacity —** Showing, hiding, and fading elements using visibility and opacity helpers.

**1.7.7 Overflow Control —** Handling content overflow with `.overflow-*` utilities.

**1.7.8 Floats & Clearfix —** Legacy float positioning and clearing techniques where required.

**1.7.9 Interaction Helpers —** Controlling user-select, pointer-events, and cursor behavior.

**1.7.10 Vertical Alignment —** Aligning inline and table-cell content vertically.

**1.7.11 Responsive Variants —** Applying utilities conditionally across breakpoints.

---

You now have the complete **Topic — Brief lists for Sections 1.1 through 1.7**. If you want, I can compile them into a single compact revision sheet or convert them into your StudyMap format.