---
title: React 相關型別
tags:
  - frontend
---

```tsx
type StyleProps = React.CSSProperties
const Container = (styles: StyleProps) => (<div styles={styles}>Text goes here</div>)

type Children = React.ReactNode
const List = (props: {children: Children}) => <div>{props.children}</div>

type HandleChange = (event: React.ChangEvent<HTMLInputElement>) => void

type customComponent = React.FC


type ButtonComponent = React.ComponentProps<'button'>
const CustomButton = ({children, ...rest}: ButtonComponent) => {
  return (
    <button {...rest}>
      {children}
    </button>
  )
})

type ButtonComponent2 = React.ComponentProps<typeof CustomButton>
type ButtonComponent3 = Omit<React.ComponentProps<typeof CustomButton>, 'children'>
```

Reducer

```tsx
import { useReducer } from 'react'

type State = {
  count: number
}

type Action = { type: 'increment' } | { type: 'decrement' } | { type: 'reset'; payload: number }

function init(initialCount: number): State {
  return { count: initialCount }
}

function reducer(state: State, action: Action): State {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 }
    case 'decrement':
      return { count: state.count - 1 }
    case 'reset':
      return init(action.payload)
    default:
      throw new Error()
  }
}

function Counter({ initialCount }: { initialCount: number }) {
  const [state, dispatch] = useReducer(reducer, initialCount, init)

  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'reset', payload: initialCount })}>Reset</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
    </>
  )
}

export default Counter
```
