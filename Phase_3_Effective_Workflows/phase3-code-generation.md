# Phase 3: Code Generation with AI

**Goal**: Master AI-powered code generation techniques for maximum productivity and quality  
**Prerequisites**: Complete Phase 1 (Foundations) and Phase 2 (Prompt Engineering)

---

## Table of Contents
1. [Understanding AI Code Generation](#understanding-ai-code-generation)
2. [What Impacts Code Generation Quality](#what-impacts-code-generation-quality)
3. [Best Practices for High-Quality Generation](#best-practices-for-high-quality-generation)
4. [Generating Boilerplate and Scaffolding](#generating-boilerplate-and-scaffolding)
5. [Creating Functions from Comments](#creating-functions-from-comments)
6. [Generating Test Cases](#generating-test-cases)
7. [Creating Configuration Files](#creating-configuration-files)
8. [Common Pitfalls and Solutions](#common-pitfalls-and-solutions)
9. [Hands-On Exercises](#hands-on-exercises)

---

## Understanding AI Code Generation

### When to Use AI Code Generation

**✅ Excellent Use Cases:**
- Boilerplate code (CRUD operations, API endpoints, model definitions)
- Repetitive patterns (similar functions with variations)
- Standard implementations (authentication, validation, error handling)
- Test case generation (unit tests, integration tests)
- Configuration files (JSON, YAML, environment configs)
- Data transformations and mappings
- Type definitions and interfaces
- Documentation scaffolding

**⚠️ Use with Caution:**
- Complex business logic (requires deep domain understanding)
- Security-critical code (authentication, encryption, authorization)
- Performance-sensitive algorithms (needs optimization and profiling)
- Novel algorithms (AI may hallucinate or use incorrect approaches)

**❌ Avoid AI Generation For:**
- Code you don't understand (learning anti-pattern)
- Production secrets or credentials
- Untested critical paths
- When you're learning a new concept (write it yourself first)

### How AI Code Generation Works

AI code generation relies on:
1. **Context**: Files in your workspace, open tabs, conversation history
2. **Patterns**: Common coding patterns from training data
3. **Prompt Quality**: Specificity and clarity of your request
4. **Language/Framework Knowledge**: AI's familiarity with your tech stack
5. **Examples**: Code samples you provide in the conversation

**Context Window Matters**: AI can "see" approximately:
- ~200K tokens (Claude Sonnet 4.5)
- Your recent conversation
- Referenced files
- Workspace structure

---

## What Impacts Code Generation Quality

### 1. Prompt Specificity

**Poor Prompt** ❌
```
Create a user function
```

**Good Prompt** ✅
```
Create a TypeScript function that validates user registration data.

Requirements:
- Accept { email, password, age } as parameters
- Email: must be valid format
- Password: min 8 chars, 1 uppercase, 1 number, 1 special char
- Age: must be 18+
- Return { isValid: boolean, errors: string[] }
- Use regex for validation
- No external libraries
```

**Impact**: Specific prompts reduce iterations by 70-80%

### 2. Context Provision

**Without Context** ❌
```
Add error handling to the API
```

**With Context** ✅
```
Add error handling to the API endpoints in src/routes/users.ts

Use our existing error types from src/utils/errors.ts:
- ValidationError (400)
- NotFoundError (404)
- ServerError (500)

Follow the pattern used in src/routes/products.ts (lines 24-45)
```

**Impact**: Context reduces incorrect implementations by ~60%

### 3. Tech Stack Clarity

**Vague** ❌
```
Create a database connection
```

**Clear** ✅
```
Create a PostgreSQL database connection using node-postgres (pg).

Requirements:
- Use connection pooling
- Environment variables: DB_HOST, DB_PORT, DB_NAME, DB_USER, DB_PASSWORD
- Export a singleton pool instance
- Add error handling with retry logic (max 3 retries)
- TypeScript with proper types
- Add connection health check function
```

**Impact**: Tech stack clarity prevents wrong library/approach ~80% of time

### 4. Existing Code Examples

**No Example** ❌
```
Create an API endpoint
```

**With Example** ✅
```
Create an API endpoint for deleting users.

Follow the pattern from our existing GET endpoint:

// Existing pattern
router.get('/users/:id', async (req, res) => {
  try {
    const user = await userService.findById(req.params.id);
    if (!user) throw new NotFoundError('User not found');
    res.json({ success: true, data: user });
  } catch (error) {
    handleError(error, res);
  }
});

Create similar DELETE endpoint following this structure.
```

**Impact**: Examples ensure consistency and reduce style mismatches by ~90%

### 5. Constraints and Limitations

**No Constraints** ❌
```
Create a sorting function
```

**With Constraints** ✅
```
Create a sorting function for product listings.

Constraints:
- Must work with arrays up to 10,000 items
- Sort by: price (ascending/descending), name (A-Z), date (newest first)
- Time complexity: O(n log n) or better
- No external libraries (use native JavaScript)
- Return new array (don't mutate original)
- Type-safe (TypeScript generics)
- Max 40 lines of code
```

**Impact**: Constraints guide AI toward optimal solutions and prevent over-engineering

---

## Best Practices for High-Quality Generation

### Practice 1: Use the "Specification First" Approach

**Step-by-Step Process:**

1. **Write the specification** (before generating code)
   ```
   I need a user authentication middleware.
   
   Specification:
   - Extract JWT token from Authorization header
   - Verify token signature and expiration
   - Attach user object to req.user
   - Return 401 if unauthorized
   - Use jsonwebtoken library
   - TypeScript with Express types
   ```

2. **Generate the code**
   ```
   Generate the authentication middleware based on the spec above.
   ```

3. **Request explanation** (if needed)
   ```
   Explain how the token verification works and potential edge cases.
   ```

4. **Iterate and refine**
   ```
   Add rate limiting to prevent brute force attacks.
   ```

**Why This Works**: Separating specification from implementation forces clarity

### Practice 2: Generate in Layers

**Instead of**: "Create a complete user management system"

**Do This**:
1. Generate data models first
   ```
   Create TypeScript interfaces for User, UserProfile, and UserPreferences
   ```

2. Generate database schema
   ```
   Create PostgreSQL schema for users table based on the interfaces above
   ```

3. Generate services layer
   ```
   Create UserService class with CRUD operations using the schema
   ```

4. Generate API endpoints
   ```
   Create REST API endpoints using UserService
   ```

5. Generate tests
   ```
   Create unit tests for UserService methods
   ```

**Benefit**: Each layer can reference previous layers, ensuring consistency

### Practice 3: Provide Negative Examples

Tell AI what **not** to do:

```
Create a login endpoint.

DO:
- Use bcrypt for password comparison
- Return JWT token on success
- Rate limit to 5 attempts per minute

DON'T:
- Store passwords in plain text
- Use MD5 or SHA1 for hashing
- Return sensitive user data in token
- Include password in response
- Use synchronous bcrypt methods
```

**Impact**: Prevents common security mistakes and anti-patterns

### Practice 4: Request Multiple Approaches

```
Generate a caching solution for API responses.

Provide two approaches:
1. In-memory caching (for single server)
2. Redis-based caching (for distributed system)

Compare trade-offs: performance, complexity, scalability
```

**Benefit**: Helps you choose the best solution for your context

### Practice 5: Specify Output Format

```
Create validation functions for user input.

Output format:
- Each validator as a separate function
- Use descriptive function names (validateEmail, validatePassword)
- Include JSDoc comments with examples
- Export all functions at the end
- Include unit tests in comments
```

**Impact**: Ensures code is immediately usable and well-documented

### Practice 6: Use Incremental Refinement

**Start Simple**:
```
Create a basic HTTP server in Node.js with Express
```

**Add Features One by One**:
```
1. Add middleware for JSON parsing and CORS
2. Add two endpoints: GET /health and POST /users
3. Add error handling middleware
4. Add request logging with Morgan
5. Add environment-based configuration
```

**Benefit**: Easier to understand, test, and debug each addition

---

## Generating Boilerplate and Scaffolding

### Effective Patterns

#### 1. Project Initialization

**Prompt Template**:
```
Create a [project type] starter with:
- [Technology stack]
- Directory structure
- Basic configuration files
- Essential dependencies
- README with setup instructions

Project type: REST API
Stack: Node.js, Express, TypeScript, PostgreSQL, Jest
```

**Example Output Structure**:
```
project/
├── src/
│   ├── config/
│   │   └── database.ts
│   ├── models/
│   ├── routes/
│   ├── services/
│   ├── middleware/
│   ├── utils/
│   └── server.ts
├── tests/
├── .env.example
├── tsconfig.json
├── package.json
└── README.md
```

#### 2. CRUD Operations

**High-Quality Prompt**:
```
Generate complete CRUD operations for a Product resource.

Stack: Express + TypeScript + PostgreSQL + Zod validation

Include:
1. TypeScript interface (Product)
2. Zod validation schema
3. Database queries (using pg library)
4. Service layer with business logic
5. REST endpoints (routes)
6. Error handling
7. API response types

Product fields:
- id (UUID, auto-generated)
- name (string, required, max 100 chars)
- description (string, optional)
- price (number, min 0)
- stock (integer, min 0)
- createdAt (timestamp)
- updatedAt (timestamp)

Follow RESTful conventions.
```

#### 3. Model/Entity Scaffolding

**Prompt Pattern**:
```
Generate a [Model Name] entity with:
- [List of fields with types and constraints]
- [Relationships to other entities]
- [Methods needed]
- [Validation rules]

Use [ORM/Framework] conventions.

Example:
Generate a BlogPost entity with:
- id (auto-increment primary key)
- title (string, max 200 chars, required)
- content (text, required)
- authorId (foreign key to User, required)
- categoryId (foreign key to Category, optional)
- status (enum: draft, published, archived)
- publishedAt (datetime, nullable)
- createdAt, updatedAt (timestamps)

Methods:
- publish() - sets status to published and publishedAt to now
- archive() - sets status to archived
- isPublished() - returns boolean

Use Prisma ORM conventions.
```

#### 4. Configuration Files

**Effective Approach**:
```
Create configuration files for a [project type] project.

Include:
1. TypeScript config (strict mode, path aliases)
2. ESLint config (Airbnb style with customizations)
3. Prettier config (2-space indent, single quotes, trailing commas)
4. Jest config (coverage thresholds: 80%)
5. Docker config (multi-stage build)
6. .env.example (all required environment variables)

Project: Node.js REST API with PostgreSQL
```

### Boilerplate Generation Best Practices

✅ **DO:**
- Specify exact versions or version ranges
- Request comments explaining configuration choices
- Ask for consistency with existing project conventions
- Request .example files for sensitive configs
- Generate documentation alongside code

❌ **DON'T:**
- Generate boilerplate without understanding it
- Skip reviewing security implications
- Copy-paste without adapting to your project
- Generate outdated patterns (ask for modern approaches)

---

## Creating Functions from Comments

### The Comment-to-Code Pattern

This is one of AI's strongest capabilities. Here's how to maximize quality:

### Pattern 1: Detailed Comment → Function

**Low-Quality Comment** ❌
```javascript
// validate user
```

**High-Quality Comment** ✅
```typescript
/**
 * Validates user registration data
 * 
 * Requirements:
 * - Email must be valid format (RFC 5322 compliant)
 * - Password: min 8 chars, 1 uppercase, 1 lowercase, 1 number, 1 special char
 * - Username: 3-20 chars, alphanumeric and underscore only
 * - Age: must be 18 or older
 * 
 * @param email - User's email address
 * @param password - User's password
 * @param username - User's chosen username
 * @param age - User's age in years
 * 
 * @returns Object with isValid boolean and array of error messages
 * 
 * @example
 * validateUserRegistration('test@example.com', 'Pass123!', 'john_doe', 25)
 * // Returns: { isValid: true, errors: [] }
 * 
 * @example
 * validateUserRegistration('invalid', 'weak', 'ab', 15)
 * // Returns: { isValid: false, errors: ['Invalid email', 'Weak password', ...] }
 */
```

**Then prompt**: "Implement this function based on the JSDoc comment"

### Pattern 2: Algorithm Description → Implementation

**Effective Comment Structure**:
```typescript
/**
 * Merge two sorted arrays into one sorted array
 * 
 * Algorithm:
 * 1. Use two pointers, one for each array
 * 2. Compare elements at pointers
 * 3. Add smaller element to result and advance that pointer
 * 4. Continue until one array is exhausted
 * 5. Append remaining elements from the other array
 * 
 * Time complexity: O(n + m) where n, m are array lengths
 * Space complexity: O(n + m) for the result array
 * 
 * @param arr1 - First sorted array
 * @param arr2 - Second sorted array
 * @returns New sorted array containing all elements
 * 
 * Edge cases to handle:
 * - Empty arrays
 * - Arrays with duplicate values
 * - Arrays of different lengths
 */
function mergeSortedArrays<T>(arr1: T[], arr2: T[]): T[] {
  // Generate implementation here
}
```

**Prompt**: "Implement mergeSortedArrays following the algorithm in the comment"

### Pattern 3: Business Logic Description → Code

```typescript
/**
 * Calculate shipping cost for an order
 * 
 * Business Rules:
 * - Base rate: $5.00
 * - Free shipping for orders over $100
 * - Express shipping (2-3 days): +$10
 * - Standard shipping (5-7 days): base rate only
 * - International: +$15
 * - Heavy items (>20kg): +$2 per kg over 20kg
 * - Fragile items: +$3 insurance
 * 
 * @param orderTotal - Total order value in dollars
 * @param weight - Package weight in kg
 * @param isInternational - Whether shipping internationally
 * @param isFragile - Whether package contains fragile items
 * @param shippingSpeed - 'standard' or 'express'
 * 
 * @returns Shipping cost in dollars (rounded to 2 decimals)
 * 
 * @example
 * calculateShipping(150, 5, false, false, 'standard') // Returns: 0 (free shipping)
 * calculateShipping(80, 25, true, true, 'express')    // Returns: 45 (5+10+15+10+3)
 */
```

**Prompt**: "Implement calculateShipping with full error handling and validation"

### Comment-to-Code Best Practices

**Make Comments Comprehensive**:
- Include all requirements and constraints
- Specify edge cases to handle
- Provide examples (especially edge cases)
- Document expected errors and how to handle them
- Include complexity requirements if relevant
- Specify return types and formats

**Use Type Annotations**:
```typescript
// ✅ Good: Type information provided
function processUserData(
  userData: { id: string; name: string; email: string; age: number },
  options: { validateEmail: boolean; checkAge: boolean }
): { success: boolean; data?: ProcessedUser; error?: string }

// ❌ Bad: No type information
function processUserData(userData, options)
```

**Provide Context**:
```typescript
// ✅ Include context about the system
/**
 * Authenticate user and generate session token
 * 
 * Context:
 * - We use JWT tokens stored in httpOnly cookies
 * - Token expiration: 7 days
 * - Refresh tokens stored in database (tokens table)
 * - Failed attempts are rate-limited (5 per 15 min)
 * 
 * Related files:
 * - src/utils/jwt.ts - Token utilities
 * - src/middleware/rateLimiter.ts - Rate limiting
 * - src/models/User.ts - User model with password hash
 */
```

---

## Generating Test Cases

### Test Generation Strategies

#### Strategy 1: Generate Tests from Implementation

**After writing/generating a function**:
```
Generate comprehensive unit tests for the calculateShipping function.

Requirements:
- Use Jest framework
- Test all business rules from the function comments
- Include edge cases: zero values, negative values, null/undefined
- Test boundary conditions (e.g., exactly $100 order)
- Mock external dependencies if any
- Aim for 100% code coverage
- Use descriptive test names (readable as sentences)
- Group related tests with describe blocks
```

#### Strategy 2: Test-First Approach (TDD)

**Before writing implementation**:
```
Create Jest tests for a user registration function that should:
- Validate email format
- Check password strength (min 8 chars, uppercase, number, special char)
- Ensure username is unique (mock database check)
- Return success/error response

Generate tests ONLY (no implementation).
Use test names that describe expected behavior.
```

Then: "Now implement the function to pass these tests"

#### Strategy 3: Coverage-Based Test Generation

```
Analyze this function and generate tests for uncovered scenarios:

[paste your function]

Current coverage: 65%
Uncovered branches:
- Error handling for invalid input types
- Edge case: array with single element
- Edge case: empty string values

Generate additional tests to reach 90%+ coverage.
```

### Test Quality Factors

**High-Quality Test Characteristics**:

1. **Descriptive Names**
   ```typescript
   // ✅ Good
   it('should return free shipping for orders over $100', () => {})
   it('should add $10 for express shipping', () => {})
   it('should reject negative weights with error', () => {})
   
   // ❌ Bad
   it('test 1', () => {})
   it('should work', () => {})
   it('shipping test', () => {})
   ```

2. **Arranged in AAA Pattern**
   ```typescript
   it('should calculate shipping with multiple fees', () => {
     // Arrange
     const orderTotal = 80;
     const weight = 25;
     const isInternational = true;
     const isFragile = true;
     const shippingSpeed = 'express';
     
     // Act
     const result = calculateShipping(orderTotal, weight, isInternational, isFragile, shippingSpeed);
     
     // Assert
     expect(result).toBe(43);
   });
   ```

3. **Test One Thing**
   ```typescript
   // ✅ Good: Each test focuses on one scenario
   it('should apply free shipping for orders over $100', () => {})
   it('should add express shipping fee', () => {})
   
   // ❌ Bad: Testing multiple unrelated things
   it('should handle shipping and also validate email format', () => {})
   ```

### Test Generation Prompts

**Comprehensive Test Suite**:
```
Generate a complete test suite for [function/class name].

Framework: [Jest/Mocha/etc]

Include:
1. Happy path tests (expected usage)
2. Edge cases (boundary values, empty inputs, single items)
3. Error cases (invalid inputs, null/undefined, wrong types)
4. Integration scenarios (if applicable)
5. Performance tests (if relevant)

Use:
- Descriptive test names
- Arrange-Act-Assert pattern
- Mocks for [list dependencies to mock]
- Test data factories for complex objects

Target coverage: 90%+
```

**Specific Test Type**:
```
Generate integration tests for the User API endpoints.

Endpoints to test:
- POST /api/users (create user)
- GET /api/users/:id (get user)
- PUT /api/users/:id (update user)
- DELETE /api/users/:id (delete user)

Test scenarios:
- Successful operations
- Validation errors (invalid email, short password, etc.)
- Authentication/authorization (401, 403)
- Not found errors (404)
- Database errors (500)

Use:
- Supertest for HTTP requests
- Test database with setup/teardown
- Factory functions for test data
- Test each endpoint in separate describe block
```

---

## Creating Configuration Files

### Configuration Generation Patterns

#### Pattern 1: Specify Environment and Requirements

```
Create configuration files for a production-ready Node.js TypeScript project.

Environment: Production + Development
Requirements:
- Strict TypeScript with path aliases
- ESLint (Airbnb style) + Prettier integration
- Jest with coverage reports
- Docker multi-stage build (development + production)
- CI/CD ready (GitHub Actions)
- Environment variables validation
- Logging configuration
- Security headers

Generate:
1. tsconfig.json
2. .eslintrc.json
3. .prettierrc
4. jest.config.js
5. Dockerfile
6. .dockerignore
7. .github/workflows/ci.yml
8. .env.example

Include comments explaining non-obvious settings.
```

#### Pattern 2: Migration from Existing Config

```
Convert my existing JavaScript ESLint config to TypeScript-compatible.

Current .eslintrc.js:
[paste current config]

Requirements:
- Add TypeScript parser and plugin
- Enable type-aware linting rules
- Keep existing rules
- Add recommended TypeScript rules
- Ignore build output directories
```

#### Pattern 3: Framework-Specific Configs

```
Create the necessary configuration files for a Next.js 14 project with:
- TypeScript (strict mode)
- Tailwind CSS
- ESLint and Prettier
- Absolute imports (@/components, @/lib, etc.)
- Environment variables (.env.local template)
- Path aliases

Generate all config files with explanatory comments.
```

### Config File Best Practices

**Request Explanations**:
```
Generate a tsconfig.json for a Node.js backend project.
Include comments explaining each compiler option.

Must have:
- Strict type checking
- ES2022 target
- CommonJS modules
- Source maps for debugging
- Path aliases: @/* -> src/*
- Exclude: node_modules, dist, tests
```

**Specify Integration Requirements**:
```
Create ESLint and Prettier configs that work together without conflicts.

ESLint:
- Airbnb base rules
- TypeScript support
- React hooks rules (we use React)
- Import order plugin

Prettier:
- 2-space indentation
- Single quotes
- Trailing commas (ES5)
- 100 character line width

Ensure Prettier runs after ESLint (no conflicts).
```

**Environment-Specific Configs**:
```
Create a configuration system for different environments.

Environments: development, staging, production

Config structure:
- config/default.ts (shared settings)
- config/development.ts (overrides)
- config/production.ts (overrides)
- config/index.ts (load based on NODE_ENV)

Settings to configure:
- Database connection
- API endpoints
- Logging level
- CORS origins
- Rate limits
- Cache TTL

Use TypeScript for type safety.
Include validation for required environment variables.
```

---

## Common Pitfalls and Solutions

### Pitfall 1: Generated Code Doesn't Fit Your Style

**Problem**: AI generates code in a different style than your project

**Solutions**:
```
Generate [what you need] following our code style:

Style guide:
- Use functional programming (no classes)
- Prefer const over let
- Use arrow functions
- Destructure parameters
- Early returns instead of nested if-else
- No semicolons
- Single quotes

Example from our codebase:
[paste example showing your style]
```

### Pitfall 2: Over-Engineered Solutions

**Problem**: AI generates overly complex code for simple tasks

**Solutions**:
```
Create a simple [what you need].

Constraints:
- No external libraries
- Maximum 30 lines
- No classes (use simple functions)
- No abstractions unless necessary
- Optimize for readability over cleverness

Keep it simple and straightforward.
```

### Pitfall 3: Missing Error Handling

**Problem**: Generated code lacks proper error handling

**Solutions**:
```
Generate [function/endpoint] with comprehensive error handling:

Handle:
- Invalid input types (throw TypeError)
- Out of range values (throw RangeError)
- Network failures (retry 3 times, then throw)
- Database errors (log and throw custom DatabaseError)
- Unexpected errors (log stack trace, throw generic error)

Use try-catch blocks.
Return errors in consistent format: { success: false, error: { code, message } }
```

### Pitfall 4: Insecure Code Generation

**Problem**: AI might generate code with security vulnerabilities

**Solutions**:
```
Generate [what you need] following security best practices:

Security requirements:
- Validate and sanitize all user inputs
- Use parameterized queries (prevent SQL injection)
- Hash passwords with bcrypt (12 rounds)
- No eval() or similar dangerous functions
- Rate limit authentication endpoints
- Use HTTPS-only cookies for tokens
- Add CSRF protection
- Implement proper authorization checks

Flag any potential security concerns in comments.
```

### Pitfall 5: Hard-Coded Values

**Problem**: Generated code contains hard-coded values instead of config

**Solutions**:
```
Generate [what you need] using environment variables for config.

Move to env vars:
- API endpoints
- Database connection strings
- Secret keys
- Timeouts and limits
- Feature flags

Use a config module that loads from .env file.
Include .env.example with dummy values.
Add validation for required variables at startup.
```

### Pitfall 6: No Tests Generated

**Problem**: Forgetting to ask for tests with the code

**Solutions**:
```
Generate [what you need] AND comprehensive unit tests.

For the function:
- [requirements]

For the tests:
- Use Jest
- Test happy path and edge cases
- Aim for 100% coverage
- Include test data fixtures

Provide both in the response.
```

### Pitfall 7: Outdated Patterns

**Problem**: AI uses outdated libraries or patterns

**Solutions**:
```
Generate [what you need] using modern best practices as of 2024+.

Requirements:
- Use latest stable versions
- Prefer native solutions over libraries when possible
- Use async/await (not callbacks)
- Use ES6+ features (destructuring, spread, etc.)
- TypeScript with strict type checking
- No deprecated APIs or methods

If suggesting libraries, mention version numbers.
```

---

## Hands-On Exercises

### Exercise 1: Boilerplate Generation (Beginner)

**Task**: Generate a complete Express REST API endpoint

**Prompt Template**:
```
Create a complete REST API endpoint for managing books.

Stack: Express + TypeScript + In-Memory Storage (array)

Book interface:
- id (string, UUID)
- title (string, required)
- author (string, required)
- isbn (string, optional)
- publishedYear (number, optional)
- createdAt (Date)

Generate:
1. TypeScript interface
2. In-memory data store
3. All CRUD operations (GET all, GET by id, POST, PUT, DELETE)
4. Input validation
5. Error responses (404, 400, 500)
6. TSDoc comments

Use Express Router.
```

**Success Criteria**:
- [ ] Code compiles without errors
- [ ] All CRUD operations work
- [ ] Proper error handling
- [ ] Type-safe TypeScript
- [ ] You understand every line

**Extension**: Ask AI to generate tests for the endpoints

---

### Exercise 2: Function from Comments (Intermediate)

**Task**: Write detailed comments, then generate function

1. **Write your comment**:
   ```typescript
   /**
    * Process customer order and calculate final price
    * 
    * Pricing rules:
    * - Base price from items array
    * - Discount codes: 'SAVE10' (10% off), 'SAVE20' (20% off)
    * - Free shipping over $50
    * - Tax rate: 8.5% (applied after discount)
    * 
    * Edge cases:
    * - Invalid discount codes should be ignored (not error)
    * - Negative prices should throw error
    * - Empty items array should throw error
    * 
    * @param items - Array of {name, price, quantity}
    * @param discountCode - Optional discount code
    * @returns {subtotal, discount, tax, shipping, total}
    */
   ```

2. **Prompt**: "Generate this function with full error handling and validation"

3. **Prompt**: "Now generate Jest tests covering all scenarios"

**Success Criteria**:
- [ ] Function works correctly for all scenarios
- [ ] Tests pass
- [ ] Edge cases handled
- [ ] Code is readable and maintainable

---

### Exercise 3: Test Generation (Intermediate)

**Task**: Generate tests for an existing function

**Given Function**:
```typescript
function isPalindrome(str: string): boolean {
  const cleaned = str.toLowerCase().replace(/[^a-z0-9]/g, '');
  return cleaned === cleaned.split('').reverse().join('');
}
```

**Your Prompt**:
```
Generate comprehensive Jest tests for the isPalindrome function above.

Test cases to include:
- Simple palindromes: 'racecar', 'noon'
- Palindromes with spaces: 'A man a plan a canal Panama'
- Palindromes with punctuation: 'Was it a car or a cat I saw?'
- Non-palindromes: 'hello', 'world'
- Edge cases: empty string, single character, numbers
- Case sensitivity handling
- Special characters

Use descriptive test names and group with describe blocks.
```

**Success Criteria**:
- [ ] All tests pass
- [ ] Coverage > 90%
- [ ] Edge cases covered
- [ ] Test names are descriptive

---

### Exercise 4: Configuration Files (Intermediate)

**Task**: Generate a complete config setup for a new project

**Prompt**:
```
Create all configuration files for a new full-stack TypeScript project.

Frontend: React 18 + Vite + TypeScript
Backend: Node.js + Express + TypeScript + PostgreSQL
Testing: Jest + React Testing Library

Generate:
1. Frontend tsconfig.json (with path aliases)
2. Backend tsconfig.json (different from frontend)
3. Root tsconfig.json (shared base config)
4. ESLint config (TypeScript + React)
5. Prettier config (integrate with ESLint)
6. Jest config (separate for frontend/backend)
7. Vite config (with proxy to backend during dev)
8. Docker Compose (postgres + backend + frontend)
9. .env.example (all required variables)
10. .gitignore (complete)

Include comments explaining non-obvious settings.
```

**Success Criteria**:
- [ ] All configs work together without conflicts
- [ ] TypeScript compiles successfully
- [ ] Linting and formatting work
- [ ] Tests can run
- [ ] You understand what each setting does

---

### Exercise 5: Refactor with AI (Advanced)

**Task**: Generate improved version of legacy code

**Given Legacy Code**:
```javascript
function getUserData(id) {
  var user = database.query('SELECT * FROM users WHERE id = ' + id);
  if (user) {
    var orders = database.query('SELECT * FROM orders WHERE user_id = ' + id);
    return {
      user: user,
      orders: orders,
      orderCount: orders.length
    };
  } else {
    return null;
  }
}
```

**Your Prompt**:
```
Refactor this legacy function to modern TypeScript with best practices.

Improvements needed:
- Convert to TypeScript with proper types
- Use parameterized queries (prevent SQL injection)
- Use async/await
- Add error handling
- Add input validation
- Use proper database client (node-postgres)
- Return consistent response format
- Add JSDoc comments
- Make it testable (dependency injection)

Explain the improvements made.
```

**Success Criteria**:
- [ ] Code is modern and secure
- [ ] Type-safe
- [ ] No SQL injection vulnerability
- [ ] Proper error handling
- [ ] You understand all changes made

---

### Exercise 6: Multi-File Generation (Advanced)

**Task**: Generate related files that work together

**Prompt**:
```
Create a complete feature: User authentication system

Files to generate:
1. src/models/User.ts - User interface and type guards
2. src/services/AuthService.ts - Registration, login, token refresh
3. src/routes/auth.ts - Express routes
4. src/middleware/authenticate.ts - JWT verification middleware
5. src/utils/jwt.ts - Token generation and verification
6. tests/auth.test.ts - Integration tests

Stack: Express + TypeScript + PostgreSQL + JWT + Bcrypt

Requirements:
- Password hashing with bcrypt
- JWT tokens (access + refresh)
- Input validation with Zod
- Rate limiting (5 attempts per 15 min)
- Email verification flow (send verification code)
- Password reset flow
- Consistent error responses

Generate all files with proper imports and exports.
The files should work together seamlessly.
```

**Success Criteria**:
- [ ] All files compile without errors
- [ ] Imports/exports are correct
- [ ] Files are cohesive and follow single responsibility
- [ ] Tests pass
- [ ] System works end-to-end

---

## Progress Checklist

Track your mastery of code generation:

### Boilerplate and Scaffolding
- [ ] Generated a project starter template
- [ ] Created CRUD operations for a resource
- [ ] Scaffolded a multi-file feature
- [ ] Generated configuration files for a new project
- [ ] Created a reusable template you actually use

### Functions from Comments
- [ ] Generated 5+ functions from detailed comments
- [ ] Used comment-first approach on a real task
- [ ] Generated complex business logic from specification
- [ ] Created algorithm implementation from description
- [ ] Refactored function by updating comments first

### Test Generation
- [ ] Generated unit tests for existing functions
- [ ] Used TDD approach (tests first, then implementation)
- [ ] Generated integration tests for API endpoints
- [ ] Achieved 90%+ coverage using AI-generated tests
- [ ] Generated edge case tests you didn't think of

### Configuration Files
- [ ] Generated TypeScript configuration
- [ ] Created ESLint + Prettier setup
- [ ] Generated test framework configuration
- [ ] Created Docker and docker-compose files
- [ ] Set up environment variable system

### Advanced Skills
- [ ] Generated multi-file features that work together
- [ ] Refactored legacy code with AI assistance
- [ ] Created reusable code templates
- [ ] Generated code for a framework you're learning
- [ ] Used AI to explore alternative implementations

---

## Key Takeaways

**Quality Factors** (in order of importance):
1. 🎯 **Prompt Specificity** - Clear requirements = better code (70-80% reduction in iterations)
2. 📚 **Context Provision** - Show examples and existing patterns (60% fewer errors)
3. 🔍 **Tech Stack Clarity** - Specify exact libraries and versions (80% correct approach rate)
4. ⚠️ **Constraint Definition** - Performance, security, style requirements
5. 🔄 **Iterative Refinement** - Start simple, add complexity gradually

**Best Practices Summary**:
- ✅ Write specifications before generating code
- ✅ Generate in layers (models → services → routes → tests)
- ✅ Provide examples from your codebase
- ✅ Request multiple approaches for complex problems
- ✅ Always generate tests alongside code
- ✅ Review and understand every generated line
- ✅ Iterate on generated code (don't accept first version blindly)

**Common Pitfalls to Avoid**:
- ❌ Vague prompts without context
- ❌ Accepting generated code without review
- ❌ Ignoring security implications
- ❌ Not generating tests
- ❌ Hard-coded values instead of config
- ❌ Outdated patterns and libraries

**Next Steps**:
- Complete all hands-on exercises
- Apply these techniques to real project work
- Build a library of effective prompts for your common tasks
- Move to [Phase 3: Code Review & Refactoring](phase3-code-review-refactoring.md)

---

## Additional Resources

**Bookmark These Prompts**:
- Generate boilerplate: [Link to template]
- Function from comment: [Link to template]
- Test generation: [Link to template]
- Config files: [Link to template]

**Further Practice**:
- [LeetCode](https://leetcode.com) - Generate solutions, then optimize
- [Project Starter Kits](link) - Compare AI-generated vs official templates
- [Design Patterns](link) - Generate pattern implementations

---

*Remember: AI-generated code is a starting point, not the finish line. Always review, test, and understand what you generate.*

**Next**: [Code Review & Refactoring →](phase3-code-review-refactoring.md)
