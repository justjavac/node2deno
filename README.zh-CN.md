# 从 Node.js 到 Deno

为 Node.js 开发者准备的 Deno 代码片段。

## 翻译

- [English](./README.md)
- [简体中文](./README.zh-CN.md)

## 控制台（Console）

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

## 文件系统（File system）

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
## 路径（Path）

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

## 子进程（Subprocess）
- [4.0](#4.0) <a name='4.0'></a> 子进程简介
  - [Node `child_process` 官方文档](https://nodejs.org/api/child_process.html)
  - [Deno `Deno.run` 官方文档](https://doc.deno.land/builtin/stable#Deno.run)
  - [Deno `Web worker` 官方手册](https://deno.land/manual/runtime/workers#workers)

```ts
// Node
// 所有有关于子进程的代码都在`child_process` 模块中
const child_process = require("child_process");
// 创建一个子进程
const p = child_process.spawn("ping", ["-h"]);
// 或者有I/O缓存的版本 `child_process.exec`
// 或者同步阻塞的版本 `child_process.spawnSync`
// 标准输出(stdout) 重定向
p.stdout.on("data", data => console.log(data));
// 标准错误(stderr) 重定向
p.stderr.on("data", error => console.log(error));
// 子进程关闭事件的监听
p.on("close", code => console.log(`child process exited with code ${code}`))

// 下面是如何通过创建一个全新的NodeJS实例来运行JS“模块”的代码
child_process.fork("hello.js")

// 最终输出: 
//  <Buffer d1 a1 cf ee 20 2d 68 20 b2 bb d5 fd c8 b7 a1 a3 0d 0a 0d 0a d3 c3 b7 a8 3a 20 70 69 6e 67 20 5b 2d 74 5d 20 5b 2d 61 5d 20 5b 2d 6e 20 63 6f 75 6e 74 ... 1447 more bytes>
//  child process exited with code 1
//  hello from hello.js


// Deno
// Deno 使用 `Deno.run` 来创建子进程
// `allow-run` 权限是*必需*的
const p = Deno.run({
  cmd: ["ping", "-h"],
  stdout: "piped",
  stderr: "piped",
});

// 现在，我们读取出之前我们设置好的通过管道传来的子进程的标准输出内容
console.log(await p.output());
// 当然了，子进程的标准错误也是类似的
console.log(await p.stderrOutput());

// 在Deno的世界里，我们并没有和Node中的fork类似的工具函数，
// 当然，你可以仍然通过使用过 `Deno.run` 来创建一个全新的
// Deno进程实例来运行一个JS脚本
const decoder = new TextDecoder();
const forkLike = Deno.run({
  cmd: ["deno", "run", "hello.js"],
  stdout: "piped",
});
console.log(decoder.decode(await forkLike.output())); // hello from hello.js
// 或者使用在我看来不知道高到哪里去了的方式 `Web workers`
new Worker(
  new URL("./hello.js", import.meta.url).href,
  { type: "module" },
); // hello from hello.js

// 最终输出: 
//   Uint8Array(1497) [
//    209, 161, 207, 238,  32,  45, 104,  32, 178, 187, 213, 253, 200, 183, 161,
//    163,  13,  10,  13,  10, 211, 195, 183, 168,  58,  32, 112, 105, 110, 103,
//     32,  91,  45, 116,  93,  32,  91,  45,  97,  93,  32,  91,  45, 110,  32,
//     99, 111, 117, 110, 116,  93,  32,  91,  45, 108,  32, 115, 105, 122, 101,
//     93,  32,  91,  45, 102,  93,  32,  91,  45, 105,  32,  84,  84,  76,  93,
//     32,  91,  45, 118,  32,  84,  79,  83,  93,  13,  10,  32,  32,  32,  32,
//     32,  32,  32,  32,  32,  32,  32,  32,  91,  45,
//    ... 1397 more items
//  ]
//  Uint8Array(0) []
//  hello from hello.js

//  hello from hello.js
```)