# Convenciones de Código y Estructura

> Homogeneidad extrema. Los agentes trabajan con mayor precisión cuando el repositorio mantiene convenciones consistentes en todos sus paquetes.

## 1. Idioma y Estilo General

- **Código fuente, identificadores, comentarios y mensajes de commit técnicos:** usar **inglés** para nombres de variables, funciones, tipos, interfaces y archivos de código.
- **Documentación del proyecto y especificaciones (`docs/`, `specs/`, `progress/`):** en español o inglés consistente según el documento, manteniendo claridad técnica y notación EARS estricta en `requirements.md`.
- **Formato:** Prettier (`pnpm run format`) y ESLint (`pnpm run lint` con `--max-warnings 0`).

## 2. TypeScript (v7.x)

- **Strict mode activo:** heredado de `@repo/typescript-config/base.json`.
- **Cero `any`:** prohibido usar `any`. Usa `unknown` y estrecha el tipo mediante type guards o esquemas de validación.
- **Imports de tipos:** usa siempre `import type { ... }` para importar únicamente tipos o interfaces.
- **Módulos:** ESM (`import` / `export`), nunca CommonJS (`require` / `module.exports`).
- **Tipado explícito:** declara tipos de argumentos y retorno en funciones públicas exportadas entre módulos o paquetes.

## 3. Next.js 16 & React 19 (`apps/web`, `apps/docs`, `packages/ui`)

| Elemento                | Convención                  | Ejemplo                         |
|-------------------------|-----------------------------|---------------------------------|
| Rutas App Router        | `kebab-case` (`page.tsx`)   | `apps/web/app/skills/page.tsx`  |
| Componentes React       | `PascalCase.tsx`            | `SkillCard.tsx`                 |
| Hooks personalizados    | `use-<name>.ts` (`useX`)    | `useSkillFilter.ts`             |
| Utilidades / Dominio    | `kebab-case.ts`             | `parse-skill-manifest.ts`       |
| Tests                   | `*.test.ts` / `*.test.tsx`  | `SkillCard.test.tsx`            |

Reglas de componentes:
- Usa **named exports** (`export function SkillCard(...)`) para componentes de librería y UI; usa `export default` únicamente donde Next.js App Router lo exige (`page.tsx`, `layout.tsx`, `error.tsx`, `loading.tsx`).
- Mantén los componentes pequeños y con una única responsabilidad.

## 4. Estándar de Agent Skills y Workflows

Toda *skill* alojada o distribuida en este ecosistema debe respetar el estándar abierto:

- Ubicación en workspace: `.agents/skills/<skill-name>/SKILL.md`.
- El campo `name` del frontmatter YAML debe ser `kebab-case` (`^[a-z0-9]+(-[a-z0-9]+)*$`) y coincidir exactamente con el nombre de su carpeta.
- **Atribución obligatoria para terceros:**
  - En `skills-lock.json` o en el manifiesto del catálogo deben figurar `source`, `sourceType` y enlace/referencia al repositorio original.
  - En el propio `SKILL.md` o metadatos del catálogo, nunca eliminar los créditos ni licencias del autor original.

## 5. Testing y Manejo de Errores

- Toda feature nueva con `"sdd": true` debe incluir tests automatizados que verifiquen cada requisito `R<n>` de su especificación.
- Los errores deben manejarse de forma explícita y predecible (nunca silenciar errores con bloques `catch {}` vacíos ni exponer trazas internas al usuario final en la web o CLI).
