## **[[1.1 Definitions & Keywords — React]]**

```
React · JavaScript Library ·User Interface Library · Component-Based Architecture ·
Declarative UI · Virtual DOM ·Reconciliation ·JSX · Functional Components · Class Components · Props · State · Hooks · useState · useEffect · useContext · useReducer ·Controlled Components ·Uncontrolled Components ·Composition · One-Way Data Flow ·Event Handling ·Synthetic Events · Conditional Rendering · Lists and Keys · Context API · Side Effects ·Lifecycle Methods · Concurrent Rendering ·Suspense ·Strict Mode · Server-Side Rendering (SSR) · Hydration · Client-Side Rendering (CSR)
```

---

## **[[1.2 Core Principles — React]]**

```
1. Declarative UI Definition — UI expressed as a function of state
2. Component Encapsulation — Logic and rendering co-located within components
3. Unidirectional Data Flow — Data flows downward through component hierarchy
4. Virtual DOM Abstraction — UI changes computed before touching real DOM
5. State-Driven Rendering — UI re-renders in response to state changes
6. Composition over Inheritance — Behavior shared via component composition
7. Side-Effect Isolation — Effects handled separately from rendering logic
8. Deterministic Rendering — Same props and state produce same UI
9. Incremental Adoption — React can be introduced gradually
10. Platform Agnosticism — Core model shared across web and native targets
```

---

## **[[1.3 Mental Models — React]]**

```
1. UI-as-a-Function Model — View = f(state)
2. Component Tree Model — UI structured as a hierarchy of components
3. State Snapshot Model — Each render represents a snapshot in time
4. Diff-and-Patch Model — Virtual DOM diffs determine minimal DOM updates
```

---

## **[[1.4 Architecture Overview — React]]**

### **High-Level Diagram**

```
Application State →
Component Render Functions →
Virtual DOM Tree →
Reconciliation Algorithm →
Minimal DOM Mutations →
Browser Paint →
Updated UI
```

---

### **[[1.4.2 Components & Responsibilities — React]]**

```
1. Components — Encapsulate rendering logic and behavior
2. JSX Transformer — Translates JSX into React element objects
3. React Reconciler — Computes differences between render trees
4. Renderer (React DOM) — Applies updates to host environment
5. Hooks System — Manages state, effects, and lifecycle behavior
6. Event System — Normalizes browser events
7. Context System — Provides scoped global state
8. Concurrent Scheduler — Prioritizes rendering work
```

---

### **[[1.4.3 Data / Render Flow — React]]**

```
State or Props Change →
Component Re-render Triggered →
Virtual DOM Rebuilt →
Diffing and Reconciliation →
DOM Updates Applied →
Browser Repaints →
UI Updated
```

---

## **[[1.5 Internals & Mechanics — React]]**

1. **JSX Compilation** — JSX transformed into `createElement` calls
    
2. **Fiber Architecture** — Incremental, interruptible rendering model
    
3. **Reconciliation Algorithm** — Tree diffing using keys and heuristics
    
4. **Hook State Management** — Hook calls mapped by call order
    
5. **Effect Scheduling** — Side effects executed post-commit phase
    
6. **Synthetic Event System** — Event delegation and normalization
    
7. **Concurrent Rendering Engine** — Non-blocking UI updates
    
8. **Hydration Mechanism** — Attaches interactivity to server-rendered markup
    

---

## **[[1.6 Limitations & Trade-offs — React]]**

|Limitation|Impact / Trade-off|
|---|---|
|**Learning Curve**|Requires understanding of state and re-rendering|
|**Tooling Dependence**|Build tools needed for JSX and bundling|
|**Abstraction Overhead**|Virtual DOM adds runtime complexity|
|**Frequent Re-renders**|Poor state design impacts performance|
|**Not a Full Framework**|Routing and state management external|
|**SEO Considerations**|CSR requires SSR or pre-rendering for SEO|
|**Ecosystem Volatility**|Rapid evolution of patterns and APIs|

---

## 🎓 **Micro-Conclusion (Inline Insight)**

> Section 1 defines React as a **declarative, component-driven UI library** that models interfaces as **pure functions of state**.  
> Its architecture emphasizes **predictability, composability, and incremental rendering**, while trading off simplicity and requiring disciplined state management.

---

If you want the **same academic-grade document** next for:

- Next.js
    
- Redux
    
- Vue
    
- Angular
    
- MERN Stack (end-to-end architecture)
    

State the next subject only.