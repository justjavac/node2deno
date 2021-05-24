# 从 Node.js 到 Deno

为 Node.js 开发者准备的 Deno 代码片段。

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
const content = "Some content!";
await fs.writeFile("./file.txt", content);

// Deno
// Requires `--allow-write` permission
const content = "Some content!";
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
## Path

- [3.0](#3.0) <a name='3.0'></a> Path 概要
  - [Node path官方文档](https://nodejs.org/api/path.html)
  - [Deno path官方文档](https://doc.deno.land/https/deno.land/std@0.97.0/path/mod.ts)

```ts
// Node
// 所有有关路径的工具函数都在名为`path`的模块中
const path = require("path");
// 比如 basename
console.log(path.basename('/foo/bar/baz/asdf/quux.html')) // `quux.html`
// 所有的平台相关的代码都分别存在于`posix` 和 `win32` 子模块中
// 比如 分隔符（delimiter）
// posix版
console.log(path.posix.delimiter); // `:`
// win32版
console.log(path.win32.delimiter); // `;`

// Deno
// Deno 的官方库API非常像 Node
import { win32, posix, basename } from "https://deno.land/std@0.97.0/path/mod.ts"
console.log(basename('/foo/bar/baz/asdf/quux.html')) // `quuz.html`
// posix版
console.log(posix.delimiter); // `:`
// win32版
console.log(win32.delimiter); // `;`
```
