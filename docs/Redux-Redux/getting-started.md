---
title: React Redux 基礎
tags:
  - frontend
  - redux
---

## 安裝

```terminal
npm install @reduxjs/toolkit react-redux
```

### 基本設定

- [[Redux] Redux TypeScript 筆記](https://pjchender.dev/react/redux-typescript/#%E5%BB%BA%E7%AB%8B%E5%B8%B6%E6%9C%89%E5%9E%8B%E5%88%A5%E5%AE%9A%E7%BE%A9%E7%9A%84-usedispatch-%E5%92%8C-useselectordefine-typed-hooks)

#### 引用 store

```tsx title="src\main.tsx"
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'

// 引用 store 並用 Provider 將 store 注入到 APP 中
// import redux store to app
import { store } from './app/store'
import { Provider } from 'react-redux'

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
)
```

#### 建立 slice

```tsx title="src\features\counter\counterSlice.ts"
import { createSlice, PayloadAction } from '@reduxjs/toolkit'
import { RootState } from '../../app/store'

interface Post {
  id: string
  title: string
  content: string
}

// 定義 initialState 的型別資訊，目的是讓 createSlice 知道該 slice 中 state 的型別資訊
// define type of initialState, createSlice will know what state should be
export interface CounterState {
  value: number
  posts: Post[]
}

export const initialState: CounterState = {
  value: 0,
  posts: [],
}

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    // 直接操作 state
    // use state directly
    increment: (state) => {
      state.value++
    },
    // 直接回傳整個 state
    // return whole state object
    decrement: (state) => ({ value: state.value - 1 }),
    // 使用 PayloadAction 型別來定義 action.payload 能夠接受的型別
    // use type PayloadAction to confine what payload should be
    // 這裡的 action.payload 需要時 number
    // define action.payload as number
    incrementByAmount: (state, action: PayloadAction<number>) => ({
      value: state.value + action.payload,
    }),
    reset: (state) => ({ value: 0 }),
    // redux 使用 immer.js ， 所以可以放心針對物件直接操作
    // redux handle mutations with immer.js
    postAdded(state, action: PayloadAction<Post>) {
      state.post.push(action.payload)
    },
  },
})

// 為了避免未來 initialState 結構有變化，改在這邊輸出取得 value 的方法
const getAllValues = (state: RootState) => state.counter.value

// 輸出 actions 讓 useDispatch 使用
// Action creators are generated for each case reducer function
export const { increment, decrement, incrementByAmount, reset, postAdded } = counterSlice.actions

export default counterSlice.reducer
```

另外 reducer 也有物件寫法
you can write reducers with a object

```tsx
export const postsSlice = createSlice({
  name: 'posts',
  initialState,
  reducers: {
    postAdded_001(state, action: PayloadAction<Post>) {
      state.push(action.payload)
    },

    postAdded_002: {
      reducer(state, action: PayloadAction<Post>) {
        state.push(action.payload)
      },
      // 傳值前預先處理
      prepare(title, content) {
        return {
          payload: {
            id: crypto.randomUUID(),
            title,
            content,
          },
        }
      },
    },
  },
})
```

傳入的參數就可以簡化

```tsx
import { postAdded } from './postsSlice'
import { useAppDispatch } from '../../app/utils'

const AddPostForm = () => {
  const dispatch = useAppDispatch()

  dispatch_001(
    postAdded({
      id: crypto.randomUUID(),
      title,
      content,
    })
  )

  dispatch(postAdded_002(title, content))
}
```

#### 將 slice 引用到 store 當中

```tsx title="src\app\store.ts"
import { configureStore } from '@reduxjs/toolkit'
import counterReducer from '../features/counter/counterSlice'

export const store = configureStore({
  reducer: { counter: counterReducer },
})

// define types for redux
// https://redux.js.org/usage/usage-with-typescript#define-root-state-and-dispatch-types
export type AppDispatch = typeof store.dispatch
export type RootState = ReturnType<typeof store.getState>
```

#### 建立 hook 型別

```tsx title="src\app\utils.ts"
import { TypedUseSelectorHook, useDispatch, useSelector } from 'react-redux'

import type { AppDispatch, RootState } from './store'

// 之後在元件中，使用帶有型別資訊的 useDispatch 和 useSelector
// define useDispatch and useSelector by state of store
export const useAppDispatch = () => useDispatch<AppDispatch>()
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector
```

#### 使用 slice

```tsx title="src\features\counter\Counter.tsx"
import { useState } from 'react'

import { decrement, increment, incrementByAmount, reset } from './counterSlice'
import { useAppDispatch, useAppSelector } from '../../app/utils'

const Counter = () => {
  const useDispatch = useAppDispatch()
  // 這裡的 state.counter 對應到 store.ts 的 reducer: { counter: counterReducer }
  const count = useAppSelector((state) => state.counter.value)

  const [incrementAmount, setIncrementAmount] = useState(0)
  const addValue = Number(incrementAmount) || 0

  const resetAll = () => {
    setIncrementAmount(0)
    useDispatch(reset())
  }
  return (
    <section className="counter">
      <p>{count}</p>

      <div>
        <button onClick={() => useDispatch(increment())}>+</button>
        <button onClick={() => useDispatch(decrement())}>-</button>
      </div>

      <input type="number" value={incrementAmount} onChange={(e) => setIncrementAmount(+e.target.value)} />

      <div>
        <button onClick={() => useDispatch(incrementByAmount(addValue))}>Add Amount</button>
        <button onClick={resetAll}>Reset</button>
      </div>
    </section>
  )
}

export default Counter
```

#### thunk

> The word "thunk" is a programming term that means "a piece of code that does some delayed work."

##### 建立 thunk

```tsx
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit'
import axios from 'axios'

const POSTS_URL = 'https://jsonplaceholder.typicode.com/posts'

interface PostsFetched {
  userId: number
  id: number
  title: string
  body: string
}

// 這裡需要定義該 API 返回的型別，返回的值會放到 action.payload 中
export const fetchPosts = createAsyncThunk('posts/fetchPosts', async () => {
  const { data } = await axios.get<PostsFetched[]>(POSTS_URL)
  return data
})

export type Status = 'idle' | 'loading' | 'succeeded' | 'failed'

export interface PostsState {
  posts: Post[]
  status: Status
  error: undefined | string
}
export const initialState: PostsState = {
  posts: [],
  status: 'idle',
  error: undefined,
}

export const postsSlice = createSlice({
  name: 'posts',
  initialState,
  reducers: {
    // ...
  },
  // fetchPosts 會定義各種 API 結果，針對各結果對 store 處理
  extraReducers(builder) {
    builder
      .addCase(fetchPosts.pending, (state, action) => {
        state.status = 'loading'
      })
      .addCase(fetchPosts.fulfilled, (state, action) => {
        state.status = 'succeeded'
        // action.payload 的型別為 PostsFetched[]
        const loadPosts = action.payload.map((post) => ({
          id: String(post.id),
          title: post.title,
          userId: String(post.userId),
          content: post.body,
        }))
        state.posts = state.posts.concat(loadPosts)
      })
      .addCase(fetchPosts.fulfilled, (state, action) => {
        // 等同於 state = action.payload
        return action.payload
      })
      .addCase(fetchPosts.rejected, (state, action) => {
        state.status = 'failed'
        state.error = action.error.message
      })
  },
})

export const getPostsStatus = (state: RootState) => state.posts.status
export const getPostsError = (state: RootState) => state.posts.error
```

##### 使用 thunk

```tsx
import { useEffect } from 'react'
import { useAppDispatch } from '../../app/utils'
import { selectAllPosts, getPostsStatus, getPostsError, fetchPosts } from './postsSlice'

const PostsLists = () => {
  const dispatch = useAppDispatch()

  const postsError = useAppSelector(getPostsError)
  const postsStatus = useAppSelector(getPostsStatus)

  useEffect(() => {
    if (postsStatus === 'idle') {
      // 呼叫 API ，針對結果 postsStatus、postsError 顯示畫面
      dispatch(fetchPosts())
    }
  }, [postsStatus])

  let content = null
  switch (postsStatus) {
    case 'loading':
      content = <p>"Loading ..."</p>
      break
    case 'succeeded':
      content = posts.slice().map((post) => <PostExcerpt key={post.id} post={post} />)
      break
    case 'failed':
      content = <p>{postsError}</p>
      break
    default:
      break
  }

  return (
    <section>
      <h2>Posts</h2>
      {content}
    </section>
  )
}
```

順便一提 thunk 有提供`.unwrap`函式，執行時等同於呼叫 Promise，可以 await 也方便錯誤處理

> Redux Toolkit adds a .unwrap() function to the returned Promise, which will return a new Promise that either has the actual action.payload value from a fulfilled action, or throws an error if it’s the rejected action. This lets us handle success and failure in the component using normal try/catch logic

```tsx
const onSavePostClicked = async () => {
  try {
    // res 會是打API的response
    // 若沒有執行unwrap()，會得到action
    const res = await dispatch(addNewPost({ title, content, user: userId })).unwrap()
  } catch (err) {
    console.error('Failed to save the post: ', err)
  }
}
```

##### createSelector

React DevTools Profiler 可以測 re-render 的問題，例如下列 code

```tsx
export const UserPage = ({ match }) => {
  const { userId } = match.params

  const user = useSelector((state) => selectUserById(state, userId))

  const postsForUser = useSelector((state) => {
    const allPosts = selectAllPosts(state)
    return allPosts.filter((post) => post.user === userId)
  })

  // omit rendering logic
}
```

因為 postsForUser 回傳 Array.prototype.filter 的結果(每次 filter 結果都不相等)，所以每次 state 無關 user 的內容被更新時，postsForUser 會另外更新，造成 UserPage 組件 re-render

```tsx
const arr = [1, 2, 3]
const a = arr.filter((i) => i > 2)
const b = arr.filter((i) => i > 2)
console.log(a === b) // false
```

![](https://i.imgur.com/Bpynbjx.png)

橘色的部分就是觸發 Render 動作時 render 的組件，灰色則是沒有 re-render 的組件

#### Memoizing Selector Functions

上述的問題類似於 react 的 useEffect，可以用類似 useMemo 的方式處理，RTK 提供`createSelector`用於取代`useSelector`

```tsx
// features/posts/postsSlice.js
import { createSlice, createAsyncThunk, createSelector } from '@reduxjs/toolkit'

// omit slice logic

export const selectAllPosts = (state) => state.posts.posts
export const selectPostById = (state, postId) => state.posts.posts.find((post) => post.id === postId)

export const selectPostsByUser = createSelector([selectAllPosts, (state, userId) => userId], (posts, userId) => posts.filter((post) => post.user === userId))

// 原本
const postsForUser = useSelector((state) => {
  const allPosts = selectAllPosts(state)
  return allPosts.filter((post) => post.user === userId)
})

// 改成
const postsForUser = useSelector((state) => selectPostsByUser(state, userId))
```

另外 createSelector 回傳的 function 型別是看 input fn、output fn 傳入的東西決定

```tsx
export const selectPostsByUser = createSelector(
  // 這裡的array代表一個function應該傳入的params，分別是state、userId, showResult
  [(state) => state.posts, (_, userId) => userId, (_, _, showResult) => showResult],
  // 藉由input function，用state取得posts，用userId取得id, 用showResult取得result
  (posts, id, result) => posts.filter((post) => post.user === id)
)

// selectPostsByUser接受的參數就是input function 的 state、userId, showResult
const postsForUser = useSelector((state) => selectPostsByUser(state, userId, result))
```

特別注意的是`createSelector`第一個參數是 input selector functions，也就是原本的 selector (型別是 (state, ...params) => result)，這個 selector function 的回傳結果，會變成`createSelector`第二個參數 output selector function 的引數

selectPostsByUser(state, userId)會檢查傳入的 state, userId 是否有更動，是的話就回傳最新的 state

```tsx
const selectAllPosts = (state) => state.posts.posts

const selectUserId = (state, userId) => userId

export const selectPostsByUser = createSelector(
  [selectAllPosts, selectUserId],
  // 這裡的posts就是selectAllPosts回傳的state.posts.posts
  // 這裡的userId就是selectUserId回傳的userId
  (posts, userId) => posts.filter((post) => post.user === userId)
)
```

另一種解法是使用`memo`

```tsx
import React from 'react'
import { Link } from 'react-router-dom'
import PostAuthor from './PostAuthor'
import ReactionButtons from './ReactionButtons'

const PostExcerpt = (props: { post: Post }) => {
  const { id, content, title, userId } = props.post
  return (
    <article>
      <h3>{title}</h3>
      <p>{content.substring(0, 75)}...</p>
      <p className="postCredit">
        <Link to={`post/${id}`}>View Post</Link>
        <PostAuthor userId={userId} />
      </p>
      <ReactionButtons post={props.post} />
    </article>
  )
}

export default React.memo(PostExcerpt)
```

#### Normalization

Normalized state 定義

1. state 的 data 只會有一份，不會有額外複製的一份
2. 用 hash map 方式取得資料
3. 特定 data type 會保存 IDs

範例

```javascript
{
  users: {
    ids: ["user1", "user2", "user3"],
    entities: {
      "user1": {id: "user1", firstName, lastName},
      "user2": {id: "user2", firstName, lastName},
      "user3": {id: "user3", firstName, lastName},
    }
  }
}

// 取得資料
const userId = 'user2'
const userObject = state.users.entities[userId]
```

Redux 提供`createEntityAdapter` API，把 store 的資料存到` { ids: [], entities: {} }`當中，然後回傳 reducer 與 selectors

這個 API 有幾個特點

- 不用自己寫 normalization
- createEntityAdapter builder 會提供資料的 CRUD 方法
- 根據資料排序 ID - sortComparer (等同於 Array.sort())
- 提供`getSelectors`取得特定 selector 方法，例如 selectAll、selectById
- 用`getInitialState`客製初始化資料

##### 實作

initialState 設定

```tsx
export interface Post {
  id: string
  title: string
  content: string
  userId: string
  reactions: Reactions
  date: string
}

export interface PostsState {
  status: PostStatus
  error: undefined | string
  count: number
}

// 原本 initialState 是這樣設定
export const initialState: PostsState = {
  posts: [],
  status: 'idle',
  error: undefined,
  count: 0,
}

// 改為
const postAdapter = createEntityAdapter<Post>({
  // 預設是取得entities裡面元素的id，如果沒有會報錯，需要另外指定
  selectId: (post) => post.title,
  sortComparer: (a, b) => b.date.localeCompare(a.date),
})

export const initialState = postAdapter.getInitialState({
  status: 'idle',
  error: undefined,
  count: 0,
} as PostsState)
```

adapter 使用在 createSlice

```tsx
export const postsSlice = createSlice({
  name: 'posts',
  initialState,
  reducers: {
    reactionAdded(state, action: PayloadAction<{ postId: string; reaction: keyof Reactions }>) {
      const { postId, reaction } = action.payload
      // const existingPost = state.posts.find((post) => post.id === postId)
      const existingPost = state.entities[postId]
      if (existingPost) existingPost.reactions[reaction]++
    },
  },
  extraReducers(builder) {
    builder
      // ...
      .addCase(fetchPosts.fulfilled, (state, action) => {
        const loadPosts = action.payload.map(postFetchedToPost)
        // state.posts = state.posts.concat(loadPosts)
        postAdapter.upsertMany(state, loadPosts)
      })
      .addCase(addNewPost.fulfilled, (state, action) => {
        // state.posts.push(postFetchedToPost(action.payload, state.posts.length))
        postAdapter.addOne(state, postFetchedToPost(action.payload, Object.keys(state.entities).length))
      })
      .addCase(updatePost.fulfilled, (state, action) => {
        const { id } = action.payload
        if (!id) return
        const otherPost = state.posts.filter((post) => post.id !== id)
        // state.posts = otherPost.concat(action.payload)
        postAdapter.upsertOne(state, action.payload)
      })
      .addCase(deletePost.fulfilled, (state, action) => {
        const { id } = action.payload
        if (!id) return
        // state.posts = state.posts.filter((post) => post.id !== id)
        postAdapter.removeOne(state, id)
      })
  },
})
```

selector 替換

```tsx
// 過去寫法
export const selectAllPosts = (state: RootState) => state.posts.posts
export const selectPostById = (state: RootState, postId: string) => state.posts.posts.find((post) => post.id === postId)

// 現在能一次回傳所有類型selector
export const {
  selectAll: selectAllPosts,
  selectById: selectPostById,
  selectIds: selectPostIds
} = postAdapter.getSelectors<RootState>((state) => state.posts)
```
