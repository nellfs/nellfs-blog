+++
title = 'Why the New TypeScript Compiler Is a Go Port'
date = '2025-12-14T14:28:14-03:00'
draft = true
+++

# Why the New TypeScript Compiler Is a Go Port, Not a 'X' Language Rewrite
## A simple and direct explanation

On March 11th, 2025, Anders Hejlsberg announced that the new TypeScript compiler would be **ported to Go**.
The work had already started; the announcement just made it public.

Since then, many people have asked:

- Why Go?
- Why call it a *port* instead of a *rewrite*?
- Why not Rust or C# or my 'X' language?

Before starting the migration, the TypeScript team evaluated several native languages, including **Rust** and **C#**. In the end, **Go stood out**, and here’s why.

---

## The current compiler is already function-oriented

The existing TypeScript compiler is written in a very **function-based style** (not class-heavy or object-oriented).

Go naturally follows a similar approach:
- Functions as the primary building blocks
- Simple data structures
- Minimal abstraction overhead

Because of this, many parts of the compiler can be ported in a very **direct, almost 1-to-1 way**: TypeScript functions become Go functions with similar structure and logic.

You can clearly see this in the source code of both compilers, here a scanner function as an example:

- TypeScript scanner:  
  https://github.com/microsoft/TypeScript/blob/0a071327153b4c386dfcab19a584e0d6224d1354/src/compiler/scanner.ts#L1171C14-L1211

- Go scanner:  
  https://github.com/microsoft/typescript-go/blob/6c175a0ad35e6bf0eca7dd3b5c622a5ab9e6fd35/internal/scanner/scanner.go#L1829C1-L1868C2

The structure is very similar. This is what a *port* means.

---

## Why not your 'X' language?

Your answer is probably explained in the sections "Why not Rust" and "Why not or C#" :).

---

## Why not Rust?

Rust would likely require **significant changes to the code structure**.

To satisfy Rust’s compiler ownership and lifetime rules, the engineers would need to rethink many APIs and data flows. That would lead more refactoring, more divergence from the original Typescript code, and a longer development time.

That’s closer to a **rewrite** than a port.

---

## Why not my 'X' language?

C# was considered, but there were practical issues:

- AOT compilation in C# is still relatively new
- Native AOT compiled binaries do not support as many platforms as Go
- C# was designed around **bytecode and a runtime** first, not static native binaries first

Go, on the other hand, has a long track record of producing **portable, static native binaries** across many platforms.

---

## Business and performance reasons

Porting the compiler to Go is easier, faster and safer (from a business and maintenance perspective)

And it brings real benefits.

In some cases, the new TypeScript compiler is already **up to 10× faster**.
This comes from being able to fully take advantage of native execution and Go's multi-threaded concurrency model

---

## In short  
Go allowed the TypeScript team to keep the compiler’s design, move fast, reduce risk, and gain major performance improvements without rewriting everything from scratch.
