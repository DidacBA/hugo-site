---
title:  "Welcome to my blog/website!"
date:   2019-07-25 15:48:11 -0400
showDate: true
draft: false
tags: ["update","webassembly", "C"]
---

Hello! The purpose of this blog is mainly to serve as a diary of my personal progress as a web developer. Consider this first post the before picture.

Yesterday I tried getting a short C function to compile to WebAssembly and running it from the browser. I got the main function to print the message to the console in the browser, but I couldn't manage to import the fahrToCels function into the JavaScript environment. It was also giving me CORS errors, which I solved by serving the files from a Node server. Need to read more about the WebAssembly JavaScript API (also bundling wasm modules with webpack, played with that and completely broke it). Here's the simple C code I compiled to WebAssembly.

```c
int fahrToCels(int fahr)
{
  return 5 * (fahr - 32) / 9;
}

int main()
{
  printf("WebAssembly!\n");
  return 0;
} 
```

showDate: true
draft: true
tags: ["blog","story"]