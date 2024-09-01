---
slug: typescript-challenge-006
title: 006 Type Lookup
authors: william
tags: [Typescript, typescript-challenge]
---

<iframe width="640" height="360" src="https://www.youtube.com/embed/VJgin0N7m7I?list=PLOlZuxYbPik180vcJfsAM6xHYLVxrEgHC" title="Promise.all with John Chadwick - TypeScript Type Challenges #20 [MEDIUM]" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"></iframe>

24/01/11

[challenge list](https://github.com/type-challenges/type-challenges/issues/21338)

[GITHUB link](https://github.com/type-challenges/type-challenges/issues/22461)

### Intro

```typescript
import type { Equal, Expect } from './test-utils'

interface Cat {
  type: 'cat'
  breeds: 'Abyssinian' | 'Shorthair' | 'Curl' | 'Bengal'
}

interface Dog {
  type: 'dog'
  breeds: 'Hound' | 'Brittany' | 'Bulldog' | 'Boxer'
  color: 'brown' | 'white' | 'black'
}

type Animal = Cat | Dog

type A1 = LookUp<Animal, 'dog'>
type B1 = Dog
type C1 = Expect<Equal<A1, B1>>

type A2 = LookUp<Animal, 'cat'>
type B2 = Cat
type C2 = Expect<Equal<A2, B2>>

// ============= Your Code Here =============
type LookUp<U, T> = any
```

### Solutions

```typescript
// my solution is pretty close to the answer, but it doesn't work
// type LookUp<U extends { type: string }, T extends string> = U['type'] extends T ? U : never

// solution 1
type LookUp<U extends { type: string }, T extends string> = U extends { type: T } ? U : never

// ============== Alternatives ==============
type LookUp<U extends { type: string }, T extends U['type']> = Extract<U, { type: T }>

// util type Extract can filter too!!!!
type LookUp<U, T> = Extract<U, { type: T }>

// tricky one, turn into an object and map it with key
type LookUp<U, T extends PropertyKey> = {
  [K in T]: U extends { type: T } ? U : never
}[T]
```
