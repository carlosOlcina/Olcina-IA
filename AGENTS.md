<!-- BEGIN:turborepo-agent-rules -->

# This is NOT the Turborepo you know

Turborepo configuration, task behavior, and CLI commands can vary between installed versions and may differ from your training data. Resolve the `turbo` package from this file's directory or relevant workspace; in monorepos, it may not be visible from the repository root. For example, run `node -p "require.resolve('turbo/package.json')"` from a workspace that depends on `turbo`.

Read `docs/README.md` inside that installed package first, then read the relevant pages from its `docs/` directory before changing Turborepo configuration or commands. Heed deprecation notices. These bundled docs match the installed package version and are available without network access.

This block is written and re-added by `turbo` before repository-scoped commands when an AI agent is detected. In the Turborepo source repository, its template is defined in `crates/turborepo-cli/src/cli/agent_guidance.rs`. Removing the managed block while updates are enabled means a later qualifying invocation will add it again. Set `"agentGuidance": true` in the root `turbo.json` or `turbo.jsonc` to opt in; this does not remove an existing block. Keep the block committed with your work to avoid an uncommitted change on the next agent invocation.
<!-- END:turborepo-agent-rules -->

---

# Harness Engineering — Spec Driven Development (SDD)

> Este archivo es el **punto de entrada** para cualquier agente que trabaje en este
> repositorio. NO es una biblia de reglas: es un **mapa**. Lee solo lo que
> necesites cuando lo necesites (divulgación progresiva).

## 1. Antes de empezar (obligatorio)

1. Ejecuta `pnpm run lint && pnpm run check-types` para verificar el estado del monorepo.
2. Lee `progress/current.md` para entender en qué estado quedó la última sesión.
3. Lee `feature_list.json`. Toda feature nueva (`"sdd": true`) pasa por
   **Spec Driven Development** — ver `docs/specs.md` y §4 de este archivo.
4. Lee `docs/OVERVIEW.md` y `docs/specs.md` antes de tocar cualquier spec o feature con `"sdd": true`.

## 2. Mapa del repositorio

| Archivo / carpeta            | Qué contiene                                                                | Cuándo leerlo |
|------------------------------|-----------------------------------------------------------------------------|---------------|
| `docs/OVERVIEW.md`           | Visión general del ecosistema Olcina-IA (Hub Web + CLI), curación/autoría y alcance MVP vs futuro | Al iniciar contexto sobre el producto |
| `feature_list.json`          | Lista de tareas con estado (`pending` / `spec_ready` / `in_progress` / `done` / `blocked`) | Siempre, al empezar |
| `progress/current.md`        | Estado de la sesión actual                                                  | Siempre, al empezar |
| `progress/history.md`        | Bitácora append-only de sesiones anteriores                                 | Si necesitas contexto histórico |
| `specs/<feature>/`           | `requirements.md` + `design.md` + `tasks.md` (Kiro-style)                   | Antes de implementar cualquier feature con `"sdd": true` |
| `docs/architecture.md`       | Qué significa "hacer un buen trabajo" en este proyecto                      | Antes de implementar |
| `docs/conventions.md`        | Reglas de estilo, nombres, estructura y estándar de skills/atribución       | Antes de escribir código |
| `docs/specs.md`              | Proceso SDD: EARS notation, los 3 archivos, puertas de aprobación humana y cierre con PR | Antes de redactar o leer un spec |
| `docs/verification.md`       | Cómo verificar que tu trabajo funciona (incluye trazabilidad requirements)  | Antes de declarar una tarea como `done` |
| `CHECKPOINTS.md`             | Criterios objetivos de "estado final correcto"                              | Para auto-evaluarte |
| `.agents/agents/`            | Definiciones de subagentes (`leader`, `spec_author`, `implementer`, `reviewer`, `commiter`, `skill-generator`, `secure-auditer`) | Si orquestas trabajo |
| `.agents/skills/`            | Agent Skills instaladas en el proyecto                                      | Según la tecnología o tarea a abordar |
| `apps/web/`                  | Next.js 16 App: Plataforma Web (Hub / Catálogo de skills y workflows)       | Para implementar frontend del catálogo |
| `apps/docs/`                 | Next.js 16 App: Portal de documentación del ecosistema                      | Para implementar o actualizar docs web |
| `packages/ui/`               | Librería de componentes React compartidos (`@repo/ui`)                      | Para componentes compartidos entre `web` y `docs` |
| `packages/cli/`              | (Planeado en MVP) CLI instalador y sincronizador de skills y flujos SDD     | Para implementar la herramienta CLI |

## 3. Reglas duras (no negociables)

- **Una sola feature a la vez.** No mezcles cambios de varias tareas en la misma sesión.
- **No declares una tarea `done` sin validaciones y pruebas verdes.** Ejecuta
  `pnpm run lint && pnpm run check-types` (y `pnpm exec turbo run test` / `pnpm run build` según corresponda) y asegúrate de que todo pasa.
- **No saltes la fase de spec.** Toda feature con `"sdd": true` debe pasar
  por `spec_author` y obtener aprobación humana antes de tocar código.
- **No saltes las puertas de aprobación humana.** El `leader` detiene el flujo
  en `spec_ready` (aprobación del spec) y de nuevo tras el `APPROVED` del
  `reviewer` (aceptación de la feature), y espera.
- **No cierres una feature sin commits atómicos y PR.** Tras el OK humano,
  el `leader` lanza al `commiter`, que crea la rama `feat/<name>`, commitea en
  unidades atómicas, hace push y abre la PR contra `main`.
- **Atribución obligatoria en recursos de terceros.** Toda *skill* o flujo de
  terceros debe mantener y mostrar siempre su autoría original, licencia y enlace a la fuente.
- **Documenta lo que haces** en `progress/current.md` mientras trabajas, no al final.
- **Deja el repositorio limpio** antes de cerrar la sesión (ver §5).
- **Si no sabes algo, busca en `docs/`** antes de inventarlo.

## 4. Flujo de trabajo (SDD)

```text
pending → [spec_author] → spec_ready → ⏸ HUMANO APRUEBA → in_progress
       → [implementer → reviewer] → ⏸ HUMANO ACEPTA
       → [implementer: done] → [commiter: commits + PR]
```

1. El `leader` detecta la primera feature `pending` con `"sdd": true`.
2. El `leader` lanza `spec_author`, que crea
   `specs/<name>/{requirements,design,tasks}.md` y marca el status como
   `spec_ready`.
3. **Pausa.** El humano lee el spec en `specs/<name>/` y aprueba (o pide cambios).
4. Una vez aprobado, el `leader` cambia el status a `in_progress` y lanza `implementer`.
5. El `implementer` ejecuta `tasks.md` una a una, marcándolas `[x]`.
6. El `reviewer` verifica trazabilidad `R<n>` ↔ test y tasks completas;
   aprueba o rechaza. Si rechaza, el `leader` relanza al `implementer` con el
   feedback y repite el ciclo.
7. Si aprueba, **pausa de nuevo**: el `leader` presenta el veredicto y
   pregunta al humano si todo está OK o quiere modificar algo. Si pide
   cambios, se ajusta la feature (`implementer` si es dentro del spec;
   `spec_author` + nueva aprobación si cambia el spec) y se repite el paso 6.
8. Con el OK humano, el `implementer` marca `done` y mueve el resumen a
   `progress/history.md`.
9. El `leader` lanza al `commiter`, que crea la rama `feat/<name>`, hace
   commits atómicos, push y abre la PR contra `main`
   (`progress/pr_<name>.md`).

## 5. Cierre de sesión (lifecycle)

Antes de terminar:

1. Ejecuta `pnpm run lint && pnpm run check-types` (y tests/build aplicables) — todo verde.
2. Si la tarea está acabada y el humano ha dado el OK: marca
   `status: "done"` en `feature_list.json` (lo hace el `implementer`) y
   lanza al `commiter` para rama `feat/<name>`, commits atómicos, push y
   PR contra `main`.
3. Mueve el resumen de `progress/current.md` al final de `progress/history.md`.
4. Vacía `progress/current.md` dejando solo la plantilla.
5. No dejes archivos temporales, ni `console.log()` de debug, ni TODOs sin contexto.

## 6. Si te bloqueas

- Relee la sección relevante de `docs/`.
- Si la herramienta no hace lo que esperas, **no inventes un workaround**:
  documenta el bloqueo en `progress/current.md` y para la sesión.

---

# Información clave del proyecto

## 1. Datos sobre el proyecto

- **Nombre:** Olcina-IA
- **Qué es:** Ecosistema compuesto por una **Plataforma Web (Hub/Catálogo)** (`apps/web` y `apps/docs`) y una **Herramienta CLI** (`packages/cli`) para centralizar, distribuir, instalar y sincronizar mis setups personalizados de IA, *Agent Skills* y flujos de trabajo (*workflows* como este mismo ciclo SDD).
- **Criterio de curación y autoría:** Registro *opinionated* (curado por mí) con las mejores *skills* y configuraciones para mi flujo de trabajo. Pueden ser propias o de terceros; en las de terceros es **requisito estricto mantener y mostrar siempre la atribución, autoría original y enlace a la fuente**.
- **Interoperabilidad del CLI:** 100% compatible con el estándar abierto de herramientas de *skills* existente (`.agents/skills/<name>/SKILL.md`, `skills-lock.json`), actuando como gestor extendido para *skills* y *workflows* SDD sin romper la compatibilidad estándar.
- **Alcance actual vs. futuro:** El foco actual (MVP) es definir los flujos base, la estructura del repositorio, el catálogo web y el CLI instalador. La idea de una *capa de memoria persistente entre sesiones* está en el aire como **Backlog Exploratorio (Out of Scope)** — ver `docs/OVERVIEW.md`.

## 2. Reglas de estilo

- Todo el código generado debe seguir las siguientes reglas:
- Usar siempre el idioma **inglés** en el código fuente: comentarios de código, nombres de variables, funciones, clases, tipos y archivos.
- Priorizar la simplicidad sobre la complejidad.
- Evitar duplicación de código (DRY).
- Seguir el principio KISS.
- Aplicar SOLID cuando tenga sentido.
- No añadir comentarios innecesarios; el código debe ser autoexplicativo.
- Utilizar nombres descriptivos para variables, funciones, clases y archivos.
- Mantener funciones pequeñas y con una única responsabilidad.
- Evitar números y cadenas mágicas; utilizar constantes.
- No utilizar abreviaturas salvo que sean ampliamente conocidas.
- Mantener una estructura consistente en todo el proyecto.
- Respetar el formateo configurado por Prettier y las reglas de ESLint.
- No deshabilitar reglas del linter sin una justificación.
- No introducir dependencias nuevas sin que aporten un beneficio claro.
- Preferir composición frente a herencia cuando sea posible.
- Eliminar código muerto y código comentado.
- Evitar optimizaciones prematuras.
- Manejar los errores de forma explícita y consistente.
- No asumir valores nulos o indefinidos; validar cuando sea necesario.
- Mantener la compatibilidad con la arquitectura existente del proyecto.
- Generar código listo para producción.
- Antes de modificar código existente, entender su contexto.
- Reutilizar componentes y utilidades existentes antes de crear otros nuevos.
- No cambiar APIs públicas sin una justificación.
- Mantener compatibilidad hacia atrás cuando sea posible.
- No modificar archivos no relacionados con la tarea.
- Si existen varias soluciones válidas, elegir la más simple.
- Mantener el mismo estilo de código que el resto del proyecto.
- No generar código especulativo o no utilizado.
- Utilizar un tipado fuerte y explícito. Definir tipos, interfaces y genéricos cuando aporten claridad, evitando `any` y prefiriendo `unknown` cuando el tipo no pueda determinarse de forma segura.
- Finalizar siempre la tarea con una solución completa, evitando dejar TODOs salvo que se solicite.

## 3. Antes de empezar a trabajar (Carga de Skills)

Una vez tengas idea del contexto de la tarea, carga todas las *skills* que sean necesarias para esa tarea:

- Si la tarea toca `apps/web/`, `apps/docs/` o `packages/ui/` (componentes React, páginas Next.js, diseño o estilos), carga las skills relevantes de frontend/Next.js disponibles en `.agents/skills/`: `design-taste-frontend`, `minimalist-ui`, `vercel-react-best-practices`, `vercel-composition-patterns`, `next-dev-loop` o `next-cache-components-optimizer`.
- Si la tarea toca configuración de monorepo o pipelines, carga la skill `turborepo`.
- Si la tarea toca despliegues o configuración de Vercel, carga la skill `vercel-cli`.
