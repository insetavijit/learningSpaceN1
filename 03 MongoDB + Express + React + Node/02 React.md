> [!quote] Jordan Walke (Creator of React, Facebook)  
> **"React is a JavaScript library for building user interfaces. Learn once, write anywhere."**  
> _React: A declarative, efficient, and flexible JavaScript library for building user interfaces._

## **1.1 React Fundamentals & Core Concepts**

### **Learning Intent (~95–105 words)**

This section builds a rigorous mental model of React as a component-based UI library with declarative rendering and unidirectional data flow. Learners master JSX syntax, component composition, the virtual DOM concept, and React's rendering philosophy. The objective is not merely to "use React," but to **think in components**—so every UI is decomposed into reusable, testable pieces that encapsulate rendering logic and state. Strong grounding here prevents common mistakes like direct DOM manipulation, establishes clear understanding of props vs state, and provides the foundation for hooks, lifecycle management, and performance optimization that define production React applications.

|Topic|Focus & Purpose|
|---|---|
|**[[1.1.1 What is React?]]**|React as a UI library (not framework); component-based architecture; declarative programming model; virtual DOM concept; React vs alternatives (Angular, Vue); when React is appropriate. Establishes React's philosophy.|
|**[[1.1.2 JSX Syntax]]**|JSX as syntactic sugar for `React.createElement()`; embedding expressions (`{}`); JSX attributes (className, htmlFor); JSX children; fragments (`<React.Fragment>` or `<>`); JSX compilation (Babel). Defines React's templating.|
|**[[1.1.3 Components & Props]]**|Functional components; component composition; props as read-only data; passing props; children prop; prop types and validation (PropTypes or TypeScript); destructuring props; default props. Establishes component communication.|
|**[[1.1.4 State & useState Hook]]**|Component-local state; `useState` hook syntax; state updates (functional updates); multiple state variables; state immutability; async state updates; when to use state vs props. Defines data management.|
|**[[1.1.5 Event Handling]]**|Event handlers in JSX (`onClick`, `onChange`, `onSubmit`); synthetic events; event object; preventing default; stopping propagation; binding `this` (class components context); passing arguments to handlers. Enables interactivity.|
|**[[1.1.6 Conditional Rendering]]**|`if` statements; ternary operators; logical `&&` operator; switch-case for multiple conditions; avoiding rendered content with `null`; early returns. Defines dynamic UI patterns.|
|**[[1.1.7 Lists & Keys]]**|Rendering arrays with `map()`; key prop significance (reconciliation); unique stable keys; index as key anti-pattern; filtered/sorted lists; nested lists. Critical for list rendering performance.|

---

## **1.2 React Hooks (Core)**

### **Learning Intent (~100–110 words)**

React Hooks revolutionized functional components by enabling state and lifecycle features without classes. This section teaches the core hooks (`useState`, `useEffect`, `useContext`, `useRef`, `useReducer`) and custom hooks. Learners master side effects management, context consumption, mutable refs, and complex state logic. Understanding hook rules, dependency arrays, and cleanup functions is critical for preventing bugs. The goal is to write modern functional components that leverage hooks idiomatically while avoiding common pitfalls like missing dependencies, infinite loops, and stale closures—skills tested in 90%+ of React interviews as hooks are now the standard approach to React development.

|Topic|Focus & Purpose|
|---|---|
|**[[1.2.1 useState Hook]]**|Declaring state; state updates; functional updates (`prev => next`); multiple state variables; lazy initialization; state as arrays/objects; immutability patterns. Establishes stateful functional components.|
|**[[1.2.2 useEffect Hook]]**|Side effects in functional components; effect cleanup; dependency array; running effects (mount, update, unmount); common effects (data fetching, subscriptions, timers); async effects; avoiding infinite loops. Critical for lifecycle equivalents.|
|**[[1.2.3 useContext Hook]]**|Consuming context without Consumer; `useContext` syntax; when to use context; avoiding prop drilling; context performance considerations; multiple contexts. Simplifies context consumption.|
|**[[1.2.4 useRef Hook]]**|Mutable refs; accessing DOM elements; persisting values across renders; `current` property; when `useRef` vs `useState`; avoiding re-renders with refs; refs in focus management. Enables imperative operations.|
|**[[1.2.5 useReducer Hook]]**|Complex state logic; reducer function; actions and dispatch; useReducer vs useState; initializing state; migrating from useState to useReducer; Redux patterns in local state. Manages structured state updates.|
|**[[1.2.6 Custom Hooks]]**|Extracting reusable logic; naming convention (`useSomething`); custom hook composition; sharing stateful logic; testing custom hooks; common custom hook patterns (useFetch, useLocalStorage, useDebounce). Enables code reuse.|
|**[[1.2.7 Hook Rules & Best Practices]]**|Only call hooks at top level; only call in React functions; ESLint plugin; dependency arrays accuracy; exhaustive-deps warnings; avoiding stale closures; hook ordering. Prevents common mistakes.|

---

## **1.3 Performance Optimization**

### **Learning Intent (~95–105 words)**

React's default rendering behavior re-renders components when props or state change, which can cause performance issues in complex applications. This section teaches performance optimization techniques using `React.memo`, `useMemo`, `useCallback`, code splitting, and lazy loading. Learners master when to optimize, how to prevent unnecessary re-renders, and how to profile React applications. Understanding the reconciliation algorithm, memoization strategies, and the cost-benefit of optimization is critical. The goal is to build performant applications that maintain 60fps rendering while avoiding premature optimization—skills that separate junior from senior React developers in interviews.

|Topic|Focus & Purpose|
|---|---|
|**[[1.3.1 React Rendering Behavior]]**|When React re-renders; parent-to-child render cascade; reconciliation algorithm; virtual DOM diffing; why re-renders happen; identifying render causes. Understands rendering mechanism.|
|**[[1.3.2 React.memo]]**|Memoizing functional components; shallow prop comparison; custom comparison function; when React.memo helps; when it doesn't; memoizing expensive components. Prevents unnecessary renders.|
|**[[1.3.3 useMemo Hook]]**|Memoizing expensive calculations; `useMemo` syntax; dependency array; when to use useMemo; memoization overhead; useMemo for object/array stability. Caches computed values.|
|**[[1.3.4 useCallback Hook]]**|Memoizing functions; preventing function recreation; passing callbacks to memoized children; useCallback with React.memo; dependency arrays; useCallback vs useMemo. Stabilizes function references.|
|**[[1.3.5 Code Splitting & Lazy Loading]]**|Dynamic imports; `React.lazy()`; `Suspense` component; route-based splitting; component-based splitting; error boundaries for lazy components; bundle size optimization. Reduces initial load time.|
|**[[1.3.6 Profiling & Performance Tools]]**|React DevTools Profiler; identifying slow components; flame graphs; ranked charts; measuring render time; Chrome DevTools; `why-did-you-render` library. Enables performance analysis.|
|**[[1.3.7 Windowing/Virtualization]]**|Rendering large lists efficiently; `react-window`, `react-virtualized`; virtual scrolling; item size estimation; lazy loading list items. Handles massive datasets.|

---

## **1.4 Advanced Component Patterns**

### **Learning Intent (~90–100 words)**

Beyond basic components, React supports advanced patterns for code reuse and composition. This section teaches render props, higher-order components (HOCs), compound components, and the provider pattern. Learners master when each pattern solves specific problems, understand their trade-offs, and recognize that modern hooks often replace older patterns. The goal is to understand legacy codebases using these patterns and choose appropriate patterns for new code—balancing code reuse, composition flexibility, and maintainability while understanding that hooks have simplified many scenarios where these patterns were previously necessary.

|Topic|Focus & Purpose|
|---|---|
|**[[1.4.1 Render Props]]**|Sharing code via function props; render prop pattern; children as function; mouse tracking example; render props vs hooks; legacy pattern understanding. Enables flexible composition.|
|**[[1.4.2 Higher-Order Components (HOCs)]]**|Wrapping components; HOC signature; enhancing components; withAuth, withRouter examples; HOC naming; hooks alternative; HOC composition. Legacy abstraction pattern.|
|**[[1.4.3 Compound Components]]**|Flexible, composable APIs; implicit state sharing; context in compound components; React.Children utilities; examples (Select, Tabs); compound vs configuration props. Builds expressive component APIs.|
|**[[1.4.4 Controlled vs Uncontrolled Components]]**|Controlled inputs (state-driven); uncontrolled inputs (ref-driven); when to use each; form libraries (Formik, React Hook Form); defaultValue vs value. Defines form control patterns.|
|**[[1.4.5 Error Boundaries]]**|Catching JavaScript errors in components; `componentDidCatch`, `getDerivedStateFromError`; error boundary implementation; fallback UI; error boundaries don't catch (events, async, SSR errors). Graceful error handling.|

---

## **1.5 Context API & Global State**

### **Learning Intent (~85–95 words)**

The Context API provides a way to share data across the component tree without prop drilling. This section teaches context creation, providers, consumers, and when context is appropriate versus state management libraries. Learners master context performance implications, understand when context causes re-renders, and recognize context's role in theme switching, authentication, and i18n. The goal is to use context effectively for truly global state while understanding its limitations and when dedicated state management (Redux, Zustand) is necessary.

|Topic|Focus & Purpose|
|---|---|
|**[[1.5.1 Context Basics]]**|`React.createContext()`; Provider component; Consumer component (legacy); useContext hook; default context value; nested providers. Establishes context fundamentals.|
|**[[1.5.2 Context Patterns]]**|Theme context; authentication context; language/i18n context; context composition; multiple contexts; avoiding context hell. Practical context applications.|
|**[[1.5.3 Context Performance]]**|When context causes re-renders; optimizing context updates; splitting contexts; memoizing context values; using refs in context; context with useMemo/useCallback. Prevents performance issues.|
|**[[1.5.4 When to Use Context]]**|Context vs prop drilling; context vs state management libraries; infrequent updates (theme, locale, auth) vs frequent updates; context limitations. Guides appropriate usage.|

---

## **1.6 Forms & Validation**

### **Learning Intent (~80–90 words)**

Forms are central to web applications. This section teaches controlled components, form submission, validation, and form libraries that simplify complex forms. Learners master handling inputs, textareas, selects, checkboxes, radio buttons, and file uploads. Understanding validation strategies (client-side, server-side, real-time) and form libraries (React Hook Form, Formik) enables building robust, user-friendly forms with proper error handling and validation feedback.

|Topic|Focus & Purpose|
|---|---|
|**[[1.6.1 Controlled Form Inputs]]**|Binding inputs to state; `onChange` handlers; form data structure; multi-input forms; input types (text, number, email, date); controlled selects and textareas. Establishes form control.|
|**[[1.6.2 Form Submission]]**|Handling `onSubmit`; preventing default; collecting form data; async submission; loading states; success/error feedback; form reset. Defines submission flow.|
|**[[1.6.3 Form Validation]]**|Client-side validation; validation patterns; real-time vs on-submit validation; error messages; validation libraries (Yup, Joi); HTML5 validation. Ensures data integrity.|
|**[[1.6.4 React Hook Form]]**|Uncontrolled forms with React Hook Form; registration; validation; error handling; watch; setValue; form arrays; integration with UI libraries. Modern form solution.|
|**[[1.6.5 File Uploads]]**|File input handling; reading files; FormData for submission; image previews; drag-and-drop uploads; upload progress; multiple files. Enables file handling.|

---

## **1.7 Routing (React Router)**

### **Learning Intent (~85–95 words)**

Single-page applications require client-side routing. This section teaches React Router—the de facto routing library. Learners master declarative routing, route parameters, nested routes, programmatic navigation, and protected routes. Understanding routing is essential for building multi-page SPAs with proper URL management, deep linking, and browser history integration. The goal is to structure applications with clean routing that supports navigation, URL parameters, and authentication-based access control.

|Topic|Focus & Purpose|
|---|---|
|**[[1.7.1 React Router Basics]]**|Installing React Router; `BrowserRouter`, `Routes`, `Route`; `Link` and `NavLink` components; route matching; route ordering; index routes. Establishes routing fundamentals.|
|**[[1.7.2 Route Parameters]]**|Dynamic segments (`:id`); accessing params with `useParams`; optional parameters; multiple parameters; query strings with `useSearchParams`. Enables dynamic routing.|
|**[[1.7.3 Nested Routes]]**|Outlet component; parent-child routes; relative routing; index routes for default child; layout routes. Defines hierarchical routing.|
|**[[1.7.4 Programmatic Navigation]]**|`useNavigate` hook; navigating imperatively; passing state; replacing history; `Navigate` component; redirect patterns. Enables code-triggered navigation.|
|**[[1.7.5 Protected Routes]]**|Authentication-based routing; route guards; redirecting unauthorized users; loading states; role-based routing. Secures application pages.|
|**[[1.7.6 Lazy Loading Routes]]**|Code splitting by route; React.lazy with React Router; Suspense for loading states; optimizing bundle size; prefetching routes. Improves initial load.|

---

## **1.8 HTTP Requests & Data Fetching**

### **Learning Intent (~90–100 words)**

React applications typically fetch data from APIs. This section teaches data fetching patterns using `fetch`, `axios`, and data fetching libraries (React Query, SWR). Learners master handling loading states, errors, and caching. Understanding when to fetch (on mount, on interaction, on route change), how to handle race conditions, and leveraging modern data fetching libraries that provide caching, revalidation, and optimistic updates is critical for building responsive, data-driven applications.

|Topic|Focus & Purpose|
|---|---|
|**[[1.8.1 Fetching Data with useEffect]]**|Fetch API in useEffect; async/await in effects; loading states; error handling; AbortController for cleanup; dependency arrays for refetching. Establishes basic fetching.|
|**[[1.8.2 Axios Library]]**|Installing axios; axios vs fetch; interceptors; default config; error handling; request/response transformation; cancellation. Alternative HTTP client.|
|**[[1.8.3 React Query (TanStack Query)]]**|`useQuery` hook; `useMutation` hook; caching; automatic refetching; invalidation; pagination; infinite queries; optimistic updates. Modern data fetching solution.|
|**[[1.8.4 SWR (Stale-While-Revalidate)]]**|useSWR hook; revalidation strategies; global configuration; mutations; pagination; conditional fetching; comparing SWR vs React Query. Alternative data fetching library.|
|**[[1.8.5 Data Fetching Patterns]]**|Fetch on mount; fetch on user interaction; fetch on route change; polling; WebSockets for real-time data; race condition handling. Defines fetching strategies.|

---

# **SECTION 2 — APPLIED (Real-World React Development)**

## **Learning Intent (~90–100 words)**

This section converts theory into practical React application development. Learners build complete applications, integrate state management libraries, work with TypeScript, implement authentication, and deploy React apps. The focus shifts from isolated concepts to integrated solutions—building dashboards, social feeds, and e-commerce UIs. Applied mastery means delivering production-ready React applications that are testable, performant, accessible, and maintainable while following industry best practices for component architecture, state management, and code organization.

### **2.1 State Management Libraries**

|Area|Purpose|
|---|---|
|**2.1.1 Redux Toolkit**|Redux core concepts (store, actions, reducers); Redux Toolkit (`configureStore`, `createSlice`); async logic with `createAsyncThunk`; Redux DevTools; when Redux is necessary.|
|**2.1.2 Zustand**|Lightweight state management; store creation; selectors; async actions; Zustand vs Redux; when simplicity matters; middleware.|
|**2.1.3 Recoil**|Atoms and selectors; derived state; async queries; atom families; when Recoil fits; Recoil vs Context API.|
|**2.1.4 MobX**|Observable state; reactions; computed values; actions; MobX with React; when MobX's reactive model helps.|

### **2.2 TypeScript with React**

|Area|Purpose|
|---|---|
|**2.2.1 TypeScript Basics**|TypeScript in React projects; type annotations; interfaces; type aliases; union types; generics; tsconfig.json for React.|
|**2.2.2 Typing Components**|Functional component types; props interfaces; `React.FC` vs explicit typing; children prop; event types; generic components.|
|**2.2.3 Typing Hooks**|useState with types; useEffect typing; useRef types; useContext with TypeScript; custom hook typing; useReducer with discriminated unions.|
|**2.2.4 TypeScript Best Practices**|Type inference; avoiding `any`; utility types (`Partial`, `Pick`, `Omit`); type guards; when TypeScript adds value in React.|

### **2.3 Styling in React**

|Area|Purpose|
|---|---|
|**2.3.1 CSS Modules**|Scoped CSS; naming conventions; composition; global styles; CSS Modules with Create React App; class name binding.|
|**2.3.2 Styled Components**|CSS-in-JS with styled-components; tagged template literals; props-based styling; theme provider; global styles; server-side rendering considerations.|
|**2.3.3 Tailwind CSS**|Utility-first CSS; JIT mode; configuration; responsive utilities; component extraction; Tailwind with React; when Tailwind accelerates development.|
|**2.3.4 Material-UI / Ant Design**|Component libraries; theming; customization; grid system; forms; when UI libraries fit project needs.|

### **2.4 Testing React Applications**

|Area|Purpose|
|---|---|
|**2.4.1 React Testing Library**|Queries (getBy, findBy, queryBy); user interactions; async testing; accessibility testing; testing custom hooks; best practices (test behavior, not implementation).|
|**2.4.2 Jest**|Test runners; assertions; mocking modules; snapshot testing; coverage; watch mode; configuring Jest for React.|
|**2.4.3 End-to-End Testing**|Cypress or Playwright; testing user flows; component testing; visual regression; E2E test strategies; CI integration.|

### **2.5 Authentication & Authorization**

|Area|Purpose|
|---|---|
|**2.5.1 JWT Authentication**|Storing tokens (localStorage, cookies); sending tokens in requests; refresh tokens; token expiration; secure storage considerations.|
|**2.5.2 OAuth Integration**|OAuth 2.0 flow; social login (Google, GitHub); handling OAuth callbacks; third-party libraries (Auth0, Firebase Auth).|
|**2.5.3 Protected Routes**|Auth context; PrivateRoute component; redirecting unauthenticated users; role-based routing; loading states during auth check.|

### **2.6 Mini Projects (React-Focused)**

|Level|Project|Outcome|
|---|---|---|
|Beginner|Todo app with local storage|Reinforces useState, useEffect, lists, keys, event handling, localStorage.|
|Intermediate|Weather dashboard with API|Builds data fetching, error handling, loading states, environment variables, axios/React Query.|
|Advanced|Social media feed with infinite scroll|Applies complex state, pagination, optimistic updates, performance optimization, React Query.|
|Production|E-commerce app with cart & checkout|Applies Redux/Zustand, routing, authentication, forms, payment integration (Stripe), testing.|

---

# **SECTION 3 — PROFESSIONAL (Production Deployment & Operations)**

## **Learning Intent (~85–95 words)**

This section positions React within professional production environments. Learners master build optimization, deployment strategies, monitoring, and performance measurement. The goal is long-term competence—shipping React applications that are production-ready, performant, accessible, and maintainable. Professional React developers understand bundle optimization, server-side rendering, static site generation, CI/CD integration, and monitoring strategies that ensure applications perform well for real users.

|Area|Focus|
|---|---|
|**3.1 Build Optimization**|Webpack/Vite configuration; code splitting; tree shaking; minification; source maps; environment variables; bundle analysis (webpack-bundle-analyzer); optimizing dependencies.|
|**3.2 Server-Side Rendering (SSR)**|SSR benefits (SEO, initial load); Next.js framework; `getServerSideProps`; hydration; SSR challenges; when SSR is necessary; SSR vs CSR vs SSG.|
|**3.3 Static Site Generation (SSG)**|SSG with Next.js; `getStaticProps`, `getStaticPaths`; incremental static regeneration; when SSG fits; Gatsby as alternative; SSG vs SSR.|
|**3.4 Deployment**|Deploying to Vercel, Netlify; AWS S3 + CloudFront; Heroku; Docker containers; CI/CD with GitHub Actions; environment-specific builds; preview deployments.|
|**3.5 Monitoring & Analytics**|Error tracking (Sentry); performance monitoring (New Relic, Datadog); user analytics (Google Analytics, Mixpanel); logging in production; real user monitoring (RUM).|
|**3.6 Accessibility (a11y)**|Semantic HTML in JSX; ARIA attributes; keyboard navigation; focus management; screen reader testing; color contrast; accessibility audits (axe, Lighthouse).|
|**3.7 SEO for React**|Meta tags; Open Graph; structured data; sitemap generation; robots.txt; prerendering for crawlers; SSR for SEO; React Helmet.|

---

# **SECTION 4 — REFERENCE & ECOSYSTEM CONTEXT**

## **Learning Intent (~75–85 words)**

This section defines React's ecosystem, evolution, and position in the frontend landscape. Learners understand React versions (16.8 Hooks, 18 Concurrent Features), React's roadmap (Server Components, Compiler), competing frameworks, and career paths. Understanding the React ecosystem—from Create React App to Vite to Next.js—enables informed technology decisions. Ecosystem literacy helps developers navigate the vast React tooling landscape and position React appropriately for project requirements.

|Area|Focus|
|---|---|
|**4.1 React Versions & Evolution**|React 16.8 (Hooks); React 17 (gradual upgrades); React 18 (Concurrent Rendering, automatic batching, Suspense SSR); React 19 roadmap (Server Components, Compiler); staying current.|
|**4.2 React Frameworks**|Next.js (full-stack, SSR/SSG); Remix (nested routing, data loading); Gatsby (static sites); Create React App (deprecated); Vite (fast dev); when frameworks add value.|
|**4.3 React vs Alternatives**|React vs Vue (template vs JSX); React vs Angular (library vs framework); React vs Svelte (virtual DOM vs compiled); when React excels; migration considerations.|
|**4.4 React Ecosystem Tools**|React DevTools; ESLint (eslint-plugin-react-hooks); Prettier; Storybook; React Spring (animations); Framer Motion; date libraries (date-fns, Day.js).|
|**4.5 Career Paths & Skills**|React developer roles; frontend vs full-stack; React Native for mobile; salaries; building React portfolio; certification (Meta's React certification); job market demand.|
|**4.6 Contributing to React**|React GitHub; RFCs; open source contribution; React Working Group; beta docs feedback; community involvement.|

---