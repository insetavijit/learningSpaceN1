> [!quote] Yajur Veda 19.30
> **“विद्याम् चाविद्याम् च यस्तद्वेदोभयं सह।”**
> *“True wisdom is gained by uniting knowledge with action.”*
> Learning Loop: **Explore → Learn → Do → Comprehend → Improve → Repeat**

# 1. FOUNDATIONS (ALL – PHP CONTEXT)

1.1 **Definitions** – core PHP terms
* Script, interpreter, runtime
* Statements, expressions, variables
* Scalars (int, float, string, bool)
* Arrays, associative arrays, objects
* Functions, methods, classes, interfaces, traits
* Namespace, autoloading, composer package
1.2 **Core Principles** – how PHP “thinks”
* Request–response model (per-request execution)
* Weak vs strict typing (`declare(strict_types=1)`)
* Superglobals (`$_GET`, `$_POST`, `$_SERVER`, `$_SESSION`, `$_COOKIE`, `$_FILES`)
* Error vs Exception handling
* Configuration via `php.ini` and `.htaccess`
* Runtime environment: SAPI (CLI, FPM, Apache module)
1.3 **Mental Models** – intuition builders
* “PHP as a template engine” → mixing HTML & PHP
* “PHP as a scripting glue” → connecting DB, cache, APIs
* “Stateless per request” → nothing persists unless you store it
* “Composer is npm for PHP” – dependency & autoload manager
* “Framework as traffic controller” – routing, controllers, views, services
1.4 **Architecture Overview** – typical PHP web stack
1.4.1 High-Level Diagram
* Browser → Web server (Nginx/Apache) → PHP-FPM/PHP module → App (plain PHP / framework) → DB / cache / services
1.4.2 **Components & Responsibilities**
* Front controller (`index.php`)
* Router → routes URL to controller/action
* Controller → coordinates request handling
* Model/Entity → data & domain logic
* View/template → HTML rendering (Blade/Twig/PHP templates)
* Service/Repository → business rules & DB abstraction
* Config & Env → `.env` / config files
1.4.3 Data Flow
* Input: query string, POST data, JSON payloads
* Processing: validation, sanitization, business logic
* Persistence: MySQL/PostgreSQL/Redis/etc
* Output: HTML, JSON APIs, file downloads
1.5 Internals & Mechanics
* Zend Engine basics: opcode, compilation, execution
* Memory model: zvals, references, copy-on-write (high-level)
* Opcache: caching compiled bytecode
* Garbage collection for cyclic references
* Session mechanism: IDs, storage handlers
* Autoloading (PSR-4), class loading flow
1.6 Limitations & Trade-offs
* Request-scoped memory (no in-process long-lived state)
* Performance vs native (C/Go) for CPU-heavy workloads
* Concurrency via web server, not threads in same process
* Security pitfalls: injection, XSS, CSRF, file upload risks
* Legacy code & mixed styles across codebases

---
# 2. APPLIED PRACTICE (4 Sheets – PHP FOCUSED)

2.1 Code Examples – growing difficulty
2.1.1 Basic Examples
* Variables, types, operators
* Control flow (`if`, `switch`, `for`, `foreach`)
* Functions, default args, type hints, return types
* Basic string & array functions
* Simple form handling with `$_GET` / `$_POST`

2.1.2 Intermediate Examples

* Working with associative arrays & nested arrays
* OOP: classes, inheritance, interfaces, traits
* Namespaces & autoloading with Composer
* File uploads, file system operations
* Basic PDO query & prepared statements
* Simple REST endpoint returning JSON

2.1.3 Advanced Examples
* Dependency Injection and service containers
* Middleware-style request pipeline
* Custom exception hierarchy & global error handler
* Using a framework (Laravel/Symfony) example controller/service
* Queues & jobs (e.g., Laravel queues)
* Writing and using a Composer package
2.2 Hands-on Mini Projects
2.2.1 Beginner Project – “PHP Basics Lab”
* CLI utilities: calculator, todo-list in a file
* Simple contact form → email or file store
2.2.2 Intermediate Project – “Mini CRUD App”
* CRUD for one entity (e.g., notes/blog posts)
* Login/logout with sessions
* Simple validation & flash messages
2.2.3 Production Project – “Real-world Web App”
* Built with Laravel/Symfony
* Authentication, roles/permissions
* REST API + frontend (Blade/SPA client)
* Logging, config envs, error pages
2.3 Patterns & Workflows
2.3.1 Design Patterns (PHP examples)
* Singleton (and why to avoid)
* Factory, Strategy, Adapter, Repository
* MVC and variations (Service layer, DTOs)
* Observer/Events & Listeners
2.3.2 Common Workflows
* Implementing form → validation → DB → redirect flow
* Adding a new endpoint in a framework
* Introducing a new service & wiring via DI container
* Writing and running unit tests (PHPUnit/Pest)
2.3.3 Anti-patterns (PHP flavored)
* Massive “god” classes, fat controllers
* SQL queries mixed directly into views
* Echoing everywhere instead of templating
* Using `$_POST`/`$_GET` directly deep in application logic
* Overusing static methods & global state
2.4 Tools, Tips & Debugging Notes
* PHP CLI usage (`php -S`, `php -m`, `php -i`)
* Composer commands (`install`, `update`, `dump-autoload`)
* Debugging with `var_dump`, `dd`, Xdebug, logs
* IDE helpers (PHPStorm, VS Code + extensions)
* Performance profiling basics (Xdebug profiler, Blackfire)
2.5 Real-World Use Cases
2.5.1 Industry Applications
* CMS (WordPress, Drupal)
* E-commerce (WooCommerce, custom carts)
* APIs for mobile apps
2.5.2 Business Applications
* Internal admin dashboards
* Automation scripts (report generation, ETL-style tasks)
* Integration bridges between services
2.5.3 System Integrations
* Working with REST/GraphQL APIs
* Payment gateways, email services, cloud storage
* Webhooks (incoming/outgoing)
---
# 3. QUICK REFERENCE (PHP)

3.1 Cheatsheets
* Syntax & operators
* Common string/array/date functions
* OOP keywords (`class`, `interface`, `trait`, `abstract`, `final`)
* Composer & PSR standards overview
3.2 Snippets
* Connect to DB with PDO (prepared statements)
* Basic router snippet (no framework)
* File upload handler
* JSON API response helper
* Secure password hashing (`password_hash`, `password_verify`)
3.3 Templates
3.3.1 “Prompt” Templates (for you + AI)
* “Refactor this PHP function to be more testable…”
* “Convert this procedural PHP into OOP with classes and interfaces…”
3.3.2 Code Templates
* Class + interface skeletons
* Controller skeleton (index/show/create/store/update/destroy)
* Service + repository pair
3.3.3 Boilerplates
* Minimal plain-PHP MVC mini-framework
* Minimal REST API structure (router → controller → service → repo)
3.4 Condensed Architecture Diagrams
* One-page app structure for:
  * Plain PHP app
  * Laravel-style app
  * API-only backend
3.5 Error & Issue Playbook
3.5.1 Common Errors
* “Undefined index/variable”
* “Headers already sent”
* “Call to undefined function/method”
* “Class not found (autoload)”
3.5.2 Causes
* Typos, missing `isset`, output before headers
* Missing `use` or namespace mismatch
* Composer autoload not updated
3.5.3 Fixes
* Defensive checks (`isset`, `??`)
* Move header/session calls before any output
* Run `composer dump-autoload`, fix namespaces
3.6 Best Practices
3.6.1 Do’s & Don’ts
* DO use prepared statements & escaping
* DO enable strict types in new code
* DO centralize config & avoid hardcoding
* DON’T trust user input
* DON’T mix HTML, SQL, and logic in one giant file

3.6.2 Performance Guidelines
* Use Opcache
* Avoid unnecessary loops/queries
* Cache expensive queries and rendered fragments
* Prefer prepared statements reused in loops
3.6.3 Security Considerations
* Input validation & output escaping
* CSRF protection for forms
* Secure session handling & cookie flags
* Safe file upload patterns

---

# 4. ACTIVE RECALL (3 Sheets – PHP CONTENT)
4.1 Flashcards (Q/A) – PHP-focused **[🗸]**
* Syntax, types, superglobals
* Session model, error vs exception
* Common functions & language constructs

4.2 Quizzes – progressively harder **[🗸]**
4.2.1 Beginner Quiz
* Syntax, variables, arrays, loops
4.2.2 Intermediate Quiz
* OOP, namespaces, Composer, PDO, sessions
4.2.3 Expert Quiz
* Design patterns, framework internals, security edge cases, performance tuning
4.3 Challenges & Exercise
* “Rewrite this insecure code to be secure”
* “Refactor this procedural script into MVC”
* “Add a new feature to an existing mini-app with tests”

4.4 Memory Anchors – PHP-specific **[🗸]**
4.4.1 Analogies
* Composer = npm/poetry
* Controller = receptionist + manager
* PSR-4 = map namespace to folder
4.4.2 Visual Mnemonics
* Diagram of request path with icons
* “Layer cake” picture for architecture
4.4.3 Comparison Tables
* `include` vs `require` vs `_once`
* `==` vs `===`
* Procedural vs OOP vs framework
4.5 Spaced Repetition Plan
4.5.1 Daily Quick Review
* 5–10 flashcards + 1 small snippet to read/write
4.5.2 Weekly Review
* One mini-project improvement or refactor
4.5.3 30-Day Refresh
* Rebuild a previous project from memory w/ improvements
4.5.4 90-Day Mastery Cycle
* Build a new app from scratch (or in a new framework) using all best practices
---
# 5. STAYING CURRENT (PHP ECOSYSTEM)

5.1 New Features & Updates
* Track new PHP versions (8.x, 9.x etc)
* Summaries of key RFCs & new language features
* New core extensions or deprecations
5.2 Breaking Changes & Deprecations
* Removed functions & ini options
* Deprecated syntax patterns
* Framework version upgrade breaking changes
5.3 Migration Guides
* PHP version upgrades (e.g., 7.x → 8.x)
* Framework upgrades (Laravel major versions, Symfony LTS upgrades)
* From plain PHP → framework adoption strategy
5.4 Ecosystem Shifts
* Movement towards strict typing & enums
* Popular frameworks & microframeworks
* PSR standards adoption (logging, HTTP, containers)
5.5 Market & Industry Trends
* Demand for full-stack PHP devs (PHP + JS frontend)
* API-first & headless CMS trends
* PHP in serverless / containers
5.6 Monthly Upgrade Ritual
5.6.1 Update This Document
* Add new functions, patterns, or features you used this month
5.6.2 Refresh Templates
* Update boilerplates with latest best practices (e.g. strict types, enums)
5.6.3 Evaluate New Tools
* IDE plugins, static analyzers (Psalm, PHPStan), testing tools
5.6.4 Archive Outdated Notes
* Mark legacy patterns (like old MySQL API) as “historic only”

---

<p align="center">--- [ 🌷 PHP Learning Garden 🌷 ] ---</p>  

I