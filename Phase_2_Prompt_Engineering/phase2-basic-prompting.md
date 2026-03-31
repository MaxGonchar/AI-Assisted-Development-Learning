# Phase 2: Basic Prompting

**Topic**: Fundamental techniques for effective AI communication  
**Duration**: 2-3 days  
**Prerequisites**: Completed Phase 1 (Understanding AI basics and VS Code setup)

---

## Overview

Basic prompting is the foundation of effective AI-assisted coding. This guide covers the core principles that will make your interactions with AI coding assistants productive and efficient. Think of prompting as giving clear instructions to a highly capable but literal-minded junior developer.

**What You'll Learn:**
- How to write clear, specific requests
- Why context matters and how to provide it
- Breaking down complex tasks into manageable steps
- Using examples to guide AI behavior

---

## 1. Be Specific and Clear in Requests

### The Problem with Vague Prompts

AI assistants work best with precise instructions. Vague requests lead to generic or incorrect solutions.

### ❌ Bad Examples:
```
"Fix this function"
"Make it better"
"Add validation"
"Create a component"
```

**Why these fail:**
- Too ambiguous - AI doesn't know what's "wrong" or what "better" means
- No context about requirements or constraints
- Forces AI to guess your intentions

### ✅ Good Examples:

**Instead of:** "Fix this function"  
**Say:**
```
"This function throws a TypeError when the input is null. 
Update it to return an empty array when input is null or undefined."
```

**Instead of:** "Make it better"  
**Say:**
```
"Refactor this function to improve performance by:
1. Reducing nested loops from O(n²) to O(n)
2. Using a Map instead of repeated array lookups
3. Adding early returns for edge cases"
```

**Instead of:** "Add validation"  
**Say:**
```
"Add input validation to this function that:
- Checks email format using regex
- Ensures password is at least 8 characters
- Validates age is between 18 and 120
- Returns descriptive error messages for each validation failure"
```

**Instead of:** "Create a component"  
**Say:**
```
"Create a React functional component called UserCard that:
- Accepts props: name (string), email (string), avatar (string)
- Displays the avatar image (50x50px, rounded)
- Shows name in bold and email in gray below it
- Uses TypeScript with proper prop types
- Follows our existing component style in src/components/ProfileCard.tsx"
```

### Practice Exercise 1: Specificity

**Your Task:** Rewrite these vague prompts to be specific and clear.

1. ❌ "Create a function for users"
   - ✅ Your improved version: ___________

2. ❌ "Debug this code"
   - ✅ Your improved version: ___________

3. ❌ "Write tests"
   - ✅ Your improved version: ___________

<details>
<summary>Click to see example answers</summary>

1. ✅ "Create a TypeScript function called `validateUser` that accepts a user object with email, password, and age properties. Return true if all fields are valid, false otherwise. Email must match standard format, password must be 8+ characters, age must be 18+."

2. ✅ "This code crashes with 'Cannot read property length of undefined' on line 24 when the API returns an empty response. Add a check to handle undefined or null values before accessing the length property."

3. ✅ "Write Jest unit tests for the `calculateTotal` function in src/utils/cart.js. Test cases should cover: empty cart, single item, multiple items, discount application, and handling invalid inputs."

</details>

---

## 2. Provide Context

Context is critical information that helps AI understand your situation, constraints, and requirements.

### Types of Context to Provide

#### A. File Purpose and Role
Tell AI what the file/function is supposed to do.

```
"I'm working on an authentication middleware for an Express API.
This middleware should verify JWT tokens and attach user data to the request object."
```

#### B. Dependencies and Tech Stack
Specify what libraries, frameworks, and versions you're using.

```
"Using React 18.2, TypeScript 5.0, and React Router v6.
Need to create a protected route component."
```

#### C. Constraints and Requirements
Mention limitations, standards, or requirements.

```
"This function will run in a serverless environment (AWS Lambda).
- Must complete in under 3 seconds
- Cannot use file system operations
- Should use async/await, not callbacks
- Must handle rate limiting from the external API"
```

#### D. Existing Code Patterns
Reference how similar tasks are handled in your codebase.

```
"Our project uses a custom error handling pattern.
See src/utils/errors.ts for our AppError class.
Please follow the same pattern for this new feature."
```

### Real-World Examples

#### Example 1: Without Context ❌
```
"Create a function to fetch user data"
```

**Result:** AI might create something generic that doesn't fit your needs.

#### Example 1: With Context ✅
```
"Create a function to fetch user data with this context:

File purpose: User service layer (src/services/userService.ts)
Tech stack: Node.js with TypeScript, using axios for HTTP requests
Dependencies: We have a custom API client at src/api/client.ts

Requirements:
- Function should be async and return a Promise<User>
- Handle network errors by throwing our custom NetworkError (see src/errors/)
- Cache results for 5 minutes using our cacheService
- Accept a userId parameter (string)
- API endpoint format: /api/v2/users/:userId

Return type should match our User interface in src/types/user.ts"
```

**Result:** AI generates code that fits perfectly into your existing architecture.

#### Example 2: Debugging with Context

**Without Context ❌:**
```
"Why isn't this working?

function processItems(items) {
  return items.map(item => item.price * 1.1);
}
```

**With Context ✅:**
```
"I'm getting a 'Cannot read property price of undefined' error on this function.

Context:
- This runs in a shopping cart component (React)
- The items array comes from an API response
- Sometimes the API returns items without a price property (out of stock items)
- I need to handle missing prices by using 0 as default
- Function should also round to 2 decimal places

Current code:
function processItems(items) {
  return items.map(item => item.price * 1.1);
}
```

### Practice Exercise 2: Adding Context

**Your Task:** Add relevant context to these requests.

1. Basic: "Create a login function"
   - Add context about: tech stack, where it fits in your app, requirements

2. Your context-rich version:
   ___________

<details>
<summary>Click to see an example answer</summary>

✅ "Create a login function with this context:

File: src/services/auth.ts (TypeScript)
Tech stack: React app with Firebase Authentication
Purpose: Handle user login via email/password

Requirements:
- Function name: loginUser
- Parameters: email (string), password (string)
- Return: Promise<UserCredential>
- Handle common errors: invalid credentials, network issues, email not verified
- Throw custom AuthError with user-friendly messages (see src/errors/AuthError.ts)
- Set user session in localStorage after successful login
- Follow async/await pattern used throughout the project"

</details>

---

## 3. Break Complex Tasks into Smaller Steps

AI handles focused, single-purpose tasks better than large, multi-part requests.

### Why Breaking Down Matters

- **Clarity**: Easier to understand and execute
- **Quality**: More accurate results for each step
- **Debugging**: Simpler to identify and fix issues
- **Learning**: You understand each piece better

### Strategy: The Step-by-Step Approach

Instead of asking for everything at once, request components sequentially.

#### ❌ One Big Request (Overwhelming):
```
"Create a complete user authentication system with login, signup, 
password reset, email verification, JWT tokens, refresh tokens, 
rate limiting, and admin dashboard"
```

**Problems:**
- Too much to handle at once
- AI might skip important details
- Harder to verify correctness
- Difficult to customize

#### ✅ Broken Into Steps (Manageable):

**Step 1:**
```
"Create a User model for authentication with these fields:
- id (UUID)
- email (string, unique, validated)
- passwordHash (string)
- createdAt (timestamp)
- lastLogin (timestamp, nullable)

Use TypeScript and include appropriate types."
```

**Step 2 (after reviewing Step 1):**
```
"Now create a signup function that:
- Accepts email and password
- Validates email format
- Checks password strength (8+ chars, 1 number, 1 special char)
- Hashes password using bcrypt
- Creates user in database
- Returns the created user (without password)
- Handles duplicate email errors"
```

**Step 3 (after reviewing Step 2):**
```
"Create a login function that:
- Accepts email and password
- Looks up user by email
- Compares password with stored hash using bcrypt
- Generates JWT token on success
- Updates lastLogin timestamp
- Returns user object and token
- Handles invalid credentials"
```

And so on... Each step builds on the previous one.

### When to Break Down vs Keep Together

**Break down when:**
- Task involves multiple distinct components
- You need to customize/review each part
- Learning or exploring new patterns
- Task complexity is high

**Keep together when:**
- Components are tightly coupled and simple
- Following a well-established pattern
- Task is straightforward and standard

### Practice Exercise 3: Breaking Down Tasks

**Your Task:** Break this large request into 4-5 manageable steps:

```
"Create a blog post system with CRUD operations, markdown support, 
tags, categories, featured images, SEO metadata, comments, 
and user authentication"
```

**Your steps:**
1. ___________
2. ___________
3. ___________
4. ___________
5. ___________

<details>
<summary>Click to see example breakdown</summary>

**Step 1: Data Model**
```
"Create TypeScript interfaces for a blog post system:
- BlogPost (id, title, content, authorId, createdAt, updatedAt, published)
- Tag (id, name)
- Category (id, name, description)
Include proper types and relationships."
```

**Step 2: Basic CRUD**
```
"Create functions for basic blog post operations:
- createPost(post: BlogPost): Promise<BlogPost>
- getPostById(id: string): Promise<BlogPost>
- updatePost(id: string, updates: Partial<BlogPost>): Promise<BlogPost>
- deletePost(id: string): Promise<void>
Use async/await and include error handling."
```

**Step 3: Markdown Processing**
```
"Add markdown support to blog posts:
- Install and configure 'marked' library
- Create a function to convert markdown content to HTML
- Sanitize HTML output for security
- Add syntax highlighting for code blocks using 'highlight.js'"
```

**Step 4: Tags and Categories**
```
"Implement tags and categories:
- addTagToPost(postId: string, tagId: string): Promise<void>
- removeTagFromPost(postId: string, tagId: string): Promise<void>
- getPostsByTag(tagId: string): Promise<BlogPost[]>
- setPostCategory(postId: string, categoryId: string): Promise<void>
- getPostsByCategory(categoryId: string): Promise<BlogPost[]>"
```

**Step 5: Featured Images & SEO**
```
"Add featured image and SEO metadata to BlogPost:
- featuredImage (url, alt text, dimensions)
- SEO meta (title, description, keywords, og:image)
Create a function to validate image URLs and generate SEO-friendly slugs from titles."
```

</details>

---

## 4. Use Examples When Explaining Desired Behavior

Examples are incredibly powerful for communicating your expectations to AI.

### Why Examples Work

- **Concrete**: Shows exactly what you want, not just describing it
- **Style Transfer**: AI learns your coding patterns and preferences
- **Edge Cases**: Examples can demonstrate how to handle special situations
- **Less Ambiguity**: Reduces misinterpretation

### Types of Examples

#### A. Input/Output Examples

Show what data goes in and what should come out.

```
"Create a function to format phone numbers.

Examples:
Input: "1234567890" → Output: "(123) 456-7890"
Input: "123-456-7890" → Output: "(123) 456-7890"
Input: "+11234567890" → Output: "+1 (123) 456-7890"
Input: "12345" → Output: Error - Invalid phone number
```

#### B. Code Style Examples

Show AI how you write code in your project.

```
"Create a new API route handler following this pattern from our existing code:

Existing handler example:
export const getUser = async (req: Request, res: Response) => {
  try {
    const { id } = req.params;
    const user = await userService.getById(id);
    
    if (!user) {
      return res.status(404).json({ error: 'User not found' });
    }
    
    res.json({ success: true, data: user });
  } catch (error) {
    logger.error('Error fetching user:', error);
    res.status(500).json({ error: 'Internal server error' });
  }
};

Now create a similar handler for getting a product by ID."
```

#### C. Before/After Examples

Show what you have and what you want it to become.

```
"Refactor this code to use async/await:

Current (promises):
function fetchUserData(userId) {
  return api.getUser(userId)
    .then(user => {
      return api.getUserPosts(user.id);
    })
    .then(posts => {
      return { user, posts };
    })
    .catch(error => {
      console.error('Error:', error);
      throw error;
    });
}

Convert to async/await with proper error handling."
```

#### D. Edge Case Examples

Show how to handle unusual inputs.

```
"Create a function to calculate average with these edge cases:

calculateAverage([1, 2, 3]) → 2
calculateAverage([]) → 0 (not NaN)
calculateAverage([5]) → 5
calculateAverage([1.5, 2.5, 3.5]) → 2.5 (not rounded)
calculateAverage(null) → throw Error('Invalid input')
calculateAverage([1, 'two', 3]) → throw Error('Non-numeric value')
```

### Real-World Scenario

**Without Example ❌:**
```
"Create a function to validate user input"
```

**With Examples ✅:**
```
"Create a function to validate user registration input.

Expected behavior examples:

Valid inputs:
✓ { email: "user@example.com", password: "SecureP@ss1", age: 25 }
✓ { email: "test+alias@domain.co.uk", password: "Str0ng!Pass", age: 18 }

Invalid inputs:
✗ { email: "notanemail", password: "SecureP@ss1", age: 25 }
  → Error: "Invalid email format"

✗ { email: "user@example.com", password: "weak", age: 25 }
  → Error: "Password must be at least 8 characters with 1 number and 1 special character"

✗ { email: "user@example.com", password: "SecureP@ss1", age: 17 }
  → Error: "Must be 18 or older"

✗ { email: "user@example.com", password: "SecureP@ss1" }
  → Error: "Age is required"

Function should return { valid: true } or { valid: false, errors: string[] }"
```

### Practice Exercise 4: Using Examples

**Your Task:** Add 3-4 examples to clarify this request:

```
"Create a function to parse date strings into Date objects"
```

**Your examples:**
1. ___________
2. ___________
3. ___________
4. ___________

<details>
<summary>Click to see example answer</summary>

✅ "Create a function to parse date strings into Date objects.

Handle these formats:

Examples:
parseDate("2024-03-15") → Date object for March 15, 2024
parseDate("03/15/2024") → Date object for March 15, 2024
parseDate("15-Mar-2024") → Date object for March 15, 2024
parseDate("March 15, 2024") → Date object for March 15, 2024
parseDate("invalid") → throw Error('Invalid date format')
parseDate("") → throw Error('Date string cannot be empty')
parseDate(null) → throw Error('Date string is required')

The function should handle US date formats and return JavaScript Date objects."

</details>

---

## Combining All Four Principles

The most effective prompts combine specificity, context, breakdown, and examples.

### Complete Example: Building a Feature

**Scenario:** You need to add a "favorite posts" feature to a blog app.

#### ❌ Basic Request (Missing all principles):
```
"Add favorites functionality"
```

#### ✅ Excellent Request (Using all principles):

```
Create a favorites system for blog posts.

CONTEXT:
- Tech Stack: React 18, TypeScript, Firebase Firestore
- Existing code: User auth in src/contexts/AuthContext.tsx
- Post data structure in src/types/Post.ts

STEP 1 - Data Structure:
Create a TypeScript interface for favorites:

interface Favorite {
  userId: string;
  postId: string;
  createdAt: Date;
}

REQUIREMENTS:
- Store in Firestore collection called 'favorites'
- Composite key: userId + postId (unique)
- Index on userId for fast queries

STEP 2 - Backend Functions:
Create these functions in src/services/favoritesService.ts

Functions needed:
1. addFavorite(userId: string, postId: string): Promise<void>
2. removeFavorite(userId: string, postId: string): Promise<void>
3. isFavorite(userId: string, postId: string): Promise<boolean>
4. getUserFavorites(userId: string): Promise<Post[]>

EXAMPLE - Following our existing pattern from likeService.ts:
export const addLike = async (userId: string, postId: string) => {
  const likeRef = doc(db, 'likes', `${userId}_${postId}`);
  await setDoc(likeRef, {
    userId,
    postId,
    createdAt: serverTimestamp()
  });
};

Follow this same pattern for favorites, including:
- Error handling with try/catch
- Firestore transaction for add/remove to prevent race conditions
- Return meaningful error messages
```

---

## Common Mistakes to Avoid

### 1. Assuming AI Knows Your Codebase
❌ "Update the function to match the other one"  
✅ "Update this function to match the pattern in src/utils/validation.ts (paste relevant code)"

### 2. Using Jargon Without Explanation
❌ "Add the usual CRUD stuff"  
✅ "Add CRUD operations: Create, Read, Update, Delete functions for the User model"

### 3. Combining Unrelated Tasks
❌ "Fix the login bug and also add dark mode to the settings page"  
✅ Handle these as two separate conversations

### 4. Not Specifying Error Handling
❌ "Create an API call function"  
✅ "Create an API call function that handles network errors, 404s, 500s, and timeout after 10 seconds"

### 5. Forgetting to Mention Constraints
❌ "Optimize this function"  
✅ "Optimize this function for readability. Performance is not critical here, prioritize maintainability"

---

## Quick Reference Checklist

Before sending a prompt, verify:

- [ ] **Specific**: Is it clear exactly what you want?
- [ ] **Context**: Have you explained the tech stack, file purpose, constraints?
- [ ] **Broken Down**: Is this focused on one manageable task?
- [ ] **Examples**: Have you shown what you want (if applicable)?
- [ ] **Constraints**: Have you mentioned limitations, performance needs, style requirements?
- [ ] **Error Handling**: Have you specified how to handle errors?
- [ ] **Output Format**: Have you said what format you want (function, class, interface, etc.)?

---

## Practice Projects

### Project 1: Todo List Function
Write a prompt to create a "mark todo as complete" function. Apply all four principles.

### Project 2: API Integration
Write a prompt to integrate a weather API. Include context, examples, and break it into steps.

### Project 3: Refactoring Request
Take a piece of your code and write a prompt asking AI to refactor it. Be specific about what improvements you want.

---

## Next Steps

After mastering basic prompting:
1. ✅ Complete the practice exercises above
2. ✅ Try writing 10 well-structured prompts for your own coding tasks
3. ✅ Move to **phase2-practical-prompting-techniques.md** to learn Few-Shot, Chain-of-Thought, and Role Prompting
4. ✅ Track what works well in your prompt style

**Remember:** Good prompting is a skill that improves with practice. Start applying these principles today!

---

## Key Takeaways

1. **Be Specific**: Vague requests get vague results. Describe exactly what you want.
2. **Provide Context**: Tech stack, file purpose, constraints, and existing patterns matter.
3. **Break It Down**: Complex tasks work better as a series of focused steps.
4. **Use Examples**: Show don't just tell - examples clarify expectations perfectly.
5. **Iterate**: First response not perfect? Refine your prompt and try again.

The difference between frustration and productivity with AI often comes down to how well you communicate your needs. Master these basics, and you'll be amazed at what AI can help you accomplish.
