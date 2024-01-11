---
slug: typescript-challenge-003
title: 003 Last of Array
authors: william
tags: [Typescript, typescript-challenge]
---

<iframe width="640" height="360" src="https://www.youtube.com/embed/ZS6zfnWdHr8?list=PLOlZuxYbPik180vcJfsAM6xHYLVxrEgHC" title="Last of Array with Aaron Harper - TypeScript Type Challenges #15 [MEDIUM]" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"></iframe>

24/01/11

[challenge list](https://github.com/type-challenges/type-challenges/issues/21338)

[GITHUB link](https://github.com/type-challenges/type-challenges/issues/21549)

### Intro

```typescript
import type { Equal, Expect } from './test-utils'

type A1 = Last<[3, 2, 1]>
type B1 = 1
type C1 = Expect<Equal<A1, B1>>

type A2 = Last<[() => 123, { a: string }]>
type B2 = { a: string }
type C2 = Expect<Equal<A2, B2>>

type A3 = Last<[]>
type B3 = never
type C3 = Expect<Equal<A3, B3>>

// ============= Your Code Here =============
```

### Solutions

```typescript
// my solution
type Last<T> = T extends [infer Header, ...infer Other] ? (Other['length'] extends 1 ? Other[0] : Last<Other>) : never

// answer
type Last<T> = T extends [...unknown[], infer Tail] ? Tail : never

// very tricky, but u can get length of the Array!
type Last<T extends unknown[]> = [never, ...T][T['length']]
```
