# From Node.js to Deno

Deno is a JavaScript/TypeScript runtime with secure defaults and a great
developer experience.

- Deno does not use `npm`.
- Deno does not use `package.json` in its module resolution algorithm.
- All async actions in Deno return a promise.
- Deno requires explicit permissions for file, network, and environment access.
- Deno always dies on uncaught errors.
- Deno uses "ES Modules" and does not support `require()`.

## Contents

  - [Console](#console)
  - [File system](#file-system)

## Console

- [1.0](#1.0) <a name='1.0'></a> Class `Console`

```ts
// Node
const { Console } = console;
new Console({ stdout: process.stdout, stderr: process.stderror });

// Deno
const { Console } = console;
// undefined

// Chrome
const { Console } = console;
// undefined
```

- [1.1](#1.1) <a name='1.1'></a> `console.error`

```ts
// Node
console.error(new Error("Whoops, something bad happened"));
// Error: Whoops, something bad happened
//     at REPL1:1:15
//     at Script.runInThisContext (vm.js:133:18)
//     at REPLServer.defaultEval (repl.js:484:29)
//     at bound (domain.js:413:15)
//     at REPLServer.runBound [as eval] (domain.js:424:12)
//     at REPLServer.onLine (repl.js:817:10)
//     at REPLServer.emit (events.js:327:22)
//     at REPLServer.EventEmitter.emit (domain.js:467:12)
//     at REPLServer.Interface._onLine (readline.js:337:10)
//     at REPLServer.Interface._line (readline.js:666:8)

// Deno
console.error(new Error("Whoops, something bad happened"));
// Error: Whoops, something bad happened
//     at <anonymous>:2:15

// Chrome
console.error(new Error("Whoops, something bad happened"));
// Error: Whoops, something bad happened
//     at <anonymous>:1:15
```

- [1.1](#1.2) <a name='1.2'></a> `console.trace`

```ts
// Node
console.trace("Show me");
// Trace: Show me
//     at REPL14:1:9
//     at Script.runInThisContext (vm.js:133:18)
//     at REPLServer.defaultEval (repl.js:484:29)
//     at bound (domain.js:413:15)
//     at REPLServer.runBound [as eval] (domain.js:424:12)
//     at REPLServer.onLine (repl.js:817:10)
//     at REPLServer.emit (events.js:327:22)
//     at REPLServer.EventEmitter.emit (domain.js:467:12)
//     at REPLServer.Interface._onLine (readline.js:337:10)
//     at REPLServer.Interface._line (readline.js:666:8)

// Deno
console.trace("Show me");
// Trace: Show me
//     at <anonymous>:2:9

// Chrome
console.trace("Show me");
// Show me
// (anonymous) @ VM45:1
```

## File system

- [2.0](#2.0) <a name='2.0'></a> Reading file

```ts
// Node
import fs from "fs/promises";
const data = await fs.readFile("./README.md", "utf8");

// Deno
// Requires `--allow-read` permission
const data = await Deno.readTextFile("./README.md");
```

- [2.1](#2.1) <a name='2.1'></a> Writing file

```ts
// Node
import fs from "fs/promises";
const content = 'Some content!'
await fs.writeFile("./file.txt", content);

// Deno
// Requires `--allow-write` permission
const content = 'Some content!'
await Deno.writeTextFile("./file.txt", content);
```

- [2.2](#2.2) <a name='2.2'></a> File descriptors

```ts
// Node
import fs from "fs/promises";
const file = await fs.open("./file.txt");

// Deno
// Requires `allow-read` and/or `allow-write` permissions depending on options.
const file = await Deno.open("./file.txt");
```
