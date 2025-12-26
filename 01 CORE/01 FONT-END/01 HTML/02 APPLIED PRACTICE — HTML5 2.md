### **1.1 Document Structure, Semantics & Content Categories**

This section builds a rigorous mental model of how HTML expresses document meaning, hierarchy, and content roles. Learners master the skeleton of an HTML document, semantic sectioning, text meaning, navigation, and embedded resources. The objective is not merely to “use tags,” but to **encode intent**—so browsers, search engines, and assistive technologies can correctly interpret content. Strong grounding here ensures every future layout, style, and script rests on markup that is predictable, accessible, and standards-compliant.

| Topic                                             | Focus & Purpose                                                                                                                                                                                       |
| ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[[1.1.1 HTML Document Skeleton]]**              | DOCTYPE, `<html>`, `<head>`, `<body>`, encoding, language, and base URL define standards mode, metadata, and global document context. Establishes the non-negotiable structure every page depends on. |
| **[[1.1.2 Sectioning & Structural Semantics]]**   | `<header>`, `<main>`, `<section>`, `<article>`, `<nav>`, `<aside>`, `<footer>` express document outline and landmarks. Enables assistive navigation and meaningful page hierarchy.                    |
| **[[1.1.3 Text Semantics & Content Meaning]]**    | Headings, paragraphs, emphasis, quotes, lists, and inline semantics encode meaning, not style. Critical for readability, SEO, and accessibility.                                                      |
| **[[1.1.4 Hyperlinks & Navigation]]**             | Anchors, URLs, fragments, `rel`, and navigation semantics define how documents connect and how users traverse information spaces.                                                                     |
| **[[1.1.5 Embedded Media & External Resources]]** | Images, responsive media, audio/video, iframes, figures, and captions integrate rich content while preserving meaning, performance, and accessibility.                                                |

### **1.2 Forms & User Input — Structure and Semantics**

Forms introduce HTML as an interface for human input and system communication. This section teaches how data is structured, constrained, validated, and made accessible using native semantics. Learners understand the submission lifecycle and how browsers participate in validation before any backend logic exists. The goal is to design forms that are usable, secure by default, and interoperable with server-side systems.

| Topic                                                | Focus & Purpose                                                                                                 |
| ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **[[1.2.1 Form Structure & Submission Model]]**      | `<form>`, methods, actions, fieldsets, legends, and lifecycle define how data flows from user to server.        |
| **[[1.2.2 Input Controls & Types]]**                 | Inputs, selects, textareas, buttons, and native widgets model different data types and UX behaviors.            |
| **[[1.2.3 Native Validation & Feedback Semantics]]** | Required fields, patterns, ranges, validity states, and Constraint Validation API provide built-in correctness. |
| **[[1.2.4 Form Accessibility Semantics]]**           | Labels, descriptions, grouping, and keyboard navigation ensure inclusive data entry experiences.                |
### **1.3 Accessibility & Semantic Contracts**

This section formalizes the contract between HTML and assistive technologies. Learners master native semantics, focus behavior, and when ARIA is appropriate—or harmful. The goal is to ensure interfaces are perceivable, operable, and understandable without relying on scripts or styling. Accessibility here is treated as **structural correctness**, not an afterthought.

| Topic                                        | Focus & Purpose                                                                             |
| -------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **[[1.3.1 Native Accessibility Semantics]]** | Implicit roles, landmarks, focus rules, and interactive semantics built into HTML elements. |
| **[[1.3.2 ARIA Integration Boundaries]]**    | When ARIA is allowed, forbidden, and how native HTML always takes precedence.               |
| **[[1.3.3 Keyboard & Focus Model]]**         | Tab order, focus management, and sequential navigation define non-pointer interaction.      |

### **1.4 Browser Interpretation & Rendering Model**

Understanding how browsers parse and render HTML builds intuition for why markup errors matter. Learners study tokenization, DOM construction, rendering pipelines, and error recovery. This knowledge explains quirks, layout bugs, and cross-browser behavior—empowering developers to write HTML that cooperates with the browser engine rather than fighting it.

| Topic                                               | Focus & Purpose                                                                       |
| --------------------------------------------------- | ------------------------------------------------------------------------------------- |
| **[[1.4.1 Parsing & DOM Construction]]**            | Tokenization, tree building, incremental parsing, and recovery from malformed markup. |
| **[[1.4.2 Rendering Pipeline Interaction]]**        | DOM–CSSOM to render tree, layout, paint, reflow, and repaint.                         |
| **[[1.4.3 Error Tolerance & Compatibility Model]]** | Backward compatibility, malformed HTML handling, standards vs quirks mode.            |

---

```php
# HTML Document Skeleton & Metadata
<!DOCTYPE html>, html, head, title, base, link, meta, style, body, noscript
# Sectioning & Structural Semantics
address, article, aside, footer, header, h1, h2, h3, h4, h5, h6, hgroup, main, nav, section, search
# Flow & Block-level Content
div, blockquote, dd, dl, dt, figcaption, figure, hr, li, menu, ol, p, pre, ul
# Inline Text Semantics & Content Meaning
a, abbr, b, bdi, bdo, br, cite, code, data, dfn, em, i, kbd, mark, q, rp, rt, ruby, s, samp, small, span, strong, sub, sup, time, u, var, wbr, del, ins
# Demarcating Edits
del, ins
# Image and Multimedia
area, img, map, picture, source, audio, track, video
# Embedded Content & External Resources
embed, iframe, object, param, figure, figcaption
# SVG and MathML Embedded
svg, math
# Scripting & Interactive Graphics
canvas, script, noscript
# Table Content
caption, col, colgroup, table, tbody, td, tfoot, thead, th, tr
# Forms & Interactive Elements
button, datalist, fieldset, form, input, label, legend, meter, optgroup, option, output, progress, select, textarea, details, dialog, summary
# Web Components
slot, template
```

---
