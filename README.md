# @kkulebaev/tsconfig

[![npm](https://img.shields.io/npm/v/@kkulebaev/tsconfig.svg)](https://www.npmjs.com/package/@kkulebaev/tsconfig)
[![publish](https://github.com/kkulebaev/tsconfig/actions/workflows/publish.yml/badge.svg)](https://github.com/kkulebaev/tsconfig/actions/workflows/publish.yml)

Shared TypeScript config presets for Vue 3 + Vite + vue-tsc projects.

## Install

```sh
pnpm add -D @kkulebaev/tsconfig typescript
```

## Usage

In your `tsconfig.json`:

```json
{
  "extends": "@kkulebaev/tsconfig/vue",
  "include": ["src/**/*", "env.d.ts"],
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

Keep only project-specific overrides (`paths`, `baseUrl`, `types`, `include`, `exclude`). Remove any flags already provided by the preset.

## Included flags

| Flag | Value | Rationale |
|------|-------|-----------|
| `target` | `ES2023` | Modern JS output; covers ES2023 methods (e.g. `Array.prototype.toReversed`) used in consumer projects |
| `module` | `ESNext` | Native ESM for Vite's bundler |
| `moduleResolution` | `Bundler` | Resolves imports the way Vite/esbuild does (TS 5.0+) |
| `lib` | `["ES2023","DOM","DOM.Iterable"]` | `DOM.Iterable` — типизация FormData/Headers/NodeList, активно используется в API-слое |
| `jsx` | `preserve` | Vue SFC compiler handles JSX transform |
| `strict` | `true` | Full strict mode: noImplicitAny, strictNullChecks, etc. |
| `noFallthroughCasesInSwitch` | `true` | Prevents accidental fallthrough in switch statements |
| `noUncheckedIndexedAccess` | `false` | Explicitly deferred to v0.2 `./vue-strict` after grep-audit |
| `esModuleInterop` | `true` | Correct default imports from CommonJS modules |
| `allowSyntheticDefaultImports` | `true` | Pairs with esModuleInterop for type-level compat |
| `forceConsistentCasingInFileNames` | `true` | Guards against cross-OS import casing bugs (TS 5+ default) |
| `resolveJsonModule` | `true` | Enables typed imports of `.json` files |
| `isolatedModules` | `true` | Required for Vite's single-file transpilation model |
| `skipLibCheck` | `true` | Skip type-checking of `.d.ts` files in `node_modules` |

## Version pinning policy

- **`^0.1.0` (recommended)** — allows patch/minor updates within `0.x`; suitable for most projects.
- **`0.1.0` (exact)** — pin to a specific version for critical or stabilizing projects where any flag change must be an explicit decision.

Breaking changes (flag additions that may create new error classes) will always bump the minor version and be documented in release notes.

## Constraints / Compatibility

This preset is designed for a **`--noEmit` stack**: Vite/esbuild handle JavaScript emit; `vue-tsc` is used solely for type-checking (run with `--noEmit`).

- **Vite + vue-tsc projects**: fully supported and tested.
- **Runtime requirement**: `target: ES2023` requires a runtime that supports ES2023 natively (Chrome 110+, Safari 16.4+, Node 20+). For current browserslist targets in typical Vue 3 + Vite projects this is already covered; Vite/esbuild can downcompile if needed by setting `build.target` in `vite.config.ts`.
- **Projects emitting via `tsc`**: the preset may require an explicit override `"noEmit": false` in your local `tsconfig.json`. Compatibility with `tsc`-emit workflows is **not guaranteed**, especially if the conditional `allowImportingTsExtensions + noEmit` flags are active (determined per-project via Step 4b grep audit at release time).

If your project emits via `tsc`, review each flag carefully and override as needed.

## Roadmap — v0.2

Planned additions in a new `./vue-strict` preset (each flag included only after explicit grep-audit of consumer projects — see Principle #1 lock-in):

- `verbatimModuleSyntax: true`
- `noUncheckedIndexedAccess: true`
- `noUnusedLocals: true`
- `noUnusedParameters: true`
- `useDefineForClassFields: true`
- `noImplicitOverride: true`
- possibly `exactOptionalPropertyTypes: true`

Each flag is included only after an explicit grep-audit and baseline measurement; grep commands and results are published in the release notes.

Additional planned presets: `./react`, `./node` (with a shared `./base` foundation).

## Releasing

CI publishes to npm with [provenance](https://docs.npmjs.com/generating-provenance-statements) via OIDC trusted publishing when a `v*.*.*` tag is pushed:

```sh
# bump version in package.json, then:
git tag v0.2.0
git push --tags
```

The workflow at `.github/workflows/publish.yml` verifies that the tag matches `package.json` version, validates `vue.json`, and publishes via `npm publish --provenance --access public`.

No npm token is stored in the repository. The npm registry verifies the GitHub Actions workflow identity directly. Configure the trusted publisher once at <https://www.npmjs.com/package/@kkulebaev/tsconfig/access> (Publishing access → Trusted publishers → GitHub Actions, repo `kkulebaev/tsconfig`, workflow `publish.yml`).

## License

MIT © [Konstantin Kulebaev](mailto:konstantinkulebaev@gmail.com)
