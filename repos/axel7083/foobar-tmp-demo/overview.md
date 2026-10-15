# axel7083/foobar-tmp-demo

A Podman Desktop extension template, used as macaron's sandbox.

- One source file: `src/extension.ts` (`activate` registers a webview).
- `npm run build` is `tsc`: it type-checks and emits in one step. There are no tests.
- `tsconfig.json` compiles to CommonJS with `resolveJsonModule`: `import pkg from '../package.json'` works, import attributes (`with { type: "json" }`) don't.
- The repository's own `AGENTS.md` comes first; this file only adds what it doesn't say.
