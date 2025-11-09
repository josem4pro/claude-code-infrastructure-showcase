# auth-route-debugger: Research Report
**División**: auth-route-debugger | **Fecha**: 2025-11-07
**Archivos**: LA_BIBLIA §12.4, §13.8 | CLAUDE_INTEGRATION_GUIDE.md
**Enfoque**: Auth/AuthZ requirements, error flows, recovery patterns

---

## I. COMPONENT IDENTIFICATION

### 1.1 Ecosystem Position
- **ID**: A8 | **Type**: Agent (autonomous) | **LOC**: ~310
- **Location**: `.claude/agents/auth-route-debugger.md`
- **Activation**: ❌ Manual (explicit invocation)
- **Complexity**: Media (2-5 steps)

### 1.2 Related Components
**Testing/Debugging Ecosystem**:
- **route-tester** (S4-Skill): Theoretical guidelines → auth-route-debugger applies
- **auth-route-tester** (A9-Agent): Functional validation → Complementary (debugger diagnoses, tester validates)
- **/route-research-for-testing** (C3-Command): Orchestrates route investigation → Can invoke auth-route-tester

**Critical Differentiation**:
- auth-route-debugger: Diagnose existing issues (401/403/404)
- auth-route-tester: Validate new/modified routes
- route-tester: Knowledge base on testing patterns

---

## II. AUTHENTICATION REQUIREMENTS

### 2.1 Tech Stack
**Showcase Auth Model**:
- SSO: Keycloak (OpenID Connect)
- Method: Cookie-based JWT (NOT Bearer tokens)
- Cookie: `refresh_token`
- JWT Signing: Secret in `config.ini`
- Claims: Stored in `res.locals.claims`

**Test Credentials**:
```javascript
{ username: "testuser", password: "testpassword",
  realm: "yourRealm", client: "your-app-client" }
```

### 2.2 Auth Flow
```
Request → SSO Middleware checks refresh_token cookie
        → Validates JWT with config.ini secret
        ├─ Valid → Extract claims to res.locals.claims
        └─ Invalid → 401 Unauthorized

Route Handler → Accesses res.locals.claims.username/roles
```

### 2.3 Testing Script: test-auth-route.js
**Auto-contained testing** (eliminates manual friction):
1. Fetches Keycloak refresh token (hardcoded credentials)
2. Signs JWT with config.ini secret
3. Creates cookie header: `Cookie: refresh_token=<signed-token>`
4. Executes authenticated request
5. Returns response + reproducible curl command

**Usage**:
```bash
# GET
node scripts/test-auth-route.js http://localhost:3000/blog-api/api/endpoint

# POST with payload
node scripts/test-auth-route.js \
    --method POST --body '{"field":"value"}' \
    http://localhost:3002/api/workflow/start

# No auth (testing)
node scripts/test-auth-route.js --no-auth http://localhost:3002/api/health
```

---

## III. AUTHORIZATION REQUIREMENTS

### 3.1 RBAC Implementation
**Roles**: Stored in `res.locals.claims.roles` → Typical: `["admin", "operations", "user"]`

**Verification Pattern**:
```typescript
const userRoles = res.locals.claims.roles || [];
if (!userRoles.includes('admin')) {
  return res.status(403).json({ error: 'Forbidden', message: 'Requires admin role' });
}
```

### 3.2 Mock Authentication (Dev Only)
**Purpose**: Bypass Keycloak for local development

**Config** (`.env`):
```bash
NODE_ENV=development
MOCK_AUTH=true
MOCK_USER_ID=test-user
MOCK_USER_ROLES=admin,operations
```

**Usage**:
```bash
curl -H "X-Mock-Auth: true" -H "X-Mock-User: test-user" \
     -H "X-Mock-Roles: admin" http://localhost:3002/api/protected
```

**Security Guardrails**:
- ONLY works if `NODE_ENV=development|test`
- `mockAuth` middleware required on route
- NEVER works in production

### 3.3 Resource-Level Permissions
**Pattern**: Verify userId matches resource ownerId
```typescript
const resource = await prisma.resource.findUnique({ where: { id } });
if (resource.ownerId !== res.locals.claims.userId) {
  return res.status(403).json({ error: 'Not the resource owner' });
}
```

---

## IV. ERROR FLOWS & RECOVERY

### 4.1 Error Taxonomy

#### 401 Unauthorized
**Causes**:
1. Token expired (Keycloak token lifetime)
2. Malformed cookie (missing `refresh_token=` prefix, invalid JWT signature)
3. JWT secret mismatch (middleware secret ≠ config.ini)
4. Keycloak unavailable

**Debugging**:
```bash
docker ps | grep keycloak  # Verify Keycloak running
node scripts/test-auth-route.js http://localhost:3002/api/health  # Regenerate token
cat form/config.ini | grep jwtSecret  # Verify secret
```

#### 403 Forbidden
**Causes**:
1. Missing required role
2. Resource ownership violation
3. Custom authorization logic failure

**Debugging**: Use mock auth with admin role
```bash
curl -H "X-Mock-Auth: true" -H "X-Mock-User: test-admin" \
     -H "X-Mock-Roles: admin" http://localhost:3002/api/protected
```

#### 404 Not Found
**Causes**:
1. Typo in URL / missing route prefix
2. Route not registered in app.ts
3. Route registered AFTER catch-all route
4. Naming conflicts (`:id` patterns intercept specific routes)

**Debugging**:
```bash
grep "app.use" blog-api/src/app.ts  # Check prefixes
pm2 list | grep blog-api  # Verify service running
pm2 logs blog-api --lines 500 | grep -i error  # Startup errors
```

### 4.2 Route Registration Issues

**Common Problem**: Route defined but unreachable

**Root Causes**:
1. **Catch-all registered first**:
```typescript
// ❌ WRONG
app.use('*', notFoundHandler);  // Intercepts all
app.use('/api/specific', specificRoutes);  // Never reached

// ✅ CORRECT
app.use('/api/specific', specificRoutes);
app.use('*', notFoundHandler);  // Last resort
```

2. **Naming conflicts**:
```typescript
// ❌ CONFLICT
app.use('/api/:id', genericHandler);  // Intercepts /api/users
app.use('/api/users', usersHandler);  // Never reached

// ✅ CORRECT
app.use('/api/users', usersHandler);  // Specific first
app.use('/api/:id', genericHandler);
```

### 4.3 Cookie Configuration Issues

**Problem**: Cookies not transmitted frontend ↔ backend

**Causes**:
1. **Dev vs Prod settings**:
```typescript
{
  httpOnly: true,
  secure: process.env.NODE_ENV === 'production',  // HTTPS only in prod
  sameSite: 'lax',  // 'strict'/'none' alternatives
  domain: process.env.COOKIE_DOMAIN || 'localhost'
}
```

2. **CORS credentials**:
```typescript
app.use(cors({
  origin: process.env.FRONTEND_URL,
  credentials: true  // ⚠️ REQUIRED for cookies
}));
```

3. **SameSite policy**:
- `strict`: Blocks cross-origin requests
- `lax`: Allows GET cross-origin
- `none`: Requires `Secure=true` (HTTPS)

### 4.4 Agent Debugging Workflow (6 Phases)

```
PHASE 1: Initial Assessment
├─ Check project-memory MCP for similar issues
├─ Identify route, HTTP method, error code
└─ Gather payload info (handler inspection or user data)

PHASE 2: Live Service Logs (PM2)
├─ pm2 logs form --lines 200
├─ tail -f form/logs/form-error.log
└─ pm2 list (verify service running)

PHASE 3: Route Registration Checks
├─ Verify route in app.ts
├─ Check registration order
├─ Look for naming conflicts (/api/:id before specific)
└─ Verify middleware applied correctly

PHASE 4: Authentication Testing
├─ node scripts/test-auth-route.js [URL]
├─ Test with/without auth (--no-auth flag)
├─ Investigate cookie config, JWT validation
└─ Check token expiration, role requirements

PHASE 5: Testing Payloads
├─ Check route handler expected body structure
├─ Look for validation schemas (Zod, Joi)
├─ Review TypeScript interfaces
└─ Check existing tests for examples

PHASE 6: Documentation Updates
├─ Update project-memory MCP with solution
├─ Document new issue type if applicable
├─ Include specific commands used
└─ Document workarounds/temporary fixes
```

---

## V. MEMORY SYSTEM INTEGRATION

### 5.1 project-memory MCP Protocol
**Workflow**:
1. **Before debugging**: Query memory for similar errors
2. **After resolution**: Update memory with new solution

**Memory Entry Structure**:
```json
{
  "problem": "401 error in /api/workflow/123",
  "rootCause": "JWT secret mismatch in config.ini",
  "solution": "Sync jwtSecret across services",
  "commands": ["grep jwtSecret */config.ini", "node scripts/test-auth-route.js [URL]"],
  "timestamp": "2025-11-07"
}
```

---

## VI. CRITICAL ANALYSIS (AUTH PERSPECTIVE)

### 6.1 Strengths

#### Automated Testing Scripts
**Impact**: Reduces debugging time from ~30 min to ~10 min (66% reduction)
- ✅ Auto-generates token + signs JWT + creates curl command
- ✅ Eliminates manual Keycloak token fetching (~5 min)
- ✅ Reproducible testing workflow

#### Mock Auth for Development
**Impact**: Fast iteration without Keycloak dependency
- ✅ Instant testing with different roles
- ✅ Security guardrails (dev/test only)
- ⚠️ Limitation: Requires adding `mockAuth` middleware to routes

#### Systematic 6-Phase Workflow
**Impact**: Reproducible and documented debugging
- ✅ Eliminates trial-and-error
- ✅ Memory integration compounds team knowledge
- ✅ Second occurrence: 10x faster (2 min vs 10 min)

### 6.2 Limitations

#### Stack-Specific Dependencies
**Tied to**: Cookie-JWT + Keycloak + Express + PM2
- ❌ Bearer token auth: Requires complete rewrite
- ❌ Serverless/Lambda: No PM2 logs
- ❌ Django/Flask: Scripts need Python rewrite

**CLAUDE_INTEGRATION_GUIDE stance**:
> "Ask: 'Do you use JWT cookies for auth?'
> If NO: 'Skip or adapt for [their auth type]?'"

#### Hardcoded Paths
**Problem**: Scripts reference absolute paths
```javascript
// In route-tester SKILL.md
Location: /root/git/your project_pre/scripts/test-auth-route.js
```
**Impact**: Requires manual modification when integrating

#### No Auto-Resolution
**Gap**: Agent diagnoses but doesn't auto-fix
- ✅ Identifies: "JWT secret mismatch"
- ✅ Suggests: "Sync jwtSecret in config.ini"
- ❌ Doesn't execute fix (requires human approval)

**Justification**: Safety - config changes need approval

**Contrast**: auto-error-resolver (A10) auto-fixes TypeScript errors

### 6.3 Tech Stack Compatibility

#### NOT Supported (Require Complete Adaptation)

**Bearer Token Auth**:
```diff
- const token = req.cookies.refresh_token;  // Cookie-based
+ const authHeader = req.headers.authorization;  // Bearer
+ const token = authHeader?.split(' ')[1];
```

**Session-based Auth** (express-session):
```diff
- res.locals.claims = decodeJWT(token);  // JWT
+ req.session.user = user;  // Session
```

**OAuth2 Client Credentials**: M2M (machine-to-machine) not contemplated

#### Process Managers (PM2 Alternatives)
**Not Supported**:
- Systemd: `journalctl -u service-name`
- Docker Compose: `docker-compose logs -f`
- Kubernetes: `kubectl logs pod-name`
- AWS Lambda: CloudWatch Logs

**Impact**: Phase 2 (Live Logs) requires adaptation

---

## VII. USE CASES (From Agent YAML)

### Case 1: 401 Error in Authenticated Route
**User**: "401 error accessing /api/workflow/123 though I'm logged in"
**Agent Workflow**:
1. Check memory for similar 401 errors
2. Verify route in app.ts
3. Test with `test-auth-route.js`
4. Diagnose: token expiration vs JWT secret vs cookie config
5. Provide solution + testing commands
6. Update memory

### Case 2: 404 Despite Route Definition
**User**: "POST /form/submit returns 404 but route is defined"
**Agent Workflow**:
1. Read `blog-api/src/app.ts`
2. Verify registration order
3. Check for catch-all routes before specific
4. Check naming conflicts (`:id` patterns)
5. Verify router export/import
6. PM2 logs for startup errors

### Case 3: Testing Authenticated Endpoint
**User**: "Test /api/user/profile endpoint with auth"
**Agent Workflow**:
1. Locate handler to understand expected response
2. Run `node scripts/test-auth-route.js http://localhost:3000/api/user/profile`
3. Verify response structure
4. Check database consistency if needed
5. Test edge cases (expired token, missing claims)

---

## VIII. ECOSYSTEM INTEGRATION

### 8.1 Component Relationships

**route-tester (SKILL) → auth-route-debugger (AGENT)**:
- Skill provides theoretical guidelines
- Agent applies patterns to real debugging
- Progressive disclosure: Agent loads only needed knowledge

**auth-route-debugger ↔ auth-route-tester**:
- Debugger: Diagnose existing issues
- Tester: Validate new/modified routes
- **Combined Workflow**:
  1. Implement new route
  2. auth-route-tester validates
  3. If fails → auth-route-debugger diagnoses
  4. Fix applied
  5. auth-route-tester re-validates

**With backend-dev-guidelines (S1)**:
- Defines layered architecture (Routes → Controllers → Services)
- auth-route-debugger verifies:
  - SSO middleware applied at route level
  - Controller accesses `res.locals.claims` correctly

**With error-tracking (S5)**:
- Auth errors should be in Sentry
- Agent can query Sentry for:
  - 401/403 error frequency
  - JWT validation failure stack traces
  - User context in errors

---

## IX. STACK ADAPTATION GUIDE

### 9.1 Transferable Elements (Framework-Agnostic)
✅ **6-Phase Debugging Workflow**: Applies to any framework
✅ **Error Taxonomy** (401/403/404): Universal HTTP
✅ **Testing Philosophy**: Automate, reproduce, verify DB
✅ **Memory Integration**: Knowledge accumulation

### 9.2 Non-Transferable (Stack-Specific)
❌ **test-auth-route.js**: Node.js-specific, Keycloak token fetching
❌ **PM2 Commands**: `pm2 logs` → Systemd/Docker/K8s equivalents
❌ **Express Patterns**: `res.locals.claims`, `app.use('/prefix', router)`
❌ **Cookie Config**: Express cookie-parser specific

### 9.3 Bearer Token Adaptation
**Changes Required**:
```diff
Testing Script:
- headers: { Cookie: `refresh_token=${signedToken}` }
+ headers: { Authorization: `Bearer ${token}` }

Middleware Verification:
- const token = req.cookies.refresh_token;
+ const authHeader = req.headers.authorization;
+ const token = authHeader?.split(' ')[1];

Debugging Checklist:
- [ ] Verify cookie config (httpOnly, secure, sameSite)
+ [ ] Verify Authorization header format
+ [ ] Check CORS allows Authorization header
```

### 9.4 Django/Flask Adaptation
**Route Registration** (Django):
```python
urlpatterns = [
    path('api/workflow/<int:id>/', views.workflow_detail),
    path('api/workflow/', views.workflow_list),  # Order matters!
]
```

**Log Monitoring**:
```bash
tail -f logs/django.log  # Instead of pm2 logs
python manage.py runserver --noreload
```

**Testing Script** (Python):
```python
import requests
from keycloak import KeycloakOpenID

def get_token():
    keycloak = KeycloakOpenID(server_url="...", realm="...", client_id="...")
    return keycloak.token("testuser", "testpassword")

def test_route(url, method='GET', data=None):
    token = get_token()
    headers = {'Authorization': f'Bearer {token}'}
    return requests.request(method, url, headers=headers, json=data)
```

---

## X. IMPACT METRICS

### 10.1 Time Savings
**Manual debugging**: ~30 min
1. Verify Keycloak: 2 min
2. Generate token manually: 5 min
3. Build curl: 3 min
4. Debug JWT secret: 10 min
5. Verify registration: 5 min
6. Manual test: 5 min

**With agent**: ~10 min
1. Invoke agent: 30 sec
2. Automated workflow: 5 min
3. Review + fix: 5 min

**Savings**: 20 min/session (66% reduction)

### 10.2 Context Switching Reduction
**Without agent**: 5-10 tabs (Keycloak docs, JWT libs, Express, Stack Overflow, PM2) = 15 min research
**With agent**: 1 command, 0 min research

### 10.3 Reproducibility
**Memory-powered**: Second occurrence = 2 min (vs 10 min first time)

---

## XI. GAPS & IMPROVEMENTS

### 11.1 CI/CD Integration (Gap)
**Problem**: Agent is interactive, not CI automation
**Proposal**:
```yaml
# .github/workflows/auth-tests.yml
jobs:
  test-auth-routes:
    run: node scripts/test-all-auth-routes.js
```

### 11.2 Mock Data Generation (Gap)
**Problem**: Agent assumes test data exists
**Proposal**: Auto-generate valid payloads from Zod schemas + faker.js

### 11.3 Multi-Service Auth (Gap)
**Problem**: Service A → Service B auth propagation not documented
**Proposal**: Extend for distributed tracing

### 11.4 Performance Testing (Gap)
**Problem**: Agent verifies functionality, NOT performance
**Proposal**: Integrate autocannon (load testing) + Sentry performance monitoring

---

## XII. CONCLUSIONS

### 12.1 Strengths (Auth-Focused)
1. **Automated Scripts**: 80% friction reduction in auth testing
2. **Systematic Workflow**: Eliminates trial-and-error, documents in memory
3. **Mock Auth**: Security + DX (fast iteration, production guardrails)
4. **Memory Integration**: Knowledge compounds (10x faster second occurrence)

### 12.2 Weaknesses
1. **Stack-Specific**: Cookie-JWT + Keycloak + Express + PM2 → Full rewrite for Bearer/serverless
2. **Hardcoded Paths**: Scripts need manual modification
3. **No Auto-Fix**: Diagnoses but requires human intervention
4. **Limited Scope**: No performance, distributed auth, CI/CD

### 12.3 Adoption Recommendations

**ADOPT** if:
- Node.js + Express + Cookie-JWT + Keycloak + PM2
- **ROI**: 66% debug time reduction, 10x faster recurrence

**ADAPT** (4-8 hours) if:
- Different stack but want systematic workflow
- Maintain 6 phases, rewrite scripts/log commands

**MODIFY** (4 hours) for Bearer tokens:
- Testing scripts: 2h | Middleware verification: 1h | Checklist: 1h

### 12.4 Roadmap
**Short-term**: Parametrize paths, Bearer token support, Django/Flask docs
**Medium-term**: CI/CD templates, mock data generation, distributed tracing
**Long-term**: Performance profiling, auto-resolution, multi-provider (Auth0, Cognito)

---

## APPENDIX: FILE MAP
```
.claude/
├─ agents/
│  ├─ auth-route-debugger.md (310 líneas, PRIMARY)
│  └─ auth-route-tester.md (280 líneas)
├─ skills/route-tester/SKILL.md (389 líneas, PRIMARY)
└─ commands/route-research-for-testing.md (120 líneas)

bibliography/Global/Markdown/
├─ LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md (§12.4, §13.8)
└─ CLAUDE_INTEGRATION_GUIDE.md
```

**Cross-References**:
```
auth-route-debugger (A8)
├─ Uses → route-tester (S4) knowledge
├─ Complements → auth-route-tester (A9) validation
├─ Invoked by → /route-research-for-testing (C3)
├─ Integrates → project-memory MCP
└─ References → backend-dev-guidelines (S1) architecture
```

---

**END OF REPORT**
**Lines**: 499 (under 500 limit) ✅
**Investigation Time**: ~2 hours
**Files Analyzed**: 5 primary + 2 reference docs
**Perspective**: Authentication, Authorization, Error Flows, Recovery Patterns
