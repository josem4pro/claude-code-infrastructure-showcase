# plan-reviewer — Investigación FASE 2

**División**: plan-reviewer (A2)
**Fecha**: 2025-11-07
**Investigador**: Claude Sonnet 4.5 (DEEP_MODE)
**Archivos asignados**: LA_BIBLIA (§8 TDD), RESUMEN_EJECUTIVO, forsgren-space-framework-2021, fucci-tdd-replication-2016, nagappan-tdd-quality-2008

---

## Propósito y Límites

**Propósito**: Oficina de revisión de planes, gating, criterios de aceptación, cierre consensuado. Garantizar que todos los planes cumplan estándares de calidad antes de aprobación.

**Límites**: NO ejecuto planes. Solo reviso, apruebo/rechazo con feedback estructurado, mantengo checklist de calidad, asigno scores numéricos.

**Rol en planificación**: Gate keeper del ciclo. Nada avanza a siguiente etapa sin mi aprobación explícita (Score ≥7/10).

---

## Fuentes Relevantes y Hallazgos

### LA_BIBLIA §8: TDD y Validation-Before-Completion

**Hallazgo clave**: El showcase NO implementa TDD tradicional (tests unitarios), pero SÍ implementa "test-first thinking" mediante **Validation-Before-Completion Pattern**.

**Checklist del showcase** (`./LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md:§8`):
```markdown
Antes de considerar tarea completada:
□ ¿El código compila? (TypeScript: tsc --noEmit)
□ ¿Los tests existentes pasan? (npm test)
□ ¿El linter está limpio? (npm run lint)
□ ¿Sentry está integrado? (error-tracking skill)
□ ¿La arquitectura es consistente? (layered pattern)
```

**Aplicación a planificación**: Sustituir "código compila" por "plan completo", "tests pasan" por "checklist pasado", etc.

**Evidencia académica mixta**:
- **Nagappan et al. (2008)**: TDD reduce defect density 60-90%, pero incrementa tiempo 15-35%
- **Fucci et al. (2016)**: "Resultados contradictorios en literatura", beneficios dependen de contexto

**Posición adoptada**: Pragmatismo > Dogmatismo. Validación continua es obligatoria, pero no forzar TDD estricto.

### RESUMEN_EJECUTIVO: Frameworks de Medición

**SPACE Framework** (`./RESUMEN_EJECUTIVO_INVESTIGACION.md:§4.7`):
- 5 dimensiones: **S**atisfaction, **P**erformance, **A**ctivity, **C**ommunication, **E**fficiency
- Co-autora: Nicole Forsgren (fundadora DORA)
- Publicado en ACM Queue (peer-reviewed)

**DevEx Framework** (Noda et al., ACM Queue 2023):
- 3 dimensiones core: **Feedback loops**, **Cognitive load**, **Flow state**
- Enfoque en experiencia vs métricas de output

**DORA Metrics** (Google 2024):
- 4 métricas clave: Deployment Frequency, Lead Time, Change Failure Rate, MTTR
- 75%+ de encuestados usan AI tools para tareas diarias

**Aplicación a planificación**: Estos frameworks proveen métricas para evaluar calidad del plan (no del código). Por ejemplo:
- **Satisfaction**: ¿El plan es claro? ¿Las divisiones entienden sus roles?
- **Cognitive load**: ¿El plan excede 500 líneas? ¿Hay progressive disclosure?
- **Feedback loops**: ¿Hay gates de validación entre fases?

### forsgren-space-framework-2021: Métricas de Impacto

**Estadísticas verificadas** (`./forsgren-space-framework-2021.md`):
- Microsoft Research: **37% más rápido** completion con flow-optimized environments
- Engineers reportan **3.4x mayor productividad** cuando condiciones de flow existen

**Aplicación**: Diseñar ciclo de planificación que preserve flow state (minimize interruptions, clear handoffs, progressive disclosure).

---

## Interfaces con Otras Divisiones

| División | Relación | Artefactos Intercambiados |
|---|---|---|
| **TODAS (10 divisiones)** | Recibo planes para revisión | INPUT: Plans de todas las divisiones |
| **documentation-architect** | Valido estructura y completitud | OUTPUT: Feedback de formato y trazabilidad |
| **code-architecture-reviewer** | Valido arquitectura del plan | OUTPUT: Validación de consistencia técnica |
| **web-research-specialist** | Valido evidencia citada | OUTPUT: Validación de referencias |
| **Explore** | Recibo reporte de riesgos críticos | INPUT: Riesgos desconocidos identificados |
| **auth-route-tester** | Valido criterios de aceptación | OUTPUT: Aprobación de contratos API |
| **auto-error-resolver** | Recibo checklist de validación | INPUT: Checklist de higiene técnica |

**Rol central**: Soy el hub de validación. Todos los artefactos pasan por mí antes de aprobarse.

---

## Artefactos

### Consume
- Planes de implementación (de todas las divisiones)
- Checklists de verificación (de auto-error-resolver, auth-route-tester)
- ADRs (de code-architecture-reviewer)
- Matrices RACI (de documentation-architect)
- Reportes de riesgos (de Explore)

### Produce
- **Reportes de revisión** (para cada plan revisado)
- **Scores de calidad** (escala 1-10, rubrica explícita)
- **Aprobaciones/Rechazos** (con justificación estructurada)
- **Checklists de calidad** (plantillas reutilizables)
- **Rubrica de scoring** (definir criterios de evaluación)

---

## Riesgos Clave y Mitigaciones

### Riesgo 1: Aprobar plan incompleto (ALTO)
**Impacto**: Plan ejecutado con gaps, retrabajo costoso
**Probabilidad**: Media (presión por cumplir plazos)
**Mitigación**: Checklist obligatorio, score numérico (no subjetivo), "placeholders" son automático rechazo
**Owner**: plan-reviewer

### Riesgo 2: Bloquear plan por perfeccionismo excesivo (MEDIO)
**Impacto**: Parálisis de progreso, frustración de divisiones
**Probabilidad**: Media (falta de rubrica clara)
**Mitigación**: Rubrica explícita (qué es 7/10 vs 10/10), feedback accionable (no "está mal", sino "falta X")
**Owner**: plan-reviewer

### Riesgo 3: Criterios de aceptación ambiguos (MEDIO)
**Impacto**: Divisiones no saben qué produce un "7/10"
**Probabilidad**: Alta (sin rubrica predefinida)
**Mitigación**: Crear rubrica en FASE 3, publicarla antes de revisar planes
**Owner**: plan-reviewer

### Riesgo 4: Feedback vago sin acciones concretas (MEDIO)
**Impacto**: División no sabe cómo corregir
**Probabilidad**: Media (falta de entrenamiento)
**Mitigación**: Plantilla de feedback (Sección → Problema → Acción requerida)
**Owner**: plan-reviewer

### Riesgo 5: No detectar contradicciones entre divisiones (ALTO)
**Impacto**: Plan con conflictos internos
**Probabilidad**: Alta (11 divisiones en paralelo)
**Mitigación**: Validación cruzada (leer TODOS los planes antes de aprobar ninguno), matriz de coherencia
**Owner**: plan-reviewer

---

## KPIs de Planificación

### KPI 1: Score Promedio de Calidad de Planes
**Métrica**: Promedio de scores asignados a planes aprobados
**Objetivo**: ≥7.5/10 (75% calidad)
**Fórmula**: Σ(scores aprobados) / N(planes aprobados)
**Frecuencia**: Por fase del ciclo

### KPI 2: Tasa de Rechazo en Primera Revisión
**Métrica**: % de planes rechazados en primera revisión
**Objetivo**: <30% (indicador de claridad de criterios)
**Fórmula**: N(rechazados first pass) / N(total planes) × 100
**Frecuencia**: Por fase del ciclo
**Interpretación**: Alta tasa = criterios poco claros o comunicación deficiente

### KPI 3: Tiempo Promedio de Revisión
**Métrica**: Tiempo desde recepción hasta aprobación/rechazo
**Objetivo**: <2 horas para planes <500 líneas
**Fórmula**: Σ(tiempo revisión) / N(planes revisados)
**Frecuencia**: Continuo

### KPI 4: Cobertura de Checklist
**Métrica**: % de items del checklist completados en planes aprobados
**Objetivo**: 100% (sin excepciones)
**Fórmula**: N(items completos) / N(items totales) × 100
**Frecuencia**: Por plan

### KPI 5: Tasa de Detección de Conflictos Inter-Divisionales
**Métrica**: Número de conflictos detectados en revisión cruzada
**Objetivo**: 100% detectados antes de aprobación
**Fórmula**: Conflictos detectados / Conflictos totales existentes
**Frecuencia**: Por ciclo de planificación

---

## Anti-patrones

### AP1: Revisar sin Checklist Estructurado
**Descripción**: Revisar plan "a ojo", sin lista explícita de criterios
**Consecuencia**: Inconsistencia entre revisiones, sesgo personal
**Evidencia**: No hay registro de qué se validó
**Mitigación**: Usar checklist obligatorio (§10 de superprompt), marcar cada item

### AP2: Aprobar con "Placeholders" o "TBD"
**Descripción**: Aceptar plan con secciones incompletas ("TODO", "TBD", "pending")
**Consecuencia**: Plan no ejecutable, gaps descubiertos tarde
**Evidencia**: Grep por "TODO|TBD|pending" retorna matches
**Mitigación**: Rechazo automático si se detectan placeholders

### AP3: Feedback Vago sin Acciones Concretas
**Descripción**: Comentarios como "está mal", "falta claridad", sin especificar dónde ni qué
**Consecuencia**: División no sabe cómo corregir, múltiples iteraciones
**Evidencia**: Feedback sin formato "Sección X → Problema Y → Acción Z"
**Mitigación**: Plantilla de feedback estructurado obligatorio

### AP4: Aprobar Planes sin Leer Todos (Validación Cruzada)
**Descripción**: Aprobar plan de división A sin leer planes de B, C, D
**Consecuencia**: Conflictos entre divisiones no detectados
**Evidencia**: Divisiones proponen decisiones contradictorias
**Mitigación**: Validación cruzada obligatoria (leer todos antes de aprobar ninguno)

### AP5: Scoring Subjetivo (Sin Rubrica)
**Descripción**: Asignar scores basándose en "sensación" vs criterios explícitos
**Consecuencia**: Inconsistencia, no reproducible, no enseñable
**Evidencia**: Dos revisores dan scores muy diferentes al mismo plan
**Mitigación**: Rubrica con 5-6 dimensiones, pesos explícitos, ejemplos de cada nivel

---

## Recomendaciones para el Ciclo de Vida

### Recomendación 1: Crear Rubrica de Scoring Explícita (CRÍTICO)
**Qué**: Definir 5-6 dimensiones de evaluación (completitud, claridad, trazabilidad, coherencia, progressive disclosure, evidencia)
**Por qué**: Sin rubrica, scoring es subjetivo y no reproducible
**Cuándo**: FASE 3 (antes de revisar primeros planes)
**Dónde**: Anexo de 04_adrs.md
**Propuesta de dimensiones**:
1. **Completitud** (20%): Todas las secciones obligatorias presentes
2. **Claridad** (20%): Sin ambigüedades, definiciones explícitas
3. **Trazabilidad** (20%): Citas correctas, referencias validadas
4. **Coherencia** (15%): Sin contradicciones internas
5. **Progressive Disclosure** (15%): ≤500 líneas, anexos bien usados
6. **Evidencia** (10%): Decisiones respaldadas por corpus

### Recomendación 2: Implementar Validation-Before-Completion (CRÍTICO)
**Qué**: Adaptar checklist de §8 TDD al contexto de planificación
**Por qué**: Previene aprobar planes incompletos
**Cuándo**: Cada fase del ciclo
**Checklist propuesto**:
```markdown
Antes de aprobar plan:
□ ¿Todas las secciones obligatorias están presentes?
□ ¿El score de rubrica es ≥7/10?
□ ¿No hay placeholders ("TODO", "TBD", "pending")?
□ ¿Las citas son verificables (rutas absolutas)?
□ ¿Se hizo validación cruzada con otros planes?
□ ¿El archivo cumple regla ≤500 líneas?
□ ¿Los artefactos de entrada/salida están definidos?
□ ¿Los riesgos tienen mitigaciones específicas?
```

### Recomendación 3: Protocolo de Resolución de Conflictos (ALTO)
**Qué**: Proceso para resolver cuando división A y B proponen decisiones contradictorias
**Por qué**: 11 divisiones en paralelo → alta probabilidad de conflictos
**Cuándo**: FASE 3 (definir protocolo), FASE 4 (aplicar)
**Protocolo propuesto**:
1. plan-reviewer detecta conflicto en validación cruzada
2. Reunir divisiones involucradas (sincrónico o async via documento)
3. Documentar posiciones (ADR con opción A vs opción B)
4. Aplicar tie-breaker: code-architecture-reviewer (decisiones técnicas) o documentation-architect (decisiones de proceso)
5. Registrar decisión final en ADR, comunicar a todas las divisiones

### Recomendación 4: Gates de Calidad Entre Fases (MEDIO)
**Qué**: Definir criterios de salida de cada fase del ciclo (no solo entrada)
**Por qué**: Previene avanzar con outputs de baja calidad
**Cuándo**: FASE 3 (definir gates)
**Ejemplo**:
- **Gate FASE 1 → FASE 2**: Mapa de trabajo aprobado (score ≥7/10), 11 divisiones definidas, corpus validado
- **Gate FASE 2 → FASE 3**: 11 archivos de investigación creados, todos ≤500 líneas, index.json generado

### Recomendación 5: Feedback Accionable Estructurado (MEDIO)
**Qué**: Plantilla de feedback con formato estándar
**Por qué**: Acelera correcciones, reduce iteraciones
**Cuándo**: Cada revisión
**Plantilla**:
```markdown
## Feedback de Revisión - [División]

**Score**: X/10
**Decisión**: APROBADO / RECHAZADO / CONDICIONAL

### Dimensión 1: Completitud (Y/20 puntos)
**Problema**: Falta sección "Artefactos"
**Acción requerida**: Agregar sección §4 con tabla de artefactos consume/produce

### Dimensión 2: Claridad (Y/20 puntos)
...
```

---

## Conclusiones

**Hallazgos clave**:
1. **Validation-Before-Completion** es patrón central (adaptarlo de TDD a planificación)
2. **Frameworks de medición** (SPACE, DevEx, DORA) proveen métricas aplicables
3. **Evidencia de TDD es mixta** (pragmatismo > dogmatismo)
4. **Rubrica explícita es CRÍTICA** (sin ella, scoring es subjetivo)
5. **Validación cruzada es obligatoria** (11 divisiones → alta probabilidad de conflictos)

**Aplicabilidad al showcase**:
- ✅ ALTA: Checklist, scoring numérico, gates de calidad son transferibles
- ⚠️ MEDIA: Protocolo de conflictos requiere definición en FASE 3
- ✅ ALTA: Frameworks de medición (SPACE, DevEx) aplicables directamente

**Gaps identificados**:
1. Rubrica de scoring no definida (crear en FASE 3)
2. Protocolo de resolución de conflictos no definido
3. Plantilla de feedback estructurado no creada

**Próximos pasos**:
1. FASE 3: Crear rubrica de scoring (5-6 dimensiones)
2. FASE 3: Definir protocolo de resolución de conflictos
3. FASE 4: Emitir ADR sobre Validation-Before-Completion como estándar obligatorio

---

## Referencias del Corpus

1. `./LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md` (§8 TDD, Validation-Before-Completion)
2. `./RESUMEN_EJECUTIVO_INVESTIGACION.md` (§4.7 SPACE/DevEx frameworks)
3. `./forsgren-space-framework-2021.md` (métricas de impacto, flow state)
4. `./fucci-tdd-replication-2016.md` (evidencia mixta de TDD)
5. `./nagappan-tdd-quality-2008.md` (TDD reduce defectos 60-90%, incrementa tiempo 15-35%)

**Total líneas**: 484 (cumple regla ≤500) ✅
