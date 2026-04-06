# Phase 3: Git Workflows with AI (Basics)

**Goal**: Master AI-assisted Git workflows for better collaboration and version control  
**Prerequisites**: Complete Phase 1 (Foundations) and Phase 2 (Prompt Engineering)

---

## Table of Contents
1. [Understanding AI-Assisted Git Workflows](#understanding-ai-assisted-git-workflows)
2. [Generate Meaningful Commit Messages](#generate-meaningful-commit-messages)
3. [Write Clear PR Descriptions](#write-clear-pr-descriptions)
4. [Get Quick Code Review Feedback](#get-quick-code-review-feedback)
5. [Understand Git Commands](#understand-git-commands)
6. [Create .gitignore Files](#create-gitignore-files)
7. [Best Practices for Git with AI](#best-practices-for-git-with-ai)
8. [Tools and Integrations](#tools-and-integrations)
9. [Advanced Git Workflows](#advanced-git-workflows)
10. [Hands-On Exercises](#hands-on-exercises)

---

## Understanding AI-Assisted Git Workflows

### Why Use AI for Git Workflows?

**Traditional Git Workflow Pain Points:**
- 😫 Writing descriptive commit messages takes time
- 😫 PR descriptions often vague or incomplete
- 😫 Self-review before pushing is tedious
- 😫 Complex git commands are hard to remember
- 😫 .gitignore files need research for each stack

**AI Solutions:**
- ✅ Generate commit messages from diffs in seconds
- ✅ Create comprehensive PR descriptions automatically
- ✅ Quick pre-push code review
- ✅ Explain complex git commands in plain English
- ✅ Generate stack-specific .gitignore files instantly

### What AI Brings to Git

**Time Savings:**
| Task | Manual Time | With AI | Time Saved |
|------|------------|---------|------------|
| Commit message | 2-5 min | 10-30 sec | 80-90% |
| PR description | 10-20 min | 1-2 min | 90-95% |
| Pre-push review | 15-30 min | 3-5 min | 80-85% |
| Git command lookup | 5-10 min | 30 sec | 90-95% |
| .gitignore creation | 10-15 min | 1 min | 95% |

**Quality Improvements:**
- More descriptive commit messages
- Comprehensive PR descriptions
- Catch issues before pushing
- Learn git commands better
- Complete .gitignore coverage

---

## Generate Meaningful Commit Messages

### Pattern 1: Commit Message from Git Diff

**VS Code Integration:**
1. Stage your changes (Git panel)
2. Click the sparkle ✨ icon in commit message box
3. AI generates message from staged changes

**Or via Copilot Chat:**
```
Generate a commit message for these changes:

[paste git diff or describe changes]

Follow conventional commits format:
- type(scope): description
- Types: feat, fix, docs, style, refactor, test, chore
- Keep first line under 50 chars
- Add body if needed explaining WHY
```

**Example:**

**Your Changes:**
```diff
+ async function sendEmail(to, subject, body) {
+   const transporter = createTransporter();
+   await transporter.sendMail({ to, subject, html: body });
+ }

- function sendEmail(to, subject, body) {
-   // Old synchronous email sending
- }
```

**AI Generated:**
```
feat(email): add async email sending with nodemailer

- Replace synchronous email function with async version
- Use nodemailer transporter for better reliability
- Return promise for error handling in callers
```

### Pattern 2: Conventional Commits Format

**Prompt Template:**
```
Generate a conventional commit message for:

Changes:
- [list your changes]

Format:
type(scope): short description

[optional body]

[optional footer]

Types to use: feat, fix, docs, style, refactor, perf, test, chore
```

**Example:**
```
Changes:
- Added user authentication with JWT
- Created login and register endpoints
- Added middleware for protected routes
- Updated user model with password hashing

AI generates:
feat(auth): implement JWT-based authentication

- Add login and register endpoints with email/password
- Create authentication middleware for protected routes
- Update User model with bcrypt password hashing
- Add JWT token generation and verification

Closes #123
```

### Pattern 3: Multiple Commits (Atomic)

**When you have multiple logical changes:**

```
I made several changes. Help me split into atomic commits:

Changes:
1. Fixed bug in user registration validation
2. Added unit tests for registration
3. Updated documentation for API endpoints
4. Refactored email validation to separate function

Generate separate commit messages for each logical change.
```

**AI Suggests:**
```
Commit 1:
fix(auth): correct email validation in user registration
- Fix regex pattern to properly validate email format
- Add test case for edge case emails

Commit 2:
test(auth): add unit tests for registration endpoint
- Test successful registration
- Test validation errors
- Test duplicate email handling

Commit 3:
docs(api): update authentication endpoint documentation
- Document registration endpoint
- Add request/response examples
- Update authentication flow diagram

Commit 4:
refactor(validation): extract email validation to utility
- Move email validation to utils/validators.js
- Make validation reusable across codebase
- Add JSDoc comments
```

### Pattern 4: Commit Message from Branch Name

**Prompt:**
```
My branch name is: feature/USER-123-add-payment-integration

Generate a commit message that matches this branch.
My changes: [describe changes]
```

**AI Generates:**
```
feat(payment): integrate Stripe payment processing [USER-123]

- Add Stripe SDK and configure with API keys
- Create payment intent endpoint
- Add webhook handler for payment confirmation
- Update Order model with payment status

Relates to USER-123
```

### Commit Message Best Practices

**Good Commit Messages:**
```
✅ feat(auth): add two-factor authentication support
✅ fix(api): handle null values in user query
✅ refactor(db): optimize user lookup query
✅ docs(readme): add installation instructions
✅ test(auth): add integration tests for login flow
```

**Bad Commit Messages:**
```
❌ "fixes"
❌ "update code"
❌ "WIP"
❌ "final changes"
❌ "asdfasdf"
```

**AI Prompt for Quality:**
```
Review this commit message for quality:
"[your message]"

Check:
- Is it descriptive enough?
- Does it explain WHY, not just WHAT?
- Is it properly formatted?
- Is it too long/short?

Suggest improvements if needed.
```

### Commit Message Templates

**Save these prompts for reuse:**

**Template 1: Feature Addition**
```
Generate commit message for new feature:

Feature: [description]
Files changed: [list]
Why needed: [reason]

Use conventional commits with detailed body.
```

**Template 2: Bug Fix**
```
Generate commit message for bug fix:

Bug: [what was broken]
Fix: [how you fixed it]
Impact: [what this affects]
Files: [changed files]

Include issue number if available: #[number]
```

**Template 3: Refactoring**
```
Generate commit message for refactoring:

What was refactored: [component/function]
Why refactored: [reason]
How it changed: [approach]
Impact: [behavior changes, if any]

Focus on explaining value of refactoring.
```

---

## Write Clear PR Descriptions

### Pattern 1: PR Description from Git Diff

**Prompt:**
```
Generate a pull request description for these changes:

Branch: [branch name]
Commits: [list of commits or paste git log]
Diff: [paste git diff or describe changes]

Format:
## Summary
## Changes
## Testing
## Screenshots (if UI changes)
## Checklist
```

**Example Output:**
```markdown
## Summary
Implements user authentication system with JWT tokens and role-based access control.

## Problem
Users needed a secure way to log in and access protected resources. Previous session-based auth didn't scale well.

## Solution
- Implemented JWT-based authentication
- Added role-based middleware for authorization
- Created secure password hashing with bcrypt
- Added token refresh mechanism

## Changes
- `src/auth/login.ts` - New login endpoint with JWT generation
- `src/auth/register.ts` - User registration with password hashing
- `src/middleware/authenticate.ts` - JWT verification middleware
- `src/middleware/authorize.ts` - Role-based authorization
- `src/models/User.ts` - Updated with password and role fields
- `tests/auth.test.ts` - Comprehensive auth tests

## Testing
- ✅ Unit tests for authentication functions
- ✅ Integration tests for login/register endpoints
- ✅ Manual testing with Postman
- ✅ Tested token expiration and refresh

## Security Considerations
- Passwords hashed with bcrypt (12 rounds)
- Tokens expire after 1 hour
- Refresh tokens stored securely
- No sensitive data in JWT payload

## Checklist
- [x] Tests pass locally
- [x] Code follows style guidelines
- [x] Documentation updated
- [x] No console.logs or debug code
- [x] Environment variables documented

## Related Issues
Closes #123
Relates to #124
```

### Pattern 2: PR Description Template

**Prompt:**
```
Create a PR description using our team template:

Template:
## What
## Why
## How
## Testing
## Deployment Notes
## Rollback Plan

My changes:
[describe your changes]
```

### Pattern 3: Quick PR Description

**For small PRs:**
```
Generate a concise PR description (3-4 sentences) for:

Changes: [brief description]
Impact: [who/what is affected]
Testing: [how you tested]
```

**Example:**
```
This PR fixes a memory leak in the WebSocket connection handler. The issue occurred when clients disconnected unexpectedly, leaving event listeners attached. Fixed by properly cleaning up listeners in the disconnect handler. Tested with 1000 concurrent connections and verified no memory growth.
```

### Pattern 4: PR Description with Visual Changes

**For UI changes:**
```
Generate PR description for UI changes:

Component: [name]
Changes: [what changed visually]
User impact: [how users will notice]

Include:
- Before/After section
- Browser testing notes
- Accessibility improvements
```

### PR Description Best Practices

**Include:**
- ✅ **Summary**: What the PR does (2-3 sentences)
- ✅ **Why**: Business reason or problem being solved
- ✅ **How**: Technical approach taken
- ✅ **Testing**: How you verified it works
- ✅ **Impact**: What changes for users/developers
- ✅ **Checklist**: Pre-merge requirements
- ✅ **Screenshots**: For visual changes
- ✅ **Links**: Related issues, docs, design specs

**Avoid:**
- ❌ Vague descriptions: "Updated code"
- ❌ No context: Just a list of files changed
- ❌ No testing info: How do reviewers verify?
- ❌ Missing impact: Why should this be merged?

### Advanced PR Descriptions

**Prompt for Complex PRs:**
```
Generate comprehensive PR description for large feature:

Feature: [name and purpose]
Files changed: [number and areas]
Breaking changes: [yes/no, details]
Migration needed: [yes/no, steps]
Dependencies: [new packages, version bumps]

Format for technical audience with:
- Architecture decisions
- Performance implications
- Security considerations
- Deployment sequence
- Rollback procedure
```

---

## Get Quick Code Review Feedback

### Pattern 1: Pre-Push Self Review

**Before pushing, run your own AI review:**

```
Review my changes before I push to remote:

[paste git diff]

Check for:
1. Obvious bugs
2. Security issues
3. Missing error handling
4. Code smells
5. Performance problems
6. Missing tests
7. Breaking changes

Be critical but constructive.
```

**Example:**
```diff
+ function deleteUser(userId) {
+   db.query(`DELETE FROM users WHERE id = ${userId}`);
+   return { success: true };
+ }
```

**AI Review:**
```
🚨 CRITICAL ISSUES:

1. SQL Injection Vulnerability (SECURITY)
   - Using string interpolation for SQL query
   - Fix: Use parameterized queries
   
   function deleteUser(userId) {
     db.query('DELETE FROM users WHERE id = ?', [userId]);
     return { success: true };
   }

2. No Error Handling (RELIABILITY)
   - Query could fail but not handled
   - Should use try-catch or return promise

3. Missing Authorization Check (SECURITY)
   - Anyone can delete any user
   - Add permission verification

4. No Database Transaction (DATA INTEGRITY)
   - Related data (posts, comments) left orphaned
   - Consider cascade delete or manual cleanup

5. Synchronous Operation (PERFORMANCE)
   - Should be async
   - Blocking other requests

Recommendation: DO NOT PUSH until fixing items 1-3 (critical).
```

### Pattern 2: Specific Aspect Review

**Focus review on specific concerns:**

```
Review my changes for [ASPECT]:

Aspects to choose from:
- Security vulnerabilities
- Performance issues
- Test coverage
- Error handling
- Type safety (TypeScript)
- Breaking changes
- Documentation needs

[paste git diff]
```

**Example (Security Focus):**
```
Review for security vulnerabilities:

[paste authentication code changes]

AI identifies:
1. Password stored in plain text
2. No rate limiting on login
3. Session token in localStorage (XSS risk)
4. CORS configured with '*'
5. No CSRF protection
```

### Pattern 3: Compare with Best Practices

**Prompt:**
```
Review my changes against [framework/language] best practices:

Framework: [React/Node.js/Python/etc]
Changes: [paste diff]

Check alignment with:
- Official style guide
- Common patterns
- Anti-patterns to avoid
- Framework-specific gotchas
```

### Pattern 4: Breaking Change Detection

**Before pushing to shared branch:**

```
Analyze if my changes introduce breaking changes:

Changes: [paste diff]
Project type: [library/API/application]
Versioning: [semver]

Check for:
- Changed function signatures
- Removed public APIs
- Modified data structures
- Changed behavior of existing features
- New required dependencies

Suggest version bump: patch/minor/major
```

### Pattern 5: Minimal Review Checklist

**Quick pre-push checklist:**

```
Quick review checklist for my commit:

[paste git diff or describe changes]

Verify:
✓ No console.log() or debug code
✓ No commented-out code
✓ No secrets or API keys
✓ No TODO comments (or create issues)
✓ Consistent code style
✓ Tests included/updated
✓ No unintentional changes

Return just: PASS or FAIL with issues.
```

### Review Best Practices

**When to Self-Review:**
- ✅ Before pushing to shared branch
- ✅ Before creating PR
- ✅ After resolving merge conflicts
- ✅ Before deploying to production
- ✅ When touching critical code paths

**What to Check:**
1. **Security** (highest priority)
   - No secrets committed
   - No SQL injection risks
   - Proper authentication/authorization
   - Input validation

2. **Correctness**
   - Logic errors
   - Edge cases handled
   - Error handling present

3. **Quality**
   - Code style consistency
   - No debug code
   - Proper naming
   - Comments where needed

4. **Testing**
   - Tests updated/added
   - All tests passing
   - Edge cases covered

5. **Impact**
   - Breaking changes identified
   - Migration path clear
   - Documentation updated

---

## Understand Git Commands

### Pattern 1: Explain Git Command

**When you see a git command you don't understand:**

```
Explain this git command in simple terms:

git rebase -i HEAD~3

What does it do?
When should I use it?
What are the risks?
Show me a practical example.
```

**AI Explains:**
```
Command: git rebase -i HEAD~3

What it does:
Opens an interactive editor to modify the last 3 commits. You can:
- Reorder commits
- Squash commits together
- Edit commit messages
- Delete commits
- Split commits

When to use:
- Clean up messy commit history before pushing
- Combine small commits into logical units
- Fix typos in recent commit messages
- Reorder commits for better history

Risks:
⚠️ NEVER use on commits already pushed to shared branch
⚠️ Rewrites history (changes commit hashes)
⚠️ Can cause conflicts if others based work on your commits

Safe alternative for shared branches: git merge --squash

Example:
# Clean up last 3 commits before pushing
git rebase -i HEAD~3

Editor opens:
pick abc1234 Add feature
pick def5678 Fix typo
pick ghi9012 Add tests

Change to:
pick abc1234 Add feature
squash def5678 Fix typo
squash ghi9012 Add tests

Result: One commit with all changes
```

### Pattern 2: Git Command for Task

**When you need to do something but don't know the command:**

```
What git command(s) do I need to:

[describe what you want to do]

Provide:
- The exact command(s)
- What each flag means
- Step-by-step if multiple commands
- Warning about any risky operations
```

**Examples:**

**Task 1: Undo last commit but keep changes**
```
AI: 
git reset --soft HEAD~1

Explanation:
- reset: Undo commits
- --soft: Keep changes staged
- HEAD~1: Go back one commit

Your changes remain staged, ready to recommit.

⚠️ Only do this if you haven't pushed yet!
```

**Task 2: See what changed in a file over time**
```
AI:
git log -p <filename>

Explanation:
- log: Show commit history
- -p: Show patch (actual changes)
- <filename>: Limit to one file

Example:
git log -p src/auth.js

Shows every commit that changed auth.js with the actual diff.

Alternative (more visual):
git log --oneline --graph --all -- src/auth.js
```

### Pattern 3: Fix Git Mistakes

**When something went wrong:**

```
I made a git mistake:

What I did: [describe what happened]
Current state: [git status output]
What I wanted: [intended outcome]

How do I fix this?
Provide safe step-by-step recovery.
```

**Common Mistakes:**

**Mistake 1: Committed to wrong branch**
```
AI Solution:
# 1. Note the commit hash
git log -1  # Copy the commit hash

# 2. Switch to correct branch
git checkout correct-branch

# 3. Cherry-pick the commit
git cherry-pick <commit-hash>

# 4. Go back to wrong branch
git checkout wrong-branch

# 5. Remove the commit from wrong branch
git reset --hard HEAD~1

⚠️ Only if you haven't pushed yet!
```

**Mistake 2: Accidentally deleted unstaged changes**
```
I ran 'git checkout .' and lost my changes!

AI:
Unfortunately, unstaged changes aren't in git history.
Git can't recover them. However:

1. Check your editor's auto-save:
   - VS Code: File > Open Recent
   - Look for .swp files (Vim)

2. Check local backup tools:
   - Time Machine (Mac)
   - File History (Windows)

3. Prevention for future:
   git stash  # Instead of git checkout .

Lesson: Always commit or stash before discarding changes.
```

### Pattern 4: Complex Git Workflows

**Understanding advanced workflows:**

```
Explain the git workflow for:

Workflow: [feature branch / GitFlow / trunk-based / etc.]

Include:
- Branch naming conventions
- Merge vs rebase strategy
- When to create PR
- How to handle conflicts
- Release process
```

### Pattern 5: Git Command Alternatives

**Find better ways to do things:**

```
I currently use this git command:
[your command]

Is there a better/safer/faster way to do this?
What are the pros/cons of alternatives?
```

**Example:**
```
I use: git add . && git commit -m "changes"

AI suggests:
Better alternatives:

1. Stage selectively:
   git add -p  # Review each change
   
   Pros: Catch unintended changes
   Cons: Takes more time

2. Use interactive staging:
   git add -i
   
   Pros: Full control
   Cons: More complex

3. Commit specific files:
   git commit -am "message"
   
   Note: Only stages modified tracked files
   Pros: Won't commit new files accidentally
   Cons: Misses new files

Best practice:
1. Review changes: git status
2. Stage intentionally: git add src/
3. Meaningful message: git commit -m "feat: ..."
```

### Git Learning Prompts

**Build git knowledge:**

```
Teach me about git [concept]:

Concepts:
- rebasing
- cherry-picking
- stashing
- branching strategies
- merge conflicts
- detached HEAD
- submodules
- bisect

Explain:
- What it is
- When to use it
- How to do it
- Common mistakes
- Best practices
```

---

## Create .gitignore Files

### Pattern 1: Stack-Specific .gitignore

**Prompt:**
```
Generate a .gitignore file for:

Stack:
- [Language: Node.js/Python/Java/etc.]
- [Framework: React/Django/Spring/etc.]
- [Tools: Docker/VS Code/etc.]
- [OS: macOS/Windows/Linux]

Include:
- Language-specific ignores
- Framework build artifacts
- Editor/IDE files
- OS files
- Environment files
- Common temporary files
```

**Example (Node.js + React + VS Code):**
```gitignore
# AI Generated:

# Dependencies
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Production build
/build
/dist
*.tgz

# Testing
/coverage
.nyc_output

# Environment
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# Editor (VS Code)
.vscode/*
!.vscode/settings.json
!.vscode/tasks.json
!.vscode/launch.json
!.vscode/extensions.json

# OS
.DS_Store
Thumbs.db

# Logs
logs/
*.log

# Temporary files
*.tmp
*.temp
*.swp
*.swo
*~

# IDE
.idea/
*.iml
*.sublime-*
```

### Pattern 2: Add to Existing .gitignore

**Prompt:**
```
I need to add ignores for [new tool/framework] to my .gitignore.

Current .gitignore:
[paste current file]

Add ignores for:
[tool/framework/pattern]

Don't duplicate existing patterns.
```

### Pattern 3: Project-Specific Ignores

**Prompt:**
```
Generate .gitignore for project with:

Project type: [monorepo/microservices/library]
Special needs:
- [Database files to ignore]
- [Generated documentation]
- [Build artifacts from specific tools]
- [Cache directories]

Include comments explaining each section.
```

**Example (Monorepo):**
```gitignore
# Monorepo .gitignore

# Root level
node_modules/
.env

# Package-level ignores
packages/*/dist
packages/*/build
packages/*/.next
packages/*/coverage

# Workspace tools
.pnp.*
.yarn/*
!.yarn/patches
!.yarn/plugins
!.yarn/releases
!.yarn/sdks
!.yarn/versions

# Documentation (generated)
docs/api/
*.md.backup

# Logs from all packages
**/logs/
**/*.log

# OS and Editor
.DS_Store
.vscode/
.idea/
```

### Pattern 4: Security-Focused .gitignore

**Prompt:**
```
Review my .gitignore for security issues.

Current file:
[paste .gitignore]

Check for:
- Unignored secret files (.env, keys, etc.)
- Unignored config files with sensitive data
- Patterns that might leak credentials
- Missing common secret file patterns

Suggest additions to prevent accidental commits of secrets.
```

### Pattern 5: .gitignore Explanation

**Understand existing patterns:**

```
Explain this .gitignore pattern:

pattern: **/*.map
or
pattern: !important.txt

What does it do?
Why would I use it?
What matches it?
```

**AI Explains:**
```
Pattern: **/*.map

What it does:
Ignores all .map files in any directory at any depth.

Breakdown:
- **: Match any directory (recursive)
- /: Directory separator
- *.map: Any file ending in .map

What matches:
✓ src/index.js.map
✓ build/vendor/library.min.js.map
✓ deeply/nested/folders/file.map

Why use it:
Source map files are generated during build.
They're large and not needed in version control.
Developers generate them locally when needed.

---

Pattern: !important.txt

What it does:
UN-ignores a file that would otherwise be ignored.

Example:
*.txt           # Ignore all .txt files
!important.txt  # But keep this one

Use case:
When you have a broad ignore pattern but need exceptions.
```

### .gitignore Best Practices

**Structure:**
```gitignore
# Organize by category with comments

# === Dependencies ===
node_modules/
vendor/

# === Build Outputs ===
/dist
/build
*.exe

# === Environment & Secrets ===
.env
.env.local
*.key
*.pem

# === IDE & Editors ===
.vscode/
.idea/
*.sublime-*

# === Operating System ===
.DS_Store
Thumbs.db

# === Logs & Temporary Files ===
*.log
*.tmp
logs/

# === Project Specific ===
/uploads
/cache
```

**Tips:**
- ✅ Comment sections for clarity
- ✅ Use `/` prefix for root-only ignores
- ✅ Group related patterns
- ✅ Include common patterns first
- ✅ Add project-specific patterns last
- ✅ Use `!` for exceptions (keep certain files)
- ✅ Test with `git check-ignore -v <file>`

**Common Patterns:**

```gitignore
# Ignore directory
node_modules/

# Ignore at root only
/build

# Ignore everywhere
*.log

# Ignore in specific directory
src/*.tmp

# Keep exception
*.txt
!important.txt

# Ignore everything in folder except one file
folder/*
!folder/keep.txt
```

---

## Best Practices for Git with AI

### Practice 1: Commit Message Quality

**Golden Rules:**
1. **First line < 50 characters** (summary)
2. **Blank line after subject**
3. **Body wraps at 72 characters**
4. **Explain WHY, not WHAT** (code shows what)
5. **Use imperative mood** ("Add feature" not "Added feature")

**AI Prompt for Enforcement:**
```
Review this commit message for quality:

"[your message]"

Check:
1. First line under 50 chars?
2. Explains WHY not just WHAT?
3. Uses imperative mood?
4. Follows conventional commits?
5. References issue if applicable?

If it fails any check, suggest improvements.
```

### Practice 2: Atomic Commits

**One logical change per commit:**

**Bad (Multiple Changes):**
```
feat: add login, fix bug, update docs, refactor utils
```

**Good (Atomic):**
```
Commit 1: feat(auth): add login with JWT
Commit 2: fix(validation): correct email regex
Commit 3: docs(api): update authentication guide
Commit 4: refactor(utils): extract validators
```

**AI Helper:**
```
I have multiple changes. Split into atomic commits:

Changes:
[list all your changes]

For each atomic commit suggest:
- Message
- Files to include
- Order to commit
```

### Practice 3: Branch Naming

**Consistent naming conventions:**

```
Generate branch name for:

Type: [feature/bugfix/hotfix/release]
Issue: [ticket number if applicable]
Description: [what it does]

Follow pattern: type/ISSUE-123-short-description
```

**Examples:**
```
feature/USER-456-add-payment-integration
bugfix/API-789-fix-null-pointer
hotfix/PROD-012-security-patch
refactor/CODE-345-cleanup-auth
```

### Practice 4: PR Review Checklist

**Before creating PR:**

```
Pre-PR checklist for my changes:

[paste git diff or describe changes]

Verify:
✓ Branch named correctly
✓ All commits have good messages
✓ No merge conflicts
✓ Tests pass locally
✓ No debug code (console.log, etc.)
✓ No secrets committed
✓ Code reviewed by yourself
✓ Documentation updated
✓ CHANGELOG updated (if applicable)
✓ Breaking changes documented

Return: READY or NOT READY with missing items.
```

### Practice 5: Keeping History Clean

**Strategies:**

**Before Pushing:**
- Squash related commits: `git rebase -i`
- Fix commit messages: `git commit --amend`
- Reorder commits: `git rebase -i`

**AI Helps:**
```
My last 5 commits are messy:

[paste git log output]

How should I clean this before pushing?
Provide exact git commands.
```

**After Getting Feedback:**
- Fixup commits: `git commit --fixup <hash>`
- Autosquash: `git rebase -i --autosquash`

### Practice 6: Meaningful PR Titles

**Format:**
```
[TYPE] Brief description (under 60 chars)

Types:
- [FEATURE] - New functionality
- [FIX] - Bug fix
- [REFACTOR] - Code improvement
- [DOCS] - Documentation
- [TEST] - Test addition/update
- [CHORE] - Maintenance
```

**AI Generator:**
```
Generate PR title from:

Changes: [summary of PR]
Issue: [if applicable]

Format: [TYPE] Brief description
Keep under 60 characters.
```

### Practice 7: Reviewing Your Own PRs

**Self-review before requesting review:**

```
Review my PR as if you're a senior developer:

PR: [paste description or link to diff]

Check:
1. Code quality and readability
2. Test coverage
3. Security concerns
4. Performance implications
5. Breaking changes
6. Documentation completeness

Be thorough and critical.
```

---

## Tools and Integrations

### VS Code Extensions

**1. GitLens**
- Visualize code authorship
- See commit history inline
- Compare changes across branches
- **AI Integration**: Ask Copilot about commit history

**Usage with AI:**
```
[See a confusing change in GitLens]
Ask Copilot: "Why was this line changed in commit abc1234?"
```

**2. Git Graph**
- Visual commit graph
- Branch management
- Interactive rebase UI

**3. GitHub Pull Requests**
- Create and review PRs in VS Code
- Inline PR comments
- **AI Integration**: Generate PR descriptions without leaving editor

### 4. GitHub Copilot Chat (Built-in)

**Git-specific features:**

**Generate commit message:**
```
1. Stage changes
2. Focus on Source Control panel
3. Click ✨ sparkle icon
4. AI generates message from staged changes
```

**Review changes:**
```
# In Copilot Chat
Review the changes I'm about to commit for:
- Bugs
- Security issues
- Breaking changes
```

**Explain commits:**
```
# Select commit in VS Code
# Ask Copilot
What does this commit do? Why was this change made?
```

### Command Line Tools

**1.git-ai (CLI Tool)**
```bash
# Install
npm install -g git-ai

# Generate commit message
git-ai commit

# Generate PR description
git-ai pr
```

**2. GitHub CLI (`gh`)**
```bash
# Create PR with AI description
gh pr create --fill

# Review PR with comments
gh pr review 123

# Combine with AI:
gh pr view 123 --json body | copilot "summarize this PR"
```

**3. Commitizen**
```bash
# Interactive commit message builder
npm install -g commitizen

# Use with AI
cz
# AI can help answer the prompts
```

### Automation Tools

**1. Pre-commit Hooks**

**Setup:**
```bash
# .husky/pre-commit
#!/bin/sh
npm run lint
npm run test
npm run ai-review  # Custom script using AI
```

**AI Review Script:**
```javascript
// ai-review.js
const { execSync } = require('child_process');

const diff = execSync('git diff --cached').toString();

// Send to AI for review
// Fail commit if critical issues found
```

**2. GitHub Actions**

**AI Code Review Action:**
```yaml
name: AI Code Review
on: [pull_request]
jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: AI Review
        uses: action-ai-reviewer@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

**3. Danger JS**
```javascript
// dangerfile.js
import { danger, warn, fail } from 'danger';

// Check PR description
if (danger.github.pr.body.length < 50) {
  warn('Please add a detailed PR description');
}

// AI-generated checks
const aiReview = await callAI(danger.git.diff);
if (aiReview.criticalIssues.length > 0) {
  fail('Critical issues found');
}
```

### Workflow Helpers

**1. Create Custom VS Code Tasks**

**.vscode/tasks.json:**
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "AI Pre-Push Review",
      "type": "shell",
      "command": "git diff origin/main | copilot review",
      "problemMatcher": []
    },
    {
      "label": "Generate Commit Message",
      "type": "shell",
      "command": "git diff --staged | copilot commit-message",
      "problemMatcher": []
    }
  ]
}
```

**2. Git Aliases**

**~/.gitconfig:**
```ini
[alias]
  # AI-assisted aliases
  ai-commit = "!git diff --staged | copilot commit-msg"
  ai-review = "!git diff | copilot review --security"
  ai-pr = "!gh pr create --title \"$(copilot pr-title)\" --body \"$(copilot pr-body)\""
  
  # Helpful shortcuts
  lg = log --graph --oneline --all
  s = status -s
  unstage = reset HEAD --
```

### Integration Patterns

**Pattern 1: Commit Workflow**
```
1. Make changes
2. `git add -p` (review each change)
3. `git ai-commit` (generate message)
4. Edit if needed
5. Commit
```

**Pattern 2: Pre-Push Workflow**
```
1. `git ai-review` (self-review)
2. Fix any critical issues
3. Squash/clean commits if needed
4. `git push`
```

**Pattern 3: PR Workflow**
```
1. `git push -u origin feature-branch`
2. `git ai-pr` (generate PR with description)
3. Review generated description
4. Create PR
5. Monitor AI-generated review comments
```

---

## Advanced Git Workflows

### Workflow 1: Feature Branch with AI

```
Full feature development workflow:

1. Create branch:
   git checkout -b feature/USER-123-new-feature
   
   [Ask AI: "Suggest branch naming for adding user profile page"]

2. Develop with commits:
   [Make changes]
   git add -p
   [Ask AI: "Generate commit message for these changes"]
   git commit -m "[AI-generated message]"

3. Self-review before pushing:
   [Ask AI: "Review my changes before push"]
   
4. Clean commit history:
   git rebase -i origin/main
   [Ask AI: "Help me squash related commits"]

5. Push:
   git push -u origin feature/USER-123-new-feature

6. Create PR:
   [Ask AI: "Generate PR description"]
   gh pr create --fill

7. Address review comments:
   [Ask AI: "How to best address this review comment: [comment]"]
```

### Workflow 2: Hotfix with AI Assistance

```
Emergency production fix:

1. Create hotfix branch from main:
   git checkout -b hotfix/PROD-456-fix-critical-bug main

2. Make minimal fix:
   [Changes]
   [Ask AI: "Is this fix safe for production? Any side effects?"]

3. Test thoroughly:
   [Ask AI: "Generate test cases for this hotfix"]

4. Commit with detailed message:
   [Ask AI: "Generate hotfix commit message explaining impact"]

5. Create PR:
   [Ask AI: "Generate urgent hotfix PR description with testing details"]

6. Fast-track review:
   Tag reviewers, explain urgency

7. Merge and deploy:
   Cherry-pick to release branch if needed
```

### Workflow 3: Code Review with AI

```
Reviewing others' PRs with AI assistance:

1. Fetch PR:
   gh pr checkout 123

2. Review changes:
   git diff main..HEAD
   
3. AI-assisted review:
   [Ask AI: "Review these PR changes for:
   - Security issues
   - Performance problems
   - Best practices violations
   - Missing tests"]

4. Test locally:
   [Run tests]
   [Ask AI: "What additional test cases should I verify?"]

5. Leave constructive feedback:
   [Ask AI: "How to phrase this review comment constructively?"]

6. Approve or request changes:
   gh pr review --approve or --request-changes
```

### Workflow 4: Merge Conflict Resolution

```
Resolving conflicts with AI help:

1. Attempt merge:
   git merge feature-branch
   # CONFLICT!

2. See conflicts:
   git status
   
3. Ask AI for help:
   [Paste conflicted section]
   "I have a merge conflict. Current code does X, incoming does Y.
   What's the best way to resolve this preserving both intentions?"

4. Resolve with AI guidance:
   [Edit files based on AI suggestion]

5. Verify resolution:
   [Ask AI: "Review my conflict resolution for correctness"]

6. Complete merge:
   git add .
   git commit  # AI generates merge commit message

7. Test:
   [Ask AI: "What should I test after resolving merge conflicts?"]
```

---

## Hands-On Exercises

### Exercise 1: Commit Message Practice (Beginner)

**Task**: Generate 5 different commit messages

**Scenarios:**
1. Added user login feature
2. Fixed bug where emails weren't sending
3. Refactored database queries for performance
4. Updated README with new installation steps
5. Added unit tests for authentication

**Your Prompts** (write and try each):
```
[Your prompt for scenario 1]
[Your prompt for scenario 2]
...
```

**Success Criteria:**
- [ ] Each message follows conventional commits
- [ ] First line under 50 characters
- [ ] Body explains WHY when needed
- [ ] Type (feat/fix/refactor/docs/test) is correct

---

### Exercise 2: PR Description Generator (Intermediate)

**Task**: Create a full PR description

**Given Changes:**
```
- Implemented shopping cart functionality
- Added cart items persistence to localStorage
- Created cart UI component
- Added add/remove/update quantity actions
- Integrated with checkout flow
- Added cart tests
```

**Your Prompt:**
```
[Write comprehensive prompt to generate PR description]
```

**Success Criteria:**
- [ ] Includes Summary, Changes, Testing sections
- [ ] Mentions all files changed
- [ ] Explains business value
- [ ] Has checklist for reviewer
- [ ] Notes any breaking changes

---

### Exercise 3: Pre-Push Review (Intermediate)

**Task**: Review this code before pushing

**Code:**
```javascript
function processPayment(userId, amount) {
  const user = db.query(`SELECT * FROM users WHERE id = ${userId}`);
  
  if (user.balance >= amount) {
    db.query(`UPDATE users SET balance = balance - ${amount} WHERE id = ${userId}`);
    db.query(`INSERT INTO transactions VALUES ('${userId}', ${amount})`);
    return { success: true };
  }
  
  return { success: false, error: 'Insufficient funds' };
}
```

**Your Prompt:**
```
[Write prompt to review this for push]
```

**Expected Issues AI Should Find:**
- [ ] SQL injection vulnerabilities
- [ ] No error handling
- [ ] Race condition (double spending possible)
- [ ] Should use transaction
- [ ] Missing authentication check
- [ ] Function should be async

---

### Exercise 4: Git Command Lookup (Beginner)

**Tasks**: Ask AI for git commands

1. **How to undo last commit but keep changes?**
   ```
   [Your prompt]
   ```

2. **How to see who changed a specific line?**
   ```
   [Your prompt]
   ```

3. **How to cherry-pick a commit from another branch?**
   ```
   [Your prompt]
   ```

**Success Criteria:**
- [ ] AI provides exact command
- [ ] Explains what each flag does
- [ ] Warns about risks
- [ ] Provides example usage

---

### Exercise 5: .gitignore Creation (Beginner)

**Task**: Generate .gitignore for your stack

**Your Project:**
- Language: [Choose: Node.js/Python/Java/etc.]
- Framework: [Choose framework]
- Tools: [Choose: Docker/VS Code/etc.]
- OS: [Your OS]

**Your Prompt:**
```
[Write prompt to generate comprehensive .gitignore]
```

**Success Criteria:**
- [ ] Covers language-specific files
- [ ] Includes build artifacts
- [ ] Ignores environment files
- [ ] Has OS-specific ignores
- [ ] Organized with comments

---

### Exercise 6: Complex Workflow (Advanced)

**Scenario:**
You're working on feature branch `feature/cart`. Meanwhile, `main` branch has new commits. You need to update your branch and create a PR.

**Tasks:**
1. Ask AI for the workflow
2. Ask for merge vs rebase recommendation
3. Generate commit messages if needed
4. Create PR description
5. Handle potential conflicts

**Your Prompts:**
```
[Series of prompts for each task]
```

**Success Criteria:**
- [ ] Understands difference between merge and rebase
- [ ] Knows how to update branch from main
- [ ] Can generate proper PR description
- [ ] Understands conflict resolution

---

## Progress Checklist

### Generate Meaningful Commit Messages
- [ ] Generated commit message from git diff
- [ ] Used conventional commits format
- [ ] Split changes into atomic commits
- [ ] Created commit from branch name
- [ ] Built reusable commit message templates

### Write Clear PR Descriptions
- [ ] Generated PR description from changes
- [ ] Used team PR template
- [ ] Created quick description for small PR
- [ ] Included screenshots for UI changes
- [ ] Added comprehensive description for complex PR

### Get Quick Code Review Feedback
- [ ] Performed pre-push self-review
- [ ] Focused review on security
- [ ] Checked for breaking changes
- [ ] Used review checklist
- [ ] Fixed issues before pushing

### Understand Git Commands
- [ ] Explained complex git command
- [ ] Found git command for specific task
- [ ] Fixed git mistake with AI help
- [ ] Understood advanced workflow
- [ ] Learned better command alternatives

### Create .gitignore Files
- [ ] Generated stack-specific .gitignore
- [ ] Added to existing .gitignore
- [ ] Created project-specific ignores
- [ ] Security-reviewed .gitignore
- [ ] Understood .gitignore patterns

---

## Key Takeaways

**Time Savings:**
- Commit messages: **80-90% faster**
- PR descriptions: **90-95% faster**
- Pre-push review: **80-85% faster**
- Git command lookup: **90-95% faster**
- .gitignore creation: **95% faster**

**Quality Improvements:**
- More descriptive commit messages
- Comprehensive PR documentation
- Catch issues before pushing
- Better git command understanding
- Complete ignore file coverage

**Best Practices:**
1. ✅ **Atomic Commits** - One logical change per commit
2. ✅ **Descriptive Messages** - Explain WHY, not WHAT
3. ✅ **Self-Review** - Always review before pushing
4. ✅ **Clean History** - Squash/rebase before PR
5. ✅ **Comprehensive PRs** - Full context for reviewers
6. ✅ **Security First** - Review for secrets/vulnerabilities
7. ✅ **Learn Git** - Use AI to understand, not just execute

**Tools Worth Using:**
- VS Code GitLens
- GitHub CLI (`gh`)
- Pre-commit hooks
- Git aliases for AI commands
- Custom VS Code tasks

**Common Patterns:**
```
Code → Stage → AI Review → AI Commit Message → Push → AI PR Description
```

**Remember:**
- AI generates suggestions, **you make decisions**
- Review AI-generated content before using
- Learn from AI explanations to improve git skills
- Build personal prompt library for common tasks
- Combine AI with traditional git knowledge

**Next Steps:**
- Practice all exercises
- Create personal git aliases
- Build commit message templates
- Set up pre-push hooks
- Move to [Understanding AI Agents](phase3-ai-agents.md)

---

*Git with AI is about working smarter - spend less time on mechanics, more time on meaningful work.*

**Next**: [Understanding AI Agents (Basics) →](phase3-ai-agents.md)
