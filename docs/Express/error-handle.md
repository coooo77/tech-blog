---
title: Express 錯誤處理
tags:
  - NodeJs
  - Express
  - backend
  - server
---

## dependencies

### express-async-handler

route 使用 express-async-handler

```typescript title="src/controllers/authController.ts"
import asyncHandler from 'express-async-handler'

export default {
  login: asyncHandler(async (req, res) => {
    // ....
  }),
}
```

### express-async-errors

直接在最頂層 app.ts 使用

```typescript title="src\index.ts"
import 'express-async-errors'
import express from 'express'

const app = express()
// ...
```

## error middleware

前面的 module 安裝後，錯誤就能直接捕捉

```typescript title="src\middleware\errorHandler.ts"
// 自定義 Debugger logger
import { logEvents } from './logger'

import type { Request, Response, NextFunction } from 'express'

const ERROR_LOG_FILENAME = 'errLog.log'

export function errorHandler(error: Error, req: Request, res: Response, next: NextFunction) {
  const msg = `${error.name}: ${error.message}\t${req.method}\t${req.url}\t${req.headers.origin}`

  logEvents(msg, ERROR_LOG_FILENAME)

  const status = res.statusCode || 500

  res.status(status)

  res.json({ message: error.message, isError: true, _from: 'errorHandler' })
}
```
