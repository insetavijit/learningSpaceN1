
> [!quote] Tim Berners-Lee (Computer Scientist, London, 1955)  
> **“The power of the Web is in its universality. Access by everyone regardless of disability is an essential aspect.”**  
> _Semantic, accessible HTML is the foundation of an open web._

## **1.1 Document Structure, Semantics & Content Categories**

### **Learning Intent (~90–100 words)**

This section builds a rigorous mental model of how HTML expresses document meaning, hierarchy, and content roles. Learners master the skeleton of an HTML document, semantic sectioning, text meaning, navigation, and embedded resources. The objective is not merely to “use tags,” but to **encode intent**—so browsers, search engines, and assistive technologies can correctly interpret content. Strong grounding here ensures every future layout, style, and script rests on markup that is predictable, accessible, and standards-compliant.

| Topic                                             | Focus & Purpose                                                                                                                                                                                       |
| ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[[1.1.1 HTML Document Skeleton]]**              | DOCTYPE, `<html>`, `<head>`, `<body>`, encoding, language, and base URL define standards mode, metadata, and global document context. Establishes the non-negotiable structure every page depends on. |
| **[[1.1.2 Sectioning & Structural Semantics]]**   | `<header>`, `<main>`, `<section>`, `<article>`, `<nav>`, `<aside>`, `<footer>` express document outline and landmarks. Enables assistive navigation and meaningful page hierarchy.                    |
| **[[1.1.3 Text Semantics & Content Meaning]]**    | Headings, paragraphs, emphasis, quotes, lists, and inline semantics encode meaning, not style. Critical for readability, SEO, and accessibility.                                                      |
| **[[1.1.4 Hyperlinks & Navigation]]**             | Anchors, URLs, fragments, `rel`, and navigation semantics define how documents connect and how users traverse information spaces.                                                                     |
| **[[1.1.5 Embedded Media & External Resources]]** | Images, responsive media, audio/video, iframes, figures, and captions integrate rich content while preserving meaning, performance, and accessibility.                                                |

---

## **1.2 Forms & User Input — Structure and Semantics**

### **Learning Intent (~75–90 words)**

Forms introduce HTML as an interface for human input and system communication. This section teaches how data is structured, constrained, validated, and made accessible using native semantics. Learners understand the submission lifecycle and how browsers participate in validation before any backend logic exists. The goal is to design forms that are usable, secure by default, and interoperable with server-side systems.

| Topic                                                | Focus & Purpose                                                                                                 |
| ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **[[1.2.1 Form Structure & Submission Model]]**      | `<form>`, methods, actions, fieldsets, legends, and lifecycle define how data flows from user to server.        |
| **[[1.2.2 Input Controls & Types]]**                 | Inputs, selects, textareas, buttons, and native widgets model different data types and UX behaviors.            |
| **[[1.2.3 Native Validation & Feedback Semantics]]** | Required fields, patterns, ranges, validity states, and Constraint Validation API provide built-in correctness. |
| **[[1.2.4 Form Accessibility Semantics]]**           | Labels, descriptions, grouping, and keyboard navigation ensure inclusive data entry experiences.                |

---

## **1.3 Accessibility & Semantic Contracts**

### **Learning Intent (~70–85 words)**

This section formalizes the contract between HTML and assistive technologies. Learners master native semantics, focus behavior, and when ARIA is appropriate—or harmful. The goal is to ensure interfaces are perceivable, operable, and understandable without relying on scripts or styling. Accessibility here is treated as **structural correctness**, not an afterthought.

|Topic|Focus & Purpose|
|---|---|
|**[[1.3.1 Native Accessibility Semantics]]**|Implicit roles, landmarks, focus rules, and interactive semantics built into HTML elements.|
|**[[1.3.2 ARIA Integration Boundaries]]**|When ARIA is allowed, forbidden, and how native HTML always takes precedence.|
|**[[1.3.3 Keyboard & Focus Model]]**|Tab order, focus management, and sequential navigation define non-pointer interaction.|

---

## **1.4 Browser Interpretation & Rendering Model**

### **Learning Intent (~75–90 words)**

Understanding how browsers parse and render HTML builds intuition for why markup errors matter. Learners study tokenization, DOM construction, rendering pipelines, and error recovery. This knowledge explains quirks, layout bugs, and cross-browser behavior—empowering developers to write HTML that cooperates with the browser engine rather than fighting it.

|Topic|Focus & Purpose|
|---|---|
|**[[1.4.1 Parsing & DOM Construction]]**|Tokenization, tree building, incremental parsing, and recovery from malformed markup.|
|**[[1.4.2 Rendering Pipeline Interaction]]**|DOM–CSSOM to render tree, layout, paint, reflow, and repaint.|
|**[[1.4.3 Error Tolerance & Compatibility Model]]**|Backward compatibility, malformed HTML handling, standards vs quirks mode.|

---

# **SECTION 2 — APPLIED (Real-World HTML Authoring)**

## **Learning Intent (~80–90 words)**

This section converts theory into professional markup practice. Learners design real page structures, content-heavy documents, and HTML intended to integrate cleanly with CSS and JavaScript. The focus is on patterns that scale: clarity, reuse, and predictable hooks for styling and scripting.

### **2.1 Applied Markup Patterns**

|Area|Purpose|
|---|---|
|**2.1.1 Semantic Page Layouts**|Marketing pages, blogs, docs, dashboards using landmarks and hierarchy.|
|**2.1.2 Content-Heavy Documents**|Long articles, structured tables, references, and citations.|
|**2.1.3 HTML for CSS & JavaScript Integration**|Classes, `data-*`, and DOM targeting strategies without breaking semantics.|

### **2.2 Mini Projects (Markup-Only)**

|Level|Project|Outcome|
|---|---|---|
|Beginner|Semantic personal profile page|Reinforces structure and text/media semantics.|
|Intermediate|Multi-page content website|Builds navigation and layout consistency.|
|Production|SEO & accessibility-ready business page|Applies full semantic, metadata, and accessibility discipline.|

---

# **SECTION 3 — PROFESSIONAL (Standards, Tooling & Longevity)**

## **Learning Intent (~70–85 words)**

This section positions HTML as a living standard within professional systems. Learners gain literacy in specifications, validation discipline, and how HTML operates inside CMSs and frameworks. The goal is long-term competence—writing markup that survives tooling changes and evolving ecosystems.

|Area|Focus|
|---|---|
|**3.1 Standards & Specification Literacy**|WHATWG Living Standard, content categories, conformance rules.|
|**3.2 Tooling & Validation**|W3C validator, DevTools DOM inspection, accessibility audits.|
|**3.3 HTML in Systems**|CMS pipelines, server-rendered HTML, framework substrates.|

---

# **SECTION 4 — REFERENCE & EDGE CONSIDERATIONS**

## **Learning Intent (~60–75 words)**

This section defines the boundaries of HTML—what to avoid, how to handle global documents, and where HTML stops and other technologies begin. It prevents legacy habits and clarifies responsibility across the stack.

|Area|Focus|
|---|---|
|**4.1 Deprecated & Obsolete Features**|Elements and attributes to avoid; legacy anti-patterns.|
|**4.2 Internationalization & Global Documents**|`lang`, `dir`, multilingual structure.|
|**4.3 HTML Limitations & Boundaries**|No logic, no layout authority, reliance on CSS and JavaScript.|

---
