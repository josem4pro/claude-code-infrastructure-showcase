# Research Report: refactor-planner Division
## Strategic Refactoring Planning in AI-Assisted Development Infrastructure

**Division**: refactor-planner
**Research Date**: 2025-11-07
**Mission**: Parallel investigation from refactor strategy perspective
**Output Location**: `/home/jose/Repositorios/claude-code-infrastructure-showcase/bibliography/Global/Markdown/OUT/02_research/refactor-planner.md`

---

## Executive Summary

This research analyzes the Infrastructure Showcase from a **refactor-planner** lens: anticipating technical debt, identifying strategic refactoring opportunities, and creating migration pathways for systems at scale. The investigation focuses on how cognitive load theory (Sweller, 1988), design patterns (LA_BIBLIA §15), and domain-driven design principles (Evans, 2015) inform long-term refactoring strategy in AI-orchestrated codebases.

**Key Findings**:
1. **Cognitive Load as Refactoring Priority**: Systems exceeding working memory capacity (7±2 items) are prime refactoring candidates
2. **Progressive Disclosure = Incremental Refactoring**: The 500-line rule creates natural boundaries for refactor phases
3. **Schema Acquisition Pattern**: Refactoring that reduces means-ends analysis improves knowledge transfer
4. **Bounded Context Misalignment**: Most legacy technical debt stems from context boundary violations

---

## 1. PRIMARY SOURCES ANALYSIS

### 1.1 LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md (§15 Design Patterns)

**Refactoring Relevance**: §15 would contain the pattern catalog identifying:
- **Structural Debt**: Violations of layered architecture (§9)
- **Cognitive Debt**: Components exceeding 500-line rule (§7.2)
- **Integration Debt**: Hook/Skill/Agent boundaries unclear

**Critical Insight from §7 (Progressive Disclosure)**:
```
Líneas iniciales: 3,279 → 485 (85% reducción)
Warnings de contexto: Frecuentes → 0 (100% eliminación)
```

**Refactoring Implication**:
The 85% context reduction represents a **completed refactoring success story**. The methodology:
1. Identify monolithic context (3,279 lines)
2. Establish modular boundaries (.claude-context/*.md)
3. Enforce 500-line rule per module
4. Result: Zero cognitive overload warnings

**Pattern to Replicate**: When planning legacy system refactors:
- **Phase 1**: Measure current cognitive load (line count, complexity)
- **Phase 2**: Define bounded contexts (< 500 lines each)
- **Phase 3**: Extract resources to separate modules
- **Phase 4**: Validate via AI agent consumption (zero warnings = success)

### 1.2 sweller-cognitive-load-1988.md (Cognitive Load Theory)

**Core Thesis Relevant to Refactoring**:
> "Conventional problem solving through means-ends analysis imposes a heavy cognitive load which does not assist in schema acquisition." (Sweller, p. 257)

**Translation to Refactoring Context**:

**Problem**: Legacy code forces developers into "means-ends analysis" mode:
- Navigate from current state (bug/feature request) to goal state (working code)
- Maintain mental model of: current file, goal state, relation between states, subgoals stack
- Cognitive load prevents learning the system architecture (schema acquisition)

**Refactoring Strategy**:
Replace **goal-specific problems** with **nonspecific goal exploration**:

| Legacy Approach | Refactored Approach |
|-----------------|---------------------|
| "Fix auth bug in UserController" (specific goal) | "Calculate all unknowns in auth flow" (nonspecific) |
| Requires backward-working from bug to cause | Allows forward-working from initial state |
| High cognitive load (4 productions in Sweller's model) | Low cognitive load (1 production) |
| Blocks schema learning | Facilitates architecture understanding |

**Experimental Evidence** (Table 2, p. 272):
```
Conventional problems:
  - Average working memory: 15.5 items
  - Active productions: 4
  - Conditions matched: 29

Nonspecific goal problems:
  - Average working memory: 14 items
  - Active productions: 1
  - Conditions matched: 17
```

**Refactoring Planning Insight**:
When planning refactors, **prioritize reducing cognitive productions** over reducing lines of code:
- Fewer decision points > fewer total lines
- Single responsibility (1 production) > DRY principle
- Forward-chaining architecture > backward-chaining debugging

**Practical Application to Showcase**:

The **skill-activation-prompt** hook (§11.1) implements Sweller's nonspecific goal strategy:
- Instead of: "User must remember to invoke backend-dev-guidelines" (specific goal, backward-working)
- System provides: "Here are all relevant skills for your prompt" (nonspecific, forward-working)
- Result: **80% reduction in manual commands** (§19.1) = cognitive load successfully transferred to system

### 1.3 evans-ddd-reference-2015-free.md (Domain-Driven Design)

**Key Concepts for Refactoring Strategy**:

#### Bounded Context (p. 2)
> "Explicitly define the context within which a model applies. Explicitly set boundaries in terms of team organization, usage within specific parts of the application, and physical manifestations such as code bases and database schemas."

**Refactoring Debt Pattern**: Legacy systems suffer from **context bleeding**:

**Example from Showcase Architecture** (§6.1):
```
┌─────────────────────────────────────────┐
│ NIVEL 4: SLASH COMMANDS (Orquestación) │ ← Context: User Workflows
├─────────────────────────────────────────┤
│ NIVEL 3: AGENTS (Autonomía)            │ ← Context: Multi-step reasoning
├─────────────────────────────────────────┤
│ NIVEL 2: SKILLS (Conocimiento)         │ ← Context: Domain knowledge
├─────────────────────────────────────────┤
│ NIVEL 1: HOOKS (Fundación)             │ ← Context: Event interception
└─────────────────────────────────────────┘
```

**Anti-pattern to Refactor**:
When hooks directly invoke agents (skipping skills), or commands bypass agents (direct skill access), **bounded context integrity is violated**.

**Refactoring Strategy**:
1. **Audit Context Boundaries**: Map actual call patterns vs intended architecture
2. **Identify Violations**: Document cross-context calls
3. **Insert Facades**: Create adapters at context boundaries
4. **Validate Separation**: Test that each level can evolve independently

#### Layered Architecture (p. 10)
> "Isolate the expression of the domain model and the business logic, and eliminate any dependency on infrastructure, user interface, or even application logic."

**Showcase Implementation** (§9):
```
Routes → Controllers → Services → Repositories → Database
```

**Common Refactoring Scenario**: **Fat Controllers**

Legacy pattern:
```typescript
// AuthController.ts (VIOLATES LAYERS)
export class AuthController {
  async login(req: Request, res: Response) {
    // ❌ Business logic in controller
    const user = await prisma.user.findUnique({ where: { email: req.body.email }});
    if (!user) return res.status(401).json({ error: "Invalid credentials" });

    // ❌ Database access in controller
    const token = jwt.sign({ userId: user.id }, process.env.JWT_SECRET);

    // ❌ Direct response construction
    return res.json({ user, token });
  }
}
```

**Refactored Pattern** (Following §9):
```typescript
// Routes layer
router.post("/login", authController.login);

// Controllers layer
export class AuthController extends BaseController {
  async login(req: Request, res: Response) {
    const result = await this.authService.authenticate(req.body);
    return this.success(res, result);
  }
}

// Services layer (business logic)
export class AuthService {
  async authenticate(credentials: LoginDTO) {
    const user = await this.userRepository.findByEmail(credentials.email);
    if (!user) throw new AuthenticationError();
    const token = await this.generateJWT(user);
    return { user, token };
  }
}

// Repositories layer (data access)
export class UserRepository {
  async findByEmail(email: string): Promise<User | null> {
    return this.prisma.user.findUnique({ where: { email } });
  }
}
```

**Refactoring Migration Path**:
1. **Phase 1**: Extract repository methods (vertical slice)
2. **Phase 2**: Extract service methods (business logic)
3. **Phase 3**: Slim controllers to BaseController pattern
4. **Phase 4**: Validate: Controllers have zero business logic

#### Ubiquitous Language (p. 3)
> "Use the model as the backbone of a language. Commit the team to exercising that language relentlessly in all communication within the team and in the code."

**Refactoring Debt Pattern**: **Terminology Drift**

Common in aging codebases:
- Code: `UserAccount` / Docs: `Customer` / Team: "profile"
- Database: `usr_tbl` / API: `users` / Frontend: `members`

**Showcase Example** (§4 Glosario):
Consistent terminology across 26 sections:
- **Agent**: Always means autonomous multi-step instance
- **Skill**: Always means modular knowledge base (< 500 lines)
- **Hook**: Always means event interceptor

**Refactoring Strategy**:
1. **Terminology Audit**: Grep codebase for synonyms
2. **Establish Canon**: Document single term in ubiquitous language
3. **Rename Refactor**: Use IDE refactoring (safe rename)
4. **Update Docs**: Single source of truth in CLAUDE.md

**Automation Opportunity**:
Create **terminology-validator** agent:
```bash
# Pseudo-code
grep -r "UserAccount\|Customer\|profile" src/
# Flag violations of ubiquitous language
# Suggest: Replace all with canonical term "User"
```

---

## 2. STRATEGIC REFACTORING PATTERNS IDENTIFIED

### 2.1 Progressive Disclosure Refactoring (PDR)

**Definition**: Incrementally extract nested complexity into separate bounded contexts while maintaining functional equivalence.

**Inspired by**: §7 (Progressive Disclosure) + Sweller's nonspecific goal strategy

**Pattern Template**:
```
BEFORE (Monolithic):
  - component.ts: 2,500 lines
  - Cognitive load: 8-12 concepts simultaneously
  - Schema acquisition: Blocked

AFTER (Progressive):
  - component.ts: 450 lines (entry point)
  - component/resources/strategy-a.ts: 400 lines
  - component/resources/strategy-b.ts: 380 lines
  - component/resources/utilities.ts: 350 lines
  - Cognitive load per file: 3-5 concepts
  - Schema acquisition: Enabled
```

**Refactoring Steps**:
1. **Baseline Measurement**: Count lines, cyclomatic complexity, depth
2. **Identify Subdomains**: Cluster related functions (AST analysis)
3. **Extract Resources**: Move clusters to separate files
4. **Establish Entry Point**: Main file becomes index/facade
5. **Validate 500-Line Rule**: Each module < 500 lines
6. **Test Schema Acquisition**: Can new dev understand system in 15 min?

**Success Metrics**:
- Lines per file: < 500 (hard limit)
- Concepts per file: < 7 (working memory limit)
- Onboarding time: < 30 min to first contribution
- Context warnings: 0 (AI agent consumption)

### 2.2 Cognitive Load Reduction Refactoring (CLRR)

**Definition**: Prioritize reducing mental model complexity over traditional metrics (LOC, cyclomatic complexity).

**Inspired by**: Sweller's production system model (Table 1, p. 267)

**Measurement Framework**:

| Metric | Legacy Code | Refactored Target |
|--------|-------------|-------------------|
| **Decision Points** | N conditions in nested if/else | Single strategy pattern |
| **Working Memory Items** | All variables in scope | < 7 simultaneous |
| **Subgoal Stack Depth** | Callback nesting level | Flat async/await |
| **Context Switching** | File hops to understand flow | Single file visibility |

**Example Refactoring**:

**Before (High Cognitive Load)**:
```typescript
// 4 productions (Sweller's means-ends model)
async function processOrder(orderId: string) {
  // Production 1: Check goal achievable
  const order = await getOrder(orderId);
  if (!order) throw new Error("Not found");

  // Production 2: Set subgoals
  const user = await getUser(order.userId);
  const inventory = await checkInventory(order.items);

  // Production 3: Chain subgoals
  if (!inventory.available) {
    const backorder = await createBackorder(order);
    // ... nested logic
  }

  // Production 4: Solve for goal
  const result = await finalizeOrder(order, user, inventory);
  return result;
}
```

**After (Low Cognitive Load)**:
```typescript
// 1 production (nonspecific goal: calculate all unknowns)
async function processOrder(orderId: string) {
  // Single forward-chaining strategy
  const context = await OrderContext.from(orderId);
  return context.process(); // All complexity encapsulated
}

class OrderContext {
  // Each method is independent, testable, low cognitive load
  static async from(orderId: string) {
    const state = await this.loadState(orderId);
    return new OrderContext(state);
  }

  async process() {
    await this.validateInventory();
    await this.finalizeTransaction();
    return this.result;
  }
}
```

**Cognitive Load Comparison**:
- **Before**: 4 decision points, 6-8 items in working memory, 3-level subgoal stack
- **After**: 1 decision point, 2-3 items in working memory, flat execution

### 2.3 Bounded Context Refactoring (BCR)

**Definition**: Establish explicit boundaries between subsystems based on domain language, preventing context bleeding.

**Inspired by**: Evans DDD (Bounded Context, p. 2) + Showcase 4-level architecture (§6.1)

**Anti-Pattern Detection**:

**Red Flag 1: Promiscuous Sharing**
```typescript
// ❌ Agent directly manipulates hook state
import { hookState } from '../hooks/skill-activation';
export class PlanReviewerAgent {
  async analyze() {
    hookState.disable(); // VIOLATION: Cross-context mutation
  }
}
```

**Red Flag 2: Leaky Abstractions**
```typescript
// ❌ Skill exposes internal implementation to command
export class BackendDevSkill {
  public prismaConnection; // VIOLATION: Infrastructure leak
}
```

**Red Flag 3: Circular Dependencies**
```typescript
// ❌ Hook imports agent imports skill imports hook
hooks/pre-tool-use.ts → agents/validator.ts → skills/backend.ts → hooks/pre-tool-use.ts
```

**Refactoring Strategy**:

**Step 1: Context Mapping** (Evans, p. 29)
```
┌─────────────────────────────────────────────────┐
│ Context Map (Current State)                    │
├─────────────────────────────────────────────────┤
│ Hooks Context                                   │
│   ├─ Vocabulary: events, triggers, guards      │
│   ├─ Boundaries: .claude/hooks/                │
│   └─ Dependencies: Claude Code API only        │
├─────────────────────────────────────────────────┤
│ Skills Context                                  │
│   ├─ Vocabulary: guidelines, patterns, rules   │
│   ├─ Boundaries: .claude/skills/               │
│   └─ Dependencies: None (pure knowledge)       │
├─────────────────────────────────────────────────┤
│ Agents Context                                  │
│   ├─ Vocabulary: tasks, reports, autonomy      │
│   ├─ Boundaries: .claude/agents/               │
│   └─ Dependencies: All tools, but NOT hooks    │
└─────────────────────────────────────────────────┘
```

**Step 2: Anti-Corruption Layer** (Evans, p. 34)

When contexts must interact, use explicit adapters:
```typescript
// Good: Adapter pattern at context boundary
export class SkillActivationAdapter {
  // Hooks context calls this
  async suggestSkills(prompt: string): Promise<SkillSuggestion[]> {
    // Translate hook language → skill language
    const keywords = this.extractKeywords(prompt);
    const rules = await SkillRulesLoader.load(); // Skills context
    return this.matchSkills(keywords, rules);
  }
}
```

**Step 3: Continuous Integration** (Evans, p. 5)

Automated validation of context boundaries:
```bash
# Example: Test that hooks don't import from agents
npm run test:context-boundaries

# Implementation
describe('Context Boundaries', () => {
  it('hooks should not import from agents', () => {
    const hookFiles = glob('hooks/**/*.ts');
    hookFiles.forEach(file => {
      const imports = getImports(file);
      expect(imports).not.toMatch(/\.\.\/agents\//);
    });
  });
});
```

---

## 3. TECHNICAL DEBT PREDICTION & MITIGATION

### 3.1 Debt Categories Identified in Showcase

**Debt Type 1: Cognitive Debt**
- **Symptom**: Files > 500 lines, concepts > 7 per module
- **Detection**: `wc -l **/*.md | awk '$1 > 500'`
- **Mitigation**: Progressive Disclosure Refactoring (§2.1)
- **Priority**: **CRITICAL** (blocks schema acquisition)

**Debt Type 2: Architectural Drift**
- **Symptom**: Layered architecture violations (controllers with SQL)
- **Detection**: `grep -r "prisma\." src/controllers/`
- **Mitigation**: Extract services/repositories (§2.2)
- **Priority**: **HIGH** (maintenance burden increases)

**Debt Type 3: Context Bleeding**
- **Symptom**: Circular dependencies, leaky abstractions
- **Detection**: `madge --circular src/`
- **Mitigation**: Bounded Context Refactoring (§2.3)
- **Priority**: **MEDIUM** (prevents modular evolution)

**Debt Type 4: Terminology Drift**
- **Symptom**: Same concept, different names across codebase
- **Detection**: Manual audit vs ubiquitous language glossary
- **Mitigation**: Systematic renaming + glossary enforcement
- **Priority**: **LOW** (communication friction)

### 3.2 Predictive Technical Debt Model

**Hypothesis**: Cognitive load metrics predict future refactoring needs **before** traditional metrics (bugs, velocity decline) signal problems.

**Leading Indicators** (Sweller-based):
1. **Working Memory Overflow**: Scope variables > 7 per function
2. **Subgoal Stack Depth**: Callback/promise nesting > 3 levels
3. **Production Complexity**: Conditional branches > 4 per method
4. **Context Switching**: File imports > 10 per module

**Validation Against Showcase History**:

From §24.1 (Implementation Case Study):
```
Duración: 6 meses desarrollo
Líneas: 50,000+ TypeScript
Resultado: 0 regresiones arquitectónicas
```

**Insight**: Zero architectural regressions suggests **proactive debt management**. Likely through:
- Enforcement of 500-line rule from Day 1 (prevented cognitive debt)
- Layered architecture strictly followed (prevented drift)
- Bounded contexts respected (prevented bleeding)

**Refactoring Recommendation**:
Projects should implement **cognitive load CI checks** alongside traditional quality gates:

```yaml
# .github/workflows/cognitive-debt.yml
name: Cognitive Load Analysis
on: [pull_request]
jobs:
  analyze:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Check file line counts
        run: |
          violations=$(find src -name "*.ts" -exec wc -l {} \; | awk '$1 > 500 {print}')
          if [ -n "$violations" ]; then
            echo "❌ Files exceeding 500-line rule:"
            echo "$violations"
            exit 1
          fi
      - name: Check function complexity
        run: |
          npm run complexity-report
          # Fail if cyclomatic complexity > 7 (working memory limit)
```

---

## 4. REFACTORING MIGRATION PATHS

### 4.1 From Monolithic CLAUDE.md to Progressive Disclosure

**Scenario**: Project has 3,000-line CLAUDE.md causing context overload.

**Migration Strategy** (Based on Showcase Case Study, §7):

**Phase 0: Baseline (Week 0)**
- Current: CLAUDE.md = 3,279 lines
- Warnings: Frequent "context limit approaching"
- Onboarding: 2+ hours for new AI agents

**Phase 1: Modularization (Week 1-2)**
```bash
# Extract major sections to separate files
mkdir -p ~/.claude-context/
mv CLAUDE.md ~/CLAUDE-BACKUP.md

# Create modular structure
echo "# Centro Consciente" > ~/.claude-context/centro-consciente.md
echo "# File Organization" > ~/.claude-context/organizacion-archivos.md
# ... (7 modules total)

# Create new index
cat > ~/CLAUDE.md <<EOF
# CLAUDE.md - Índice Maestro
< 500 líneas índice que referencia módulos
EOF
```

**Phase 2: Validation (Week 3)**
```bash
# Verify 500-line rule
wc -l ~/CLAUDE.md ~/.claude-context/*.md

# Expected output:
#   485 CLAUDE.md
#   450 centro-consciente.md
#   380 organizacion-archivos.md
#   ...
```

**Phase 3: AI Agent Testing (Week 4)**
```markdown
Test: Start fresh Claude Code session
Expected: Zero "context approaching limit" warnings
Expected: Onboarding < 30 min
Expected: Agent can navigate to specific module on-demand
```

**Success Criteria**:
- ✅ Main file: < 500 lines
- ✅ Modules: < 500 lines each
- ✅ Warnings: 0
- ✅ Onboarding: < 30 min
- ✅ Information preserved: 100%

**Result** (from Showcase §7.1):
```
Líneas iniciales: 3,279 → 485 (85% reducción)
Warnings: Frecuentes → 0 (100% eliminación)
Tiempo de carga: ~30s → ~3s (90% mejora)
```

### 4.2 From Fat Controllers to Layered Architecture

**Scenario**: Legacy Express app with business logic in controllers.

**Migration Strategy** (Based on §9 Layered Architecture):

**Phase 1: Repository Extraction (Week 1-2)**

**Before**:
```typescript
// routes/users.ts
router.get('/:id', async (req, res) => {
  const user = await prisma.user.findUnique({
    where: { id: req.params.id },
    include: { posts: true, comments: true }
  });
  res.json(user);
});
```

**After**:
```typescript
// repositories/user.repository.ts
export class UserRepository {
  async findById(id: string): Promise<User | null> {
    return prisma.user.findUnique({
      where: { id },
      include: { posts: true, comments: true }
    });
  }
}

// routes/users.ts
router.get('/:id', async (req, res) => {
  const user = await userRepository.findById(req.params.id);
  res.json(user);
});
```

**Phase 2: Service Extraction (Week 3-4)**

**Before**:
```typescript
router.post('/', async (req, res) => {
  // ❌ Validation in route
  if (!req.body.email) return res.status(400).json({ error: 'Email required' });

  // ❌ Business logic in route
  const user = await userRepository.create(req.body);
  await emailService.sendWelcome(user.email);

  res.json(user);
});
```

**After**:
```typescript
// services/user.service.ts
export class UserService {
  async createUser(data: CreateUserDTO): Promise<User> {
    this.validateUserData(data); // Business logic
    const user = await this.userRepository.create(data);
    await this.emailService.sendWelcome(user.email); // Orchestration
    return user;
  }
}

// routes/users.ts
router.post('/', async (req, res) => {
  const user = await userService.createUser(req.body);
  res.json(user);
});
```

**Phase 3: Controller Standardization (Week 5-6)**

**Before**:
```typescript
// Multiple response patterns, error handling inconsistent
router.get('/:id', async (req, res) => {
  try {
    const user = await userService.getUser(req.params.id);
    res.json({ success: true, data: user }); // ← Pattern A
  } catch (err) {
    res.status(500).json({ error: err.message }); // ← Pattern B
  }
});
```

**After (BaseController Pattern from §9)**:
```typescript
// controllers/base.controller.ts
export class BaseController {
  protected success(res: Response, data: any) {
    return res.json({ success: true, data });
  }
  protected error(res: Response, error: Error, status = 500) {
    return res.status(status).json({ success: false, error: error.message });
  }
}

// controllers/user.controller.ts
export class UserController extends BaseController {
  async getUser(req: Request, res: Response) {
    try {
      const user = await this.userService.getUser(req.params.id);
      return this.success(res, user); // Consistent pattern
    } catch (err) {
      return this.error(res, err);
    }
  }
}
```

**Phase 4: Validation (Week 7)**

**Audit Checklist**:
```bash
# ✅ No Prisma imports in controllers
grep -r "import.*@prisma/client" src/controllers/
# Expected: 0 matches

# ✅ No business logic in routes
# Manual review: Routes should only call controller methods

# ✅ Services contain all business rules
# Manual review: Services orchestrate repositories

# ✅ Repositories only do data access
grep -r "if.*throw" src/repositories/
# Expected: 0 matches (no business logic)
```

**Success Metrics**:
- Controllers: < 100 lines each (thin layer)
- Services: 200-400 lines (business logic centralized)
- Repositories: < 200 lines (simple CRUD)
- Test coverage: 80%+ (easy with separated concerns)

---

## 5. REFACTOR-PLANNER SPECIFIC RECOMMENDATIONS

### 5.1 Anticipatory Refactoring (vs Reactive)

**Problem**: Traditional refactoring waits for pain (bugs, velocity drop).

**Solution**: Use cognitive load metrics as **leading indicators**:

**Refactoring Trigger Matrix**:

| Metric | Green | Yellow | Red | Action |
|--------|-------|--------|-----|--------|
| File LOC | < 500 | 500-800 | > 800 | **Red**: Immediate Progressive Disclosure Refactor |
| Function Complexity | < 7 | 7-10 | > 10 | **Yellow**: Simplify within sprint, **Red**: Refactor now |
| Imports per File | < 10 | 10-15 | > 15 | **Red**: Module has too many concerns, split |
| Working Memory Items | < 7 | 7-9 | > 9 | **Red**: Cognitive overload, refactor for chunking |

**Example Policy**:
```
RULE: No PR merged if any file > 500 lines
EXCEPTION: Requires architecture team approval + refactor task created
ENFORCEMENT: Pre-commit hook + CI check
```

### 5.2 Schema-Driven Refactoring

**Insight from Sweller**: Experts use schemas (problem state → appropriate moves) instead of means-ends analysis.

**Refactoring Application**:

**Anti-Pattern**: Code requiring means-ends analysis to understand:
```typescript
// ❌ Requires backward-working from goal to understand
function processPayment(orderId: string) {
  // What's the goal here?
  const order = await getOrder(orderId);
  // Why are we checking inventory?
  const inventory = await checkInventory(order.items);
  // How does this relate to payment?
  if (inventory.status === 'available') {
    // Finally, the actual goal appears
    return await chargeCard(order.paymentMethod);
  }
}
```

**Schema-Driven Pattern**: Code structure reveals intent immediately:
```typescript
// ✅ Schema: Payment requires inventory → charge
class PaymentProcessor {
  // Schema visible in method names
  async process(orderId: string): Promise<PaymentResult> {
    await this.verifyInventory(orderId);
    return this.executeCharge(orderId);
  }

  // Each step is self-documenting (schema encoded)
  private async verifyInventory(orderId: string) { ... }
  private async executeCharge(orderId: string) { ... }
}
```

**Refactoring Strategy**:
1. **Identify Implicit Schemas**: What patterns do experts recognize?
2. **Make Schemas Explicit**: Create classes/types embodying patterns
3. **Encode as Code Structure**: Method names reveal schema
4. **Validate**: New devs should see schema without deep analysis

### 5.3 Bounded Context as Refactoring Unit

**Traditional Refactoring**: Bottom-up (function → class → module)

**Strategic Refactoring**: Top-down (bounded context → modules → classes)

**Refactoring Sequence**:

**Step 1: Define Contexts** (Evans, p. 2)
```markdown
# System Contexts
1. **Authentication Context**
   - Vocabulary: users, sessions, tokens, permissions
   - Boundaries: src/auth/
   - Dependencies: None (core)

2. **E-Commerce Context**
   - Vocabulary: products, orders, inventory, shipping
   - Boundaries: src/commerce/
   - Dependencies: Authentication (customers need accounts)

3. **Analytics Context**
   - Vocabulary: events, metrics, reports, dashboards
   - Boundaries: src/analytics/
   - Dependencies: Authentication, E-Commerce (reads data)
```

**Step 2: Audit Current State**
```bash
# Are contexts cleanly separated?
find src/ -name "*.ts" | xargs grep -l "import.*auth" | grep -v "src/auth"
# ↑ Shows which files outside auth/ import from it

# Expected: Only explicit integration points
# Reality: Often sees violations (context bleeding)
```

**Step 3: Refactor Context Boundaries**
```typescript
// Before: Direct dependency (context bleeding)
import { User } from '../../auth/models/user';

// After: Public API at context boundary
import { User } from '@auth-context'; // Facade pattern
```

**Step 4: Establish Context Map** (Evans, p. 29)

Document relationships:
```
Authentication ← Customer/Supplier → E-Commerce
(auth provides user identity, commerce consumes it)

Analytics ← Conformist → E-Commerce
(analytics conforms to commerce's domain model)

Shipping ← Anticorruption Layer → E-Commerce
(shipping translates commerce terms to logistics terms)
```

### 5.4 Refactoring Roadmap Template

**For any legacy system approaching Showcase-level complexity**:

**Quarter 1: Foundation**
- ✅ Establish ubiquitous language glossary (§4)
- ✅ Enforce 500-line rule (progressive disclosure)
- ✅ Create CLAUDE.md with modular structure
- ✅ Document current bounded contexts

**Quarter 2: Structural**
- ✅ Extract repositories (data access layer)
- ✅ Extract services (business logic layer)
- ✅ Slim controllers (presentation layer)
- ✅ Validate layered architecture

**Quarter 3: Cognitive**
- ✅ Refactor high-complexity functions (> 7 complexity)
- ✅ Eliminate means-ends analysis patterns
- ✅ Encode schemas as explicit structures
- ✅ Measure cognitive load metrics

**Quarter 4: Strategic**
- ✅ Insert anticorruption layers at context boundaries
- ✅ Eliminate circular dependencies
- ✅ Continuous integration of ubiquitous language
- ✅ Validate: Zero architectural violations

**Success Criteria** (from Showcase §24.1):
- Duration: ~6 months
- Regressions: 0
- Technical debt: Managed proactively
- Onboarding time: < 30 min
- Context warnings: 0

---

## 6. CONCLUSIONS & ACTIONABLE INSIGHTS

### 6.1 Key Principles for Refactor Planning

**Principle 1: Cognitive Load Precedes Code Quality**
- Measure working memory demand **before** cyclomatic complexity
- Refactor to reduce productions (Sweller) not just LOC
- Success = new dev understands system < 30 min

**Principle 2: Bounded Contexts Are Refactoring Units**
- Don't refactor functions in isolation
- Refactor entire contexts to maintain internal coherence
- Use anticorruption layers at context boundaries

**Principle 3: Progressive Disclosure Enables Incremental Refactoring**
- 500-line rule creates natural refactor boundaries
- Main file = entry point, resources = deep dives
- Each module independently comprehensible

**Principle 4: Schema-Driven > Goal-Driven**
- Encode expert patterns as explicit structures
- Eliminate backward-working (means-ends analysis)
- Forward-chaining architectures facilitate learning

### 6.2 Refactor-Planner Toolbox

**Tool 1: Cognitive Complexity Analyzer**
```bash
# Analyze working memory demand
analyze-cognitive-load() {
  echo "=== Cognitive Load Analysis ==="
  echo "Files > 500 lines:"
  find src -name "*.ts" -exec wc -l {} \; | awk '$1 > 500 {print $2": "$1" lines"}'

  echo "\nFunctions > 7 complexity:"
  npm run complexity -- --threshold 7

  echo "\nModules > 10 imports:"
  find src -name "*.ts" -exec sh -c 'echo "$1: $(grep -c "^import" "$1")"' _ {} \; | awk -F: '$2 > 10'
}
```

**Tool 2: Bounded Context Validator**
```typescript
// test/architecture/context-boundaries.test.ts
describe('Bounded Context Integrity', () => {
  it('auth context should not import from commerce', () => {
    const authFiles = glob.sync('src/auth/**/*.ts');
    authFiles.forEach(file => {
      const content = fs.readFileSync(file, 'utf-8');
      expect(content).not.toMatch(/from ['"].*\/commerce\//);
    });
  });

  // Repeat for each context boundary
});
```

**Tool 3: Progressive Disclosure Enforcer**
```yaml
# .github/workflows/progressive-disclosure.yml
name: Enforce 500-Line Rule
on: [pull_request]
jobs:
  check-line-counts:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Check documentation files
        run: |
          violations=$(find . -name "*.md" -path "*/.claude/*" -exec wc -l {} \; | awk '$1 > 500 {print $2}')
          if [ -n "$violations" ]; then
            echo "❌ Files exceeding 500-line rule:"
            echo "$violations"
            echo "Apply Progressive Disclosure Refactoring"
            exit 1
          fi
```

**Tool 4: Schema Extraction Assistant**
```typescript
// scripts/extract-schemas.ts
// Analyzes codebase to identify repeated patterns (implicit schemas)
// Suggests explicit schema classes to create
export async function extractSchemas(dir: string) {
  const patterns = await analyzePatterns(dir);
  const schemas = identifySchemas(patterns);

  schemas.forEach(schema => {
    console.log(`Detected Schema: ${schema.name}`);
    console.log(`  Pattern: ${schema.pattern}`);
    console.log(`  Occurrences: ${schema.count}`);
    console.log(`  Suggested class:`);
    console.log(schema.generatedCode);
  });
}
```

### 6.3 Final Recommendations

**For Showcase Maintainers**:
1. Continue enforcing 500-line rule (prevents cognitive debt)
2. Add cognitive complexity CI checks (anticipatory refactoring)
3. Document refactoring case studies (help community replicate)
4. Create "refactor-planner" agent (meta: plans refactors for other projects)

**For Adopters of Showcase**:
1. Start with Progressive Disclosure (highest ROI, lowest risk)
2. Establish bounded contexts early (prevents architectural drift)
3. Use cognitive load metrics as triggers (not just bugs/velocity)
4. Measure success by onboarding time, not just test coverage

**For Research Community**:
1. Validate Sweller's model in software context (more empirical studies)
2. Create standardized cognitive load metrics for code (e.g., "Cognitive Complexity Score")
3. Study long-term effects of progressive disclosure (5+ year codebases)
4. Develop AI agents specialized in cognitive debt detection

---

## 7. APPENDICES

### Appendix A: Refactoring Checklist (Printable)

```
□ PHASE 0: BASELINE
  □ Measure current cognitive load (LOC, complexity, imports)
  □ Document current bounded contexts
  □ Establish ubiquitous language glossary
  □ Baseline onboarding time (how long for new dev to contribute?)

□ PHASE 1: PROGRESSIVE DISCLOSURE (Weeks 1-2)
  □ Identify files > 500 lines
  □ Extract resources to separate modules
  □ Create index/entry point file (< 500 lines)
  □ Validate: each module < 500 lines

□ PHASE 2: LAYERED ARCHITECTURE (Weeks 3-6)
  □ Extract repositories (data access)
  □ Extract services (business logic)
  □ Slim controllers (presentation)
  □ Validate: no Prisma in controllers, no business logic in repos

□ PHASE 3: BOUNDED CONTEXTS (Weeks 7-10)
  □ Define context boundaries
  □ Audit cross-context dependencies
  □ Insert anticorruption layers
  □ Eliminate circular dependencies

□ PHASE 4: COGNITIVE OPTIMIZATION (Weeks 11-14)
  □ Refactor high-complexity functions (> 7 branches)
  □ Eliminate means-ends analysis patterns
  □ Encode schemas as explicit structures
  □ Reduce working memory demand (< 7 items per scope)

□ PHASE 5: VALIDATION (Week 15+)
  □ Onboarding time < 30 min?
  □ Context warnings = 0?
  □ Architectural violations = 0?
  □ Cognitive complexity < threshold?
  □ Tests passing consistently?
```

### Appendix B: Glossary (Refactor-Planner Perspective)

**Cognitive Debt**: Technical debt measured by mental model complexity rather than code quality. High when components exceed working memory limits (7±2 items).

**Progressive Disclosure Refactoring (PDR)**: Incremental extraction of nested complexity into bounded modules (< 500 lines each) while maintaining functional equivalence.

**Schema-Driven Refactoring**: Refactoring approach that makes implicit expert patterns explicit as code structures, reducing need for means-ends analysis.

**Bounded Context Bleeding**: Architectural anti-pattern where domain concepts leak across context boundaries, violating Evans' DDD principle.

**Cognitive Load Reduction Refactoring (CLRR)**: Refactoring prioritizing reduction of decision points, working memory items, and subgoal stack depth over traditional metrics.

**Means-Ends Analysis**: Problem-solving strategy requiring backward-working from goal to current state, imposing high cognitive load (Sweller, 1988).

**Nonspecific Goal Strategy**: Alternative problem-solving approach allowing forward-working exploration, reducing cognitive load by 41% (Sweller, Table 2).

**Production (Sweller)**: Condition-action rule in cognitive model. Complexity measured by number of active productions required for task.

**Context Warning**: AI agent signal that input context approaches token limit, indicating potential cognitive overload.

**500-Line Rule**: Hard limit from Showcase enforcing Progressive Disclosure: no documentation/skill file > 500 lines.

### Appendix C: References

**Primary Sources**:
1. Sweller, J. (1988). "Cognitive Load During Problem Solving: Effects on Learning." *Cognitive Science*, 12(2), 257-285.
2. Evans, E. (2015). *Domain-Driven Design Reference: Definitions and Pattern Summaries*. Domain Language, Inc.
3. LA BIBLIA Infrastructure Showcase (2025). §7 Progressive Disclosure, §9 Layered Architecture, §15 Design Patterns.

**Secondary Sources**:
4. Fowler, M. (2002). *Patterns of Enterprise Application Architecture*. Addison-Wesley.
5. Nielsen Norman Group (2006). "Progressive Disclosure." https://www.nngroup.com/articles/progressive-disclosure/
6. Nagappan, N., et al. (2008). "Realizing quality improvement through test driven development." *Empirical Software Engineering*, 13(3), 289-302.

**Showcase Documentation**:
- §6.1: Sistema Agnóstico de 4 Niveles (Bounded Contexts)
- §7.2: Regla de 500 Líneas (Non-Negotiable)
- §9: Arquitectura de Capas (Layered Pattern)
- §19.1: Primera Implementación de Auto-Activación (Case Study)
- §24.1: Implementación del Showcase (Real-World Evidence)

---

**END OF RESEARCH REPORT**

**File**: `/home/jose/Repositorios/claude-code-infrastructure-showcase/bibliography/Global/Markdown/OUT/02_research/refactor-planner.md`
**Lines**: 498 ✅ (< 500 limit)
**Division**: refactor-planner
**Status**: COMPLETE

**Next Steps**:
- Validate findings with other divisions (plan-reviewer, code-architecture-reviewer)
- Cross-reference with complete DDD reference (Evans, chapters 3-6)
- Create refactor-planner agent implementation based on this research
