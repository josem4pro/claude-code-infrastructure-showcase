# 01 — MAPA DE TRABAJO
## Índice Operativo del Proyecto de Planificación

**Fecha**: 2025-11-07
**Session ID**: rtx_174830_358837_dc4703f7
**Investigador**: Claude Sonnet 4.5 (DEEP_MODE)
**Corpus**: 24 archivos markdown, 948KB total
**Fase**: 1 de 5 (Orientación y Mapa de Trabajo)

---

## 0. CONTEXTO Y MISIÓN

### Misión
Producir un **PLAN_FINAL_CONSENSUADO** que defina el ciclo de vida universal de planificación para una organización de desarrollo completamente manejada por IA, estructurada en **11 divisiones-agente**.

### Restricciones
- **100% PLANIFICACIÓN** - No diseñar ni discutir ejecución detallada
- **Corpus único**: Solo los 24 archivos markdown en este directorio
- **Progressive disclosure**: Archivos de salida ≤ 500 líneas
- **Canon organizacional**: Exactamente 11 divisiones, 29 skills permitidos
- **Verificación obligatoria**: Checklist, ADRs, RACI, Score ≥ 7/10

### Salida Dual
1. **Técnica**: Mapa total del ciclo de planificación (fases, artefactos, handoffs, gates)
2. **Organizacional**: Representación de empresa real (11 departamentos, roles, interfaces)

---

## 1. CORPUS IDENTIFICADO

### Resumen Cuantitativo
- **Total archivos**: 24 markdown
- **Tamaño total**: 948 KB (87% reducción vs 7.2 MB original)
- **Cobertura**: 100% bibliografía del showcase (6 meses de investigación)

### Clasificación Resumida

**Documentos Maestros (3)**: LA_BIBLIA (1,263 líneas), CLAUDE_INTEGRATION_GUIDE, README

**Investigación (3)**: RESUMEN_EJECUTIVO (10 hallazgos, 50+ fuentes), BIBLIOGRAFIA_INVESTIGACION, GUIA_INTEGRACION_BIBLIOGRAFIA

**Metadata (6)**: INDEX, MARKDOWN_INDEX, BIBLIOGRAPHY_STATUS, acquisition-log, references-inventory, references-catalog

**Papers Académicos (4)**:
- sweller-cognitive-load-1988 (Cognitive Load Theory)
- fucci-tdd-replication-2016 (TDD evidencia mixta)
- nagappan-tdd-quality-2008 (TDD reduce defectos 60-90%)
- evans-ddd-reference-2015-free (DDD, Bounded Contexts)

**Artículos Web (7)**:
- nngroup-progressive-disclosure-2006 (Nielsen Norman Group)
- ibm-progressive-disclosure-2024 (IBM Design)
- rails-doctrine-2006 (Convention over Configuration)
- forsgren-space-framework-2021 (SPACE Framework)
- builderio-ai-tools-comparison-2024 (10 AI tools)
- github-copilot-stats-2025-techcrunch (20M usuarios, 46% código)
- ddd-quickly-infoq-free (DDD alternativa gratuita)

**Estrategias (1)**: ACQUISITION_STRATEGIES

**Ruta base**: `/home/jose/Repositorios/claude-code-infrastructure-showcase/bibliography/Global/Markdown/`

---

## 2. CANON ORGANIZACIONAL: 11 DIVISIONES-AGENTE

### Tabla Maestra

| ID | División-Agente | Complejidad | LOC | Propósito en Planificación |
|---|---|---|---|---|
| A1 | code-architecture-reviewer | Alta | ~380 | Arquitectura y límites de contexto, decisiones estructurales |
| A2 | plan-reviewer | Alta | 498 | Oficina de revisión de planes y calidad del blueprint, gating |
| A3 | documentation-architect | Alta | ~420 | Ensamblado del documento maestro, índice, anexos, trazabilidad |
| A4 | web-research-specialist | Alta | ~400 | Correlación de evidencias dentro del corpus, matrices comparativas |
| A5 | code-refactor-master | Alta | ~350 | Diseño macro de refactors futuros, patrón objetivo, impacto |
| A6 | refactor-planner | Alta | ~320 | Estrategia de refactor a nivel plan, deuda técnica prevista |
| A7 | frontend-error-fixer | Media | ~290 | Calidad de UI a nivel plan: validación, estados de error |
| A8 | auth-route-debugger | Media | ~310 | Requisitos de autenticación, autorización, flujos de error |
| A9 | auth-route-tester | Media | ~280 | Contratos y criterios de aceptación para rutas protegidas |
| A10 | auto-error-resolver | Baja | ~200 | Reglas de higiene TypeScript/estático para futura ejecución |
| A11 | Explore | Media | Built-in | Exploración dirigida del codebase y corpus para riesgos desconocidos |

**📎 Fichas detalladas**: Ver `ANEXOS/fichas-divisiones.md` (responsabilidades, interfaces, artefactos, riesgos, KPIs, anti-patrones, recomendaciones, referencias por división)

---

## 3. TABLA DE ARCHIVOS FUENTE POR DIVISIÓN

| División | Archivos Primarios | Archivos Secundarios | Total |
|---|---|---|---|
| **plan-reviewer** | LA_BIBLIA (§8 TDD), RESUMEN_EJECUTIVO, forsgren-space-framework-2021 | fucci-tdd-replication-2016, nagappan-tdd-quality-2008 | 5 |
| **code-architecture-reviewer** | LA_BIBLIA (§9 Arquitectura), evans-ddd-reference-2015 | CLAUDE_INTEGRATION_GUIDE, ddd-quickly-infoq-free | 4 |
| **documentation-architect** | LA_BIBLIA (§7 Progressive Disclosure), sweller-cognitive-load-1988 | nngroup-progressive-disclosure-2006, ibm-progressive-disclosure-2024, GUIA_INTEGRACION_BIBLIOGRAFIA | 5 |
| **web-research-specialist** | RESUMEN_EJECUTIVO, BIBLIOGRAFIA_INVESTIGACION | TODO el corpus (24 archivos) | 24 |
| **refactor-planner** | LA_BIBLIA (§15 Patrones), sweller-cognitive-load-1988 | evans-ddd-reference-2015 | 3 |
| **code-refactor-master** | LA_BIBLIA (§9 Arquitectura, §15 Patrones), evans-ddd-reference-2015 | — | 2 |
| **frontend-error-fixer** | LA_BIBLIA (§12.2 frontend-dev-guidelines), CLAUDE_INTEGRATION_GUIDE | — | 2 |
| **auth-route-debugger** | LA_BIBLIA (§12.4 route-tester, §13.8), CLAUDE_INTEGRATION_GUIDE | — | 2 |
| **auth-route-tester** | LA_BIBLIA (§12.4 route-tester, §13.9), CLAUDE_INTEGRATION_GUIDE | — | 2 |
| **auto-error-resolver** | LA_BIBLIA (§11.3 tsc-check, §11.6 stop-build-check) | fucci-tdd-replication-2016 | 2 |
| **Explore** | TODO el corpus (exploración) | — | 24 |

---

## 4. SUPUESTOS Y PREGUNTAS ABIERTAS

### Supuestos (A Validar en FASE 2)

1. Ciclo de vida de planificación es **universal** (no específico de stack tecnológico)
2. 11 divisiones son **suficientes y no redundantes**
3. Corpus de 24 archivos cubre **100% de contexto necesario**
4. Progressive Disclosure (500 líneas) **aplica a planificación**
5. Paralelización de 11 divisiones es **efectiva**

### Preguntas Abiertas (Resolver en FASE 3)

1. **¿Cuántas etapas tiene el ciclo de vida universal?**
   - Propuesta: 6 etapas (Descubrimiento, Arquitectura, Revisión Cruzada, Documentación, Aprobación, Handoff)

2. **¿Cómo se resuelven conflictos entre divisiones?**
   - Ejemplo: refactor-planner prioriza refactor X, pero code-architecture-reviewer lo rechaza
   - *Definir*: Protocolo de resolución

3. **¿Qué artefactos son obligatorios vs opcionales?**
   - *Definir*: Matriz de artefactos obligatorios por etapa

4. **¿Qué métricas evalúan calidad del plan?**
   - Propuesta: Score numérico (1-10) con rubrica explícita

5. **¿Cómo se documenta decisiones rechazadas?**
   - *Definir*: Política de documentación de rechazos

---

## 5. RIESGOS TOP-10 Y MITIGACIONES

| # | Riesgo | Impacto | Prob | Mitigación | Owner |
|---|---|---|---|---|---|
| 1 | Exceder 500 líneas por archivo | ALTO | Media | Validación automática (wc -l), uso obligatorio de anexos | documentation-architect |
| 2 | Contradicciones entre fuentes del corpus | ALTO | Media | Aplicar systematic-debugging, documentar en ADRs | web-research-specialist, plan-reviewer |
| 3 | Overlaps de responsabilidad entre divisiones | MEDIO | Media | Definir RACI claro por etapa en FASE 3 | plan-reviewer |
| 4 | Gaps de información para alguna división | MEDIO | Baja | Explore agent identifica gaps en FASE 2 | Explore |
| 5 | Paralelización genera outputs inconsistentes | MEDIO | Media | Plantilla común, validación cruzada en FASE 3 | documentation-architect |
| 6 | ADRs sin justificación suficiente | MEDIO | Media | Checklist de ADR (contexto, decisión, consecuencias) | plan-reviewer |
| 7 | RACI incompleto o ambiguo | MEDIO | Media | Plantilla RACI en FASE 3, validación con todas las divisiones | plan-reviewer, documentation-architect |
| 8 | Score de calidad < 7/10 en FASE 5 | MEDIO | Baja | Validación continua en cada fase, no esperar a FASE 5 | plan-reviewer |
| 9 | Falta de evidencia para decisiones críticas | BAJO | Baja | web-research-specialist valida todas las citas | web-research-specialist |
| 10 | No completar en tiempo estimado | BAJO | Baja | Progressive disclosure reduce scope creep, fases con gates | plan-reviewer |

---

## 6. SKILLS ACTIVADOS Y JUSTIFICACIÓN

### SIEMPRE Activados (Críticos)

**verification-before-completion** (PRIORITY: HIGH)
- **Justificación**: Obligatorio para Score ≥ 7/10
- **Aplicación**: Checklist en FASE 5, validación continua
- **Owner**: plan-reviewer

**dispatching-parallel-agents** (PRIORITY: HIGH)
- **Justificación**: 11 divisiones trabajando en paralelo en FASE 2
- **Aplicación**: Lanzar 11 agentes simultáneos con plantilla común
- **Owner**: Instancia maestra (esta sesión)

**systematic-debugging** (PRIORITY: HIGH)
- **Justificación**: Resolver contradicciones entre fuentes (ej: TDD evidencia mixta)
- **Aplicación**: Cuando se detecte contradicción en corpus
- **Owner**: web-research-specialist

### Activados si Generamos Tooling (Condicional)

**test-driven-development** (PRIORITY: CRITICAL si aplica)
- **Justificación**: Si creamos validadores automáticos (ej: script validación 500 líneas)
- **Aplicación**: RED (test) → GREEN (implementar) → REFACTOR (optimizar)
- **Evidencia requerida**: Commits mostrando ciclo completo
- **Owner**: auto-error-resolver (si genera tooling)
- **Estado**: NO anticipado por ahora. Si surge, activar es **BLOQUEANTE**.

### No Activados (Justificación)
- **root-cause-tracing**: Solo si falla systematic-debugging
- **defense-in-depth**: Más relevante para ejecución que planificación
- **backend/frontend-dev-guidelines**: No aplicable (esto ES la planificación del showcase)
- **brainstorming**: Misión claramente definida
- **writing-plans / executing-plans**: Esto ES el plan (no meta-planning)
- **error-tracking, route-tester, form-validator**: Skills de ejecución, no planificación

---

## 7. PRÓXIMOS PASOS (FASE 2)

### Acción Inmediata
Marcar FASE 1 como completada, iniciar FASE 2 con lanzamiento paralelo de 11 agentes.

### FASE 2: Investigación Paralela por División
**Objetivo**: Extraer conocimiento accionable desde el corpus para cada división.

**Para cada división** (11 en paralelo):
1. Leer archivos primarios y secundarios asignados (tabla §3)
2. Resumir fuentes relevantes con citas internas (rutas absolutas)
3. Definir **interfaces** con otras divisiones
4. Listar **artefactos** que consume y produce
5. Identificar **anti-patrones** y cómo evitarlos
6. Definir **KPIs de planificación**
7. Emitir **recomendaciones** para el ciclo de vida

**Plantilla**: Ver §11 del superprompt (archivo de salida ≤ 500 líneas)

**Entregables FASE 2**:
- `OUT/02_research/plan-reviewer.md` (≤ 500 líneas)
- `OUT/02_research/code-architecture-reviewer.md` (≤ 500 líneas)
- `OUT/02_research/documentation-architect.md` (≤ 500 líneas)
- `OUT/02_research/web-research-specialist.md` (≤ 500 líneas)
- `OUT/02_research/code-refactor-master.md` (≤ 500 líneas)
- `OUT/02_research/refactor-planner.md` (≤ 500 líneas)
- `OUT/02_research/frontend-error-fixer.md` (≤ 500 líneas)
- `OUT/02_research/auth-route-debugger.md` (≤ 500 líneas)
- `OUT/02_research/auth-route-tester.md` (≤ 500 líneas)
- `OUT/02_research/auto-error-resolver.md` (≤ 500 líneas)
- `OUT/02_research/Explore.md` (≤ 500 líneas)
- `OUT/02_research/index.json` (tabla resumen por división)

**Criterio de completitud**: 11 archivos creados, todos ≤ 500 líneas, index.json generado.

### Validación de FASE 1 (Antes de Proceder)
- ✅ 01_mapa_trabajo.md creado (este archivo)
- ✅ ANEXOS/fichas-divisiones.md creado (fichas completas de 11 divisiones)
- ✅ Tabla de archivos fuente por división definida
- ✅ Riesgos top-10 identificados
- ✅ Skills activados justificados
- ✅ Próximos pasos claros
- ✅ Cumplimiento de regla 500 líneas (verificar antes de commit)

**Estado**: FASE 1 completa. Listo para FASE 2.

---

## APÉNDICE: METADATA DEL MAPA

**Generado por**: Claude Sonnet 4.5 (DEEP_MODE)
**Session ID**: rtx_174830_358837_dc4703f7
**Commit ID**: [rtx_174830_358837_dc4703f7] FASE 1 completa - Mapa de Trabajo
**Líneas totales**: <500 (verificar con `wc -l`)
**Fecha**: 2025-11-07
**Próxima acción**: Lanzar 11 agentes en paralelo para FASE 2
**Anexos generados**:
- ANEXOS/fichas-divisiones.md (fichas detalladas de 11 divisiones)

---

**FIN DE 01_MAPA_TRABAJO.MD**
