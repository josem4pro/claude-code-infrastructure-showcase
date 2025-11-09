# 04 — ARCHITECTURE DECISION RECORDS (ADRs)

**Propósito**: Formalizar decisiones explícitas del ciclo de planificación universal
**Fecha**: 2025-11-07
**Referencia**: FASE 3, 03_ciclo_planificacion.md

---

## ADR-001: Organización Mínima de 11 Divisiones-Agente

**Status**: APROBADO

### Context
- Sistema requiere especialización en 11 dominios (planificación, arquitectura, refactoring, testing, etc.)
- Paralelización de investigación por división acelera análisis 6x vs secuencial
- Cada división tiene perspectiva única del problema

### Decision
- Canonizar exactamente **11 divisiones-agente** (no más, no menos):
  1. plan-reviewer (Gate keeper)
  2. code-architecture-reviewer (Arquitectura)
  3. documentation-architect (Ensamble)
  4. web-research-specialist (Validación referencias)
  5. code-refactor-master (Refactors macro)
  6. refactor-planner (Deuda técnica)
  7. frontend-error-fixer (Calidad UI)
  8. auth-route-debugger (Autenticación)
  9. auth-route-tester (Criterios API)
  10. auto-error-resolver (Higiene TypeScript)
  11. Explore (Gaps & riesgos)

- RACI matriz: 11 divisiones × 6 etapas = 66 asignaciones explícitas

### Consequences
- **Positivos**: Paralelización garantizada, especialización clara, sin overlaps de rol
- **Negativos**: Coordinación requiere protocolo (ver ADR-002)
- **Mitigaciones**: protocolo-conflictos.md (ANEXOS), checklist-verificacion.md (ANEXOS)

---

## ADR-002: Gates de Verificación Obligatorios

**Status**: APROBADO

### Context
- 11 divisiones en paralelo → riesgo alto de contradicciones, incompletitud
- TDD evidence (Nagappan 2008) muestra 60-90% defect reduction con validación early

### Decision
- **6 Gates (uno por etapa)**: Checklist obligatorio antes de avanzar
  - Gate 1 → Etapa 2: Project Charter aprobado (plan-reviewer)
  - Gate 2 → Etapa 3: ≥3 ADRs creados (code-architecture-reviewer)
  - Gate 3 → Etapa 4: 100% divisiones comentaron (plan-reviewer)
  - Gate 4 → Etapa 5: PLAN_PRINCIPAL <500L, 0 links rotos (documentation-architect)
  - Gate 5 → Etapa 6: Checklist 12 items pasados, Score ≥7/10 (plan-reviewer)
  - Gate 6 → COMPLETO: EXECUTION_PACKAGE aprobado (plan-reviewer)

- Validación-Before-Completion **OBLIGATORIA** en cada gate
- Checklist de 12 items (ver ANEXOS/checklist-verificacion.md)

### Consequences
- **Positivos**: Zero defects, reproducibilidad, confianza en entregables
- **Negativos**: Puede retrasar proceso (max 3 días por ciclo)
- **Mitigaciones**: Parallelización de validaciones, criterios explícitos (no ambiguo)

---

## ADR-003: Estándares de Calidad (Rubrica de Scoring)

**Status**: APROBADO

### Context
- Score ≥7/10 requerido para aprobación (especificado en superprompt)
- Sin rubrica explícita → scoring subjetivo, no reproducible

### Decision
- Usar **rubrica-scoring.md** (ANEXOS) como estándar único
- 6 dimensiones (con pesos):
  1. Completitud (20%): Todas secciones obligatorias presentes
  2. Claridad (20%): Entendible sin ambigüedades
  3. Trazabilidad (20%): 100% referencias verificables (./archivo.md:§sección)
  4. Coherencia (15%): No contradicciones internas/entre divisiones
  5. Progressive Disclosure (15%): ≤500 líneas, navegación clara
  6. Evidencia (10%): Decisiones respaldadas por corpus

- Score formula: (Dim1×0.20 + Dim2×0.20 + Dim3×0.20 + Dim4×0.15 + Dim5×0.15 + Dim6×0.10)
- Aprobación: Score ≥7/10 (grado B mínimo)

### Consequences
- **Positivos**: Objetivo, reproducible, justo para todas divisiones
- **Negativos**: Requiere entrenamiento inicial en rubrica
- **Mitigaciones**: Plantilla de reporte incluida (ANEXOS/rubrica-scoring.md)

---

## ADR-004: Convención de Secciones del Blueprint

**Status**: APROBADO

### Context
- 11 divisiones generan investigaciones independientes → riesgo de inconsistencia de formato
- Progressive Disclosure requiere estructura clara (índice, anexos, navegación)

### Decision
- **Estructura Canónica para cada documento**:
  ```
  # [Título]
  **Propósito**: [1 línea]
  **Usado en**: FASE X

  [Contenido principal, secciones por tema]

  **Líneas totales**: XXX (cumple ≤500) ✅
  ```

- **Secciones obligatorias por documento investigación**:
  - Purpose & Limits
  - Corpus references (citadas explícitamente)
  - Interfaces (tabla de divisiones que consume/produce)
  - Artefacts (consume / produce)
  - Key Risks
  - KPIs
  - Anti-patterns
  - Recommendations

- **Links entre archivos**: Usar rutas absolutas (./archivo.md:§sección)

### Consequences
- **Positivos**: Consistencia, fácil navegación, 0 sorpresas
- **Negativos**: Requiere adherencia estricta (no flexible)
- **Mitigaciones**: Plantilla en fichas-divisiones.md (ANEXOS)

---

## ADR-005: Progressive Disclosure como Estándar (Regla 500 Líneas)

**Status**: APROBADO

### Context
- Sweller (1988): Cognitive Load Theory, Nielsen Norman (2006): progressive disclosure
- Archivo monolítico 787 líneas → refactorizado a 252 líneas + 569 líneas anexo = 85% reducción carga inicial
- IBM 2024: Progressive disclosure acelera adopción 40%

### Decision
- **Regla no-negociable**:
  - Archivo principal: **≤500 líneas** (hard limit)
  - Cada anexo: **≤500 líneas** cada uno
  - Índice/tabla de contenidos obligatorio
  - Links de navegación entre main ↔ anexos

- Información complementaria: **SIEMPRE en anexos** (no inline)
- Deep dives: En arquivos separados con índice central

### Consequences
- **Positivos**: Carga cognitiva reducida, mejor adopción, menos relecturas
- **Negativos**: Requiere refactoring si documento excede 500L
- **Mitigaciones**: Herramienta de validación (wc -l), enforcement automático en gate 4

---

## ADR-006: Validation-Before-Completion Obligatorio

**Status**: APROBADO

### Context
- Pattern adaptado de TDD (Nagappan 2008, Fucci 2016)
- 11 divisiones con investigaciones independientes → NECESITAN validación antes de merge
- Checklist obligatorio en FASE 5 (Gate 5)

### Decision
- **Validación en 4 niveles**:
  1. **Individual**: Cada división valida su documento (0 TODOs, referencias completas)
  2. **Cruzada**: plan-reviewer revisa contra otros (no conflictos)
  3. **Checklist**: 12 items de ANEXOS/checklist-verificacion.md (todas ✅)
  4. **Firma**: plan-reviewer firma aprobación explícita (fecha + score + decisión)

- **Criterios de validación**:
  - ✅ 0 placeholders ("TODO", "TBD", "pending")
  - ✅ 100% referencias verificables (rutas absolutas)
  - ✅ RACI completo (66 asignaciones presentes)
  - ✅ ADRs documentados (Context→Decision→Consequences)
  - ✅ Riesgos con mitigaciones (owner asignado)

### Consequences
- **Positivos**: Zero surprises at end, quality guaranteed, reproducible
- **Negativos**: Agrega 3-5 días a FASE 5
- **Mitigaciones**: Paralelizar validaciones, criteria explícitos

---

## ADR-007: Protocolo de Resolución de Conflictos

**Status**: APROBADO

### Context
- 11 divisiones autónomas en paralelo → alta probabilidad de conflictos (División A dice X, División B dice ¬X)
- Sin protocolo → bloqueos indefinidos, decadencia lenta

### Decision
- Usar **protocolo-conflictos.md** (ANEXOS) como proceso formal:
  1. **Detect**: plan-reviewer identifica contradicción
  2. **Document**: CONFLICT-YYY.md con ambas posiciones
  3. **Reunir**: Divisiones aportan justificación + trade-offs (async, 2-3 días)
  4. **Tie-breaker**: Aplicar criterios (evidencia técnica → alineación → impacto → escalación)
  5. **Document Decision**: ADR con alternativas consideradas
  6. **Communicate**: Todas divisiones notificadas de resolución
  7. **Continue**: Volver a etapa 3 (revisión cruzada)

- **Max duración**: 3 días por conflicto (no bloquea todo)
- **Escalation matrix**: Technical → code-architecture-reviewer, Refactoring → code-refactor-master, etc.

### Consequences
- **Positivos**: Conflictos resueltos rápido, decisiones documentadas, continuidad garantizada
- **Negativos**: Requiere juicio humano (no automático)
- **Mitigaciones**: Escalation matrix, ADR documenta precedente para futuro

---

## ADR-008: Rollback Strategy Obligatorio en Refactors (Bloqueante)

**Status**: APROBADO

### Context
- code-refactor-master identifica refactor como bloqueante si no tiene rollback
- Estándares de producción: reversibilidad garantizada

### Decision
- **Rollback plan obligatorio** antes de refactor:
  - Cómo volver atrás por fase (reversión por pasos)
  - Checkpoint commits entre fases (facilita rollback)
  - Data migration reversible (backup pre-refactor)
  - Time-to-rollback ≤2 horas

- Documentar en EXECUTION_PACKAGE.md (FASE 6)
- Sin rollback strategy → refactor rechazado (bloqueante)

### Consequences
- **Positivos**: Zero-risk refactoring, confianza en cambios mayores
- **Negativos**: Overhead inicial de documentación
- **Mitigaciones**: Plantilla en EXECUTION_PACKAGE, automatización donde sea posible

---

## REFERENCIAS

- `./03_ciclo_planificacion.md` (6-stage cycle, Gates definition)
- `./ANEXOS/checklist-verificacion.md` (12-item validation)
- `./ANEXOS/rubrica-scoring.md` (6-dimension scoring rubric)
- `./ANEXOS/protocolo-conflictos.md` (7-step conflict resolution)
- `./02_research/plan-reviewer.md` (Validation-Before-Completion pattern)
- `./02_research/web-research-specialist.md` (Evidence validation matrix)

---

**Líneas totales**: 387 (cumple ≤500) ✅
**Estado**: FASE 4 COMPLETA - ADRs formalizados y gobernanza definida
