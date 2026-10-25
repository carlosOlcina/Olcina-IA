# Arquitectura — Qué significa "hacer un buen trabajo"

> Este documento define el estándar de calidad arquitectónica de **Olcina-IA**.
> Los agentes implementadores y revisores evalúan el código contra este archivo y contra `docs/OVERVIEW.md`.

## Principios Arquitectónicos

1. **Monorepo con Turborepo y pnpm:**
   - El proyecto utiliza **pnpm workspaces** y **Turborepo** (`turbo.json`) para orquestar `apps/*` y `packages/*`.
   - Ejecuta siempre las tareas a través de `pnpm run <script>` o `pnpm exec turbo run <task>` (p. ej. `pnpm run lint`, `pnpm run check-types`, `pnpm run build`).

2. **Separación clara de responsabilidades en el Monorepo:**
   - `apps/web/` — **Plataforma Web (Hub / Catálogo):** Aplicación Next.js 16 (App Router, React 19) para explorar el catálogo curado de *skills*, configuraciones y flujos SDD, visualizar su atribución y copiar comandos de instalación.
   - `apps/docs/` — **Portal de Documentación:** Aplicación Next.js 16 servida bajo `/docs` (configurada mediante rewrites en `vercel.json`) con la documentación técnica del ecosistema, del CLI y de los flujos de trabajo.
   - `packages/ui/` (`@repo/ui`) — **Librería de Componentes Compartidos:** Componentes de interfaz reutilizables entre `apps/web` y `apps/docs`, desacoplados de la lógica de rutas de cada app.
   - `packages/cli/` *(en construcción dentro del MVP)* — **Herramienta CLI de Olcina-IA:** Paquete ejecutable de Node.js/TypeScript para instalar y sincronizar *skills* y flujos completos de desarrollo (como SDD) en proyectos destino.
   - `packages/eslint-config/` y `packages/typescript-config/` — Configuraciones base estandarizadas para todo el workspace.

3. **Curación *Opinionated* y Atribución Estricta:**
   - Todo recurso del catálogo (*skill*, agente, regla o *workflow*) debe contar con metadatos estructurados claros.
   - Si una *skill* o flujo proviene de **terceros**, el modelo de datos, la UI del catálogo (`apps/web`) y la salida del CLI (`packages/cli`) **DEBEN preservar y mostrar siempre**:
     - Autor/a u organización original.
     - URL del repositorio o fuente original.
     - Licencia original.

4. **Interoperabilidad 100% con el Estándar Abierto de Skills:**
   - Las *skills* siguen la convención estándar `.agents/skills/<skill-name>/SKILL.md` y el manifiesto `skills-lock.json`.
   - El CLI de Olcina-IA actúa como un superconjunto compatible (capaz de instalar tanto *skills* estándar como flujos multi-agente SDD completos) sin introducir formatos propietarios incompatibles que rompan el estándar abierto.

5. **TypeScript Estricto (TypeScript 7):**
   - Prohibido el uso de `any` (utilizar `unknown` + refinamiento de tipos o validación de esquemas cuando la entrada sea dinámica).
   - Tipado explícito en contratos públicos, props de componentes exportados y funciones de dominio.

6. **React 19 y Next.js 16 (App Router):**
   - Preferir React Server Components (RSC) por defecto; usar `"use client"` únicamente en las hojas interactivas que requieran estado de navegador o eventos del DOM.
   - Seguir las prácticas de `vercel-react-best-practices`, `vercel-composition-patterns` y `design-taste-frontend` para lograr interfaces de alta calidad visual y rendimiento óptimo.

## Flujo del Ecosistema

```text
                ┌──────────────────────────────────────────┐
                │       Registro Curado (Olcina-IA)        │
                │  Skills (SKILL.md) + Workflows (SDD)     │
                │  + Metadatos de Autoría y Atribución     │
                └───────────┬──────────────────┬───────────┘
                            │                  │
              ┌─────────────▼──────┐    ┌──────▼─────────────┐
              │  Plataforma Web    │    │  Herramienta CLI   │
              │  (apps/web & docs) │    │  (packages/cli)    │
              │  Exploración y Docs│    │  Sync & Install    │
              └────────────────────┘    └──────┬─────────────┘
                                               │
                                               ▼
                                 ┌───────────────────────────┐
                                 │   Proyecto Destino        │
                                 │ .agents/skills/           │
                                 │ .agents/agents/ (SDD)     │
                                 │ AGENTS.md, docs/, specs/  │
                                 └───────────────────────────┘
```

## Qué NO hacer (Anti-patrones)

- ❌ **Omitir la atribución de terceros:** Nunca registrar ni renderizar una *skill* o configuración de terceros sin su autor original y enlace a la fuente.
- ❌ **Romper la compatibilidad del estándar abierto de *skills*:** No alterar la estructura requerida de `SKILL.md` ni de `skills-lock.json` de manera que deje de funcionar con herramientas estándar.
- ❌ **Implementar funcionalidades fuera de alcance (*Out of Scope*):** No implementar la capa de memoria persistente entre sesiones (documentada solo en el Backlog Exploratorio de `docs/OVERVIEW.md`) a menos que una feature aprobada lo indique explícitamente.
- ❌ **Dependencias circulares entre paquetes:** `packages/ui` no debe importar de `apps/web` ni de `apps/docs`.
- ❌ **Dejar `console.log()` de depuración o código muerto** en el repositorio.
