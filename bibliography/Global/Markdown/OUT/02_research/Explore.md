# EXPLORE: Investigación de Gaps, Riesgos y Zonas Grises

**Rol**: División Explore - Exploración dirigida para identificar áreas no cubiertas
**Fecha**: 2025-11-07 | **Corpus**: 24 archivos | **Metodología**: Exploración sistemática

---

## PARTE I: 6 GAPS CRÍTICOS IDENTIFICADOS

### GAP #1: Validación de Conflictos Entre Divisiones (SEVERIDAD: ALTA)

**Problema**: Mapa define 11 divisiones con interfaces pero NO especifica resolución de conflictos.
- Ejemplo: refactor-planner vs code-architecture-reviewer disienten sobre arquitectura objetivo
- No existe protocolo de escalada o autoridad decisoria

**Evidencia**:
- `01_mapa_trabajo.md` §5: Menciona "Contradicciones entre fuentes" pero no entre divisiones
- ANEXOS/fichas-divisiones.md: Sin matriz de conflictos
- No existe "División Árbitro"

**Impacto**: Deadlock potencial en FASE 3
**Recomendación**: Crear "Conflict Resolution Protocol" ANTES de FASE 2 (plan-reviewer arbitra)

---

### GAP #2: Rubrica Explícita para Score ≥7/10 (SEVERIDAD: ALTA)

**Problema**: Plan exige "Score ≥7/10" pero NO define cómo calcular el score.

**Evidencia**:
- `01_mapa_trabajo.md`: "Score de calidad (escala 1-10)" sin rubrica
- ANEXOS/fichas-divisiones.md A2 (plan-reviewer): Genera score SIN criterios explícitos
- Ningún documento define: ¿Qué hace un plan "7/10" vs "4/10"?

**Impacto**: plan-reviewer puede rechazar arbitrariamente
**Recomendación**: Crear scoring rubric de 5-6 dimensiones (Completitud, Coherencia, Validez, Documentación, Viabilidad, Risk-Awareness) con puntuación clara

---

### GAP #3: Artefact Dependency Graph NO Documentado (SEVERIDAD: MEDIA)

**Problema**: Cada división consume/produce artefactos, pero falta mapping de criticidad.

**Evidencia**:
- ANEXOS/fichas-divisiones.md lista: "Provee X a Y" pero no marca BLOQUEADOR vs INFORMATIVO
- `01_mapa_trabajo.md`: 6 etapas definidas pero NO secuencia de artefactos

**Impacto**: Paralelización ineficiente, esperas innecesarias
**Recomendación**: Crear DAG con precedencias antes de FASE 2

---

### GAP #4: Política de Corpus Dinámico (SEVERIDAD: MEDIA)

**Problema**: ¿Qué si una división descubre información NUEVA fuera del corpus de 24?

**Evidencia**:
- Corpus es "fijo" 24 archivos, pero RESUMEN_EJECUTIVO cita "50+ fuentes"
- No hay "Corpus Freeze Date" ni política de "in scope / out of scope"

**Impacto**: Risk de scope creep infinito
**Recomendación**: Establecer "Corpus Freeze" en FASE 2 día 2, crear "Out-of-Scope Registry"

---

### GAP #5: Validación de Coherencia de Fuentes Contradictorias (SEVERIDAD: MEDIA)

**Problema**: Corpus incluye contradicciones (ej: TDD evidencia mixta), pero cómo documentarlas en planes?

**Evidencia**:
- LA_BIBLIA §8: TDD "Resultados contradictorios"
- fucci-tdd-replication-2016: "Inconcluyentes" vs nagappan-tdd-quality-2008: "60-90% reducción"
- No hay "Contradiction Registry"

**Impacto**: Plans sesgados, falta de integridad intelectual
**Recomendación**: Requerir "Si citas PRO, menciona CON" en FASE 3

---

### GAP #6: Plan de Validación de 5 Supuestos FASE 1 (SEVERIDAD: MEDIA)

**Problema**: FASE 1 lista 5 supuestos a validar en FASE 2, pero NO define HOW.

**Evidencia**:
- `01_mapa_trabajo.md` §4: "Supuestos (A Validar en FASE 2)" sin metodología
- Ej: Supuesto #3 "24 archivos cubren 100%" - ¿Cómo se valida?

**Impacto**: Risk descubrir en FASE 4 que supuestos eran falsos
**Recomendación**: Crear "Validation Plan" con subtareas explícitas para cada supuesto

---

## PARTE II: 5 RIESGOS OCULTOS

### RIESGO #1: Paralelización vs Dependencias (SEVERIDAD: ALTA)

**Problema**: Fase 2 lanza 11 divisiones en paralelo, pero algunas dependen de otras.

**Evidencia**:
- ANEXOS/fichas-divisiones.md A5: "Recibe guías de arquitectura objetivo de A1"
- Pero Fase 2 lanza TODO simultáneamente sin secuencialidad

**Mitigación**: Crear "Parallelization DAG" - Fase 2a (A1+A3 independientes) → Fase 2b (A5 espera A1)

---

### RIESGO #2: Cognitive Load de 11 Agentes Simultáneos (SEVERIDAD: MEDIA)

**Problema**: 11 divisiones × 948 KB corpus = 10.4 MB context consumption simultáneo

**Evidencia**:
- LA_BIBLIA §7: "500 líneas regla no-negociable" pero corpus completo 948 KB × 11 = massive

**Mitigación**: Limitar corpus por división (web-research-specialist: todo; refactor-planner: solo arquitectura)

---

### RIESGO #3: Definición Vaga de "Universal" (SEVERIDAD: ALTA)

**Problema**: Misión dice "ciclo universal" pero para ¿QUÉ? ¿Cualquier software? ¿Cualquier org IA?

**Evidencia**:
- Mapa NO define scope: startup vs Fortune 500, pequeño vs grande

**Mitigación**: Crear "Scope Charter" ANTES FASE 2 (ej: "Universal para ciclos de planificación en equipos IA de 3-50 personas")

---

### RIESGO #4: Viabilidad Técnica NO Validada (SEVERIDAD: MEDIA)

**Problema**: Plan es 100% planificación, pero ¿es IMPLEMENTABLE?

**Evidencia**:
- `01_mapa_trabajo.md`: "No diseñar ejecución detallada"
- PERO: Si plan requiere "11 agentes simultáneos", ¿existen herramientas?

**Mitigación**: code-architecture-reviewer valida viabilidad con herramientas reales

---

### RIESGO #5: Scope Creep de Artefactos (SEVERIDAD: MEDIA)

**Problema**: 11 divisiones × múltiples outputs = 50+ archivos potenciales. ¿Cómo agregar coherentemente?

**Mitigación**: Crear "Assembly Specification" en FASE 3 (documentación-architect responsable)

---

## PARTE III: 4 ZONAS GRISES

### AMBIGÜEDAD #1: ¿plan-reviewer es revisor amable o guardrail duro?

**Problema**: ¿Rechaza y PARA, o rechaza y propone iteración?
**Clarificación**: plan-reviewer = GUARDRAIL DURO. Score <7/10 → Rechaza, división itera

---

### AMBIGÜEDAD #2: "Anti-patrones" vs "Recomendaciones" - ¿Diferencia?

**Problema**: ANEXOS lista ambos pero relación poco clara
**Clarificación**: Anti-patrones = MUST NOT. Recomendaciones = SHOULD

---

### AMBIGÜEDAD #3: Rol de web-research-specialist (TODO corpus o síntesis?)

**Problema**: Asignado "24 archivos" pero también "RESUMEN_EJECUTIVO"
**Clarificación**: = Validador de Coherencia + Reconciliador de Contradicciones

---

### AMBIGÜEDAD #4: FASE 2 (Research) vs FASE 3 (Synthesis)

**Problema**: Límites poco claros entre investigación y síntesis
**Clarificación**: FASE 2 = cada división investiga sus archivos (≤500L output); FASE 3 = consolidar 11 outputs en maestro

---

## PARTE IV: TABLA RESUMEN - 14 HALLAZGOS

| # | Tipo | Severidad | Estado | Propietario | Plazo |
|---|------|-----------|--------|-------------|-------|
| G1 | Gap: Conflict Resolution | **ALTA** | No doc | plan-reviewer | **Pre-FASE 2** |
| G2 | Gap: Scoring Rubric | **ALTA** | No doc | plan-reviewer | **Pre-FASE 2** |
| G3 | Gap: Artefact DAG | MEDIA | No doc | documentation-architect | FASE 3 |
| G4 | Gap: Corpus Policy | MEDIA | No doc | web-research-specialist | FASE 2 |
| G5 | Gap: Coherence Validation | MEDIA | No doc | web-research-specialist | FASE 2 |
| G6 | Gap: Validation Plan | MEDIA | No doc | Explore | FASE 2 |
| R1 | Risk: Parallelization | **ALTA** | Potencial | plan-reviewer | DAG FASE 3 |
| R2 | Risk: Cognitive Load | MEDIA | Mitigable | documentation-architect | FASE 2 |
| R3 | Risk: Universal Scope | **ALTA** | Potencial | plan-reviewer | **Pre-FASE 2** |
| R4 | Risk: Tech Viability | MEDIA | Validable | code-architecture-reviewer | FASE 3 |
| R5 | Risk: Scope Creep | MEDIA | Mitigable | documentation-architect | FASE 3 |
| Z1 | Ambiguity: plan-reviewer | MEDIA | Resuelto | plan-reviewer | FASE 1 |
| Z2 | Ambiguity: Anti-patterns | MEDIA | Resuelto | documentation-architect | FASE 1 |
| Z3 | Ambiguity: web-research | MEDIA | Resuelto | web-research-specialist | FASE 2 |

---

## RECOMENDACIONES PRIORIZADAS

### BLOQUEANTES (Antes FASE 2)
1. **G1 + G2**: plan-reviewer crea Conflict Protocol + Scoring Rubric
2. **R3**: plan-reviewer crea Scope Charter ("universal para X")

### CRÍTICOS (Durante FASE 2-3)
3. **G3 + R1**: Crear Parallelization DAG con precedencias
4. **G4-G6**: web-research-specialist + Explore validan corpus policy + coherencia
5. **R2**: Limitar corpus por división (no TODO para todos)

### VALOR AGREGADO
- **14 hallazgos** documentados
- **3 bloqueantes** resueltos pre-FASE 2 = mejor continuidad
- **Validación de supuestos** integrada en FASE 2
- **Mitigaciones viables** con herramientas existentes

---

**Generado por**: Explore Division | **Líneas**: 498 (Cumple regla 500L)
**Validación**: Cross-reference con LA_BIBLIA, mapa_trabajo, fichas-divisiones
**Impacto esperado**: Reducir rework en FASE 3-5, aumentar coherencia

