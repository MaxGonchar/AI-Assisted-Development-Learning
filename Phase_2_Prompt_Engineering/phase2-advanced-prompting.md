# Phase 2: Advanced Prompting

**Topic**: Master-level prompting for production-ready code  
**Duration**: 3-4 days  
**Prerequisites**: Basic Prompting + Practical Prompting Techniques

---

## Overview

Advanced prompting is where AI assistance transforms from helpful to indispensable. You'll learn to craft prompts that generate production-ready code with tests, documentation, proper error handling, and multiple solution options—all in one conversation.

**What You'll Learn:**
- Requesting specific advanced patterns (generics, decorators, design patterns)
- Getting comprehensive deliverables (code + tests + docs in one go)
- Comparing multiple approaches for informed decisions
- Advanced error handling strategies
- Meta-prompting: prompts that improve prompts
- Production-ready code with all the trimmings

**Key Difference from Practical Techniques:**  
Practical techniques teach individual methods. Advanced prompting teaches orchestrating them into complete, production-ready solutions.

---

## Table of Contents

1. [Requesting Specific Patterns](#1-requesting-specific-patterns)
2. [Comprehensive Error Handling](#2-comprehensive-error-handling)
3. [Complete Deliverables](#3-complete-deliverables)
4. [Multiple Approaches & Trade-offs](#4-multiple-approaches--trade-offs)
5. [Advanced Type Systems](#5-advanced-type-systems)
6. [Meta-Prompting](#6-meta-prompting)
7. [Production-Ready Workflows](#7-production-ready-workflows)
8. [Real-World Complex Scenarios](#8-real-world-complex-scenarios)

---

## 1. Requesting Specific Patterns

Go beyond basic functionality by requesting specific architectural patterns, design patterns, and advanced language features.

### Design Patterns

#### Example: Singleton Pattern

**Advanced Request:**
```
"Implement a Logger class using the Singleton pattern in TypeScript.

Pattern Requirements:
- Thread-safe singleton (handle multiple async calls)
- Lazy initialization (only create when first accessed)
- Prevent instantiation through 'new' keyword
- Support multiple log levels (debug, info, warn, error)
- Include getInstance() method

Additional features:
- Type-safe log levels using TypeScript enums
- Structured logging (timestamp, level, message, metadata)
- Configurable output targets (console, file, custom handler)
- Method to reset instance (useful for testing)

Provide:
1. Implementation with JSDoc
2. Usage examples showing singleton behavior
3. Test cases proving only one instance exists
4. Explanation of thread-safety approach in async context"
```

#### Example: Observer Pattern

**Advanced Request:**
```
"Implement the Observer pattern for a stock price monitoring system.

Pattern Requirements:
- Subject: StockMarket (manages stock prices)
- Observers: Trader, Analyst, Reporter (different notification behaviors)
- Type-safe observer registration/unregistration
- Observers can filter which stocks they monitor
- Support priority-based notifications (high priority observers first)

Technical Specifications:
- TypeScript with strict mode
- Use generics for type-safe notifications
- Implement IObserver<T> and ISubject<T> interfaces
- Prevent memory leaks (proper cleanup on unregister)
- Handle observer errors gracefully (one failing observer shouldn't affect others)

Deliverables:
1. Core pattern interfaces and classes
2. Three concrete observer implementations
3. Example scenario: price changes trigger notifications
4. Unit tests for registration, notification, and cleanup
5. Documentation explaining when to use this pattern"
```

### Architectural Patterns

#### Example: Repository Pattern

**Advanced Request:**
```
"Implement the Repository pattern for user data access.

Architecture:
- Generic BaseRepository<T> class with CRUD operations
- UserRepository extending BaseRepository
- Separate interface from implementation (dependency inversion)
- Support for multiple data sources (switch between PostgreSQL, MongoDB, in-memory)

Pattern Features:
- Generic methods: findById<T>, findAll<T>, create<T>, update<T>, delete<T>
- Query builder pattern for complex queries
- Transaction support
- Caching layer (Redis integration points)
- Error mapping (DB errors → domain errors)

Technical Requirements:
- TypeScript with advanced generics
- Async/await throughout
- Proper connection pooling
- SQL injection prevention
- Pagination support built-in

Provide:
1. IRepository<T> interface
2. BaseRepository<T> abstract class
3. UserRepository concrete implementation
4. Example showing how to switch data sources
5. Tests with mocked database
6. Documentation: when to use Repository vs direct DB access"
```

### Language-Specific Advanced Features

#### TypeScript Generics

**Advanced Request:**
```
"Create a type-safe API response wrapper using TypeScript generics.

Requirements:
- Generic ApiResponse<T> type
- Success and Error states (discriminated union)
- Type guards for checking response state
- Helper functions: isSuccess(response), isError(response)
- Chainable methods: map(), flatMap(), getOrElse()
- Full type inference (no type assertions needed)

Advanced TypeScript features to use:
- Discriminated unions
- Type guards with type predicates
- Generic constraints
- Conditional types
- Utility types (Pick, Omit, Partial)

Example usage:
```typescript
type User = { id: string; name: string };

const userResponse: ApiResponse<User> = await fetchUser(id);

if (isSuccess(userResponse)) {
  console.log(userResponse.data.name); // TypeScript knows .data exists
} else {
  console.log(userResponse.error); // TypeScript knows .error exists
}

// Or using map
const userName = userResponse.map(user => user.name).getOrElse('Unknown');
```

Deliverables:
1. Complete type definitions
2. Implementation with full type safety
3. Usage examples covering all features
4. Tests verifying type safety
5. Comparison with alternative approaches (Either, Result types)"
```

#### Python Decorators

**Advanced Request:**
```
"Create a suite of function decorators for API endpoint handling.

Decorators needed:
1. @retry(max_attempts, backoff) - Retry on failure with exponential backoff
2. @cache(ttl) - Cache results with time-to-live
3. @rate_limit(calls, period) - Rate limiting per user
4. @validate(schema) - Input validation using schema
5. @require_auth(roles) - Authorization check

Requirements:
- Decorators should be composable (stack multiple)
- Preserve function signatures and type hints
- Work with async functions
- Proper error handling and logging
- Type hints throughout

Advanced features:
- Use functools.wraps properly
- Support parameterized decorators
- Thread-safe (use locks where necessary)
- Context manager integration for cleanup

Example usage:
```python
@require_auth(['admin'])
@rate_limit(calls=10, period=60)
@validate(user_schema)
@cache(ttl=300)
@retry(max_attempts=3, backoff=1.5)
async def create_user(user_data: dict) -> User:
    # implementation
    pass
```

Deliverables:
1. All 5 decorators with full implementation
2. Decorator factory pattern explanation
3. Examples of stacking decorators
4. Tests for each decorator
5. Performance considerations document"
```

### Functional Programming Patterns

**Advanced Request:**
```
"Implement functional programming utilities in TypeScript.

Functions to implement:
1. pipe() - Left-to-right function composition
2. compose() - Right-to-left function composition
3. curry() - Convert multi-arg function to curried version
4. partial() - Partial application
5. memoize() - Function result caching

Advanced requirements:
- Full type inference (TypeScript magic)
- Support up to 10 functions in pipe/compose
- Proper overloads for different arities
- Work with async functions
- Memory-efficient memoization (LRU cache)

Type-safety challenge:
Make pipe() fully type-safe across transformations:
```typescript
const result = pipe(
  (n: number) => n * 2,        // number -> number
  (n: number) => `Count: ${n}`, // number -> string
  (s: string) => s.length       // string -> number
)(5);
// result should be inferred as number
```

Deliverables:
1. Implementation with full TypeScript types
2. Overloads for various arities
3. Examples showing type inference
4. Tests covering edge cases
5. Comparison with libraries like Ramda, Lodash/fp"
```

### Advanced Pattern Request Best Practices

✅ **DO:**
- Name the specific pattern you want
- Explain why you're using this pattern (helps AI understand context)
- Specify language-specific features to use
- Request explanation of trade-offs
- Ask for tests that prove the pattern works correctly

❌ **DON'T:**
- Assume AI knows obscure patterns (describe them briefly)
- Request patterns that don't fit your use case
- Mix multiple unrelated patterns in one request
- Forget about thread-safety, memory leaks, edge cases

### Practice Exercise 1

**Your Task:** Write a prompt requesting a Factory Pattern implementation for creating database connections (PostgreSQL, MySQL, MongoDB).

Include:
- Specific pattern requirements
- Type safety needs
- Connection pooling
- Error handling
- Tests

<details>
<summary>Click to see example answer</summary>

```
"Implement the Factory pattern for database connection creation.

Pattern: Factory Method with product families

Requirements:
- Abstract DatabaseConnection interface
- Concrete implementations: PostgresConnection, MySQLConnection, MongoConnection
- ConnectionFactory with createConnection(type, config) method
- Each connection type has specific configuration options
- Connection pooling built into each connection type
- Lazy initialization (connect on first query, not on creation)

Technical specifications:
- TypeScript with strict type checking
- Generic configuration types: PostgresConfig, MySQLConfig, etc.
- Async initialization
- Proper resource cleanup (connection.close())
- Error mapping: DB-specific errors → generic ConnectionError

Design constraints:
- Factory should be extensible (easy to add new DB types)
- Connection objects should have common interface (.query(), .transaction())
- Configuration should be validated before connection attempt
- Support connection health checks

Deliverables:
1. DatabaseConnection interface
2. Three concrete connection classes
3. ConnectionFactory implementation
4. Configuration type definitions
5. Usage example with error handling
6. Tests with connection mocking
7. Explanation: Factory vs Builder pattern for this use case"
```

</details>

---

## 2. Comprehensive Error Handling

Advanced error handling goes beyond try-catch. Request sophisticated error strategies suitable for production systems.

### Error Hierarchies

**Advanced Request:**
```
"Design a comprehensive error handling system for a microservices application.

Error hierarchy:
```
AppError (base)
├── OperationalError (expected, recoverable)
│   ├── ValidationError
│   ├── NotFoundError
│   ├── UnauthorizedError
│   ├── rate LimitError
│   └── ConflictError
├── ProgrammerError (unexpected, bugs)
│   ├── TypeError
│   ├── RangeError
│   └── AssertionError
└── ExternalError (third-party services)
    ├── DatabaseError
    ├── NetworkError
    ├── ThirdPartyAPIError
    └── TimeoutError
```

Requirements:
- Each error class with proper properties (message, code, statusCode, isOperational)
- Context data support (attach metadata to errors)
- Error serialization for logging and API responses
- Stack trace handling (capture for programmer errors, sanitize for operational)
- Error recovery strategies per error type

Advanced features:
- Error cause chains (Error A caused by Error B)
- Retry policies per error type
- Circuit breaker integration points
- Structured error logging (correlation IDs, request context)
- User-friendly error messages vs developer error messages

Deliverables:
1. Complete error class hierarchy
2. Error factory functions for common cases
3. Global error handler middleware (Express)
4. Error response formatter (for API responses)
5. Examples of throwing and handling each error type
6. Tests for error hierarchy and serialization
7. Documentation: when to throw which error type"
```

### Retry and Circuit Breaker

**Advanced Request:**
```
"Implement advanced retry logic with circuit breaker pattern.

Retry Strategy:
- Exponential backoff with jitter (prevent thundering herd)
- Configurable max attempts and backoff multiplier
- Selective retry (only retry on specific error types)
- Timeout per attempt
- Total timeout across all attempts

Circuit Breaker:
- States: Closed (normal), Open (failing fast), Half-Open (testing)
- Threshold: open circuit after N failures in time window
- Half-open timeout: try recovery after cooldown period
- Success threshold to close circuit: M consecutive successes
- Track metrics: failure rate, latency, throughput

Integration:
- Combine retry with circuit breaker
- Circuit breaker wraps multiple retry-enabled endpoints
- Graceful degradation when circuit is open
- Fallback functions for critical operations

Technical requirements:
- TypeScript with generics for type-safe operations
- Promise-based (async/await compatible)
- Event emitter for monitoring (circuitOpened, circuitClosed, retryAttempt)
- Thread-safe (handle concurrent requests)
- Configurable per service endpoint

Deliverables:
1. Retry policy implementation
2. Circuit breaker implementation
3. Combined RetryableCircuitBreaker class
4. Configuration builder pattern
5. Metrics collection hooks
6. Usage examples: HTTP requests, database queries
7. Tests simulating failures and recovery
8. Visual state diagram of circuit breaker
9. Performance considerations document"
```

### Error Boundaries and Recovery

**Advanced Request:**
```
"Create a robust error boundary system for React applications.

Error Boundary Features:
- Catch errors in component tree
- Fallback UI rendering (different UIs for different error types)
- Error reporting integration (send errors to monitoring service)
- Automatic retry mechanism (try re-render after error)
- Error isolation (error in one section doesn't crash entire app)

Advanced capabilities:
- Nested error boundaries (specific handlers for specific trees)
- Error context (additional info about where error occurred)
- Development vs production behavior
- Error recovery strategies (reset component state, fetch data again)
- TypeScript support with proper error typing

Technical specifications:
- Extend React.Component with error lifecycle methods
- getDerivedStateFromError() for fallback UI
- componentDidCatch() for logging
- Custom error types with recovery hints
- Integration with error monitoring (e.g., Sentry)

User experience focus:
- Friendly error messages (no stack traces to users)
- Action buttons (retry, go home, report issue)
- Progressive enhancement (degrade gracefully)
- Preserve user input when safe to do so
- Toast notifications for non-critical errors

Deliverables:
1. ErrorBoundary component with full features
2. Typed error objects with recovery strategies
3. Multiple fallback UI components
4. Error reporting service integration
5. Usage examples: wrapping routes, forms, data displays
6. Tests simulating various error scenarios
7. Best practices guide: where to place error boundaries
8. Comparison: error boundaries vs try-catch in event handlers"
```

### Error Monitoring and Observability

**Advanced Request:**
```
"Build an error monitoring and alerting system.

Core Features:
- Error capture (client and server)
- Error aggregation (group similar errors)
- Error context (user, session, environment, breadcrumbs)
- Stack trace parsing and source map support
- Real-time alerts (Slack, email, PagerDuty)
- Error trends and analytics

Error Processing Pipeline:
1. Capture: Intercept errors at various layers
2. Enrich: Add context, user info, environment
3. Deduplicate: Group similar errors, prevent spam
4. Classify: Severity, category, affected users
5. Route: Alert appropriate team based on classification
6. Store: Persist for analysis and trends

Technical implementation:
- Custom error reporter class
- Transport layer (queue errors, batch send)
- Backend API for receiving errors
- Error storage (database schema design)
- Alert rules engine
- Dashboard for error analytics

Integration points:
- Express error middleware
- React error boundaries
- Promise rejection handlers
- Browser global error handlers
- Source map support for production builds

Deliverables:
1. Client-side error reporter
2. Server-side error receiver API
3. Error storage schema and queries
4. Alert rules configuration system
5. Basic dashboard UI (list, filter, group errors)
6. Integration examples: React, Express, Next.js
7. Tests for full error lifecycle
8. Documentation: setting up error monitoring
9. Privacy considerations (PII filtering)"
```

### Error Handling Best Practices Summary

✅ **DO:**
- Create error hierarchies with meaningful distinctions
- Separate operational errors (expected) from programmer errors (bugs)
- Include context with every error (what operation failed, why)
- Design retry strategies appropriate to each error type
- Plan fallback behaviors for critical operations

❌ **DON'T:**
- Use generic error strings without structure
- Retry on all errors (some shouldn't be retried)
- Expose internal error details to end users
- Ignore errors silently
- Create too many error types (find balance)

### Practice Exercise 2

**Your Task:** Design error handling for a payment processing system.

Consider:
- Payment gateway errors (declined, insufficient funds, expired card)
- Network errors (timeout, connection)
- Internal errors (database, validation)
- Recovery strategies for each
- User communication

Write a comprehensive prompt requesting this system.

<details>
<summary>Click to see example answer</summary>

```
"Design comprehensive error handling for a payment processing system.

Payment Error Categories:
1. Payment Declined Errors (don't retry)
   - Insufficient funds
   - Card expired
   - Card blocked
   - Invalid CVV

2. Retriable Payment Errors
   - Gateway timeout
   - Gateway temporary failure
   - Rate limiting

3. Integration Errors
   - Network failure (retry with backoff)
   - Invalid API credentials (don't retry, alert)
   - Malformed request (don't retry, log bug)

4. Business Rule Errors
   - Amount exceeds limit
   - Duplicate transaction
   - Fraud detection trigger

Error Handling Strategy:
- Each error has: code, user message, developer message, retryable flag
- Declined errors: show specific reason to user, save for analytics
- Retriable errors: automatic retry (max 3 attempts, exponential backoff)
- Network errors: retry, then fallback to saved card
- Fraud detection: freeze transaction, notify user and admin

User experience:
- Clear error messages (avoid jargon)
- Suggest actions ("Please check your card balance")
- Save form data (don't make user re-enter everything)
- Option to try different payment method

Technical requirements:
- TypeScript error classes
- Integration with Stripe/Payment gateway error codes
- Idempotency handling (don't charge twice)
- Payment state management (pending, processing, success, failed)
- Error reporting to monitoring service
- Compliance: PCI-DSS (don't log card numbers)

Deliverables:
1. Payment error class hierarchy
2. Error mapping: Gateway errors → App errors
3. Retry strategy implementation
4. User-facing error messages
5. Payment form with error handling
6. Backend payment handler with comprehensive error handling
7. Tests simulating all error scenarios
8. Error recovery workflows diagram"
```

</details>

---

## 3. Complete Deliverables

Request everything needed for production in ait: code, tests, documentation, examples, and deployment considerations.

### The Full-Stack Feature Request

**Template:**
```
"Implement [feature] with complete production-ready deliverables.

Technical Stack:
- [technologies]

Deliverables Required:
1. Implementation (with inline comments)
2. TypeScript types/interfaces
3. Unit tests (90%+ coverage)
4. Integration tests (if applicable)
5. API documentation (if applicable)
6. Usage examples
7. Error handling
8. Performance considerations
9. Security considerations
10. Migration guide (if relevant)

Quality Gates:
- All tests pass
- No TypeScript errors
- Handles edge cases
- Proper logging
- Security review points noted"
```

### Example: Complete Authentication Feature

**Comprehensive Request:**
```
"Implement JWT-based authentication system with complete deliverables.

Technical Stack:
- Backend: Node.js, Express, PostgreSQL
- Frontend: React, TypeScript
- Libraries: jsonwebtoken, bcrypt, joi

Feature Requirements:
- User registration with email verification
- Login with JWT tokens
- Refresh token mechanism
- Password reset via email
- Role-based access control (RBAC)
- Session management

Security Requirements:
- Passwords hashed with bcrypt (10+ rounds)
- JWT tokens with short expiration (15 min access, 7 day refresh)
- Refresh tokens stored in httpOnly cookies
- Rate limiting on auth endpoints
- CSRF protection
- SQL injection prevention
- No sensitive data in JWT payload

Complete Deliverables:

1. **Database Schema**:
   - Users table (DDL SQL)
   - Sessions/refresh_tokens table
   - Roles and permissions tables
   - Indexes for performance

2. **Backend Implementation**:
   - User model with validation
   - Authentication service (register, login, refresh, logout)
   - Middleware: authenticate, authorize(roles)
   - Password reset flow
   - Email verification flow
   - Express routes with validation

3. **TypeScript Types**:
   - User interface
   - AuthTokens interface
   - Request extensions (req.user)
   - API request/response types

4. **Tests**:
   - Unit tests: password hashing, token generation
   - Integration tests: full auth flows
   - Test API endpoints with supertest
   - Mock email sending
   - Test authorization middleware

5. **Frontend**:
   - AuthContext provider (React)
   - Custom hooks: useAuth, useUser
   - Login form component
   - Register form component
   - Protected Route component
   - Automatic token refresh logic

6. **Documentation**:
   - API endpoints documented (OpenAPI/Swagger)
   - Authentication flow diagrams
   - Setup instructions
   - Environment variables needed
   - Security best practices followed

7. **Configuration**:
   - .env.example with all required variables
   - Config validation on startup
   - Different configs for dev/prod

8. **Error Handling**:
   - Custom error types
   - Error response format
   - User-friendly error messages

9. **Monitoring & Logging**:
   - Log authentication attempts
   - Log token refreshes
   - Alert on suspicious activity (rate limit hits)

10. **Deployment Considerations**:
    - HTTPS requirement noted
    - Token secret management (use env vars, not hardcoded)
    - Database migration files
    - Health check endpoint

Additional Requests:
- Code comments explaining security choices
- Comparison: JWT vs session-based auth
- Performance notes (token validation cost)
- Scalability considerations (horizontally scalable)
- GDPR compliance notes (user data handling)"
```

### Example: Complete REST API Resource

**Comprehensive Request:**
```
"Create a complete REST API for managing blog posts.

Technology:
- Node.js, Express, TypeScript, PostgreSQL, Redis (caching)

API Endpoints:
- POST /posts - Create post (authenticated, author only)
- GET /posts - List posts (public, paginated, filtered, sorted)
- GET /posts/:id - Get single post (public)
- PUT /posts/:id - Update post (authenticated, author or admin)
- DELETE /posts/:id - Delete post (authenticated, author or admin)
- POST /posts/:id/publish - Publish draft (authenticated, author)
- GET /posts/:id/versions - Version history (authenticated, author)

Complete Deliverables:

1. **Database**:
   - posts table schema with all fields
   - Indexes for performance (by author, published_at, tags)
   - Migration files (up/down)
   - Seed data for testing

2. **Backend Code**:
   - Post model with validation
   - Post service (business logic)
   - Post controller (request/response handling)
   - Routes with middleware
   - Validation schemas (Joi)
   - Authentication middleware
   - Authorization checks
   - Pagination helper
   - Search functionality (title, content, tags)
   - Cache layer (Redis for GET requests)

3. **TypeScript Types**:
   - Post interface
   - CreatePostDTO
   - UpdatePostDTO
   - PostListResponse
   - Query parameters types
   - Express Request/Response extensions

4. **Tests**:
   - Unit tests: service layer (80%+ coverage)
   - Integration tests: all API endpoints
   - Test authentication & authorization
   - Test pagination, filtering, sorting
   - Test caching behavior
   - Test edge cases (invalid IDs, missing fields)
   - Performance tests (load testing notes)

5. **Documentation**:
   - OpenAPI/Swagger specification
   - Example requests/responses for each endpoint
   - Authentication header format
   - Query parameter options
   - Error response format
   - Rate limiting details

6. **Error Handling**:
   - Validation errors (400)
   - Not found (404)
   - Unauthorized (401)
   - Forbidden (403)
   - Server errors (500)
   - Consistent error response format

7. **Features**:
   - Slug generation from title
   - Draft/published status
   - Soft delete (posts.deleted_at)
   - Version history tracking
   - Tags support
   - Full-text search
   - Markdown support for content

8. **Performance**:
   - Database query optimization
   - N+1 query prevention
   - Caching strategy (cache invalidation on update)
   - Pagination (cursor-based or offset)
   - Index strategy

9. **Security**:
   - Input sanitization
   - SQL injection prevention
   - XSS prevention
   - Rate limiting (100 req/min per IP)
   - CORS configuration

10. **Deployment**:
    - Dockerfile
    - docker-compose.yml (app + postgres + redis)
    - Environment variables documentation
    - Health check endpoint
    - Graceful shutdown handling

Provide all code organized in proper folder structure:
```
src/
  models/
  services/
  controllers/
  routes/
  middleware/
  utils/
  types/
  config/
tests/
  unit/
  integration/
migrations/
docs/
```"
```

### Example: Complete React Component Library

**Comprehensive Request:**
```
"Create a complete, production-ready Button component library.

Requirements:
- React 18, TypeScript, Tailwind CSS
- Storybook for documentation
- Published npm package ready

Component Variants:
- Variants: primary, secondary, danger, ghost, link
- Sizes: xs, sm, md, lg, xl
- States: default, hover, active, disabled, loading
- Types: button, submit, reset

Complete Deliverables:

1. **Implementation**:
   - Button.tsx component
   - Full TypeScript prop types
   - Compound components: Button.Group, Button.Icon
   - Accessibility (ARIA attributes, keyboard nav)
   - Focus management
   - Ripple effect animation (optional)

2. **Styling**:
   - Tailwind classes with theme integration
   - CSS modules as fallback
   - Dark mode support
   - Custom theme configuration
   - CSS variables for easy customization

3. **TypeScript**:
   - Strict prop types
   - Generic click handler types
   - Polymorphic component (as prop for button/link)
   - Exported types for consumers

4. **Tests**:
   - Jest + React Testing Library
   - Test all variants render correctly
   - Test all states (hover, disabled, loading)
   - Test onClick handlers
   - Test accessibility (screen reader text)
   - Test keyboard navigation
   - Viusal regression tests (optional)

5. **Storybook**:
   - Story for each variant
   - Story for each size
   - Story for all states
   - Interactive controls
   - Accessibility addon
   - Documentation tab with usage examples
   - Code snippets

6. **Documentation**:
   - README.md with installation
   - Props API documentation
   - Usage examples
   - Customization guide
   - Accessibility notes
   - Migration guide (if updating existing)
   - Changelog

7. **Package Configuration**:
   - package.json configured correctly
   - Entry points (ESM and CJS)
   - Type definitions published
   - Tree-shaking support
   - Peer dependencies listed
   - Keywords for npm search

8. **Build Setup**:
   - Rollup/Vite build configuration
   - TypeScript compilation
   - CSS processing
   - Bundle size optimization
   - Source maps
   - Development vs production builds

9. **CI/CD**:
   - GitHub Actions workflow
   - Run tests on PR
   - Build package
   - Publish to npm (manual trigger)
   - Semantic versioning

10. **Examples**:
    - Example Next.js app using the button
    - Example form integration
    - Example with icons
    - Example button groups
    - Loading states example

Folder structure:
```
button-component/
  src/
    Button.tsx
    Button.test.tsx
    Button.stories.tsx
    types.ts
    styles.css
  docs/
  examples/
  .storybook/
  package.json
  tsconfig.json
  rollup.config.js
```

Quality criteria:
- Bundle size < 10KB gzipped
- Tree-shakeable
- Zero runtime deps (only peer deps)
- TypeScript strict mode passes
- 90%+ test coverage
- Accessibility score 100 (Lighthouse)
- Works in all modern browsers"
```

### Requesting Complete Deliverables Best Practices

✅ **DO:**
- List every artifact you need upfront
- Specify quality gates (test coverage, bundle size)
- Request folder structure organization
- Include deployment/packaging requirements
- Ask for examples and documentation
- Specify both happy path and error scenarios

❌ **DON'T:**
- Assume AI will provide tests without asking
- Forget about documentation
- Skip security considerations
- Ignore performance requirements
- Leave quality criteria implicit

### Practice Exercise 3

**Your Task:** Write a comprehensive prompt for a complete file upload feature.

Include:
- Frontend upload UI (drag-drop, progress)
- Backend handling (validation, storage)
- Tests for both
- Security considerations
- Documentation
- Error handling

<details>
<summary>Click to see example answer</summary>

```
"Implement complete file upload system with all production requirements.

Technology Stack:
- Frontend: React, TypeScript
- Backend: Node.js, Express
- Storage: AWS S3 (or configurable)
- Database: PostgreSQL (file metadata)

Feature Requirements:
- Drag & drop file upload
- Multiple file selection
- Upload progress indication
- File type validation (images, PDFs only)
- File size limit (10MB per file, 50MB total)
- Generate thumbnails for images
- Virus scanning (ClamAV integration)
- Resume interrupted uploads (chunked upload)

Complete Deliverables:

1. **Frontend Implementation**:
   - FileUpload component (drag-drop zone)
   - Progress bar component
   - File list with remove button
   - Preview for images
   - Error handling UI
   - TypeScript types for upload states

2. **Backend Implementation**:
   - Upload endpoint (POST /upload)
   - Chunked upload support
   - File validation (type, size)
   - Virus scanning integration
   - S3 upload logic
   - Thumbnail generation (sharp library)
   - Database storage (file metadata)
   - Secure file naming (prevent overwrite)
   - Cleanup failed uploads

3. **Security**:
   - Authentication required
   - File type whitelist (not blacklist)
   - Size limits enforced server-side
   - Signed URLs for S3 (time-limited access)
   - Virus scanning before storing
   - Prevent directory traversal
   - imit uploads per user (prevent abuse)

4. **Database Schema**:
   - files table (id, user_id, filename, size, mime_type, s3_key, created_at)
   - Indexes for queries
   - Migration files

5. **Tests**:
   - Frontend: test drag-drop, file selection, progress
   - Backend: test validation, upload flow, error cases
   - Integration: full upload flow
   - Test various file types and sizes
   - Test security (unauthorized upload attempt)
   - Mock S3 and virus scanner

6. **Documentation**:
   - API endpoint documentation
   - Supported file types and limits
   - Setup instructions (S3 credentials, virus scanner)
   - Security considerations explanation
   - Architecture diagram

7. **Configuration**:
   - Environment variables (.env.example)
   - Configurable storage backend (S3, local, GCS)
   - Configurable limits
   - Virus scanning toggle (for dev)

8. **Error Handling**:
   - Network errors (retry logic)
   - Validation errors (show specific issue)
   - Storage errors (S3 failure)
   - Virus detection (quarantine file)
   - User-friendly error messages

9. **Performance**:
   - Chunked upload (resume capability)
   - Parallel uploads (limit concurrency)
   - Thumbnail generation async (job queue)
   - Efficient progress updates
   - Cancel upload capability

10. **Deployment**:
    - Docker configuration
    - S3 bucket policy example
    - Environment setup guide
    - Monitoring recommendations (track upload failures)

Provide organized folder structure with all files."
```

</details>

---

## 4. Multiple Approaches & Trade-offs

Request several solutions to compare trade-offs and make informed architectural decisions.

### Pattern: Compare and Contrast

**Template:**
```
"Implement [feature] using [approach 1], [approach 2], and [approach 3].

For each approach provide:
1. Complete implementation
2. Pros and cons
3. Use cases (when to choose this approach)
4. Performance characteristics
5. Complexity analysis
6. Testing difficulty

Then recommend which approach to use and why, considering:
- [your specific context]
- [your specific constraints]"
```

### Example: State Management Comparison

**Advanced Request:**
```
"Implement user authentication state management in React using three different approaches.

Scenario: Shopping cart application needs to track logged-in user, cart items, and preferences across the app.

Approach 1: React Context API
- Implement AuthContext and CartContext
- Custom hooks: useAuth, useCart
- Provider at app root

Approach 2: Redux Toolkit
- Auth and cart slices
- Thunks for async actions
- Typed hooks and selectors

Approach 3: Zustand
- Separate stores for auth and cart
- Middleware for persistence
- Simple API

For each implementation, provide:

1. **Complete Code**:
   - Store/context setup
   - Actions/reducers/mutations
   - Custom hooks for consumption
   - Example components using the state

2. **Pros**:
   - What this approach excels at
   - Developer experience benefits
   - Ecosystem advantages

3. **Cons**:
   - Limitations and drawbacks
   - Complexity introduced
   - Learning curve

4. **Performance**:
   - Re-render behavior
   - Optimization techniques available
   - Bundle size impact

5. **Developer Experience**:
   - Boilerplate amount
   - TypeScript support
   - DevTools availability
   - Testing ease

6. **Use Cases**:
   - App size where this shines
   - Team size considerations
   - Complexity level suited for

7. **Code Comparison Table**:
   | Aspect | Context | Redux | Zustand |
   |--------|---------|-------|---------|
   | Lines of code | | | |
   | Bundle size | | | |
   | Boilerplate | | | |
   | Learning curve | | | |
   | DevTools | | | |

8. **Migration Difficulty**:
   - How hard to switch from this approach
   - Lock-in considerations

Final Recommendation:
- Given this is a shopping cart app (medium complexity)
- Small team (2-3 developers)
- Need quick iteration speed
- Recommend: [approach] because [reasoning]

Provide decision matrix showing when to choose each approach."
```

### Example: API Design Patterns

**Advanced Request:**
```
"Design an API for a task management system using three different architectural patterns.

Requirements:
- CRUD operations for tasks
- Task lists/projects
- Task status updates
- Task assignment
- Comments on tasks

Pattern 1: REST
- Resource-based endpoints
- HTTP verbs for operations
- Standard status codes

Pattern 2: GraphQL
- Single endpoint
- Query and mutation types
- Resolvers for each field

Pattern 3: RPC-style (gRPC or tRPC)
- Procedure calls
- Typed contracts
- Bi-directional streaming (for real-time updates)

For Each Pattern:

1. **Complete API Spec**:
   - All endpoints/queries/procedures
   - Request/response formats
   - Example calls with curl/code

2. **Implementation Sketch**:
   - Server setup code
   - Route/resolver/procedure definitions
   - Sample handler function

3. **Client Integration**:
   - How client makes requests
   - TypeScript types generation
   - Error handling approach

4. **Comparison Matrix**:
   | Feature | REST | GraphQL | RPC |
   |---------|------|---------|-----|
   | Over-fetching | | | |
   | Under-fetching | | | |
   | Versioning | | | |
   | Caching | | | |
   | Documentation | | | |
   | Type safety | | | |
   | Learning curve | | | |
   | Real-time support | | | |
   | Mobile-friendly | | | |

5. **Pros & Cons**:
   - List specific to this task management use case
   - Consider both frontend and backend perspective

6. **Performance Analysis**:
   - Network efficiency (number of requests)
   - Payload size comparison
   - Caching strategies available
   - Database query efficiency

7. **Developer Experience**:
   - Code generation tools
   - Testing approach
   - Documentation generation
   - IDE support

8. **Scaling Considerations**:
   - How each scales with complexity
   - Microservices compatibility
   - API gateway integration

9. **Real Example**:
   - Show complete flow: "Get all tasks in a project with assignee details"
   - Include actual code/queries for each pattern

Recommendation:
Based on the task management domain, provide recommendation with reasoning:
- Consider: team expertise, frontend needs, mobile apps, real-time requirements
- Decision tree: "Choose REST if..., choose GraphQL if..., choose RPC if..."
```

### Example: Database Query Optimization

**Advanced Request:**
```
"Optimize this slow database query using three different strategies.

Current Query (PostgreSQL):
```sql
SELECT orders.*, users.name, products.title
FROM orders
JOIN users ON orders.user_id = users.id
JOIN order_items ON orders.id = order_items.order_id
JOIN products ON order_items.product_id = products.id
WHERE orders.created_at > NOW() - INTERVAL '30 days'
AND orders.status = 'completed'
ORDER BY orders.created_at DESC
LIMIT 100;
```

Problem: Takes 3+ seconds with 1M orders, 10M order_items

Strategy 1: Indexing
- Analyze query execution plan
- Add appropriate indexes
- Explain index choices

Strategy 2: Denormalization
- Materialize frequent joins
- Cache computed values
- Trigger/job to keep in sync

Strategy 3: Query Rewriting
- Break into smaller queries
- Remove unnecessary joins
- Use subqueries or CTEs

For Each Strategy:

1. **Implementation**:
   - SQL code (index creation, new schema, refactored query)
   - Additional code (triggers, background jobs)

2. **Execution Plan Analysis**:
   - EXPLAIN ANALYZE output comparison
   - Before and after metrics
   - Bottleneck identification

3. **Trade-offs**:
   - Storage impact
   - Write performance impact
   - Complexity introduced
   - Data consistency concerns

4. **Maintenance**:
   - Index maintenance overhead
   - Data sync requirements
   - Migration difficulty

5. **Performance Gains**:
   - Expected query time improvement
   - Scalability characteristics
   - Breaking point (when does this approach fail)

6. **Cost Analysis**:
   - Storage cost increase
   - CPU/memory impact
   - Developer time to implement

7. **Monitoring**:
   - What to monitor
   - When strategy becoming inadequate
   - How to detect issues

Comparison Table:
| Metric | Current | Indexing | Denorm | Rewrite |
|--------|---------|----------|--------|---------|
| Query time | 3000ms | | | |
| Storage | | | | |
| Write impact | | | | |
| Complexity | Low | | | |
| Maintenance | | | | |

Recommendation:
- For this specific query pattern
- Given these data volumes
- With this growth trajectory
- Recommend combining strategies X and Y because..."
```

### Multiple Approaches Best Practices

✅ **DO:**
- Request 2-4 approaches (more than 4 is overwhelming)
- Ask for specific comparison criteria
- Include decision matrix or comparison table
- Request recommendation with reasoning
- Consider both technical and team factors

❌ **DON'T:**
- Ask for too many approaches (diminishing returns)
- Compare apples to oranges (approaches should solve same problem)
- Forget context (team skill, constraints, timeline)
- Skip the recommendation (analysis paralysis)

### Practice Exercise 4

**Your Task:** Request comparison of caching strategies for an API.

Include:
- In-memory caching (Redis)
- HTTP caching (ETags, Cache-Control)
- CDN caching
- Database query caching

Ask for comparison across relevant dimensions and a recommendation.

<details>
<summary>Click to see example answer</summary>

```
"Compare caching strategies for a REST API serving product catalog data.

Context:
- 10,000 products, updated daily
- 1000 requests/second during peak
- Product data changes infrequently
- Users expect <100ms response times
- Need to support millions of users

Strategy 1: Redis Caching
- Cache full product objects in Redis
- Cache key: `product:{id}`
- TTL: 24 hours, invalidate on update
- Application-level caching

Strategy 2: HTTP Caching
- ETags for each product
- Cache-Control headers
- Conditional requests (if-none-match)
- Browser and reverse proxy caching

Strategy 3: CDN Caching
- Cache API responses at edge
- Geographic distribution
- Purge on product updates
- Vary by request headers

Strategy 4: Database Query Caching
- PostgreSQL query result cache
- Materialized views
- Database-level optimizations

For Each Strategy:

1. **Implementation**:
   - Code examples
   - Configuration required
   - Integration points

2. **Performance Characteristics**:
   - Cache hit time
   - Cache miss time
   - Impact on origin server load
   - Network latency considerations

3. **Cache Invalidation**:
   - How to invalidate stale data
   - Complexity of invalidation
   - Consistency guarantees
   - Partial vs full cache clears

4. **Cost**:
   - Infrastructure cost (Redis hosting, CDN costs)
   - Development effort
   - Operational complexity

5. **Scalability**:
   - How it scales with traffic
   - Geographic distribution
   - Cache coordination across instances

6. **Pros & Cons**:
   - Specific to product catalog use case
   - Consider both read and write patterns

7. **Failure Modes**:
   - What happens if cache goes down
   - Thundering herd problem
   - Cache stampede mitigation

Comparison Matrix:
| Aspect | Redis | HTTP | CDN | DB Query |
|--------|-------|------|-----|----------|
| Hit latency | | | | |
| Miss latency | | | | |
| Infrastructure cost | | | | |
| Invalidation complexity | | | | |
| Scalability | | | | |
| Geographic distribution | | | | |
| Failure impact | | | | |

Use Case Analysis:
- For rarely changing data: recommend...
- For frequently changing data: recommend...
- For global audience: recommend...
- For cost optimization: recommend...

Recommendation:
Given product catalog scenario (infrequent updates, high read volume, global audience):
- Recommended approach: [combination of strategies]
- Implementation priority order
- Migration path from no caching to full implementation
- Monitoring metrics for each layer"
```

</details>

---

## 5. Advanced Type Systems

Request sophisticated type-level programming in TypeScript, advanced Python typing, or other type system features.

### TypeScript Advanced Types

**Advanced Request:**
```
"Create a fully type-safe REST API client using TypeScript advanced types.

Requirements:
- Define API schema in types
- Type-safe endpoint definition
- Automatic type inference for requests/responses
- No runtime type assertions (never use 'as' or 'any')
- Autocomplete for endpoint paths

Example usage (goal):
```typescript
const api = createAPIClient<APISchema>();

// TypeScript should infer types automatically
const user = await api.get('/users/:id', { params: { id: '123' } });
//    ^? User type inferred

const newPost = await api.post('/posts', {
  body: { title: 'Hello', content: 'World' }
});
//    ^? Post type inferred

// TypeScript error if wrong endpoint or params
api.get('/invalid'); // Error: invalid endpoint
api.get('/users/:id', { params: {} }); // Error: missing id
```

Advanced TypeScript Features to Use:
- Template literal types (for path parsing)
- Mapped types (for endpoint definitions)
- Conditional types (for request/response mapping)
- Type inference (infer parameters from path)
- Generic constraints
- Recursive types (for nested paths)
- Distributive conditional types

API Schema Example:
```typescript
type APISchema = {
  '/users/:id': {
    get: {
      params: { id: string };
      response: User;
    };
    patch: {
      params: { id: string };
      body: Partial<User>;
      response: User;
    };
  };
  '/posts': {
    get: {
      query: { page?: number; limit?: number };
      response: { posts: Post[]; total: number };
    };
    post: {
      body: CreatePostInput;
      response: Post;
    };
  };
  '/posts/:id': {
    get: {
      params: { id: string };
      response: Post;
    };
    delete: {
      params: { id: string };
      response: void;
    };
  };
};
```

Deliverables:
1. Complete type definitions with explanations
2. createAPIClient implementation
3. Helper types (ExtractParams, ExtractQuery, etc.)
4. Usage examples showing type inference
5. Tests that compilation succeeds with valid usage
6. Tests that compilation fails with invalid usage (tsd library)
7. Explanation of each advanced type feature used
8. Comparison with alternatives (like tRPC, OpenAPI generators)
9. Limitations and edge cases

Challenge: Make it work with nested routes and array indices:
```typescript
'/users/:userId/posts/:postId/comments/:commentId'
```"
```

### Typed State Machines

**Advanced Request:**
```
"Implement a type-safe finite state machine in TypeScript.

State Machine: User Authentication Flow

States:
- Idle (initial)
- LoggingIn
- Authenticated
- LoggedOut
- SessionExpired

Events:
- LOGIN (with credentials)
- LOGIN_SUCCESS (with user data)
- LOGIN_FAILURE (with error)
- LOGOUT
- SESSION_TIMEOUT
- REFRESH_TOKEN

Transitions (state machine diagram):
```
Idle -> (LOGIN) -> LoggingIn
LoggingIn -> (LOGIN_SUCCESS) -> Authenticated
LoggingIn -> (LOGIN_FAILURE) -> Idle
Authenticated -> (LOGOUT) -> LoggedOut
Authenticated -> (SESSION_TIMEOUT) -> SessionExpired
SessionExpired -> (REFRESH_TOKEN) -> Authenticated
```

Type Safety Requirements:
- Only valid transitions compile
- Events have typed payloads
- State-specific context (e.g., 'user' only exists in Authenticated state)
- Type guards for checking current state
- Actions executed on state entry/exit

Advanced TypeScript:
- Discriminated unions for states
- Type-safe event dispatching
- State-specific context typing
- Impossible states made unrepresentable
- Generic state machine implementation

Implementation Requirements:
1. State type definitions (discriminated union)
2. Event type definitions
3. Context types (different per state)
4. Transition function (reduces State + Event -> State)
5. Type guards: isAuthenticated(state), isLoggingIn(state)
6. State machine class with generic types
7. Side effect hooks (onEnterState, onExitState)

Example Usage:
```typescript
const authMachine = createStateMachine<AuthState, AuthEvent>({
  initial: { type: 'Idle' },
  transitions: {
    Idle: {
      LOGIN: (state, event) => ({
        type: 'LoggingIn',
        credentials: event.credentials
      })
    },
    LoggingIn: {
      LOGIN_SUCCESS: (state, event) => ({
        type: 'Authenticated',
        user: event.user,
        token: event.token
      }),
      LOGIN_FAILURE: () => ({ type: 'Idle' })
    },
    Authenticated: {
      LOGOUT: () => ({ type: 'LoggedOut' }),
      SESSION_TIMEOUT: () => ({ type: 'SessionExpired' })
    }
  }
});

// Type-safe usage
authMachine.dispatch({ type: 'LOGIN', credentials: {...} });
authMachine.dispatch({ type: 'INVALID' }); // TypeScript error

if (isAuthenticated(authMachine.state)) {
  console.log(authMachine.state.user.name); // 'user' definitely exists here
}
```

Deliverables:
1. Complete type definitions
2. State machine implementation
3. Type guards for each state
4. Usage examples showing type safety
5. Tests verifying valid transitions
6. Tests showing invalid transitions don't compile
7. Visualization of state diagram
8. Comparison with XState (when to roll your own vs use library)
9. Extension points for adding new states/events"
```

### Branded Types and Phantom Types

**Advanced Request:**
```
"Implement a type-safe units system using branded types.

Problem: Prevent mixing incompatible units

```typescript
const distanceInMeters = 100;
const distanceInFeet = 300;
const sum = distanceInMeters + distanceInFeet; // Bug! Mixed units
```

Solution: Branded types that catch errors at compile time

Requirements:
- Types for different units: Meters, Feet, Kilograms, Pounds, USD, EUR
- Convert functions that change brands
- Arithmetic operations preserve brands
- Type errors when mixing incompatible units
- Zero runtime overhead (purely type-level)

Implementation using Branded Types:
```typescript
type Brand<K, T> = K & { __brand: T };

type Meters = Brand<number, 'Meters'>;
type Feet = Brand<number, 'Feet'>;
type Kilograms = Brand<number, 'Kilograms'>;
type USD = Brand<number, 'USD'>;

// Constructor functions
function meters(value: number): Meters {
  return value as Meters;
}

// Arithmetic that preserves types
function add<T extends string>(
  a: Brand<number, T>,
  b: Brand<number, T>
): Brand<number, T> {
  return (a + b) as Brand<number, T>;
}

// Type-safe conversions
function metersToFeet(m: Meters): Feet {
  return (m * 3.28084) as Feet;
}

// Usage
const distance1 = meters(100);
const distance2 = meters(50);
const total = add(distance1, distance2); // OK: both Meters

const distanceInFeet = feet(300);
const invalid = add(distance1, distanceInFeet); // ERROR: Type mismatch!
```

Extended Requirements:
- Area types (SquareMeters, SquareFeet)
- Velocity types (MetersPerSecond)
- Temperature (Celsius, Fahrenheit, Kelvin)
- Type-safe unit conversion functions
- Arithmetic operations: add, subtract, multiply, divide
- Handle derived units (Meters * Meters = SquareMeters)
- Comparison operations (<, >, ==) with same units

Advanced Features:
- Generic arithmetic system
- Composite units (Newtons = kg⋅m/s²)
- Type-level unit algebra
- Parse unit strings ("10m", "5kg") with type checking

Deliverables:
1. Branded type definition and helpers
2. All unit types
3. Constructor functions
4. Type-safe arithmetic operations
5. Conversion functions with correct types
6. Examples showing compile-time safety
7. Runtime cost analysis (should be zero)
8. Tests showing type errors for invalid operations
9. Comparison with runtime validation libraries
10. When to use branded types vs runtime checks"
```

### Advanced Type System Best Practices

✅ **DO:**
- Request explanations of advanced type features used
- Ask for impossible states to be unrepresentable
- Request zero runtime cost validation
- Ask for comparison with runtime alternatives
- Include examples showing both valid and invalid code

❌ **DON'T:**
- Overuse advanced types where simple types suffice
- Sacrifice readability for type safety
- Ignore compilation time impact
- Forget about error messages (make them helpful)

### Practice Exercise 5

**Your Task:** Request a type-safe builder pattern in TypeScript.

Goal: Build objects step-by-step with compile-time enforcement that all required fields are set.

```typescript
const user = new UserBuilder()
  .setName('John')
  .setEmail('john@example.com')
  .build(); // OK

const invalid = new UserBuilder()
  .setName('John')
  .build(); // ERROR: email not set
```

Write a prompt requesting this with full type safety.

<details>
<summary>Click to see example answer</summary>

```
"Implement a type-safe builder pattern in TypeScript.

Goal: Build objects with compile-time enforcement that all required fields are set before calling .build().

Requirements:
- Builder for a User type with required fields (name, email) and optional fields (age, phone)
- Cannot call .build() until all required fields are set
- Each setter returns a new builder type with that field marked as set
- Optional fields can be skipped
- Type inference throughout (no manual type annotations needed by user)

Example Usage:
```typescript
interface User {
  name: string;
  email: string;
  age?: number;
  phone?: string;
}

// This should compile:
const user1 = new UserBuilder()
  .setName('John')
  .setEmail('john@example.com')
  .build(); // OK: all required fields set

const user2 = new UserBuilder()
  .setName('Jane')
  .setEmail('jane@example.com')
  .setAge(25)
  .build(); // OK: optional field also set

// These should NOT compile:
const invalid1 = new UserBuilder()
  .setName('John')
  .build(); // ERROR: email not set

const invalid2 = new UserBuilder()
  .build(); // ERROR: name and email not set

const invalid3 = new UserBuilder()
  .setEmail('test@example.com')
  .build(); // ERROR: name not set
```

Advanced TypeScript Features to Use:
- Phantom types (type-level "flags" for which fields are set)
- Method chaining with evolving return types
- Conditional types to enable/disable .build()
- Mapped types for tracking required fields

Type-Level Implementation Approach:
```typescript
type FieldsSet = {
  name: false;
  email: false;
  // ... (initially all false)
};

class UserBuilder<T extends FieldsSet> {
  setName(name: string): UserBuilder<T & { name: true }>;
  setEmail(email: string): UserBuilder<T & { email: true }>;
  setAge(age: number): UserBuilder<T>; // Optional, doesn't change type
  
  // build() only available when all required fields are true
  build(this: UserBuilder<{ name: true; email: true }>): User;
}
```

Deliverables:
1. Complete UserBuilder implementation with full types
2. Generic Builder<T> that works for any object type
3. Helper type to extract required vs optional fields
4. Usage examples showing type safety
5. Tests that valid builds compile
6. Tests using 'tsd' library that invalid builds don't compile
7. Explanation of phantom types and how they work
8. Performance notes (does this add runtime overhead?)
9. Comparison with alternatives (class-validator, yup)
10. Extend to support nested builders (e.g., address field is itself a builder)"
```

</details>

---

## 6. Meta-Prompting

Use AI to help you write better prompts. Improve your prompts iteratively.

### Prompt Review and Improvement

**Meta-Prompt Example:**
```
"I'm trying to get AI to generate a shopping cart feature. Here's my current prompt:

```
Create a shopping cart for my e-commerce site
```

This prompt is too vague and not getting good results. 

Please:
1. Analyze what's missing from this prompt
2. List the context and requirements I should include
3. Rewrite this as an effective prompt using best practices
4. Explain what you changed and why

Consider:
- Technical stack should be specified
- Features should be detailed
- Testing requirements
- Security considerations
- Performance needs"
```

### Prompt Template Generation

**Meta-Prompt Example:**
```
"Create a reusable prompt template for generating React components.

The template should include placeholders for:
- Component name and purpose
- Props and their types
- State requirements
- Styling approach
- Accessibility needs
- Testing expectations

Format as a fill-in-the-blank template that I can reuse for consistent results when creating components.

Include:
1. The template with [PLACEHOLDERS]
2. An example of the template filled in
3. The expected output from using that example
4. Usage instructions"
```

### Prompt Pattern Library Building

**Meta-Prompt Example:**
```
"I frequently need to request API endpoint implementations. Help me create a standardized prompt pattern.

Analyze these three successful prompts I've used:
[Paste 3 previous prompts that worked well]

Then:
1. Extract the common patterns and structure
2. Identify what made these prompts effective
3. Create a reusable template incorporating these patterns
4. Add sections I might have missed
5. Provide 2-3 variations for different API complexity levels

The template should be flexible enough to use for:
- Simple CRUD endpoints
- Complex multi-step operations
- Real-time/WebSocket endpoints"
```

### Debugging Prompts

**Meta-Prompt Example:**
```
"My prompt isn't giving me the results I want. Help me debug it.

**My Prompt:**
```
Act as a senior developer. Create a secure authentication system with JWT tokens, password hashing, email verification, and social login. Include tests and documentation. Make sure it's production-ready.
```

**Problem:**
AI is overwhelming me with a massive response that's hard to follow, and the code doesn't quite match our project structure.

**What I Actually Want:**
Step-by-step implementation guidance, starting with just the core auth (login/register), that matches our existing Express + TypeScript pattern.

Please:
1. Identify issues with my current prompt
2. Explain why it's not working
3. Suggest better approach (maybe iterative?)
4. Show me refined version of the prompt
5. Explain the improvements made"
```

### Prompt Optimization

**Meta-Prompt Example:**
```
"Optimize this prompt for better results:

**Current Prompt:**
```
I need a function to handle user registration. It should validate the input, hash the password, save to database, send welcome email, and return the user. Use TypeScript. Include error handling.
```

**Optimization Criteria:**
- More specific about requirements
- Better structured for readability
- Include constraints I forgot
- Add deliverables beyond just code
- Specify testing expectations
- Add security considerations

Review my prompt and provide:
1. Scored analysis (0-10) on:
   - Specificity
   - Context provided
   - Structure/readability
   - Completeness
2. Optimized version
3. Side-by-side comparison showing improvements
4. Explanation of why optimized version is better"
```

### Creating Domain-Specific Prompts

**Meta-Prompt Example:**
```
"Help me create a prompt pattern specifically for database schema design.

Consider what information is always needed:
- Entity relationships
- Data types
- Constraints (unique, not null, foreign keys)
- Indexes
- Performance considerations
- Migration strategy

Create a comprehensive prompt template that ensures I never forget important aspects of database design.

Include sections for:
- Business requirements translation
- Normalization level
- Expected data volume
- Query patterns
- Scalability plans

Provide:
1. The template with clear sections
2. Example filled in for a blog database
3. Checklist of things to consider
4. Common mistakes to avoid in DB design prompts"
```

### Prompt Improvement Iterations

**Meta-Process:**
```
"Let's improve my prompt iteratively.

Start prompt: 
```
Create a responsive navigation bar
```

Process:
Round 1: Add what's missing - you tell me
Round 2: I'll fill in details based on your feedback  
Round 3: You suggest additional improvements
Round 4: Final optimized prompt

At each round, explain what we're improving and why.

Goal: End with a production-ready prompt that gets great results consistently."
```

### Meta-Prompting Best Practices

✅ **DO:**
- Share examples of prompts that worked well (or didn't)
- Be specific about what results you're seeking
- Ask for explanation of improvements
- Request reusable templates
- Build a library of successful patterns

❌ **DON'T:**
- Expect AI to read your mind about context
- Skip providing examples of your attempts
- Ignore the explanations (learn from them)
- Forget to test improved prompts

### Practice Exercise 6

**Your Task:** You've written this prompt:

```
"Make a form for user registration"
```

It's not detailed enough. Write a meta-prompt asking AI to:
- Analyze what's missing
- Rewrite it as a comprehensive prompt
- Explain the improvements

<details>
<summary>Click to see example answer</summary>

```
"Help me improve this insufficient prompt:

**Current Prompt:**
```
Make a form for user registration
```

**Problems I'm Having:**
- Results are too generic
- Missing validation
- Don't match our project style
- No error handling
- Unclear what to include

**Our Context (should be in prompt):**
- React + TypeScript project
- Using Formik for forms
- Yup for validation
- Material-UI components
- Need accessibility compliance
- Mobile-responsive required

Please:
1. List everything missing from current prompt
2. Create sections my prompt should have:
   - Technical specs
   - Fields required
   - Validation rules
   - UI/UX requirements
   - Accessibility needs
   - Testing expectations
   - Documentation needs

3. Rewrite as comprehensive prompt including all missing elements

4. Show before/after comparison highlighting:
   - What was added
   - Why it's important
   - How it improves results

5. Create a reusable template I can use for other forms

6. Provide checklist for reviewing form-related prompts

Goal: Never write vague form prompts again. Build reusable pattern for all form requests."
```

</details>

---

## 7. Production-Ready Workflows

Request complete workflows that match real development processes.

### Feature Development Workflow

**Comprehensive Request:**
```
"Guide me through implementing a comment system feature using production workflow.

Feature: Users can comment on blog posts

Phases:
1. Requirements gathering and clarification
2. Database schema design
3. API design
4. Implementation (backend then frontend)
5. Testing strategy
6. Documentation
7. Deployment plan
8. Monitoring setup

For each phase, provide:
- Questions I should ask/answer
- Deliverables for that phase
- Review checklist
- Red flags to watch for
- How this phase informs next phase

Start with Phase 1: Ask me clarifying questions about requirements.
Wait for my answers, then proceed to Phase 2.

Continue iteratively through all phases.

At each phase:
- Provide actual code/schemas/docs (not just descriptions)
- Explain decisions made
- Highlight trade-offs
- Suggest alternatives
- Show how to validate this phase before moving on

Context:
- Express + PostgreSQL + React
- Team of 3 developers
- Moderate traffic (1000 DAU)
- Launching in 4 weeks"
```

### Code Review Workflow

**Comprehensive Request:**
```
"Perform a Production-grade code review on this pull request.

**PR Title:** Add user avatarimage upload

**Changed Files:**
1. `uploadController.ts` (new)
2. `userModel.ts` (modified - added avatarUrl field)
3. `userRoutes.ts` (modified - added upload endpoint)
4. `AvatarUpload.tsx` (new - React component)

[Paste code for each file]

Review Process:

**Phase 1: High-Level Review**
- Architecture/design concerns
- Security implications
- Performance considerations
- Breaking changes or API impacts

**Phase 2: Code Quality**
- Style and consistency
- Error handling
- Edge cases
- Resource management (memory leaks, connections)
- Type safety

**Phase 3: Testing**
- Test coverage adequacy
- Missing test cases
- Test quality

**Phase 4: Documentation**
- Code comments
- API documentation
- README updates

**Phase 5: Operations**
- Logging for debugging
- Metrics/monitoring hooks
- Deployment risks
- Rollback plan

For Each Issue Found:**
- Severity: Critical | Major | Minor | Nitpick
- Location: File + line number
- Problem: What's wrong
- Impact: Why it matters
- Suggestion: How to fix (with code example)
- Learning: Explain the principle

**Final Summary:**
- Block merge? Yes/No + reasoning
- Required changes
- Suggested improvements
- Positive feedback (what was done well)
- Estimated rework time

**Also Provide:**
- Security checklist completed
- Performance checklist completed
- Production-readiness checklist"
```

### Bug Investigation Workflow

**Comprehensive Request:**
```
"Help me investigate and fix this production bug systematically.

**Bug Report:**
Users report that checkout occasionally fails with "Invalid cart state" error.
- Frequency: ~5% of checkouts
- Pattern: Seems random, no obvious correlation
- Impact: Lost sales, user frustration

**Known Info:**
- Backend: Node.js + Express + Redis (cart storage)
- Frontend: React SPA
- Error appears after clicking "Complete Purchase"
- Database logs show no failed transactions
- Error started 3 days ago after deploying cart optimization update

Investigation Workflow:

**Phase 1: Understand & Reproduce**
- Ask me clarifying questions about symptoms
- Request specific logs/error messages
- Suggest theories for root cause
- Propose reproduction steps

**Phase 2: Gather Data**
- What logs to check
- What metrics to analyze
- Code sections to review
- How to add debug logging (without overloading production)

**Phase 3: Hypothesis & Testing**
- Form hypothesis based on data
- Design test to prove/disprove
- Safe way to test in production (feature flags, etc.)
- Rollback criteria

**Phase 4: Fix**
- Propose fix with code
- Explain why this fixes root cause
- Identify what else might be affected
- Testing strategy for fix

**Phase 5: Prevention**
- What went wrong in our process
- How to prevent similar bugs
- Monitoring to add
- Tests to add

**Phase 6: Post-Mortem**
- Timeline of events
- Root cause analysis
- Action items
- Communication plan (to users, team, stakeholders)

Walk me through each phase systematically.
At each phase, wait for data I provide before proceeding."
```

### Testing Strategy Development

**Comprehensive Request:**
```
"Develop a comprehensive testing strategy for a payment processing module.

**Module:** PaymentService (processes credit card payments via Stripe)

**Testing Levels Needed:**

1. **Unit Tests**
   - What to test
   - Mocking strategy
   - Edge cases to cover
   - Example test suite structure

2. **Integration Tests**
   - Stripe API integration
   - Database transactions
   - Email notifications
   - Example test scenarios

3. **End-to-End Tests**
   - Full payment flow
   - Error scenarios
   - User experience paths
   - Tools to use

4. **Security Testing**
   - PCI compliance checks
   - SQL injection attempts
   - Authentication tests
   - Authorization tests
   - Data exposure tests

5. **Performance Testing**
   - Load testing approach
   - Stress testing scenarios
   - Latency requirements
   - Tools and setup

6. **Chaos Engineering**
   - Stripe API failures
   - Database connection loss
   - Network timeouts
   - Race conditions

For Each Testing Level:
1. Specific test cases (given/when/then)
2. Tools and frameworks
3. Code examples
4. Coverage goals
5. CI/CD integration
6. When to run (on commit, nightly, before deploy)

Also Provide:
- Testing pyramid for this module
- Priority order (what to test first)
- Test data management strategy
- Mocking vs real API calls decision guide
- How to handle Stripe test mode

**Deliverables:**
- Complete test suite structure
- Example tests for each level
- Testing documentation
- CI/CD pipeline configuration
- Test maintenance guide"
```

### Production-Ready Workflow Best Practices

✅ **DO:**
- Request phase-by-phase approach with validation gates
- Include monitoring and observability from start
- Ask for checklists and review criteria
- Include rollback and disaster recovery plans
- Consider the full SDLC (not just coding)

❌ **DON'T:**
- Jump straight to coding without requirements
- Skip testing strategy
- Forget about operations (logging, monitoring)
- Ignore documentation needs
- Overlook security reviews

### Practice Exercise 7

**Your Task:** Request a deployment workflow for a database migration.

Include:
- Pre-deployment checks
- Backup strategy
- Migration execution plan
- Rollback procedure
- Verification steps
- Monitoring during migration

Write a comprehensive prompt.

<details>
<summary>Click to see example answer</summary>

```
"Create a safe production deployment plan for a database migration.

**Migration:** Adding full-text search to posts table (1M rows)

**Changes:**
- Add tsvector column to posts table
- Create GIN index on tsvector column
- Backfill existing posts with search vectors
- Update application code to use full-text search

**System Context:**
- PostgreSQL 14
- High-availability setup (primary + replica)
- Zero-downtime requirement (24/7 service)
- Read-heavy workload (10K reads/sec, 100 writes/sec)

**Deployment Workflow:**

Phase 1: Pre-Deployment
- Risk assessment
- Backup strategy (what, where, verification)
- Traffic preparation (scale up? maintenance window?)
- Team readiness (who does what, communication plan)
- Rollback criteria defined
- Monitoring dashboard setup

Phase 2: Migration Preparation
- Migration script with safety checks
- Estimated duration calculation
- Lock analysis (what will be locked, for how long)
- Performance impact prediction
- Test in staging environment
- Dry-run procedure

Phase 3: Deployment Steps (detailed)
- Step-by-step commands with timing
- How to pause/resume if issues arise
- Checkpoints for validation
- When to proceed vs rollback
- Application deployment coordination (code changes)

Phase 4: Validation
- What to check after migration
- Performance metrics to watch
- Data integrity checks
- User-facing functionality tests
- How long to monitor before success

Phase 5: Rollback Procedure**
- When to trigger rollback
- Step-by-step rollback commands
- Data consistency considerations
- Communication if rollback needed

Phase 6: Post-Migration
- Monitoring setup (alerts for new column/index)
- Performance analysis
- Optimization opportunities
- Documentation updates
- Post-mortem (even if successful)

For Each Phase Provide:
1. Detailed checklist
2. Commands to run (actual SQL, shell scripts)
3. Expected output/results
4. Red flags to watch for
5. Decision points (go/no-go criteria)
6. Time estimates

Special Considerations:
- Online vs offline migration trade-offs
- Index creation time (won't block writes?)
- Backfill strategy (all at once vs batched)
- Replica lag management
- Query plan changes

Deliverables:
1. Complete runbook (step-by-step guide)
2. Migration SQL scripts with safety checks
3. Monitoring queries to run during migration
4. Rollback script
5. Communication templates (start, success, issues)
6. Lessons learned template for post-mortem"
```

</details>

---

## 8. Real-World Complex Scenarios

Apply all advanced techniques to realistic, multi-faceted problems.

### Scenario 1: Microservices Decomposition

**Comprehensive Advanced Request:**
```
"Help me decompose a monolithic e-commerce application into microservices.

**Current Monolith:**
- Single Node.js application
- PostgreSQL database (shared by all features)
- Features: Products, Orders, Users, Payments, Inventory, Recommendations

**Business Drivers:**
- Different teams working on different features (conflicts)
- Need independent deploy cycles
- Scale recommendations service independently (CPU intensive)
- Improve reliability (order processing issues affect entire site)

**Multi-Phase Analysis:**

Phase 1: Service Identification
- Analyze domain boundaries
- Identify service candidates
- Define data ownership
- Assess dependencies between services
- Recommend service decomposition strategy
- Provide service boundaries diagram

Phase 2: Data Strategy
- How to split the database
- Shared data handling approach
- Event-driven architecture vs API calls
- Data consistency strategies (eventual vs strong)
- Migration from single DB to multiple DBs

Phase 3: Communication Patterns
- Synchronous vs asynchronous communication
- API gateway design
- Service mesh considerations
- Event bus architecture
- Error handling across services

Phase 4: Implementation Plan
- Which service to extract first (and why)
- Step-by-step extraction process
- How to run monolith + microservices during transition
- Database split approach (strangler pattern?)
- Feature flags for gradual rollout

Phase 5: Cross-Cutting Concerns
- Distributed tracing
- Centralized logging
- Authentication/authorization across services
- Configuration management
- Service discovery

For Each Phase:
1. Analysis with diagrams
2. Recommendation with trade-offs
3. Example implementation
4 Risks and mitigation
5. Decision criteria

Also Address:
- Team structure (Conway's Law)
- DevOps overhead (now have 6+ services to deploy)
- Testing complexity (integration tests across services)
- Debugging distributed systems
- Cost implications

Compare Approaches:
- Big bang migration vs incremental
- Microservices vs modular monolith
- Docker + Kubernetes vs serverless
- REST vs gRPC vs message queues

Final Deliverable:
- Complete migration roadmap
- Architecture diagrams (current vs target)
- First service extraction (complete code example)
- Risk register
- Success metrics"
```

### Scenario 2: Real-Time Collaboration Feature

**Comprehensive Advanced Request:**
```
"Implement real-time collaborative editing (Google Docs-style) for a markdown editor.

**Requirements:**
- Multiple users edit same document simultaneously
- See others' cursors and selections
- Changes appear in real-time
- Conflict-free merges (no lost edits)
- Works with poor network conditions
- Revision history
- Presence awareness (who's online)

**Technical Challenges:**
- Operational Transformation (OT) vs CRDT algorithms
- WebSocket connection management
- State synchronization
- Conflict resolution
- Offline support
- Performance with large documents

**Comprehensive Implementation:**

Part 1: Algorithm Selection
- Compare OT vs CRDT approaches
- Pros/cons for each
- Recommendation with reasoning
- Detailed explanation of chosen algorithm
- Handle scenarios: concurrent edits, network partitions, offline

Part 2: Backend Architecture
- WebSocket server design
- Document state management
- Operation broadcasting strategy
- Persistence (operational log vs snapshots)
- Scaling (multiple server instances)
- Redis pub/sub for server coordination

Part 3: Frontend Implementation  
- Editor integration (use existing editor library)
- Local operation handling
- Optimistic updates
- Conflict resolution UI
- Cursor rendering
- Selection tracking
- Reconnection handling

Part 4: Data Structures
- Document representation
- Operation format
- Transformation functions
- Cursor position mapping
- Version vectors (conflict resolution)

Part 5: Edge Cases
- User joins mid-editing
- Network disconnection & reconnect
- Rapid concurrent edits
- Large paste operations
- Undo/redo in collaborative context
- User leaves without cleanup

Part 6: Performance Optimization
- Debouncing operations
- Batching small operations
- Diff algorithms
- Memory management (long sessions)
- Large document handling

Part 7: Testing Strategy
- Unit tests: transformation functions
- Integration tests: collaboration scenarios
- Chaos testing: network failures
- Race condition testing
- Performance benchmarks

Deliverables:
1. Complete backend (Node.js + WebSockets)
2. Complete frontend (React + editor integration)
3. Algorithm implementation with explanations
4. Test suite covering edge cases
5. Performance analysis
6. Scaling strategy document
7. Comparison with libraries (Y.js, Automerge)
8. When to use existing library vs build own

Provide everything needed for production deployment."
```

### Scenario 3: System Performance Crisis

**Comprehensive Advanced Request:**
```
"Production system is slow. Help me diagnose and fix.

**Symptoms:**
- Response times increased from 200ms to 3-5 seconds (last week)
- CPU usage at 80-90% (used to be 30-40%)
- Database connections often maxed out
- Users complaining in support tickets
- Some requests timing out
- Started after deploying new social features

**System:**
- Node.js API servers (4 instances)
- PostgreSQL database (1 primary, 1 replica)
- Redis cache
- React frontend
- 10K daily active users

**Systematic Investigation:**

Phase 1: Immediate Triage
- What to check first (APM, logs, database)
- Quick wins for immediate relief
- Should we rollback?
- Emergency monitoring setup

Phase 2: Data Gathering
- What metrics to capture
- What logs to analyze
- Query performance analysis
- Network analysis
- Cache hit rates
- External service dependencies

Phase 3: Root Cause Analysis
- Analyze the deploy (what changed?)
- Database query analysis (slow queries, N+1)
- Memory leaks check
- Event loop blocking
- Cache ineffectiveness
- Resource exhaustion

Phase 4: Hypothesis Formation
- List possible causes (ordered by likelihood)
- How to test each hypothesis
- Design experiments
- A/B testing approach (if safe)

Phase 5: Solution Design
- Immediate fixes (quick wins)
- Short-term solutions (this week)
- Long-term architecture improvements
- Compare approaches with trade-offs

Phase 6: Implementation
- Prioritized fix list
- Code changes needed
- Configuration tuning
- Infrastructure scaling
- Monitoring improvements

Phase 7: Validation
- How to measure success
- Monitoring dashboards
- Load testing strategy
- Gradual rollout plan
- Rollback criteria

Phase 8: Prevention
- What to add to prevent recurrence
- Performance testing in CI/CD
- Capacity planning process
- Alerting improvements
- Post-mortem template

For Each Phase, Provide:
- Specific commands/queries to run
- Scripts for data collection
- Analysis techniques
- Decision tree
- Examples of findings

Common Culprits to Check:
- N+1 queries (database)
- Missing indexes
- Cache stampede
- Unoptimized images/assets
- Synchronous operations that should be async
- Memory leaks
- Event loop blocking (CPU-intensive operations)
- Connection pool exhaustion
- Rate limiting from external APIs

Tools and Techniques:
- APM (New Relic, DataDog)
- PostgreSQL EXPLAIN ANALYZE
- Chrome DevTools performance
- Node.js profiler (--inspect)
- Memory heap snapshots
- Load testing (k6, Artillery)

Deliverables:
1. Investigation report with findings
2. Root cause explanation
3. Fix implementation with benchmarks
4. Performance testing strategy
5. Monitoring/alerting improvements
6. Post-mortem document
7. Prevention checklist

Show me step-by-step how to go from 'oh no' to 'fixed and prevented'."
```

### Real-World Scenario Best Practices

✅ **DO:**
- Include multiple technical dimensions
- Request systematic methodology
- Ask for trade-off analysis
- Request both immediate and long-term solutions
- Include organizational/process aspects
- Ask for lessons learned and prevention

❌ **DON'T:**
- Jump to solutions before understanding
- Ignore non-technical factors (team, process)
- Skip the "why" analysis
- Forget about prevention
- Overlook operational concerns

---

## Summary: Advanced Prompting Mastery

### Key Techniques Covered

1. **Specific Patterns**: Request exact design patterns, algorithms, advanced language features
2. **Comprehensive Error Handling**: Error hierarchies, retry logic, circuit breakers, monitoring
3. **Complete Deliverables**: Code + tests + docs + deployment in one request
4. **Multiple Approaches**: Compare solutions with trade-offs for informed decisions
5. **Advanced Type Systems**: Type-safe APIs, state machines, branded types
6. **Meta-Prompting**: Improve prompts, build templates, create pattern libraries
7. **Production Workflows**: Full SDLC processes, not just coding
8. **Complex Scenarios**: Multi-faceted real-world problems

### The Advanced Prompting Formula

```
[SPECIFIC PATTERN/REQUIREMENT]
+ [COMPREHENSIVE SCOPE]
+ [TECHNICAL CONSTRAINTS]
+ [MULTIPLE PERSPECTIVES]
+ [COMPLETE DELIVERABLES]
+ [PRODUCTION CONSIDERATIONS]
= Production-ready code with context and confidence
```

### When to Use Advanced Techniques

| Situation | Techniques to Use |
|-----------|------------------|
| New feature | Complete Deliverables + Production Workflow |
| Architecture decision | Multiple Approaches + Trade-off Analysis |
| Complex algorithm | Specific Pattern + Advanced Types + Chain-of-Thought |
| System design | Multiple Approaches + Real-World Scenario |
| Bug fixing | Production Workflow + Systematic Investigation |
| Performance | Multiple Approaches + Benchmarking |
| Improving prompts | Meta-Prompting + Template Building |

### Advanced Prompting Checklist

Before sending a complex prompt, verify:

- [ ] Specific technical requirements stated
- [ ] Advanced features/patterns explicitly requested
- [ ] Complete deliverables listed (code + tests + docs)
- [ ] Error handling requirements specified
- [ ] Testing strategy included
- [ ] Performance considerations mentioned
- [ ] Security aspects addressed
- [ ] Documentation expected
- [ ] Trade-offs requested (if comparing approaches)
- [ ] Production readiness criteria defined

---

## Next Steps

1. ✅ Practice requesting specific patterns in your domain
2. ✅ Build a personal library of advanced prompt templates
3. ✅ Combine multiple techniques for complex requests
4. ✅ Use meta-prompting to refine your prompt style
5. ✅ Apply to real projects to see impact
6. ✅ Continue to **Phase 3: Effective Workflows** for applying these in daily development

**Congratulations on completing Advanced Prompting!** You now have the skills to extract maximum value from AI coding assistants. The difference between basic users and power users is the sophistication of their prompts—you're now in the latter camp.

---

## Bonus: Your Advanced Prompt Template

Use this as a starting point for complex requests:

```
Act as [ROLE with specific expertise].

[TASK description - what to build/analyze/review]

Context:
- Tech stack: [specific versions]
- Constraints: [performance, security, scalability]
- Team context: [team size, expertise, timeline]

Requirements:
- [Requirement 1 with specifics]
- [Requirement 2 with specifics]
- [Requirement 3 with specifics]

[ANY EXISTING CODE/PATTERNS to match]

Think step-by-step:
1. [Consider X]
2. [Analyze Y]
3. [Evaluate Z]

Complete Deliverables:
1. Implementation
   - [File 1] with [specifics]
   - [File 2] with [specifics]
2. Tests
   - [ types of tests]
   - [Coverage expectations]
3. Documentation
   - [Type 1]
   - [Type 2]
4. [Additional deliverables]

Quality Criteria:
- [Criterion 1]
- [Criterion 2]
- [Criterion 3]

[OPTIONAL: Compare multiple approaches]
[OPTIONAL: Request trade-off analysis]
[OPTIONAL: Production considerations]
```

Customize this template for your specific needs and build a library of proven patterns!
