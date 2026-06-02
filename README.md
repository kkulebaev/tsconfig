<pre align="center">
████████╗███████╗ ██████╗ ██████╗ ███╗   ██╗███████╗██╗ ██████╗ 
╚══██╔══╝██╔════╝██╔════╝██╔═══██╗████╗  ██║██╔════╝██║██╔════╝ 
   ██║   ███████╗██║     ██║   ██║██╔██╗ ██║█████╗  ██║██║  ███╗
   ██║   ╚════██║██║     ██║   ██║██║╚██╗██║██╔══╝  ██║██║   ██║
   ██║   ███████║╚██████╗╚██████╔╝██║ ╚████║██║     ██║╚██████╔╝
   ╚═╝   ╚══════╝ ╚═════╝ ╚═════╝ ╚═╝  ╚═══╝╚═╝     ╚═╝ ╚═════╝ 
</pre>

<p align="center">
  <em>Zero-config TypeScript preset для Vue 3 + Vite + vue-tsc — одна строка <code>extends</code>, четырнадцать аудированных <code>compilerOptions</code>.</em>
</p>

<div align="center">

<p>
  <a href="https://www.npmjs.com/package/@kkulebaev/tsconfig"><img alt="npm" src="https://img.shields.io/npm/v/@kkulebaev/tsconfig?style=for-the-badge&logo=npm&color=cb3837&logoColor=white"></a>
  <a href="https://www.npmjs.com/package/@kkulebaev/tsconfig"><img alt="downloads" src="https://img.shields.io/npm/dm/@kkulebaev/tsconfig?style=for-the-badge&logo=npm&color=cb3837&logoColor=white"></a>
  <a href="LICENSE"><img alt="license" src="https://img.shields.io/npm/l/@kkulebaev/tsconfig?style=for-the-badge&color=blue"></a>
  <a href="https://github.com/kkulebaev/tsconfig/actions/workflows/publish.yml"><img alt="publish" src="https://img.shields.io/github/actions/workflow/status/kkulebaev/tsconfig/publish.yml?style=for-the-badge&logo=githubactions&logoColor=white&label=ci"></a>
</p>

<p>
  <a href="https://docs.npmjs.com/generating-provenance-statements"><img alt="provenance" src="https://img.shields.io/badge/provenance-verified-2da44e?style=flat-square&logo=sigstore&logoColor=white"></a>
  <img alt="tree shaking" src="https://img.shields.io/badge/runtime-zero-blueviolet?style=flat-square">
  <a href="https://vuejs.org/"><img alt="vue3" src="https://img.shields.io/badge/Vue-3-42b883?style=flat-square&logo=vue.js&logoColor=white"></a>
  <a href="https://vitejs.dev/"><img alt="vite" src="https://img.shields.io/badge/Vite-bundler-646cff?style=flat-square&logo=vite&logoColor=white"></a>
  <a href="https://github.com/vuejs/language-tools"><img alt="vue-tsc" src="https://img.shields.io/badge/vue--tsc-tested-42b883?style=flat-square&logo=vue.js&logoColor=white"></a>
  <a href="https://www.typescriptlang.org/"><img alt="typescript" src="https://img.shields.io/badge/TypeScript-%E2%89%A55.0-3178c6?style=flat-square&logo=typescript&logoColor=white"></a>
</p>

</div>

---

## Install

```sh
pnpm add -D @kkulebaev/tsconfig typescript
```

## Usage

Замени корневой `tsconfig.json` проекта на:

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

Оставь только project-specific поля (`paths`, `baseUrl`, `types`, `include`, `exclude`). Любые флаги, которые уже задаются пресетом, нужно удалить.

## Included flags

| Flag | Value | Rationale |
|------|-------|-----------|
| `target` | `ES2023` | Современный JavaScript output; покрывает ES2023-методы (например, `Array.prototype.toReversed`) |
| `module` | `ESNext` | Нативный ESM для bundler'а Vite |
| `moduleResolution` | `Bundler` | Резолвит импорты так же, как Vite/esbuild (TS 5.0+) |
| `lib` | `["ES2023","DOM","DOM.Iterable"]` | `DOM.Iterable` даёт типы для итераторов `FormData`/`Headers`/`NodeList` — активно используется в API-слое |
| `jsx` | `preserve` | JSX-трансформацию выполняет Vue SFC compiler |
| `strict` | `true` | Полный strict mode — `noImplicitAny`, `strictNullChecks` и компания |
| `noFallthroughCasesInSwitch` | `true` | Предотвращает случайные fallthrough в `switch`-блоках |
| `noUncheckedIndexedAccess` | `false` | Явно отложен в `./vue-strict` v0.2 (требует grep-аудита перед включением) |
| `esModuleInterop` | `true` | Корректные default-импорты из CommonJS-модулей |
| `allowSyntheticDefaultImports` | `true` | Идёт в паре с `esModuleInterop` для совместимости на уровне типов |
| `forceConsistentCasingInFileNames` | `true` | Защищает от cross-OS багов с регистром в импортах |
| `resolveJsonModule` | `true` | Типизированные импорты `.json`-файлов |
| `isolatedModules` | `true` | Обязательно для single-file transpilation модели Vite (`const enum` несовместим) |
| `skipLibCheck` | `true` | Пропускает type-check `.d.ts` файлов в `node_modules` |
| `allowImportingTsExtensions` | `true` | Разрешает `import './foo.ts'` (скрипты, сгенерированный код) |
| `noEmit` | `true` | Идёт в паре с предыдущим — `tsc` только type-check'ает, JS эмитят Vite/esbuild |

## Compatibility

Пресет рассчитан на **`--noEmit` stack**: JavaScript эмитят Vite/esbuild, `vue-tsc` используется только для type-check.

- **Vue 3 + Vite + vue-tsc**: полностью поддержано, battle-tested.
- **TypeScript**: требуется `>=5.0` (`peerDependency`) — `exports`-based `extends` resolution и массивный `extends` появились в TS 5.
- **Runtime**: `target: ES2023` требует Chrome 110+, Safari 16.4+, Node 20+. Vite может занижать через `build.target` в `vite.config.ts`.
- **Проекты, эмитящие через `tsc`**: совместимость не гарантируется — потребуется явный override `noEmit: false`. Если эмит делает `tsc`, пересмотри каждый флаг.

## Version pinning

| Pin | Use when |
|-----|----------|
| `^0.1.0` *(recommended)* | Разрешает patch/minor обновления в пределах `0.x` — для обычных проектов |
| `0.1.0` *(exact)* | Для критичных или stabilizing проектов, где каждое изменение флага — осознанное решение |

Добавление флагов, которые могут создать новые классы ошибок, всегда выходит **minor** релизом и описывается в release notes.

## Roadmap — v0.2

Отдельный пресет `./vue-strict`, layered on top of `./vue`. Каждый строгий флаг включается только после явного grep-аудита consumer-кода:

- `verbatimModuleSyntax: true`
- `noUncheckedIndexedAccess: true`
- `noUnusedLocals: true`
- `noUnusedParameters: true`
- `useDefineForClassFields: true`
- `noImplicitOverride: true`
- возможно `exactOptionalPropertyTypes: true`

Дальше планируются `./react`, `./node` — поверх общей базы `./base`.

## License

MIT © [Konstantin Kulebaev](mailto:konstantinkulebaev@gmail.com)
