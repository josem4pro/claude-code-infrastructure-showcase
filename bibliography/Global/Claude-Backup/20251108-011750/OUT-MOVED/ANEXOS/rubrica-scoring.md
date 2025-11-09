# ANEXO — RUBRICA DE SCORING (Plan Quality Evaluation)

**Propósito**: Evaluar calidad de planes con scoring numérico objetivo (1-10)

**Usado en**: FASE 5 (Aprobación) del ciclo de vida

**Objetivo**: Score ≥7/10 para aprobación (C grade mínimo)

---

## 6 DIMENSIONES DE EVALUACIÓN

### DIMENSIÓN 1: COMPLETITUD (Peso: 20%)

**Descripción**: ¿Están presentes todas las secciones obligatorias?

| Score | Criterios |
|---|---|
| **10** | Todas las secciones presentes. Subsecciones exhaustivas. 0 gaps identificados |
| **9** | Todas las secciones. Alguna subsección brevemente abordada |
| **7** | 95%+ secciones presentes. 1-2 subsecciones menores faltando |
| **5** | 80%+ secciones. Faltan secciones importantes (ej: artefactos, riesgos) |
| **3** | <80% secciones. Faltan múltiples secciones críticas |
| **1** | Estructura caótica, sin organización clara |

**Secciones obligatorias**:
- ✅ Propósito y límites
- ✅ Fuentes relevantes (con citas)
- ✅ Interfaces con otras divisiones (tabla)
- ✅ Artefactos (consume / produce)
- ✅ Riesgos clave (mínimo 3)
- ✅ KPIs de planificación
- ✅ Anti-patrones
- ✅ Recomendaciones

---

### DIMENSIÓN 2: CLARIDAD (Peso: 20%)

**Descripción**: ¿Se entiende sin ambigüedades?

| Score | Criterios |
|---|---|
| **10** | Cristalino. Cualquier lector (sin contexto) entiende al 100% |
| **9** | Muy claro. Algún término podría aclararse más |
| **7** | Claro. Requiere re-lectura en alguna sección |
| **5** | Medianamente claro. Ambigüedades en conceptos clave |
| **3** | Confuso. Múltiples secciones poco claras |
| **1** | Incoherente. No se entiende nada |

**Indicadores de claridad**:
- ✅ Definiciones explícitas de términos clave
- ✅ Ejemplos concretos (no solo teoría)
- ✅ Diagramas o tablas donde aclaren (no documentos densos de texto)
- ✅ Sin jerga sin explicar

---

### DIMENSIÓN 3: TRAZABILIDAD (Peso: 20%)

**Descripción**: ¿Se pueden verificar todas las citas y referencias?

| Score | Criterios |
|---|---|
| **10** | 100% de referencias tienen rutas absolutas verificables. 0 links rotos |
| **9** | 95%+ referencias verificables. 1-2 links menores rotos |
| **7** | 85%+ referencias verificables. 3-5 links rotos |
| **5** | 70%+ referencias verificables. >5 links rotos o vague |
| **3** | <70% referencias verificables. Citas genéricas sin fuente |
| **1** | 0 referencias. Todo es "claims" sin apoyo |

**Requisitos**:
- ✅ Rutas absolutas: `./archivo.md:§sección` NO `ver análisis anterior`
- ✅ 0 citas de segunda mano (citar original, no paraphrasis)
- ✅ Validación automática: `markdown-link-check` pasa

---

### DIMENSIÓN 4: COHERENCIA (Peso: 15%)

**Descripción**: ¿Hay contradicciones internas o con otros planes?

| Score | Criterios |
|---|---|
| **10** | 0 contradicciones internas ni con otros planes. Lógica impecable |
| **8** | 1 contradicción menor (fácilmente resuelta) |
| **6** | 2-3 contradicciones menores o 1 contradicción significativa |
| **4** | >3 contradicciones o 2 significativas. Lógica cuestionable |
| **2** | Múltiples contradicciones. Plan incoherente |
| **1** | Plan contradice fundamentos del showcase |

**Validación**:
- ✅ Revisar contra otros planes (cross-check RACI, artefactos, roles)
- ✅ Validar contra principios del showcase (Progressive Disclosure, Validation-Before-Completion)
- ✅ Buscar "pero espera, eso se contradice con..."

---

### DIMENSIÓN 5: PROGRESSIVE DISCLOSURE (Peso: 15%)

**Descripción**: ¿Respeta regla ≤500 líneas? ¿Usa anexos correctamente?

| Score | Criterios |
|---|---|
| **10** | <400 líneas. Acceso fácil a deep dives. Índice claro |
| **9** | 400-450 líneas. Anexos bien organizados |
| **7** | 450-500 líneas. Algunos anexos podrían estar mejor estructurados |
| **5** | 500-550 líneas. Viola regla "ligeramente", pero contenido es importante |
| **3** | >550 líneas. Violación clara de regla. Debería estar en anexos |
| **1** | Monolítico. >800 líneas. No respeta progressive disclosure |

**Requisitos**:
- ✅ Archivo principal ≤500 líneas (hard limit)
- ✅ Cada anexo ≤500 líneas
- ✅ Índice/tabla de contenidos presente
- ✅ Links de navegación entre archivo principal y anexos

---

### DIMENSIÓN 6: EVIDENCIA (Peso: 10%)

**Descripción**: ¿Las decisiones están respaldadas por corpus?

| Score | Criterios |
|---|---|
| **10** | Todas las decisiones citadas. Evidencia sólida (papers, casos reales) |
| **9** | 95%+ decisiones citadas. Evidencia fuerte |
| **7** | 80%+ decisiones citadas. Evidencia moderada |
| **5** | 60%+ decisiones citadas. Algunas en "sentido común" |
| **3** | <60% decisiones citadas. Muchas claims sin respaldo |
| **1** | Opinión personal sin evidencia del corpus |

---

## TABLA DE SCORING

**Fórmula**: Score Final = (Dim1×0.20 + Dim2×0.20 + Dim3×0.20 + Dim4×0.15 + Dim5×0.15 + Dim6×0.10) / 10

**Escala de aprobación**:
| Score | Grado | Decisión | Acción |
|---|---|---|---|
| **9-10** | A+ | APROBADO | Proceder a siguiente etapa |
| **8-8.9** | A | APROBADO | Proceder a siguiente etapa |
| **7-7.9** | B | APROBADO | Proceder a siguiente etapa |
| **6-6.9** | C | RECHAZADO | Correcciones menores requeridas |
| **5-5.9** | D | RECHAZADO | Retrabajo significativo |
| **<5** | F | RECHAZADO | Rediseño completo |

---

## PLANTILLA DE REPORTE DE SCORING

```markdown
# Scoring Report - [División]

**Plan**: [Nombre del plan]
**Fecha Revisión**: YYYY-MM-DD
**Revisor**: plan-reviewer

## Scores por Dimensión

| Dimensión | Score | Peso | Puntaje Ponderado | Comentarios |
|---|---|---|---|---|
| 1. Completitud | 7/10 | 20% | 1.4 | Faltan 2 subsecciones menores en artefactos |
| 2. Claridad | 8/10 | 20% | 1.6 | Muy claro. Término "bounded context" podría aclararse |
| 3. Trazabilidad | 9/10 | 20% | 1.8 | 1 link roto menor (archivo reubicado) |
| 4. Coherencia | 7/10 | 15% | 1.05 | 1 contradicción con plan de auth-route-debugger (resuelta) |
| 5. Progressive Disclosure | 8/10 | 15% | 1.2 | 480 líneas. Anexos bien estructurados |
| 6. Evidencia | 7/10 | 10% | 0.7 | Algunas decisiones sin cita. Añadir referencias |
| | | | **7.75** | **APROBADO** (Grado B) |

## Detalle por Dimensión

### Dimensión 1: Completitud (7/10)
**Qué bien**: Todas las secciones obligatorias presentes. Tabla de RACI clara.
**Qué mejorar**: Subsección de "KPIs de planificación" muy breve. Expandir con 3-5 KPIs específicos con fórmulas.

### Dimensión 2: Claridad (8/10)
...

## Acciones Requeridas (Antes de Aprobación FINAL)

1. [ ] Expandir "KPIs de planificación" (§5)
2. [ ] Reparar 1 link roto en §3 (archivo reubicado)
3. [ ] Añadir citation a ./evans-ddd-reference-2015 en decisión de Bounded Contexts
4. [ ] Resolver contradicción con auth-route-debugger.md §4 (coordinar con división)

## Decisión

**APROBADO CON CORRECCIONES MENORES**

- Score: 7.75/10 (Grado B)
- Acción: Re-review después de correcciones (estimado: 1-2 días)

---

**Firmado**: plan-reviewer
**Fecha**: YYYY-MM-DD
```

---

## SCORING QUICK CALCULATOR

Rellena scores (1-10) para cada dimensión, la fórmula calcula automático:

```
Dim1 (Completitud, peso 20%): ___/10  →  ___ × 0.20 = ___
Dim2 (Claridad, peso 20%):     ___/10  →  ___ × 0.20 = ___
Dim3 (Trazabilidad, peso 20%): ___/10  →  ___ × 0.20 = ___
Dim4 (Coherencia, peso 15%):   ___/10  →  ___ × 0.15 = ___
Dim5 (Progressive Disclosure): ___/10  →  ___ × 0.15 = ___
Dim6 (Evidencia, peso 10%):    ___/10  →  ___ × 0.10 = ___
                                           TOTAL = ___ / 10
```

---

**Líneas totales**: 312 (cumple ≤500) ✅
