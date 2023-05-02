---
title: React Redux / RTK Query 進階
tags:
  - frontend
  - redux
---

## query 客製化

RTK Query 可以做客製化設定，達到axios interceptor的效果。例如預設 api [帶上 token](https://redux-toolkit.js.org/rtk-query/api/fetchBaseQuery#setting-default-headers-on-requests)

```ts
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react'
import type { RootState } from '../store'

const baseQuery = fetchBaseQuery({
  baseUrl: 'http://localhost:3500',
  credentials: 'include',
  prepareHeaders: (headers, { getState }) => {
    // 這裡取得存在 store 的 token
    const token = (getState() as RootState).auth.token

    if (token) {
      headers.set('authorization', `Bearer ${token}`)
    }
    return headers
  },
})

export const apiSlice = createApi({
  baseQuery,
  tagTypes: ['Note', 'User'],
  endpoints: (builder) => ({})
})
```

fetchBaseQuery 實際上是回傳一個 [function](https://redux-toolkit.js.org/rtk-query/api/fetchBaseQuery#signature)，專門處理 api

```ts
type FetchBaseQuery = (
  args: FetchBaseQueryArgs
) => (
  args: string | FetchArgs,
  api: BaseQueryApi,
  extraOptions: ExtraOptions
) => FetchBaseQueryResult
```

所以也可以針對回傳結果再進一步處理。這裡是 token 過期，refresh 後再重新請求的寫法

```ts
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react'

import { setCredentials } from '../../features/auth/authSlice'

import type { RootState } from '../store'

const baseQuery = fetchBaseQuery({
  baseUrl: 'http://localhost:3500',
  credentials: 'include',
  prepareHeaders: (headers, { getState }) => {
    const token = (getState() as RootState).auth.token

    if (token) {
      headers.set('authorization', `Bearer ${token}`)
    }
    return headers
  },
})

export const apiSlice = createApi({
  baseQuery: async (args, api, extraOptions) => {
    // console.log(args) // request url, method, body
    // console.log(api) // signal, dispatch, getState()
    // console.log(extraOptions) //custom like {shout: true}

    let result = await baseQuery(args, api, extraOptions)

    // If you want, handle other status codes, too
    if (result?.error?.status === 403) {
      console.log('sending refresh token')

      // send refresh token to get new access token
      const refreshResult = await baseQuery('/auth/refresh', api, extraOptions)

      if (refreshResult?.data) {
        // store the new token
        api.dispatch(setCredentials({ ...refreshResult.data }))

        // retry original query with new access token
        result = await baseQuery(args, api, extraOptions)
      } else {
        if (refreshResult?.error?.status === 403) {
          ;(refreshResult.error.data as { message: string }).message = 'Your login has expired. '
        }
        return refreshResult
      }
    }

    return result
  },
  tagTypes: ['Note', 'User'],
  endpoints: (builder) => ({}),
})
```

## error response 處理

見 [Error result example](https://redux-toolkit.js.org/rtk-query/usage-with-typescript#error-result-example)

## Usage Without React Hooks

[cache behavior](https://redux-toolkit.js.org/rtk-query/usage/usage-without-react-hooks)