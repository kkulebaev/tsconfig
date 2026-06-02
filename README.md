<pre align="center">
████████╗███████╗ ██████╗ ██████╗ ███╗   ██╗███████╗██╗ ██████╗ 
╚══██╔══╝██╔════╝██╔════╝██╔═══██╗████╗  ██║██╔════╝██║██╔════╝ 
   ██║   ███████╗██║     ██║   ██║██╔██╗ ██║█████╗  ██║██║  ███╗
   ██║   ╚════██║██║     ██║   ██║██║╚██╗██║██╔══╝  ██║██║   ██║
   ██║   ███████║╚██████╗╚██████╔╝██║ ╚████║██║     ██║╚██████╔╝
   ╚═╝   ╚══════╝ ╚═════╝ ╚═════╝ ╚═╝  ╚═══╝╚═╝     ╚═╝ ╚═════╝ 
</pre>

<p align="center">
  <em>Zero-config TypeScript preset для Vue 3 + Vite + vue-tsc — одна строка <code>extends</code>, двадцать три аудированных <code>compilerOptions</code>.</em>
</p>

<div align="center">

<p>
  <a href="https://www.npmjs.com/package/@kkulebaev/tsconfig"><img alt="npm" src="https://img.shields.io/npm/v/@kkulebaev/tsconfig?style=for-the-badge&logo=npm&color=cb3837&logoColor=white"></a>
  <a href="https://www.npmjs.com/package/@kkulebaev/tsconfig"><img alt="downloads" src="https://img.shields.io/npm/dm/@kkulebaev/tsconfig?style=for-the-badge&logo=npm&color=cb3837&logoColor=white"></a>
  <a href="LICENSE"><img alt="license" src="https://img.shields.io/npm/l/@kkulebaev/tsconfig?style=for-the-badge&color=blue"></a>
  <a href="https://github.com/kkulebaev/tsconfig/actions/workflows/publish.yml"><img alt="publish" src="https://img.shields.io/github/actions/workflow/status/kkulebaev/tsconfig/publish.yml?style=for-the-badge&logo=githubactions&logoColor=white&label=ci"></a>
</p>

<p>
  <img alt="stable" src="https://img.shields.io/badge/API-stable-2da44e?style=flat-square">
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

Корневой `tsconfig.json` проекта заменяется на:

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

В локальном `tsconfig.json` остаются только project-specific поля (`paths`, `baseUrl`, `types`, `include`, `exclude`). Все флаги, уже заданные пресетом, удаляются.

## Included flags

| Flag | Value | Rationale |
|------|-------|-----------|
| `target` | `ES2023` | Современный JavaScript output; покрывает ES2023-методы (например, `Array.prototype.toReversed`) |
| `module` | `ESNext` | Native ESM-выхлоп для bundler'а Vite — `import`/`export` без транспайла в CommonJS, поддержка top-level await и динамического `import()` как промиса |
| `moduleResolution` | `Bundler` | Алгоритм резолва Vite/esbuild — учитывает `package.json` exports/conditions, поддерживает `paths`, не требует обязательных расширений в импортах, без legacy node10 fallback'ов. Требует TS 5.0+ |
| `lib` | `["ES2023","DOM","DOM.Iterable"]` | `ES2023` — встроенные типы ES2023 (`Array.prototype.toReversed`/`toSorted`/`with`, `Symbol.dispose`, hashbang grammar). `DOM` — браузерные API (`Document`, `Element`, `Window`, `fetch`, `localStorage` и т.д.). `DOM.Iterable` — итераторы DOM-коллекций (`NodeList[Symbol.iterator]`, `FormData.entries()`, `Headers.entries()`, `URLSearchParams.entries()`) |
| `jsx` | `preserve` | JSX-трансформацию выполняет Vue SFC compiler |
| `strict` | `true` | Включает все strict-флаги: `noImplicitAny`, `strictNullChecks`, `strictFunctionTypes`, `strictBindCallApply`, `strictPropertyInitialization`, `noImplicitThis`, `useUnknownInCatchVariables`, `alwaysStrict` |
| `noFallthroughCasesInSwitch` | `true` | Предотвращает случайные fallthrough в `switch`-блоках |
| `noImplicitReturns` | `true` | Все ветки функции должны возвращать значение (если есть хотя бы один `return value`). Ловит забытый `return` в `if/else` |
| `noImplicitOverride` | `true` | Требует `override` keyword при переопределении методов класса |
| `noErrorTruncation` | `true` | TS не обрезает длинные error-сообщения — облегчает дебаг complex generic-типов |
| `noUnusedLocals` | `true` | Ошибка на неиспользуемые локальные переменные |
| `noUnusedParameters` | `true` | Ошибка на неиспользуемые параметры функций (префикс `_` разрешён) |
| `noUncheckedIndexedAccess` | `false` | Явно выключен — польза от пометки `T \| undefined` при индексном доступе сомнительна относительно количества borrow-чек и `!`-assertion'ов, которые появляются в коде |
| `useDefineForClassFields` | `true` | Поля класса инициализируются через `Object.defineProperty` (ES native) |
| `verbatimModuleSyntax` | `true` | Type-only импорты должны быть явные (`import type { Foo }`) |
| `esModuleInterop` | `true` | Runtime-флаг: разрешает default-импорт CommonJS-модулей (`import fs from 'fs'`). Без него потребовался бы `import * as fs from 'fs'`. Меняет emitted JS — добавляет helper для unwrap'а default-экспорта |
| `allowSyntheticDefaultImports` | `true` | Type-level комплемент `esModuleInterop`: typecheck не блокирует default-импорт модуля без явного `default` export. На runtime не влияет, нужен только для согласия компилятора |
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
- **Проекты, эмитящие через `tsc`**: совместимость не гарантируется — требуется явный override `noEmit: false` и пересмотр каждого флага.

## Changelog

См. [CHANGELOG.md](CHANGELOG.md) или [GitHub Releases](https://github.com/kkulebaev/tsconfig/releases) — версии генерируются автоматически через [release-please](https://github.com/googleapis/release-please) на основе [Conventional Commits](https://www.conventionalcommits.org/).

## License

MIT © [Konstantin Kulebaev](mailto:konstantinkulebaev@gmail.com)
