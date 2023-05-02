---
title: React 進階
tags:
  - frontend
  - react
---

## 使用 Child component 內的資源

原則上 React 要操作 Child Component 的資源，是直接傳入 Parent Component 的 function

```tsx
function ChildComponent({ parentMethod }) {
  const msg = 'this is a child'
  const showMsg = () => parentMethod(msg)
  return <button onClick={showMsg}>Click</button>
}

function ParentComponent() {
  const getChildMsg = (msg) => console.log(msg) // 'this is a child'
  return <ChildComponent parentMethod={getChildMsg} />
}
```

React 不能直接取得 Child Component 的內容，甚至是 ref 本身；官方有提供例子

```tsx
import { useRef } from 'react'

function MyInput(props) {
  return <input {...props} />
}

export default function MyForm() {
  const inputRef = useRef(null)

  function handleClick() {
    inputRef.current.focus()
  }

  return (
    <>
      {/* 這裡ref是無效的，永遠會是null */}
      <MyInput ref={inputRef} />
      <button onClick={handleClick}>Focus the input</button>
    </>
  )
}
```

會得到錯誤：

```terminal
Warning: Function components cannot be given refs. Attempts to access this ref will fail. Did you mean to use React.forwardRef()?
```

## 使用 forwardRef

正確作法是使用 forwardRef 、useImperativeHandle

```tsx
import { forwardRef, useRef } from 'react'

const MyInput = forwardRef((props, ref) => {
  return <input {...props} ref={ref} />
})

export default function Form() {
  const inputRef = useRef(null)

  function handleClick() {
    inputRef.current.focus()
  }

  return (
    <>
      <MyInput ref={inputRef} />
      <button onClick={handleClick}>Focus the input</button>
    </>
  )
}
```

如果需要讓父層取得子層內容，可以使用 useImperativeHandle

```tsx
import { forwardRef, useRef } from 'react'

const Child = forwardRef(({ foo, bar }, ref) => {
  const childRef = useRef()

  const mergedRefs = {
    childRef,
    foo,
    bar,
  }

  // 這裡設定 Parent 可以藉由 useRef 取得到什麼
  useImperativeHandle(ref, () => mergedRefs)

  return <div>Child component</div>
})

const Parent = () => {
  const childRef = useRef()

  const handleClick = () => {
    console.log(childRef.current.childRef)
    console.log(childRef.current.foo)
    console.log(childRef.current.bar)
  }

  return (
    <div>
      <Child foo={fooRef} bar={barRef} ref={childRef} />
      <button onClick={handleClick}>Click me</button>
    </div>
  )
}

export default Parent
```
