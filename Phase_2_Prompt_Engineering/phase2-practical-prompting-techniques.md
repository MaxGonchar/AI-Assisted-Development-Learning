# Phase 2: Practical Prompting Techniques

**Topic**: Advanced prompting methods for better AI coding results  
**Duration**: 3-4 days  
**Prerequisites**: Basic Prompting fundamentals

---

## Overview

While basic prompting teaches you to be clear and specific, practical prompting techniques give you powerful patterns that dramatically improve AI output quality. These are proven methods used by experienced AI developers to get better code, better explanations, and more reliable results.

**What You'll Learn:**
- **Few-Shot Prompting**: Teaching by example
- **Chain-of-Thought**: Getting AI to reason step-by-step
- **Role/Persona Prompting**: Leveraging expertise perspectives
- **Constraint-Based**: Setting precise boundaries
- **Iterative Refinement**: Building up complexity gradually
- **Code Context**: Maintaining consistency with existing code
- **Explicit Output Format**: Requesting specific structures

---

## Table of Contents
1. [Few-Shot Prompting](#1-few-shot-prompting)
2. [Chain-of-Thought Prompting](#2-chain-of-thought-prompting)
3. [Role/Persona Prompting](#3-rolepersona-prompting)
4. [Constraint-Based Prompting](#4-constraint-based-prompting)
5. [Iterative Refinement](#5-iterative-refinement)
6. [Code Context](#6-code-context)
7. [Explicit Output Format](#7-explicit-output-format)
8. [Combining Techniques](#8-combining-techniques)
9. [Real-World Scenarios](#9-real-world-scenarios)

---

## 1. Few-Shot Prompting

**Concept**: Show AI examples of what you want before asking for new code. Like teaching someone by demonstration rather than just verbal instructions.

### Why Few-Shot Works

- AI learns your coding style and patterns
- Reduces ambiguity about what you want
- Ensures consistency across your codebase
- Faster than explaining every detail in words

### Template

```
"Here's how I [do X] in this project:

[Example 1]
[Example 2]
[Example 3 - optional]

Now create similar code for [new task]"
```

### Example 1: API Error Handling

**❌ Without Few-Shot (Generic Result):**
```
"Create error handling for the user authentication endpoint"
```

AI might generate generic try-catch blocks that don't match your project's style.

**✅ With Few-Shot (Project-Specific):**
```
"Here's how I handle API errors in this project:

// Example 1: From productController.ts
export const getProduct = async (req: Request, res: Response) => {
  try {
    const product = await productService.getById(req.params.id);
    res.json({ success: true, data: product });
  } catch (error) {
    if (error instanceof NotFoundError) {
      return res.status(404).json({ 
        success: false, 
        error: { code: 'PRODUCT_NOT_FOUND', message: error.message } 
      });
    }
    logger.error('Product fetch failed:', error);
    res.status(500).json({ 
      success: false, 
      error: { code: 'INTERNAL_ERROR', message: 'Failed to fetch product' } 
    });
  }
};

// Example 2: From orderController.ts
export const createOrder = async (req: Request, res: Response) => {
  try {
    const order = await orderService.create(req.body);
    res.status(201).json({ success: true, data: order });
  } catch (error) {
    if (error instanceof ValidationError) {
      return res.status(400).json({ 
        success: false, 
        error: { code: 'VALIDATION_ERROR', message: error.message } 
      });
    }
    logger.error('Order creation failed:', error);
    res.status(500).json({ 
      success: false, 
      error: { code: 'INTERNAL_ERROR', message: 'Failed to create order' } 
    });
  }
};

Now create similar error handling for the user authentication endpoint (loginUser function)."
```

**Result:** AI generates code that perfectly matches your error handling pattern, response structure, and logging approach.

### Example 2: React Component Style

**✅ Few-Shot for Consistent Components:**
```
"Here are examples of our React component style:

// Example 1: Button.tsx
interface ButtonProps {
  label: string;
  onClick: () => void;
  variant?: 'primary' | 'secondary';
  disabled?: boolean;
}

export const Button: React.FC<ButtonProps> = ({ 
  label, 
  onClick, 
  variant = 'primary',
  disabled = false 
}) => {
  return (
    <button 
      className={`btn btn-${variant}`}
      onClick={onClick}
      disabled={disabled}
    >
      {label}
    </button>
  );
};

// Example 2: Card.tsx
interface CardProps {
  title: string;
  content: string;
  footer?: React.ReactNode;
}

export const Card: React.FC<CardProps> = ({ title, content, footer }) => {
  return (
    <div className="card">
      <h3 className="card-title">{title}</h3>
      <div className="card-content">{content}</div>
      {footer && <div className="card-footer">{footer}</div>}
    </div>
  );
};

Following this pattern, create an Alert component with:
- Props: message (string), type ('success' | 'error' | 'warning'), dismissible (boolean)
- Show an icon based on type
- Include a close button if dismissible
- Use TypeScript with proper types
- Follow the same export and naming conventions"
```

### Example 3: Database Query Patterns

**✅ Few-Shot for Data Layer:**
```
"Here's how we write database queries in this project:

// Example 1: userRepository.ts
export const findUserById = async (id: string): Promise<User | null> => {
  try {
    const result = await db.query(
      'SELECT id, email, name, created_at FROM users WHERE id = $1',
      [id]
    );
    return result.rows[0] || null;
  } catch (error) {
    logger.error('Database query failed:', error);
    throw new DatabaseError('Failed to fetch user');
  }
};

// Example 2: userRepository.ts
export const createUser = async (userData: CreateUserInput): Promise<User> => {
  try {
    const result = await db.query(
      'INSERT INTO users (email, name, password_hash) VALUES ($1, $2, $3) RETURNING id, email, name, created_at',
      [userData.email, userData.name, userData.passwordHash]
    );
    return result.rows[0];
  } catch (error) {
    if (error.code === '23505') { // Unique constraint violation
      throw new ConflictError('User already exists');
    }
    logger.error('Database insert failed:', error);
    throw new DatabaseError('Failed to create user');
  }
};

Following this pattern, create a function to update user profile (email and name only)."
```

### Few-Shot Best Practices

✅ **DO:**
- Show 2-3 diverse examples
- Include realistic code from your actual project
- Show edge case handling in examples
- Keep examples focused on the pattern you want replicated

❌ **DON'T:**
- Show 10 examples (2-3 is optimal)
- Use generic, trivial examples
- Mix different patterns in examples (be consistent)
- Show examples without explaining the new task

### Practice Exercise 1

**Your Task:** Write a few-shot prompt to create a new validation function.

You have these existing validation functions:
```typescript
export const validateEmail = (email: string): boolean => {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email);
};

export const validatePassword = (password: string): boolean => {
  return password.length >= 8 && /[0-9]/.test(password) && /[^a-zA-Z0-9]/.test(password);
};
```

Write a prompt to create a `validateUsername` function (3-20 chars, alphanumeric + underscore only).

<details>
<summary>Click to see example answer</summary>

```
"Here's how we write validation functions in this project:

export const validateEmail = (email: string): boolean => {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email);
};

export const validatePassword = (password: string): boolean => {
  return password.length >= 8 && /[0-9]/.test(password) && /[^a-zA-Z0-9]/.test(password);
};

Following this pattern, create a validateUsername function that:
- Accepts a username string
- Returns boolean
- Validates: 3-20 characters, alphanumeric and underscore only
- Use regex for validation"
```

</details>

---

## 2. Chain-of-Thought Prompting

**Concept**: Ask AI to explain its reasoning step-by-step before or while generating code. This leads to more thoughtful, accurate solutions.

### Why Chain-of-Thought Works

- Forces AI to "think" through the problem
- Reveals reasoning, making it easier to spot errors
- Produces better solutions for complex logic
- Helps you understand the approach
- Particularly powerful for algorithms and optimization

### Template

```
"[Task description]

Think step-by-step:
1. [First consideration]
2. [Second consideration]
3. [Third consideration]
...
Then provide the implementation."
```

Or simply:
```
"[Task]. Think through this step-by-step, then provide the code."
```

### Example 1: Algorithm Optimization

**❌ Without Chain-of-Thought:**
```
"Optimize this function:

function findDuplicates(arr) {
  const duplicates = [];
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] === arr[j] && !duplicates.includes(arr[i])) {
        duplicates.push(arr[i]);
      }
    }
  }
  return duplicates;
}
"
```

AI might give you optimized code without explaining the changes.

**✅ With Chain-of-Thought:**
```
"Optimize this function to find duplicates in an array:

function findDuplicates(arr) {
  const duplicates = [];
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] === arr[j] && !duplicates.includes(arr[i])) {
        duplicates.push(arr[i]);
      }
    }
  }
  return duplicates;
}

Think step-by-step:
1. What's the current time complexity and why?
2. What data structure could improve lookup performance?
3. How can we track items we've already seen?
4. What's the optimized approach?

Then provide the improved implementation with complexity analysis."
```

**Expected Response Format:**
```
1. Current complexity is O(n³) because:
   - Outer loop: O(n)
   - Inner loop: O(n)
   - includes() check: O(n)

2. A Set or Map would give O(1) lookups instead of O(n)

3. We can track seen items and duplicates separately using Sets

4. Optimized approach: Single pass with frequency counting

Improved implementation:
function findDuplicates(arr) {
  const seen = new Set();
  const duplicates = new Set();
  
  for (const item of arr) {
    if (seen.has(item)) {
      duplicates.add(item);
    } else {
      seen.add(item);
    }
  }
  
  return Array.from(duplicates);
}

Time Complexity: O(n)
Space Complexity: O(n)
```

### Example 2: Debugging Complex Issues

**✅ Chain-of-Thought for Debugging:**
```
"This React component causes infinite re-renders:

const UserProfile = ({ userId }) => {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    fetchUser(userId).then(data => setUser(data));
  });
  
  return <div>{user?.name}</div>;
};

Think through the debugging process step-by-step:
1. What causes an effect to re-run?
2. What happens when state updates inside an effect?
3. What's missing from this useEffect?
4. How should we fix it?

Then provide the corrected code with explanation."
```

### Example 3: Architecture Decisions

**✅ Chain-of-Thought for Design:**
```
"I need to implement a caching layer for API responses.

Think through the design step-by-step:
1. Where should the cache live (memory, localStorage, Redis)?
2. What cache invalidation strategy should we use?
3. How do we handle cache misses?
4. Should we cache errors?
5. How do we prevent cache stampede?
6. What about cache size limits?

Consider our context:
- React frontend application
- Average API response: 5KB
- API rate limit: 100 requests/minute
- Data updates infrequently (hourly)

Then provide the implementation approach with rationale."
```

### Example 4: Security Review

**✅ Chain-of-Thought for Security:**
```
"Review this authentication code for security issues:

app.post('/login', (req, res) => {
  const { username, password } = req.body;
  const user = db.query('SELECT * FROM users WHERE username = "' + username + '"');
  
  if (user && user.password === password) {
    res.json({ token: user.id });
  } else {
    res.status(401).json({ error: 'Invalid credentials' });
  }
});

Think step-by-step through potential security vulnerabilities:
1. How is the SQL query constructed?
2. How is password comparison done?
3. What's used as the authentication token?
4. What's missing from the response?
5. Are there any timing attacks possible?
6. What about rate limiting?

Then provide a secure implementation."
```

### Chain-of-Thought Best Practices

✅ **DO:**
- Use for complex algorithms and optimization
- Use when debugging tricky issues
- Use for architecture and design decisions
- Use for security reviews
- Ask for trade-off analysis

❌ **DON'T:**
- Use for simple, straightforward tasks
- Overuse on boilerplate generation
- Forget to ask for the actual code after reasoning

### Practice Exercise 2

**Your Task:** Write a chain-of-thought prompt for this scenario.

```
You need to implement a debounce function for a search input 
that makes API calls. The function should prevent excessive 
API calls while typing.
```

Write the prompt with step-by-step thinking guide.

<details>
<summary>Click to see example answer</summary>

```
"Implement a debounce function for a search input that makes API calls.

Requirements:
- Search input triggers API call on every keystroke (currently)
- Need to wait until user stops typing
- Should cancel pending calls if user continues typing

Think step-by-step:
1. What is debouncing and how does it work?
2. How do we track if user is still typing?
3. How do we cancel previous pending calls?
4. What should the delay be (reasonable default)?
5. Should we handle leading/trailing edge?
6. How do we clean up timers?

Use TypeScript and consider React hooks integration.

Then provide the implementation with usage example."
```

</details>

---

## 3. Role/Persona Prompting

**Concept**: Ask AI to adopt a specific expert role or perspective. This influences the focus, depth, and style of the response.

### Why Role Prompting Works

- Activates relevant knowledge domains
- Adjusts response style and priorities
- Provides appropriate level of detail
- Helps get specialized insights

### Available Personas

- **Senior [Language] Developer**: Best practices, patterns, maintainability
- **Security Expert**: Vulnerabilities, attack vectors, secure coding
- **Performance Engineer**: Optimization, profiling, efficiency
- **Code Reviewer**: Quality, readability, potential issues
- **DevOps Engineer**: Deployment, infrastructure, automation
- **Accessibility Expert**: WCAG compliance, screen readers, inclusive design
- **Junior Developer**: Detailed explanations, learning-focused

### Template

```
"Act as a [role] with expertise in [domain].

[Your task/question]

Consider [specific aspects relevant to that role]."
```

### Example 1: Security Expert

**❌ Without Role:**
```
"Review this login function"
```

**✅ With Security Expert Role:**
```
"Act as a security expert reviewing authentication code.

Review this login function:

export const login = async (email: string, password: string) => {
  const user = await db.query(
    `SELECT * FROM users WHERE email = '${email}'`
  );
  
  if (user && user.password === password) {
    const token = jwt.sign({ userId: user.id }, 'secret123');
    return { token, user };
  }
  throw new Error('Invalid credentials');
};

Identify security vulnerabilities and provide a secure implementation.
Focus on:
- SQL injection risks
- Password handling
- Token security
- Information disclosure
- Timing attacks"
```

### Example 2: Performance Engineer

**✅ Performance-Focused Review:**
```
"Act as a performance engineer optimizing React applications.

Review this component for performance issues:

const Dashboard = ({ userId }) => {
  const [data, setData] = useState([]);
  
  useEffect(() => {
    fetch(`/api/data/${userId}`).then(res => res.json()).then(setData);
  }, [userId]);
  
  const processedData = data.map(item => ({
    ...item,
    calculated: expensiveCalculation(item.value)
  }));
  
  return (
    <div>
      {processedData.map(item => (
        <Card key={item.id} data={item} onClick={() => handleClick(item)} />
      ))}
    </div>
  );
};

Identify performance bottlenecks and provide optimized version.
Consider:
- Unnecessary re-renders
- Expensive calculations
- Component memoization
- List rendering optimization
- Event handler optimization"
```

### Example 3: Code Reviewer

**✅ Code Review Perspective:**
```
"Act as a senior code reviewer evaluating code quality.

Review this pull request code:

class UserService {
  constructor() {
    this.users = [];
  }
  
  addUser(user) {
    this.users.push(user);
    return user;
  }
  
  getUser(id) {
    return this.users.find(u => u.id == id);
  }
  
  updateUser(id, data) {
    const index = this.users.findIndex(u => u.id == id);
    if (index !== -1) {
      this.users[index] = { ...this.users[index], ...data };
    }
    return this.users[index];
  }
}

Provide feedback on:
- Code style and consistency
- Potential bugs
- Type safety concerns
- Error handling
- API design
- Best practices violations

Suggest improvements with explanations."
```

### Example 4: Accessibility Expert

**✅ Accessibility Focus:**
```
"Act as an accessibility expert reviewing web applications for WCAG compliance.

Review this modal component:

const Modal = ({ isOpen, onClose, children }) => {
  if (!isOpen) return null;
  
  return (
    <div className="modal-overlay" onClick={onClose}>
      <div className="modal-content">
        <button className="close-btn" onClick={onClose}>×</button>
        {children}
      </div>
    </div>
  );
};

Identify accessibility issues and provide an improved version.
Consider:
- Screen reader support
- Keyboard navigation
- Focus management
- ARIA attributes
- Color contrast
- Semantic HTML"
```

### Example 5: Junior Developer (Explaining Mode)

**✅ Learning-Focused Explanation:**
```
"Act as a senior developer explaining to a junior developer.

Explain how async/await works in JavaScript.

The junior developer knows:
- Basic function syntax
- Callbacks
- What promises are (conceptually)

They don't know:
- How to use async/await syntax
- Error handling with try/catch
- How it relates to promises

Use simple language, provide examples, and explain step-by-step."
```

### Multiple Perspectives

You can ask for multiple expert perspectives:

```
"Review this authentication implementation from three perspectives:

[paste code]

1. As a security expert: Identify vulnerabilities
2. As a performance engineer: Identify bottlenecks
3. As a code reviewer: Assess code quality

Provide separate analysis from each perspective."
```

### Role Prompting Best Practices

✅ **DO:**
- Choose roles relevant to your specific concern
- Combine with chain-of-thought for deeper analysis
- Be specific about what aspects to focus on
- Use different roles for different review passes

❌ **DON'T:**
- Use vague roles like "expert" without specifying domain
- Choose conflicting roles (e.g., "optimize for speed AND size" without priority)
- Expect AI to actually "be" that person (it's a framing technique)

### Practice Exercise 3

**Your Task:** Choose appropriate roles for these scenarios:

1. Reviewing a database schema design → Role: ___________
2. Optimizing a slow web page → Role: ___________
3. Reviewing form validation for errors → Role: ___________
4. Explaining Redux to a beginner → Role: ___________

<details>
<summary>Click to see suggested answers</summary>

1. **Database Architect** or **Senior Backend Developer**
   - Focus on normalization, indexing, relationships, scalability

2. **Performance Engineer** or **Frontend Performance Expert**
   - Focus on bundle size, rendering, network requests, Core Web Vitals

3. **Security Expert** or **UX Engineer**
   - Security: injection risks, validation bypasses
   - UX: user experience, error messages, edge cases

4. **Senior Developer** or **Technical Educator**
   - Focus on clear explanations, analogies, progressive complexity

</details>

---

## 4. Constraint-Based Prompting

**Concept**: Explicitly specify boundaries, limitations, and requirements upfront. This prevents AI from making assumptions that don't fit your needs.

### Types of Constraints

1. **Technical Stack**: Languages, frameworks, libraries to use/avoid
2. **Performance**: Time complexity, memory limits, response time
3. **Size**: Line count, file size, bundle size
4. **Style**: Coding conventions, naming patterns
5. **Dependencies**: What libraries are allowed/forbidden
6. **Compatibility**: Browser support, version requirements
7. **Environment**: Server, serverless, edge, browser
8. **Business**: Budget limits, compliance requirements

### Template

```
"[Task description]

Constraints:
- [Constraint type 1]
- [Constraint type 2]
- [Constraint type 3]
...
"
```

### Example 1: Serverless Function

**✅ Environment Constraints:**
```
"Create a function to resize images uploaded by users.

Constraints:
- Must run in AWS Lambda (Node.js 18)
- Maximum execution time: 10 seconds
- Memory limit: 512MB
- Cannot use filesystem (use /tmp with 512MB limit)
- Must handle images up to 10MB
- Output format: JPEG, optimized for web
- Use sharp library (already in dependencies)
- Return error for unsupported formats
- Include proper logging for CloudWatch

Input: S3 event trigger with image upload
Output: Resized image written back to S3"
```

### Example 2: Frontend Bundle Size

**✅ Size and Performance Constraints:**
```
"Create a date picker component for our React app.

Constraints:
- Bundle size: Maximum 15KB (gzipped)
- No external libraries (lodash, moment.js, date-fns forbidden due to size)
- Browser support: Chrome 90+, Firefox 88+, Safari 14+
- Mobile-friendly (touch support required)
- Performance: Must handle 200+ date cells without lag
- Accessibility: WCAG 2.1 AA compliant
- Use only React hooks (no class components)
- TypeScript required

The component will be code-split loaded, so bundle size is critical."
```

### Example 3: Legacy System Integration

**✅ Compatibility Constraints:**
```
"Create a function to integrate with our legacy SOAP API.

Constraints:
- Must use Node.js (cannot modify the SOAP service)
- The SOAP service uses WS-Security with username token
- Service response time is slow (3-5 seconds typical)
- Service occasionally times out (need retry logic)
- Maximum 3 retry attempts with exponential backoff
- Must parse XML response (no JSON available)
- Handle SOAP faults gracefully
- Log all requests for audit trail
- Environment: TypeScript, Node 16 LTS
- Available libraries: soap, xml2js, axios

Timeout constraint: 15 seconds total (including retries)"
```

### Example 4: Code Style Constraints

**✅ Style and Convention Constraints:**
```
"Create a user validation module.

Constraints - Code Style:
- Use functional programming style (no classes)
- Use pure functions (no side effects)
- All functions must be exported individually (no default exports)
- Maximum function length: 15 lines
- Use descriptive names (no abbreviations except common ones: id, url, api)
- All functions must have JSDoc comments
- Use explicit return types in TypeScript
- Prefer early returns over nested if statements
- Use const for all variables (no let unless absolutely necessary)
- Error messages must be user-friendly (no technical jargon)

Follow the style in our existing utils/validation.ts file."
```

### Example 5: Algorithm Constraints

**✅ Performance Constraints:**
```
"Implement a search function for our product catalog.

Constraints - Performance:
- Dataset: 100,000 products
- Search must return results in < 100ms
- Memory limit: 256MB
- No external search services (Elasticsearch, Algolia)
- Must handle: partial matching, typos (1-2 char difference), synonyms
- Return top 20 results ranked by relevance
- Browser environment (client-side search)
- Must work offline (data cached locally)

Time complexity requirement: Better than O(n) per search
Space complexity: O(n) acceptable for index building

Preprocessing allowed during app initialization."
```

### Example 6: Security and Compliance Constraints

**✅ Security Constraints:**
```
"Create a function to handle payment processing.

Constraints - Security & Compliance:
- PCI DSS compliant (never log or store card numbers)
- Must use Stripe API (company policy)
- All sensitive data must be encrypted at rest
- API calls must use HTTPS only
- Rate limiting: max 100 payment attempts per user per hour
- Must include fraud detection (3D Secure for international cards)
- Transaction logging required (audit trail)
- GDPR compliant (data retention: 7 years for tax, user data deleteable)
- Must handle: USD, EUR, GBP
- Environment: Node.js backend, PostgreSQL database
- All errors returned to client must be sanitized (no sensitive info)

No test mode in production environment."
```

### Constraint Priority

When constraints conflict, specify priority:

```
"Create a caching implementation.

Constraints (in priority order):
1. Data consistency (critical) - stale data is unacceptable
2. Performance (high) - < 50ms cache lookups
3. Memory usage (medium) - prefer memory over disk if needed
4. Code simplicity (low) - complexity acceptable if it serves 1-3

If you must choose between performance and memory, choose performance."
```

### Constraint Negotiation

Ask AI to evaluate feasibility:

```
"Create an image carousel component.

Desired Constraints:
- Bundle size: < 5KB gzipped
- Supports infinite scroll
- Smooth 60fps animations
- Touch gestures (swipe, pinch-zoom)
- Lazy loading
- Thumbnail preview
- No external libraries

Are these constraints realistic together? If not, what trade-offs would you recommend?"
```

### Constraint-Based Best Practices

✅ **DO:**
- List all constraints upfront (not scattered through conversation)
- Specify priority when constraints might conflict
- Include "why" for unusual constraints (helps AI optimize)
- Be realistic (impossible constraints waste time)
- Include environment details (browser, server, edge)

❌ **DON'T:**
- Add conflicting constraints without priority
- Forget to mention critical limitations
- Use vague constraints ("must be fast", "must be small")
- Assume AI knows your constraints

### Practice Exercise 4

**Your Task:** Add appropriate constraints to this vague request:

```
"Create a function to send email notifications"
```

Consider:
- Your tech stack environment
- Performance requirements
- Email service constraints
- Security needs
- Rate limiting
- Error handling

Write the full constrained prompt.

<details>
<summary>Click to see example answer</summary>

```
"Create a function to send email notifications.

Context:
- Backend: Node.js with TypeScript
- Email service: SendGrid (company account)
- Use case: Password resets, account confirmations

Constraints:
- Environment: AWS Lambda with 5 second timeout
- SendGrid rate limit: 100 emails/second (our plan)
- Must implement queue for bulk sends (don't send 1000 emails in parallel)
- Retry failed sends (max 3 attempts)
- Must sanitize email addresses (validation)
- HTML and plain text versions required
- Template system: use Handlebars (already in project)
- Logging: Log sent emails (email address, template used, timestamp)
- Security: Never log email content (may contain sensitive data)
- Error handling: Return success/failure status, don't throw
- Must respect user unsubscribe status (check before sending)
- Maximum email size: 30MB (SendGrid limit)

Return type: Promise<{ success: boolean, messageId?: string, error?: string }>"
```

</details>

---

## 5. Iterative Refinement

**Concept**: Start with a simple, working solution, then progressively improve it through multiple rounds. Build complexity gradually rather than all at once.

### Why Iterative Refinement Works

- Easier to verify each step is correct
- Maintains working code at each stage
- You can stop at "good enough" rather than over-engineering
- Better learning experience (see evolution of code)
- Reduces overwhelming complexity

### Template

```
Round 1: "Create a basic [X] that [core functionality]"
Round 2: "Now add [first enhancement]"
Round 3: "Add [second enhancement]"
Round 4: "Improve [specific aspect]"
...
```

### Example 1: Building a Search Feature

**Round 1 - Basic Functionality:**
```
"Create a basic search function for an array of products.

Products have: id, name, price
Just implement simple exact name matching (case-insensitive)

Return matching products."
```

**Round 2 - Add Partial Matching:**
```
"Great! Now update it to support partial matching.

Example: "lapt" should match "Laptop Computer"
Keep case-insensitive."
```

**Round 3 - Add Multiple Fields:**
```
"Now extend the search to also match against product description and category fields.

Products now have: id, name, description, category, price
Any field match should return the product."
```

**Round 4 - Add Ranking:**
```
"Add relevance ranking to the results:
- Exact name matches should rank highest
- Name partial matches rank second
- Description matches rank third
- Category matches rank last

Return results sorted by rank, then alphabetically."
```

**Round 5 - Performance Optimization:**
```
"Optimize for performance. Our dataset has 50,000 products.

Current implementation re-filters the entire array on each search.
Use an index or other data structure to improve search speed."
```

**Round 6 - Add Fuzzy Matching:**
```
"Add fuzzy matching to handle typos (1-2 character difference).

Examples:
- "Laptos" should still find "Laptops"
- "Komputer" should find "Computer"

But don't slow down the search significantly."
```

### Example 2: Authentication System

**Round 1 - Basic Login:**
```
"Create a basic login function.

Accept email and password
Check against hardcoded user for now:
{ email: 'test@example.com', password: 'password123' }

Return true if match, false otherwise."
```

**Round 2 - Add Password Hashing:**
```
"Update to use bcrypt for password hashing instead of plain text.

Assume the stored password is already hashed.
Use bcrypt.compare() to check."
```

**Round 3 - Add Database:**
```
"Replace hardcoded user with database lookup.

Use provided db.query() function (returns Promise)
Look up user by email from 'users' table."
```

**Round 4 - Add JWT Tokens:**
```
"Instead of returning true/false, return a JWT token on successful login.

Token should include: userId, email
Use jwt.sign() with our secret key from environment."
```

**Round 5 - Add Error Handling:**
```
"Add proper error handling:
- User not found → 'Invalid credentials' (don't reveal which field is wrong)
- Wrong password → 'Invalid credentials'
- Database errors → 'Service temporarily unavailable'

Throw custom error objects with status codes."
```

**Round 6 - Add Rate Limiting:**
```
"Add rate limiting to prevent brute force attacks:
- Maximum 5 login attempts per email per 15 minutes
- Use Redis or in-memory store
- Return 'Too many attempts' error when exceeded."
```

### Example 3: React Component Evolution

**Round 1 - Static Component:**
```
"Create a simple Button component in React.

Props: label (string)
Just render a button element with the label."
```

**Round 2 - Add Click Handler:**
```
"Add onClick functionality.

New prop: onClick (function)
Call it when button is clicked."
```

**Round 3 - Add Variants:**
```
"Add visual variants:

New prop: variant ('primary' | 'secondary' | 'danger')
Apply different CSS classes based on variant."
```

**Round 4 - Add Loading State:**
```
"Add loading state:

New prop: isLoading (boolean)
When loading:
- Show a spinner icon
- Disable the button
- Change cursor to not-allowed"
```

**Round 5 - Add Accessibility:**
```
"Improve accessibility:

- Support disabled prop
- Add proper aria-attributes
- Support keyboard navigation
- Add aria-busy when loading
- Ensure proper focus styles"
```

**Round 6 - Add TypeScript:**
```
"Convert to TypeScript:

- Create proper interface for props
- Add type safety for variant values
- Export types for consumers"
```

### When to Use Iterative Refinement

**✅ Good Use Cases:**
- Learning new patterns or technologies
- Building complex features from scratch
- When requirements are not fully clear yet
- Teaching/mentoring scenarios
- Exploratory development

**❌ Poor Use Cases:**
- Simple, well-defined tasks (just write the full solution)
- When you already have a clear, complete spec
- Time-sensitive emergency fixes
- Boilerplate code generation

### Iterative Refinement Best Practices

✅ **DO:**
- Ensure each iteration produces working code
- Build on previous iteration (don't restart)
- Increase complexity gradually
- Test/verify between iterations
- Stop when "good enough" (don't over-engineer)

❌ **DON'T:**
- Make huge leaps between iterations
- Add too many features in one iteration
- Skip basic working version
- Keep going beyond project needs

### Practice Exercise 5

**Your Task:** Plan iteration sequence for building a shopping cart.

Features needed:
- Add/remove items
- Update quantities
- Calculate totals
- Apply discount codes
- Persist to localStorage
- Show item count badge
- Animated additions

Plan 5-6 iterations with clear progression:

1. ___________
2. ___________
3. ___________
4. ___________
5. ___________
6. ___________

<details>
<summary>Click to see example answer</summary>

1. **Basic cart structure**: Array of items with add/remove functions
2. **Quantity management**: Update quantities, prevent negative values
3. **Price calculation**: Calculate subtotal, tax, total
4. **Discount codes**: Apply percentage or fixed amount discounts
5. **Persistence**: Save/load cart from localStorage on init and changes
6. **UI enhancements**: Item count badge, animations for add/remove

Each iteration builds on the previous, maintaining working functionality.

</details>

---

## 6. Code Context

**Concept**: Share relevant existing code to ensure AI's output matches your project's patterns, style, and architecture.

### Why Code Context Matters

- Maintains consistency across codebase
- AI learns your specific patterns and conventions
- Prevents suggesting incompatible approaches
- Reduces need to explain every detail
- Results in code that "fits" immediately

### What Context to Share

1. **Similar existing functionality** (most useful)
2. **Shared utilities/helpers** used across project
3. **Type definitions** and interfaces
4. **Error handling patterns**
5. **Import structures** and file organization
6. **Configuration** and constants

### Example 1: API Route Consistency

**✅ With Context from Existing Routes:**
```
"Create a new API route for updating product prices.

Here's our existing route structure from routes/products.ts:

import { Router } from 'express';
import { authenticate } from '../middleware/auth';
import { validate } from '../middleware/validation';
import { productService } from '../services/productService';
import { logger } from '../utils/logger';
import { productSchema } from '../schemas/product';

const router = Router();

router.get('/:id', authenticate, async (req, res) => {
  try {
    const product = await productService.getById(req.params.id);
    
    if (!product) {
      return res.status(404).json({ 
        success: false, 
        error: 'Product not found' 
      });
    }
    
    res.json({ success: true, data: product });
  } catch (error) {
    logger.error('Failed to fetch product:', error);
    res.status(500).json({ 
      success: false, 
      error: 'Internal server error' 
    });
  }
});

router.post('/', authenticate, validate(productSchema), async (req, res) => {
  try {
    const product = await productService.create(req.body);
    res.status(201).json({ success: true, data: product });
  } catch (error) {
    logger.error('Failed to create product:', error);
    res.status(500).json({ 
      success: false, 
      error: 'Internal server error' 
    });
  }
});

export default router;

Following this exact structure, create a PUT route at '/:id/price' 
that updates product price. Requires authentication and validation."
```

### Example 2: Component Structure

**✅ With Context from Existing Component:**
```
"Create a UserAvatar component.

Here's our existing ProfileCard component to match the style:

import React from 'react';
import { User } from '@/types/user';
import styles from './ProfileCard.module.css';

interface ProfileCardProps {
  user: User;
  onEdit?: () => void;
  variant?: 'compact' | 'full';
}

export const ProfileCard: React.FC<ProfileCardProps> = ({ 
  user, 
  onEdit,
  variant = 'full' 
}) => {
  return (
    <div className={styles.card} data-variant={variant}>
      <div className={styles.header}>
        <h3 className={styles.name}>{user.name}</h3>
        {onEdit && (
          <button 
            onClick={onEdit} 
            className={styles.editButton}
            aria-label="Edit profile"
          >
            Edit
          </button>
        )}
      </div>
      <p className={styles.email}>{user.email}</p>
    </div>
  );
};

Follow this pattern: same folder structure, same naming conventions,
same TypeScript style, same CSS modules approach, same prop patterns.

UserAvatar props:
- user (User type)
- size ('small' | 'medium' | 'large')
- showName (boolean, optional)"
```

### Example 3: Error Handling Pattern

**✅ With Context from Error Utilities:**
```
"Add error handling to the payment processing function.

Our project error handling pattern from utils/errors.ts:

export class AppError extends Error {
  constructor(
    message: string,
    public statusCode: number,
    public code: string,
    public isOperational: boolean = true
  ) {
    super(message);
    Object.setPrototypeOf(this, AppError.prototype);
  }
}

export class ValidationError extends AppError {
  constructor(message: string) {
    super(message, 400, 'VALIDATION_ERROR');
  }
}

export class NotFoundError extends AppError {
  constructor(resource: string) {
    super(`${resource} not found`, 404, 'NOT_FOUND');
  }
}

export class PaymentError extends AppError {
  constructor(message: string, public originalError?: any) {
    super(message, 402, 'PAYMENT_FAILED');
  }
}

And our error handler middleware:

export const errorHandler = (
  error: Error,
  req: Request,
  res: Response,
  next: NextFunction
) => {
  if (error instanceof AppError) {
    logger.warn('Operational error:', {
      code: error.code,
      message: error.message,
      path: req.path
    });
    
    return res.status(error.statusCode).json({
      success: false,
      error: {
        code: error.code,
        message: error.message
      }
    });
  }
  
  logger.error('Unexpected error:', error);
  res.status(500).json({
    success: false,
    error: {
      code: 'INTERNAL_ERROR',
      message: 'An unexpected error occurred'
    }
  });
};

Use these exact error classes and patterns in the payment function."
```

### Example 4: Testing Patterns

**✅ With Context from Existing Tests:**
```
"Write tests for the new calculateDiscount function.

Follow our existing test pattern from userService.test.ts:

import { userService } from './userService';
import { db } from '../config/database';
import { ValidationError, NotFoundError } from '../utils/errors';

jest.mock('../config/database');

describe('UserService', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });
  
  describe('createUser', () => {
    it('should create a user with valid data', async () => {
      const userData = {
        email: 'test@example.com',
        name: 'Test User',
        password: 'SecurePass123!'
      };
      
      const mockUser = { id: '123', ...userData, createdAt: new Date() };
      (db.query as jest.Mock).mockResolvedValue({ rows: [mockUser] });
      
      const result = await userService.createUser(userData);
      
      expect(result).toEqual(mockUser);
      expect(db.query).toHaveBeenCalledWith(
        expect.stringContaining('INSERT INTO users'),
        expect.arrayContaining([userData.email, userData.name])
      );
    });
    
    it('should throw ValidationError for invalid email', async () => {
      const userData = {
        email: 'invalid-email',
        name: 'Test User',
        password: 'SecurePass123!'
      };
      
      await expect(userService.createUser(userData))
        .rejects
        .toThrow(ValidationError);
    });
  });
});

Use the same:
- Jest mocking approach
- describe/it structure
- Naming conventions ('should...')
- Mock data patterns
- Error testing approach
- Matchers style"
```

### How Much Context to Include

**Too Little ❌:**
```
"Create a new component"
```
No context = AI guesses everything

**Too Much ❌:**
```
[Paste entire 500-line file]
```
AI might miss key parts, or context window fills up

**Just Right ✅:**
```
"Create a new component following this pattern:

[Paste most relevant 30-50 lines showing structure/style]

Key points:
- We use TypeScript with strict mode
- Components in separate files with .tsx extension
- Props interface defined above component
- Use functional components with hooks
- CSS modules for styling
- Export as named export"
```

### Code Context Best Practices

✅ **DO:**
- Share most similar existing code
- Include imports structure
- Show error handling patterns
- Share type definitions
- Paste relevant utilities
- Explain "why" if pattern is unusual

❌ **DON'T:**
- Share full files (excerpt key parts)
- Share outdated/deprecated code as examples
- Mix multiple different patterns
- Forget to explain which parts to match

### Practice Exercise 6

You have this existing code:

```typescript
// services/authService.ts
export class AuthService {
  private tokenStore: Map<string, TokenData>;
  
  constructor(private config: AuthConfig) {
    this.tokenStore = new Map();
  }
  
  async generateToken(userId: string): Promise<string> {
    // implementation
  }
}
```

**Your Task:** Write a prompt to create a new `SessionService` that follows the same pattern.

<details>
<summary>Click to see example answer</summary>

```
"Create a new SessionService following our existing service pattern.

Example from services/authService.ts:

export class AuthService {
  private tokenStore: Map<string, TokenData>;
  
  constructor(private config: AuthConfig) {
    this.tokenStore = new Map();
  }
  
  async generateToken(userId: string): Promise<string> {
    // implementation
  }
}

Follow this pattern for SessionService:
- Class-based service
- Private property for data store (use Map for sessions)
- Constructor accepts config object
- Private properties for internal state
- Async methods for operations
- TypeScript with proper types

SessionService should:
- Store session data in a Map (sessionId -> SessionData)
- Constructor accepts SessionConfig
- Methods: createSession, getSession, deleteSession, cleanupExpired
- Export as named export"
```

</details>

---

## 7. Explicit Output Format

**Concept**: Tell AI exactly how you want the response structured. Gets you the right format on the first try instead of reformatting later.

### Why Explicit Format Matters

- Saves time (no back-and-forth to fix format)
- Ensures you get all necessary parts
- Makes AI response immediately usable
- Especially important for complex requests

### What to Specify

1. **Code structure**: Function, class, module, hook, etc.
2. **Language/syntax**: TypeScript, ES6, JSX, etc.
3. **Documentation**: JSDoc, inline comments, README
4. **Tests**: Test file, testing approach, coverage
5. **Examples**: Usage examples, edge cases
6. **Explanations**: Include rationale, trade-offs

### Example 1: Function with Documentation

**❌ Vague Request:**
```
"Create a function to validate passwords"
```

**✅ Explicit Format:**
```
"Create a password validation function.

Output format:
1. TypeScript function with:
   - Explicit parameter types
   - Return type: { valid: boolean; errors: string[] }
   - Exported as named export

2. JSDoc comment including:
   - Description
   - @param documentation
   - @returns documentation
   - @example with 2-3 examples

3. Inline comments explaining regex patterns

4. Separate interface for return type

Format like this:
```typescript
interface ValidationResult {
  // ...
}

/**
 * JSDoc here
 */
export const validatePassword = (password: string): ValidationResult => {
  // implementation
};
```
```

### Example 2: React Component with Multiple Files

**✅ Explicit Multi-File Format:**
```
"Create a Modal component for our React app.

Output format - provide 3 separate files:

1. **Modal.tsx** (component file):
```typescript
// Component implementation
// Use named export
// TypeScript interface for props
```

2. **Modal.module.css** (styles):
```css
/* CSS modules format */
/* Classes: .overlay, .modal, .closeButton */
```

3. **Modal.test.tsx** (tests):
```typescript
// Jest + React Testing Library
// Test: renders, closes on overlay click, keyboard navigation
```

Also provide a usage example showing how to import and use it."
```

### Example 3: API Documentation

**✅ Explicit Documentation Format:**
```
"Document the User API endpoints.

Output format (Markdown):

# User API

## Endpoints

For each endpoint provide:

### [METHOD] /endpoint/path

**Description**: What it does

**Authentication**: Required/Optional/None

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|

**Request Body:** (if applicable)
```json
{
  "example": "structure"
}
```

**Response:**
```json
{
  "success": true,
  "data": {}
}
```

**Error Responses:**
- 400: Description
- 404: Description

**Example:**
```bash
curl example
```

Use this format for:
- GET /users/:id
- POST /users
- PUT /users/:id
- DELETE /users/:id"
```

### Example 4: Algorithm with Explanation

**✅ Explicit Explanation Format:**
```
"Implement a function to find the longest palindromic substring.

Output format:

1. **Implementation** (TypeScript):
   - Function signature with types
   - Optimized solution (better than O(n³))

2. **Explanation** (markdown):
   - How the algorithm works
   - Why this approach
   - Time complexity analysis
   - Space complexity analysis

3. **Examples** (code comments):
   - 3 test cases with expected outputs
   - 1 edge case

4. **Alternative approaches** (brief):
   - Other possible solutions
   - Trade-offs of each

Structure:
```typescript
/**
 * Algorithm explanation here
 * Time: O(?)
 * Space: O(?)
 */
export const longestPalindrome = (s: string): string => {
  // implementation with comments
};

// Example 1: ...
// Example 2: ...
```

## Alternative Approaches
1. Brute force: ...
2. Dynamic programming: ...
3. Expand around center: ..."
```

### Example 5: Configuration File

**✅ Explicit Config Format:**
```
"Create a configuration file for our Express app.

Output format: TypeScript file

Structure:
1. Interface defining config shape
2. Development config object
3. Production config object
4. Function to get current config based on NODE_ENV
5. Export both interface and getConfig function

Include these config sections:
- server (port, host)
- database (url, poolSize, timeout)
- jwt (secret, expiresIn)
- logging (level, format)
- cors (origins, credentials)

Format:
```typescript
interface AppConfig {
  // sections here
}

const development: AppConfig = {
  // dev values
};

const production: AppConfig = {
  // prod values
};

export const getConfig = (): AppConfig => {
  // selection logic
};

export type { AppConfig };
```

Add inline comments explaining each config option."
```

### Format Specification Shortcuts

**Request types:**
- "As a TypeScript interface"
- "As a JSON object"
- "As a JavaScript class"
- "As a React functional component"
- "As Express middleware"
- "As a Jest test suite"
- "As Markdown documentation"
- "As SQL DDL statements"

**Documentation requests:**
- "With JSDoc comments"
- "With inline explanations"
- "With usage examples"
- "With type annotations"
- "With error handling documentation"

**Test requests:**
- "With unit tests"
- "With test cases included"
- "With mocked dependencies"
- "With integration test example"

### Output Format Best Practices

✅ **DO:**
- Specify file structure for multi-file outputs
- Request documentation style explicitly
- Mention code style preferences
- Ask for examples when needed
- Specify test format if needed

❌ **DON'T:**
- Assume AI knows your preferred format
- Leave export style ambiguous
- Forget to specify language/dialect
- Skip format specification for complex requests

### Practice Exercise 7

**Your Task:** Write an explicit output format specification for:

"Create a utility to debounce function calls"

Specify:
- Code structure
- Documentation style
- Examples
- Tests

Write your format specification:

<details>
<summary>Click to see example answer</summary>

```
"Create a utility function to debounce function calls.

Output format:

1. **debounce.ts** - Implementation file:
```typescript
/**
 * [JSDoc with description, @param, @returns, @example]
 */
export const debounce = <T extends (...args: any[]) => any>(
  func: T,
  delay: number
): (...args: Parameters<T>) => void => {
  // Implementation with inline comments
};
```

Requirements:
- TypeScript with generics
- Preserves function signature
- Returns cleanup function
- Named export

2. **Usage examples** (as code comments):
```typescript
// Example 1: Search input
// Example 2: Window resize
// Example 3: API call
```

3. **debounce.test.ts** - Test file:
```typescript
// Jest tests
// Test: delays execution
// Test: cancels previous calls
// Test: preserves arguments
// Test: cleanup works
// Use jest.useFakeTimers()
```

4. **Brief explanation**: How debouncing works and when to use it (2-3 sentences)"
```

</details>

---

## 8. Combining Techniques

**The Real Power**: Combining multiple techniques creates prompts that consistently deliver excellent results.

### Common Combinations

### Combination 1: Role + Chain-of-Thought

**Pattern**: Expert perspective with detailed reasoning

```
"Act as a security expert reviewing authentication code.

Review this login implementation:
[code]

Think step-by-step:
1. Identify security vulnerabilities
2. Rate severity (Critical/High/Medium/Low)
3. Explain potential exploits
4. Recommend specific fixes

Then provide secure implementation."
```

### Combination 2: Few-Shot + Constraints

**Pattern**: Learn from examples while respecting limits

```
"Create an API endpoint following our patterns.

Example endpoint (userController.ts):
[paste example code]

Constraints:
- Must use our error handling pattern
- Response format: { success, data, error }
- Authentication required
- Rate limit: 100 req/min per user
- Validate input with Joi

Create POST /products endpoint following these patterns and constraints."
```

### Combination 3: Role + Iterative + Explicit Format

**Pattern**: Expert builds solution progressively in specific format

```
"Act as a senior React developer.

Build a DataTable component iteratively:

Round 1: Basic table structure (TypeScript, props interface, render rows)
Round 2: Add sorting
Round 3: Add pagination
Round 4: Add filtering

For each round provide:
1. Updated component code
2. Props interface
3. Brief explanation of changes

Use functional components with hooks."
```

### Combination 4: Chain-of-Thought + Constraints + Code Context

**Pattern**: Logical reasoning within project constraints and style

```
"Optimize this database query (from our productService.ts):

[paste current query implementation]

Context - Our existing optimized query pattern (orderService.ts):
[paste example of well-optimized query]

Constraints:
- PostgreSQL 14
- Must use connection pool (max 20 connections)
- Query timeout: 5 seconds
- Dataset: 1M products, 10M orders
- READ COMMITTED isolation level

Think step-by-step:
1. Identify current bottlenecks
2. Analyze query plan
3. Suggest index strategy
4. Consider caching opportunities
5. Evaluate trade-offs

Then provide optimized implementation following our pattern from orderService.ts."
```

### Combination 5: All Techniques Together

**The Ultimate Prompt Structure:**

```
[ROLE] Act as a [expert type]

[TASK] Create/Review/Optimize [what]

[CONTEXT] Here's our existing [relevant code]:
[paste code]

[CONSTRAINTS]
- Technical: [stack, versions, libraries]
- Performance: [requirements]
- Environment: [where it runs]
- Style: [conventions]

[CHAIN-OF-THOUGHT]
Think through:
1. [consideration]
2. [consideration]
3. [consideration]

[FEW-SHOT] (if applicable)
Example 1: [example]
Example 2: [example]

[ITERATIVE] (if applicable)
Start with: [basic version]
Then: [enhancement 1]
Then: [enhancement 2]

[OUTPUT FORMAT]
Provide:
1. [file 1] with [specifications]
2. [file 2] with [specifications]
3. [explanation/documentation]

[CODE CONTEXT]
Match the style and patterns in [reference file]
```

### Real-World Complete Example

```
Act as a senior full-stack developer with expertise in Node.js and PostgreSQL.

Create a rate-limiting middleware for our Express API.

Existing middleware pattern (errorHandler.ts):
import { Request, Response, NextFunction } from 'express';
import { logger } from '../utils/logger';

export const errorHandler = (
  err: Error,
  req: Request,
  res: Response,
  next: NextFunction
) => {
  logger.error('Error:', { error: err.message, path: req.path });
  res.status(500).json({ success: false, error: 'Internal server error' });
};

Constraints:
- Environment: Node.js 18, Express 4.x, Redis for storage
- Rate limit: 100 requests per 15 minutes per IP
- Premium users (from req.user.tier): 1000 requests per 15 minutes
- Must return appropriate headers (X-RateLimit-Limit, X-RateLimit-Remaining, X-RateLimit-Reset)
- Response on limit exceeded: 429 status with retry-after header
- Fallback: If Redis is down, log error and allow request (fail open)

Think step-by-step:
1. How to track requests per IP efficiently in Redis?
2. How to handle premium vs free users?
3. How to calculate time windows and resets?
4. How to handle Redis failures gracefully?
5. Should we rate limit by endpoint or globally?

Then provide:

1. rateLimit.ts - Middleware implementation:
```typescript
// TypeScript with proper types
// Export as named export
// Include inline comments
```

2. Usage example:
```typescript
// How to apply to routes
```

3. Test scenarios:
```typescript
// Jest tests covering: normal flow, limit exceeded, Redis failure, premium users
```

Follow the same middleware pattern structure as errorHandler.ts (parameters, error handling, logging style).
```

### When to Combine vs Keep Simple

**Use combinations for:**
- Complex features
- Security-critical code
- Performance-sensitive code
- Maintaining consistency in large codebases
- Code reviews
- Architecture decisions

**Keep simple for:**
- Quick one-off code snippets
- Boilerplate generation
- Simple CRUD operations
- Straightforward documentation

### Combining Techniques Best Practices

✅ **DO:**
- Combine complementary techniques
- Structure prompts clearly (use headings/sections)
- Include only relevant techniques
- Build complexity gradually even in combined prompts

❌ **DON'T:**
- Use ALL techniques on every prompt (overwhelming)
- Mix conflicting directions
- Make prompts so long AI loses focus
- Forget the core task amidst all the technique layering

---

## 9. Real-World Scenarios

Let's apply these techniques to common development situations.

### Scenario 1: Implementing a New Feature

**Context:** Adding "Save for Later" functionality to an e-commerce app

**Effective Combined Prompt:**
```
Act as a senior e-commerce developer.

Implement "Save for Later" feature for shopping cart.

Existing cart structure (cartStore.ts):
[paste relevant cart code showing add/remove item patterns]

Existing API pattern (productsAPI.ts):
[paste one API call showing error handling, auth headers]

Requirements & Constraints:
- Backend: Node.js + Express + MongoDB
- Frontend: React + TikTok + React Query
- Users can move items from cart to "saved" list
- Saved items persist to database
- Show saved items count in header
- Must work offline (sync when online)
- Max 50 saved items per user

Implement in phases:
1. Database schema for saved_items collection
2. Backend API endpoints (POST /saved-items, DELETE, GET)
3. Frontend hook for saved items management
4. UI component showing saved items list

For each phase provide:
- Implementation code matching our existing patterns
- Brief explanation of approach
- Any edge cases handled

Start with phase 1 (database schema).
```

### Scenario 2: Debugging a Performance Issue

**Context:** Dashboard loads slowly with large datasets

**Effective Diagnostic Prompt:**
```
Act as a performance engineer specializing in React optimization.

Debug this slow-loading dashboard component:

[paste component code]

Context:
- Loads 10,000 data points from API
- Re-renders on every parent state change
- User reports 3-4 second render time
- Target: Under 500ms

Think step-by-step:
1. What's causing unnecessary re-renders?
2. Are there expensive calculations on every render?
3. Is the data structure optimal?
4. Are there missing optimizations (memo, callback, etc.)?
5. Should we virtualize the list?
6. Is the issue data fetching or rendering?

Provide:
1. Analysis of current performance problems
2. Optimized version with annotations explaining each optimization
3. Trade-offs of the optimizations
4. Expected performance improvement
```

### Scenario 3: Code Review

**Context:** Reviewing a teammate's PR

**Effective Review Prompt:**
```
Act as a senior code reviewer evaluating this PR for merge.

PR: Add user profile update functionality

Changed files:
1. userController.ts:
[paste code]

2. userService.ts:
[paste code]

3. UserProfileForm.tsx:
[paste code]

Review focusing on:
- Security issues (especially with user input)
- Error handling completeness
- Data validation
- TypeScript type safety
- Consistency with our patterns (reference: productController.ts style)
- Potential bugs or edge cases
- Performance concerns
- Accessibility (for React component)

For each issue found provide:
- Severity: Critical/Major/Minor
- Location: file and line
- Problem: what's wrong
- Suggestion: how to fix it
- Example code: show the fix

Also mention what was done well.
```

### Scenario 4: Learning a New Library

**Context:** First time using React Query

**Effective Learning Prompt:**
```
Act as a teacher explaining React Query to a developer familiar with useEffect for data fetching.

I need to refactor this component from useEffect to React Query:

[paste component with useEffect data fetching]

Teach iteratively:

Step 1: Explain core React Query concepts (queries, mutations, cache)
- Compare to useEffect approach
- When to use React Query vs useEffect

Step 2: Convert the data fetching to useQuery
- Show before/after
- Explain the parameters
- Show the returned values (data, isLoading, error)

Step 3: Add error handling and loading states
- React Query's built-in state management
- How it's better than manual state

Step 4: Add refetching and caching strategy
- Stale time, cache time
- How to trigger refetch

For each step:
- Explain the concept
- Show code
- Explain the benefits

Keep explanations simple but thorough.
```

### Scenario 5: Refactoring Legacy Code

**Context:** Old callback-based code needs modernization

**Effective Refactoring Prompt:**
```
Act as a senior JavaScript developer specializing in code modernization.

Refactor this legacy code to modern async/await:

[paste old callback code]

Constraints:
- Must maintain exact same functionality
- Cannot change the API surface (function names, parameters, return values)
- Must improve error handling
- Node.js 16+ (can use modern features)
- Add TypeScript types

Approach iteratively:
1. Convert callbacks to promises
2. Then convert promises to async/await
3. Then add proper error handling with try/catch
4. Then add TypeScript types
5. Finally optimize and clean up

Provide each version showing the evolution, with brief notes on what changed and why.

Start with callback to Promise conversion.
```

### Scenario 6: API Integration

**Context:** Integrating a third-party payment API

**Effective Integration Prompt:**
```
Act as a backend developer experienced with payment integrations.

Integrate Stripe payment processing into our Express API.

Our existing API patterns (orderController.ts):
[paste example controller showing error handling, validation, response format]

Our error handling (errors.ts):
[paste error classes]

Requirements & Constraints:
- Stripe API for credit card processing
- Support: USD, EUR, GBP
- Store payment intent ID in database
- Handle failed payments gracefully
- Webhook for payment confirmation
- No card data should touch our servers (use Stripe Elements)
- PCI compliance: never log card info
- Retry failed payments (max 3 attempts)
- Send email on success/failure

Think through:
1. What endpoints do we need?
2. How to structure Stripe integration (service layer)?
3. How to handle errors from Stripe API?
4. How to verify webhook authenticity?
5. How to handle idempotency?

Provide:
1. payment Service.ts (Stripe integration logic)
2. payment Controller.ts (Express routes)
3. paymentWebhook.ts (webhook handler)
4. Example frontend integration snippet

Match our existing controller and service patterns exactly.
```

### Scenario 7: Writing Tests

**Context:** Need comprehensive tests for critical function

**Effective Testing Prompt:**
```
Create comprehensive tests for this order validation function:

[paste function code]

Our testing pattern (userService.test.ts):
[paste example test showing structure, mocking, and assertions we use]

Test requirements:
- Jest framework
- Coverage: 100% (all branches)
- Mock database calls
- Mock external services (email, payment)
- Test happy path and all error cases
- Use descriptive test names ("should..." format)
- Group related tests with describe blocks

Provide tests covering:
1. Valid orders (various combinations)
2. Invalid input (missing fields, wrong types)
3. Business rule violations (max quantity, min price, etc.)
4. Database errors
5. External service failures
6. Edge cases (empty cart, huge quantities, etc.)

Structure:
- One describe block per major scenario
- Clear setup in beforeEach
- Mock reset in afterEach
- Descriptive variable names

Format: Complete test file ready to run.
```

---

## Quick Reference: Technique Selection

| Situation | Recommended Techniques |
|-----------|----------------------|
| New feature in existing codebase | Few-Shot + Code Context + Constraints |
| Complex algorithm | Chain-of-Thought + Explicit Format |
| Security review | Role (Security Expert) + Chain-of-Thought |
| Performance optimization | Role (Performance Engineer) + Chain-of-Thought + Constraints |
| Learning new concept | Role (Teacher) + Iterative + Examples |
| Refactoring | Iterative + Code Context + Constraints |
| Code review | Role (Reviewer) + Chain-of-Thought |
| API integration | Few-Shot + Constraints + Code Context |
| Writing tests | Few-Shot + Code Context + Explicit Format |
| Building from scratch | Iterative + Explicit Format |
| Quick utility function | Constraints + Explicit Format |
| Documentation | Explicit Format + Examples |

---

## Key Takeaways

1. **Few-Shot Prompting**: Show examples of your style to get consistent results
2. **Chain-of-Thought**: Ask for step-by-step reasoning for complex problems
3. **Role Prompting**: Leverage expert perspectives for specialized reviews
4. **Constraints**: Be explicit about limits, requirements, and environment
5. **Iterative Refinement**: Build complexity gradually, verify each step
6. **Code Context**: Share relevant existing code for consistency
7. **Explicit Format**: Specify exactly how you want the output structured
8. **Combine Techniques**: The real power comes from thoughtful combinations

**Remember**: Not every prompt needs every technique. Choose the ones that fit your specific situation. Start simple, add techniques as needed.

---

## Practice Challenge

**Your Final Exercise:**

You're building a notification system for your app. Users can receive notifications for:
- New messages
- Friend requests
- Comments on their posts

Write a complete prompt using at least 4 of the techniques learned today.

Include:
- Clear role or persona
- Relevant context (invent reasonable existing code patterns)
- Appropriate constraints
- Explicit output format

<details>
<summary>Click to see example answer</summary>

```
Act as a senior full-stack developer with expertise in real-time systems.

Build a notification system for our social media app.

Existing event emitter pattern (postService.ts):
import { EventEmitter } from 'events';

export class PostService extends EventEmitter {
  async createPost(postData: CreatePostInput): Promise<Post> {
    const post = await db.posts.create(postData);
    this.emit('post:created', post);
    return post;
  }
}

Existing WebSocket pattern (server.ts):
io.on('connection', (socket) => {
  socket.on('authenticate', async (token) => {
    const user = await verifyToken(token);
    socket.join(`user:${user.id}`);
  });
});

Requirements & Constraints:
- Backend: Node.js + Socket.io + PostgreSQL + Redis
- Frontend: React + Socket.io-client
- Notification types: 'message', 'friend_request', 'comment'
- Store in database (PostgreSQL)
- Real-time delivery via WebSocket
- Show unread count in header
- Mark as read functionality
- Persist to DB even if user offline
- Redis for pub/sub across multiple servers
- Max 100 notifications per user (delete oldest)

Think step-by-step:
1. Database schema for notifications table
2. How to emit notification events from various services
3. How to broadcast to correct user via WebSocket
4. How to handle offline users
5. How to sync unread count

Build iteratively:

Phase 1: Database schema and notification service foundation
- notifications table schema
- NotificationService class (create, markAsRead, getUnreadCount methods)
- Match PostService event emitter pattern

Provide:
```typescript
// Schema DDL
// NotificationService class implementation
// TypeScript types/interfaces
```

Phase 2: Real-time delivery
- WebSocket integration
- Emit notifications to connected users
- Handle offline users (store only)

Provide:
```typescript
// Socket event handler
// Integration with NotificationService
```

Start with Phase 1.
```

This combines:
- Role prompting (senior full-stack developer)
- Few-shot (showing existing patterns)
- Constraints (tech stack, requirements, limits)
- Chain-of-thought (think step-by-step)
- Iterative (phased approach)
- Code context (existing patterns to match)
- Explicit format (TypeScript code with comments)

</details>

---

## Next Steps

1. ✅ Practice each technique individually first
2. ✅ Start combining 2-3 techniques that complement each other
3. ✅ Build a personal library of effective prompt templates
4. ✅ Experiment with different combinations for your common tasks
5. ✅ Move to **phase2-advanced-prompting.md** for even more sophisticated techniques

**Mastering these practical techniques will dramatically improve your AI-assisted coding productivity!**
