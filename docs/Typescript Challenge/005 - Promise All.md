---
slug: typescript-challenge-005
title: 005 Promise All
authors: william
tags: [Typescript, typescript-challenge]
---

<iframe width="640" height="360" src="https://www.youtube.com/embed/VJgin0N7m7I?list=PLOlZuxYbPik180vcJfsAM6xHYLVxrEgHC" title="Promise.all with John Chadwick - TypeScript Type Challenges #20 [MEDIUM]" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"></iframe>

24/01/11

[challenge list](https://github.com/type-challenges/type-challenges/issues/21338)

[GITHUB link](https://github.com/type-challenges/type-challenges/issues/22460)

### Intro

```typescript
import type { Equal, Expect } from './test-utils'

const pa1 = PromiseAll([1, 2, 3] as const)
type A1 = typeof pa1
type B1 = Promise<[1, 2, 3]>
type C1 = Expect<Equal<A1, B1>>

const pa2 = PromiseAll([1, Promise.resolve(3)] as const)
type A2 = typeof pa2
type B2 = Promise<[1, number]>
type C2 = Expect<Equal<A2, B2>>

const pa3 = PromiseAll([1, 2, Promise.resolve(3)])
type A3 = typeof pa3
type B3 = Promise<[number, number, number]>
type C3 = Expect<Equal<A3, B3>>

// ============= Your Code Here =============
declare function PromiseAll<T>(): any
```

### Solutions

```typescript
declare function PromiseAll<T extends unknown[]>(
  value: readonly [...T]
): Promise<{
  [P in keyof T]: T[P] extends PromiseLike<infer R> ? R : T[P]
}>

type Await<T> = T extends Promise<infer R> ? Await<R> : T
declare function PromiseAll<T extends unknown[]>(
  value: readonly [...T]
): Promise<{
  [P in keyof T]: Await<T[P]>
}>
```
