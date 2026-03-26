# VS Code Setup for AI-Assisted Development

## Table of Contents
1. [Installation & Configuration](#installation--configuration)
2. [Copilot Chat vs. Inline Suggestions](#copilot-chat-vs-inline-suggestions)
3. [Essential Keyboard Shortcuts](#essential-keyboard-shortcuts)
4. [Understanding Workspace Context](#understanding-workspace-context)
5. [Copilot Edits Mode](#copilot-edits-mode)
6. [Copilot in Terminal](#copilot-in-terminal)
7. [Code Actions & Quick Fixes](#code-actions--quick-fixes)
8. [IntelliSense + AI Synergy](#intellisense--ai-synergy)
9. [Configuration Best Practices](#configuration-best-practices)
10. [Troubleshooting](#troubleshooting)
11. [Hands-On Exercises](#hands-on-exercises)

---

## Installation & Configuration

### Step 1: Install VS Code

**Official Download**: https://code.visualstudio.com/download

**Platform-Specific Notes**:

**macOS**:
```bash
# Via Homebrew (recommended)
brew install --cask visual-studio-code

# Or download .dmg from official site
```
- First launch: May need to allow in System Preferences → Security
- Add to PATH: `Cmd+Shift+P` → "Shell Command: Install 'code' command in PATH"

**Windows**:
- Download installer from official site
- ✅ Check "Add to PATH" during installation
- ✅ Check "Register Code as editor for supported file types"
- Restart terminal after installation

**Linux**:
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install code

# Fedora/RHEL
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo dnf install code
```

---

### Step 2: Install GitHub Copilot

**Prerequisites**:
- Active GitHub account
- GitHub Copilot subscription (individual, business, or enterprise)
  - **Free tier**: Available for verified students, teachers, and open source maintainers
  - **Individual**: $10/month or $100/year (as of 2026)
  - **Business/Enterprise**: Contact GitHub

**Official Documentation**: https://docs.github.com/en/copilot/using-github-copilot/getting-started-with-github-copilot

#### Installation Steps:

1. **Open VS Code Extensions**
   - `Cmd+Shift+X` (macOS) or `Ctrl+Shift+X` (Windows/Linux)
   - Or click Extensions icon in Activity Bar

2. **Search for "GitHub Copilot"**
   - Install **"GitHub Copilot"** (main extension)
   - Install **"GitHub Copilot Chat"** (chat interface)

3. **Sign in to GitHub**
   - After installation, VS Code will prompt you to sign in
   - Click "Sign in to GitHub"
   - Authorize VS Code in browser
   - Return to VS Code

4. **Verify Installation**
   - Look for Copilot icon in status bar (bottom-right)
   - Icon should show checkmark (enabled) or X (disabled)
   - Click icon to toggle on/off

**🔗 Direct Extension Links**:
- GitHub Copilot: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot
- GitHub Copilot Chat: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat

---

### Step 3: Initial Configuration

**Key Settings to Review**:

1. **Open Settings**
   - `Cmd+,` (macOS) or `Ctrl+,` (Windows/Linux)
   - Or: `Code` → `Preferences` → `Settings`

2. **GitHub Copilot Settings**

Search for "copilot" in settings:

```json
{
  // Enable/disable Copilot
  "github.copilot.enable": {
    "*": true,
    "markdown": true,
    "plaintext": false
  },
  
  // Inline suggestion behavior
  "github.copilot.editor.enableAutoCompletions": true,
  
  // Enable Copilot in chat
  "github.copilot.chat.enabled": true,
  
  // Advanced: Enable for specific languages
  "github.copilot.enable": {
    "python": true,
    "javascript": true,
    "typescript": true,
    "html": true,
    "css": true
  }
}
```

**⚠️ Important Settings to Consider**:

**Editor Settings (affects AI and coding experience)**:
```json
{
  // Show inline suggestions
  "editor.inlineSuggest.enabled": true,
  
  // Accept suggestion on Tab (default)
  "editor.tabCompletion": "on",
  
  // Suggest on trigger characters
  "editor.suggestOnTriggerCharacters": true,
  
  // Quick suggestions delay (milliseconds)
  "editor.quickSuggestionsDelay": 10,
  
  // Font ligatures (optional, for better code reading)
  "editor.fontLigatures": true,
  
  // Minimap (overview of file)
  "editor.minimap.enabled": true
}
```

---

### Step 4: Verify Everything Works

**Quick Test**:

1. Create new file: `test.js`
2. Type: `// Function to calculate factorial`
3. Press `Enter`
4. Wait 1-2 seconds
5. **Expected**: Gray inline suggestion appears
6. Press `Tab` to accept

**If no suggestions appear**, see [Troubleshooting](#troubleshooting) section.

---

## Copilot Chat vs. Inline Suggestions

Understanding the difference between these two modes is crucial for effective AI-assisted development.

### Inline Suggestions (Ghost Text)

**What it is**:
- Gray text that appears as you type
- Predicts what you'll write next
- Based on current file context

**When to use**:
- Writing standard code patterns
- Implementing known algorithms
- Creating boilerplate code
- Continuing existing patterns

**How to use**:
1. Start typing code or comment
2. Wait for gray suggestion to appear
3. `Tab` - Accept entire suggestion
4. `Cmd+→` (macOS) or `Ctrl+→` (Windows/Linux) - Accept word by word
5. `Esc` - Dismiss suggestion
6. `Alt+]` - Next suggestion
7. `Alt+[` - Previous suggestion

**Example Workflow**:
```javascript
// Type this:
function sortArray

// Copilot suggests:
function sortArray(arr) {
  return arr.sort((a, b) => a - b);
}

// Press Tab to accept
```

**Pros**:
- ⚡ Very fast - appears instantly
- 🎯 Context-aware - based on your file
- 🔄 Non-disruptive - doesn't break flow
- 💰 Low overhead - minimal token usage

**Cons**:
- ❌ Can't ask questions
- ❌ Can't explain code
- ❌ Limited to current file context
- ❌ Can't handle complex multi-step tasks

---

### Copilot Chat (Interactive Mode)

**What it is**:
- Conversational interface with AI
- Appears in sidebar or inline
- Can discuss code, ask questions, explain concepts

**When to use**:
- Need explanations of code
- Asking "how to do X"
- Debugging complex issues
- Learning new concepts
- Refactoring suggestions
- Code reviews

**How to access**:

**Method 1: Chat Panel (Sidebar)**
- `Cmd+Shift+I` (macOS) or `Ctrl+Shift+I` (Windows/Linux)
- Or click chat icon in Activity Bar
- Persistent conversation panel

**Method 2: Inline Chat (In-Editor)**
- `Cmd+I` (macOS) or `Ctrl+I` (Windows/Linux)
- Chat appears directly in your code
- Quick questions without switching context

**Method 3: Quick Chat**
- `Cmd+Shift+I` then type
- Fast access for one-off questions

**Example Workflow**:
```
You: "Explain this function"
[Select code first]

Copilot: "This function implements a binary search algorithm..."

You: "How can I make it more efficient?"

Copilot: "Here are 3 optimizations: ..."
```

**Pros**:
- 💬 Interactive - can ask follow-ups
- 🧠 Intelligent - understands context
- 📚 Educational - explains concepts
- 🔧 Versatile - handles many tasks
- 🔍 Deep analysis - can review entire files

**Cons**:
- ⏱️ Slower than inline suggestions
- 💰 Higher token usage (costs more)
- 🔄 Breaks flow - need to switch context
- 📝 Requires clear prompts

---

### Decision Matrix: Which Mode to Use?

| Task | Use Inline | Use Chat | Reason |
|------|-----------|----------|--------|
| Write getter/setter | ✅ | ❌ | Standard pattern, inline is faster |
| Debug cryptic error | ❌ | ✅ | Need explanation and discussion |
| Complete function | ✅ | ❌ | Code completion is what inline does best |
| Learn new library | ❌ | ✅ | Need interactive Q&A |
| Generate test cases | ❌ | ✅ | Complex task requiring specification |
| Add simple comment | ✅ | ❌ | Inline can predict comment content |
| Refactor 5 files | ❌ | ✅ (Edits) | Multi-file = use Copilot Edits |
| Fix typo | ❌ | ❌ | Just fix it manually - too trivial |
| Implement algorithm | ✅ then ✅ | Chat to discuss, inline to implement |
| Code review | ❌ | ✅ | Needs analysis and explanation |

---

## Essential Keyboard Shortcuts

**⌨️ Master these shortcuts** for efficient AI-assisted coding.

### Quick Reference Card

**macOS** | **Windows/Linux** | **Action**
----------|-------------------|------------
`Cmd+I` | `Ctrl+I` | **Inline Chat** - Quick AI conversation in editor
`Cmd+Shift+I` | `Ctrl+Shift+I` | **Chat Panel** - Open sidebar chat
`Tab` | `Tab` | **Accept** inline suggestion
`Esc` | `Esc` | **Dismiss** inline suggestion
`Cmd+→` | `Ctrl+→` | **Accept word** from suggestion
`Alt+]` | `Alt+]` | **Next** suggestion
`Alt+[` | `Alt+[` | **Previous** suggestion
`Cmd+Shift+P` | `Ctrl+Shift+P` | **Command Palette** - Access all commands
`Cmd+K Cmd+I` | `Ctrl+K Ctrl+I` | **Hover info** - Show documentation
`Cmd+.` | `Ctrl+.` | **Quick Fix** - Show code actions

### Deep Dive: Inline Chat (`Cmd+I` / `Ctrl+I`)

**Most powerful shortcut for AI-assisted coding.**

**How it works**:
1. Place cursor or select code
2. Press `Cmd+I` (macOS) or `Ctrl+I` (Windows/Linux)
3. Inline chat box appears
4. Type request: "Add error handling" or "Explain this"
5. AI responds with code changes or explanation
6. Accept, reject, or refine

**Common Use Cases**:

```javascript
// Example 1: Generate code at cursor
// Cursor here: |
// Press Cmd+I: "Create a user validation function"
// Result: Function appears at cursor

// Example 2: Refactor selection
function messyCode() {
  // Select this entire function
  // Press Cmd+I: "Refactor to use async/await"
  // Result: Suggested changes appear
}

// Example 3: Add feature
const data = fetchData();
// Cursor at end of line
// Press Cmd+I: "Add error handling and retry logic"
// Result: Wrapped in try/catch with retry
```

**Pro Tips**:
- ✅ Select code first for context-aware suggestions
- ✅ Use natural language: "make this faster" works
- ✅ Chain requests: Accept → Cmd+I → "now add tests"
- ✅ Preview changes before accepting

---

### Deep Dive: Command Palette (`Cmd+Shift+P` / `Ctrl+Shift+P`)

**Access to ALL Copilot features and commands.**

**Key Copilot Commands**:
```
> GitHub Copilot: Enable
> GitHub Copilot: Disable
> GitHub Copilot: Toggle
> GitHub Copilot: Open Chat
> GitHub Copilot: Explain This
> GitHub Copilot: Generate Tests
> GitHub Copilot: Fix This
> GitHub Copilot: Generate Documentation
```

**Workflow Tip**:
Type `>copilot` in Command Palette to see all available commands.

---

### Customizing Shortcuts

**Change keyboard shortcuts**:
1. `Cmd+K Cmd+S` (macOS) or `Ctrl+K Ctrl+S` (Windows/Linux)
2. Search for "copilot"
3. Click shortcut to rebind

**Recommended Custom Shortcuts**:
```json
// In keybindings.json (Cmd+Shift+P > "Preferences: Open Keyboard Shortcuts (JSON)")
[
  {
    // Quick access to "Explain This"
    "key": "cmd+shift+e",
    "command": "github.copilot.explain"
  },
  {
    // Quick access to Generate Tests
    "key": "cmd+shift+t",
    "command": "github.copilot.generateTests"
  }
]
```

---

## Understanding Workspace Context

**How AI "sees" your project** - Critical for getting relevant suggestions.

### What is Workspace Context?

**Workspace context** = All the information Copilot has access to when generating suggestions.

**Includes**:
1. ✅ Current file (highest priority)
2. ✅ Open tabs (medium priority)
3. ✅ Related files (detected by imports, similar names)
4. ✅ Workspace structure (folder/file names)
5. ✅ Recently accessed files
6. ❌ File contents NOT yet opened (limited)
7. ❌ Runtime values or database data

---

### How Copilot Builds Context

**Context Priority Ranking**:
```
[Highest Priority]
1. Current cursor position → immediate surrounding code
2. Selected text → what you've highlighted
3. Current file → entire file content
4. Open tabs → other files you're viewing
5. Related files → detected via imports/references
6. Workspace metadata → file/folder names
[Lowest Priority]
```

**Practical Example**:

```
Your project:
├── src/
│   ├── auth/
│   │   ├── login.ts ← Currently editing (HIGHEST context)
│   │   ├── signup.ts ← Open in tab (MEDIUM context)
│   │   └── types.ts ← Imported in login.ts (MEDIUM context)
│   ├── utils/
│   │   └── validation.ts ← Closed, not imported (LOW context)
│   └── database/
│       └── users.ts ← Closed (VERY LOW context)

What Copilot "knows" when you're in login.ts:
✅✅✅ login.ts - everything in this file
✅✅ signup.ts - because it's in an open tab
✅✅ types.ts - because it's imported
✅ auth/ folder exists
❌ validation.ts - hasn't been opened or imported
❌ users.ts - not in context

Result: Suggestions will reflect patterns from login.ts, signup.ts, and types.ts
```

---

### Improving Context Quality

**Techniques to give Copilot better context**:

#### 1. Open Related Files
```
Bad workflow:
- Only have login.ts open
- Ask: "How do I authenticate users?"
- Copilot doesn't know your auth patterns

Good workflow:
- Open: login.ts, types.ts, middleware.ts
- Ask same question
- Copilot sees your existing patterns and matches them
```

#### 2. Use Descriptive File Names
```
Bad:
utils1.ts, helpers.ts, stuff.ts

Good:
userValidation.ts, authHelpers.ts, dateFormatting.ts

Why: Copilot uses file names to understand purpose
```

#### 3. Add Context Comments
```javascript
// At top of file - helps Copilot understand purpose
/**
 * User Authentication Module
 * Uses JWT tokens with 24-hour expiration
 * Depends on: bcrypt, jsonwebtoken
 * Database: PostgreSQL via Prisma
 */

// Now when you write code, Copilot knows:
// - To use JWT
// - To use bcrypt for passwords
// - To reference Prisma models
```

#### 4. Reference Code in Prompts
```
// In Copilot Chat:
Bad: "Create a validation function"

Good: "Create a validation function similar to the one in userValidation.ts, 
      but for email addresses"

Why: Explicitly tells Copilot to look at specific file
```

#### 5. Use Imports
```typescript
// Copilot sees imports and understands your stack
import { PrismaClient } from '@prisma/client';
import bcrypt from 'bcrypt';
import jwt from 'jsonwebtoken';

// Now Copilot will suggest Prisma queries, bcrypt hashing, JWT signing
// Because it knows these are available
```

---

### Workspace Context Limitations

**What Copilot CANNOT do** (common misconceptions):

❌ **Read closed files automatically**
```
You: "Update all files to use the new API"
Reality: Copilot can't scan all files
Solution: Use Copilot Edits mode (see next section)
```

❌ **Know runtime values**
```javascript
const userId = getUserFromDB(); // Copilot doesn't know actual value
// It can't suggest code like: "if (userId === 12345)"
```

❌ **Access environment variables**
```javascript
// Copilot doesn't know what's in your .env file
process.env.DATABASE_URL // Can't suggest actual connection string
```

❌ **Understand your business logic without context**
```
You: "Calculate commission"
Copilot: Suggests generic percentage calculation
Reality: Your business has complex tiered commission rules
Solution: Provide context in comments or chat
```

---

### Workspace Settings for Context

**Configure what files Copilot can access**:

```json
// .vscode/settings.json (project-specific)
{
  // Exclude files/folders from Copilot context
  "github.copilot.advanced": {
    "excludedFolders": [
      "node_modules",
      "dist",
      ".git",
      "build"
    ]
  }
}
```

**Why exclude folders**:
- ⚡ Faster suggestions (less to process)
- 🎯 Better suggestions (no noise from dependencies)
- 💰 Lower token usage (efficiency)

---

## Copilot Edits Mode

**Multi-file autonomous editing** - The most powerful Copilot feature for complex tasks.

### What is Copilot Edits?

**Copilot Edits** = Agent mode that can:
- Edit multiple files at once
- Coordinate changes across codebase
- Execute complex refactoring tasks
- Work autonomously toward a goal

**Think of it as**: "Copilot Chat, but it can actually modify multiple files for you"

**Official Documentation**: https://docs.github.com/en/copilot/using-github-copilot/code-editing/using-copilot-edits

---

### When to Use Copilot Edits

**Perfect for**:
- ✅ Multi-file refactoring
- ✅ Adding features across several files
- ✅ Updating APIs used in many places
- ✅ Renaming/restructuring components
- ✅ Adding consistent patterns across codebase
- ✅ Generating related files (types, tests, configs)

**NOT good for**:
- ❌ Single-file changes (use inline chat)
- ❌ Exploratory "how do I" questions (use chat)
- ❌ Quick fixes (use quick actions)
- ❌ Learning/understanding code (use chat)

---

### How to Access Copilot Edits

**Method 1: Command Palette**
```
Cmd+Shift+P (macOS) or Ctrl+Shift+P (Windows/Linux)
> "GitHub Copilot: Open Edits"
```

**Method 2: Chat Panel**
- Open Copilot Chat panel
- Click "Edits" tab at top
- Start describing your task

**Method 3: Keyboard Shortcut** (if configured)
```
Default: No shortcut
Recommended: Set your own (e.g., Cmd+Shift+E)
```

---

### Using Copilot Edits: Step-by-Step

**Example Task**: Refactor authentication to use async/await instead of promises

#### Step 1: Open Copilot Edits
- `Cmd+Shift+P` → "GitHub Copilot: Open Edits"

#### Step 2: Add Files to Working Set
```
Click "+ Add Files" or type file names:
- src/auth/login.ts
- src/auth/signup.ts
- src/auth/middleware.ts
- src/auth/logout.ts
```

**Why specify files**:
- ⚡ Faster - doesn't scan entire codebase
- 🎯 Safer - won't modify unrelated files
- 💵 Cheaper - processes less content

#### Step 3: Describe the Task
```
Clear prompt:

"Refactor all authentication functions to use async/await instead of .then()/.catch()

Requirements:
- Replace promise chains with async/await
- Add proper try/catch blocks
- Maintain existing error types
- Keep the same function signatures
- Update all 4 files in the working set"
```

#### Step 4: Review Changes
- Copilot suggests changes file by file
- Review each change in diff view
- Accept ✅, reject ❌, or ask for modifications

#### Step 5: Apply Changes
- Click "Accept All" to apply to all files
- Or accept file-by-file for more control

---

### Best Practices for Copilot Edits

#### ✅ DO: Be Specific

**Vague** ❌:
```
"Improve the authentication code"
```
**What happens**: Copilot doesn't know what to improve, makes random changes

**Specific** ✅:
```
"Refactor authentication to:
1. Use async/await instead of promises
2. Add input validation
3. Improve error messages
4. Add TypeScript types

Files: src/auth/*.ts"
```
**What happens**: Copilot has clear goals and constraints

---

#### ✅ DO: Limit Scope

**Too broad** ❌:
```
Working set: [entire src/ folder]
Task: "Modernize the codebase"
```
**Risk**: Copilot might change unrelated files

**Right-sized** ✅:
```
Working set: src/auth/ folder only
Task: "Update auth module to use new User type from types.ts"
```
**Result**: Focused, predictable changes

---

#### ✅ DO: Provide Examples

**Without example** ❌:
```
"Add error handling"
```

**With example** ✅:
```
"Add error handling like this example:

try {
  const result = await operation();
  return { success: true, data: result };
} catch (error) {
  logger.error('Operation failed:', error);
  return { success: false, error: error.message };
}

Apply this pattern to all async functions in the working set."
```

---

#### ✅ DO: Review Incrementally

**Bad workflow** ❌:
```
1. Add 20 files to working set
2. Give complex task
3. Click "Accept All" without reviewing
4. Hope for the best
```

**Good workflow** ✅:
```
1. Start with 3-5 related files
2. Give clear task
3. Review each file's changes
4. Accept changes
5. Add next batch of files
6. Repeat
```

---

#### ❌ DON'T: Let It Run Unmonitored

**Dangerous** ❌:
```
1. Start Copilot Edits
2. Go get coffee ☕
3. Come back to 50 files changed
4. No idea what changed or why
```

**Safe** ✅:
```
1. Start Copilot Edits
2. Watch changes as they're suggested
3. Interrupt if going wrong direction
4. Review each file before accepting
```

---

#### ❌ DON'T: Use for Exploration

**Wrong use** ❌:
```
Working set: Various files
Task: "How does authentication work in this app?"
```
**Problem**: Edits mode is for DOING, not LEARNING

**Right tool** ✅:
```
Use Copilot Chat instead:
"Explain how authentication works in these files..."
```

---

### Copilot Edits vs. Chat Comparison

| Feature | Copilot Chat | Copilot Edits |
|---------|--------------|---------------|
| **Purpose** | Q&A, learning, single-file edits | Multi-file modifications |
| **Modifies files** | One at a time | Multiple simultaneously |
| **Scope** | Current file or selection | Working set (multiple files) |
| **Interaction** | Conversational | Task-driven |
| **Best for** | Understanding, debugging, small changes | Refactoring, features, consistency |
| **Review** | Accept/reject one change | Review file-by-file diffs |
| **Speed** | Fast for questions | Slower but handles more |

---

## Copilot in Terminal

**AI assistance for command-line tasks** - Less known but very useful feature.

**Official Documentation**: https://docs.github.com/en/copilot/github-copilot-in-the-cli

### What is Copilot in Terminal?

**Copilot CLI** = AI that helps you with:
- Shell commands
- Git operations
- Command explanations
- Fixing command errors

**Note**: This requires **GitHub Copilot CLI** (separate installation from VS Code extension)

---

### Installation

**Prerequisites**:
- GitHub Copilot subscription
- GitHub CLI installed

**Install GitHub CLI** (if not already installed):
```bash
# macOS
brew install gh

# Windows
winget install GitHub.cli

# Linux
sudo apt install gh  # Debian/Ubuntu
```

**Install Copilot Extension for GitHub CLI**:
```bash
gh extension install github/gh-copilot
```

**Authenticate**:
```bash
gh auth login
# Follow prompts to authorize
```

---

### Using Copilot in Terminal

**Basic Commands**:

```bash
# Explain a command
gh copilot explain "git rebase -i HEAD~3"

# Suggest a command
gh copilot suggest "find all .js files modified in last 7 days"

# Git-specific suggestions
gh copilot suggest -t git "undo last commit but keep changes"

# Shell-specific suggestions
gh copilot suggest -t shell "compress folder and exclude node_modules"
```

**Interactive Mode**:
```bash
# Start interactive session
gh copilot suggest

# Then type naturally:
> "Show disk usage of largest directories"
> "Kill all node processes"
> "Create a new branch from main"
```

---

### Common Use Cases

#### 1. Complex Git Operations
```bash
# Instead of googling:
gh copilot suggest -t git "squash last 3 commits"

# Copilot suggests:
git rebase -i HEAD~3
# Then shows explanation of interactive rebase
```

#### 2. Finding Files
```bash
gh copilot suggest "find all TypeScript files that import React"

# Copilot suggests:
grep -rl "import.*React" --include="*.ts" --include="*.tsx" .
```

#### 3. System Administration
```bash
gh copilot suggest "show which process is using port 3000"

# macOS/Linux:
lsof -i :3000

# Windows:
netstat -ano | findstr :3000
```

#### 4. File Manipulation
```bash
gh copilot suggest "batch rename all .jpeg files to .jpg"

# Copilot suggests:
for file in *.jpeg; do mv "$file" "${file%.jpeg}.jpg"; done
```

---

### VS Code Terminal Integration

**Use Copilot in VS Code's integrated terminal**:

1. **Open Terminal in VS Code**
   - `` Ctrl+` `` (backtick)
   - Or: `View` → `Terminal`

2. **Use gh copilot commands** as shown above

**Pro Tip**: Create shell aliases for common commands:

```bash
# Add to ~/.zshrc or ~/.bashrc
alias gce='gh copilot explain'
alias gcs='gh copilot suggest'
alias gcsg='gh copilot suggest -t git'

# Now you can use:
gce "docker-compose up -d"
gcs "find large files"
gcsg "revert merge commit"
```

---

### Terminal Copilot Settings

**Configure in VS Code settings**:

```json
{
  // Terminal suggestions
  "terminal.integrated.suggest.enabled": true,
  
  // Terminal shell integration
  "terminal.integrated.shellIntegration.enabled": true,
  
  // Command completion
  "terminal.integrated.commandSuggestionsEnabled": true
}
```

---

## Code Actions & Quick Fixes

**AI-powered refactoring and fixes** - Accessible via lightbulb icon. 💡

### What are Code Actions?

**Code Actions** = Context-aware suggestions that appear when:
- There's an error or warning
- Code can be refactored
- Copilot detects improvement opportunities

**Visual indicator**: Yellow lightbulb 💡 in editor margin

---

### Accessing Code Actions

**Method 1: Click the Lightbulb**
- Position cursor on code with suggestion
- Click yellow/blue lightbulb icon in margin

**Method 2: Keyboard Shortcut**
- Position cursor on code
- Press `Cmd+.` (macOS) or `Ctrl+.` (Windows/Linux)

**Method 3: Right-Click**
- Right-click on code
- Select "Quick Fix..." or "Refactor..."

---

### Types of Code Actions

#### 1. Error Fixes
```typescript
// Error: Property 'name' does not exist on type '{}'
const user = {};
console.log(user.name); // ❌ Error

// Press Cmd+. on the error
// Copilot suggests:
// ✅ Add missing property
// ✅ Add type annotation
// ✅ Use optional chaining
```

#### 2. Refactoring
```javascript
// Select this code:
const x = 5;
const y = 10;
const result = x + y;
console.log(result);

// Press Cmd+.
// Copilot suggests:
// → Extract to function
// → Extract to constant
// → Convert to arrow function
```

#### 3. Import Management
```typescript
// Use undefined variable:
const app = express(); // ❌ 'express' not imported

// Press Cmd+.
// Copilot suggests:
// → Add import: import express from 'express';
// → Install package if missing
```

#### 4. Type Inference
```typescript
// Hover over function with missing types:
function add(a, b) {
  return a + b;
}

// Press Cmd+.
// Copilot suggests:
// → Infer types from usage
// → Add explicit types: function add(a: number, b: number): number
```

#### 5. Code Generation
```javascript
// Type TODO comment:
// TODO: Add validation function

// Press Cmd+.
// Copilot suggests:
// → Generate function from comment
```

---

### Copilot-Specific Actions

**Special AI-powered actions** (requires Copilot):

```javascript
// Select code, press Cmd+.
// Copilot-specific options:
// - "Copilot: Explain this"
// - "Copilot: Generate tests"
// - "Copilot: Generate documentation"
// - "Copilot: Fix with Copilot"
// - "Copilot: Optimize this"
```

**Example**:
```python
def complex_function(data):
    # Some complex logic here
    result = process(data)
    return result

# Select function → Cmd+. → "Copilot: Generate tests"
# Result: Test file created with comprehensive test cases
```

---

### Combining with IntelliSense

**Workflow**: Error → Quick Fix → IntelliSense → Copilot

```typescript
// Step 1: Type code
async function fetchUser(id) {

// Step 2: IntelliSense suggests 'fetch', 'axios', etc.
// Choose 'fetch', press Tab

async function fetchUser(id) {
  const response = await fetch

// Step 3: Copilot suggests full implementation (gray text)
async function fetchUser(id) {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
}

// Step 4: Error appears (no error handling)
// Step 5: Cmd+. → "Add try/catch"

// Final:
async function fetchUser(id) {
  try {
    const response = await fetch(`/api/users/${id}`);
    if (!response.ok) throw new Error('User not found');
    return response.json();
  } catch (error) {
    console.error('Failed to fetch user:', error);
    throw error;
  }
}
```

---

## IntelliSense + AI Synergy

**IntelliSense** (built-in) + **Copilot** (AI) = Powerful combination

### What is IntelliSense?

**IntelliSense** = VS Code's built-in code completion that provides:
- Method/property suggestions
- Parameter hints
- Quick documentation
- Type information

**Not AI-powered** - Based on:
- Language definitions
- Type declarations
- Imported libraries
- Project code analysis

---

### IntelliSense vs. Copilot

| Feature | IntelliSense | Copilot |
|---------|--------------|---------|
| **Triggers** | `.` character, `Ctrl+Space` | Automatic as you type |
| **Suggests** | Existing methods, properties | New code, complete blocks |
| **Accuracy** | 100% (from definitions) | High but not perfect |
| **Scope** | Available APIs only | Can predict any code |
| **Speed** | Instant | Very fast (slight delay) |
| **Use case** | Discover available methods | Generate implementation |

---

### The Synergy: How They Work Together

**Workflow Example**:

```typescript
// 1. Type object name and dot
user.

// 2. IntelliSense shows available properties:
// - id
// - name
// - email
// - createdAt
// (from type definition)

// 3. Select 'email' from IntelliSense
user.email

// 4. Continue typing
if (user.email

// 5. Copilot predicts rest:
if (user.email.includes('@company.com')) {
  // Grant admin access
  user.role = 'admin';
}

// 6. Accept with Tab
```

**Key insight**: 
- **IntelliSense** ensures you use correct API
- **Copilot** suggests how to use it

---

### Trigger IntelliSense Manually

Sometimes you want explicit method lists:

**Keyboard Shortcut**:
- `Ctrl+Space` - Trigger IntelliSense manually

**When to use**:
```typescript
// Explore object properties:
const user = getUser();
user. // Ctrl+Space to see all properties

// Check function parameters:
bcrypt.hash // Ctrl+Space to see: hash(data, saltRounds)

// Discover library features:
lodash. // Ctrl+Space to browse all lodash methods
```

---

### IntelliSense Settings

**Optimize for AI-assisted development**:

```json
{
  // Suggestions
  "editor.suggest.enabled": true,
  "editor.suggestOnTriggerCharacters": true,
  "editor.quickSuggestions": {
    "other": true,
    "comments": false,
    "strings": true
  },
  
  // Show inline suggestions (Copilot)
  "editor.inlineSuggest.enabled": true,
  
  // Accept suggestions with Tab
  "editor.tabCompletion": "on",
  
  // Suggestion details
  "editor.suggest.showStatusBar": true,
  "editor.suggest.preview": true,
  
  // Parameter hints
  "editor.parameterHints.enabled": true,
  
  // Hover info
  "editor.hover.enabled": true,
  "editor.hover.delay": 300
}
```

---

### Power User Workflows

#### Workflow 1: Discover → Generate
```typescript
// 1. Don't know what methods are available
const array = [1, 2, 3, 4, 5];
array. // Ctrl+Space

// 2. IntelliSense shows: map, filter, reduce, etc.
// 3. Choose 'filter'

array.filter

// 4. Copilot suggests implementation:
array.filter(n => n % 2 === 0)
```

#### Workflow 2: Type Check → Auto-Complete
```typescript
// 1. Define interface
interface User {
  id: number;
  name: string;
  email: string;
}

// 2. Create object
const newUser: User = {
  // Ctrl+Space here

// 3. IntelliSense suggests all required properties
// 4. Tab through each property
// 5. Copilot suggests values for each
}
```

#### Workflow 3: Import → Use
```typescript
// 1. Import library
import axios from 'axios';

// 2. Type 'axios.'
axios.

// 3. IntelliSense shows: get, post, put, delete, etc.
// 4. Select 'get'

axios.get

// 5. Copilot completes:
axios.get('https://api.example.com/users')
  .then(response => response.data)
  .catch(error => console.error(error));
```

---

## Configuration Best Practices

### Recommended settings.json

**Global Settings** (User level):

File: `~/.vscode/settings.json` or `Cmd+,` → Open Settings (JSON)

```json
{
  // ============================================
  // GITHUB COPILOT SETTINGS
  // ============================================
  
  "github.copilot.enable": {
    "*": true,
    "yaml": false,
    "plaintext": false,
    "markdown": true
  },
  
  "github.copilot.editor.enableAutoCompletions": true,
  
  // ============================================
  // EDITOR SETTINGS (for AI)
  // ============================================
  
  "editor.inlineSuggest.enabled": true,
  "editor.suggestOnTriggerCharacters": true,
  "editor.quickSuggestions": {
    "other": true,
    "comments": false,
    "strings": true
  },
  "editor.tabCompletion": "on",
  "editor.suggest.preview": true,
  "editor.parameterHints.enabled": true,
  
  // ============================================
  // PERFORMANCE
  // ============================================
  
  "files.exclude": {
    "**/.git": true,
    "**/.DS_Store": true,
    "**/node_modules": true,
    "**/dist": true,
    "**/build": true
  },
  
  // ============================================
  // QUALITY OF LIFE
  // ============================================
  
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.organizeImports": true,
    "source.fixAll": true
  },
  
  "editor.minimap.enabled": true,
  "editor.wordWrap": "on",
  "editor.rulers": [80, 120],
  
  // ============================================
  // TERMINAL
  // ============================================
  
  "terminal.integrated.suggest.enabled": true,
  "terminal.integrated.shellIntegration.enabled": true
}
```

---

### Project-Specific Settings

**Workspace Settings** (project level):

File: `.vscode/settings.json` (in project root)

```json
{
  // Example: React/TypeScript project
  
  "github.copilot.enable": {
    "*": true,
    "test": true
  },
  
  // Exclude generated files from Copilot context
  "github.copilot.advanced": {
    "excludedFolders": [
      "node_modules",
      "dist",
      "build",
      ".next",
      "coverage"
    ]
  },
  
  // TypeScript-specific
  "typescript.suggest.autoImports": true,
  "typescript.updateImportsOnFileMove.enabled": "always",
  
  // React-specific
  "emmet.includeLanguages": {
    "javascript": "javascriptreact",
    "typescript": "typescriptreact"
  }
}
```

---

### Privacy Settings

**Configure data sharing**:

```json
{
  // Disable telemetry (privacy)
  "telemetry.telemetryLevel": "off",
  
  // Copilot telemetry
  "github.copilot.advanced": {
    "telemetry": "disabled"
  },
  
  // Exclude sensitive files
  "files.exclude": {
    "**/.env": true,
    "**/.env.local": true,
    "**/secrets": true
  }
}
```

**Content Exclusions** (Enterprise):

Create `.copilotignore` file (similar to `.gitignore`):

```
# .copilotignore
# Files to exclude from Copilot

.env
.env.*
secrets/
config/production.yml
src/legacy/proprietary/
```

---

### Extension Recommendations

**Recommended extensions** to install alongside Copilot:

```json
// .vscode/extensions.json
{
  "recommendations": [
    // Core AI
    "github.copilot",
    "github.copilot-chat",
    
    // Languages
    "ms-python.python",
    "ms-vscode.vscode-typescript-next",
    
    // Formatting
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    
    // Git
    "eamodio.gitlens",
    
    // Quality of Life
    "usernamehw.errorlens",
    "christian-kohler.path-intellisense",
    "formulahendry.auto-rename-tag"
  ]
}
```

---

## Troubleshooting

### Common Issues and Solutions

#### Issue 1: No Inline Suggestions

**Symptoms**: Copilot installed but no gray text appears

**Checklist**:
1. ✅ Check Copilot is enabled
   - Look for checkmark in status bar (bottom-right)
   - Click icon to toggle
   
2. ✅ Verify subscription is active
   - Visit: https://github.com/settings/copilot
   - Check subscription status
   
3. ✅ Check file type is supported
   ```json
   "github.copilot.enable": {
     "*": true  // Make sure this is true
   }
   ```
   
4. ✅ Restart VS Code
   - `Cmd+Q` (macOS) or close and reopen
   
5. ✅ Check internet connection
   - Copilot requires active internet
   
6. ✅ Sign out and sign in again
   - `Cmd+Shift+P` → "GitHub Copilot: Sign Out"
   - Then sign in again

---

#### Issue 2: Suggestions Are Low Quality

**Symptoms**: Copilot suggests irrelevant or wrong code

**Solutions**:

1. **Improve context**
   - Open related files in tabs
   - Add descriptive comments
   - Use better variable names

2. **Clear cache**
   ```bash
   # Close VS Code
   # macOS:
   rm -rf ~/Library/Application\ Support/Code/CachedExtensionVSIXs/github.copilot*
   
   # Windows:
   del %APPDATA%\Code\CachedExtensionVSIXs\github.copilot*
   
   # Linux:
   rm -rf ~/.config/Code/CachedExtensionVSIXs/github.copilot*
   
   # Reopen VS Code
   ```

3. **Be more explicit**
   ```javascript
   // Vague:
   // user function
   
   // Better:
   // Fetches user by ID from PostgreSQL database
   // Returns User object or null if not found
   // Throws error if database connection fails
   async function getUserById(id: number): Promise<User | null> {
   ```

---

#### Issue 3: Chat Not Responding

**Symptoms**: Chat panel frozen or not answering

**Solutions**:

1. **Check output panel**
   - `Cmd+Shift+U` → Select "GitHub Copilot" from dropdown
   - Look for errors

2. **Restart Language Server**
   - `Cmd+Shift+P` → "GitHub Copilot: Restart Language Server"

3. **Clear chat history**
   - Click "..." in chat panel → "Clear Chat"

4. **Check API status**
   - Visit: https://www.githubstatus.com/
   - Check if Copilot services are up

---

#### Issue 4: High CPU/Memory Usage

**Symptoms**: VS Code slow, fan spinning, memory high

**Solutions**:

1. **Exclude large folders**
   ```json
   "files.exclude": {
     "**/node_modules": true,
     "**/dist": true,
     "**/.git": true
   }
   ```

2. **Limit open tabs**
   - Close unused tabs
   - Each open file adds to context

3. **Disable minimap** (optional)
   ```json
   "editor.minimap.enabled": false
   ```

4. **Check other extensions**
   - Disable extensions temporarily
   - Identify if conflict exists

---

#### Issue 5: Copilot Not Installed/Missing

**Symptoms**: Can't find Copilot in extensions

**Solutions**:

1. **Check VS Code version**
   - Copilot requires VS Code 1.73 or later
   - Update VS Code to latest

2. **Install manually**
   ```bash
   code --install-extension GitHub.copilot
   code --install-extension GitHub.copilot-chat
   ```

3. **Check extension marketplace**
   - Some regions have restricted access
   - Use VPN if necessary (check GitHub ToS)

---

## Hands-On Exercises

### Exercise 1: Install and Verify

**Objective**: Complete setup and verify all features work

**Tasks**:
- [ ] Install GitHub Copilot extension
- [ ] Install GitHub Copilot Chat extension
- [ ] Sign in to GitHub
- [ ] Verify inline suggestions work
- [ ] Verify chat panel works
- [ ] Verify inline chat (`Cmd+I`) works

**Test**:
```javascript
// test.js
// Create a function to calculate factorial

// Expected: Copilot suggests implementation
// Press Tab to accept
```

---

### Exercise 2: Master Keyboard Shortcuts

**Objective**: Build muscle memory for essential shortcuts

**Tasks** (do each 5 times):
- [ ] Open inline chat: `Cmd+I` / `Ctrl+I`
- [ ] Open chat panel: `Cmd+Shift+I` / `Ctrl+Shift+I`
- [ ] Accept suggestion: `Tab`
- [ ] Dismiss suggestion: `Esc`
- [ ] Next suggestion: `Alt+]`
- [ ] Code action: `Cmd+.` / `Ctrl+.`

**Practice file**: Create simple functions and use shortcuts to modify them

---

### Exercise 3: Context Experiment

**Objective**: Understand how context affects suggestions

**Setup**:
```
Create two projects:
1. Project A: Single file, no imports
2. Project B: Multiple files, imports, descriptive names
```

**Task**:
Ask Copilot to "create a user validation function" in both projects

**Compare**:
- Quality of suggestions
- Relevance to project
- Understanding of patterns

**Learning**: Better context = better suggestions

---

### Exercise 4: Copilot Edits Practice

**Objective**: Use Copilot Edits for multi-file refactoring

**Setup**:
```
Create simple project:
- auth/login.js
- auth/signup.js
- auth/logout.js
(All using promises)
```

**Task**:
Use Copilot Edits to refactor all to async/await

**Steps**:
1. Open Copilot Edits
2. Add all auth/*.js files
3. Describe refactoring task
4. Review changes
5. Accept

---

### Exercise 5: Chat vs. Inline vs. Edits

**Objective**: Learn when to use which mode

**Scenarios** - Choose the right mode:

1. "Explain how this sorting algorithm works"
   - Answer: ___________
   
2. "Add try/catch to this function"
   - Answer: ___________
   
3. "Refactor 5 files to use new API"
   - Answer: ___________
   
4. "What's the syntax for async/await in Python?"
   - Answer: ___________

**Answers**:
1. Chat (explanation)
2. Inline chat (single modification)
3. Copilot Edits (multi-file)
4. Chat (question)

---

## Completion Checklist

### Installation & Configuration
- [ ] VS Code installed (latest version)
- [ ] GitHub Copilot extension installed
- [ ] GitHub Copilot Chat extension installed
- [ ] Signed in to GitHub account
- [ ] Subscription verified as active
- [ ] Basic settings configured

### Features Understanding
- [ ] Understand difference between inline suggestions and chat
- [ ] Know how to access Copilot Chat
- [ ] Can use inline chat (`Cmd+I` / `Ctrl+I`)
- [ ] Understand workspace context concept
- [ ] Know when to use Copilot Edits
- [ ] Familiar with code actions (`Cmd+.` / `Ctrl+.`)

### Keyboard Shortcuts
- [ ] `Cmd/Ctrl+I` - Inline chat (muscle memory)
- [ ] `Cmd/Ctrl+Shift+I` - Chat panel (muscle memory)
- [ ] `Tab` - Accept suggestion (muscle memory)
- [ ] `Alt+]` / `Alt+[` - Cycle suggestions
- [ ] `Cmd/Ctrl+.` - Code actions

### Practical Skills
- [ ] Can generate code from comments
- [ ] Can ask Copilot to explain code
- [ ] Can use Copilot for refactoring
- [ ] Can review and accept/reject suggestions
- [ ] Know how to improve suggestion quality

### Best Practices
- [ ] Always review AI-generated code
- [ ] Understand context limitations
- [ ] Know when to use each mode (chat/inline/edits)
- [ ] Configured privacy settings appropriately

---

## Next Steps

Now that your VS Code is set up, you're ready to:

1. **Learn Privacy & Security Basics** (Phase 1, next section)
2. **Master Prompt Engineering** (Phase 2)
3. **Practice Daily** with the workflows you've learned

---

## Reference Links

### Official Documentation
- **GitHub Copilot Docs**: https://docs.github.com/en/copilot
- **VS Code Docs**: https://code.visualstudio.com/docs
- **Copilot Getting Started**: https://docs.github.com/en/copilot/using-github-copilot/getting-started-with-github-copilot
- **Copilot Subscription**: https://github.com/settings/copilot

### VS Code Resources
- **Keyboard Shortcuts Reference**: https://code.visualstudio.com/docs/getstarted/keybindings
- **Settings Reference**: https://code.visualstudio.com/docs/getstarted/settings
- **IntelliSense Guide**: https://code.visualstudio.com/docs/editor/intellisense

### Extension Links
- **GitHub Copilot**: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot
- **GitHub Copilot Chat**: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat

### Community
- **GitHub Copilot Discussions**: https://github.com/orgs/community/discussions/categories/copilot
- **VS Code Community**: https://code.visualstudio.com/community

---

*Last updated: March 2026*
*Tested on: VS Code 1.85+, GitHub Copilot 1.150+*
