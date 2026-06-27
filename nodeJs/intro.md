# 🚀 NodeJs: The Complete Picture

**NodeJs** is an opensource, cross platform, runtime environment.

### What It Is Not

It is **not** a programming language, and it is **not** a framework. It is the environment that allows us to build backend servers, web apps, and APIs using javascript outside the browser.

---

## 🌍 What is a Runtime Environment?

Js as a programming language has a lot of limitations. A **runtime environment** provides extra capabilities to js.

In Javascript, there are functions like `setTimeOut` and `setTimeIntervals` (`setInterval`), but js doesn't know how to handle timers. Now, the runtime environment provides these extra capabilities; the runtime knows how to handle timers.

### The Browser Runtime

The browser can act as a runtime environment for javascript, like reading and modifying HTML.

- All **DOM** related functions is not part of official js.
- The browser also provide networking capabilities _(like fetch or XMLHttpRequest)_.

### The NodeJs Runtime

> **Extra Detail:** Instead of DOM and HTML, the Node runtime provides server capabilities to JS, like File System (`fs`) access, OS access, and network socket handling.

---

## ⚙️ How NodeJs Code is Written (Execution)

js is not compiled as welll as neither interpreted.

- **AOT:** // what we do in c++ is ahead of time compilation
- **JIT:** // in js we do just in time compilation (jit)

**Jit is a mechanism.** > **Extra Detail:** JIT takes the best of both worlds. It starts by interpreting code so your app launches instantly, but then it compiles the frequently used code behind the scenes so it runs blazing fast.

---

## 🧠 Internals of NodeJs

lets talk about internals of node js. node js provides different kinds of capabilities.

> **Extra Detail (Libuv):** To provide these asynchronous, non-blocking capabilities, NodeJs uses a hidden C++ library called **Libuv**. Libuv handles the Event Loop, thread pool, and background I/O tasks.

### Chrome V8 Engine

**v8 engine** is made in c++ _(by Google)_. nodejs uses v8 as js engine.

There are diff componenets of v8 engine:

1. **Parser** 2. **Interpreter** -> _Ignition_ 3. **Compiler** -> _Turbofan_ ---

## 🏎️ Internals of V8 Engine (The Pipeline)

### 1. The Parser

**Parser** takes the code, organises it, and creates **AST** (Abstract syntax tree).

> **Extra Detail:** The AST is a tree data structure that logically maps out the syntax of your written code so the machine can understand the instructions.

### 2. Ignition (The Interpreter)

**ignition Interpreter** -> takes your corresponding AST and converts to **byte code**.

- **What is Byte Code?** -> an intermediate code, not the code we have written and not the final code that will be executed.
- _Extra Detail:_ It is heavily optimized for Ignition to read extremely fast.
- after conversion it also starts runnning the byte code.

### 3. Turbofan (The Compiler)

ignition converts to less optimised byte code, there might be some funcitons which are called again and again _(often referred to as "hot code")_.

- here comes **Turbofan** for the rescue. turbofan is an optimized compiler, it tries to cache the repetable byte code, lot of optimizations are there.
- **machine code** is created on the past runs.
- turbofan during the runtime is capable of creating optimised machine code, and can fallback also, if it doesn't run well.
  > **Extra Detail (De-optimization):** This fallback is called "de-optimization". It happens if dynamically typed variables suddenly change types—like passing a string to a function that was expecting a number.

---

## 🗑️ Garbage Collection

v8 engine also provides **gc** (garbage collection) capabilities _(to automatically free up unused memory)_.

it uses the **Orinoco** algo. it uses old and young generation segregations:

- 🌱 **young** -> newly created.
  > _Extra Detail:_ Memory here fills up fast. V8 uses a "Scavenger" process to clean this up rapidly because most objects die young.
- 🌳 **old** -> already survived 2-3 gc.
  > _Extra Detail:_ These are long-lasting objects like active server connections or global variables. V8 uses a slower "Mark-Sweep" process here.

it also do **memory compaction**.

> **Extra Detail:** When objects in the old generation are deleted, it leaves fragmented "holes" in the memory. Compaction pushes all the remaining live objects tightly together to free up large, continuous blocks of memory.
