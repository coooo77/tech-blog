---
slug: typescript-challenge-002
title: Typescript challenge 002 - Chainable Options
authors: william
tags: [Typescript, typescript-challenge]
---

<iframe width="640" height="360" src="https://www.youtube.com/embed/UxxrDnJ5mIA?list=PLOlZuxYbPik180vcJfsAM6xHYLVxrEgHC" title="Chainable Options with John Chadwick - TypeScript Type Challenges #12 [MEDIUM]" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

[challenge list](https://github.com/type-challenges/type-challenges/issues/21338)

### Intro

```typescript
declare const chainable: Chainable

const a1 = chainable.option('foo', 123).option('bar', { value: 'Hello World' }).option('name', 'type-challenges').get()
type B1 = {
  foo: number
  bar: {
    value: string
  }
  name: string
}
type C1 = Expect<Alike<typeof a1, B1>>

const a2 = chainable.option('name', 'another name').option('name', 'last name').get()
type B2 = {
  name: string
}
type C2 = Expect<Alike<typeof a2, B2>>

const a3 = chainable.option('name', 'another name').option('name', 123).get()
type B3 = {
  name: number
}
type C3 = Expect<Alike<typeof a3, B3>>
```

it looks like type Chainable is an object which can add keys and values.

```typescript
const chainable = {}
chainable.name = 'william'
```

also overwrite values is allowed.

```typescript
const chainable = {}
chainable.tel = '123'
chainable.tel = '321'
```

#### Mindset

collect types recursively and if keys exist, replace it with values. e.g.

```typescript
type TupleToUnion<T> = T extends [infer Head, ...infer Tail] ? Head | TupleToUnion<Tail> : never
```

### Solutions

```typescript
type Chainable<Acc extends Record<PropertyKey, unknown> = {}> = {
  option<Key extends PropertyKey, Value>(
    key: Key,
    value: Value
  ): Chainable<Key extends keyof Acc ? Pick<Acc, Exclude<keyof Acc, Key>> & Record<Key, Value> : Acc & Record<Key, Value>>
  get(): Acc
}
```

### Answers

```typescript
type Chainable<Acc = {}> = {
  option<K extends PropertyKey, V>(key: K, value: V): Chainable<Omit<Acc, K> & Record<K, V>>
  get(): Acc
}
```

[GITHUB link](https://github.com/type-challenges/type-challenges/issues/21548)
