# 从 Node.js 到 Deno

Deno 是一个 JavaScript/TypeScript 的运行时，默认使用安全环境执行代码，有着卓越的开发体验。

- Deno 不使用 `npm`。
- Deno 不使用 `package.json` 解析模块。
- Deno 所有的乙部 API 都返回 promise。
- Deno 需要显式指名文件、网络、系统环境变量等的权限。
- Deno 使用 "ES Modules"，并且不支持 `require()`。

## 翻译

- [English](./README.md)
- [简体中文](./README.zh-CN.md)

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
