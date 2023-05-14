---
title: React Router Dom
tags:
  - React
  - React Router Dom
  - NodeJs
  - frontend
---

[tutorial](https://reactrouter.com/en/main/start/tutorial)

```terminal
npm i react-router-dom
```

## 基本用法 Basic Usage

`<Route />` 必須位於 `<Routes />` 底下

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import { BrowserRouter, Routes, Route } from 'react-router-dom'

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
  <React.StrictMode>
      <BrowserRouter>
        <Routes>
          <Route path="/*" element={<App />} />
        </Routes>
      </BrowserRouter>
  </React.StrictMode>
)
```

- Route `index` 屬性代表預設路徑 [Ref](https://reactrouter.com/en/main/route/route#index)，等同於`/`

> Determines if the route is an index route. Index routes render into their parent's Outlet at their parent's URL (like a default child route).

- Routes 則是會根據網址去匹配底下的組件 [Ref](https://reactrouter.com/en/main/components/routes#routes)

> Rendered anywhere in the app, <Routes> will match a set of child routes from the current location.

```jsx
import { Routes, Route, Navigate } from 'react-router-dom'

function App() {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<PostsLists />} />

        <Route path="/post">
          <Route index element={<AddPostForm />} />
          <Route path=":postId" element={<SinglePostPage />} />
        </Route>
      </Route>

      {/* Catch all - replace with 404 component if you want */}
      <Route path="*" element={<Navigate to="/" replace />} />
    </Routes>
  )
}
```

`Outlet` 等同於 vue-router 的 router-view

```jsx
import { Outlet } from 'react-router-dom'

const Layout = () => {
  return (
    <main className="app">
      <Outlet />
    </main>
  )
}
```

`Link` 等同於 vue-router 的 router-link

```jsx
import { Link } from 'react-router-dom'

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
    </article>
  )
}
```

### useParams

```jsx
function App() {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route path="/post">
          <Route index element={<AddPostForm />} />
          <Route path=":postId" element={<SinglePostPage />} />
        </Route>
      </Route>
    </Routes>
  )
}
```

```jsx title="src\features\posts\SinglePostPage.tsx
const SinglePostPage = () => {
  const { postId } = useParams()
  const post = useAppSelector((state) => selectPostById(state, postId || ''))
  return post ? (
    <article>
      <h2>{post.title}</h2>
      <Link to={`/post/edit/${post.id}`}>Edit Post</Link>
    </article>
  ) : (
    <section>
      <h2>Post not found</h2>
    </section>
  )
}
```

### useNavigate

```jsx
import { useNavigate } from 'react-router-dom'

const EditPostForm = () => {
  const navigate = useNavigate()
  navigate(`/post/${postId}`)
}
```

### 預先處理

如果想要讓特定 route 共用特定的處理，可以這樣做

```jsx title="src\features\auth\Prefetch.tsx"
import { store } from '../../app/store'
import { notesApiSlice } from '../notes/notesApiSlice'
import { usersApiSlice } from '../users/usersApiSlice'
import { useEffect } from 'react'
import { Outlet } from 'react-router-dom'

const Prefetch = () => {
  useEffect(() => {
    console.log('subscribing')
    const notes = store.dispatch(notesApiSlice.endpoints.getNotes.initiate())
    const users = store.dispatch(usersApiSlice.endpoints.getUsers.initiate())

    return () => {
      console.log('unsubscribing')
      notes.unsubscribe()
      users.unsubscribe()
    }

    // 也可以寫成這樣
    useEffect(() => {
      store.dispatch(notesApiSlice.util.prefetch('getNotes', undefined, { force: true }))
      store.dispatch(usersApiSlice.util.prefetch('getUsers', undefined, { force: true }))
    }, [])
  }, [])

  return <Outlet />
}
export default Prefetch
```

```jsx title="src\App.tsx"
import { Routes, Route } from 'react-router-dom'

import Layout from './components/Layout'
import Public from './components/Public'
import DashLayout from './components/DashLayout'

import Login from './features/auth/Login'
import Welcome from './features/auth/Welcome'
import Prefetch from './features/auth/Prefetch'
import NotesList from './features/notes/NotesList'
import UsersList from './features/users/UsersList'

function App() {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<Public />} />

        <Route path="login" element={<Login />} />

        {/* 這裡把 dash、notes、users route 都做預先處理，進入前都會取得最新資料 */}
        <Route element={<Prefetch />}>
          <Route path="dash" element={<DashLayout />}>
            <Route index element={<Welcome />} />

            <Route path="notes">
              <Route index element={<NotesList />} />
            </Route>

            <Route path="users">
              <Route index element={<UsersList />} />
            </Route>
          </Route>
        </Route>
      </Route>
    </Routes>
  )
}

export default App
```

[Prefetching Without Hooks](https://redux-toolkit.js.org/rtk-query/usage/prefetching#prefetching-without-hooks)
[Removing a subscription](https://redux-toolkit.js.org/rtk-query/usage/usage-without-react-hooks#removing-a-subscription)