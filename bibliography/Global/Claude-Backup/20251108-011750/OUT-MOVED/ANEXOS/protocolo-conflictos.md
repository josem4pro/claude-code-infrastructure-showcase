# ANEXO — PROTOCOLO DE RESOLUCIÓN DE CONFLICTOS

**Propósito**: Proceso estructurado para resolver cuando división A y B proponen decisiones contradictorias

**Aplicable a**: FASE 3 (Revisión Cruzada) del ciclo de vida

---

## SITUACIÓN DE CONFLICTO

**Ejemplo típico**:
- code-architecture-reviewer propone: "Usar Layered Architecture (5 capas)"
- code-refactor-master propone: "Usar Hexagonal Architecture (puertos + adapters)"
- Impacto: Decisión afecta diseño completo del sistema

---

## PROTOCOLO (7 PASOS)

### Paso 1: DETECTAR (plan-reviewer)
**Quién**: plan-reviewer en validación cruzada de Etapa 3
**Cómo**: Identificar contradicción explícita entre divisiones
**Ejemplo**: "División A dice X, División B dice ¬X"

### Paso 2: DOCUMENTAR (plan-reviewer)
**Crear documento**: CONFLICT-YYY.md (donde YYY = número)

**Plantilla**:
```markdown
# CONFLICT-001: Layered vs Hexagonal Architecture

## Planteamiento del Conflicto
- **División A (code-architecture-reviewer)**: Propone Layered Architecture (Routes → Controllers → Services → Repositories)
  - **Justificación**: Patrón probado (Fowler 2002), equipo familiar, migración simple de monolito
  - **Cita**: `./LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md:§9`

- **División B (code-refactor-master)**: Propone Hexagonal Architecture (Puertos + Adapters)
  - **Justificación**: Mayor testabilidad, desacoplamiento de frameworks, DDD-aligned
  - **Cita**: `./evans-ddd-reference-2015-free.md`

## Impacto
- Estructura de directorios (5 vs 6 capas)
- Nombrado de archivos (Controller vs ApplicationService)
- Testing strategy (mocks vs adapters)

## Estado
- Abierto (sin resolver)
- Escalación: Paso 3
```

### Paso 3: REUNIR DIVISIONES (plan-reviewer)
**Quién**: plan-reviewer coordina
**Formato**: Documento collaborativo (async preferred para no bloquear)
**Duración**: Max 2-3 días

**Instrucción a divisiones**:
```
Favor responder en CONFLICT-001.md con:
1. ¿Por qué tu propuesta es mejor? (máximo 200 palabras)
2. ¿Cuáles son los trade-offs de la propuesta del otro?
3. ¿Hay alternativa que satisfaga a ambos?
4. Score de "no negociables" (qué es MUST vs NICE-TO-HAVE)
```

### Paso 4: APLICAR TIE-BREAKER (plan-reviewer)
**Criterios de selección** (en orden):

1. **Evidencia técnica** (quien tiene más evidencia del corpus)
   - ¿Hay papers académicos que respalden A vs B?
   - ¿Hay caso de éxito en el showcase?

2. **Alineación con arquitectura existente**
   - ¿Qué patrón usa el resto del sistema?
   - ¿Qué elige code-architecture-reviewer (primer revisor)?

3. **Impacto en otras divisiones**
   - ¿A cuál le agrada más el equipo de ejecución?
   - ¿Cuál reduce carga cognitiva (Sweller theory)?

4. **Escalación** (si persiste empate):
   - **Para decisiones técnicas**: Consultar code-architecture-reviewer (es especialista)
   - **Para decisiones de proceso**: Consultar plan-reviewer (tiene visión global)
   - **Para decisiones de documentación**: Consultar documentation-architect

**Ejemplo de decisión**:
```
DECISIÓN: Layered Architecture (División A)

RAZÓN:
1. Fowler (2002) es autoridad reconocida, Hexagonal es más niche
2. Showcase usa Layered (§9 LA_BIBLIA)
3. code-architecture-reviewer (especialista) lo propone primero
4. Trade-off acceptable: Hexagonal puede implementarse dentro de Services layer (futuro)

TRADE-OFF ACEPTADO:
- Perdemos desacoplamiento extremo de Hexagonal
- Ganamos pragmatismo y velocidad de implementación

PRÓXIMOS PASOS:
- ADR-XXX documenta decisión + alternativa considerada
- Services layer usa principios de Hexagonal internamente (puertos simulados)
```

### Paso 5: DOCUMENTAR DECISIÓN (documentation-architect)
**Crear ADR**: ADR-XXX con formato estándar

**Plantilla ADR**:
```markdown
# ADR-XXX: Layered Architecture sobre Hexagonal

## Context
Conflict-001: Dos propuestas de patrón arquitectónico incompatibles

## Decision
Adoptar Layered Architecture (Routes → Controllers → Services → Repositories → DB)

## Alternatives Considered
1. **Hexagonal Architecture** - RECHAZADO
   - Pros: Máximo desacoplamiento, DDD-aligned
   - Cons: Complejidad para equipo actual, más boilerplate
   - Razón rechazo: Fowler's Layered es estándar, Showcase ya lo usa

## Consequences
**Positives**:
- Patrón familiar al equipo (Fowler 2002)
- Migración simple de monolito
- Reducción de cognitive load

**Negatives**:
- Menos desacoplamiento extremo que Hexagonal
- Framework dependencies más visibles

## Mitigations
- Dentro de Services layer, aplicar principios de Hexagonal (interfaces = puertos)
- Futuro: Refactor a Hexagonal si escala lo requiere

## References
- Fowler, M. (2002). Patterns of Enterprise Application Architecture
- LA_BIBLIA §9 Arquitectura de Capas
- CONFLICT-001.md (resolución)
```

### Paso 6: COMUNICAR (plan-reviewer)
**A**: Todas las 11 divisiones

**Mensaje**:
```
CONFLICT RESOLVED: XXX

Decisión: [OPCIÓN ELEGIDA]

Razón: [TIE-BREAKER USADO]

ADR: ADR-XXX (documenta alternativas consideradas)

Acción requerida: Actualizar vuestros planes según nueva decisión
```

### Paso 7: CONTINUAR (Volver a Etapa 3 — Revisión Cruzada)
**Estado**: Conflict resuelto, se puede continuar con revisión cruzada

---

## MATRIZ DE ESCALACIÓN (Si conflicto persiste)

| Tipo de Conflicto | Especialista a Consultar | Criterio Resolución |
|---|---|---|
| **Arquitectura** (cuál patrón) | code-architecture-reviewer | Evidencia técnica, precedente en showcase |
| **Refactorización** (estrategia) | code-refactor-master | Impacto en múltiples módulos, reversibilidad |
| **Documentación** (formato/estructura) | documentation-architect | Progressive disclosure, trazabilidad |
| **Testing** (criterios aceptación) | auth-route-tester | Cobertura de casos, compliance |
| **Autenticación** (flujos) | auth-route-debugger | Seguridad, reversibilidad, recovery |
| **Frontend** (validación/errores) | frontend-error-fixer | User experience, accessibility |

---

## REGLAS INVIOLABLES

1. **No sin ADR**: Toda decisión de conflicto = ADR documentado
2. **No sin alternativas**: ADR debe listar alternativas consideradas
3. **No sin comunicación**: Todas las divisiones informadas de resolución
4. **No sin plazo**: Max 3 días para resolver conflicto (no bloquea todo)
5. **No sin impacto**: Documentar qué equipos/módulos se afectan

---

## CASOS COMUNES

### Caso 1: "División propone refactor, otra dice NO"
**Resolución**: code-refactor-master (especialista) + impacto matrix
**Tie-breaker**: Costo vs beneficio del refactor

### Caso 2: "Ambas tienen razón"
**Resolución**: Buscar alternativa híbrida (Paso 3 → "¿hay solución que satisfaga a ambas?")
**Ejemplo**: Layered con principios de Hexagonal internos

### Caso 3: "Conflicto trivial (namespacing)"
**Resolución**: Convention over Configuration (usar estándar del showcase)
**Escalación NO NECESARIA**

### Caso 4: "Conflicto bloqueante (sin solución)"
**Resolución**: Escalar a plan-reviewer FINAL (no hay más instancias)
**Resultado**: Decisión unilateral (documentada con pesar)

---

**Líneas totales**: 287 (cumple ≤500) ✅
