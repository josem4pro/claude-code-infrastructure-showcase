# ANEXO — CHECKLIST DE VERIFICACIÓN-ANTES-DE-COMPLETAR

**Propósito**: 12-item checklist para validar plan antes de aprobación final

**Usado en**: FASE 5 (Aprobación) del ciclo de vida

**Status actual**: Todas las casillas deben estar ✅ (marcar al validar)

---

## CHECKLIST MASTER (12 ITEMS)

### Item 1: Canon Organizacional ✅
**Criterio**: 11 divisiones-agente presentes, NO más NO menos

**Validación**:
```bash
# Buscar mención de cada división en el plan
grep -c "plan-reviewer" PLAN.md          # Debe ser > 0
grep -c "code-architecture-reviewer" PLAN.md  # Debe ser > 0
# ... etc (11 divisiones totales)
```

**Línea plan-reviewer**: Veo explícitamente los 11 agentes listados
- [ ] plan-reviewer (A2)
- [ ] code-architecture-reviewer (A1)
- [ ] documentation-architect (A3)
- [ ] web-research-specialist (A4)
- [ ] code-refactor-master (A5)
- [ ] refactor-planner (A6)
- [ ] frontend-error-fixer (A7)
- [ ] auth-route-debugger (A8)
- [ ] auth-route-tester (A9)
- [ ] auto-error-resolver (A10)
- [ ] Explore (A11)

---

### Item 2: Skills Correctamente Activados ✅
**Criterio**: Solo 29 skills permitidos, priorización explícita

**Validación**:
```bash
grep -c "verification-before-completion\|dispatching-parallel-agents\|systematic-debugging" PLAN.md
# Debe retornar ≥3 (estos 3 son CRÍTICOS)
```

**Checklist**:
- [ ] `verification-before-completion` = HIGH (activado)
- [ ] `dispatching-parallel-agents` = HIGH (activado)
- [ ] `systematic-debugging` = HIGH (activado)
- [ ] `test-driven-development` = Comentado (solo si generamos tooling + RED→GREEN→REFACTOR evidenciado)
- [ ] Ningún skill prohibido mencionado (no inventar skills)

---

### Item 3: Sin Placeholders ✅
**Criterio**: Cero "TODO", "TBD", "pending" en texto

**Validación**:
```bash
grep -E "TODO|TBD|pending|WIP|FIXME" PLAN.md
# Output debe estar VACÍO
```

**Checklist**:
- [ ] `grep "TODO" = 0 matches`
- [ ] `grep "TBD" = 0 matches`
- [ ] `grep "pending" = 0 matches` (en contexto de placeholder, no en "pending task")

---

### Item 4: RACI Completo por Etapa ✅
**Criterio**: 11 divisiones × 6 etapas = 66 asignaciones documentadas

**Validación**:
```bash
# Verificar tabla RACI está presente
grep "RACI" PLAN.md  # Debe tener tabla
# Verificar cada etapa tiene asignaciones
for ETAPA in Descubrimiento Arquitectura "Revisión Cruzada" Documentación Aprobación Handoff; do
  grep "$ETAPA" RACI-TABLE.md | wc -l  # Debe ser 11 (11 divisiones)
done
```

**Checklist**:
- [ ] Tabla RACI presente (11 filas × 6 columnas)
- [ ] Responsable (R) asignado por etapa
- [ ] Accountable (A) asignado por etapa
- [ ] Consulted (C) documentado (mínimo 1-2)
- [ ] Informed (I) documentado
- [ ] 0 celdas vacías

---

### Item 5: ADRs Presentes con Justificación ✅
**Criterio**: Mínimo 3-5 ADRs, todas con formato Context→Decision→Consequences

**Validación**:
```bash
find . -name "ADR-*.md" | wc -l  # Debe ser ≥3
grep -c "Context\|Decision\|Consequences" ADR-*.md  # Debe ser ≥9 (3 elementos × 3 ADRs)
```

**Checklist**:
- [ ] ADR-001 presente (patrón base)
- [ ] ADR-002 presente (arquitectura específica)
- [ ] ADR-003+ presente (decisiones clave)
- [ ] Cada ADR tiene: Context → Decision → Consequences
- [ ] Cada ADR cita alternativas consideradas
- [ ] 0 ADRs con justificación vaga

---

### Item 6: Rutas Absolutas Verificables ✅
**Criterio**: Todas las citas usan formato `./archivo.md:§sección`, NO "ver análisis anterior"

**Validación**:
```bash
# Buscar referencias vagas
grep -E "ver (el|la|anterior|análisis|documento)" PLAN.md | wc -l  # Debe ser 0
# Buscar rutas bien formadas
grep "./.*\.md:" PLAN.md | wc -l  # Debe ser >10
```

**Checklist**:
- [ ] 100% de referencias usan formato absoluto `./archivo.md:§sección`
- [ ] 0 referencias vagas ("ver documento", "análisis anterior")
- [ ] `markdown-link-check` pasa (0 links rotos)
- [ ] Rutas existen en corpus local (validar 24 archivos disponibles)

---

### Item 7: Progresive Disclosure (≤500 líneas) ✅
**Criterio**: Archivo principal <500 líneas, anexos bien usados

**Validación**:
```bash
wc -l PLAN_PRINCIPAL.md  # Debe ser <500
find ANEXOS -name "*.md" -exec wc -l {} \; | awk '$1 > 500'  # No debe retornar nada
```

**Checklist**:
- [ ] PLAN_PRINCIPAL.md < 500 líneas
- [ ] Todos los anexos < 500 líneas (cada uno)
- [ ] Índice/tabla de contenidos presente
- [ ] Links de navegación principal ↔ anexos funcionan
- [ ] Información complementaria está en anexos (no in line)

---

### Item 8: Artefactos Explícitos (Entrada/Salida) ✅
**Criterio**: Cada división documentó qué consume y qué produce

**Validación**:
```bash
# Por cada archivo de investigación FASE 2
for DIVISION in plan-reviewer code-architecture-reviewer ...; do
  grep "Consume:\|Produce:" 02_research/$DIVISION.md  # Debe retornar 2+ matches
done
```

**Checklist**:
- [ ] Cada división tiene sección "Artefactos que consume"
- [ ] Cada división tiene sección "Artefactos que produce"
- [ ] Artefactos entrada/salida coinciden entre divisiones (ej: A produce X → B consume X)
- [ ] Grafo de artefactos es acíclico (DAG, no circular)

---

### Item 9: Riesgos Identificados con Mitigaciones ✅
**Criterio**: Mínimo 5-10 riesgos, todos con mitigación específica (no genérica)

**Validación**:
```bash
grep -c "Riesgo\|Risk\|Problema" PLAN.md  # Debe ser ≥5
grep -E "Mitigación|Mitigation|Acción" PLAN.md | wc -l  # Debe ser ≥5
```

**Checklist**:
- [ ] ≥5 riesgos identificados (top-10 es mejor)
- [ ] CADA riesgo tiene mitigación específica (no genérica)
- [ ] Mitigaciones son accionables (no "hope for the best")
- [ ] Owner asignado a cada riesgo (quién es responsable)
- [ ] Severidad y probabilidad estimadas (ALTO/MEDIA/BAJA)

---

### Item 10: Validación Cruzada (Sine Conflictos Internos) ✅
**Criterio**: Plan coherente, sin contradicciones inter-divisiones

**Validación**:
```bash
# Leer TODOS los 11 planes en paralelo
# Buscar: División A dice X, División B dice ¬X
# Ejemplo: refactor-planner dice "Refactor fase 1", code-refactor-master dice "No durante fase 1"
```

**Checklist**:
- [ ] Leer todos 11 planes (FASE 2 archivos de investigación)
- [ ] Buscar contradicciones (División A vs B)
- [ ] Si hay conflictos: resolver usando Protocolo en ANEXOS/protocolo-conflictos.md
- [ ] Score de coherencia ≥7/10 (rubrica-scoring)
- [ ] Documento CONFLICT-*.md creado (si hubo conflictos)

---

### Item 11: Score Final ≥7/10 (Rubrica) ✅
**Criterio**: Plan evaluado con 6 dimensiones, resultado ≥7/10 (grado B)

**Validación**:
```bash
# Ejecutar scoring con rubrica en ANEXOS/rubrica-scoring.md
# Calcular: (Dim1×0.20 + Dim2×0.20 + Dim3×0.20 + Dim4×0.15 + Dim5×0.15 + Dim6×0.10)
# Resultado debe ser ≥7.0
```

**Checklist** (usar rubrica-scoring.md):
- [ ] Dim1 (Completitud) ≥7/10
- [ ] Dim2 (Claridad) ≥7/10
- [ ] Dim3 (Trazabilidad) ≥7/10
- [ ] Dim4 (Coherencia) ≥6/10 (peso menor)
- [ ] Dim5 (Progressive Disclosure) ≥7/10
- [ ] Dim6 (Evidencia) ≥6/10 (peso menor)
- [ ] **SCORE FINAL ≥7/10**

---

### Item 12: Aprobación Explícita de plan-reviewer ✅
**Criterio**: plan-reviewer FIRMA documento de aprobación (no implícito)

**Validación**:
```bash
# Buscar firma explícita
grep -c "APROBADO\|APPROVED\|Sign-off\|plan-reviewer.*apruer" PLAN.md  # Debe ser ≥1
```

**Checklist**:
- [ ] Documento de aprobación presente
- [ ] plan-reviewer FIRMA (nombre, fecha, score)
- [ ] Firma incluye: "APROBADO" + Fecha + Score + Firmante
- [ ] Correcciones requeridas (si <7/10) están listadas y completadas
- [ ] Ejemplo de firma esperada:
```
---
## APROBACIÓN FINAL

Score: 7.5/10 (Grado B)
Decisión: APROBADO
Acción: Proceder a FASE 6 (Handoff)

Firmado por: plan-reviewer
Fecha: 2025-11-07
```

---

## PROCEDIMIENTO DE VALIDACIÓN

**Paso 1**: plan-reviewer abre este checklist
**Paso 2**: Valida cada item (toma 30-60 min)
**Paso 3**: Marca ✅ cuando item pasa
**Paso 4**: Si alguno ✗: documento CORRECTION-LOG.md con acciones requeridas
**Paso 5**: Division hace correcciones (1-3 días)
**Paso 6**: plan-reviewer re-valida (solo items que fallaron)
**Paso 7**: Una vez todos ✅: FIRMA documento de aprobación

---

## APROBACIÓN FINAL

```markdown
# CHECKLIST VALIDATION REPORT

**Plan**: [Nombre]
**Revisor**: plan-reviewer
**Fecha Validación**: YYYY-MM-DD

## Items Validados

- [x] Item 1: Canon Organizacional
- [x] Item 2: Skills Correctamente Activados
- [x] Item 3: Sin Placeholders
- [x] Item 4: RACI Completo
- [x] Item 5: ADRs Presentes
- [x] Item 6: Rutas Absolutas
- [x] Item 7: Progressive Disclosure
- [x] Item 8: Artefactos Explícitos
- [x] Item 9: Riesgos + Mitigaciones
- [x] Item 10: Validación Cruzada
- [x] Item 11: Score ≥7/10
- [x] Item 12: Aprobación Explícita

**RESULTADO**: 12/12 ✅ **APROBADO**

**Próximo paso**: FASE 6 (Handoff a Ejecución)
```

---

**Líneas totales**: 329 (cumple ≤500) ✅
