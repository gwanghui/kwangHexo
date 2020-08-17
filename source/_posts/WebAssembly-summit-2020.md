---
title: WebAssembly-summit-2020
date: 2020-02-23 16:42:19
tags:
---
## Building a new kind of ecosystem
https://webassembly-summit.org/speakers/1/
- 보안을 강화하기 위한 Web Assembly
1. Sandboxing

2. Memory model
- Memory isolation

3. Interface Types
- exchange high-level values between memories
- we can isolate the memory between modules without making it too hard to share data

4. WebAssembly System Interface
- we actually have the concept of permissions baked into them
- we can give different modules, different permissions to different resource.

- WebAssembly nano process
- protect from malicious code

- vulnerable code

## Shipping Tiny Web Assembly

https://webassembly-summit.org/speakers/2/
- Sometimes code size is negligible(무시할만한) compared to other factors like asset size
- Sometimes the magic ability to run an app on the web at all is worth a large code size (ship a framework, VM, etc)

### WebAssembly : An opportunity for Small Code!
- binary format
- dead code elimination is possible

### Advice For All ToolChains
#### 1 slide of obvious stuff
- Enable compression on the server!
- Minify your Javascript too!
- Run Binaryen's wasm-opt

- What It Does (wasm-opt)
    - Dead code elimination
    - constant propagation
    - Inlining
    - Local optimizations (CoalesceLocals, SimplifyLocals, etc)
    - memory segment optimization (MemoryPacking)
    - Structured control flow (ReReloop, RemoveUnusedBrs)
    - etc
    
### Advice for specific Languages & Toolchains
#### General C/C++
- If you don't use c++ exceptions, build with -fno-exceptions
- avoid RTTI if you don't need it, build with -fno-rtti
- Careful with templates
- virtual calls may inhibit DCE
- prefer simple C over C++ standard library

#### Use WEB APIs directly
- Even better than printf, call a Web API, e.g. using EM_JS:
 
## JavaScriptCore's new WebAssembly interpreter
https://webassembly-summit.org/speakers/4/

- WebAssembly
    - BBQ :  View Bytecode Quickly (less optimize)
    - OMG : Optimize Machine code Generator (full optimize)
    
https://webkit.org/blog/9329/

## WebAssembly Music
https://webassembly-summit.org/speakers/5/

### Introduction
- WebAssembly deliver performance for rendering realtime audio
- Low latency possible with AudioWorklet
- Let's create a synthesizer and sequencer in WebAssembly

### Background

### Warning
- Experimenting with synthesizers can produce sudden unexpected very loud noise, witch may damage your hearing
- Keep the volume low, especially if using headphones
- Make sure you now where the mute button is :-)
 
### The Basics
- The simplest instrument
- Add to the mix

### App map

### sequencer (same as in 4klang)
- A simple pattern sequencer
- Short fixed length patterns

```javascript
[[0, 0, 0, 0, 0, 0, 0, 0],
[64, 0, 65, 0, 0, 67, 64, 0],
[22, 23, 34, 34, 34, 0, 44, 45],
[22, 33, 0, 0, 34, 0, 44, 55]]
```

- Track for each instrument with a list of patterns to play
```javascript
[[0, 1, 0, 2],
[1, 1, 3, 3],
[0, 0, 1, 2]]
```

- This is all it takes ato orchestrate the instruments

### Generate sequencer data from code

### Record MIDI and generate code
- While playing, midi input data is stored to patterns
- If we want to use the recording, we can paste it as code
- Pattern data is "reverse engineered" to javascript code, with durations on the notes instead of repeated hold commands

### AssemblyScript (why did I choose it?)
- High level readability
- Low level control
- Pure WebAssembly output (no additional js lib)
- Builds optimized for speed and size (Binaryen)
- Create WEbAssembly binaries in the browser
- Great for live coding: rapid development, instant results, directly in the browser!
 
### Synthesizing instruments in AssemblyScript
 - No sample data, just in code
 
### Data driven or code driven?
 Envelope -> oscillator -> filter -> out
 
 - You can synthesize an instrument by connecting envelopes, oscillators, filters etc.
 - Typical to create a data structure to be interpreted at runtime.
 - With AssemblyScript/WebAssembly we can instead generate and compile the code in the browser
 - Just like modern web-frameworks resolving configuration at compile-time (such as language)
 - Out binary can contain the logic directly rather than an interpreter of data describing the logic
 - Faster and smaller builds, no interpreter overhead
 - Not just for synthesizers but also for e.g smart contracts
 - Compiling is cheap, make pre-configured binaries rather than configuring at runtime.
  
 ### AudioWorklet
 - The "proper" way of using AudioWorklet would be to have one node per instrument and let WebAudio do the orchestration/mixing
 - But then we couldn't have music produced by a single WebAssembly executable binary
 - Made a polyfill for the purpose of serving this app
 - AudioWorklet model of render audio callback for WASI(Web Assembly System Interface)? (Similar to Jack and Core Audio)
 
 ### Sources on github
 https://github.com/petersalomonsen/javascriptmusic
 
 - project contains the WebAssembly music experiment, and also the predecessing javascript music projects for Midi synths and 4klang.
 
 
 