---
title: Function Overloads 的使用方法
tags:
  - typescript
---

## Class 寫法

```typescript
class Example {
  public name?: string
  public surname?: string
  public category?: string
  public address?: string

  constructor()
  constructor(name: string, surname: string, category: string, address?: string)
  constructor(name?: string, surname?: string, category?: string, address?: string) {
    this.name = name
    this.surname = surname
    this.category = category
    this.address = address
  }

  getName(): undefined
  getName(isSpecial: true): string
  getName(isSpecial: false): number
  getName(isSpecial?: boolean): string | number | undefined {
    if (isSpecial === undefined) return
    if (isSpecial) return 'special'
    return 123456
  }
}

const a = new Example()
// const result1: readonly [undefined, string, number]
const result1 = [a.getName(), a.getName(true), a.getName(false)] as const
```

## Function 寫法

```typescript
function example2(): null
function example2(showString: true): string
function example2(showString: false): number
function example2(showString?: boolean): string | number | null {
  if (showString === undefined) return null
  if (showString) return 'string'
  return 0
}

// (showString?: boolean): string | number | null
const result2 = [example2(), example2(true), example2(false)] as const
```

## Array function 寫法

```typescript
type Example3 = {
  (): null
  (showString: true): string
  (showString: false): number
  (showString?: boolean): string | number | null
}

const example3 = ((showString?: boolean): string | number | null => {
  if (showString === undefined) return null
  if (showString) return 'string'
  return 0
}) as Example3

// const result3: readonly [null, string, number]
const result3 = [example3(), example3(true), example3(false)] as const
```

## 邏輯規格

overload function 最後一個 function 必須是前面所有 function 的總型別

例如

```
function A(): type A
function A(): type B
function A(): type C

type C = type A | type B
```

## 範例

```typescript
function example(): undefined
function example(value: string): number {
  if (value === undefined) return value
  return +value
}
```

因為第一行 function 跟 第二行 function 型別完全不相交 (一個要有參數，一個沒有參數)，所以這個 overload 無法成立

所以要這樣寫

```typescript
function example(): undefined
function example(value: string): number
function example(value?: string): number | undefined {
  if (value === undefined) return value
  return +value
}
```

可以進一步簡化

```typescript
function example(): undefined
function example(value?: string) {
  if (value === undefined) return value
  return +value
}
```

## 參考

- [Function Overloads](https://www.typescriptlang.org/docs/handbook/2/functions.html#function-overloads)
- [Typescript overload arrow function is not working](https://stackoverflow.com/questions/56109614/typescript-overload-arrow-function-is-not-working)
- [Typescript class: "Overload signature is not compatible with function implementation"](https://stackoverflow.com/questions/39407311/typescript-class-overload-signature-is-not-compatible-with-function-implementa)
