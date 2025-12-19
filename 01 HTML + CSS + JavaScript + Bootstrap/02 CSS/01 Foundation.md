## **[[1.1 Definitions & Keywords — CSS]]**

```
CSS (Cascading Style Sheets) · Style Sheet Language · Declarative Styling ·
Presentation Layer · Visual Formatting Model ·
Selectors · Declarations · Properties · Values ·
Rule Sets · At-Rules ·
Cascade · Specificity · Inheritance · Source Order ·
Box Model · Content Box · Padding · Border · Margin ·
Display Models · Block · Inline · Inline-Block · None ·
Positioning · Static · Relative · Absolute · Fixed · Sticky ·
Layout Systems · Normal Flow · Flexbox · Grid Layout ·
Media Queries · Responsive Design ·
Pseudo-Classes · Pseudo-Elements ·
Custom Properties (CSS Variables) ·
Computed Values · Used Values · Initial Values ·
Reflow · Repaint ·
Browser Rendering Engine ·
Progressive Enhancement · Backward Compatibility
```

---

## **[[1.2 Core Principles — CSS]]**

```
1. Separation of Presentation — CSS controls visual appearance, not document structure
2. Cascading Resolution — Conflicting rules resolved through cascade, specificity, and order
3. Declarative Styling Model — Styles describe desired outcomes, not procedural steps
4. Inheritance Mechanism — Certain properties flow from parent to child elements
5. Progressive Enhancement — Base styles first; advanced features layered conditionally
6. Responsive Adaptation — Layout and presentation adapt across viewport conditions
7. Predictable Layout Models — Defined algorithms govern box sizing and layout behavior
8. Non-Destructive Overrides — Styles can be layered and overridden without altering markup
9. Device and Medium Awareness — Styles adapt to screens, print, and other media
10. Standards-Driven Evolution — Features evolve incrementally with backward compatibility
```

---

## **[[1.3 Mental Models — CSS]]**

```
1. Layered Paint System — Styles applied in ordered layers from general to specific
2. Box-Based Visualization — Every element represented as a rectangular box
3. Constraint-Based Layout — Layout resolved by rules and relationships, not coordinates
4. Reactive Rendering Model — Visual output recalculated when inputs (DOM, viewport, styles) change
```

---

## **[[1.4 Architecture Overview — CSS]]**

### **High-Level Diagram**

```
HTML Elements →
CSS Rules Matched →
Cascade & Specificity Resolved →
Computed Styles Generated →
Layout Algorithm Executes →
Painting →
Compositing →
Rendered Page
```

---

### **[[1.4.2 Components & Responsibilities — CSS]]**

```
1. Selectors — Identify target elements in the DOM
2. Declarations — Define property–value pairs
3. Cascade Engine — Resolves rule conflicts
4. Inheritance System — Propagates inheritable properties
5. Box Model — Governs element sizing and spacing
6. Layout Engines — Flexbox, Grid, and normal flow algorithms
7. Media Query System — Applies conditional styling rules
8. Custom Properties Layer — Enables theming and runtime configuration
```

---

### **[[1.4.3 Data / Render Flow — CSS]]**

```
CSS Source →
Parsing →
Selector Matching →
Cascade Resolution →
Computed Style Assignment →
Layout Calculation →
Paint →
Composite →
Final Visual Output
```

---

## **[[1.5 Internals & Mechanics — CSS]]**

1. **Selector Matching Algorithms** — Efficient matching of rules to DOM elements
    
2. **Cascade Resolution Model** — Ordered evaluation of origin, importance, specificity, and order
    
3. **Computed and Used Values Pipeline** — Transformation of authored values into renderable values
    
4. **Layout Algorithm Execution** — Flexbox and Grid resolve dimensions and alignment
    
5. **Reflow Triggers** — Structural or dimensional changes causing layout recalculation
    
6. **Repaint Triggers** — Visual-only changes affecting pixel rendering
    
7. **Compositing Layers** — GPU-accelerated rendering of independent layers
    
8. **Feature Detection and Fallbacks** — Support for partial implementations across browsers
    

---

## **[[1.6 Limitations & Trade-offs — CSS]]**

|Limitation|Impact / Trade-off|
|---|---|
|**No Application Logic**|Cannot express conditions or control flow|
|**Indirect State Handling**|Limited state via selectors and pseudo-classes|
|**Cascade Complexity**|Large stylesheets can become hard to reason about|
|**Global Scope by Default**|Risk of unintended side effects|
|**Browser Implementation Variance**|Minor inconsistencies across engines|
|**Debugging Difficulty**|Visual bugs often require deep cascade analysis|
|**Performance Sensitivity**|Inefficient selectors and layouts impact rendering|

---

## 🎓 **Micro-Conclusion (Inline Insight)**

> Section 1 establishes CSS as a **declarative, rule-based presentation system** that transforms structured documents into responsive visual interfaces.  
> Its power lies in the cascade, layout algorithms, and rendering pipeline — enabling separation, scalability, and adaptive design without altering content structure.

---

If you want this **continued consistently** for **JavaScript**, **Browser Rendering**, or **Web Architecture**, state the subject only.