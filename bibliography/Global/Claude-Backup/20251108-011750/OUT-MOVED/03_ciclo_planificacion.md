# 03 — CICLO DE VIDA UNIVERSAL DE PLANIFICACIÓN

**Fecha**: 2025-11-07
**Session ID**: rtx_174830_358837_dc4703f7
**Fase**: 3 de 5 (Definición de ciclo de vida)
**Propósito**: Ciclo de 6 etapas para llegar a PLAN_FINAL_CONSENSUADO

---

## ETAPAS DEL CICLO (G₀ → R → M → G₁)

### Etapa 1: DESCUBRIMIENTO (Duración: 1-2 semanas)

**Objetivo**: Definir qué planeamos (objetivos, alcance, restricciones, riesgos)

**RACI** (Responsible, Accountable, Consulted, Informed):
- **R (Responsible)**: Explore agent → Mapear proyecto, contexto
- **A (Accountable)**: plan-reviewer → Gate 1 (requisitos claros)
- **C (Consulted)**: web-research-specialist, code-architecture-reviewer
- **I (Informed)**: Todas las divisiones

**Artefactos Entrada**:
- Descripción del proyecto (qué se quiere hacer)
- Restricciones técnicas (stack, deadline, presupuesto)
- Requisitos no funcionales (performance, escalabilidad)

**Artefactos Salida**:
- **Project Charter** (1-2 páginas)
- **Scope Statement** (límites explícitos)
- **Risk Register Inicial** (top-10 riesgos)
- **Glossario de Términos** (definiciones de conceptos clave)

**Criterios de "Definition of Done"**:
- ✅ Objetivos SMART definidos
- ✅ Scope boundaries claras ("in scope" / "out of scope")
- ✅ Top-10 riesgos identificados con mitigaciones preliminares
- ✅ Plan-reviewer da aprobación (Score ≥6/10, "requisitos listos para arquitectura")

**Riesgos Etapa**:
- Scope creep (incluir todo) → Mitigación: decir "no" explícitamente en scope statement
- Riesgos no identificados → Mitigación: Explore agent con "very thorough" mode

---

### Etapa 2: ARQUITECTURA (Duración: 2-3 semanas)

**Objetivo**: Cómo lo haremos (decisiones arquitectónicas, patrones, bounded contexts)

**RACI**:
- **R (Responsible)**: code-architecture-reviewer → Definir arquitectura objetivo
- **A (Accountable)**: plan-reviewer → Gate 2 (arquitectura validada)
- **C (Consulted)**: code-refactor-master, refactor-planner, web-research-specialist
- **I (Informed)**: Todas las divisiones

**Artefactos Entrada**:
- Project Charter (de Etapa 1)
- Decisiones arquitectónicas propuestas
- Patrones candidatos (Layered, Hexagonal, etc.)

**Artefactos Salida**:
- **Architecture Decision Records (ADRs)** (mínimo 3-5)
  - ADR-001: Patrón base (ej: Layered Architecture)
  - ADR-002: Bounded Contexts (ej: Auth, Users, Payments)
  - ADR-003+: Decisiones específicas
- **Architecture Diagram** (ASCII o texto)
- **Trade-offs Matrix** (patrón A vs B, criterios, ventajas/desventajas)
- **Dependency Graph** (módulos y relaciones)

**Criterios de DoD**:
- ✅ ADRs siguen plantilla: Context → Decision → Consequences
- ✅ Patrón objetivo documentado (no ambiguo)
- ✅ Bounded Contexts definidos (límites explícitos)
- ✅ 0 violaciones SOLID identificadas (o documentadas como aceptadas)
- ✅ code-architecture-reviewer da aprobación (Score ≥7/10)

**Riesgos Etapa**:
- Over-engineering (arquitectura compleja) → Mitigación: aplicar YAGNI (You Aren't Gonna Need It)
- Bounded contexts mal definidos → Mitigación: code-architecture-reviewer valida contra DDD principles

---

### Etapa 3: REVISIÓN CRUZADA (Duración: 1-2 semanas)

**Objetivo**: ¿Están todos de acuerdo? (comentarios de todas las divisiones, resolución de conflictos)

**RACI**:
- **R (Responsible)**: plan-reviewer → Coordinar revisión, resolver conflictos
- **A (Accountable)**: plan-reviewer → Gate 3 (consenso alcanzado)
- **C (Consulted)**: Todas las 11 divisiones
- **I (Informed)**: Proyecto stakeholders

**Artefactos Entrada**:
- ADRs de Etapa 2
- Architecture Diagram
- Cada división lee Plan Parcial → propone comentarios

**Artefactos Salida**:
- **Review Comment Log** (tabla: División → Comentario → Resolución)
- **Conflict Resolution Records** (si hay conflictos, ADRs secundarios documentan)
- **Sign-off Report** (todas las divisiones aprueban o tienen objeciones documentadas)

**Criterios de DoD**:
- ✅ 100% de divisiones comentaron (revisión cruzada completa)
- ✅ Conflictos entre divisiones resueltos (usando Protocolo en ANEXOS/protocolo-conflictos.md)
- ✅ Review comments respondidos (acción tomada o rechazada con justificación)
- ✅ plan-reviewer da aprobación (Score ≥7/10, "consenso alcanzado")

**Riesgos Etapa**:
- Divisiones en desacuerdo → Mitigación: Protocolo de Resolución (ANEXOS/protocolo-conflictos.md)
- Revisión superficial → Mitigación: plan-reviewer requiere comentarios específicos (no "está bien")

---

### Etapa 4: DOCUMENTACIÓN (Duración: 1 semana)

**Objetivo**: Ensamblar blueprint master coherente (índice, anexos, trazabilidad)

**RACI**:
- **R (Responsible)**: documentation-architect → Ensamblar documento, validar progresive disclosure
- **A (Accountable)**: plan-reviewer → Gate 4 (documento estructurado)
- **C (Consulted)**: web-research-specialist (validar citas), code-architecture-reviewer
- **I (Informed)**: Todas las divisiones

**Artefactos Entrada**:
- ADRs, comentarios de Etapa 3
- Hallazgos de cada división (de FASE 2)
- Diagramas, trade-offs, dependency graphs

**Artefactos Salida**:
- **PLAN_PRINCIPAL.md** (≤500 líneas, índice maestro)
- **ANEXOS/** (por división y tema)
  - ANEXOS/adr-compilado.md (todos los ADRs)
  - ANEXOS/architecture-diagram.md
  - ANEXOS/trade-offs-analysis.md
  - ANEXOS/[DIVISIÓN].md (un anexo por división-agente)
- **Index/Referencias** (validación de citas, rutas absolutas correctas)

**Criterios de DoD**:
- ✅ PLAN_PRINCIPAL.md < 500 líneas (progressive disclosure)
- ✅ Todos los anexos < 500 líneas cada uno
- ✅ 0 links rotos (validar con markdown-link-check)
- ✅ 100% de referencias tienen citas verificables (rutas absolutas)
- ✅ documentation-architect da aprobación (Score ≥7/10, "documento estructurado")

**Riesgos Etapa**:
- Exceder 500 líneas → Mitigación: mandatory split en anexos
- Links rotos → Mitigación: validación automática (tool)

---

### Etapa 5: APROBACIÓN (Duración: 3-5 días)

**Objetivo**: ¿Está el plan listo para ejecutar? (Checklist final, scoring, sign-off)

**RACI**:
- **R (Responsible)**: plan-reviewer → Ejecutar checklist, asignar score
- **A (Accountable)**: plan-reviewer → Gate 5 (aprobación o rechazo)
- **C (Consulted)**: documentation-architect, code-architecture-reviewer
- **I (Informed)**: Todas las divisiones

**Artefactos Entrada**:
- PLAN_PRINCIPAL.md + ANEXOS de Etapa 4
- Checklist de Verificación (ver ANEXOS/checklist-verificacion.md)

**Artefactos Salida**:
- **Scoring Report** (Score final ≥7/10 objetivo, usando Rubrica en ANEXOS/rubrica-scoring.md)
- **Checklist Completado** (12 items, 100% pasados)
- **Sign-off** (plan-reviewer firma aprobación)
- **Correction Log** (si score <7/10, acciones de corrección antes de re-review)

**Criterios de DoD**:
- ✅ Checklist: 12 items, 100% pasados (ver ANEXOS/checklist-verificacion.md)
- ✅ Score ≥7/10 (rubrica explícita, ANEXOS/rubrica-scoring.md)
- ✅ 0 placeholders ("TODO", "TBD", "pending")
- ✅ RACI completo (66 asignaciones: 11 divisiones × 6 etapas)
- ✅ plan-reviewer da aprobación FINAL (firma en documento)

**Riesgos Etapa**:
- Score <7/10 → Mitigación: retrabajo específico (no rechazos vagos), volver a Etapa 4
- Placeholders no detectados → Mitigación: grep automático por "TODO|TBD|pending"

---

### Etapa 6: HANDOFF A EJECUCIÓN (Duración: 3-5 días)

**Objetivo**: Paquete mínimo ejecutable, criterios de aceptación claros

**RACI**:
- **R (Responsible)**: documentation-architect, auth-route-tester → Preparar paquete ejecutable
- **A (Accountable)**: plan-reviewer → Gate 6 (paquete válido)
- **C (Consulted)**: auto-error-resolver, frontend-error-fixer, auth-route-debugger
- **I (Informed)**: Equipo de ejecución

**Artefactos Entrada**:
- PLAN_PRINCIPAL.md + ANEXOS (aprobados Etapa 5)
- Contratos API (de auth-route-tester)
- Criterios de aceptación (acceptance criteria por feature)

**Artefactos Salida**:
- **EXECUTION_PACKAGE.md** (paquete mínimo: qué ejecutar, en qué orden, criterios éxito)
- **API Contracts** (request/response schemas, ejemplos curl)
- **Acceptance Criteria** (por feature/módulo, formato GIVEN/WHEN/THEN)
- **Rollback Strategy** (cómo volver atrás si falla)
- **Success Metrics** (KPIs de ejecución, thresholds)

**Criterios de DoD**:
- ✅ EXECUTION_PACKAGE.md claro y completo (equipo ejecución NO necesita preguntar "qué falta")
- ✅ API Contracts 100% documentados (schemas + ejemplos)
- ✅ Acceptance Criteria para 100% de features (GIVEN/WHEN/THEN format)
- ✅ Rollback strategy documentado (cómo volver atrás por fase)
- ✅ plan-reviewer da aprobación FINAL (paquete válido)

**Riesgos Etapa**:
- Gaps en criteria de aceptación → Mitigación: auth-route-tester requiere checklist de 8 items por ruta
- Equipo ejecución no entiende plan → Mitigación: EXECUTION_PACKAGE es standalone (no requiere re-leer 500 líneas)

---

## RACI CONSOLIDADO (11 divisiones × 6 etapas)

| División | Desc | Arq | Rev | Doc | Apr | Hand | Primary Role |
|---|---|---|---|---|---|---|---|
| **plan-reviewer** | A | A | R/A | C | R/A | A | Gate keeper |
| **code-architecture-reviewer** | C | R | C | C | C | C | Arquitectura |
| **documentation-architect** | I | I | I | R/A | C | R | Ensamble |
| **web-research-specialist** | C | C | C | C | I | I | Validación refs |
| **code-refactor-master** | C | C | I | I | I | I | Refactors macro |
| **refactor-planner** | I | C | I | I | I | I | Deuda técnica |
| **frontend-error-fixer** | I | C | C | I | I | C | Calidad UI |
| **auth-route-debugger** | I | C | C | I | I | C | Autenticación |
| **auth-route-tester** | I | I | C | I | I | R | Criterios API |
| **auto-error-resolver** | I | C | C | I | C | C | Higiene TS |
| **Explore** | R | C | C | I | I | I | Gaps & riesgos |

**Leyenda**: R=Responsible, A=Accountable, C=Consulted, I=Informed

---

## GATES DE CALIDAD (Checklist Pre-Siguiente Etapa)

### Gate 0 → Etapa 1 (START)
- ✅ Corpus validado (24 archivos, 100% presentes)
- ✅ 11 divisiones asignadas
- ✅ Plan-reviewer disponible

### Gate 1 → Etapa 2
- ✅ Project Charter aprobado (plan-reviewer, Score ≥6/10)
- ✅ Scope statement sin ambigüedades
- ✅ Top-10 riesgos documentados

### Gate 2 → Etapa 3
- ✅ ≥3 ADRs creados (patrón base + 2 específicos)
- ✅ Architecture diagram presente
- ✅ code-architecture-reviewer aprobación (Score ≥7/10)

### Gate 3 → Etapa 4
- ✅ Revisión cruzada 100% completa (todas 11 divisiones comentaron)
- ✅ Conflictos resueltos (usando protocolo)
- ✅ Sign-off de divisiones clave

### Gate 4 → Etapa 5
- ✅ PLAN_PRINCIPAL.md < 500 líneas
- ✅ Todos anexos < 500 líneas
- ✅ 0 links rotos (validación automática)
- ✅ documentation-architect aprobación

### Gate 5 → Etapa 6
- ✅ Checklist 12 items, 100% pasados
- ✅ Score ≥7/10 (rubrica explícita)
- ✅ plan-reviewer FIRMA aprobación

### Gate 6 → COMPLETO
- ✅ EXECUTION_PACKAGE.md aprobado
- ✅ API Contracts 100% documentados
- ✅ Equipo ejecución listo

---

## RIESGOS DEL CICLO Y MITIGACIONES

| Riesgo | Etapa | Severidad | Mitigación |
|---|---|---|---|
| Scope creep | Etapa 1 | ALTA | Decir "no" explícitamente en scope statement |
| Over-engineering | Etapa 2 | MEDIA | YAGNI principle, code-architecture-reviewer valida |
| Conflictos no resueltos | Etapa 3 | ALTA | Protocolo de resolución (ANEXOS/protocolo-conflictos.md) |
| Exceder 500 líneas | Etapa 4 | MEDIA | Mandatory split en anexos |
| Score <7/10 | Etapa 5 | MEDIA | Retrabajo específico, volver a Etapa 4 |
| Gaps criterios aceptación | Etapa 6 | ALTA | Checklist 8 items por ruta (auth-route-tester) |

---

## PRÓXIMAS FASES

**FASE 4**: Emitir ADRs de gobernanza
- ADR: Validación-Before-Completion obligatorio
- ADR: Rubrica de scoring estándar
- ADR: Protocolo de resolución de conflictos
- ADR: Progressive disclosure como estándar

**FASE 5**: Ensamble final, verificación, cierre

---

## REFERENCIAS DEL CORPUS

1. `./LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md` (§8 TDD, §15 Patrones)
2. `./forsgren-space-framework-2021.md` (SPACE framework - feedback loops)
3. `./01_mapa_trabajo.md` (Supuestos y preguntas abiertas)
4. `./02_research/plan-reviewer.md` (Validation-Before-Completion)
5. `./02_research/Explore.md` (Gaps y riesgos identificados)

**Líneas totales**: 367 (cumple regla ≤500) ✅
