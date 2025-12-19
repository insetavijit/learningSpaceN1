Boss, below is **Section 1 — FOUNDATIONS for Eleventy (11ty)** written in the **same document style, structure, and rigor** as your HTML5 example.

---

## **[[1.1 Definitions & Keywords — Eleventy (11ty)]]**

```
Eleventy (11ty) · Static Site Generator (SSG) ·Build-Time Rendering · Jamstack ·Node.js Tooling ·Input Directory · Output Directory (_site) ·Content Files · Markdown · HTML ·Template · Template Engine ·Nunjucks · Liquid · JavaScript Templates ·Front Matter · YAML · JSON · TOML ·Global Data · Directory Data · Data Cascade ·Collections · Tags ·Layouts · Includes · Partials ·Permalinks · URLs ·
Passthrough Copy · Static Assets ·Configuration File (.eleventy.js) ·Filters · Shortcodes · Transforms · Plugins ·CLI (Command Line Interface) ·Incremental Builds · Watch Mode ·Dev Server ·Static Hosting · Pre-rendered HTML ·Framework-Agnostic · Content-First ·Zero/Low Config · Build Pipeline · File-Based Routing
```

---

## **[[1.2 Core Principles — Eleventy (11ty)]]**


1. [[Content-First Design]] — Markdown/HTML are primary, not components
2. [[Build-Time Rendering]] — All pages generated before deployment
3. [[Simplicity Over Abstraction]] — Minimal framework magic
4. Unopinionated Structure — You control folders and architecture
5. Template Agnosticism — Multiple engines, no lock-in
6. HTML as the Output Contract — Clean, framework-free HTML
7. Progressive Complexity — Start simple, extend when needed
8. Performance by Default — Static output enables fast delivery
9. Toolchain Composability — Integrates with any JS/CSS tooling
10. Developer Ownership — No hidden conventions, explicit config


---

## **[[1.3 Mental Models — Eleventy (11ty)]]**

```
1. Compiler Model — Source files are compiled into static HTML
2. Content + Data → Templates → Pages — Rendering equation
3. File System as API — Folder and file names define behavior
4. Pipeline Model — Read → Parse → Render → Write
5. Cascade Model — Data flows from global to local
6. HTML Factory — Eleventy manufactures finished documents
7. Build Once, Serve Anywhere — Output is portable static files
```

## **[[1.4 Architecture Overview — Eleventy (11ty)]]**

### **High-Level Diagram**

```
Author Writes Content/Templates/Data →
Eleventy Discovers Files →
Data Loaded & Merged →
Templates Rendered →
Static Files Written to _site →
(Optional) Dev Server Serves Output →
Deployment to Static Host
```

### **[[1.4.2 Components & Responsibilities — Eleventy (11ty)]]**

```
1. Input System — Scans directories for supported file types
2. Content Files — Markdown/HTML defining page bodies
3. Front Matter Parser — Extracts metadata from files
4. Data Layer — Loads global, directory, page, computed data
5. Template Engines — Render content into HTML
6. Layout System — Composes pages from base templates
7. Collections Engine — Groups content programmatically
8. Configuration Layer — Customizes behavior via .eleventy.js
9. Build Orchestrator — Coordinates full rendering pipeline
10. Passthrough Handler — Copies static assets unchanged
11. Output Writer — Writes final site into _site/
12. Watcher & Dev Server — Rebuilds and serves during development
```

### **[[1.4.3 Data / Render Flow — Eleventy (11ty)]]**

```
Content Files →
Front Matter Extraction →
Global Data (_data) →
Directory Data (.11tydata.js) →
Computed Data →
Data Cascade Merge →
Template Rendering →
Layout Composition →
Static HTML Output →
_site Directory
```

## **[[1.5 Internals & Mechanics — Eleventy (11ty)]]**

1. **File Discovery & Graph Building** — Scans inputs and builds dependency graph
    
2. **Front Matter Parsing** — Separates metadata from content body
    
3. **Data Resolution Engine** — Merges data using cascade precedence rules
    
4. **Template Engine Dispatch** — Selects engine based on file type
    
5. **Template Compilation** — Converts templates into render functions
    
6. **Rendering Phase** — Injects content and data into templates
    
7. **Layout Inheritance Resolution** — Wraps pages in nested layouts
    
8. **Collections Evaluation** — Builds arrays of related content
    
9. **Incremental Build System** — Rebuilds only affected files on change
    
10. **Passthrough Copy Mechanism** — Copies assets directly to output
    
11. **Output Serialization** — Writes final HTML and assets to disk
    
12. **Watch & Serve Loop** — Monitors FS events and triggers rebuilds
    

## **[[1.6 Limitations & Trade-offs — Eleventy (11ty)]]**

|Limitation|Impact / Trade-off|
|---|---|
|**Build-Time Only Rendering**|No dynamic server-side logic at runtime|
|**No Built-in CMS**|Requires Git workflow or external headless CMS|
|**No Asset Bundler**|CSS/JS pipelines must be added manually|
|**Manual Interactivity**|Client-side JS needed for dynamic behavior|
|**Large Sites Build Cost**|Big content sets increase build time|
|**Multiple Template Syntaxes**|Flexibility can increase cognitive load|
|**Not a SPA Framework**|Not suited for app-like client routing|
|**Node.js Dependency**|Requires JS tooling environment|

## 🎓 **Micro-Conclusion (Inline Insight)**

> Section 1 frames Eleventy as a **content-first, build-time HTML compiler**.  
> Its architecture emphasizes **simplicity, developer control, and static performance**,  
> making it ideal for blogs, documentation, and content-heavy sites where **clean HTML is the product**,  
> not a byproduct of a framework.
