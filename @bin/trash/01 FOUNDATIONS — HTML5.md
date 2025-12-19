

### 1.1 Definitions — _What HTML Is and Is Not_

- **HTML (HyperText Markup Language)** — the structural language of the web; defines content, not behavior or styling
    
- **Markup vs Programming Language** — declarative structure, no logic or control flow
    
- **Element, Tag, Attribute** — core vocabulary used everywhere in HTML
    
- **Document** — a tree of elements interpreted by the browser
    

### 1.2 Core Principles — _Rules That Govern HTML_

- **Semantic Meaning Over Appearance** — tags describe purpose, not looks
    
- **Hierarchy & Nesting** — parent–child relationships define structure
    
- **Separation of Concerns** — HTML (structure), CSS (presentation), JS (behavior)
    
- **Progressive Enhancement** — usable content even without CSS/JS
    

### 1.3 Mental Models — _How to Think About HTML_

- **Document as a Tree (DOM)** — every element is a node in a hierarchy
    
- **HTML as Content Blueprint** — browser renders meaning before style
    
- **Flow of Content** — block vs inline elements shape layout behavior
    
- **Accessibility First** — HTML is consumed by humans _and_ machines
    

## 1.4 Architecture Overview — _How an HTML Document Is Built_

#### 1.4.1 High-Level Structure

- `<!DOCTYPE>` — tells browser which HTML standard to use
    
- `<html>` — root container of the entire document
    
- `<head>` — metadata, not visible content
    
- `<body>` — visible, interactive content
    

#### 1.4.2 Components & Responsibilities

- **Metadata Elements** — `meta`, `title`, `link`, `script`
    
- **Content Containers** — `div`, `section`, `article`, `main`
    
- **Text Elements** — `h1–h6`, `p`, `span`, `strong`, `em`
    
- **Media Elements** — `img`, `audio`, `video`, `figure`
    
- **Interactive Elements** — `a`, `button`, `form`, `input`
    

#### 1.4.3 Data Flow (Browser Perspective)

- HTML file → parsed into DOM
    
- DOM → combined with CSSOM
    
- Render Tree → layout → paint → display
    

### 1.5 Internals & Mechanics — _What Happens Under the Hood_

- **Parsing Process** — browser reads HTML top-down
    
- **Error Tolerance** — browsers auto-correct invalid HTML
    
- **DOM Construction** — live object model accessible via JavaScript
    
- **Reflow & Repaint** — changes to structure affect performance
    

### 1.6 Limitations & Trade-offs — _What HTML Cannot Do_

- No logic, loops, or conditions
    
- No styling control beyond defaults
    
- No state management
    
- Relies on CSS and JavaScript for full applications
    

### 1.7 Standards & Evolution — _Why HTML5 Matters_

- **Living Standard (WHATWG)** — continuously evolving
    
- **Backward Compatibility** — old pages still work
    
- **Native Capabilities** — forms, media, semantics without JS
    
- **Web as a Platform** — HTML is foundational, not optional
    

---