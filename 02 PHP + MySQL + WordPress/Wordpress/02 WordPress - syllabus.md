> [!quote] Matt Mullenweg (Co-founder of WordPress, Houston, 1984)  
> **"WordPress is an Open Source project, which means there are hundreds of people all over the world working on it. That also means you're free to use it for anything from your cat's home page to a Fortune 500 website without paying anyone a license fee."**  
> _WordPress democratizes publishing—powering 43% of all websites._

## **1.1 WordPress Core Architecture & Fundamentals**

### **Learning Intent (~95–105 words)**

This section builds a rigorous mental model of WordPress as both a content management system and an extensible application framework. Learners master the WordPress file structure, database schema, template hierarchy, and the distinction between WordPress as a CMS versus a development platform. The objective is not merely to "use WordPress," but to **understand its architecture**—so every customization,

theme, and plugin leverages WordPress's core systems predictably. Strong grounding here enables developers to work with WordPress rather than against it, understanding how requests flow from URL to rendered page, how data is stored and retrieved, and how themes and plugins extend functionality without modifying core files.

|Topic|Focus & Purpose|
|---|---|
|**[[1.1.1 WordPress Installation & Configuration]]**|Installing WordPress (one-click, manual); understanding `wp-config.php` (database credentials, authentication keys, debugging); WordPress file structure (`wp-content`, `wp-includes`, `wp-admin`); multisite basics. Establishes WordPress deployment fundamentals.|
|**[[1.1.2 The WordPress Database]]**|Database structure (`wp_posts`, `wp_postmeta`, `wp_users`, `wp_usermeta`, `wp_options`, `wp_terms`, `wp_term_relationships`); understanding custom tables; database prefix; backup strategies; phpMyAdmin basics. Critical for data management.|
|**[[1.1.3 The WordPress Loop]]**|Understanding the Loop; `have_posts()`, `the_post()`, template tags (`the_title()`, `the_content()`, `the_permalink()`); custom loops with `WP_Query`; loop modifications. Defines content display mechanism.|
|**[[1.1.4 Template Hierarchy]]**|WordPress template file order; conditional tags (`is_home()`, `is_page()`, `is_single()`, `is_archive()`); template parts (`get_header()`, `get_footer()`, `get_template_part()`); understanding template precedence. Critical for theme development.|
|**[[1.1.5 WordPress Request Lifecycle]]**|From URL to rendered page; `wp()` function; query parsing; template loading; action hooks fired during request; understanding the execution flow. Enables advanced customization.|

---

## **1.2 WordPress Hooks System**

### **Learning Intent (~90–100 words)**

Hooks are WordPress's plugin API—the mechanism that makes WordPress infinitely extensible. This section teaches action hooks, filter hooks, hook priorities, and creating custom hooks. Learners master how to modify WordPress behavior without editing core files, understand hook timing and execution order, and leverage WordPress's event-driven architecture. The goal is to write plugins and theme customizations that hook into WordPress at precise moments, modify data in flight, and create extensible code that other developers can hook into—the foundation of professional WordPress development.

|Topic|Focus & Purpose|
|---|---|
|**[[1.2.1 Action Hooks]]**|Understanding actions; `add_action()`, `do_action()`; hook priorities; passing parameters to actions; removing actions (`remove_action()`); common actions (`wp_head`, `wp_footer`, `init`, `wp_enqueue_scripts`). Enables adding functionality at specific points.|
|**[[1.2.2 Filter Hooks]]**|Understanding filters; `add_filter()`, `apply_filters()`; modifying and returning data; filter priorities; removing filters (`remove_filter()`); common filters (`the_content`, `the_title`, `body_class`). Defines data transformation mechanism.|
|**[[1.2.3 Hook Priority & Execution Order]]**|Priority parameter (default 10); execution order of multiple callbacks; early vs late execution; debugging hook order with Query Monitor. Critical for predictable customization.|
|**[[1.2.4 Creating Custom Hooks]]**|Building extensible plugins/themes; `do_action()` for custom actions; `apply_filters()` for custom filters; documenting hooks; namespace conventions. Enables building extensible systems.|
|**[[1.2.5 Common WordPress Hooks]]**|Essential hooks for plugin/theme development; save_post, wp_enqueue_scripts, widgets_init, pre_get_posts, template_redirect; understanding hook documentation. Practical hook reference.|

---

## **1.3 Theme Development**

### **Learning Intent (~95–105 words)**

WordPress themes control presentation—separating content from design. This section teaches theme structure, required files, template hierarchy application, child themes, and modern block theme development. Learners master creating custom themes from scratch, understanding the distinction between classic PHP themes and modern block themes, and implementing responsive, accessible designs. The goal is to build themes that are maintainable, follow WordPress standards, pass theme review requirements, and leverage modern WordPress features like the block editor while respecting user content and maintaining upgradeability.

|Topic|Focus & Purpose|
|---|---|
|**[[1.3.1 Theme Structure & Required Files]]**|Essential files (`style.css`, `index.php`, `functions.php`); theme headers; `screenshot.png`; template files; `template-parts` directory organization. Establishes theme anatomy.|
|**[[1.3.2 Functions.php & Theme Setup]]**|Theme setup hooks (`after_setup_theme`); `add_theme_support()` (custom logo, post thumbnails, HTML5, title-tag); registering navigation menus; content width; internationalization. Defines theme capabilities.|
|**[[1.3.3 Template Files & Hierarchy]]**|Creating template files; page templates; custom post type templates; archive templates; 404, search, attachment templates; applying template hierarchy knowledge. Enables custom layouts.|
|**[[1.3.4 Template Tags & Functions]]**|Post/page functions (`the_title()`, `the_excerpt()`, `the_post_thumbnail()`); navigation (`wp_nav_menu()`); pagination (`paginate_links()`, `the_posts_pagination()`); WordPress utilities. Essential theme functions.|
|**[[1.3.5 Child Themes]]**|Creating child themes; inheriting parent theme functionality; overriding templates; enqueueing child theme styles; when to use child themes vs custom themes. Preserves parent theme updates.|
|**[[1.3.6 Enqueuing Stylesheets & Scripts]]**|`wp_enqueue_style()`, `wp_enqueue_script()`; dependencies; versioning; conditional loading; `wp_localize_script()` for passing data to JavaScript. Critical for proper resource loading.|
|**[[1.3.7 Custom Post Types & Taxonomies]]**|Registering custom post types (`register_post_type()`); custom taxonomies (`register_taxonomy()`); archive templates; rewrite rules; when to use custom content types. Extends content model.|
|**[[1.3.8 Block Themes (FSE)]]**|Full Site Editing (FSE); `theme.json` configuration; block patterns; template parts as blocks; Global Styles; transitioning from classic to block themes. Modern WordPress theme development.|

---

## **1.4 Plugin Development**

### **Learning Intent (~90–100 words)**

Plugins extend WordPress functionality without modifying core or theme files. This section teaches plugin architecture, WordPress APIs, database interaction from plugins, security best practices, and distribution. Learners master building feature-complete plugins that integrate seamlessly with WordPress, follow coding standards, implement proper security measures, and provide user-friendly settings interfaces. The goal is to create plugins that are modular, testable, translatable, and follow WordPress plugin development best practices for public distribution or client projects.

|Topic|Focus & Purpose|
|---|---|
|**[[1.4.1 Plugin Structure & Headers]]**|Plugin file headers (Name, Description, Version, Author); single-file vs directory plugins; `readme.txt` format; plugin activation/deactivation hooks; uninstall cleanup. Establishes plugin basics.|
|**[[1.4.2 Plugin Activation & Deactivation]]**|`register_activation_hook()`, `register_deactivation_hook()`; database table creation; setting default options; cleanup strategies; `uninstall.php` for complete removal. Manages plugin lifecycle.|
|**[[1.4.3 Options API]]**|Storing plugin settings; `add_option()`, `get_option()`, `update_option()`, `delete_option()`; serializing arrays; autoloading options; best practices for option names. Essential for configuration.|
|**[[1.4.4 Settings API]]**|Creating admin settings pages; `register_setting()`, `add_settings_section()`, `add_settings_field()`; `settings_fields()`, `do_settings_sections()`; form validation and sanitization. Builds user-friendly settings.|
|**[[1.4.5 Shortcode API]]**|Creating shortcodes (`add_shortcode()`); shortcode attributes; enclosing vs self-closing shortcodes; security considerations; removing shortcodes; modern alternatives (blocks). Enables content insertion.|
|**[[1.4.6 Widgets API]]**|Creating custom widgets; extending `WP_Widget` class; widget forms; saving widget data; displaying widgets; registering widget areas; modern block-based widgets. Adds sidebar functionality.|
|**[[1.4.7 AJAX in WordPress]]**|WordPress AJAX architecture; `admin-ajax.php`; `wp_ajax_*` hooks; nonces for security; `wp_localize_script()` for AJAX URLs; frontend vs admin AJAX; REST API as modern alternative. Enables dynamic updates.|
|**[[1.4.8 Plugin Security]]**|Input validation and sanitization; output escaping; nonce verification; capability checks; SQL injection prevention with `$wpdb->prepare()`; XSS prevention; CSRF protection. Critical security practices.|

---

## **1.5 WordPress REST API**

### **Learning Intent (~85–95 words)**

The WordPress REST API transforms WordPress into a headless CMS and enables JavaScript-driven interfaces. This section teaches API endpoints, authentication, custom endpoints, and building decoupled applications. Learners master consuming WordPress data via HTTP requests, extending the API with custom routes, implementing authentication for protected endpoints, and understanding when REST API is appropriate versus traditional WordPress development. The goal is to build modern, API-driven WordPress applications and integrate WordPress with external services and JavaScript frameworks.

|Topic|Focus & Purpose|
|---|---|
|**[[1.5.1 REST API Basics]]**|API discovery (`/wp-json`); default endpoints (`/wp/v2/posts`, `/wp/v2/pages`, `/wp/v2/users`); understanding JSON responses; authentication overview; testing with Postman/cURL. Establishes API fundamentals.|
|**[[1.5.2 Consuming the REST API]]**|Making GET requests; filtering and querying (`?per_page`, `?search`, `?_embed`); pagination; handling responses; error handling; CORS considerations. Enables data retrieval.|
|**[[1.5.3 Authentication]]**|Authentication methods (application passwords, OAuth, JWT); cookie authentication for logged-in users; bearer tokens; securing endpoints; user capabilities and permissions. Critical for protected data.|
|**[[1.5.4 Custom Endpoints]]**|Registering custom routes (`register_rest_route()`); route namespaces; methods (GET, POST, PUT, DELETE); permission callbacks; schema definition; argument validation. Extends API functionality.|
|**[[1.5.5 REST API for Custom Post Types]]**|Exposing custom post types via API (`show_in_rest`); custom endpoints for CPTs; custom fields in API responses; `register_rest_field()`; modifying API responses. Integrates custom content.|
|**[[1.5.6 Headless WordPress]]**|WordPress as backend CMS; decoupling frontend (React, Vue, Next.js); GraphQL alternatives (WPGraphQL); pros/cons of headless architecture; when headless makes sense. Modern WordPress architecture.|

---

## **1.6 WooCommerce Development**

### **Learning Intent (~100–110 words)**

WooCommerce transforms WordPress into a powerful e-commerce platform. This section teaches WooCommerce architecture, hooks specific to e-commerce, product customization, payment gateway integration, and extending WooCommerce functionality. Learners master the WooCommerce template system, understand cart and checkout customization, work with orders and customer data, and build WooCommerce extensions. The goal is to create custom e-commerce solutions, integrate third-party services, modify the shopping experience, and build WooCommerce-compatible plugins—skills essential for e-commerce WordPress developers as WooCommerce powers 28% of all online stores globally.

|Topic|Focus & Purpose|
|---|---|
|**[[1.6.1 WooCommerce Architecture]]**|WooCommerce file structure; custom post types (product, shop_order); taxonomies (product_cat, product_tag); database tables (`wc_orders`, `wc_order_items`); HPOS (High-Performance Order Storage); understanding WooCommerce's relationship to WordPress. Establishes WooCommerce fundamentals.|
|**[[1.6.2 WooCommerce Template System]]**|Template hierarchy specific to WooCommerce; overriding templates in themes (`woocommerce/` directory); `woocommerce_template_*` hooks; customizing product loops; shop, cart, checkout, account templates. Enables custom store layouts.|
|**[[1.6.3 WooCommerce Hooks & Filters]]**|Essential WooCommerce actions (`woocommerce_before_shop_loop`, `woocommerce_after_add_to_cart_button`); filters (`woocommerce_product_get_price`, `woocommerce_add_to_cart_validation`); hook reference resources; customizing product display and behavior. Critical for customization.|
|**[[1.6.4 Product Customization]]**|Adding custom product fields; product meta data; virtual/downloadable products; variable products and variations; grouped and external products; custom product types. Extends product capabilities.|
|**[[1.6.5 Cart & Checkout Customization]]**|Modifying cart items; custom checkout fields; checkout validation; order notes; payment and shipping customization; thank you page modifications; block-based checkout (modern WooCommerce). Tailors purchase flow.|
|**[[1.6.6 Order Management]]**|Accessing order data (`WC_Order` class); custom order statuses; order meta data; bulk order actions; order emails customization; refunds and order modifications. Manages transactions.|
|**[[1.6.7 Payment Gateway Integration]]**|Creating custom payment gateways; extending `WC_Payment_Gateway` class; processing payments; handling IPN/webhooks; PCI compliance considerations; testing with sandbox environments. Enables payment processing.|
|**[[1.6.8 WooCommerce REST API]]**|WooCommerce-specific endpoints; authentication for WooCommerce API; managing products, orders, customers via API; webhooks for real-time updates; integrating external systems. Connects WooCommerce to services.|
|**[[1.6.9 Block-Based WooCommerce]]**|Cart and checkout blocks; product blocks; customizing block-based checkout; block extensibility; migrating from shortcodes to blocks; modern WooCommerce development (2024+). Future of WooCommerce themes.|

---

## **1.7 WordPress Security & Best Practices**

### **Learning Intent (~85–95 words)**

Security is non-negotiable in WordPress development. This section teaches common vulnerabilities, security hardening, secure coding practices, and WordPress-specific security measures. Learners master input validation, output escaping, nonce verification, capability checking, and understand the OWASP Top 10 as applied to WordPress. The goal is to write secure code by default, recognize security anti-patterns, implement defense-in-depth strategies, and maintain WordPress sites securely—protecting user data, preventing unauthorized access, and building trustworthy applications.

|Topic|Focus & Purpose|
|---|---|
|**[[1.7.1 Common WordPress Vulnerabilities]]**|SQL injection; XSS (Cross-Site Scripting); CSRF (Cross-Site Request Forgery); authentication bypass; file inclusion vulnerabilities; understanding WordPress-specific attack vectors. Identifies threats.|
|**[[1.7.2 Secure Coding Practices]]**|Input validation (`sanitize_text_field()`, `absint()`, `esc_url_raw()`); output escaping (`esc_html()`, `esc_attr()`, `esc_url()`, `wp_kses()`); prepared statements (`$wpdb->prepare()`); nonce verification (`wp_verify_nonce()`). Critical for secure development.|
|**[[1.7.3 User Capabilities & Permissions]]**|WordPress roles and capabilities; `current_user_can()` checks; role management; custom capabilities; least privilege principle; protecting admin functionality. Enforces authorization.|
|**[[1.7.4 WordPress Hardening]]**|Strong passwords; limiting login attempts; two-factor authentication; disabling file editing; securing `wp-config.php`; database security; SSL/HTTPS; security headers; firewall and malware scanning. Protects installations.|
|**[[1.7.5 Security Plugins & Monitoring]]**|Security plugins (Wordfence, Sucuri, iThemes Security); security scanning; activity logging; uptime monitoring; backup strategies; disaster recovery. Maintains security posture.|

---

## **1.8 Performance Optimization**

### **Learning Intent (~80–90 words)**

Fast WordPress sites improve user experience and SEO. This section teaches caching strategies, database optimization, asset optimization, and measuring performance. Learners master identifying performance bottlenecks, implementing caching layers, optimizing images and code, and leveraging CDNs. The goal is to build WordPress sites that load quickly regardless of traffic levels, understand Core Web Vitals, optimize database queries, and implement performance best practices from development through production.

|Topic|Focus & Purpose|
|---|---|
|**[[1.8.1 Caching Strategies]]**|Page caching; object caching (Redis, Memcached); browser caching; opcode caching (OPcache); cache plugins (WP Rocket, W3 Total Cache); cache invalidation strategies. Reduces server load.|
|**[[1.8.2 Database Optimization]]**|Optimizing queries; query caching; database indexing; cleaning post revisions and transients; `WP_Query` optimization; avoiding expensive queries (`meta_query`, `tax_query`). Improves query performance.|
|**[[1.8.3 Asset Optimization]]**|Image optimization (compression, lazy loading, WebP); minification and concatenation of CSS/JS; critical CSS; deferring JavaScript; font optimization; sprite sheets. Reduces page weight.|
|**[[1.8.4 CDN & Hosting]]**|Content Delivery Networks; choosing WordPress hosting (shared, VPS, managed, cloud); server-level optimizations (PHP version, HTTP/2, Gzip); database server separation. Improves global delivery.|
|**[[1.8.5 Performance Measurement]]**|Google PageSpeed Insights; GTmetrix; WebPageTest; Core Web Vitals (LCP, FID, CLS); Query Monitor plugin; profiling with Xdebug; establishing performance budgets. Enables measurement and iteration.|

---

# **SECTION 2 — APPLIED (Real-World WordPress Development)**

## **Learning Intent (~90–100 words)**

This section converts theory into practical WordPress project delivery. Learners build complete WordPress sites, implement common client requirements, work with page builders, manage content workflows, and handle multisite installations. The focus shifts from isolated concepts to integrated solutions—building business websites, blogs, membership sites, and custom web applications. Applied mastery means delivering production-ready WordPress projects that are secure, performant, maintainable, and meet real business requirements while following industry best practices.

### **2.1 Complete Site Development**

|Area|Purpose|
|---|---|
|**2.1.1 Business Website Development**|About, services, portfolio, contact pages; team member displays; testimonials; lead capture forms; SEO implementation; Google Analytics integration; GDPR compliance.|
|**2.1.2 Blog & Content Site**|Blog layouts; category/tag archives; related posts; comments management; social sharing; email subscriptions; content delivery optimization; editorial workflows.|
|**2.1.3 E-commerce Store**|WooCommerce setup; product catalogs; shopping cart; checkout flow; payment gateways; shipping configuration; inventory management; order fulfillment.|
|**2.1.4 Membership Site**|Member registration; content restriction; subscription levels; member directories; private messaging; membership plugins (MemberPress, Restrict Content Pro).|

### **2.2 Page Builders & Visual Development**

|Area|Purpose|
|---|---|
|**2.2.1 Block Editor (Gutenberg)**|Creating custom blocks with React; block patterns; reusable blocks; Full Site Editing; `block.json`; block variations; InnerBlocks; block editor extensibility.|
|**2.2.2 Elementor Integration**|Elementor custom widgets; dynamic content; theme builder; custom CSS/JavaScript in Elementor; Elementor API; building Elementor-compatible themes.|
|**2.2.3 Other Page Builders**|Beaver Builder; Divi Builder; WPBakery Page Builder; understanding page builder architecture; performance considerations; lock-in avoidance.|

### **2.3 Advanced WordPress Features**

|Area|Purpose|
|---|---|
|**2.3.1 Multisite**|WordPress Multisite setup; network admin; site creation; plugins and themes in multisite; domain mapping; multisite-specific hooks; when multisite is appropriate.|
|**2.3.2 Custom Fields (ACF)**|Advanced Custom Fields plugin; field types; field groups; displaying custom fields in themes; ACF blocks; ACF with REST API; flexible content; repeater fields.|
|**2.3.3 Multilingual Sites**|WPML; Polylang; multilingual site architecture; translating themes and plugins; language switchers; hreflang implementation; multilingual SEO.|

### **2.4 WordPress SEO & Marketing**

|Area|Purpose|
|---|---|
|**2.4.1 SEO Fundamentals**|Yoast SEO or Rank Math; meta titles and descriptions; XML sitemaps; schema markup; permalink structure; canonical URLs; robots.txt; redirects.|
|**2.4.2 Content Marketing**|Lead magnets; email list building; newsletter integration; content upgrades; social media integration; analytics and conversion tracking.|

### **2.5 Mini Projects (WordPress-Focused)**

|Level|Project|Outcome|
|---|---|---|
|Beginner|Custom theme for personal blog|Reinforces theme structure, template hierarchy, loop, template tags, custom post types.|
|Intermediate|Business site with custom plugin|Builds plugin development, hooks, settings API, custom functionality, contact forms.|
|Advanced|WooCommerce store with custom features|Applies WooCommerce customization, payment integration, product customization, checkout flow.|
|Production|Multisite network with membership|Applies multisite, user roles, content restriction, performance optimization, security hardening.|

---

# **SECTION 3 — PROFESSIONAL (Standards, Tooling & Ecosystem)**

## **Learning Intent (~85–95 words)**

This section positions WordPress within professional development workflows. Learners master WordPress Coding Standards, version control with Git, local development environments, deployment workflows, and working with WordPress at enterprise scale. The goal is long-term competence—writing code that passes WordPress.org review, collaborating effectively on teams, using modern development tools, implementing CI/CD pipelines, and understanding the WordPress ecosystem of hosting, plugins, and services. Professional WordPress developers know not just how to build, but how to maintain, scale, and deliver WordPress projects reliably.

|Area|Focus|
|---|---|
|**3.1 WordPress Coding Standards**|PHP, HTML, CSS, JavaScript coding standards; PHPCS (PHP_CodeSniffer) with WordPress rulesets; automated linting; code review practices; documentation standards (PHPDoc).|
|**3.2 Local Development Environments**|Local by Flywheel; XAMPP/MAMP; Docker (official WordPress Docker images); Lando; DevKinsta; version consistency across environments; database syncing.|
|**3.3 Version Control & Collaboration**|Git for WordPress; `.gitignore` for WordPress projects; branching strategies; managing `wp-content` vs core; database version control challenges; team workflows.|
|**3.4 Deployment & DevOps**|WP-CLI (WordPress Command Line Interface); automated deployments; staging environments; database migrations; environment-specific configurations; Continuous Integration/Continuous Deployment (CI/CD).|
|**3.5 Testing WordPress**|Unit testing with PHPUnit; integration testing; acceptance testing; WordPress test suite; mocking WordPress functions; test-driven development in WordPress.|
|**3.6 Debugging & Development Tools**|Query Monitor; Debug Bar; `WP_DEBUG` constants; error logging; Xdebug; browser DevTools for theme/plugin debugging; profiling performance.|
|**3.7 WordPress at Scale**|Enterprise WordPress architecture; load balancing; database replication; horizontal scaling; caching strategies at scale; media offloading (S3, CDN); monitoring and alerting.|

---

# **SECTION 4 — REFERENCE & ECOSYSTEM CONTEXT**

## **Learning Intent (~75–85 words)**

This section defines WordPress's boundaries, ecosystem position, and evolution. Learners understand WordPress.com vs WordPress.org, the GPL license implications, the WordPress community, and where WordPress fits in the CMS landscape. Understanding WordPress's history, market dominance (43% of all websites), open-source philosophy, and future direction (Full Site Editing, Gutenberg) enables informed technology decisions, contribution to the community, and positioning WordPress appropriately for client projects and career development.

|Area|Focus|
|---|---|
|**4.1 WordPress.com vs WordPress.org**|Hosted vs self-hosted WordPress; WordPress.com plans and limitations; Automattic's role; WP Engine and managed WordPress hosting; when to recommend which platform.|
|**4.2 WordPress Ecosystem**|Plugin directory (60,000+ plugins); theme directory (10,000+ themes); premium marketplaces (ThemeForest, CodeCanyon); WordPress-focused hosting; support resources; WordCamps and meetups.|
|**4.3 GPL & Open Source**|GNU General Public License v2; theme and plugin licensing; derivative works; commercial themes/plugins under GPL; split licensing; contributing to WordPress Core.|
|**4.4 WordPress Evolution**|WordPress history (b2/cafelog to present); major versions (2.7 admin redesign, 3.0 multisite, 4.0 media, 5.0 Gutenberg); current roadmap; Full Site Editing; Phase 3 (Collaboration); Future of WordPress.|
|**4.5 WordPress vs Alternatives**|WordPress vs Drupal, Joomla; WordPress vs headless CMS (Contentful, Strapi); WordPress vs site builders (Wix, Squarespace); when WordPress is appropriate; WordPress market share and trends.|
|**4.6 Contributing to WordPress**|Contributing to Core (Trac, GitHub); contributing to Codex/documentation; theme/plugin reviews; translating WordPress; organizing WordCamps; Make WordPress teams (Core, Design, Mobile, etc.).|
|**4.7 Career Paths in WordPress**|WordPress developer roles; freelancing vs agency vs in-house; specializations (theme developer, plugin developer, WooCommerce developer); rates and market demand; building a WordPress portfolio; certifications and learning resources.|

---