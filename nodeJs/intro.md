What is Node Js? What it is Not?
NodeJs is an opensource, cross platform, runtime environment.

What it is Not: It is not a programming language, and it is not a framework. It is the environment that allows us to build backend servers, web apps, and APIs using javascript outside the browser.

what is runtime environment
Js as a programming language has a lot of limitations.
runtime environment provides extra capabilities to js.
in Javascript there are functions like setTimeOut and setTimeIntervals (setInterval),
but js doesn't know how to handle timers. now runtime environment provide these extra capabilities, runtime knows how to handle timers.

browser can act as a runtime environment for javascript,
like reading and modifying HTML.
all DOM related functions is not part of official js.
browser also provide networking capabilities (like fetch or XMLHttpRequest).

In NodeJs: Instead of DOM and HTML, the Node runtime provides server capabilities to JS, like File System (fs) access, OS access, and network socket handling.

How NodeJs code is Written (Execution)
js is not compiled as welll as neither interpreted.
// what we do in c++ is ahead of time compilation (AOT)
// in js we do just in time compilation jit

jit is a mechanism. (Extra detail: JIT takes the best of both worlds. It starts by interpreting code so your app launches instantly, but then it compiles the frequently used code behind the scenes so it runs blazing fast).

Internals of NodeJs
lets talk about internals of node js.
node js provides different kinds of capabilities. (Extra detail: To provide these asynchronous, non-blocking capabilities, NodeJs uses a hidden C++ library called Libuv. Libuv handles the Event Loop, thread pool, and background I/O tasks).

Chrome V8 engine & Internals of V8 engine
v8 engine is made in c++ (by Google).
nodejs uses v8 as js engine.

there are diff componenets of v8 engine:

Parser ->

interpreter -> Ignition

compiler -> Turbofan

Parser takes the code organises it and creates AST (Abstract syntax tree). (Extra detail: The AST is a tree data structure that logically maps out the syntax of your written code so the machine can understand the instructions).

ignition Interpreter -> takes your corresponding AST and converts to byte code.

What is (Byte Code) -> an intermediate code, not the code we have written and not the final code that will be executed. (Extra detail: It is heavily optimized for Ignition to read extremely fast).

after conversion it also starts runnning the byte code.

ignition converts to less optimised byte code, there might be some funcitons which are called again and again (often referred to as "hot code").

here comes Turbofan for the rescue, turbofan is an optimized compiler, it tries to cache the repetable byte code, lot of optimizations are there.

machine code is created on the past runs.

turbofan during the runtime is capable of creating optimised machine code, and can fallback also, if it doesn't run well. (Extra detail: This fallback is called "de-optimization". It happens if dynamically typed variables suddenly change types—like passing a string to a function that was expecting a number).

Garbage Collection
v8 engine also provides gc capabilities (to automatically free up unused memory).
it uses Orinoco algo.

it uses old and young generation segregations:

young -> newly created. (Extra detail: Memory here fills up fast. V8 uses a "Scavenger" process to clean this up rapidly because most objects die young).

old -> already survived 2-3 gc. (Extra detail: These are long-lasting objects like active server connections or global variables. V8 uses a slower "Mark-Sweep" process here).

it also do memory compaction. (Extra detail: When objects in the old generation are deleted, it leaves fragmented "holes" in the memory. Compaction pushes all the remaining live objects tightly together to free up large, continuous blocks of memory).
