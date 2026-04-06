# Phase 3: Understanding AI Agents (Basics)

**Goal**: Master the difference between chat and agents, and learn when to use each for maximum productivity  
**Prerequisites**: Complete Phase 1 (Foundations) and Phase 2 (Prompt Engineering)

---

## Table of Contents
1. [What Are AI Agents?](#what-are-ai-agents)
2. [Chat vs. Agents: Key Differences](#chat-vs-agents-key-differences)
3. [When to Use Agents vs. Chat](#when-to-use-agents-vs-chat)
4. [How Agents Work](#how-agents-work)
5. [Agent Modes in VS Code](#agent-modes-in-vs-code)
6. [Providing Clear Goals to Agents](#providing-clear-goals-to-agents)
7. [Monitoring and Guiding Agents](#monitoring-and-guiding-agents)
8. [Agent Best Practices](#agent-best-practices)
9. [Agent Tools Comparison](#agent-tools-comparison)
10. [Hands-On Exercises](#hands-on-exercises)

---

## What Are AI Agents?

### Definition

**Chat-based AI:**
- Interactive conversation
- You ask, AI responds
- One question at a time
- Reactive (waits for your input)
- Good for exploration and learning

**AI Agents:**
- Autonomous task execution
- You set a goal, agent works toward it
- Multi-step operations
- Proactive (takes initiative)
- Good for implementation and execution

### The Mental Model

**Think of it like hiring help:**

**Chat = Consultant**
```
You: "How should I refactor this authentication code?"
Chat: "Here's what I suggest... [explains approach]"
You: "OK, show me the JWT implementation"
Chat: "Here's the code... [provides code]"
You: "Now show me the middleware"
Chat: "Here's the middleware... [provides code]"
[You copy and paste each piece]
```

**Agent = Assistant**
```
You: "Refactor our authentication to use JWT"
Agent: [Reads current auth code]
Agent: [Updates login.ts]
Agent: [Updates middleware.ts]
Agent: [Updates user model]
Agent: [Updates config]
Agent: "Done! I've refactored 4 files to use JWT"
[You review the changes]
```

### Why Agents Matter

**Traditional Workflow (Chat):**
1. Ask AI for approach
2. Ask for code sample
3. Copy code
4. Paste in your file
5. Ask for next piece
6. Repeat 10-20 times
7. ⏱️ 60-90 minutes

**Agent Workflow:**
1. Give agent clear goal
2. Specify files/scope
3. Let agent work
4. Review changes
5. ⏱️ 10-15 minutes

**Time Savings: 70-85%**

---

## Chat vs. Agents: Key Differences

### Comparison Table

| Aspect | Chat | Agent |
|--------|------|-------|
| **Interaction** | Back-and-forth conversation | Set goal, watch execution |
| **Initiative** | Waits for you | Takes action autonomously |
| **File Access** | Reads files you share | Reads and **modifies** files |
| **Scope** | Single question/task | Multi-step workflows |
| **Best For** | Learning, exploring | Implementing, executing |
| **Control** | High (you do each step) | Medium (review outcomes) |
| **Speed** | Slower (manual transfer) | Faster (direct file edits) |
| **Risk** | Low (no automatic changes) | Higher (can modify wrong files) |
| **Context** | Limited to conversation | Entire workspace |

### Visual Comparison

**Chat Interaction:**
```
┌──────────┐       ┌──────────┐
│   You    │◄─────►│   Chat   │
└──────────┘       └──────────┘
     │
     │ (manually copy/paste)
     ▼
┌──────────┐
│  Files   │
└──────────┘
```

**Agent Interaction:**
```
┌──────────┐       ┌──────────┐       ┌──────────┐
│   You    │──────►│  Agent   │──────►│  Files   │
└──────────┘       └──────────┘       └──────────┘
     │                   │
     └───(monitor)───────┘
```

### Capability Differences

**What Chat Can Do:**
- ✅ Answer questions
- ✅ Explain code
- ✅ Provide code samples
- ✅ Review code you share
- ✅ Suggest approaches
- ✅ Debug errors
- ✅ Generate snippets
- ❌ Cannot modify files directly
- ❌ Cannot plan multi-step workflows
- ❌ Cannot execute sequences

**What Agents Can Do:**
- ✅ All chat capabilities, PLUS:
- ✅ Modify multiple files
- ✅ Create new files
- ✅ Plan multi-step tasks
- ✅ Execute sequences autonomously
- ✅ Coordinate changes across codebase
- ✅ Apply patterns consistently
- ⚠️ Need clear goals and scope
- ⚠️ Require monitoring

---

## When to Use Agents vs. Chat

### Use Chat When:

**1. Exploring and Learning**
```
✓ "Explain how React hooks work"
✓ "What's the difference between JWT and sessions?"
✓ "Show me examples of the strategy pattern"
✓ "How do async/await work in JavaScript?"
```

**2. Getting Advice**
```
✓ "Should I use REST or GraphQL for this API?"
✓ "What's the best way to structure this component?"
✓ "Is this code vulnerable to SQL injection?"
✓ "How can I optimize this database query?"
```

**3. Quick Code Snippets**
```
✓ "Generate a regex for email validation"
✓ "Show me a TypeScript type for user data"
✓ "Write a function to debounce API calls"
✓ "Create a Docker command to run PostgreSQL"
```

**4. Reviewing Code**
```
✓ "Review this function for bugs"
✓ "Is this code following best practices?"
✓ "Find security issues in this endpoint"
✓ "Suggest improvements to this algorithm"
```

**5. Debugging Specific Issues**
```
✓ "Why am I getting this error: [error message]"
✓ "This function returns undefined, why?"
✓ "Help me understand this stack trace"
✓ "Why isn't this CSS working?"
```

**6. One-Off Changes**
```
✓ Single function modification
✓ Adding one import
✓ Fixing a typo in one file
✓ Updating one configuration value
```

### Use Agents When:

**1. Multi-File Refactoring**
```
✓ "Rename UserProfile to UserAccount across codebase"
✓ "Convert all class components to functional components"
✓ "Replace all .then() with async/await"
✓ "Update all imports from 'utils' to '@/utils'"
```

**2. Feature Implementation**
```
✓ "Add pagination to all API endpoints"
✓ "Implement user authentication with JWT"
✓ "Add dark mode support across the app"
✓ "Create CRUD operations for Products"
```

**3. Consistent Pattern Application**
```
✓ "Add error handling to all service functions"
✓ "Add TypeScript types to all API responses"
✓ "Add JSDoc comments to all exported functions"
✓ "Update all database queries to use parameterized queries"
```

**4. Test Generation**
```
✓ "Generate unit tests for all services"
✓ "Create integration tests for auth endpoints"
✓ "Add test coverage to utils folder"
✓ "Generate E2E tests for user flow"
```

**5. Code Migration**
```
✓ "Migrate from JavaScript to TypeScript"
✓ "Update from React 17 to React 18"
✓ "Convert CSS to Tailwind classes"
✓ "Migrate from Axios to Fetch API"
```

**6. Repetitive Tasks**
```
✓ "Add logging to all controller methods"
✓ "Update all environment variable names"
✓ "Format all files with new style guide"
✓ "Add error boundaries to all route components"
```

### Decision Flow

```
Start: Do I need to modify files?
│
├─ NO → Use Chat
│        • Learning
│        • Questions
│        • Code review
│        • Debugging help
│
└─ YES → How many files?
         │
         ├─ 1 file, simple → Use Chat
         │                    Copy/paste is fine
         │
         └─ Multiple files → Use Agent
                            • Multi-step workflow
                            • Consistent changes
                            • Coordinated updates
```

### Real-World Examples

**Scenario 1: Learning New Technology**
```
Task: "I want to learn how GraphQL works"

Use: Chat ✓
Why: Exploratory, no file changes needed
Approach: Ask questions, request examples, discuss trade-offs
```

**Scenario 2: Add Feature Across App**
```
Task: "Add user role-based permissions across 8 endpoints"

Use: Agent ✓
Why: Multiple files, coordinated changes, clear goal
Approach: Define goal, specify files, let agent execute
```

**Scenario 3: Debug Single Error**
```
Task: "Why is this function returning undefined?"

Use: Chat ✓
Why: Single issue, need explanation first
Approach: Share code, get explanation, manual fix
```

**Scenario 4: Refactor Architecture**
```
Task: "Split monolithic API into microservices pattern"

Use: Chat first, then Agent ✓
Why: Need planning (chat), then execution (agent)
Approach:
1. Chat: Discuss architecture
2. Chat: Plan file structure
3. Agent: Create new services
4. Agent: Update imports and references
5. Chat: Review and adjust
```

**Scenario 5: Quick Fix**
```
Task: "Change button color from blue to green"

Use: Chat ✓ (or manual)
Why: Single file, single line
Approach: Just do it manually, or ask chat
```

---

## How Agents Work

### The Agent Lifecycle

**1. Goal Understanding**
```
You: "Add TypeScript interfaces for all API responses"

Agent thinks:
- What files need changes? (API response handlers)
- What's the pattern? (Add interfaces)
- What should I preserve? (Existing logic)
- What's the scope? (All API files)
```

**2. Planning Phase**
```
Agent creates plan:
1. Read existing API response code
2. Identify response structures
3. Create interfaces in types file
4. Update each endpoint to use types
5. Ensure consistency across all
```

**3. Context Gathering**
```
Agent reads:
- API route files (src/routes/*.ts)
- Existing types (src/types/api.ts)
- Response structures
- Related test files
```

**4. Execution**
```
Agent executes:
1. Creates interfaces in types/api.ts
2. Updates routes/users.ts imports and types
3. Updates routes/products.ts imports and types
4. Updates routes/orders.ts imports and types
5. Ensures consistency
```

**5. Verification** (in advanced agents)
```
Agent checks:
- TypeScript compiles without errors
- All responses properly typed
- No breaking changes introduced
```

### How Agents Plan

**Example: "Add error handling to all services"**

**Agent's Internal Process:**

```
1. Parse Goal
   → "error handling" = try/catch blocks
   → "all services" = files in services/ folder
   
2. Analyze Codebase
   → Read services/ directory
   → Identify functions without try/catch
   → Note existing error handling patterns
   
3. Create Execution Plan
   Step 1: Read userService.ts
   Step 2: Wrap functions in try/catch
   Step 3: Add consistent error logging
   Step 4: Repeat for productService.ts
   Step 5: Repeat for orderService.ts
   
4. Execute Sequentially
   [Modifies each file]
   
5. Report Results
   "Added error handling to 3 services, 12 functions"
```

### Decision Making

**Agents make decisions like:**

**Decision 1: File Selection**
```
Goal: "Update all React components to use hooks"

Agent decides:
✓ Include: components/*.tsx
✓ Include: pages/*.tsx
✗ Exclude: utils/*.ts (not components)
✗ Exclude: components/*.test.tsx (tests, risky)
```

**Decision 2: Pattern Application**
```
Goal: "Add logging to all API calls"

Agent sees:
- Some files use console.log
- Some files use custom logger
- Some have no logging

Agent chooses: Use custom logger (most professional)
```

**Decision 3: Scope Boundaries**
```
Goal: "Refactor authentication"

Agent determines:
✓ auth/login.ts (directly related)
✓ auth/middleware.ts (directly related)
✓ models/User.ts (auth-related fields)
? routes/users.ts (uses auth, but not auth itself)
✗ services/email.ts (unrelated)
```

### Context Window

**What Agents "See":**

```
┌─────────────────────────────────────┐
│         Agent's Context             │
├─────────────────────────────────────┤
│ 1. Your Goal/Instructions           │
│ 2. Project Structure                │
│ 3. Files in Scope                   │
│ 4. Related Files (imports/exports)  │
│ 5. Existing Patterns                │
│ 6. Recent Changes (git)             │
└─────────────────────────────────────┘
```

**Context Limits:**
- Cannot read entire codebase at once
- Prioritizes files you mention
- Reads related files automatically
- May miss distant dependencies

**How to Help:**
```
Bad: "Fix the authentication"
     (Too vague, agent guesses context)

Good: "Refactor authentication in src/auth/ to use JWT.
       Update these files:
       - auth/login.ts
       - auth/middleware.ts
       - models/User.ts
       
       Use our existing jwt-helper from utils/jwt.ts"
       (Clear scope, specific files, existing patterns)
```

---

## Agent Modes in VS Code

### 1. GitHub Copilot Edits (Primary Agent Mode)

**What It Is:**
- Built-in VS Code agent mode
- Multi-file editing capability
- Integrated with Copilot Chat
- Context-aware across workspace

**How to Access:**
```
Method 1: Command Palette
- Cmd/Ctrl + Shift + P
- Type "Copilot Edits"
- Or use shortcut: Cmd+I (Mac) / Ctrl+I (Windows)

Method 2: Copilot Chat Panel
- Click "Edits" tab in Copilot panel
- Type your goal

Method 3: Right-click in Editor
- Right-click in file
- Select "Copilot" → "Start Editing Session"
```

**Features:**
- ✅ Multi-file editing
- ✅ Shows diff before applying
- ✅ Can add/remove files from scope
- ✅ Workspace context awareness
- ✅ Undo/redo support
- ✅ Real-time progress updates

**Example Session:**

```
1. Open Copilot Edits (Cmd+I)

2. Enter goal:
   "Add input validation to all API endpoints.
    Use validator.js library.
    Add these validations:
    - email format
    - password strength (min 8 chars)
    - required fields"

3. Specify files (or let agent choose):
   - routes/auth.ts
   - routes/users.ts
   - routes/products.ts

4. Review changes in diff view

5. Accept, reject, or modify
```

**Best Practices:**
```
✓ Clear, specific goals
✓ Mention files if you know them
✓ Reference existing patterns
✓ Monitor progress
✓ Review each file's changes
```

### 2. Copilot Chat (Hybrid Mode)

**Can Act as Mini-Agent:**
```
In Copilot Chat:
"/fix all TypeScript errors in this file"

Agent-like behavior:
- Reads file
- Identifies errors
- Suggests fixes
- Can apply directly
```

**Slash Commands:**
```
/explain [selection] - Explain code
/fix [issue] - Fix problems
/tests - Generate tests
/doc - Generate documentation
/new - Create new file/project
```

**When to Use:**
- Smaller scope (1-2 files)
- Quick fixes
- Need explanation alongside changes
- Testing before full agent task

### 3. VS Code Native Refactoring (AI-Enhanced)

**Built-in VS Code + AI:**
```
1. Select code
2. Right-click → "Refactor"
3. AI suggests context-aware refactorings
   - Extract function
   - Rename symbol
   - Move to file
   - Convert to arrow function
```

**Limitations:**
- Single file focus
- Predefined refactoring types
- Less flexible than agents

### Agent Mode Comparison

| Feature | Copilot Edits | Copilot Chat | VS Code Refactor |
|---------|---------------|--------------|------------------|
| Multi-file | ✅ Yes | ⚠️ Limited | ❌ No |
| Autonomous | ✅ Yes | ⚠️ Partial | ❌ No |
| Custom Goals | ✅ Yes | ✅ Yes | ❌ Predefined |
| Diff Preview | ✅ Yes | ⚠️ Sometimes | ✅ Yes |
| Undo/Redo | ✅ Yes | ⚠️ Manual | ✅ Yes |
| Learning | ⚠️ Less | ✅ More | ❌ None |

---

## Providing Clear Goals to Agents

### Anatomy of a Good Agent Goal

**Template:**
```
[ACTION] [SCOPE] [REQUIREMENTS]

Where:
- ACTION: What to do (refactor, add, update, create)
- SCOPE: Where to do it (files, folders, patterns)
- REQUIREMENTS: How to do it (constraints, patterns, details)
```

### Examples: Bad vs. Good

**Example 1: Authentication**

**❌ Bad:**
```
"Improve the authentication"
```
**Why Bad:**
- Vague action ("improve")
- No scope (which files?)
- No requirements (improve how?)

**✅ Good:**
```
"Refactor authentication to use JWT tokens.

Files:
- src/auth/login.ts
- src/auth/middleware.ts
- src/models/User.ts

Requirements:
- Use jsonwebtoken library
- Token expiration: 24 hours
- Store in httpOnly cookies
- Add refresh token mechanism
- Maintain existing error handling patterns
- Keep backward compatibility with session-based tests"
```
**Why Good:**
- Clear action (refactor to JWT)
- Specific scope (3 files listed)
- Detailed requirements (library, expiration, storage, etc.)

---

**Example 2: Testing**

**❌ Bad:**
```
"Add tests"
```

**✅ Good:**
```
"Generate unit tests for user service.

File to test: src/services/userService.ts
New file: src/services/__tests__/userService.test.ts

Requirements:
- Use Jest framework
- Test all exported functions
- Mock database calls (use jest.mock)
- Include edge cases:
  - Empty parameters
  - Null/undefined handling
  - Database errors
- Aim for 90%+ coverage
- Follow existing test patterns in __tests__/ folder"
```

---

**Example 3: Refactoring**

**❌ Bad:**
```
"Make the code better"
```

**✅ Good:**
```
"Refactor promise chains to async/await in API services.

Scope: src/services/*.ts (all service files)

Requirements:
- Replace .then()/.catch() with async/await
- Add try/catch blocks for error handling
- Use existing error types from utils/errors.ts
- Preserve all error messages
- Don't change function signatures
- Maintain existing return types
- Keep all existing comments"
```

---

### Goal Clarity Checklist

Before giving goal to agent, verify:

- [ ] **Action is clear**: "Add", "Refactor", "Update", "Create", etc.
- [ ] **Scope is specific**: Which files/folders?
- [ ] **Requirements are detailed**: How should it be done?
- [ ] **Constraints mentioned**: What to preserve, what to avoid
- [ ] **Patterns referenced**: Point to existing code examples
- [ ] **Success criteria clear**: What does "done" look like?

### Providing Context

**Good Context Patterns:**

**1. Reference Existing Code**
```
"Add error handling like we do in userService.ts:

try {
  // operation
} catch (error) {
  logger.error('Context', error);
  throw new APIError(error.message);
}

Apply this pattern to all services in src/services/"
```

**2. Specify File Structure**
```
"Create a new feature: product reviews

Structure:
- models/ProductReview.ts (data model)
- services/reviewService.ts (business logic)
- routes/reviews.ts (API endpoints)
- __tests__/reviewService.test.ts (tests)

Follow the pattern from existing features like user management."
```

**3. List Constraints**
```
"Migrate to new database library, BUT:
- Don't change the API response formats
- Keep existing error handling
- Maintain backward compatibility
- Don't modify test files yet
- Use the new library only in services/ folder"
```

**4. Provide Examples**
```
"Add TypeScript interfaces for API responses.

Example format:
interface UserResponse {
  id: string;
  email: string;
  createdAt: Date;
}

Create similar interfaces for:
- Product responses
- Order responses
- Review responses"
```

### Scope Definition

**How to Define Scope:**

**1. By Files:**
```
"Update these files:
- src/auth/login.ts
- src/auth/register.ts
- src/auth/middleware.ts"
```

**2. By Folder:**
```
"Apply to all files in src/services/"
```

**3. By Pattern:**
```
"Update all *.controller.ts files"
```

**4. By Feature:**
```
"Modify all authentication-related code"
(Less precise, agent must interpret)
```

**Best Practice: Mix approaches**
```
"Refactor authentication feature.

Specific files:
- src/auth/*.ts (all files in auth folder)
- src/middleware/authenticate.ts
- src/models/User.ts

Do NOT modify:
- tests/ (we'll update separately)
- config/ (leave as-is)"
```

---

## Monitoring and Guiding Agents

### Real-Time Monitoring

**What to Watch:**

**1. File List**
```
Agent starts working:
✓ auth/login.ts ✓ ← Good
✓ auth/middleware.ts ✓ ← Good
⚠️ services/email.ts ← Wait, why this file?

ACTION: Interrupt and clarify scope
```

**2. Change Preview**
```
Agent modifies code:
✓ Added JWT token generation ← Good
✓ Updated middleware ← Good
⚠️ Removed all error handling ← STOP!

ACTION: Stop agent, restore error handling
```

**3. Progress Updates**
```
Agent reports:
"Updating 1 of 5 files..." ← On track
"Updating 12 of 5 files..." ← Scope creep!

ACTION: Check what extra files are being modified
```

### When to Interrupt

**Interrupt If:**

**1. Wrong Files Being Modified**
```
Goal: "Update authentication"
Agent modifies: database schema files

YOU: "Stop - don't modify database schema.
      Only update auth logic in src/auth/ folder."
```

**2. Wrong Approach**
```
Goal: "Add error handling"
Agent: Uses console.error everywhere

YOU: "Stop - use our custom logger from utils/logger.ts instead"
```

**3. Breaking Existing Code**
```
Goal: "Refactor to TypeScript"
Agent: Removes all tests

YOU: "Stop - preserve all test files, we'll convert them separately"
```

**4. Scope Creep**
```
Goal: "Update user service"
Agent: Also updating product and order services

YOU: "Stop - only update user service for now"
```

### How to Interrupt

**In Copilot Edits:**
```
Method 1: Click "Stop" button in panel

Method 2: Add clarifying message
"Stop - I see you're modifying X. 
 Please only modify Y instead."

Method 3: Close Copilot Edits session
(Discards changes in progress)
```

### Providing Mid-Course Corrections

**Pattern 1: Redirect**
```
Agent starts modifying files...

YOU: "I see you're using 'any' types.
      Instead, create proper interfaces in types/api.ts
      and use those interfaces."

Agent: Adjusts approach mid-task
```

**Pattern 2: Add Requirements**
```
Agent partially complete...

YOU: "Good progress! Also add:
      - Input validation with Joi
      - Rate limiting with express-rate-limit
      Continue with remaining files."

Agent: Incorporates new requirements
```

**Pattern 3: Clarify Scope**
```
Agent asks or seems uncertain...

YOU: "To clarify: only update functions that make API calls.
      Leave utility functions unchanged."

Agent: Proceeds with clearer understanding
```

### Reviewing Agent Output

**Step-by-Step Review:**

**1. Overview Check**
```
Questions:
- How many files modified? (Expected?)
- Any unexpected files? (Why?)
- Any files NOT modified that should be? (Missing?)
```

**2. File-by-File Review**
```
For each file:
- Read the diff (what changed?)
- Check if changes match your goal
- Look for unintended changes
- Verify existing code preserved
```

**3. Pattern Consistency**
```
Check:
- Same pattern applied throughout?
- Consistent naming conventions?
- Error handling uniform?
- Comments/docs updated?
```

**4. Breaking Changes**
```
Look for:
- Changed function signatures
- Removed public APIs
- Modified data structures
- Changed behavior
```

**5. Test Impact**
```
Consider:
- Will tests still pass?
- Do tests need updates?
- Are edge cases handled?
```

### Accepting/Rejecting Changes

**Accept When:**
- ✅ Changes match your goal
- ✅ Code quality is good
- ✅ No unintended modifications
- ✅ Patterns are consistent
- ✅ Tests will likely pass

**Reject When:**
- ❌ Wrong files modified
- ❌ Breaking changes introduced
- ❌ Code quality poor
- ❌ Doesn't match requirements
- ❌ Too risky to merge

**Partial Accept:**
```
"Accept changes to auth/ folder,
 but reject changes to config/ folder.
 
 Let's redo the config changes with different approach."
```

### Iterative Refinement

**Strategy: Small Steps**

```
Bad: "Refactor entire application to TypeScript"
     (Too big, hard to review)

Good:
Step 1: "Convert src/utils/ to TypeScript"
        [Review, accept]
        
Step 2: "Convert src/services/ to TypeScript,
         using types from step 1"
        [Review, accept]
        
Step 3: "Convert src/routes/ to TypeScript"
        [Review, accept]
```

**Benefits:**
- Easier to review each step
- Can catch issues early
- Can adjust approach between steps
- Easier to rollback if needed

---

## Agent Best Practices

### 1. Start Small

**First Agent Task:**
```
❌ "Migrate entire app to microservices"
   (Too complex for first time)

✅ "Add JSDoc comments to src/utils/validators.ts"
   (Small, clear, easy to review)
```

**Progressive Complexity:**
```
Task 1: Single file, simple change
Task 2: Multiple files, related changes
Task 3: Feature spanning several files
Task 4: Complex refactoring
```

### 2. Be Specific

**Specificity Levels:**

**Level 1: Vague (Avoid)**
```
"Make the code better"
"Fix everything"
"Improve performance"
```

**Level 2: General (OK for exploration)**
```
"Refactor authentication"
"Add error handling"
"Create tests"
```

**Level 3: Specific (Good)**
```
"Refactor authentication to use JWT tokens in src/auth/"
"Add try/catch error handling to all async functions in services/"
"Create unit tests for userService.ts with Jest"
```

**Level 4: Detailed (Best)**
```
"Refactor authentication in src/auth/ from sessions to JWT.
 Use jsonwebtoken library, 24h expiration.
 Update login.ts, middleware.ts, and User.ts.
 Preserve existing error handling.
 Add refresh token mechanism."
```

### 3. Provide Context

**Context Types:**

**Code Context:**
```
"Use the same error handling pattern from userService.ts:

try {
  // operation
} catch (error) {
  logger.error('Operation failed', error);
  throw new APIError(error.message, error.code);
}
```

**Project Context:**
```
"We're using:
- Node.js 18
- Express 4.18
- PostgreSQL 14
- TypeScript 5.0
- Jest for testing

Apply changes accordingly."
```

**Business Context:**
```
"This is a public API with 10k users.
Changes must be backward compatible.
No breaking changes to response formats."
```

### 4. Review Incrementally

**Don't Do This:**
```
1. Give agent huge task
2. Let it run on 50 files
3. Review at the end
4. Find major issues
5. Start over
```

**Do This:**
```
1. Give agent first part of task (5-10 files)
2. Monitor progress
3. Review changes
4. Adjust if needed
5. Continue to next part
```

**Example:**
```
Task: "Add TypeScript to entire project"

Break into:
Part 1: "Convert utils/ to TypeScript"
        [Review] ✓ Good

Part 2: "Convert services/ to TypeScript, using types from utils/"
        [Review] ⚠️ Missing type for User
        
YOU: "Add User type from models/User.ts"
     [Review] ✓ Good

Part 3: "Convert routes/ to TypeScript"
        [Review] ✓ Good
```

### 5. Reference Existing Patterns

**Without Pattern:**
```
"Add logging to all API endpoints"

Agent might use:
- console.log (inconsistent)
- Different log formats
- No error context
```

**With Pattern:**
```
"Add logging to all API endpoints using our logger.

Pattern (from userController.ts):
logger.info('User action', {
  action: 'operation',
  userId: user.id,
  timestamp: Date.now()
});

Apply this format to all endpoints."

Agent follows exact pattern consistently ✓
```

### 6. Set Boundaries

**Clear Boundaries:**
```
"Refactor auth system.

Modify:
✓ src/auth/*.ts
✓ src/middleware/authenticate.ts

Do NOT modify:
✗ tests/ (we'll update separately)
✗ config/ (production config, risky)
✗ database migrations (handle manually)"
```

### 7. Test Agent Output

**After Agent Completes:**

```
1. Review changes ✓

2. Run tests:
   npm test

3. Check TypeScript:
   npm run type-check

4. Lint code:
   npm run lint

5. Manual testing:
   - Start dev server
   - Test affected features
   - Check error scenarios

6. If all pass → Commit
   If issues → Fix or redo
```

### 8. Learn From Agent Behavior

**Keep Notes:**
```
Task: "Add pagination to API endpoints"

What worked:
✓ Agent understood pattern from one example
✓ Applied consistently to all endpoints
✓ Preserved existing query parameters

What didn't work:
✗ Agent used 0-based page indexing (we use 1-based)
✗ Didn't update type definitions

Next time:
→ Specify page index base explicitly
→ Mention type files in goal
```

### 9. Use Agents for Strengths

**Agent Strengths:**
```
✓ Repetitive tasks (apply pattern 50 times)
✓ Consistent changes (update all imports)
✓ Boilerplate generation (CRUD operations)
✓ Multi-file refactoring (rename across files)
✓ Pattern application (same error handling everywhere)
```

**Agent Weaknesses:**
```
✗ Complex business logic decisions
✗ Architecture decisions
✗ Performance optimization (needs profiling)
✗ Creative problem solving
✗ Understanding user requirements
```

**Example:**
```
Good Agent Task:
"Generate CRUD endpoints for Product model following User model pattern"
(Pattern-based, repetitive, clear)

Bad Agent Task:
"Design the optimal database schema for our e-commerce platform"
(Requires judgment, domain knowledge, trade-off decisions)
```

### 10. Combine Chat and Agent

**Effective Workflow:**

```
1. Chat: Explore approach
   "What's the best way to add Redis caching to our API?"
   [Discuss options, decide approach]

2. Chat: Plan implementation
   "Show me the pattern for one endpoint"
   [Review code sample, adjust]

3. Agent: Execute implementation
   "Apply this Redis caching pattern to all endpoints in routes/*.ts"
   [Agent implements]

4. Chat: Review and adjust
   "Review the caching implementation for issues"
   [Chat reviews, suggests fixes]

5. Agent: Apply fixes
   "Update cache key generation to include user ID"
   [Agent fixes]
```

---

## Agent Tools Comparison

### GitHub Copilot Edits

**Strengths:**
- ✅ Built into VS Code
- ✅ Deep workspace context
- ✅ Multi-file editing
- ✅ Good diff preview
- ✅ Familiar VS Code interface

**Limitations:**
- ⚠️ Requires Copilot subscription
- ⚠️ Internet connection needed
- ⚠️ Limited to VS Code

**Best For:**
- General development
- TypeScript/JavaScript projects
- Multi-file refactoring
- VS Code users

**Usage:**
```
1. Cmd/Ctrl + I
2. Describe goal
3. Review changes
4. Accept/reject
```

### Cursor Composer

**Strengths:**
- ✅ Very autonomous
- ✅ Can create entire features
- ✅ Good at planning
- ✅ Handles complex tasks

**Limitations:**
- ⚠️ Separate IDE (fork of VS Code)
- ⚠️ Paid (beyond free tier)
- ⚠️ Can be overly aggressive
- ⚠️ Requires migration from VS Code

**Best For:**
- Greenfield projects
- Rapid prototyping
- Large feature development
- Users wanting more autonomy

**Usage:**
```
1. Cmd/Ctrl + K
2. Describe feature
3. Composer plans and executes
4. Review in terminal or editor
```

### Aider (CLI Agent)

**Strengths:**
- ✅ Command-line interface
- ✅ Git integration
- ✅ Supports multiple AI models
- ✅ Can run in CI/CD
- ✅ Works with any editor

**Limitations:**
- ⚠️ Command-line only
- ⚠️ Learning curve
- ⚠️ No visual diff
- ⚠️ Less intuitive

**Best For:**
- Terminal-focused workflows
- Scripting/automation
- Remote development
- CI/CD integration
- Users comfortable with CLI

**Usage:**
```bash
# Install
pip install aider-chat

# Use
aider file1.ts file2.ts --message "Add error handling"

# Approve changes
[Review in git diff]
git commit -m "Agent: Added error handling"
```

### Continue Extension

**Strengths:**
- ✅ VS Code extension
- ✅ Open source
- ✅ Multiple AI models
- ✅ Customizable

**Limitations:**
- ⚠️ Less mature than Copilot
- ⚠️ Fewer features
- ⚠️ Requires manual setup

**Best For:**
- Open source preference
- Custom AI models
- Flexibility in model choice
- Budget-conscious users

### Tool Recommendation Matrix

| Your Situation | Recommended Tool |
|----------------|------------------|
| VS Code user, want simplest | **Copilot Edits** |
| Building new projects fast | **Cursor Composer** |
| Terminal workflow, automation | **Aider** |
| Want open source, flexibility | **Continue** |
| Team already on Copilot | **Copilot Edits** |
| Budget limited | **Continue** or **Aider** |
| Need maximum autonomy | **Cursor Composer** |

### Multi-Tool Strategy

**Use Different Tools for Different Tasks:**

```
Daily development: Copilot Edits
  - Feature implementation
  - Refactoring
  - Bug fixes

Prototyping: Cursor Composer
  - New features
  - Rapid exploration

Automation: Aider
  - CI/CD tasks
  - Batch processing
  - Scripted refactoring

Learning: Continue
  - Experimenting
  - Custom models
```

---

## Hands-On Exercises

### Exercise 1: First Agent Task (Beginner)

**Goal:** Get comfortable with agent basics

**Task:**
Use Copilot Edits to add JSDoc comments to a utility file.

**Steps:**
1. Create a simple utility file:
```typescript
// utils/math.ts
export function add(a: number, b: number) {
  return a + b;
}

export function multiply(a: number, b: number) {
  return a * b;
}

export function divide(a: number, b: number) {
  return a / b;
}
```

2. Open Copilot Edits (Cmd/Ctrl + I)

3. Enter goal:
```
"Add JSDoc comments to all functions in this file.

Include:
- Description
- @param for each parameter
- @returns
- @example

Format:
/**
 * Description
 * @param {type} name - description
 * @returns {type} description
 * @example
 * // usage example
 */
```

4. Review changes
5. Accept if good

**Success Criteria:**
- [ ] All functions have JSDoc
- [ ] Format is consistent
- [ ] Examples are helpful
- [ ] No code logic changed

---

### Exercise 2: Multi-File Refactoring (Intermediate)

**Goal:** Practice multi-file agent task

**Task:**
Refactor promise chains to async/await

**Setup:**
Create 3 service files with promise chains:
```typescript
// services/user.ts
export function getUser(id: string) {
  return fetch(`/api/users/${id}`)
    .then(res => res.json())
    .then(data => data.user)
    .catch(err => {
      console.error(err);
      throw err;
    });
}

// Similar for services/product.ts and services/order.ts
```

**Your Agent Goal:**
```
[Write comprehensive goal for agent to refactor all 3 files]
```

**Expected Outcome:**
```typescript
export async function getUser(id: string) {
  try {
    const res = await fetch(`/api/users/${id}`);
    const data = await res.json();
    return data.user;
  } catch (err) {
    console.error(err);
    throw err;
  }
}
```

**Success Criteria:**
- [ ] All files converted to async/await
- [ ] Error handling preserved
- [ ] Return values same
- [ ] Consistent pattern

---

### Exercise 3: Adding Feature Across Files (Intermediate)

**Goal:** Coordinate changes across multiple files

**Task:**
Add logging to all API calls

**Setup:**
Create API service files without logging

**Your Agent Goal:**
```
[Write goal to add consistent logging to all API services]
```

**What to Specify:**
- Which files
- Logging pattern to use
- What to log (request, response, errors)
- Where to import logger from

**Success Criteria:**
- [ ] All API calls have logging
- [ ] Consistent format
- [ ] Includes error logging
- [ ] No duplicate logs

---

### Exercise 4: Monitoring and Interrupting (Advanced)

**Goal:** Practice guiding agent mid-task

**Task:**
Give agent intentionally vague goal, then guide it

**Setup:**
```
Goal: "Add error handling to services"
(Intentionally vague - no pattern specified)
```

**Expected:**
Agent will make assumptions about error handling

**Your Actions:**
1. Watch what pattern agent uses
2. If not ideal, interrupt
3. Specify your preferred pattern
4. Let agent continue

**Practice:**
- [ ] Recognizing wrong approach
- [ ] Interrupting at right time
- [ ] Providing clear correction
- [ ] Reviewing final result

---

### Exercise 5: Chat vs. Agent Decision (Beginner)

**Scenarios:**
For each scenario, decide: Chat or Agent?

1. "Explain how React useEffect works"
   - [ ] Chat
   - [ ] Agent
   - Why: _______________

2. "Convert all class components to functional components"
   - [ ] Chat
   - [ ] Agent
   - Why: _______________

3. "Why is this function returning undefined?"
   - [ ] Chat
   - [ ] Agent
   - Why: _______________

4. "Add TypeScript types to all API responses"
   - [ ] Chat
   - [ ] Agent
   - Why: _______________

5. "What's the best database for this use case?"
   - [ ] Chat
   - [ ] Agent
   - Why: _______________

**Answers:**
1. Chat (learning)
2. Agent (multi-file refactoring)
3. Chat (debugging, need explanation)
4. Agent (repetitive pattern application)
5. Chat (needs discussion/advice)

---

### Exercise 6: Progressive Agent Tasks (Advanced)

**Goal:** Break large task into smaller agent tasks

**Large Task:**
"Migrate application from JavaScript to TypeScript"

**Your Plan:**
Break into 5-7 smaller agent tasks:

```
Task 1: _______________
Scope: _______________
Success: _______________

Task 2: _______________
Scope: _______________
Success: _______________

[Continue...]
```

**Example Solution:**
```
Task 1: Add TypeScript config
Scope: Create tsconfig.json
Success: Valid config, compiles

Task 2: Convert utilities
Scope: src/utils/*.js → *.ts
Success: All utils typed

Task 3: Convert models
Scope: src/models/*.js → *.ts
Success: Data types defined

Task 4: Convert services
Scope: src/services/*.js → *.ts
Success: Service types, use model types

Task 5: Convert routes
Scope: src/routes/*.js → *.ts
Success: Route handlers typed

Task 6: Update tests
Scope: **/*.test.js → *.test.ts
Success: Tests pass with types
```

**Success Criteria:**
- [ ] Logical progression
- [ ] Each step builds on previous
- [ ] Clear success criteria
- [ ] Manageable scope per task

---

## Progress Checklist

### Understanding Agents
- [ ] Understand difference between chat and agents
- [ ] Know when to use each approach
- [ ] Recognize agent vs. chat use cases
- [ ] Understand agent planning process

### Using Agents
- [ ] Opened Copilot Edits mode
- [ ] Provided clear goal to agent
- [ ] Reviewed agent's file changes
- [ ] Accepted/rejected agent changes
- [ ] Completed first successful agent task

### Guiding Agents
- [ ] Monitored agent progress
- [ ] Interrupted agent when needed
- [ ] Provided mid-course correction
- [ ] Reviewed incrementally
- [ ] Refined agent goal based on results

### Best Practices
- [ ] Start with small agent tasks
- [ ] Provide specific goals
- [ ] Reference existing patterns
- [ ] Set clear boundaries
- [ ] Test agent output

---

## Key Takeaways

### The Core Concept

```
Chat = Interactive Consultant
- Ask → Answer → Repeat
- You copy results manually
- Great for learning and exploration

Agent = Autonomous Assistant
- Goal → Execute → Review
- Direct file modifications
- Great for implementation
```

### Decision Framework

```
Need file changes?
├─ No → Chat
└─ Yes → How many files?
          ├─ 1-2 simple → Chat (copy/paste)
          └─ Multiple coordinated → Agent
```

### Success Factors

**Clear Goals:**
- ✅ Specific action
- ✅ Defined scope
- ✅ Detailed requirements
- ✅ Referenced patterns

**Active Monitoring:**
- ✅ Watch file list
- ✅ Review changes
- ✅ Interrupt when wrong
- ✅ Guide mid-course

**Incremental Progress:**
- ✅ Start small
- ✅ Review each step
- ✅ Build complexity
- ✅ Learn from results

### Time Savings

Well-executed agent tasks save **70-85%** time on:
- Multi-file refactoring
- Feature implementation
- Pattern application
- Repetitive changes

**Example:**
```
Traditional: 2 hours
With Agent: 20 minutes
Savings: 85%
```

### Common Pitfalls

**Avoid:**
- ❌ Vague goals ("make it better")
- ❌ No file scope (agent guesses)
- ❌ No monitoring (catch issues late)
- ❌ Reviewing only at end (wasted work)
- ❌ Using agents for exploration (use chat)

### Next Steps

**Immediate:**
1. Try first agent task (Exercise 1)
2. Practice with small refactoring
3. Learn to interrupt/guide

**Soon:**
4. Multi-file agent tasks
5. Feature implementation with agents
6. Combine chat + agent workflows

**Eventually:**
7. Complex agent workflows
8. CI/CD agent integration
9. Custom agent templates

---

**Remember:**

> **Agents are tools, not magic.**
> 
> They execute what you describe.  
> Clear goals = Great results.  
> Vague goals = Frustration.
> 
> Master the goal, master the agent.

---

**Next**: Move to Phase 4 (Advanced Techniques) or explore [Context Management](../Phase_4_Advanced_Techniques/phase4-context-management.md)

