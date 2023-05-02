---
title: React 基礎
tags:
  - frontend
  - react
---

## 安裝

[Adding TypeScript](https://create-react-app.dev/docs/adding-typescript/)

```terminal
npx create-react-app my-app --template typescript
```

## snippets

[ES7+ React/Redux/React-Native snippets](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)

```vscode
ctrl + alt + r
```

- rafce - create a react arrow function component with es7 module system

## 基本用法 Basic Usage

```jsx
function myApp() {
  const name = 'dave'
  const randomName = () => {
    const names = ['dave', 'kevin', 'bob']
    const int = Math.floor(Math.random() * names.length)
    return names[int]
  }
  return (
    <div className="App">
      <p>{name}</p>
      <p>{randomName()}</p>

      {/* ✔️ 可以用陣列 */}
      {/* ✔️ Array is acceptable */}
      <p>{[1, 2, 3]}</p>

      {/* ❌ 不接受物件 */}
      {/* ❌ Object is not allowed*/}
      <p>{{ a: 1 }}</p>

      <div className={isReady ? 'text-red-500' : null}></div>
    </div>
  )
}
```

## components

```jsx title="src\Header.tsx"
export const Header = () => {
  return <div>Header</div>
}
```

```jsx
import { Header } from './Header'
export const Header = () => {
  return (
    <div className="App">
      <Header />
    </div>
  )
}
```

## click events

```jsx
export const Header = () => {
  const handleClick1 = () => console.log('you clicked')
  const handleClick2 = (name) => console.log('you clicked', name)
  const handleClick3 = (event) => console.log('clickEvent', event)
  return (
    <button onclick={handleClick1}>Click</button>
    // with parameters
    <button onclick={() => handleClick2('Dave')}>Click</button>
    <button onDoubleClick={handleClick3}>Click</button>
  )
}
```

## useState

```jsx
import { useState } from 'react'

export const Header = () => {
  const [name, setName] = useState('Dave')
  const randomName = () => {
    const names = ['dave', 'kevin', 'bob']
    const int = Math.floor(Math.random() * names.length)
    setName(names[int])
  }
  return (
    <div className="App">
      <button onclick={randomName}>change name</button>
      <p>Your name is {name}</p>
    </div>
  )
}
```

:::caution

注意 React 會將狀態拍照(snapshot)，所以改動當下取值是沒有變化的，需要用 function 修改才會立即取得更動的數值
React snapshots state, so can not get changed values immediately. You should use function to get values from snapshot

- [Queueing a Series of State Updates](https://beta.reactjs.org/learn/queueing-a-series-of-state-updates)

:::

```jsx
import { useState } from 'react'

export const Header = () => {
  const [count, setCount] = useState(0)

  const add1 = () => {
    // ❌ count 是更改前的值，所以這樣等同於 +1 一次
    // ❌ will get value changed before, and it will add once
    setCount(count + 1)
    setCount(count + 1)
    setCount(count + 1)
    console.log(count)
  }
  const add2 = () => {
    // ✔️ 用 function 可以立即更動
    // ✔️ use function to get value immediately
    setCount((count) => count + 1)
    console.log(count)
  }

  return (
    <div className="App">
      <button onclick={add1}>add1</button>
      <p>Count {count}</p>
    </div>
  )
}
```

## render list

use `Array.prototype.map` and bind value with `key`

```jsx
import { useState } from 'react'
import { FaTrashAlt } from 'react-icons/fa'

export const Header = () => {
  const [items, setItems] = useState([
    {
      id: 1,
      checked: true,
      item: 'One half pound bag of Cocoa Covered Almonds Unsalted',
    },
    {
      id: 2,
      checked: false,
      item: 'Item 2',
    },
    {
      id: 3,
      checked: false,
      item: 'Item 3',
    },
  ])

  const handleCheck = (id) => {
    const listItems = items.map((item) => (item.id === id ? { ...item, checked: !item.checked } : item))
    setItems(listItems)
  }

  const handleDelete = (id) => {
    const listItems = items.filter((item) => item.id !== id)
    setItems(listItems)
  }

  return (
    <main>
      {items.length ? (
        <ul>
          {items.map((item) => (
            <li className="item" key={item.id}>
              <input type="checkbox" onChange={() => handleCheck(item.id)} checked={item.checked} />
              <label style={item.checked ? { textDecoration: 'line-through' } : null} onDoubleClick={() => handleCheck(item.id)}>
                {item.item}
              </label>
              <FaTrashAlt onClick={() => handleDelete(item.id)} role="button" tabIndex="0" />
            </li>
          ))}
        </ul>
      ) : (
        <p style={{ marginTop: '2rem' }}>Your list is empty.</p>
      )}
    </main>
  )
}
```

## props & prop drilling

```jsx title="components/Header.jsx"
export const Header = (props) => {
  return (
    <header>
      <h1>{props.title}</h1>
    </header>
  )
}

// 或者用解構
// or Destructuring assignment
export const Header1 = ({ title }) => {
  return (
    <header>
      <h1>{title}</h1>
    </header>
  )
}

// 可以設定預設值
// use defaultProps to set default value
export const Header2 = ({ title }) => {
  return (
    <header>
      <h1>{title}</h1>
    </header>
  )
}
Header2.defaultProps = {
  title: 'Default Title',
}

export const Header2 = ({ title = 'Default Title' }) => {
  return (
    <header>
      <h1>{title}</h1>
    </header>
  )
}
```

```jsx title="App.jsx"
import { Header } from './Header'

export const App = () => {
  return <Header title="this is my title" />
}
```

:::tip

將 props 層層傳遞(prop drilling)並不是一個好做法，參照官方說明解決。

you shouldn't pass properties using prop drilling, see documentation

- [Passing Data Deeply with Context](https://beta.reactjs.org/learn/passing-data-deeply-with-context#the-problem-with-passing-props)

:::

## Controlled Component Inputs && ref

```jsx title="AddItem.jsx"
import { FaPlus } from 'react-icons/fa'
import { useRef } from 'react'

const AddItem = ({ newItem, setNewItem, handleSubmit }) => {
  const inputRef = useRef()

  return (
    <form className="addForm" onSubmit={handleSubmit}>
      <label htmlFor="addItem">Add Item</label>
      <input autoFocus ref={inputRef} id="addItem" type="text" placeholder="Add Item" required value={newItem} onChange={(e) => setNewItem(e.target.value)} />
      <button type="submit" aria-label="Add Item" onClick={() => inputRef.current.focus()}>
        <FaPlus />
      </button>
    </form>
  )
}

export default AddItem
```

```jsx title="App.jsx"
import AddItem from './AddItem'

function App() {
  const [newItem, setNewItem] = useState('')

  // ...

  const handleSubmit = (e) => {
    e.preventDefault()
    if (!newItem) return
    addItem(newItem)
    setNewItem('')
  }

  return (
    <div className="App">
      <AddItem newItem={newItem} setNewItem={setNewItem} handleSubmit={handleSubmit} />
    </div>
  )
}

export default App
```

## useEffect hook

- [Synchronizing with Effects](https://beta.reactjs.org/learn/synchronizing-with-effects)

```jsx
import { useEffect } from 'react'

useEffect(() => {
  // This runs after every render
})

useEffect(() => {
  // This runs only on mount (when the component appears)
}, [])

useEffect(() => {
  // This runs on mount *and also* if either a or b have
  // changed since the last render
}, [a, b])

useEffect(() => {
  const connection = createConnection()
  connection.connect()
  // Add cleanup if needed
  return () => {
    connection.disconnect()
  }
}, [])
```

### custom hooks

- [Collection of React Hooks](https://nikgraf.github.io/react-hooks/)
- [react-use](https://github.com/streamich/react-use)
- [Rules of Hooks](https://reactjs.org/docs/hooks-rules.html)

#### 建立一個根據視窗大小顯示對應 icon 的 hook

```jsx
// src\hooks\useWindowSize.js
import { useState, useEffect } from 'react'

const useWindowSize = () => {
  const [windowSize, setWindowSize] = useState({
    width: undefined,
    height: undefined,
  })

  useEffect(() => {
    const handleResize = () => {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight,
      })
    }

    handleResize()

    window.addEventListener('resize', handleResize)

    // return clean up function
    return () => window.removeEventListener('resize', handleResize)
  }, [])

  return windowSize
}

export default useWindowSize
```

把 useWindowSize 傳入 header 使用

```jsx
// src\Header.js
import { FaLaptop, FaTabletAlt, FaMobileAlt } from 'react-icons/fa'

const Header = ({ title, width }) => {
  return (
    <header className="Header">
      <h1>{title}</h1>
      {width < 768 ? <FaMobileAlt /> : width < 992 ? <FaTabletAlt /> : <FaLaptop />}
    </header>
  )
}

export default Header
```

- [Axios/取消请求](https://axios-http.com/zh/docs/cancellation)

#### Axios API fetch hook

```javascript
// src\hooks\useAxiosFetch.js
import { useState, useEffect } from 'react'
import axios from 'axios'

const useAxiosFetch = (dataUrl) => {
  const [data, setData] = useState([])
  const [fetchError, setFetchError] = useState(null)
  const [isLoading, setIsLoading] = useState(false)

  useEffect(() => {
    let isMounted = true
    const source = axios.CancelToken.source()

    const fetchData = async (url) => {
      setIsLoading(true)
      try {
        const response = await axios.get(url, {
          cancelToken: source.token,
        })
        if (isMounted) {
          setData(response.data)
          setFetchError(null)
        }
      } catch (err) {
        if (isMounted) {
          setFetchError(err.message)
          setData([])
        }
      } finally {
        isMounted && setIsLoading(false)
      }
    }

    fetchData(dataUrl)

    return () => {
      isMounted = false
      source.cancel()
    }
  }, [dataUrl])

  return { data, fetchError, isLoading }
}

export default useAxiosFetch
```

```javascript
import Feed from './Feed'
import { useEffect } from 'react'

const Home = () => {
  const [posts, setPosts] = useState([])
  const { data, fetchError, isLoading } = useAxiosFetch('http://localhost:3500/posts')

  useEffect(() => {
    setPosts(data)
  }, [data])

  return (
    <main className="Home">
      {isLoading && <p className="statusMsg">Loading posts...</p>}
      {!isLoading && fetchError && (
        <p className="statusMsg" style={{ color: 'red' }}>
          {fetchError}
        </p>
      )}
      {!isLoading && !fetchError && (posts.length ? <Feed posts={posts} /> : <p className="statusMsg">No posts to display.</p>)}
    </main>
  )
}

export default Home
```
