# CHECKPOINTS — Evaluación del estado final

> En sistemas multi-agente no se evalúa el camino, se evalúa el destino.
> Estos son los checkpoints objetivos que un juez (humano o IA) puede usar
> para decidir si el proyecto está sano.

## C1 — El arnés está completo

- [ ] Existen los archivos base: `AGENTS.md`, `feature_list.json`,
      `progress/current.md`, `progress/history.md`.
- [ ] Existen los documentos base en `docs/`: `docs/OVERVIEW.md`,
      `docs/architecture.md`, `docs/conventions.md`, `docs/specs.md`,
      `docs/verification.md`.
- [ ] `pnpm run lint && pnpm run check-types` termina sin errores ni warnings.

## C2 — El estado es coherente

- [ ] Como mucho una feature en `in_progress` en `feature_list.json`.
- [ ] Toda feature `done` tiene tests asociados que pasan y validaciones verdes.
- [ ] `progress/current.md` está vacío (plantilla limpia) o describe la sesión activa
      (no contiene basura de sesiones anteriores).

## C3 — El código respeta la arquitectura y la curación

- [ ] `apps/web/`, `apps/docs/`, `packages/ui/` (y `packages/cli/` cuando exista) respetan las fronteras de paquetes definidas en `docs/architecture.md`.
- [ ] Toda *skill* o flujo de terceros registrado mantiene y muestra su atribución obligatoria (autoría original, licencia y enlace a la fuente).
- [ ] Se mantiene el 100% de compatibilidad con el estándar abierto de *Agent Skills* (`.agents/skills/<name>/SKILL.md` y `skills-lock.json`).
- [ ] No hay `console.log()` sueltos para debug, ni uso de `any`, ni TODOs sin contexto.

## C4 — La verificación es real

- [ ] Los paquetes y aplicaciones modificados cuentan con sus tests correspondientes.
- [ ] `pnpm run lint && pnpm run check-types` (y `pnpm exec turbo run test` / `pnpm run build` cuando aplique) terminan en verde.

## C5 — La sesión se cerró bien

- [ ] No hay archivos sin trackear sospechosos (`*.tmp`, artefactos temporales fuera de lugar).
- [ ] `progress/history.md` tiene una entrada por la última sesión cerrada.
- [ ] La última feature trabajada está reflejada en su estado correcto en `feature_list.json`.

## C6 — Spec Driven Development

- [ ] Toda feature con `"sdd": true` en estado `spec_ready`, `in_progress`
      o `done` tiene su carpeta `specs/<name>/` con los 3 archivos:
      `requirements.md`, `design.md`, `tasks.md`.
- [ ] `requirements.md` usa EARS estricto (ver `docs/specs.md`).
- [ ] Toda feature `done` con `"sdd": true` tiene todas sus tasks marcadas
      `[x]` en `tasks.md`.
- [ ] Cada `R<n>` de `requirements.md` está cubierto por al menos un test
      concreto en `apps/*` o `packages/*`.

---

**Cómo usar este archivo:** un agente revisor (`.agents/agents/reviewer.md`)
recorre cada checkbox, marca `[x]` o `[ ]` en su informe `progress/review_<name>.md`, y rechaza el cierre de sesión si quedan boxes vacíos en C1-C6.
