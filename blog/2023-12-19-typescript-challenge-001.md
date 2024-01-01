---
slug: typescript-challenge-001
title: Typescript challenge 001 - Deep Readonly
authors: william
tags: [Typescript, typescript-challenge]
---

<iframe width="640" height="360" src="https://www.youtube.com/embed/3WNrBHVX5xU" title="Deep Readonly with Rob Meyer - TypeScript Type Challenges #9 [MEDIUM]" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Solutions

#### solution 1

It does not work. Get error `Type instantiation is excessively deep and possible infinite`

```typescript
const sample = { a: 1, b: { c: 1 } }

type ShallowReadOnly<T> = {
  readonly [P in keyof T]: T[P]
}

type DeepReadonly001<T> = T extends Record<infer Key, infer Value>
  ? Value extends Record<PropertyKey, unknown>
    ? ShallowReadOnly<DeepReadonly001<{ Key: Value }>>
    : { readonly Key: Value }
  : never

type Answer = DeepReadonly001<typeof sample>
```

#### solution 2

```typescript
type DeepReadonly<T> = {
  readonly [P in keyof T]: T[P] extends Function ? T[P] : DeepReadonly<T[P]>
}

type Answer = DeepReadonly002<typeof sample>
const check: Answer = { a: 1, b: { c: 1 } }
check.b.c = 5 // Cannot assign to 'c' because it is a read-only property
```

---

### Answers

[GITHUB link](https://github.com/type-challenges/type-challenges/issues/21546)

```typescript
// answer 01
type DeepReadonly<T> = keyof T extends never
  ? T
  : {
      readonly [P in keyof T]: DeepReadonly<T[P]>
    }

// answer 02
type DeepReadonly<T> = keyof T extends never ? T : { readonly [P in keyof T]: DeepReadonly<T[P]> }
```

what kind of object that key of T is never? it should be an empty object `{}`, or property of prototype
