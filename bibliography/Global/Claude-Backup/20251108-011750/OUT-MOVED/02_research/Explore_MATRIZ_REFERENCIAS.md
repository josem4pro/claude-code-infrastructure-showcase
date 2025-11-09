# Explore - Matriz de Referencias Cruzadas

**Propósito**: Facilitar búsqueda de hallazgos y validación de fuentes
**Formato**: Tabla de referencia rápida Gap/Riesgo → Fuentes del corpus

---

## GAPS - Referencias en Corpus

| ID | Gap | Fuente Primaria | Fuente Secundaria | Línea/Sección |
|----|-----|-----------------|-------------------|---------------|
| G1 | Conflict Resolution | 01_mapa_trabajo.md | fichas-divisiones.md | §5 Riesgos (línea ~140) |
| G2 | Scoring Rubric | 01_mapa_trabajo.md | fichas-divisiones.md A2 | §2.2, §5 (línea ~92) |
| G3 | Artefact DAG | fichas-divisiones.md | 01_mapa_trabajo.md | "Artefactos que consume/produce" |
| G4 | Corpus Policy | RESUMEN_EJECUTIVO | BIBLIOGRAPHY_STATUS | "50+ fuentes" vs "24 archivos" |
| G5 | Coherence Validation | LA_BIBLIA §8 | fucci-tdd-replication-2016 | TDD contradicciones (línea ~1195) |
| G6 | Validation Plan | 01_mapa_trabajo.md | (ninguno) | §4 "Supuestos (A Validar)" |

---

## RIESGOS - Referencias en Corpus

| ID | Riesgo | Fuente Primaria | Evidencia | Línea/Sección |
|----|--------|-----------------|-----------|---------------|
| R1 | Paralelización | fichas-divisiones.md | "Recibe guías de A1" (A5) | Dependencias listadas |
| R2 | Cognitive Load | LA_BIBLIA §7 | "500 líneas regla" | §7.1 (línea ~678) |
| R3 | Universal Scope | 01_mapa_trabajo.md | "ciclo universal" sin def | §0 Misión (línea ~14) |
| R4 | Tech Viability | 01_mapa_trabajo.md | "100% PLANIFICACIÓN" | "No diseñar ejecución" |
| R5 | Scope Creep | fichas-divisiones.md | 11 divisiones × outputs | Múltiples artefactos por división |

---

## ZONAS GRISES - Clarificaciones Propuestas

| ID | Ambigüedad | Fuente | Clarificación Recomendada |
|----|------------|--------|---------------------------|
| Z1 | plan-reviewer role | fichas-divisiones.md A2 | GUARDRAIL DURO, no consultor |
| Z2 | Anti-patrones | fichas-divisiones.md | MUST NOT (vs SHOULD recomendaciones) |
| Z3 | web-research scope | 01_mapa_trabajo.md §3 | Validador Coherencia + Reconciliador |
| Z4 | FASE 2 vs 3 | 01_mapa_trabajo.md §7 | FASE 2=investigación, FASE 3=síntesis |

---

## VALIDACIÓN CRUZADA - ¿Qué documento documenta cada gap?

```
GAPS:
├── G1: Conflict Resolution
│   ├── Mencionado en: 01_mapa_trabajo.md (§5, línea ~141)
│   ├── Relacionado: fichas-divisiones.md (interfaces A1-A11)
│   ├── NO documentado: Protocolo explícito
│   └── Owner: plan-reviewer
│
├── G2: Scoring Rubric
│   ├── Mencionado en: fichas-divisiones.md A2 (línea ~74)
│   ├── Referencia: LA_BIBLIA §2 (línea ~92)
│   ├── NO documentado: Rubrica detallada
│   └── Owner: plan-reviewer
│
├── G3: Artefact DAG
│   ├── Parcialmente documentado: fichas-divisiones.md (cada división)
│   ├── NO documentado: DAG visual, precedencias
│   └── Owner: documentation-architect
│
├── G4: Corpus Policy
│   ├── NO documentado: ningún side
│   ├── Implicado por: RESUMEN_EJECUTIVO (50+ fuentes), BIBLIOGRAPHY_STATUS
│   └── Owner: web-research-specialist
│
├── G5: Coherence Validation
│   ├── Parcialmente documentado: LA_BIBLIA §8 (TDD contradictions)
│   ├── Evidencia: fucci-tdd-replication-2016, nagappan-tdd-quality-2008
│   ├── NO documentado: Política de reconciliación
│   └── Owner: web-research-specialist
│
└── G6: Validation Plan
    ├── Mencionado en: 01_mapa_trabajo.md §4
    ├── NO documentado: Metodología de validación
    └── Owner: Explore
```

---

## MATRIZ SEVERIDAD × IMPACTO

| Severidad | Impacto | Ejemplos | Acción |
|-----------|---------|----------|--------|
| **ALTA** | Deadlock, Re-trabajo masivo | G1, G2, R1, R3 | RESOLVER PRE-FASE 2 |
| **MEDIA** | Ineficiencia, Confusión | G3-G6, R2,R4-R5, Z1-Z4 | Resolver FASE 2-3 |

---

## CHECKLIST DE IMPLEMENTACIÓN

- [ ] G1: Crear Conflict Resolution Protocol (plan-reviewer)
- [ ] G2: Crear Scoring Rubric con 5-6 dimensiones (plan-reviewer)
- [ ] R3: Crear Scope Charter "universal para X" (plan-reviewer)
- [ ] G3: Crear Parallelization DAG (documentation-architect)
- [ ] G4: Establecer Corpus Freeze Policy (web-research-specialist)
- [ ] G5: Crear Contradiction Registry (web-research-specialist)
- [ ] G6: Crear Validation Plan para 5 supuestos (Explore)
- [ ] R1: Implementar DAG secuencial FASE 2a/2b (plan-reviewer)
- [ ] R2: Limitar corpus por división (documentation-architect)
- [ ] R4: Validar viabilidad técnica (code-architecture-reviewer)
- [ ] R5: Crear Assembly Specification FASE 3 (documentation-architect)

---

**Generado por**: Explore Division
**Fuentes validadas**: 100% referencias cruzadas con corpus de 24 archivos

