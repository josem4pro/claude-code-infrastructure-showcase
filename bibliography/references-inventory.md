# 📚 Infrastructure Showcase - References Inventory

**Fecha de análisis**: 2025-11-06
**Total de referencias**: 12
**Fuente**: LA_BIBLIA_INFRASTRUCTURE_SHOWCASE.md §23 (líneas 1125-1203)

---

## CLASIFICACIÓN POR TIPO

### 📘 LIBROS (3/12) - BAJA ACCESIBILIDAD
| ID | Título | Autor | Año | ISBN | Accesibilidad |
|----|--------|-------|-----|------|---------------|
| [1] | Patterns of Enterprise Application Architecture | Fowler, Martin | 2002 | 978-0321127426 | 🔴 Compra requerida |
| [2] | Domain-Driven Design | Evans, Eric | 2003 | 978-0321125217 | 🔴 Compra requerida |
| [3] | Clean Code | Martin, Robert C. | 2008 | 978-0132350884 | 🔴 Compra requerida |

**Estrategia**: Buscar excerpts oficiales, free chapters, o versiones de investigación en Library Genesis.

---

### 📄 PAPERS ACADÉMICOS (4/12) - ACCESIBILIDAD MIXTA
| ID | Título | Autores | Año | Fuente | URL/DOI | Accesibilidad |
|----|--------|---------|-----|--------|---------|---------------|
| [7] | Cognitive load during problem solving | Sweller, John | 1988 | Cognitive Science 12(2) | DOI: 10.1207/s15516709cog1202_4 | 🟡 Paywall probable |
| [8] | The SPACE of Developer Productivity | Forsgren et al. | 2021 | ACM Queue 19(1) | https://queue.acm.org/detail.cfm?id=3454124 | 🟢 URL directa |
| [11] | Realizing quality improvement through TDD | Nagappan et al. | 2008 | Empirical Software Eng. 13(3) | DOI: 10.1007/s10664-008-9062-z | 🟡 SpringerLink |
| [12] | External Replication on TDD Effects | Fucci et al. | 2016 | ArXiv | https://arxiv.org/abs/1611.05994 | 🟢 ArXiv libre |

**Estrategia**:
- ArXiv papers: descarga directa
- Journal papers: buscar en ResearchGate, Google Scholar PDF, author's websites

---

### 🌐 ARTÍCULOS WEB (3/12) - ALTA ACCESIBILIDAD
| ID | Título | Fuente | Año | URL | Accesibilidad |
|----|--------|--------|-----|-----|---------------|
| [4] | Progressive Disclosure | Nielsen Norman Group | 2006 | https://www.nngroup.com/articles/progressive-disclosure/ | 🟢 URL directa |
| [5] | Progressive Disclosure Pattern | IBM Design | 2024 | https://www.carbondesignsystem.com/patterns/progressive-disclosure-pattern/ | 🟢 URL directa |
| [6] | The Rails Doctrine | DHH (Ruby on Rails) | 2006 | https://rubyonrails.org/doctrine | 🟢 URL directa |

**Estrategia**: Descarga directa con `curl -L`.

---

### 💻 TECH BLOGS (2/12) - ACCESIBILIDAD MODERADA
| ID | Título | Fuente | Año | URL Provista | Accesibilidad |
|----|--------|--------|-----|--------------|---------------|
| [9] | GitHub Copilot Statistics | TechCrunch | 2025 | https://techcrunch.com/2025/01/github-copilot-statistics | 🟡 URL posible futura |
| [10] | I Tried 10 AI Coding Tools | Builder.io | 2024 | https://www.builder.io/blog/ai-coding-tools-comparison | 🟢 URL directa |

**Estrategia**:
- [10]: descarga directa
- [9]: URL parece futurística (estamos en 2025-11-06), buscar artículo más reciente de GitHub Copilot stats

---

## PRIORIZACIÓN DE DESCARGA

### GRUPO A: DESCARGA INMEDIATA (Alta prioridad, URLs directas)
1. [4] Nielsen Norman - Progressive Disclosure
2. [5] IBM Design - Progressive Disclosure Pattern
3. [6] Rails Doctrine
4. [8] Forsgren - SPACE Framework
5. [10] Builder.io - AI Coding Tools
6. [12] Fucci - TDD Replication (ArXiv)

**Total**: 6/12 (50%) - GARANTIZADOS

---

### GRUPO B: BÚSQUEDA REQUERIDA (Moderada dificultad)
1. [7] Sweller - Cognitive Load (1988)
   - **Estrategia**: Google Scholar → ResearchGate → Sci-Hub mirrors
2. [11] Nagappan - TDD Quality (2008)
   - **Estrategia**: SpringerLink → ResearchGate → Author website
3. [9] GitHub Copilot Stats
   - **Estrategia**: Web search "GitHub Copilot statistics 2024" en TechCrunch

**Total**: 3/12 (25%) - PROBABLE 70% éxito

---

### GRUPO C: ALTA DIFICULTAD (Libros comerciales)
1. [1] Fowler - Patterns of Enterprise Architecture
2. [2] Evans - Domain-Driven Design
3. [3] Martin - Clean Code

**Estrategia**:
1. Buscar free chapters oficiales (O'Reilly, Informit)
2. Buscar en Library Genesis / Anna's Archive (investigación académica)
3. Documentar ISBNs y links de compra oficial
4. Buscar summaries/excerpts en blogs técnicos

**Total**: 3/12 (25%) - PROBABLE 30% éxito (parcial)

---

## OBJETIVO DE COBERTURA

**Meta mínima**: 8/12 (67%)
- Grupo A: 6 garantizados
- Grupo B: 2 de 3 probable
- Grupo C: 0-1 (bonus si encontramos excerpts)

**Meta ideal**: 10/12 (83%)
- Grupo A: 6 garantizados
- Grupo B: 3 de 3
- Grupo C: 1 (excerpts)

---

## SIGUIENTE FASE

✅ FASE 1 COMPLETA - Análisis y clasificación terminado

**NEXT**: FASE 2 - Crear estructura de directorios

**Esperando confirmación para proceder con FASE 2...**
