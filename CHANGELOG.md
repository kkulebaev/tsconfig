# Changelog

Все значимые изменения этого пакета документируются в этом файле.

Формат основан на [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
проект следует [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.3.0] — 2026-06-02

### Added
- `noImplicitReturns: true` — все ветки функции с return-значением обязаны явно возвращать его.
- `noErrorTruncation: true` — TS не обрезает длинные error-сообщения, облегчая дебаг сложных generic-типов.

## [0.2.0] — 2026-06-02

### Added
- `noImplicitOverride: true` — требует `override` keyword при переопределении методов класса.
- `noUnusedLocals: true` — ошибка на неиспользуемые локальные переменные.
- `noUnusedParameters: true` — ошибка на неиспользуемые параметры функций (префикс `_` разрешён).
- `useDefineForClassFields: true` — поля класса инициализируются через `Object.defineProperty` (ES native semantics).
- `verbatimModuleSyntax: true` — type-only импорты должны быть явные (`import type { Foo }`).

## [0.1.2] — 2026-06-02

### Changed
- CI: Node 24 + `actions/checkout@v6` / `actions/setup-node@v6`, убран промежуточный `npm install -g npm@latest` (Node 24 уже включает npm 11+ с нативной поддержкой OIDC trusted publishing).
- README: ASCII-баннер, badges, разделы Install/Usage/Included flags/Compatibility/License.

## [0.1.1] — 2026-06-02

### Changed
- CI: добавлен `npm install -g npm@latest` для активации OIDC trusted publishing (npm 11.5.1+).
- CI: убран `registry-url` из `actions/setup-node`, чтобы не записывать `.npmrc` с пустым `NODE_AUTH_TOKEN`.

## [0.1.0] — 2026-06-02

Первый публичный релиз.

### Added
- Пресет `./vue` (`vue.json`) — 16 ключей `compilerOptions` для Vue 3 + Vite + vue-tsc стека:
  - `target: ES2023`, `module: ESNext`, `moduleResolution: Bundler`, `lib: ["ES2023","DOM","DOM.Iterable"]`, `jsx: preserve`.
  - `strict: true`, `noFallthroughCasesInSwitch: true`, `noUncheckedIndexedAccess: false` (явно).
  - `esModuleInterop: true`, `allowSyntheticDefaultImports: true`, `forceConsistentCasingInFileNames: true`, `resolveJsonModule: true`, `isolatedModules: true`, `skipLibCheck: true`.
  - Conditional `allowImportingTsExtensions: true` + `noEmit: true` для scripts/generated кода.
- Namespaced `exports`: `./vue` + `./package.json` — место под будущие подпресеты без breaking change.
- `peerDependencies.typescript: ">=5.0"` — `extends`-резолв через `exports`-map требует TS 5+.
- GitHub Actions workflow с OIDC trusted publishing (provenance attestations).

[Unreleased]: https://github.com/kkulebaev/tsconfig/compare/v0.3.0...HEAD
[0.3.0]: https://github.com/kkulebaev/tsconfig/compare/v0.2.0...v0.3.0
[0.2.0]: https://github.com/kkulebaev/tsconfig/compare/v0.1.2...v0.2.0
[0.1.2]: https://github.com/kkulebaev/tsconfig/compare/v0.1.1...v0.1.2
[0.1.1]: https://github.com/kkulebaev/tsconfig/compare/v0.1.0...v0.1.1
[0.1.0]: https://github.com/kkulebaev/tsconfig/releases/tag/v0.1.0
