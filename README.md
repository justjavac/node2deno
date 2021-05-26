# From Node.js to Deno

Deno for Node.js developers.

## Console

### Class `Console`

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

### `console.error`

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

### `console.trace`

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

### Reading file

```ts
// Node
import fs from "fs/promises";
const data = await fs.readFile("./README.md", "utf8");

// Deno
// Requires `--allow-read` permission
const data = await Deno.readTextFile("./README.md");
```

### Writing file

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

### File descriptors

```ts
// Node
import fs from "fs/promises";
const file = await fs.open("./file.txt");

// Deno
// Requires `allow-read` and/or `allow-write` permissions depending on options.
const file = await Deno.open("./file.txt");
```

## Path

### Last portion of a path

```ts
// Node
import path from "path";
path.basename("/foo/bar/baz/asdf/quux.html");
// -> 'quux.html'

// Deno
import * as path from "https://deno.land/std/path/mod.ts";
path.basename("/foo/bar/baz/asdf/quux.html");
// -> 'quux.html'
```

Or with file extensions:

```ts
// Node
import path from "path";
path.basename("/foo/bar/baz/asdf/quux.html", ".html");
// -> 'quux'

// Deno
import * as path from "https://deno.land/std/path/mod.ts";
path.basename("/foo/bar/baz/asdf/quux.html", ".html");
// -> 'quux'
```

### Directory name of a path

```ts
// Node
import path from "path";
path.dirname("/foo/bar/baz/asdf/quux");
// -> '/foo/bar/baz/asdf'

// Deno
import * as path from "https://deno.land/std/path/mod.ts";
path.dirname("/foo/bar/baz/asdf/quux");
// -> '/foo/bar/baz/asdf'
```

### Extension of the path

```ts
// Node
import path from "path";
path.extname("index.html");
// -> '.html'

// Deno
import * as path from "https://deno.land/std/path/mod.ts";
path.extname("index.html");
// -> '.html'
```

### Join all path segments together

```ts
// Node
import path from "path";
path.join("/foo", "bar", "baz/asdf", "quux", "..");
// -> '/foo/bar/baz/asdf'

// Deno
import * as path from "https://deno.land/std/path/mod.ts";
path.join("/foo", "bar", "baz/asdf", "quux", "..");
// -> '/foo/bar/baz/asdf'
```

### Resolves path segments into an absolute path.

```ts
// Node
import path from "path";
path.resolve('/foo/bar', './baz');
// -> '/foo/bar/baz'

// Deno
import * as path from "https://deno.land/std/path/mod.ts";
path.resolve('/foo/bar', './baz');
// -> '/foo/bar/baz'
```

### Normalizes a path

```ts
// Node
import path from "path";
path.normalize("/foo/bar//baz/asdf/quux/..");
// -> '/foo/bar/baz/asdf'

// Deno
import * as path from "https://deno.land/std/path/mod.ts";
path.normalize("/foo/bar//baz/asdf/quux/..");
// -> '/foo/bar/baz/asdf'
```

### Parse a path

```ts
// Node
import path from "path";
path.parse("/home/user/dir/file.txt");
// -> { root: "/", dir: "/home/user/dir", base: "file.txt", ext: ".txt", name: "file" }

// Deno
import * as path from "https://deno.land/std/path/mod.ts";
path.parse("/home/user/dir/file.txt");
// -> { root: "/", dir: "/home/user/dir", base: "file.txt", ext: ".txt", name: "file" }
```

## Subprocess

### Subprocess in a short story

```ts
// Node
// All functionalities associated to subprocess are under `child_process` module
const child_process = require("child_process");
// Spawn a subprocess
const p = child_process.spawn("ping", ["-h"]);
// OR buffered version `child_process.exec`
// OR synced version `child_process.spawnSync`
// redirect stdout
p.stdout.on("data", (data) => console.log(data));
// redirect stderr
p.stderr.on("data", (error) => console.log(error));
// Subprocess Close event
p.on("close", (code) => console.log(`child process exited with code ${code}`));

// Here's how we can spawn a *BRAND NEW* nodejs instance to run a js "module"
child_process.fork("hello.js");

// Final output:
//  <Buffer d1 a1 cf ee 20 2d 68 20 b2 bb d5 fd c8 b7 a1 a3 0d 0a 0d 0a d3 c3 b7 a8 3a 20 70 69 6e 67 20 5b 2d 74 5d 20 5b 2d 61 5d 20 5b 2d 6e 20 63 6f 75 6e 74 ... 1447 more bytes>
//  child process exited with code 1
//  hello from hello.js

// Deno
// Deno uses `Deno.run` to spawn a subprocess
// `allow-run` permission *REQUIRED*
const p = Deno.run({
  cmd: ["ping", "-h"],
  stdout: "piped",
  stderr: "piped",
});

// HERE, we read out the stdout output that we set to piped before
console.log(await p.output());
// and of course the stderr
console.log(await p.stderrOutput());

// In Deno, we don't have a fork like function,
// instead you can still use `Deno.run` to spawn
// a `BRAND NEW` Deno instance to run the script
const decoder = new TextDecoder();
const forkLike = Deno.run({
  cmd: ["deno", "run", "hello.js"],
  stdout: "piped",
});
console.log(decoder.decode(await forkLike.output())); // hello from hello.js
// OR in a IMO better way: `Web workers`
new Worker(
  new URL("./hello.js", import.meta.url).href,
  { type: "module" },
); // hello from hello.js

// Final output:
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
```
