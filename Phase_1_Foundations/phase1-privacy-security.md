# Privacy & Security Basics for AI-Assisted Development

## Table of Contents
1. [Understanding Data Flow](#understanding-data-flow)
2. [What Data is Sent to AI Services](#what-data-is-sent-to-ai-services)
3. [What NOT to Share](#what-not-to-share)
4. [Configuring Content Exclusions](#configuring-content-exclusions)
5. [Account Types: Individual vs. Business vs. Enterprise](#account-types-individual-vs-business-vs-enterprise)
6. [Privacy Best Practices](#privacy-best-practices)
7. [Security Guidelines](#security-guidelines)
8. [Compliance Considerations](#compliance-considerations)
9. [Incident Response](#incident-response)
10. [Real-World Scenarios](#real-world-scenarios)
11. [Practical Exercises](#practical-exercises)

---

## Understanding Data Flow

### How GitHub Copilot Works (Data Flow)

**The Journey of Your Code**:

```
┌─────────────────┐
│   Your Code     │
│   in VS Code    │
└────────┬────────┘
         │
         │ 1. You type or trigger Copilot
         ▼
┌─────────────────┐
│  Copilot        │
│  Extension      │
│  (Local)        │
└────────┬────────┘
         │
         │ 2. Extension sends context to GitHub
         ▼
┌─────────────────┐
│  GitHub         │ ◄── Transmitted over HTTPS (encrypted)
│  Servers        │
└────────┬────────┘
         │
         │ 3. GitHub sends to OpenAI
         ▼
┌─────────────────┐
│  OpenAI API     │ ◄── Also encrypted
│  (GPT models)   │
└────────┬────────┘
         │
         │ 4. AI generates suggestion
         ▼
┌─────────────────┐
│  Response       │
│  sent back      │
│  to VS Code     │
└─────────────────┘
```

**Key Points**:
- ✅ **All transmission is encrypted** (HTTPS/TLS)
- ✅ **No data stored on your machine** by Copilot (beyond VS Code cache)
- ⚠️ **Data is sent to external servers** (GitHub + OpenAI)
- ⚠️ **Some data may be retained** for service improvement

---

### What Happens to Your Code?

**GitHub Copilot's Data Handling** (as of 2026):

| Data Type | Sent to Service? | Stored? | Used for Training? |
|-----------|-----------------|---------|-------------------|
| Code snippets (context) | ✅ Yes | Temporarily | ❌ No (for Individual/Business)* |
| Your prompts/questions | ✅ Yes | Temporarily | ❌ No (for Individual/Business)* |
| File names | ✅ Yes | Temporarily | ❌ No (for Individual/Business)* |
| Generated suggestions | ✅ Yes | Temporarily | ❌ No (for Individual/Business)* |
| Telemetry data | ✅ Yes | ✅ Yes | ✅ May be used for improvement |
| Error logs | ✅ Yes | ✅ Yes | ✅ May be used for debugging |

**Important Note**: 
- Individual and Business accounts have opted out of training by default (as of 2023+)
- Enterprise accounts have strict data isolation
- Check current policy: https://docs.github.com/en/site-policy/privacy-policies/github-copilot-privacy-statement

---

### Official Privacy Policies

**Must-Read Resources**:

- **GitHub Copilot Privacy Statement**: https://docs.github.com/en/site-policy/privacy-policies/github-copilot-privacy-statement
- **GitHub Privacy Statement**: https://docs.github.com/en/site-policy/privacy-policies/github-privacy-statement
- **OpenAI Data Usage Policy**: https://openai.com/policies/usage-policies
- **Microsoft Privacy Statement**: https://privacy.microsoft.com/privacystatement

**⚠️ CRITICAL**: These policies change over time. Review them annually or when renewing subscriptions.

---

## What Data is Sent to AI Services

### Automatic Context Collection

**When you use Copilot, the following is AUTOMATICALLY sent**:

#### 1. Current File Content
```javascript
// EVERYTHING in this file gets sent when you request suggestions
const API_KEY = "sk-1234567890"; // ⚠️ EXPOSED TO AI!
const DATABASE_URL = "postgres://user:pass@localhost/db"; // ⚠️ EXPOSED TO AI!

function processPayment(creditCard) { // ⚠️ EXPOSED TO AI!
  // ...
}
```

**Scope**: Entire current file or significant portions (depends on context window size)

---

#### 2. Open Tabs Content
```
You have these files open:
- auth.js (contains API keys) ⚠️
- config.js (contains DB credentials) ⚠️
- userService.js ⚠️

All three files may be sent as context!
```

---

#### 3. Related Files (Detected Automatically)
```typescript
// main.ts
import { User } from './models/User'; // Copilot may fetch User.ts
import { config } from './config'; // Copilot may fetch config.ts

// Both imported files can be sent as context
```

---

#### 4. File and Folder Names
```
project/
├── src/
│   ├── auth/
│   │   ├── secretAuthLogic.ts ← File name visible
│   │   └── internal_only.ts ← File name visible
│   └── proprietary/
│       └── patentedAlgorithm.ts ← File name visible

File/folder names are visible to the AI service!
```

---

#### 5. Comments and Documentation
```python
# This connects to our production database at 10.0.1.50
# Using credentials from AWS Secrets Manager: prod-db-creds
# ⚠️ All this metadata is sent!

def connect_to_db():
    # Company-specific implementation details exposed
    pass
```

---

#### 6. Git Repository Information
- Repository name (if in a git repo)
- Current branch name
- Commit messages (potentially)
- Remote URLs (potentially)

```bash
# Your repo info might be visible:
# github.com/yourcompany/secret-project
# Branch: feature/new-payment-system
```

---

#### 7. Prompts and Questions
```
You type in Copilot Chat:
"How do I connect to our internal Oracle database at 
db.internal.company.com using the admin credentials?"

⚠️ This entire question is sent and may be logged!
```

---

### What is NOT Sent

**The following stays local**:

✅ **Files not opened** in VS Code
✅ **Environment variables** (unless you paste them)
✅ **Actual runtime values** of variables
✅ **Database contents** (unless you paste them)
✅ **Files in `.gitignore`** (usually excluded, but not guaranteed)
✅ **Binary files** (images, PDFs, compiled code)

---

## What NOT to Share

### The Golden Rule

**🛑 NEVER share anything with AI that you wouldn't share with:**
- A contractor you just hired
- A junior developer on day 1
- The internet (because it might be retained)

---

### Category 1: Credentials & Secrets

**❌ NEVER share**:

```javascript
// ❌ API Keys
const OPENAI_KEY = "sk-1234567890abcdef";
const STRIPE_SECRET = "sk_live_51234567890";
const AWS_ACCESS_KEY = "AKIAI1234567890";

// ❌ Database Credentials
const DB_PASSWORD = "SuperSecret123!";
const CONNECTION_STRING = "mongodb://admin:password@prod-db.company.com";

// ❌ Authentication Tokens
const JWT_SECRET = "my-super-secret-jwt-key";
const SESSION_SECRET = "session-encryption-key-2024";

// ❌ OAuth Secrets
const GITHUB_CLIENT_SECRET = "abcdef1234567890";
const GOOGLE_CLIENT_SECRET = "xyz123-secret";

// ❌ Encryption Keys
const ENCRYPTION_KEY = "32-byte-encryption-key-here!!";
const PRIVATE_KEY = "-----BEGIN RSA PRIVATE KEY-----...";

// ❌ Passwords (even hashed)
const ADMIN_PASSWORD_HASH = "hashed-password-here";

// ❌ SSH Keys
const SSH_PRIVATE_KEY = "ssh-rsa AAAAB3...";
```

**✅ Safe alternative**:
```javascript
// ✅ Use environment variables (reference only, don't paste values)
const OPENAI_KEY = process.env.OPENAI_KEY;
const DB_PASSWORD = process.env.DB_PASSWORD;

// ✅ Reference secrets managers
const credentials = await secretsManager.getSecret('prod-db-creds');

// ✅ Use placeholder in prompts
"Show me how to connect using an API key stored in process.env.API_KEY"
```

---

### Category 2: Personally Identifiable Information (PII)

**❌ NEVER share**:

```javascript
// ❌ Real user data
const users = [
  {
    name: "John Smith",
    email: "john.smith@email.com",
    ssn: "123-45-6789",
    creditCard: "4532-1234-5678-9010",
    address: "123 Main St, Anytown, USA"
  }
];

// ❌ Customer information
const customerId = "CUST-12345"; // Real customer ID
const phoneNumber = "+1-555-123-4567"; // Real phone

// ❌ Employee data
const employees = [
  { name: "Jane Doe", salary: 95000, ssn: "987-65-4321" }
];

// ❌ Medical/Health data (HIPAA)
const patientRecord = {
  name: "Patient Name",
  diagnosis: "...",
  medications: ["..."]
};

// ❌ Financial records
const transactions = [
  { account: "ACC123456", amount: 1500.00, user: "Real Name" }
];
```

**✅ Safe alternative**:
```javascript
// ✅ Use fake/anonymized data
const users = [
  {
    name: "TestUser1",
    email: "user1@example.com",
    ssn: "XXX-XX-XXXX", // Masked
    creditCard: "XXXX-XXXX-XXXX-XXXX", // Masked
    address: "Test Address"
  }
];

// ✅ Use IDs without context
const userId = "user_123"; // No real user info

// ✅ Describe structure without real data
"I have a user object with name, email, and address fields. 
How do I validate the email?"
```

---

### Category 3: Proprietary Code & Business Logic

**❌ NEVER share**:

```javascript
// ❌ Patented algorithms
function proprietaryCompressionAlgorithm(data) {
  // Our company's patented method
  // Patent #US1234567
  // ...secret implementation...
}

// ❌ Trade secrets
const PRICING_ALGORITHM = {
  // Complex pricing logic that gives us competitive advantage
  calculateDiscount: (user) => {
    // Secret formula based on years of market research
  }
};

// ❌ Unreleased features
// TODO: New payment system launching Q3 2026
// This will be our competitive advantage
function newPaymentFlow() {
  // ...
}

// ❌ Security mechanisms
function antiCheatDetection() {
  // Our proprietary cheat detection
  // If competitors knew this, they could bypass it
}

// ❌ Internal infrastructure details
// Connects to internal-firewall-bypass.company.local
// Using special port 9999 (not documented publicly)
```

**✅ Safe alternative**:
```javascript
// ✅ Generic implementation
function compressData(data) {
  // Standard compression approach
  // Ask AI about generic compression, implement secret sauce manually
}

// ✅ Abstract the secret parts
"I need to calculate a price based on user attributes. 
Show me a general structure for a pricing calculator."
// Then implement your secret formula manually

// ✅ Describe without revealing
"I have a function that detects unusual patterns. 
How do I structure it for performance?"
// Don't reveal what patterns or how you detect them
```

---

### Category 4: Internal Infrastructure

**❌ NEVER share**:

```javascript
// ❌ Internal IP addresses
const DB_HOST = "10.0.1.50"; // Internal network
const API_ENDPOINT = "https://internal-api.company.local";

// ❌ Network topology
// prod-cluster-1: 10.0.1.0/24
// staging-cluster: 10.0.2.0/24
// admin-network: 10.0.0.0/24 (VPN only)

// ❌ Server configurations
const servers = {
  production: ["prod-web-1.internal", "prod-web-2.internal"],
  database: ["prod-db-master.internal", "prod-db-replica.internal"]
};

// ❌ Security configurations
// Firewall rules:
// - Block all except 10.0.0.0/8
// - Allow SSH only from 10.0.0.5

// ❌ Deployment details
// Deploy via internal Jenkins: https://ci.internal.company.com
// SSH to deploy@prod-server-1.internal
```

**✅ Safe alternative**:
```javascript
// ✅ Use placeholders
const DB_HOST = process.env.DB_HOST || "localhost";
const API_ENDPOINT = process.env.API_ENDPOINT;

// ✅ Generic descriptions
"I need to connect to a database server. Show me connection pooling."
// Don't mention it's your production database at a specific IP

// ✅ Abstract architecture questions
"How do I design a multi-tier application with load balancing?"
// Don't reveal your actual infrastructure
```

---

### Category 5: Compliance & Legal

**❌ NEVER share**:

```javascript
// ❌ GDPR-protected data
const euCustomers = [...]; // Real EU customer data

// ❌ HIPAA-protected data
const medicalRecords = [...]; // Real patient data

// ❌ PCI-compliant data
const creditCardNumbers = [...]; // Even encrypted

// ❌ Legal documents code
// Contract parsing logic for our legal department
// Contains confidential legal strategies

// ❌ SOC2/compliance-critical code
// Audit logging mechanism - if exposed, could be bypassed
```

**✅ Safe alternative**:
```javascript
// ✅ Describe requirements abstractly
"I need to implement GDPR-compliant data deletion. 
Show me a general approach to cascading deletes."

// ✅ Use compliance frameworks
"How do I structure logging for SOC2 compliance?"
// Don't reveal your actual logging implementation
```

---

## Configuring Content Exclusions

### Method 1: Global Settings (User Level)

**Exclude files from Copilot suggestions globally**:

File: `~/.vscode/settings.json` or `Library/Application Support/Code/User/settings.json`

```json
{
  "files.exclude": {
    "**/.env": true,
    "**/.env.*": true,
    "**/secrets/": true,
    "**/config/production.yml": true
  },
  
  "github.copilot.advanced": {
    "excludedFolders": [
      "**/secrets",
      "**/credentials",
      "**/.env*",
      "**/private"
    ]
  }
}
```

---

### Method 2: Workspace Settings (Project Level)

**Better for team sharing**:

File: `.vscode/settings.json` (in project root)

```json
{
  "github.copilot.advanced": {
    "excludedFolders": [
      "config/sensitive",
      "keys",
      "certificates",
      "internal",
      "proprietary"
    ]
  },
  
  "files.exclude": {
    "**/.env": true,
    "**/.env.local": true,
    "**/.env.production": true,
    "**/secrets.yml": true,
    "**/credentials.json": true
  }
}
```

**⚠️ Important**: Add `.vscode/settings.json` to version control so the team shares these exclusions!

---

### Method 3: .gitignore Integration

**Copilot respects .gitignore** (usually):

```bash
# .gitignore
.env
.env.*
secrets/
config/production.*
credentials/
*.key
*.pem
serviceAccount.json

# Sensitive data
/data/customer-exports/
/backups/production/

# Internal documentation
/internal-docs/
```

**Note**: While Copilot generally respects `.gitignore`, it's not 100% guaranteed. Use additional methods for critical files.

---

### Method 4: Content Filtering (Enterprise)

**Enterprise accounts can configure content filters**:

1. **GitHub Enterprise Settings** → **Copilot** → **Content Exclusions**
2. Add regex patterns for sensitive content:
   ```regex
   # Exclude files matching these patterns:
   .*\.key$
   .*\.pem$
   .*/secrets/.*
   .*/credentials/.*
   .*production.*\.yml$
   ```

3. Configure organization-wide policies

**🔗 Enterprise Documentation**: https://docs.github.com/en/enterprise-cloud@latest/copilot/managing-copilot-business/managing-policies-and-features-for-copilot-business

---

### Method 5: Pre-commit Hooks

**Prevent secrets from being committed** (which also prevents Copilot access):

```bash
# Install git-secrets
brew install git-secrets

# Initialize in repo
git secrets --install

# Add patterns
git secrets --add 'password\s*=\s*.+'
git secrets --add 'api[_-]?key\s*=\s*.+'
git secrets --add '[A-Za-z0-9+/]{40}' # Base64 secrets

# Scan existing repo
git secrets --scan-history
```

**Alternative tools**:
- **gitleaks**: https://github.com/gitleaks/gitleaks
- **trufflehog**: https://github.com/trufflesecurity/trufflehog
- **detect-secrets**: https://github.com/Yelp/detect-secrets

---

### Method 6: .copilotignore (Experimental)

**Some environments support `.copilotignore`** (similar to `.gitignore`):

```bash
# .copilotignore
# Exclude from Copilot context

# Credentials
.env*
secrets/
credentials/
*.key
*.pem

# Proprietary
proprietary/
internal/
trade-secrets/

# Production configs
config/production.*
config/*.prod.*

# Customer data
data/customers/
exports/
backups/production/
```

**⚠️ Note**: Check if your Copilot version supports this feature.

---

### Verification: Check What's Excluded

**Test your exclusions**:

1. **Open a sensitive file** (e.g., `.env`)
2. **Try to get Copilot suggestions**
3. **Verify no suggestions appear** or only generic ones

**Manual test**:
```bash
# Create test file
echo "API_KEY=secret123" > .env.test

# Open in VS Code
code .env.test

# Try to get suggestions - should be limited/none
```

---

## Account Types: Individual vs. Business vs. Enterprise

### Comparison Table

| Feature | Individual | Business | Enterprise |
|---------|-----------|----------|------------|
| **Price** | $10/month | $19/user/month | Custom pricing |
| **Data used for training** | ❌ No | ❌ No | ❌ No |
| **Data retention** | Temporary* | Temporary* | Configurable |
| **IP indemnity** | ❌ No | ✅ Yes | ✅ Yes |
| **Content exclusions** | Basic | Advanced | Full control |
| **Policy management** | User-level | Org-level | Org + repo-level |
| **Audit logs** | ❌ No | ✅ Basic | ✅ Comprehensive |
| **SSO/SAML** | ❌ No | ❌ No | ✅ Yes |
| **Data isolation** | Shared infra | Shared infra | ✅ Isolated |
| **Custom retention** | ❌ No | ❌ No | ✅ Yes |
| **Support** | Community | Email | Dedicated |
| **VPN/Private network** | ❌ No | ❌ No | ✅ Available |

*Temporary = Data used for request processing, deleted afterward (not for training)

---

### Individual Account

**Best for**: 
- Personal projects
- Open source development
- Learning and experimentation
- Side projects

**Privacy Level**: ⭐⭐⭐ Moderate

**Characteristics**:
- Code snippets sent to GitHub/OpenAI servers
- Not used for training (as of 2023+)
- Minimal audit capabilities
- No legal indemnity

**Recommended Use**:
✅ Personal projects
✅ Open source code
✅ Learning exercises
❌ Client work (without approval)
❌ Proprietary code
❌ Regulated industries

---

### Business Account

**Best for**:
- Small to medium teams
- Startup companies
- Teams without strict compliance requirements
- Commercial software development

**Privacy Level**: ⭐⭐⭐⭐ Good

**Characteristics**:
- Organization-level policy management
- Not used for training (guaranteed)
- IP indemnification (legal protection)
- Organization admin controls
- Basic audit logs

**Key Features**:
```
Organization Admin Can:
- Enable/disable Copilot for users
- Set content exclusion policies
- View usage analytics
- Manage billing
- Configure basic policies
```

**Recommended Use**:
✅ Commercial development
✅ Team collaboration
✅ Client work (with consent)
✅ Most business scenarios
⚠️ Regulated industries (with additional controls)

---

### Enterprise Account

**Best for**:
- Large corporations
- Regulated industries (finance, healthcare, government)
- Companies with strict compliance requirements
- Organizations requiring data sovereignty

**Privacy Level**: ⭐⭐⭐⭐⭐ Excellent

**Characteristics**:
- **Data isolation** (your data never mixed with others)
- **Custom data retention policies**
- **Advanced content exclusions** (repository-level)
- **Comprehensive audit logs**
- **SSO/SAML integration**
- **Private networking** options
- **Custom compliance** (SOC2, HIPAA, GDPR, etc.)

**Key Features**:
```
Enterprise Admins Can:
- Define repository-level access policies
- Configure custom data retention (or deletion)
- Require approval for certain actions
- Integrate with DLP (Data Loss Prevention)
- Set up private Copilot instances
- Control model versions
- Generate compliance reports
```

**Recommended Use**:
✅ Financial services (PCI-DSS)
✅ Healthcare (HIPAA)
✅ Government (FedRAMP)
✅ Large enterprises
✅ Any regulated industry
✅ Companies with strict IP requirements

---

### Choosing the Right Account Type

**Decision Tree**:

```
Do you work with regulated data (HIPAA, PCI-DSS, GDPR)?
├─ YES → Enterprise Account Required
└─ NO ↓

Is this for commercial/client work?
├─ YES ↓
│   └─ Is IP protection important?
│       ├─ YES → Business or Enterprise
│       └─ NO → Business (minimum)
└─ NO ↓

Is this personal/open source?
├─ YES → Individual Account OK
└─ NO → Business (recommended)
```

---

### Upgrading Account Type

**Individual → Business**:
1. Visit: https://github.com/settings/copilot
2. Click "Upgrade to Business"
3. Provide organization/billing info
4. Enable for team members

**Business → Enterprise**:
1. Contact GitHub Sales: https://github.com/enterprise/contact
2. Discuss requirements and pricing
3. Set up enterprise agreement
4. Configure policies and compliance

---

## Privacy Best Practices

### 1. Use Environment Variables

**❌ Never**:
```javascript
const apiKey = "sk-1234567890abcdef";
```

**✅ Always**:
```javascript
const apiKey = process.env.API_KEY;
```

**Setup**:
```bash
# .env (add to .gitignore!)
API_KEY=sk-1234567890abcdef
DATABASE_URL=postgres://user:pass@localhost/db

# Load in app
require('dotenv').config();
```

---

### 2. Redact Sensitive Data Before Sharing

**When asking Copilot Chat**:

**❌ Bad**:
```
"Here's my code:
const dbUrl = 'postgres://admin:SuperSecret123@prod-db.company.com:5432/proddb'
How do I improve the connection?"
```

**✅ Good**:
```
"Here's my code:
const dbUrl = process.env.DATABASE_URL
How do I improve the connection with retry logic?"
```

---

### 3. Create a "Secrets Checklist"

**Before committing code, check**:

- [ ] No hardcoded passwords
- [ ] No API keys in source
- [ ] No real customer data
- [ ] No internal IP addresses
- [ ] No production URLs
- [ ] All secrets in environment variables
- [ ] `.env` is in `.gitignore`
- [ ] No credentials in comments

---

### 4. Review Copilot Suggestions Carefully

**Copilot might suggest**:
```javascript
// Copilot suggestion:
const API_KEY = "your-api-key-here"; // ⚠️ Don't accept this!

// Better: Reject and write:
const API_KEY = process.env.API_KEY;
```

---

### 5. Separate Sensitive Code

**Project structure**:
```
project/
├── src/
│   ├── public/          ← Generic code, safe for AI
│   ├── core/            ← Business logic, safe for AI
│   └── sensitive/       ← Excluded from Copilot
│       └── proprietary.ts
├── config/
│   ├── default.yml      ← Safe defaults
│   └── production.yml   ← Excluded from Copilot
└── .env                 ← Excluded from Copilot
```

---

### 6. Use Synthetic/Mock Data

**For demonstrations and testing**:

```javascript
// ❌ Real data
const testUser = {
  name: "John Smith",
  email: "john.smith@realcompany.com",
  ssn: "123-45-6789"
};

// ✅ Synthetic data
const testUser = {
  name: "Test User",
  email: "testuser@example.com",
  ssn: "XXX-XX-XXXX"
};
```

---

### 7. Regular Audits

**Monthly checklist**:

- [ ] Review GitHub Copilot privacy policy for changes
- [ ] Check excluded folders are still excluded
- [ ] Scan repository for accidentally committed secrets
- [ ] Review access logs (Enterprise)
- [ ] Update team privacy training
- [ ] Verify `.gitignore` is comprehensive

---

### 8. Team Training

**Ensure your team knows**:
- What data Copilot can access
- What NOT to share with AI
- How to configure exclusions
- Incident reporting process
- Compliance requirements

**Training template**:
```
1. Watch privacy overview (15 min)
2. Complete "What Not to Share" quiz
3. Configure your VS Code exclusions
4. Sign privacy acknowledgment
5. Review quarterly
```

---

## Security Guidelines

### Principle of Least Exposure

**Only expose what's necessary**:

```javascript
// When asking Copilot:

// Instead of sharing entire file:
"Here's my 500-line file with authentication logic..."

// Share only relevant snippet:
"I have a function that validates JWT tokens:
function validateToken(token) { ... }
How do I improve error handling?"
```

---

### Defense in Depth

**Multiple layers of protection**:

1. **Prevention** (stop secrets from being in code)
   - Use environment variables
   - Secrets managers (AWS Secrets Manager, HashiCorp Vault)
   - Git hooks to prevent commits

2. **Detection** (find secrets if they slip through)
   - Automated scanning (gitleaks, trufflehog)
   - Code reviews
   - Secret detection in CI/CD

3. **Response** (when secrets are exposed)
   - Immediate rotation
   - Audit access logs
   - Incident response plan

4. **Exclusion** (prevent AI access)
   - Content exclusions
   - .gitignore
   - File-level permissions

---

### Secure Coding with AI

**Best practices**:

1. **Review ALL AI-generated code**
   - Check for security vulnerabilities
   - Verify input validation
   - Ensure proper error handling

2. **Don't trust AI for security-critical code**
   - Authentication logic
   - Encryption implementation
   - Access control
   - SQL query building (injection risks)

3. **Test AI-generated code**
   ```javascript
   // If Copilot generates:
   function authenticate(username, password) {
     const query = `SELECT * FROM users WHERE username='${username}'`;
     // ⚠️ SQL INJECTION VULNERABILITY!
   }
   
   // You must catch and fix:
   function authenticate(username, password) {
     const query = 'SELECT * FROM users WHERE username = $1';
     return db.query(query, [username]); // ✅ Parameterized
   }
   ```

---

### Secrets Management

**Use dedicated tools**:

**AWS Secrets Manager**:
```javascript
const AWS = require('aws-sdk');
const secretsManager = new AWS.SecretsManager();

async function getSecret(secretName) {
  const data = await secretsManager.getSecretValue({
    SecretId: secretName
  }).promise();
  return JSON.parse(data.SecretString);
}

// Instead of:
// const apiKey = "sk-123..."; ❌

// Use:
const secrets = await getSecret('prod/api-keys'); // ✅
const apiKey = secrets.openai_key;
```

**Environment Variables (simpler)**:
```javascript
// .env (never commit!)
API_KEY=sk-1234567890

// app.js
require('dotenv').config();
const apiKey = process.env.API_KEY; // ✅
```

**HashiCorp Vault** (enterprise):
```javascript
const vault = require('node-vault')({
  endpoint: 'https://vault.company.com',
  token: process.env.VAULT_TOKEN
});

const secrets = await vault.read('secret/data/api-keys');
const apiKey = secrets.data.openai_key; // ✅
```

---

## Compliance Considerations

### GDPR (General Data Protection Regulation)

**Requirements**:
- Data minimization
- Right to erasure
- Data processing records

**With Copilot**:
- ⚠️ **Do NOT paste EU citizen data** into prompts
- ⚠️ **Use Enterprise account** for GDPR-sensitive projects
- ✅ **Document data processing** in your GDPR records
- ✅ **Configure data retention** policies (Enterprise)

**Example compliant use**:
```javascript
// ❌ Non-compliant
const euUsers = [
  { name: "Jean Dupont", email: "jean@example.fr" }
];
// Asking Copilot to process this

// ✅ Compliant
"Show me how to structure a user object with name and email fields"
// Use synthetic data
```

---

### HIPAA (Health Insurance Portability and Accountability Act)

**Requirements**:
- Protected Health Information (PHI) must be secure
- Business Associate Agreements (BAA)
- Audit trails

**With Copilot**:
- 🛑 **NEVER paste real patient data**
- ⚠️ **Enterprise account required** (with BAA)
- ✅ **De-identify all data** before asking questions
- ✅ **Maintain audit logs**

**Example compliant use**:
```javascript
// ❌ HIPAA violation
const patient = {
  name: "John Doe",
  mrn: "12345",
  diagnosis: "Diabetes"
};

// ✅ Compliant (synthetic data)
const patient = {
  name: "Patient_001",
  mrn: "MRN_REDACTED",
  diagnosis: "CONDITION_A"
};
```

---

### PCI-DSS (Payment Card Industry Data Security Standard)

**Requirements**:
- Never store full credit card numbers unencrypted
- Strict access controls
- Logging and monitoring

**With Copilot**:
- 🛑 **NEVER paste credit card numbers** (even test ones)
- 🛑 **NEVER paste payment processing code** with real tokens
- ✅ **Use payment provider SDKs** (Stripe, PayPal)
- ✅ **Ask generic questions** about payment flows

**Example safe prompt**:
```
"Show me how to integrate Stripe payment processing with webhooks
for subscription payments. Use placeholder API keys."
```

---

### SOC 2 (Service Organization Control 2)

**Requirements**:
- Security controls
- Audit trails
- Change management

**With Copilot**:
- ⚠️ **Document Copilot use** in security policies
- ⚠️ **Audit AI-generated code** as part of code review
- ✅ **Use Enterprise account** for audit logs
- ✅ **Maintain change records**

---

### ISO 27001

**Requirements**:
- Information security management
- Risk assessment
- Access control

**With Copilot**:
- ✅ **Include AI tools in risk assessment**
- ✅ **Define acceptable use policy**
- ✅ **Configure content exclusions**
- ✅ **Regular security reviews**

---

## Incident Response

### If You Accidentally Share Secrets

**Immediate actions** (within minutes):

1. **Rotate the exposed secret immediately**
   ```bash
   # Example: Rotate API key
   # 1. Generate new key in provider dashboard
   # 2. Update environment variable
   # 3. Revoke old key
   ```

2. **Check Copilot chat history**
   - Open Copilot Chat
   - Click "..." → "Clear Chat" (prevents persistence)

3. **Remove from code**
   ```bash
   git filter-branch --force --index-filter \
     "git rm --cached --ignore-unmatch path/to/file-with-secret" \
     --prune-empty --tag-name-filter cat -- --all
   ```

4. **Force push** (if already committed)
   ```bash
   git push --force --all
   ```

5. **Notify your team/security team**

---

### If You Shared Customer Data

**Immediate actions**:

1. **Clear chat history** in Copilot

2. **Document the incident**
   - What data was shared
   - When it happened
   - How much data
   - What AI service

3. **Notify required parties**:
   - Security team
   - Legal/compliance team
   - Data protection officer (if applicable)
   - Potentially affected customers (if required by law)

4. **Review and improve**:
   - Why did it happen?
   - How to prevent in future?
   - Update training?
   - Improve exclusions?

---

### Incident Response Template

```markdown
# Copilot Security Incident Report

**Date/Time**: YYYY-MM-DD HH:MM
**Reporter**: [Your name]
**Severity**: [Low / Medium / High / Critical]

## What Happened
- Describe what was accidentally shared
- In which file/prompt
- Which AI service (Copilot Chat, inline)

## Data Exposed
- Type of data: [Secret / PII / Proprietary / Other]
- Sensitivity level: [1-5]
- Estimated volume: [N records / N lines]

## Immediate Actions Taken
- [ ] Secrets rotated (if applicable)
- [ ] Chat history cleared
- [ ] Code removed/redacted
- [ ] Team notified

## Root Cause
- Why did this happen?
- What controls failed?

## Prevention Plan
- How will we prevent recurrence?
- What processes need updating?
- What training is needed?

## Follow-up Actions
- [ ] Update .gitignore
- [ ] Configure content exclusions
- [ ] Team training session
- [ ] Policy update
- [ ] [Other specific actions]
```

---

## Real-World Scenarios

### Scenario 1: The API Key Mishap

**What Happened**:
```javascript
// Developer accidentally asked:
"Here's my OpenAI integration:
const client = new OpenAI({ apiKey: 'sk-proj-abc123...' });
How do I add error handling?"
```

**Problem**: 
- Real API key shared with Copilot
- Key may be logged
- Potential unauthorized usage

**Solution**:
1. Immediately rotated API key
2. Checked OpenAI usage logs for unauthorized calls
3. Updated code to use environment variables
4. Added pre-commit hook to scan for API keys
5. Team training on what NOT to share

**Prevention**:
```javascript
// Always ask like this:
"I have an OpenAI client initialized with process.env.OPENAI_API_KEY.
How do I add retry logic for failed requests?"
```

---

### Scenario 2: The Customer Data Export

**What Happened**:
Developer exported customer data for debugging and asked Copilot to analyze it:
```javascript
const customers = [
  { id: 1, name: "Acme Corp", revenue: 150000, email: "john@acme.com" },
  { id: 2, name: "TechStart Inc", revenue: 85000, email: "sarah@techstart.com" }
  // ... 50 more real customers
];
// "Help me find the top 10 customers by revenue"
```

**Problem**:
- Real customer names and emails exposed
- Potential GDPR violation
- Contract breach (if NDA in place)

**Solution**:
1. Cleared Copilot chat immediately
2. Notified legal team
3. Reviewed compliance requirements
4. No external breach (data stayed within company account)
5. Documented as incident

**Prevention**:
```javascript
// Use synthetic data:
const customers = [
  { id: 1, name: "Customer_A", revenue: 150000, email: "cust1@example.com" },
  { id: 2, name: "Customer_B", revenue: 85000, email: "cust2@example.com" }
];
// "Help me find the top 10 customers by revenue"
```

---

### Scenario 3: The Proprietary Algorithm

**What Happened**:
Junior developer pasted entire proprietary pricing algorithm (company's competitive advantage) into Copilot Chat for optimization suggestions.

**Problem**:
- Trade secret potentially compromised
- Competitive advantage at risk
- Possible violation of employment agreement

**Solution**:
1. Legal review triggered
2. Algorithm not actually compromised (stayed internal)
3. Developer training on what constitutes proprietary code
4. Added watermarks/comments to proprietary code files

**Prevention**:
```javascript
// Instead of sharing entire algorithm:
"I have a pricing calculation function that takes multiple inputs
and applies a complex formula. How do I optimize the structure
for better performance without changing the logic?"

// Share structure, not secret sauce:
function calculatePrice(input1, input2, input3) {
  // [Proprietary logic - implement manually]
  return price;
}
```

---

### Scenario 4: The Production Database URL

**What Happened**:
```javascript
// Shared in Copilot Chat:
"I'm getting connection timeout with this config:
const db = new Database('postgres://admin:SecurePass123@prod-db.company.internal:5432/maindb')
How do I fix it?"
```

**Problem**:
- Production database credentials exposed
- Internal hostname revealed
- Database name and structure hints

**Solution**:
1. Changed database password immediately
2. Reviewed database access logs
3. No unauthorized access detected
4. Updated to use secrets manager

**Prevention**:
```javascript
// Correct way:
"I'm getting connection timeout connecting to PostgreSQL.
Using connection string from environment variable.
How do I add connection pooling and retry logic?"
```

---

## Practical Exercises

### Exercise 1: Identify Sensitive Data

**Task**: Review this code and identify what should NOT be shared with AI:

```javascript
// config.js
const config = {
  appName: "MyApp",
  version: "1.0.0",
  database: {
    host: "prod-db-01.internal.company.com",
    port: 5432,
    username: "admin",
    password: "Prod2024!Secure",
    database: "maindb"
  },
  api: {
    baseUrl: "https://api.example.com",
    key: "ak-1234567890abcdef",
    secret: "sk-abcdef1234567890"
  },
  features: {
    newCheckoutFlow: true,
    experimentalPayments: false
  },
  users: [
    { name: "John Doe", email: "john@example.com", role: "admin" }
  ]
};
```

**What NOT to share** (answers below):
<details>
<summary>Click to reveal answers</summary>

❌ database.host (internal infrastructure)
❌ database.username (credential)
❌ database.password (SECRET!)
❌ api.key (SECRET!)
❌ api.secret (SECRET!)
❌ users array (if real data - this might be OK if synthetic)
⚠️ features.newCheckoutFlow (potentially proprietary)

✅ appName (OK)
✅ version (OK)
✅ api.baseUrl (OK if public API)
</details>

---

### Exercise 2: Redact Before Sharing

**Task**: You want to ask Copilot how to improve this code. Redact it first.

```javascript
const stripe = require('stripe')('sk_live_51HaB4yC2De...');

async function chargeCustomer(customerId, amount) {
  const customer = await stripe.customers.retrieve(customerId);
  const paymentMethod = customer.default_payment_method;
  
  const charge = await stripe.charges.create({
    amount: amount,
    currency: 'usd',
    customer: customerId,
    description: `Charge for ${customer.email}`
  });
  
  return charge;
}
```

**Redacted version** (answer below):
<details>
<summary>Click to reveal answer</summary>

```javascript
const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);

async function chargeCustomer(customerId, amount) {
  const customer = await stripe.customers.retrieve(customerId);
  const paymentMethod = customer.default_payment_method;
  
  const charge = await stripe.charges.create({
    amount: amount,
    currency: 'usd',
    customer: customerId,
    description: `Charge for user ${customerId}`
  });
  
  return charge;
}

// When asking Copilot:
// "I have a Stripe charge function that retrieves a customer
// and creates a charge. How do I add error handling and
// handle  failed payments?"
```
</details>

---

### Exercise 3: Configure Exclusions

**Task**: Set up content exclusions for this project structure:

```
my-project/
├── src/
├── config/
│   ├── default.yml
│   ├── development.yml
│   └── production.yml  ← Sensitive!
├── credentials/        ← Sensitive!
│   └── service-account.json
├── .env                ← Sensitive!
├── .env.local          ← Sensitive!
└── keys/               ← Sensitive!
    ├── private.key
    └── certificate.pem
```

**Your task**: Create `.vscode/settings.json` with appropriate exclusions.

**Solution** (answer below):
<details>
<summary>Click to reveal answer</summary>

```json
{
  "github.copilot.advanced": {
    "excludedFolders": [
      "credentials",
      "keys",
      "config"
    ]
  },
  
  "files.exclude": {
    "**/.env": true,
    "**/.env.*": true,
    "**/credentials/": true,
    "**/keys/": true,
    "**/config/production.*": true,
    "**/*.key": true,
    "**/*.pem": true
  }
}
```

Also create `.gitignore`:
```
.env
.env.*
credentials/
keys/
config/production.*
*.key
*.pem
```
</details>

---

### Exercise 4: Incident Response Drill

**Scenario**: You just realized you shared this in Copilot Chat 10 minutes ago:

```
"Here's my database connection code:
mongoose.connect('mongodb://admin:SuperSecret123@10.0.1.50:27017/proddb')
Help me add connection pooling"
```

**Your task**: Write out step-by-step what you'd do RIGHT NOW.

**Solution** (answer below):
<details>
<summary>Click to reveal answer</summary>

**Immediate (next 5 minutes)**:
1. Clear Copilot chat history (prevents persistence)
2. Open MongoDB admin panel
3. Change password for 'admin' user to new secure password
4. Update all services using this password to use new one
5. Check MongoDB access logs for unauthorized connections

**Next 30 minutes**:
6. Update code to use environment variable:
   ```javascript
   mongoose.connect(process.env.MONGODB_URI)
   ```
7. Add MONGODB_URI to .env (and .env to .gitignore)
8. Document incident in security log
9. Notify team lead/security team

**Next 24 hours**:
10. Review if incident needs formal reporting (depends on company policy)
11. Add pre-commit hook to scan for connection strings
12. Update team training materials
13. Add MongoDB connection strings to git-secrets patterns

**Lessons learned**:
- Always use environment variables for credentials
- Never paste connection strings with embedded credentials
- Set up automated scanning to catch this before sharing
</details>

---

## Completion Checklist

### Understanding
- [ ] Know what data is sent to GitHub/OpenAI
- [ ] Understand data retention policies
- [ ] Know difference between account types (Individual/Business/Enterprise)
- [ ] Understand temporary vs. permanent data storage

### What NOT to Share
- [ ] Can identify API keys and secrets
- [ ] Can identify PII in code
- [ ] Can identify proprietary business logic
- [ ] Can identify infrastructure details that should stay private
- [ ] Can identify compliance-sensitive data (GDPR, HIPAA, PCI)

### Configuration
- [ ] Configured content exclusions in VS Code
- [ ] Created/updated .gitignore for sensitive files
- [ ] Set up .env for environment variables
- [ ] Verified exclusions are working

### Best Practices
- [ ] Always use environment variables for secrets
- [ ] Always redact data before sharing with AI
- [ ] Review AI-generated code for security issues
- [ ] Know how to respond if secrets are accidentally shared
- [ ] Understand compliance requirements for your domain

### Team/Organization
- [ ] Know which account type you have/need
- [ ] Understand organization policies around AI
- [ ] Know who to contact for security incidents
- [ ] Documented acceptable use guidelines (if team lead)

---

## Summary: Key Takeaways

### The Golden Rules

1. **🛑 NEVER share secrets** (API keys, passwords, tokens)
2. **🛑 NEVER share real PII** (names, emails, SSNs, credit cards)
3. **🛑 NEVER share proprietary code** without permission
4. **⚠️ ALWAYS redact** sensitive data before asking questions
5. **⚠️ ALWAYS use environment variables** for configuration
6. **⚠️ ALWAYS review** AI-generated code for security issues
7. **✅ DO configure** content exclusions
8. **✅ DO use** appropriate account type for your needs
9. **✅ DO train** your team on privacy best practices
10. **✅ DO have** an incident response plan

---

### One-Minute Security Check

**Before using Copilot, ask yourself**:
- ❓ Does this file contain secrets? → Exclude it or use env vars
- ❓ Does this data identify real people? → Use synthetic data instead
- ❓ Is this proprietary to my company? → Share only generic patterns
- ❓ Would I put this on a public GitHub repo? → If no, be extra careful
- ❓ Does my industry have special compliance? → Use appropriate account type

---

## Next Steps

You've completed **Phase 1: Foundations**! 🎉

**Phase 1 completed**:
- ✅ Understanding AI Coding Assistants
- ✅ VS Code Setup
- ✅ Privacy & Security Basics

**Ready for**:
- 📚 **Phase 2: Prompt Engineering** - Learn to communicate effectively with AI
- 🎯 **Phase 3: Effective Workflows** - Build practical AI-assisted development habits

---

## Reference Links

### Official Documentation
- **GitHub Copilot Privacy**: https://docs.github.com/en/site-policy/privacy-policies/github-copilot-privacy-statement
- **GitHub Privacy Policy**: https://docs.github.com/en/site-policy/privacy-policies/github-privacy-statement
- **Copilot Trust Center**: https://resources.github.com/copilot-trust-center/
- **Enterprise Content Exclusions**: https://docs.github.com/en/enterprise-cloud@latest/copilot/managing-copilot-business

### Security Tools
- **git-secrets**: https://github.com/awslabs/git-secrets
- **gitleaks**: https://github.com/gitleaks/gitleaks
- **trufflehog**: https://github.com/trufflesecurity/trufflehog
- **detect-secrets**: https://github.com/Yelp/detect-secrets

### Compliance Resources
- **GDPR Compliance**: https://gdpr.eu/
- **HIPAA Compliance**: https://www.hhs.gov/hipaa/
- **PCI-DSS**: https://www.pcisecuritystandards.org/

---

*Last updated: March 2026*
*⚠️ Privacy policies change - always verify current terms*
