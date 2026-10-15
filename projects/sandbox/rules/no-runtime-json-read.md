---
title: Import package.json instead of reading it at runtime
severity: error
applies_to: ["src/**/*.ts"]
exclude: ["**/*.spec.ts", "**/*.test.ts"]
sources:
  - https://github.com/axel7083/foobar-tmp-demo/pull/2#discussion_r4285183794
---
## Check
Does the changed code read `package.json` from disk at runtime instead of importing it?

## Violation
An added or modified line that reads `package.json` through the file system: `readFile`, `readFileSync`, `fs.promises`, or `require` of a computed path, followed by `JSON.parse` or not.

## Not a violation
- `import pkg from '../package.json'` (or `import * as pkg from …`).
- Reading another JSON file.
- Lines that were not added or modified by the change.

## Examples
```ts
// ✅
import pkg from '../package.json';
const version = pkg.version;

// ❌
const pkg = JSON.parse(await readFile(join(extensionContext.extensionUri.fsPath, 'package.json'), 'utf8'));
```
