---
slug: typescript-challenge-007
title: 007 Trim Left
authors: william
tags: [Typescript, typescript-challenge]
---

<iframe width="640" height="360" src="https://www.youtube.com/embed/AnVtVw5xzOY?list=PLOlZuxYbPik180vcJfsAM6xHYLVxrEgHC" title="Trim Left with Opender Singh - TypeScript Type Challenges #106 [MEDIUM]" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"></iframe>

24/01/17

[challenge list](https://github.com/type-challenges/type-challenges/issues/21338)

[GITHUB link](https://github.com/type-challenges/type-challenges/issues/22462)

```typescript
import type { Equal, Expect } from './test-utils'

type A1 = TrimLeft<'str'>
type B1 = 'str'
type C1 = Expect<Equal<A1, B1>>

type A2 = TrimLeft<' str'>
type B2 = 'str'
type C2 = Expect<Equal<A2, B2>>

type A3 = TrimLeft<' str'>
type B3 = 'str'
type C3 = Expect<Equal<A3, B3>>

type A4 = TrimLeft<' str '>
type B4 = 'str '
type C4 = Expect<Equal<A4, B4>>

type A5 = TrimLeft<' \n\t foo bar '>
type B5 = 'foo bar '
type C5 = Expect<Equal<A5, B5>>

type A6 = TrimLeft<''>
type B6 = ''
type C6 = Expect<Equal<A6, B6>>

type A7 = TrimLeft<' \n\t'>
type B7 = ''
type C7 = Expect<Equal<A7, B7>>

// ============= Your Code Here =============
type Whitespace = ' ' | '\n' | '\t'
type TrimLeft<T> = any
```

### Solutions

template literals is good method for these kind problems.

```typescript
// my solution
type TrimLeft<T> = T extends `${Whitespace}${infer R}` ? TrimLeft<R> : T

// answer
type TrimLeft<T extends string> = T extends `${Whitespace}${infer U}` ? TrimLeft<U> : T

// ============== Alternatives ==============
type TrimLeft<T extends string> = T extends `${infer A}${infer B}` ? (A extends ' ' | '\n' | '\t' ? TrimLeft<B> : T) : T
```
