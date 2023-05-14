---
title: React Redux / RTK Query
tags:
  - frontend
  - redux
---

## 基本設定

定義方法

```tsx title="src\features\api\apiSlice.ts"
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react'

interface Todo {
  userId: number
  id: number
  title: string
  completed: boolean
}

export const apiSlice = createApi({
  // 預設 redux store 會用哪個 reducer 來 cache api 的結果，預設是字串 'api'，所以這段可以不用寫
  reducerPath: 'api',
  baseQuery: fetchBaseQuery({ baseUrl: 'http://localhost:3500' }),
  endpoints: (builder) => ({
    // 這裡用泛型定義getTodos返回Todo，並且參數是void，也就是不需要參數
    getTodos: builder.query<Todo, void>({ query: () => '/todos' }),
  }),
})

export const { useGetTodosQuery } = apiSlice
```

使用

```tsx
import { useGetTodosQuery } from '../api/apiSlice'

const TodoList = () => {
  const { data: todos, isLoading, isSuccess, isError, error } = useGetTodosQuery()

  let content = null
  if (isLoading) content = <p>Loading ...</p>
  if (isSuccess) content = JSON.stringify(todos)
  if (isError) content = <p>{error.toString()}</p>
}
```

## 注入 apiSlice

方法有兩種

1. 在 html 注入

```tsx title="src\main.tsx"
import React from 'react'
import ReactDOM from 'react-dom/client'

// import { store } from './app/store'
// import { Provider } from 'react-redux'

import { apiSlice } from './features/api/apiSlice'
import { ApiProvider } from '@reduxjs/toolkit/query/react'

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
  <React.StrictMode>
    {/* <Provider store={store}> */}
      <ApiProvider api={apiSlice}>
        <BrowserRouter>
          <Routes>
            <Route path="/*" element={<App />} />
          </Routes>
        </BrowserRouter>
      {/* </ApiProvider> */}
    </Provider>
  </React.StrictMode>
)
```

:::caution

如果已經使用 store 的 Provider，會造成衝突，ApiProvider、Provider 不能一起使用；所以使用 Provider 的時候，應該定義 apiSlice 至 store，而不是用 ApiProvider

:::

2. 在 store 中定義

```tsx title="src\app\store.ts"
import { configureStore } from '@reduxjs/toolkit'

import { apiSlice } from '../features/api/apiSlice'
import usersReducer from '../features/users/usersSlice'

export const store = configureStore({
  reducer: {
    users: usersReducer,
    [apiSlice.reducerPath]: apiSlice.reducer,
  },
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(apiSlice.middleware),
})
```

## 進階用法

```tsx title="src\features\api\apiSlice.ts"
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react'

export interface Todo {
  userId: number
  id: number
  title: string
  completed: boolean
}

export const apiSlice = createApi({
  reducerPath: 'api',
  baseQuery: fetchBaseQuery({ baseUrl: 'http://localhost:3000' }),
  // redux 會給這個 api 一個 tag，用這個tag 做 cache 的依據
  tagTypes: ['Todos'],
  endpoints: (builder) => ({
    getTodos: builder.query<Todo[], void>({
      query: () => '/todos',
      // providesTags 代表負責提供 todos
      providesTags: ['Todos'],
      // 取得的資料可以做預設處理，這裡把 todos 按照 id 排列
      transformResponse: (res: Todo[]) => res.sort((a, b) => b.id - a.id),
      // Handling non-standard Response status codes
      // https://redux-toolkit.js.org/rtk-query/api/fetchBaseQuery#handling-non-standard-response-status-codes
      validateStatus: (response, result) => {
        return response.status === 200 && !result.isError
      },
    }),

    addTodo: builder.mutation<Todo, Omit<Todo, 'id'>>({
      query: (todo) => ({
        url: '/todos',
        method: 'POST',
        body: todo,
      }),
      // invalidatesTags 代表執行這個 api 後，會 invalidates Todos ，也就是之前cache的todos已經不是最新了
      // 需要更新，所以觸發 getTodos
      invalidatesTags: ['Todos'],
    }),

    updateTodo: builder.mutation<Todo, Todo>({
      query: (todo) => ({
        url: `/todos/${todo.id}`,
        method: 'PATCH',
        body: todo,
      }),
      invalidatesTags: ['Todos'],
    }),

    deleteTodo: builder.mutation<void, number>({
      query: (id) => ({
        url: `/todos/${id}`,
        method: 'DELETE',
        body: id,
      }),
      invalidatesTags: ['Todos'],
    }),
  }),
})

export const { useGetTodosQuery, useAddTodoMutation, useUpdateTodoMutation, useDeleteTodoMutation } = apiSlice
```

:::note
關於 validateStatus，如果要寫在 injectEndpoints 裡面，要另外改成下列寫法

[Using validateStatus on RTK Query baseApi](https://stackoverflow.com/questions/72647439/using-validatestatus-on-rtk-query-baseapi)

```tsx
const apiSlice = createApi({
  // 不重要的設定
})

// 這邊是後端出錯會傳的客製錯誤訊息
export interface ErrorRes {
  message: string
  isError: boolean
}

const notesApiSlice = apiSlice.injectEndpoints({
  endpoints: (builder) => ({
    getNotes: builder.query<EntityState<Note>, void>({
      // 這裡要改成回傳成物件形式
      query: () => ({
        url: '/notes',
        // 這邊的 response 是 fetch api 的 Response
        validateStatus: (response: Response, result: NoteRes[] | ErrorRes) => {
          const isFailed = response.status !== 200
          // 這邊的 result.isError 是後端 api 自行定義的 key
          const isError = !Array.isArray(result) && result.isError
          return !(isFailed || isError)
        },
      }),
      // 其他設定還是寫在外面
      transformResponse: (responseData: NoteRes[]) => {
        const loadedNotes = responseData.map((note) => ({ ...note, id: note._id }))
        return notesAdapter.setAll(initialState, loadedNotes)
      },
      providesTags: (result, error, arg) => {
        return result?.ids
          ? [{ type: 'Note' as const, id: 'LIST' }, ...result.ids.map((id) => ({ type: 'Note' as const, id }))]
          : [{ type: 'Note' as const, id: 'LIST' }]
      },
    })
})
```

:::

```tsx title="src\features\todos\TodoList.tsx"
import { useGetTodosQuery, useAddTodoMutation, useUpdateTodoMutation, useDeleteTodoMutation } from '../api/apiSlice'

// Mutation 取得陣列，第一個元素是執行該 mutation 的 function ；第二個則是返回內容、錯誤訊息、是否已經初始化等等
const [addTodo, { isSuccess, isUninitialized }] = useAddTodoMutation()
const [updateTodo] = useUpdateTodoMutation()
const [deleteTodo] = useDeleteTodoMutation()

// Query 取得物件，包含返回的內容
const { data: todos, isLoading, isSuccess, isError, error } = useGetTodosQuery()

// 執行時，需要unwrap()，變成promise
await addTodo().unwrap()

// 另外也可以指定 Query 回傳結果
const {
  user,
  isLoading: isLoadingUser,
  isSuccess: isSuccessUser,
  isError: isErrorUser,
} = useGetUsersQuery(undefined, {
  // selectFromResult 的 參數為 useGetUsersQuery 原本的返回值，
  // 這裡的返回值會取代 useGetUsersQuery 的返回值
  selectFromResult: ({ data, isLoading, isSuccess, isError, error }) => ({
    user: userId ? data?.entities[userId] : undefined,
    isLoading,
    isSuccess,
    isError,
    error,
  }),
})
```

## createApi 與 createEntityAdapter 一起使用

```tsx title="src\features\api\apiSlice.ts"
export const apiSlice = createApi({
  reducerPath: 'api',
  baseQuery: fetchBaseQuery({ baseUrl: 'http://localhost:3500' }),
  tagTypes: ['Post', 'User'],
  endpoints: (builder) => ({}),
})
```

```tsx title="src\features\posts\postsSlice.ts"
import { apiSlice } from '../api/apiSlice'

const postsAdapter = createEntityAdapter<Post>()
export const initialState = postsAdapter.getInitialState()

// 使用 injectEndpoints，在 endpoints 把資料提供給 adapter
export const extendedApiSlice = apiSlice.injectEndpoints({
  endpoints: (builder) => ({
    getPosts: builder.query<EntityState<Post>, void>({
      query: () => '/posts',
      transformResponse: (responseData: Post[]) => postsAdapter.setAll(initialState, responseData.map(transformPost)),
      providesTags: (result, error, arg) => {
        const postIds = result ? result?.ids.map((id) => ({ type: 'Post', id })) : []
        return [{ type: 'Post', id: 'LIST' }, ...postIds]
      },
    }),

    /** @see https://redux-toolkit.js.org/rtk-query/usage-with-typescript#typing-providestagsinvalidatestags */
    getPostsByUserId: builder.query<EntityState<Post>, string>({
      query: (id: string) => `/posts/?userId=${id}`,
      transformResponse: (responseData: Post[]) => postsAdapter.setAll(initialState, responseData.map(transformPost)),
      providesTags: (result, error, arg) => (result
        ? [...result.ids.map((id) => ({ type: 'Post' as const, id }))]
        : []
      )
    }),

    addNewPost: builder.mutation<void, Pick<Post, 'title' | 'body' | 'userId'>>({
      query: (initialPost) => ({
        url: '/posts',
        method: 'POST',
        body: initialPost
        },
      }),
      invalidatesTags: [{ type: 'Post', id: 'LIST' }],
    }),

    updatePost: builder.mutation<void, Post>({
      query: (initialPost) => ({
        url: `/posts/${initialPost.id}`,
        method: 'PUT',
        body: {
          ...initialPost,
          date: new Date().toISOString(),
        },
      }),
      invalidatesTags: (result, error, arg) => [{ type: 'Post', id: arg.id }],
    }),

    deletePost: builder.mutation<void, Post>({
      query: ({ id }) => ({
        url: `/posts/${id}`,
        method: 'DELETE',
        body: { id },
      }),
      invalidatesTags: (result, error, arg) => [{ type: 'Post', id: arg.id }],
    }),

    addReaction: builder.mutation<void, { postId: Post['id']; reactions: Post['reactions'] }>({
      query: ({ postId, reactions }) => ({
        url: `posts/${postId}`,
        method: 'PATCH',
        body: { reactions },
      }),
      async onQueryStarted({ postId, reactions }, { dispatch, queryFulfilled }) {
        // `updateQueryData` requires the endpoint name and cache key arguments,
        // so it knows which piece of cache state to update
        const patchResult = dispatch(
          extendedApiSlice.util.updateQueryData('getPosts', undefined, (draft) => {
            // The `draft` is Immer-wrapped and can be "mutated" like in createSlice
            const post = draft.entities[postId]
            if (post) post.reactions = reactions
          })
        )
        try {
          await queryFulfilled
        } catch {
          patchResult.undo()
        }
      },
    }),
  }),
})

// hooks
export const {
  useGetPostsQuery,
  useAddNewPostMutation,
  useUpdatePostMutation,
  useDeletePostMutation,
  useAddReactionMutation,
  useGetPostsByUserIdQuery
} = extendedApiSlice

// returns the query result object
export const selectPostsResult = extendedApiSlice.endpoints.getPosts.select()

// Creates memoized selector
const selectPostsData = createSelector(
  selectPostsResult,
  (postsResult) => postsResult.data // normalized state object with ids & entities
)

// getSelectors creates these selectors and we rename them with aliases using destructuring
export const {
  selectAll: selectAllPosts,
  selectById: selectPostById,
  selectIds: selectPostIds,
  // Pass in a selector that returns the posts slice of state
} = postsAdapter.getSelectors((state: RootState) => selectPostsData(state) ?? initialState)
```

:::tip

`builder.query` 會需要使用 `providesTags`，而與之相對應的則是`builder.mutation`需要使用`invalidatesTags`；它們之間作用關係是，當 mutation 執行時， cache 需要更新，因此用 tag 代表要執行那些 api 才能更新 cache。

舉例來說，addNewPost 的 invalidatesTags 是 `{ type: 'Post', id: 'LIST' }`，對應到 getPosts 的 providesTags，所以下次需要執行 getPosts 時，就不會拿之前 getPosts 的 cache 資料，而是直接執行一次。

再舉個例子；deletePost 的 invalidatesTags 是 `{ type: 'Post', id: arg.id }`，執行再舉個例子；deletePost 後，需要使用 getPosts、getPostsByUserId 時， 就會額外發動 API 去請求資料

:::

```tsx title=""
import { extendedApiSlice } from './features/posts/postsSlice'

// fetch posts right after app starts
store.dispatch(extendedApiSlice.endpoints.getPosts.initiate())

ReactDOM.createRoot(document.getElementById('root') as HTMLElement)
  .render
  // ...
  ()
```

## setupListeners

RTK Query API 如果想要進一步設定優化，需要設定 setupListeners

```typescript title="src\app\store.ts"
import { configureStore } from '@reduxjs/toolkit'
import { setupListeners } from '@reduxjs/toolkit/query'

import { apiSlice } from './api/apiSlice'

export const store = configureStore({
  reducer: {
    [apiSlice.reducerPath]: apiSlice.reducer,
  },
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(apiSlice.middleware),
  devTools: true,
})

// 設定 listener
setupListeners(store.dispatch)

export type AppDispatch = typeof store.dispatch
export type RootState = ReturnType<typeof store.getState>
```

```typescript title="src\features\notes\NotesList.tsx"
const {
  data: notes,
  isLoading,
  isSuccess,
  isError,
  error,
} = useGetNotesQuery(undefined, {
  // 有了 listener 就能設定這些優化功能
  pollingInterval: 15000,
  refetchOnFocus: true,
  refetchOnMountOrArgChange: true,
})
```
