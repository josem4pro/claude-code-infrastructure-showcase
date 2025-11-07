# ANEXO A — FICHAS DETALLADAS DE LAS 11 DIVISIONES-AGENTE

**Fecha**: 2025-11-07
**Propósito**: Fichas completas de cada división-agente con responsabilidades, interfaces, artefactos, riesgos, KPIs y recomendaciones.
**Referencia**: Complemento de `01_mapa_trabajo.md`

---

## A1 — code-architecture-reviewer

**Propósito**: Garantizar consistencia arquitectónica y validar decisiones estructurales del plan.

**Límites**: NO implementa, solo revisa y valida arquitectura propuesta.

**Entradas**:
- Propuestas de arquitectura (layered, bounded contexts, etc.)
- Decisiones de estructura de directorios
- Patrones de diseño propuestos

**Salidas**:
- Validación de consistencia arquitectónica
- ADRs de arquitectura
- Matriz de trade-offs

**Interfaces con otras divisiones**:
- **plan-reviewer**: Recibe validación de consistencia
- **refactor-planner**: Provee guías de arquitectura objetivo
- **documentation-architect**: Provee ADRs arquitectónicos

**Artefactos que consume**: Propuestas de arquitectura, diagramas de contexto

**Artefactos que produce**: ADRs arquitectónicos, matrices de trade-offs

**Riesgos clave**:
- Over-engineering (arquitectura demasiado compleja para el problema)
- Violaciones de principios SOLID no detectadas
- Bounded contexts mal definidos

**KPIs de planificación**:
- Número de ADRs arquitectónicos emitidos
- Nivel de acoplamiento entre módulos (métrica cualitativa)
- Cobertura de patrones validados vs propuestos

**Anti-patrones a evitar**:
- Arquitectura "premature optimization"
- Mezclar capas (Routes accediendo directamente a Database)
- Bounded contexts sin límites claros

**Recomendaciones**:
- Aplicar Layered Architecture (Fowler 2002) como patrón por defecto
- Validar contra principios DDD (Evans 2015)
- Preferir simplicidad sobre flexibilidad prematura

**Referencias del corpus**:
- `LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md` (§9 Arquitectura de Capas)
- `evans-ddd-reference-2015-free.md` (Bounded Contexts, Layered Architecture)
- `CLAUDE_INTEGRATION_GUIDE.md` (Tech Stack Compatibility)

---

## A2 — plan-reviewer

**Propósito**: Oficina de revisión de planes, gating, criterios de aceptación, cierre consensuado.

**Límites**: NO ejecuta el plan, solo revisa y aprueba/rechaza con feedback estructurado.

**Entradas**:
- Planes de implementación (de todas las divisiones)
- Checklists de verificación
- Criterios de "Definition of Done"

**Salidas**:
- Aprobación/Rechazo del plan con justificación
- Checklist de calidad completado
- Score de calidad (escala 1-10)

**Interfaces con otras divisiones**:
- **TODAS**: Recibe planes de todas las divisiones para revisión
- **documentation-architect**: Provee feedback de estructura y completitud
- **code-architecture-reviewer**: Valida arquitectura del plan

**Artefactos que consume**: Planes, ADRs, matrices RACI, checklists

**Artefactos que produce**: Reportes de revisión, scores, aprobaciones/rechazos

**Riesgos clave**:
- Aprobar plan incompleto (falta de rigor)
- Bloquear plan por perfeccionismo excesivo
- Criterios de aceptación ambiguos

**KPIs de planificación**:
- Score promedio de calidad de planes (objetivo ≥7/10)
- Tasa de rechazo en primera revisión (indicador de claridad de criterios)
- Tiempo promedio de revisión

**Anti-patrones a evitar**:
- Revisar sin checklist estructurado
- Aprobar con "placeholders" o "TBD"
- Feedback vago sin acciones concretas

**Recomendaciones**:
- Aplicar "Verification-Before-Completion" pattern (§8 TDD del showcase)
- Emitir feedback accionable (no "está mal", sino "falta X en sección Y")
- Usar scoring numérico para objetividad

**Referencias del corpus**:
- `LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md` (§8 TDD, Validation-Before-Completion)
- `RESUMEN_EJECUTIVO_INVESTIGACION.md` (§10 hallazgos, frameworks de medición)
- `forsgren-space-framework-2021.md` (métricas de calidad)

---

## A3 — documentation-architect

**Propósito**: Ensamblar documento maestro, mantener índice, trazabilidad, progressive disclosure.

**Límites**: NO crea contenido técnico, solo estructura y consolida.

**Entradas**:
- Contenido de todas las divisiones
- Anexos especializados
- ADRs, matrices, diagramas

**Salidas**:
- PLAN_FINAL_CONSENSUADO.md (< 500 líneas)
- Índice maestro
- Estructura de anexos

**Interfaces con otras divisiones**:
- **TODAS**: Recibe contenido de todas las divisiones
- **plan-reviewer**: Recibe feedback de estructura
- **web-research-specialist**: Integra citas y referencias

**Artefactos que consume**: Contenido técnico, ADRs, matrices, referencias

**Artefactos que produce**: Documentos consolidados, índices, plantillas

**Riesgos clave**:
- Exceder 500 líneas (violación de progressive disclosure)
- Pérdida de trazabilidad entre decisiones y evidencias
- Inconsistencia de formato entre secciones

**KPIs de planificación**:
- Cumplimiento de regla 500 líneas (100% objetivo)
- Número de links rotos (0 objetivo)
- Nivel de trazabilidad (referencias citadas correctamente)

**Anti-patrones a evitar**:
- Documentos monolíticos sin división en anexos
- Falta de índice o tabla de contenidos
- Referencias sin rutas absolutas verificables

**Recomendaciones**:
- Aplicar Progressive Disclosure (§7 de LA_BIBLIA)
- Usar plantillas consistentes para todas las divisiones
- Mantener separación estricta Planificación vs Ejecución

**Referencias del corpus**:
- `LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md` (§7 Progressive Disclosure, regla 500 líneas)
- `sweller-cognitive-load-1988.md` (Cognitive Load Theory)
- `nngroup-progressive-disclosure-2006.md` (Patrón UX original)
- `ibm-progressive-disclosure-2024.md` (Implementación en sistemas enterprise)

---

## A4 — web-research-specialist

**Propósito**: Correlacionar evidencias del corpus, crear matrices comparativas, validar citas.

**Límites**: Solo corpus local (no búsqueda online), enfoque en correlación no en creación.

**Entradas**:
- Corpus completo (24 archivos .md)
- Preguntas de investigación de otras divisiones

**Salidas**:
- Matrices comparativas (frameworks, patrones, evidencia)
- Tabla de referencias por división
- Validación de citas y rutas

**Interfaces con otras divisiones**:
- **documentation-architect**: Provee referencias validadas
- **plan-reviewer**: Provee evidencia para decisiones
- **TODAS**: Responde a queries de investigación

**Artefactos que consume**: Corpus markdown, preguntas de investigación

**Artefactos que produce**: Matrices comparativas, tablas de referencias, reportes de correlación

**Riesgos clave**:
- Citas incorrectas o rutas rotas
- Interpretación errónea de papers académicos
- Sobrecarga de información (no priorizar relevancia)

**KPIs de planificación**:
- Cobertura del corpus (% archivos referenciados)
- Precisión de citas (0 citas incorrectas objetivo)
- Tiempo de respuesta a queries de investigación

**Anti-patrones a evitar**:
- Citar sin leer (referencias de segunda mano)
- Ignorar evidencia contradictoria
- Matrices sin criterios de comparación claros

**Recomendaciones**:
- Usar rutas absolutas para todas las referencias
- Priorizar papers peer-reviewed (ACM, IEEE) sobre blogs
- Marcar evidencia contradictoria explícitamente

**Referencias del corpus**:
- `RESUMEN_EJECUTIVO_INVESTIGACION.md` (50+ fuentes, metodología de investigación)
- `BIBLIOGRAFIA_INVESTIGACION.md` (proceso de búsqueda y selección)
- TODOS los papers académicos (sweller, fucci, nagappan, evans, forsgren)

---

## A5 — code-refactor-master

**Propósito**: Diseñar macro-refactors a nivel plan, definir patrón objetivo, evaluar impacto.

**Límites**: NO ejecuta refactor, solo planifica estrategia y riesgos.

**Entradas**:
- Código actual (descripción, no implementación)
- Patrón objetivo (ej: migrar de monolito a layered)

**Salidas**:
- Plan de refactor macro (fases, riesgos, rollback)
- Matriz de impacto (qué módulos afecta)
- Estrategia de mitigación de riesgos

**Interfaces con otras divisiones**:
- **refactor-planner**: Recibe estrategia, ejecuta diseño detallado
- **code-architecture-reviewer**: Valida patrón objetivo
- **plan-reviewer**: Recibe validación del plan de refactor

**Artefactos que consume**: Descripción de código actual, patrones objetivo

**Artefactos que produce**: Planes de refactor macro, matrices de impacto, ADRs de refactor

**Riesgos clave**:
- Subestimar complejidad del refactor
- No considerar rollback strategy
- Refactor "big bang" sin fases incrementales

**KPIs de planificación**:
- Número de fases del refactor (preferir incremental)
- Cobertura de riesgos identificados
- Claridad de criterios de éxito por fase

**Anti-patrones a evitar**:
- Refactor sin tests (cambiar comportamiento accidentalmente)
- Mezclar refactor con nuevas features
- No documentar decisiones (por qué se eligió patrón X)

**Recomendaciones**:
- Aplicar Strangler Fig Pattern para migraciones grandes
- Definir rollback strategy antes de iniciar
- Validar patrón objetivo con code-architecture-reviewer

**Referencias del corpus**:
- `LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md` (§9 Arquitectura de Capas, §15 Patrones de Diseño)
- `evans-ddd-reference-2015-free.md` (refactoring towards deeper insight)

---

## A6 — refactor-planner

**Propósito**: Estrategia de refactor a nivel plan, identificar deuda técnica prevista.

**Límites**: NO ejecuta, planifica estrategia y prioriza deuda técnica.

**Entradas**:
- Identificación de deuda técnica (de code-architecture-reviewer)
- Objetivos de calidad (de plan-reviewer)

**Salidas**:
- Plan estratégico de refactor (priorizado)
- Matriz de deuda técnica (severidad, impacto, esfuerzo)
- Estrategia de mitigación

**Interfaces con otras divisiones**:
- **code-refactor-master**: Provee estrategia, recibe diseño detallado
- **code-architecture-reviewer**: Recibe identificación de deuda técnica
- **plan-reviewer**: Recibe aprobación de estrategia

**Artefactos que consume**: Reportes de deuda técnica, objetivos de calidad

**Artefactos que produce**: Planes estratégicos de refactor, matrices de priorización

**Riesgos clave**:
- Priorizar refactor cosmético sobre crítico
- Subestimar esfuerzo de refactor
- Ignorar impacto en otras áreas

**KPIs de planificación**:
- Ratio deuda técnica crítica vs total
- Claridad de criterios de priorización
- Alineación con objetivos de negocio

**Anti-patrones a evitar**:
- Refactor sin objetivo claro (refactor por refactor)
- Priorizar por "dolor del desarrollador" sin impacto en negocio
- No considerar ROI del refactor

**Recomendaciones**:
- Usar matriz Severidad × Impacto para priorizar
- Alinear refactor con roadmap de features
- Documentar beneficios esperados (reducción de bugs, velocidad, etc.)

**Referencias del corpus**:
- `LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md` (§15 Patrones de Diseño, §20 Limitaciones)
- `sweller-cognitive-load-1988.md` (reducir carga cognitiva del código)

---

## A7 — frontend-error-fixer

**Propósito**: Calidad de UI a nivel plan: validación, estados de error, observabilidad.

**Límites**: NO implementa UI, define requisitos de calidad y manejo de errores.

**Entradas**:
- Requisitos de UX (validación, feedback, estados)
- Patrones de error handling (de backend)

**Salidas**:
- Checklist de calidad UI (validación, loading, error states)
- Requisitos de observabilidad (error tracking, analytics)
- Patrones de manejo de errores frontend

**Interfaces con otras divisiones**:
- **auth-route-debugger**: Coordina manejo de errores de autenticación
- **auto-error-resolver**: Provee reglas de validación TypeScript
- **plan-reviewer**: Recibe validación de checklist UI

**Artefactos que consume**: Requisitos de UX, patrones de error backend

**Artefactos que produce**: Checklists de calidad UI, patrones de error handling

**Riesgos clave**:
- Estados de error no contemplados (edge cases)
- Falta de feedback al usuario (loading sin indicador)
- Validación solo en frontend (sin validación backend)

**KPIs de planificación**:
- Cobertura de estados de error (% de flujos con error state)
- Completitud de validación (frontend + backend)
- Nivel de observabilidad (error tracking configurado)

**Anti-patrones a evitar**:
- Validación solo en frontend (security risk)
- Early returns con loading en vez de componentes (anti-pattern del showcase)
- Errores silenciosos (no notificar al usuario ni a Sentry)

**Recomendaciones**:
- Usar LoadingOverlay component en vez de early returns
- Integrar Sentry para error tracking (skill error-tracking)
- Validar tanto frontend como backend (defense in depth)

**Referencias del corpus**:
- `LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md` (§12.2 frontend-dev-guidelines)
- `CLAUDE_INTEGRATION_GUIDE.md` (Frontend Skills, React + MUI v7)

---

## A8 — auth-route-debugger

**Propósito**: Requisitos de autenticación, autorización, flujos de error y recuperación.

**Límites**: NO implementa auth, define requisitos y flujos a nivel plan.

**Entradas**:
- Requisitos de seguridad (roles, permisos, tokens)
- Flujos de autenticación (login, logout, refresh)

**Salidas**:
- Matriz de permisos (roles × recursos)
- Flujos de error de autenticación (401, 403, token expirado)
- Estrategia de recuperación (refresh token, re-login)

**Interfaces con otras divisiones**:
- **auth-route-tester**: Provee criterios de aceptación para testing
- **frontend-error-fixer**: Coordina manejo de errores de auth en UI
- **code-architecture-reviewer**: Valida diseño de auth

**Artefactos que consume**: Requisitos de seguridad, políticas de roles

**Artefactos que produce**: Matrices de permisos, flujos de error, ADRs de auth

**Riesgos clave**:
- Autorización insuficiente (privilege escalation)
- Tokens sin expiración o refresh
- Flujos de error no manejados (401/403 sin feedback)

**KPIs de planificación**:
- Cobertura de roles × recursos (100% objetivo)
- Claridad de flujos de recuperación
- Nivel de defense in depth (múltiples capas de validación)

**Anti-patrones a evitar**:
- Auth solo en frontend (security risk)
- Tokens en LocalStorage (XSS vulnerable, preferir httpOnly cookies)
- Rutas sin autenticación cuando deberían tenerla

**Recomendaciones**:
- Usar JWT cookies httpOnly (skill route-tester)
- Implementar refresh token strategy
- Validar permisos tanto en route como en service layer

**Referencias del corpus**:
- `LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md` (§12.4 route-tester, §13.8 auth-route-debugger)
- `CLAUDE_INTEGRATION_GUIDE.md` (route-tester skill, JWT cookie auth)

---

## A9 — auth-route-tester

**Propósito**: Contratos y criterios de aceptación para rutas y endpoints protegidos.

**Límites**: NO ejecuta tests, define contratos y criterios de validación.

**Entradas**:
- Especificación de rutas (métodos, paths, payloads)
- Matriz de permisos (de auth-route-debugger)

**Salidas**:
- Contratos de API (request/response schemas)
- Criterios de aceptación (happy path + error cases)
- Test scenarios (autenticado, no autenticado, permisos insuficientes)

**Interfaces con otras divisiones**:
- **auth-route-debugger**: Recibe matriz de permisos
- **plan-reviewer**: Recibe validación de contratos
- **auto-error-resolver**: Coordina validación TypeScript de schemas

**Artefactos que consume**: Especificaciones de rutas, matrices de permisos

**Artefactos que produce**: Contratos de API, criterios de aceptación, test scenarios

**Riesgos clave**:
- Contratos ambiguos (campos opcionales no especificados)
- Casos de error no contemplados (401, 403, 500)
- Falta de validación de schemas (payload incorrecto aceptado)

**KPIs de planificación**:
- Cobertura de endpoints (% con contrato definido)
- Cobertura de casos de error (happy path + error cases)
- Precisión de schemas (TypeScript types coinciden con API)

**Anti-patrones a evitar**:
- Contratos sin versioning
- Test scenarios solo para happy path
- Schemas desactualizados vs implementación real

**Recomendaciones**:
- Usar Zod para validación de schemas (skill backend-dev-guidelines)
- Definir contratos antes de implementación (contract-first)
- Incluir ejemplos de request/response en contratos

**Referencias del corpus**:
- `LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md` (§12.4 route-tester, §13.9 auth-route-tester)
- `CLAUDE_INTEGRATION_GUIDE.md` (route-tester skill)

---

## A10 — auto-error-resolver

**Propósito**: Reglas de higiene TypeScript/estático para futura ejecución, calidad y automatización.

**Límites**: NO ejecuta fixes, define reglas y criterios de calidad.

**Entradas**:
- Errores TypeScript frecuentes
- Patrones de código problemáticos

**Salidas**:
- Reglas de linting (ESLint, TypeScript strict)
- Criterios de auto-resolución (cuándo es seguro auto-fix)
- Checklist de validación pre-commit

**Interfaces con otras divisiones**:
- **plan-reviewer**: Provee checklist de validación
- **code-architecture-reviewer**: Valida reglas de arquitectura
- **frontend-error-fixer**: Coordina validación frontend

**Artefactos que consume**: Errores TypeScript, configuración de linting

**Artefactos que produce**: Reglas de linting, checklists de validación, ADRs de calidad

**Riesgos clave**:
- Auto-fix cambia comportamiento (unsafe)
- Reglas de linting demasiado estrictas (bloquean progreso)
- Validación solo en desarrollo (no en CI/CD)

**KPIs de planificación**:
- Número de reglas de linting definidas
- Tasa de auto-resolución segura (% de errores auto-fixables)
- Cobertura de validación (local + CI/CD)

**Anti-patrones a evitar**:
- Deshabilitar reglas con `@ts-ignore` sin justificación
- Linting inconsistente entre proyectos
- Validación TypeScript solo manual (no en hooks)

**Recomendaciones**:
- Usar stop hooks para validación pre-commit (§11.6 stop-build-check)
- Aplicar TypeScript strict mode
- Integrar validación en CI/CD pipeline

**Referencias del corpus**:
- `LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md` (§11.3 tsc-check, §11.6 stop-build-check)
- `fucci-tdd-replication-2016.md` (evidencia de TDD, aplicable a validación estática)

---

## A11 — Explore

**Propósito**: Exploración dirigida del codebase y corpus para descubrir zonas grises y riesgos desconocidos.

**Límites**: NO implementa, explora y reporta hallazgos.

**Entradas**:
- Corpus completo (24 archivos .md)
- Preguntas abiertas de otras divisiones

**Salidas**:
- Reporte de zonas grises (áreas no cubiertas por plan)
- Riesgos desconocidos identificados
- Mapeo de dependencias ocultas

**Interfaces con otras divisiones**:
- **TODAS**: Recibe queries de exploración
- **plan-reviewer**: Reporta riesgos críticos
- **web-research-specialist**: Coordina exploración de corpus

**Artefactos que consume**: Corpus, preguntas de exploración, código descriptivo

**Artefactos que produce**: Reportes de exploración, mapas de dependencias, riesgos identificados

**Riesgos clave**:
- No explorar suficiente (riesgos no identificados)
- Explorar sin foco (desperdicio de tiempo)
- Reportar hallazgos sin priorización

**KPIs de planificación**:
- Número de riesgos críticos identificados
- Cobertura de exploración (% del corpus explorado)
- Claridad de reportes (hallazgos accionables)

**Anti-patrones a evitar**:
- Exploración sin objetivo (explorar por explorar)
- Reportar hallazgos sin severidad/impacto
- No validar hallazgos antes de reportar

**Recomendaciones**:
- Usar thoroughness levels: "quick", "medium", "very thorough"
- Priorizar hallazgos por severidad × impacto
- Validar hallazgos con división correspondiente antes de escalar

**Referencias del corpus**:
- `LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md` (§13.11 Explore agent)
- TODO el corpus (potencial de exploración)

---

**FIN DE ANEXO A — FICHAS DETALLADAS**
**Líneas totales**: <500 (verificar)
**Referencia principal**: 01_mapa_trabajo.md
