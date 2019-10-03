---
title:  "WebAssembly temperature converter"
date:   2019-07-28 19:27:00 -0400
excerpt: "A WebAssembly fueled temperature conversion website"
showDate: true
draft: false
tags: ["webassembly","C"]
---

Yesterday I completed the small project I was talking about in the previous post, a Fahrenheit/Celsius converter with some WebAssembly code. The code was written in C and I used [WebAssembly.studio](https://webassembly.studio/) to kickstart the project.

This two short C functions to convert Fahrenheit and Celsius:

```c
#define WASM_EXPORT __attribute__((visibility("default")))

WASM_EXPORT
int fahrToCels(int fahr)
{
  return 5 * (fahr - 32) / 9;
}

WASM_EXPORT
int celsToFahr(int cels)
{
  return ((cels * 9) / 5) + 32;
}
```

Compiled to this WebAssembly code:

```wasm
(module
  (type $t0 (func))
  (type $t1 (func (param i32) (result i32)))
  (func $__wasm_call_ctors (type $t0))
  (func $fahrToCels (export "fahrToCels") (type $t1) (param $p0 i32) (result i32)
    get_local $p0
    i32.const 5
    i32.mul
    i32.const -160
    i32.add
    i32.const 9
    i32.div_s)
  (func $celsToFahr (export "celsToFahr") (type $t1) (param $p0 i32) (result i32)
    get_local $p0
    i32.const 9
    i32.mul
    i32.const 5
    i32.div_s
    i32.const 32
    i32.add)
  (table $T0 1 1 anyfunc)
  (memory $memory (export "memory") 2)
  (global $g0 (mut i32) (i32.const 66560))
  (global $__heap_base (export "__heap_base") i32 (i32.const 66560))
  (global $__data_end (export "__data_end") i32 (i32.const 1024)))
```

WebAssembly.studio uses gulp to build the files, so I set it up with browser-sync and less once I started developing outside of the studio environment. Using their package "@wasm/studio-utils" it is simple to create a gulp task that compiles the source code into WebAssembly code and creates the wasm file:

```js

import { Service, project } from "@wasm/studio-utils";

gulp.task("compile-c", async () => {
  const data = await Service.compileFile(project.getFile("src/main.c"), "c", "wasm", "-g -O3");
  const outWasm = project.newFile("docs/out/main.wasm", "wasm", true);
  outWasm.setData(data);
});
```

To then use that wasm module you need to fetch the file, then create an arrayBuffer, start a WebAssembly instance and once you have that instance, you can access the functions from your wasm module. With WebAssembly.studio you can also get a js file with that fetch method implemented. Here is an example from an early stage of my project:

```js

fetch('../out/tempConver.wasm')
  .then(response => response.arrayBuffer())
  .then(bytes => WebAssembly.instantiate(bytes))
  .then(results => {
    instance = results.instance;
    document.getElementById("temperature").textContent = instance.exports.fahrToCels(90);
  })
  .catch(console.error);
```

At the end, before the error catch, I am setting the text content of an html element with the ID #temperature to the result of calling my fahrToCels function from the wasm module with 90 as the argument, which produces a result of 32.

I was pretty discouraged after my frist attempt at building this project, but it ended up working fine once I found how to build and serve the project with gulp, in order to avoid the CORS issues I was getting before. I would now like to set up a webAssembly project with webpack, which was what I had attempted previously to avoid the CORS errors in development.

[Link to the finished project](https://didacbigorda.com/fahrToCelsWasm)

