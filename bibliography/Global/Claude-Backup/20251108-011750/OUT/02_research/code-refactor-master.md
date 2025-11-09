# code-refactor-master — Investigación FASE 2

**División**: code-refactor-master (A5)
**Fecha**: 2025-11-07
**Investigador**: Claude Sonnet 4.5 (DEEP_MODE)
**Archivos asignados**: LA_BIBLIA (§9 Arquitectura, §15 Patrones), evans-ddd-reference-2015

---

## Propósito y Límites

**Propósito**: Diseñar macro-refactors a nivel plan. Definir patrón objetivo, evaluar impacto en múltiples módulos, crear estrategia de migración con fases incrementales.

**Límites**: NO ejecuto refactor (eso es ejecución). Solo planifico estrategia, identifico riesgos, diseño rollback, defino criterios de éxito por fase.

**Diferencia con refactor-planner**: refactor-planner identifica deuda técnica y prioriza. Yo diseño el refactor macro (cómo migrar de patrón A a patrón B).

---

## Fuentes Relevantes y Hallazgos

### LA_BIBLIA §9: Layered Architecture

**Patrón fundacional** (`./LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md:§9`):
```
Routes → Controllers → Services → Repositories → Database
```

**Beneficios** (Fowler 2002):
- ✅ Testability: Cada capa testeable independientemente
- ✅ Maintainability: Cambios aislados a una capa
- ✅ Reusability: Services reutilizables en múltiples routes
- ✅ Clarity: Responsabilidad clara por capa

**Trade-offs**:
- ⚠️ Boilerplate: Más archivos que monolito
- ⚠️ Over-engineering: Para CRUD simple puede ser excesivo
- ⚠️ Learning curve: Desarrolladores junior requieren training

**Aplicación a refactor-master**: Este es el **patrón objetivo** por defecto. Refactors típicos:
- Monolito → Layered Architecture
- Fat Controllers → Thin Controllers + Services
- SQL en Routes → Repositories

### LA_BIBLIA §15: Patrones de Diseño

**Patrones identificados** (`./LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md:§15`):

1. **Plugin Architecture** (Skills como plugins)
   - Modularity: Componentes separados
   - Extensibility: Capacidades extensibles sin alterar core
   - Loose Coupling: Independencia entre host y plugins

2. **Layered Architecture** (4 niveles: Commands → Agents → Skills → Hooks)
   - Separation of Concerns: Cada nivel propósito único
   - Loose Coupling: Niveles inferiores no dependen de superiores
   - Progressive Complexity: Simple (Hooks) → Complejo (Commands)

3. **Convention over Configuration**
   - Defaults inteligentes (src/routes/, src/controllers/)
   - Reducción de decisiones innecesarias
   - Onboarding más rápido

4. **Progressive Disclosure**
   - Información en capas (≤500 líneas principales)
   - Deep dives en resources/
   - Reducción de cognitive load

**Aplicación a refactor-master**: Estos patrones guían diseño de refactors. Por ejemplo:
- Refactor de "configuración manual" → "Convention over Configuration"
- Refactor de "documentos monolíticos" → "Progressive Disclosure"

### evans-ddd-reference-2015: Refactoring Towards Deeper Insight

**Concepto clave** (`./evans-ddd-reference-2015-free.md`):
> "Refactoring is a process of reorganizing code that leaves behavior unchanged while making the design clearer."

**Bounded Contexts como unidades de refactor**:
- Refactorizar contextos completos (no funciones aisladas)
- Anticorruption Layers en fronteras de contexto
- Shared Kernel solo para contextos estrechamente relacionados

**Aggregates definen límites transaccionales**:
- Refactor para alinear transacciones con aggregates
- Un Repository por Aggregate Root (no por entidad)
- Value Objects sin identidad (candidates para extracción)

**Aplicación a refactor-master**:
- Definir bounded contexts antes de refactorizar
- Usar Aggregates para identificar límites transaccionales
- Extraer Value Objects para reducir complejidad

---

## Interfaces con Otras Divisiones

| División | Relación | Artefactos Intercambiados |
|---|---|---|
| **refactor-planner (A6)** | Recibo estrategia, ejecuto diseño detallado | INPUT: Plan estratégico de refactor, matriz de deuda técnica |
| **code-architecture-reviewer (A1)** | Valido patrón objetivo | OUTPUT: Diseño macro de refactor, patrón objetivo propuesto |
| **plan-reviewer (A2)** | Recibo validación del plan de refactor | OUTPUT: Plan de refactor macro con fases y rollback |
| **documentation-architect (A3)** | Proveo ADRs de refactor | OUTPUT: ADRs documentando decisiones de refactor |
| **Explore (A11)** | Recibo mapeo de dependencias ocultas | INPUT: Dependencias no evidentes que afectan refactor |

**Flujo típico**:
1. refactor-planner identifica deuda técnica → prioriza
2. code-refactor-master (yo) diseño macro-refactor → patrón objetivo, fases, rollback
3. code-architecture-reviewer valida → consistencia con arquitectura
4. plan-reviewer aprueba → score ≥7/10
5. documentation-architect documenta → ADRs, guías de implementación

---

## Artefactos

### Consume
- **Plan estratégico de refactor** (de refactor-planner)
- **Matriz de deuda técnica** (de refactor-planner)
- **Descripción de código actual** (de Explore o plan-reviewer)
- **Mapeo de dependencias** (de Explore)

### Produce
- **Plan de refactor macro** (documento con fases, riesgos, rollback)
- **Matriz de impacto** (qué módulos afecta el refactor)
- **Patrón objetivo documentado** (diagramas, ejemplos)
- **Estrategia de mitigación de riesgos** (por cada riesgo identificado)
- **ADRs de refactor** (decisiones clave con justificación)
- **Criterios de éxito por fase** (Definition of Done por fase)

---

## Riesgos Clave y Mitigaciones

### Riesgo 1: Subestimar Complejidad del Refactor (ALTO)
**Descripción**: Pensar que refactor es "simple rename" cuando afecta 50+ archivos
**Impacto**: Plan subestimado, refactor incompleto o abandonado
**Probabilidad**: Alta (sin análisis de dependencias)
**Mitigación**:
- Mapeo completo de dependencias (Explore agent)
- Matriz de impacto por módulo
- Buffer 30% en estimación de esfuerzo
- Fase piloto en módulo pequeño antes de refactor completo
**Owner**: code-refactor-master

### Riesgo 2: No Considerar Rollback Strategy (CRÍTICO)
**Descripción**: Iniciar refactor sin plan de cómo volver atrás si falla
**Impacto**: Sistema roto sin forma de recuperar
**Probabilidad**: Media (presión por avanzar rápido)
**Mitigación**:
- Rollback strategy OBLIGATORIA en plan de refactor
- Feature flags para refactors grandes
- Branching strategy (feature branch, no main)
- Tests de regresión antes de cada fase
**Owner**: code-refactor-master

### Riesgo 3: Refactor "Big Bang" sin Fases Incrementales (ALTO)
**Descripción**: Intentar refactor completo en una sola fase
**Impacto**: Alto riesgo, difícil de debuggear, imposible rollback parcial
**Probabilidad**: Alta (sin experiencia en refactors grandes)
**Mitigación**:
- **Strangler Fig Pattern**: Refactor incremental, sistema viejo y nuevo coexisten
- Fases de máximo 2 semanas cada una
- Gate de validación entre fases (tests, code review)
- Rollback independiente por fase
**Owner**: code-refactor-master

### Riesgo 4: Mezclar Refactor con Nuevas Features (MEDIO)
**Descripción**: Aprovechar refactor para agregar funcionalidad nueva
**Impacto**: Scope creep, difícil validar que comportamiento no cambió
**Probabilidad**: Media (tentación de "ya que estamos...")
**Mitigación**:
- **Regla estricta**: Refactor = zero cambios de comportamiento
- Validar con tests de regresión
- Nuevas features en pull request separado
- Code review enfocado en "comportamiento idéntico"
**Owner**: code-refactor-master

### Riesgo 5: No Documentar Decisiones de Refactor (MEDIO)
**Descripción**: Refactorizar sin explicar por qué se eligió patrón X sobre Y
**Impacto**: Próximo desarrollador no entiende arquitectura, revierte refactor
**Probabilidad**: Alta (falta de cultura de ADRs)
**Mitigación**:
- ADR OBLIGATORIO para cada decisión significativa
- Documentar trade-offs considerados
- Registrar alternativas evaluadas y por qué se descartaron
- Template: Context → Decision → Consequences
**Owner**: code-refactor-master + documentation-architect

---

## KPIs de Planificación

### KPI 1: Número de Fases del Refactor
**Métrica**: Cantidad de fases en plan de refactor macro
**Objetivo**: ≥3 fases (preferir incremental sobre big bang)
**Fórmula**: Contar fases en plan de refactor
**Frecuencia**: Por plan de refactor
**Interpretación**: 1 fase = big bang (riesgoso), 3+ fases = incremental (preferido)

### KPI 2: Cobertura de Riesgos Identificados
**Métrica**: % de riesgos con mitigación específica
**Objetivo**: 100% (todo riesgo tiene mitigación)
**Fórmula**: N(riesgos con mitigación) / N(riesgos identificados) × 100
**Frecuencia**: Por plan de refactor

### KPI 3: Claridad de Criterios de Éxito por Fase
**Métrica**: Cada fase tiene Definition of Done explícita
**Objetivo**: 100% de fases con DoD
**Fórmula**: N(fases con DoD) / N(fases totales) × 100
**Frecuencia**: Por plan de refactor

### KPI 4: Presencia de Rollback Strategy
**Métrica**: Plan incluye estrategia de rollback documentada
**Objetivo**: 100% (OBLIGATORIO)
**Fórmula**: Booleano (sí/no)
**Frecuencia**: Por plan de refactor
**Blocker**: Sin rollback strategy → plan rechazado por plan-reviewer

### KPI 5: Mapeo de Dependencias Completo
**Métrica**: Matriz de impacto cubre todos los módulos afectados
**Objetivo**: 100% de módulos afectados identificados
**Fórmula**: N(módulos en matriz) / N(módulos afectados reales) × 100
**Frecuencia**: Por plan de refactor

---

## Anti-patrones

### AP1: Refactor sin Tests (Cambiar Comportamiento Accidentalmente)
**Descripción**: Modificar código sin tests de regresión que validen comportamiento
**Consecuencia**: Bugs introducidos, comportamiento cambiado sin detectar
**Evidencia**: Tests no existen o no corren antes del refactor
**Mitigación**: **Regla**: Tests PRIMERO. Si no hay tests, crearlos ANTES de refactorizar
**Cita**: Fowler - "Refactoring leaves behavior unchanged" (requiere tests para validar)

### AP2: Mezclar Refactor con Nuevas Features
**Descripción**: Aprovechar refactor para agregar funcionalidad nueva
**Consecuencia**: Scope creep, imposible validar que comportamiento no cambió
**Evidencia**: Pull request con refactor + feature nueva en mismo commit
**Mitigación**: Pull requests separados (refactor vs feature), code review rechaza mezcla

### AP3: No Documentar Decisiones (Por Qué Patrón X)
**Descripción**: Refactor sin explicar trade-offs o alternativas consideradas
**Consecuencia**: Próximo desarrollador no entiende arquitectura, revierte cambios
**Evidencia**: No hay ADR para decisión de refactor
**Mitigación**: ADR OBLIGATORIO, template: Context → Decision → Consequences

### AP4: Big Bang Refactor (Todo en Una Fase)
**Descripción**: Intentar refactor completo sin fases incrementales
**Consecuencia**: Alto riesgo, imposible rollback parcial, difícil debuggear
**Evidencia**: Plan con 1 sola fase de "refactor completo"
**Mitigación**: Strangler Fig Pattern, fases de máximo 2 semanas, gates de validación

### AP5: Refactor sin Patrón Objetivo Claro
**Descripción**: "Refactorizar para limpiar código" sin definir arquitectura objetivo
**Consecuencia**: Refactor inconsistente, cada desarrollador interpreta diferente
**Evidencia**: No hay diagrama o documento de patrón objetivo
**Mitigación**: Documentar patrón objetivo (diagramas, ejemplos, ADR)

---

## Recomendaciones para el Ciclo de Vida

### Recomendación 1: Strangler Fig Pattern para Migraciones Grandes (CRÍTICO)
**Qué**: Patrón de Martin Fowler para refactor incremental
**Cómo**:
1. Crear nueva implementación (patrón objetivo) en paralelo a la vieja
2. Redirigir tráfico gradualmente (feature flags, routing)
3. Deprecar componentes viejos uno a uno
4. Eliminar código viejo cuando 100% migrado

**Por qué**: Minimiza riesgo, permite rollback por componente, sistema funcional en todo momento
**Cuándo**: Refactors que afectan >10 archivos o >5 módulos
**Ejemplo**: Migrar de Fat Controllers a Layered Architecture
- Fase 1: Crear Service layer (coexiste con controllers)
- Fase 2: Migrar controller 1 (usa nuevo service)
- Fase 3: Migrar controller 2, etc.
- Fase N: Eliminar lógica de controllers (ahora thin)

### Recomendación 2: Matriz de Impacto Obligatoria (CRÍTICO)
**Qué**: Tabla de módulos afectados por el refactor
**Formato**:
```markdown
| Módulo | Impacto | Tests Afectados | Mitigación |
|---|---|---|---|
| auth.service.ts | ALTO | 12 tests | Refactor fase 1 |
| user.controller.ts | MEDIO | 5 tests | Refactor fase 2 |
| db.repository.ts | BAJO | 2 tests | Fase 3 (opcional) |
```

**Por qué**: Visualiza scope real del refactor, identifica dependencias
**Cuándo**: Antes de iniciar diseño de fases
**Herramienta**: Explore agent genera mapa de dependencias

### Recomendación 3: Criterios de Éxito por Fase (CRÍTICO)
**Qué**: Definition of Done explícita para cada fase
**Formato**:
```markdown
## Fase 1: Extraer Service Layer
**DoD**:
- [ ] auth.service.ts creado con tests unitarios (>80% coverage)
- [ ] Controller migrado (usa service, thin)
- [ ] Tests de integración pasan (0 regresiones)
- [ ] Code review aprobado (arquitectura consistente)
- [ ] Documentación actualizada (README, ADR)
```

**Por qué**: Sin DoD, fases se alargan indefinidamente o terminan incompletas
**Cuándo**: Al diseñar plan de refactor macro
**Validación**: plan-reviewer verifica que cada fase tiene DoD

### Recomendación 4: Rollback Strategy Documentada (BLOCKER)
**Qué**: Plan explícito de cómo volver atrás si refactor falla
**Opciones**:
1. **Feature Flags**: Toggle para activar/desactivar refactor
2. **Branching**: Refactor en feature branch, merge solo si exitoso
3. **Blue-Green Deployment**: Dos versiones en paralelo, switch de routing
4. **Database Migrations**: Migrations reversibles (up/down)

**Por qué**: Sin rollback, refactor fallido = sistema roto
**Cuándo**: ANTES de iniciar fase 1
**Validación**: plan-reviewer rechaza plan sin rollback strategy

### Recomendación 5: ADRs de Refactor (CRÍTICO)
**Qué**: Architecture Decision Record por cada decisión significativa
**Plantilla**:
```markdown
# ADR-XXX: Migrar a Layered Architecture

## Context
Código actual: Fat Controllers con lógica de negocio mezclada

## Decision
Adoptar Layered Architecture (Routes → Controllers → Services → Repositories)

## Alternatives Considered
1. Keep Fat Controllers (descartado: no testable)
2. Microservices (descartado: overkill para proyecto actual)
3. Hexagonal Architecture (descartado: demasiado complejo)

## Consequences
**Positives**:
- Testability: Services testeables independientemente
- Maintainability: Cambios aislados a una capa

**Negatives**:
- Boilerplate: +40% archivos
- Learning curve: 2 semanas training

## Rollback Strategy
Feature flags en cada controller (toggle viejo vs nuevo)
```

**Por qué**: Documenta trade-offs, previene reversiones futuras
**Cuándo**: Al diseñar patrón objetivo
**Herramienta**: documentation-architect ensambla ADRs

---

## Conclusiones

**Hallazgos clave**:
1. **Layered Architecture** es patrón objetivo por defecto (Routes → Controllers → Services → Repositories → DB)
2. **Strangler Fig Pattern** es estrategia recomendada para refactors grandes (incremental > big bang)
3. **Bounded Contexts** (DDD) definen unidades naturales de refactor
4. **Rollback strategy** es BLOQUEANTE (plan rechazado sin ella)
5. **ADRs** documentan decisiones y previenen reversiones

**Aplicabilidad al showcase**:
- ✅ ALTA: Layered Architecture aplicable a cualquier backend
- ✅ ALTA: Plugin Architecture aplicable a sistemas extensibles
- ✅ ALTA: Progressive Disclosure aplicable a documentación
- ⚠️ MEDIA: DDD Bounded Contexts (requiere domain modeling)

**Gaps identificados**:
1. No hay plantilla de "Plan de Refactor Macro" (crear en FASE 4)
2. No hay checklist de rollback strategies (crear en FASE 4)
3. No hay ejemplos de Matriz de Impacto (documentar en ADR)

**Próximos pasos**:
1. FASE 4: Crear plantilla de "Plan de Refactor Macro"
2. FASE 4: Emitir ADR sobre "Rollback Strategy Obligatoria"
3. FASE 4: Documentar Strangler Fig Pattern como estrategia por defecto

---

## Referencias del Corpus

1. `./LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md` (§9 Layered Architecture, §15 Patrones de Diseño)
2. `./evans-ddd-reference-2015-free.md` (Bounded Contexts, Aggregates, Refactoring Towards Deeper Insight)
3. `./sweller-cognitive-load-1988.md` (referenciado por refactor-planner para reducción de carga cognitiva)

**Total líneas**: 477 (cumple regla ≤500) ✅
