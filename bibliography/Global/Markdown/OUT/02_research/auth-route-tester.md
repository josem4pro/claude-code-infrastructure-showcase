# INVESTIGACIÓN: auth-route-tester
## Contratos y Criterios de Aceptación para Rutas y Endpoints Protegidos

**Fecha**: 2025-11-07
**División**: auth-route-tester (A9)
**Investigador**: Claude Code - auth-route-tester division
**Corpus**: LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md, CLAUDE_INTEGRATION_GUIDE.md
**Enfoque**: Contratos de API, criterios de aceptación, test scenarios
**Líneas**: ≤500

---

## §1 — PROPÓSITO Y ALCANCE

### 1.1 Responsabilidad Central

**Propósito**: Definir contratos y criterios de aceptación para rutas y endpoints protegidos.

**Límites**:
- ✅ Define contratos API, criterios aceptación, test scenarios
- ❌ NO ejecuta tests (fase ejecución), solo planifica QUÉ validar

**Diferencia con agent (§13.9)**:
- **Agent**: Ejecuta tests, verifica DB, debuggea
- **División**: Define QUÉ testear y CÓMO validar éxito

### 1.2 Artefactos

**Consume**: Specs de rutas, matriz permisos (auth-route-debugger), schemas TypeScript/Zod
**Produce**: Contratos API, criterios aceptación, test scenarios, ejemplos reproducibles

---

## §2 — ANATOMÍA DE CONTRATO API

### 2.1 Estructura Completa

```markdown
## CONTRATO: POST /api/workflow/start

### Request Schema
```typescript
interface WorkflowStartRequest {
  workflowCode: string;
  entityType: "Submission" | "Project" | "User";
  entityID: number;
  initiatorUserID?: string;
}
```

### Response (200 Success)
```typescript
interface WorkflowStartResponse {
  success: true;
  workflowInstanceID: number;
  currentStepID: number;
  message: string;
}
```

### Response (400 Error)
```typescript
interface WorkflowStartError {
  success: false;
  error: string;
  code: "INVALID_WORKFLOW" | "ENTITY_NOT_FOUND" | "VALIDATION_ERROR";
}
```

### Autenticación
- Método: JWT cookie (`refresh_token`)
- Roles: `admin`, `operations`

### Ejemplo curl
```bash
POST http://localhost:3002/api/workflow/start
Cookie: refresh_token=eyJhbGci...
{"workflowCode":"DHS_CLOSEOUT","entityType":"Submission","entityID":123}
```
```

### 2.2 Elementos No-Negociables

1. **Request Schema** con tipos TypeScript (required vs optional explícitos)
2. **Response Schema** para TODOS códigos HTTP (200, 400, 401, 403, 500)
3. **Autenticación/Autorización** (método, roles, comportamiento)
4. **Ejemplos ejecutables** (curl reproducible)

---

## §3 — CRITERIOS DE ACEPTACIÓN

### 3.1 Plantilla Estandarizada

```markdown
## CRITERIOS: POST /api/workflow/start

### Happy Path
- GIVEN: Usuario autenticado rol `operations`
- AND: workflowCode existe, entityID válido
- WHEN: POST con payload válido
- THEN: Response 200 con `workflowInstanceID`
- AND: Registro en `WorkflowInstance`
- AND: Registro en `WorkflowStepInstance`
- AND: Notificación en `WorkflowNotification`

### Error Case 1: No Autenticado
- GIVEN: Sin cookie `refresh_token`
- WHEN: POST
- THEN: 401 "Authentication required"
- AND: NO writes en DB

### Error Case 2: Sin Permisos
- GIVEN: Usuario rol `viewer`
- THEN: 403 "Insufficient permissions"

### Error Case 3: Workflow Inexistente
- GIVEN: workflowCode inválido
- THEN: 400 "Workflow not found"

### Verificación DB
- Tabla `WorkflowInstance`: validar `workflowCode`, `status='active'`, `createdBy`
- Tabla `WorkflowStepInstance`: validar `stepCode`, `status='pending'`

### Performance
- Response time < 500ms (P95)
- DB queries ≤ 3
```

### 3.2 KPIs Calidad

| KPI | Objetivo |
|-----|----------|
| Cobertura endpoints | 100% con contrato |
| Cobertura error cases | ≥4 casos por ruta |
| Precisión schemas | 0 discrepancias TypeScript/Zod |
| Ejemplos reproducibles | 100% ejecutables sin modificación |

---

## §4 — MATRIZ TEST SCENARIOS

### 4.1 5 Dimensiones

```
DIMENSIÓN 1: AUTENTICACIÓN
  ├─ No autenticado (sin token)
  ├─ Token inválido/expirado
  └─ Autenticado válido

DIMENSIÓN 2: AUTORIZACIÓN
  ├─ Sin permisos (rol insuficiente)
  └─ Con permisos (admin/owner)

DIMENSIÓN 3: VALIDACIÓN PAYLOAD
  ├─ Payload vacío/inválido
  ├─ Campos required faltantes
  └─ Payload válido

DIMENSIÓN 4: INTEGRIDAD DATOS
  ├─ Referencias inexistentes (FK)
  └─ Datos válidos

DIMENSIÓN 5: SIDE EFFECTS
  ├─ DB changes (INSERT/UPDATE)
  ├─ Notificaciones
  └─ Eventos (action queue)
```

### 4.2 Tabla Mínima (5 scenarios)

| ID | Auth | Authz | Payload | Esperado | DB |
|----|------|-------|---------|----------|-----|
| TS-001 | ❌ | N/A | Válido | 401 | No writes |
| TS-002 | ✅ | ❌ | Válido | 403 | No writes |
| TS-003 | ✅ | ✅ | ❌ | 400 | No writes |
| TS-004 | ✅ | ✅ | ✅ | 200 | Verified |
| TS-005 | ✅ | ✅ | Edge | 200/400 | Verified |

---

## §5 — INTEGRACIÓN DIVISIONES

### 5.1 auth-route-debugger (A8)

**Provee**: Matriz permisos (roles × recursos), flujos error auth (401/403)
**auth-route-tester transforma en**: Test scenarios TS-001/TS-002, criterios auth/authz
**Artefacto compartido**: `auth-matrix.md`

### 5.2 plan-reviewer (A2)

**Checklist validación**:
- □ Contrato completo (request/response schemas)
- □ Criterios con happy path + ≥4 error cases
- □ Ejemplos curl reproducibles
- □ Schemas TypeScript/Zod sincronizados
- □ Verificaciones DB especificadas
- □ Matriz scenarios cubre 5 dimensiones

**Score**: 10 (completo), 7 (suficiente), <5 (rechazo)

### 5.3 auto-error-resolver (A10)

**Coordina validación TypeScript/Zod**:
- TypeScript strict mode
- Zod schemas sincronizados
- ESLint: `explicit-function-return-type`

---

## §6 — PATRONES TESTING (skill route-tester)

### 6.1 test-auth-route.js

```bash
# GET
node scripts/test-auth-route.js http://localhost:3002/api/workflow/123

# POST
node scripts/test-auth-route.js \
    http://localhost:3002/api/workflow/start \
    POST \
    '{"workflowCode":"DHS_CLOSEOUT","entityType":"Submission","entityID":123}'
```

**Automatiza**: Keycloak token, JWT signing, cookie header, curl reproducible

### 6.2 Mock Auth (Desarrollo)

```bash
# .env
MOCK_AUTH=true
MOCK_USER_ID=test-user
MOCK_USER_ROLES=admin,operations

# Testing
curl -H "X-Mock-Auth: true" -H "X-Mock-User: test-user" -H "X-Mock-Roles: admin" ...
```

**⚠️ Limitación**: Solo `NODE_ENV=development|test`, NUNCA producción

### 6.3 Verificación DB

```bash
docker exec -i local-mysql mysql -u root -ppassword1 blog_dev
mysql> SELECT * FROM WorkflowInstance WHERE id = 456;
```

**En contrato especificar**: Tabla, query, campos a validar

---

## §7 — ANTI-PATRONES Y RIESGOS

### 7.1 Anti-Patrones

**1. Sin versioning**
- ❌ `POST /api/workflow/start`
- ✅ `POST /api/v1/workflow/start`

**2. Solo happy path**
- ❌ 1 scenario (payload válido → 200)
- ✅ 5 scenarios (happy + 4 error cases)

**3. Schemas desactualizados**
- ❌ TypeScript interface sin campo añadido en implementación
- ✅ TypeScript + Zod sincronizados

### 7.2 Riesgos (KPIs Mitigación)

| Riesgo | Impacto | Mitigación | KPI |
|--------|---------|------------|-----|
| Contratos ambiguos | Alto | Especificar todos campos | 100% |
| Error cases no contemplados | Alto | Matriz 5 dimensiones | ≥5 scenarios |
| Schemas desincronizados | Medio | TypeScript strict + Zod | 0 discrepancias |

---

## §8 — RECOMENDACIONES

### 8.1 Contract-First Development

**Flujo**:
1. auth-route-tester define contrato
2. plan-reviewer valida
3. code-architecture-reviewer valida diseño
4. Implementación (contrato = spec)
5. auth-route-tester agent ejecuta tests

**Ventajas**: Claridad expectativas, evita rework, paralelización frontend

### 8.2 Zod Validation

```typescript
import { z } from 'zod';

const workflowStartSchema = z.object({
  workflowCode: z.string().min(1),
  entityType: z.enum(["Submission", "Project", "User"]),
  entityID: z.number().int().positive(),
});

// Controller
const validated = workflowStartSchema.parse(req.body);
```

**Integración**: TypeScript interface (estructura) + Zod schema (runtime validation)

### 8.3 Ejemplos Ejecutables

```bash
curl -X POST http://localhost:3002/api/workflow/start \
  -H "Content-Type: application/json" \
  -b "refresh_token=eyJhbGci..." \
  -d '{"workflowCode":"DHS_CLOSEOUT","entityType":"Submission","entityID":123}'
```

**Ventajas**: Copy/paste testing, documentación auto-actualizable, validación contratos

---

## §9 — STACK Y ADAPTABILIDAD

### 9.1 Compatibilidad

**Stack asumido**: Node.js + Express + Prisma + TypeScript, JWT cookies, Zod, MySQL/PostgreSQL

**Otros stacks**:

| Componente | Django/Flask | Go/Gin | Spring Boot |
|------------|--------------|--------|-------------|
| Contratos API | ✅ Genérico | ✅ Genérico | ✅ Genérico |
| Schemas TypeScript | ⚠️ Python types | ⚠️ Go structs | ⚠️ Java POJOs |
| Zod validation | ⚠️ Pydantic | ⚠️ validator | ⚠️ Bean Validation |
| test-auth-route.js | ⚠️ Adaptar | ⚠️ Adaptar | ⚠️ Adaptar |

### 9.2 Framework-Agnostic

**Transfiere**:
- ✅ Estructura contrato (request/response + ejemplos)
- ✅ Criterios aceptación (GIVEN/WHEN/THEN)
- ✅ Matriz scenarios (5 dimensiones)
- ✅ Verificación DB (SQL portables)
- ✅ Anti-patrones

**NO transfiere**:
- ❌ Zod → Framework específico
- ❌ Express middleware → Framework específico
- ❌ Prisma → ORM específico

---

## §10 — CHECKLIST ENTREGABLES

### 10.1 Artefactos Mínimos (8 items)

```markdown
□ Contrato API (request/response TypeScript)
□ Zod schema sincronizado
□ Criterios aceptación (happy + ≥4 error cases)
□ Matriz scenarios (≥5)
□ Ejemplo curl reproducible
□ Verificaciones DB (tablas, queries, campos)
□ Matriz permisos (roles)
□ Performance criteria (response time, queries)
```

**Score completitud**: 8/8 = 100%, objetivo ≥87.5% (7/8)

### 10.2 Validación plan-reviewer

**Checklist**:
- □ Plantilla §2.1
- □ TypeScript + Zod sincronizados §8.2
- □ GIVEN/WHEN/THEN §3.1
- □ 5 dimensiones §4.1
- □ Curl reproducible §8.3
- □ DB especificado §6.3
- □ Anti-patrones evitados §7.1
- □ Completitud ≥87.5%

**Score**: ≥7/10 aprobación, <5/10 rechazo

---

## §11 — REFERENCIAS

**LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md**:
- §1.1: Tabla (S4 route-tester, A9 auth-route-tester)
- §12.4: Skill route-tester
- §13.9: Agent auth-route-tester
- §8: TDD Validation-Before-Completion
- §9: Layered Architecture

**CLAUDE_INTEGRATION_GUIDE.md**:
- route-tester skill (JWT cookies)
- Tech Stack Compatibility
- Framework-agnostic patterns

**fichas-divisiones.md (ANEXO A)**:
- A9: auth-route-tester (propósito, interfaces, KPIs)
- A8: auth-route-debugger (matriz permisos)
- A10: auto-error-resolver (reglas TypeScript)

**Papers**:
- Nagappan (2008): TDD reduce defect density 60-90%
- Evans (2015): DDD Reference (bounded contexts)
- Fowler (2002): Layered architecture patterns

---

## §12 — CONCLUSIONES

### 12.1 Hallazgos

1. Contratos API agnósticos de stack (estructura transfiere)
2. Mínimo 5 scenarios requeridos (1 happy + 4 error)
3. TypeScript + Zod sincronización crítica
4. test-auth-route.js automatiza JWT cookies
5. Contract-first evita rework

### 12.2 Gaps

**Falta**:
- ❌ Plantillas GraphQL/tRPC (solo REST)
- ❌ Estrategia versioning API (mencionado, no detallado)
- ❌ Testing file uploads autenticados

**Recomendaciones futuras**:
- Plantilla GraphQL (schema SDL + resolvers)
- Doc versioning (URL path vs header)
- Detalle multipart/form-data auth

### 12.3 Dependencias

**BLOCKER**: auth-route-debugger (matriz permisos)
**GATING**: plan-reviewer (validación)
**QUALITY**: auto-error-resolver (reglas TypeScript)

**Entregables para**:
- → plan-reviewer: Contratos para revisión
- → documentation-architect: Sección API contracts
- → auth-route-tester agent: Criterios para ejecutar tests

---

**FIN INVESTIGACIÓN auth-route-tester**
**Líneas**: 498 (cumple ≤500)
**Estado**: COMPLETO - Listo para plan-reviewer
**Próximo**: Integrar con auth-route-debugger (matriz permisos)
