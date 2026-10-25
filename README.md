# Olcina-IA

> **Ecosistema personal curado de IA: Plataforma Web (Hub/Catálogo) + Herramienta CLI para distribuir, instalar y sincronizar Agent Skills y flujos de trabajo (SDD).**

Documentación detallada de visión y alcance en [`docs/OVERVIEW.md`](docs/OVERVIEW.md) y guía operativa para agentes en [`AGENTS.md`](AGENTS.md).

---

## 1. ¿Qué es Olcina-IA? (Visión General)

**Olcina-IA** es un ecosistema compuesto por una **Plataforma Web (Hub/Catálogo)** y una **Herramienta CLI** (actualmente en fase de diseño y construcción inminente) pensado para centralizar, distribuir y gestionar mis setups personalizados de IA, *skills* y flujos de trabajo (*workflows*).

- **Plataforma Web (`apps/web` y `apps/docs`):** Catálogo interactivo y portal de documentación donde explorar las *skills*, configuraciones de agentes y flujos de desarrollo, consultar su funcionamiento y obtener los comandos de instalación.
- **Herramienta CLI (`packages/cli` — planeada en MVP):** Gestor de línea de comandos para instalar y mantener sincronizados en cualquier repositorio tanto *skills* individuales como flujos completos de desarrollo asistido por IA.

---

## 2. Curación y Autoría (Criterio de Selección)

El repositorio funciona como un **registro *opinionated* (curado por mí)**:

- Las *skills*, agentes y configuraciones incluidas son aquellas que considero mejores o más óptimas para mi flujo de trabajo de ingeniería.
- Los recursos pueden ser de **creación propia** o de **terceros**.
- **Atribución estricta obligatoria:** En caso de recursos de terceros (o adaptaciones de estos), es un requisito innegociable mantener y mostrar siempre en el registro, en la web y en el CLI:
  1. La **atribución y autoría original**.
  2. El **enlace directo a la fuente** o repositorio de origen.
  3. La licencia original del autor.

---

## 3. Capacidades del CLI e Interoperabilidad

El CLI de **Olcina-IA** está diseñado para:

1. **Instalar y sincronizar *skills* y *workflows* completos:** Permite incorporar tanto *skills* puntuales como flujos integrales de desarrollo — por ejemplo, flujos **SDD (Spec-Driven Development)** y el propio ciclo multi-agente que gobierna este proyecto (`leader`, `spec_author`, `implementer`, `reviewer`, `commiter`).
2. **Compatibilidad 100% con el estándar abierto de *skills*:** Actúa como un gestor extendido sin romper la compatibilidad con el estándar abierto de herramientas de *Agent Skills* existente (`.agents/skills/<name>/SKILL.md`, `skills-lock.json`, frontmatter estándar).

---

## 4. Estado Actual vs. Futuro (Roadmap)

### Corto Plazo (MVP — Alcance Actual)
- **Definición de flujos base:** Configuración y adaptación del flujo multi-agente **SDD (Spec-Driven Development)** en este repositorio.
- **Estructura del repositorio y registro:** Consolidación del monorepo Turborepo y esquema de metadatos/atribución para *skills* y *workflows*.
- **Catálogo Web (`apps/web` y `apps/docs`):** Interfaz web para explorar el registro curado y su documentación.
- **Desarrollo del CLI instalador (`packages/cli`):** Herramienta CLI compatible con el estándar abierto para instalar *skills* y flujos SDD.

### Ideas Futuras / Backlog Exploratorio *(Out of Scope por ahora)*
> **Nota importante:** Este punto se documenta únicamente como exploración futura; está en el aire y **no forma parte de la arquitectura actual ni del alcance del MVP**.
- **Capa de memoria persistente para agentes:** Evaluación futura de un flujo o capa de memoria persistente para que los agentes recuerden el contexto completo entre sesiones más allá del seguimiento en archivos del repositorio.

---

## 5. Estructura del Monorepo y Flujo SDD

Este proyecto utiliza **Turborepo** con **pnpm** y sigue **Spec-Driven Development (SDD)** para el desarrollo de sus funcionalidades:

| Ruta | Descripción |
|---|---|
| `apps/web` | Aplicación Next.js 16 — Hub / Catálogo Web de *skills* y *workflows* |
| `apps/docs` | Aplicación Next.js 16 — Documentación del ecosistema y guías SDD |
| `packages/ui` | Librería de componentes React compartidos (`@repo/ui`) |
| `packages/eslint-config` | Configuraciones de ESLint compartidas |
| `packages/typescript-config` | Configuraciones de TypeScript compartidas |
| `.agents/agents/` | Subagentes del ciclo SDD (`leader`, `spec_author`, `implementer`, `reviewer`, `commiter`, `skill-generator`, `secure-auditer`) |
| `.agents/skills/` | *Skills* activas en el workspace |
| `docs/` | Documentación estándar del proyecto (`OVERVIEW.md`, `architecture.md`, `conventions.md`, `specs.md`, `verification.md`) |
| `specs/` | Especificaciones Kiro-style por feature (`requirements.md`, `design.md`, `tasks.md`) |
| `progress/` | Seguimiento de sesión activa (`current.md`) e historial (`history.md`) |
| `feature_list.json` | Registro de features y su estado en el ciclo SDD |
| `CHECKPOINTS.md` | Criterios objetivos de verificación del estado del repositorio |

### Comandos Principales

```bash
# Instalar dependencias
pnpm install

# Ejecutar entorno de desarrollo (web en :3000, docs en :3001)
pnpm run dev

# Ejecutar linter en todos los paquetes
pnpm run lint

# Verificar tipos TypeScript en todo el monorepo
pnpm run check-types

# Compilar todas las aplicaciones y paquetes
pnpm run build
```
