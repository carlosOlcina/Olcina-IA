# Olcina-IA — Visión General, Arquitectura y Alcance

> Documento fundacional del ecosistema **Olcina-IA**. Define qué es el proyecto, los criterios de curación y autoría, la arquitectura planeada (Hub Web + CLI), el estándar de interoperabilidad y el alcance actual frente a ideas exploratorias futuras.

---

## 1. Qué es el proyecto (Visión general)

**Olcina-IA** es un ecosistema centralizado pensado para definir, empaquetar, distribuir y sincronizar mis **setups personalizados de Inteligencia Artificial**, **Agent Skills** y **flujos de trabajo (workflows)** de ingeniería asistida por agentes.

El ecosistema se compone de dos pilares principales:

1. **Plataforma Web (Hub / Catálogo — `apps/web` y `apps/docs`):**
   Un portal y catálogo interactivo donde explorar, inspeccionar y documentar las *skills*, agentes, reglas y flujos de trabajo (como el ciclo **SDD — Spec-Driven Development**), incluyendo su propósito, estructura, comandos de instalación y metadatos de atribución.
2. **Herramienta CLI (En fase de diseño / construcción inminente — `packages/cli`):**
   Un gestor de línea de comandos diseñado para instalar, actualizar y sincronizar en cualquier proyecto tanto *skills* individuales como flujos completos de desarrollo multi-agente (incluyendo el arnés SDD que gobierna este mismo repositorio).

---

## 2. Curación y Autoría (Criterio de selección)

Este repositorio funciona como un **registro *opinionated* (curado personalmente)**:

- **Selección por excelencia práctica:** No busca ser un directorio masivo indiscriminado, sino una colección curada con las *skills*, prompts, agentes y configuraciones que considero mejores y más óptimas para mi flujo de trabajo diario.
- **Origen mixto (Propio + Terceros):** Los recursos del catálogo pueden ser de creación propia (desarrollados o adaptados a medida para mis flujos) o creados por terceros de la comunidad.
- **Requisito estricto de Atribución y Autoría:**
  > **Regla innegociable:** Cuando una *skill*, plantilla o configuración provenga de terceros (o sea una adaptación directa), **es obligatorio mantener, registrar y mostrar siempre la atribución completa**:
  > - Autoría original (nombre / organización / handle).
  > - Enlace directo a la fuente o repositorio original (`source` / `repository`).
  > - Licencia original e integridad de avisos de copyright.
  >
  > Tanto el manifiesto del registro (`skills-lock.json` / metadatos del catálogo), como la interfaz de la **Plataforma Web** y la salida informativa del **CLI**, deben visibilizar esta autoría de forma clara.

---

## 3. Capacidades del CLI e Interoperabilidad

El CLI de **Olcina-IA** está concebido como un **gestor extendido 100% compatible con el estándar abierto de Agent Skills**:

### 3.1. Qué permite instalar y sincronizar
- **Skills individuales (`SKILL.md`):** Instalación selectiva de *skills* con sus recursos asociados (`scripts/`, `references/`, `resources/`, `assets/`).
- **Flujos de trabajo completos (Workflows / Harnesses):** Capacidad de inicializar o sincronizar en un repositorio destino un ciclo completo de ingeniería con IA — en particular, flujos **SDD (Spec-Driven Development)** compuestos por:
  - Punto de entrada y mapa del agente (`AGENTS.md`, `CHECKPOINTS.md`).
  - Documentación estándar del arnés (`docs/specs.md`, `docs/architecture.md`, `docs/conventions.md`, `docs/verification.md`).
  - Subagentes especializados (`leader`, `spec_author`, `implementer`, `reviewer`, `commiter`, `skill-generator`, `secure-auditer`).
  - Estructura de estado y trazabilidad (`feature_list.json`, `progress/`, `specs/`).

### 3.2. Interoperabilidad con el Estándar Abierto
- **100% Compatible con el ecosistema existente:** Respeta íntegramente el formato estándar de `SKILL.md` (frontmatter YAML con `name`, `description`, etc.), la estructura `.agents/skills/<name>/SKILL.md` y el archivo de bloqueo `skills-lock.json`.
- **Sin lock-in ni rotura de herramientas estándar:** Cualquier *skill* instalada por el CLI puede ser leída por herramientas estándar del ecosistema (Antigravity, OpenCode, Claude Code, Cursor, Codex, etc.), y el CLI puede convivir con `skills-lock.json` sin romper la compatibilidad con el resto de gestores abiertos.

---

## 4. Arquitectura Planeada del Monorepo

El proyecto se estructura como un monorepo gestionado con **Turborepo** y **pnpm**:

```text
Olcina-IA/
├── apps/
│   ├── web/                  # Plataforma Web (Hub / Catálogo curado de Skills y Workflows) — Next.js 16
│   └── docs/                 # Portal de Documentación técnica y guías de flujos SDD — Next.js 16
├── packages/
│   ├── ui/                   # Sistema de diseño y componentes compartidos (@repo/ui)
│   ├── cli/                  # (En construcción MVP) CLI instalador de skills y workflows SDD
│   ├── eslint-config/        # Configuraciones compartidas de ESLint (@repo/eslint-config)
│   └── typescript-config/    # Configuraciones compartidas de TypeScript (@repo/typescript-config)
├── .agents/
│   ├── agents/               # Subagentes del flujo SDD (leader, spec_author, implementer, reviewer, commiter...)
│   └── skills/               # Skills instaladas y activas en este repositorio
├── docs/                     # Documentación estándar del proyecto y del arnés SDD
├── specs/                    # Especificaciones Kiro-style por feature (requirements, design, tasks)
├── progress/                 # Estado de la sesión actual (current.md) e historial (history.md)
├── AGENTS.md                 # Mapa principal de agentes y reglas del ciclo SDD
├── CHECKPOINTS.md            # Criterios de verificación objetiva del estado del proyecto
├── feature_list.json         # Backlog estructurado de features gobernadas por SDD
└── skills-lock.json          # Lockfile estándar de skills con trazabilidad de fuente y hash
```

---

## 5. Estado Actual vs. Futuro (Roadmap)

### 5.1. Corto Plazo — Alcance Actual (MVP)

El alcance activo y comprometido para el MVP comprende:

1. **Definición de flujos base e integración SDD:** Configuración del ciclo multi-agente **Spec-Driven Development** en el propio repositorio (`AGENTS.md`, `CHECKPOINTS.md`, `docs/*`, `.agents/agents/*`, `feature_list.json`, `progress/`, `specs/`).
2. **Estructura del repositorio y registro curado:** Organización del monorepo y definición del esquema de metadatos para *skills*, setups y *workflows* (incluyendo validación obligatoria de autoría/fuente en recursos de terceros).
3. **Catálogo Web (`apps/web` + `apps/docs`):** Construcción del Hub visual para explorar *skills* y flujos de trabajo, copiar comandos de instalación, consultar documentación y verificar la atribución original.
4. **Desarrollo del CLI instalador (`packages/cli`):** Implementación de la herramienta CLI capaz de instalar/sincronizar tanto *skills* compatibles con el estándar abierto como el flujo completo SDD en otros proyectos.

---

### 5.2. Ideas Futuras / Backlog Exploratorio *(Out of Scope por ahora)*

> [!NOTE]
> **Importante — Fuera del alcance actual:** La siguiente iniciativa se documenta **únicamente como idea exploratoria a futuro**. Está en fase de evaluación conceptual ("en el aire") y **NO forma parte de la arquitectura actual ni del MVP**. Ningún agente debe implementar ni asumir esta capa a menos que se promueva explícitamente en el futuro.

- **Capa de Memoria Persistente entre Sesiones para Agentes:**
  - *Concepto en evaluación:* Explorar la integración futura de un flujo o capa de memoria persistente que permita a los agentes recordar el contexto semántico completo, decisiones históricas y aprendizajes entre distintas sesiones de trabajo (más allá de los archivos estáticos `progress/history.md` y `specs/`).
  - *Estado:* **Out of Scope / En evaluación.** No debe incluirse en los diseños técnicos ni en las implementaciones del MVP actual.
