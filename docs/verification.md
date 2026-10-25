# Verificación — Cómo demostrar que el trabajo funciona

> Regla de oro: **el agente no dice "funciona", lo demuestra**.
> Toda feature termina con evidencia ejecutable, no con afirmaciones.

## Niveles de verificación

### Nivel 1 — Lint y Comprobación de Tipos (obligatorio siempre)

Todo cambio en el monorepo debe pasar sin errores ni warnings (`--max-warnings 0`):

```bash
pnpm run lint
pnpm run check-types
```

### Nivel 2 — Tests automatizados (obligatorio para cerrar features)

Toda función de dominio, comando del CLI o componente relevante introducido en una feature debe contar con tests que:
1. Cubran el camino feliz (*happy path*).
2. Cubran al menos un caso de error o borde (*edge case*).

```bash
pnpm exec turbo run test
```

### Nivel 3 — Build del Monorepo (obligatorio cuando se tocan apps o paquetes compartidos)

Verifica que `apps/web`, `apps/docs` y los paquetes de `packages/*` compilan limpiamente:

```bash
pnpm run build
```

### Nivel 4 — Trazabilidad de requirements (obligatorio para features con `"sdd": true`)

Cada requisito `R<n>` definido en `specs/<name>/requirements.md` debe poder mapearse a al menos un test concreto en `apps/*` o `packages/*`. El agente `reviewer` verifica esta matriz de trazabilidad en `progress/review_<name>.md` y rechaza el cierre si algún `R<n>` carece de cobertura.

### Nivel 5 — Verificación en tiempo de ejecución en Next.js (cuando aplique a UI/Rutas)

Cuando se modifiquen rutas o componentes visuales en `apps/web` o `apps/docs`, utiliza la skill `next-dev-loop` o inspección en navegador si el entorno lo permite. Si el entorno headless no dispone de navegador para captura visual, documéntalo explícitamente en `progress/impl_<name>.md` para validación visual del humano en la segunda puerta de aprobación.

## Anti-patrones (no hacer)

- ❌ *"He añadido el componente / comando, debería funcionar"* → falta evidencia ejecutable.
- ❌ Tests superficiales que no comprueban el resultado real ni el requisito `R<n>`.
- ❌ Marcar una feature como `done` con `pnpm run lint` o `pnpm run check-types` en rojo.

## Verificación final antes de cerrar sesión

```bash
pnpm run lint && pnpm run check-types && pnpm run build
```

Si algún paso falla y no puede resolverse dentro del alcance de la sesión, **no** marques la feature como `done`: documenta el fallo en `progress/current.md` y marca el estado como `blocked` en `feature_list.json`.
