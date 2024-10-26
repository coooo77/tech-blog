---
title: 使用 NodeJs 建立下載圖片
tags:
  - NodeJs
---

```typescript
'use strict'
import fs from 'fs'
import path from 'path'
import axios from 'axios'

import { fileURLToPath } from 'url'

const __filename = fileURLToPath(import.meta.url)
const __dirname = path.dirname(__filename)

function wait(sec: number) {
  return new Promise((res) => setTimeout(res, sec * 1000))
}

async function main() {
  // 取得圖片網址來源，可藉由 puppeteer 或 直接在網站上使用 $$、document.querySelectorAll 取得網址
  const urls = ['https://website/some/img001.jpg', 'https://website/some/img002.jpg']

  for (let i = 0; i < urls.length; i++) {
    try {
      const url = urls[i]

      const res = await axios.get(url, {
        responseType: 'stream',
        // 取得圖片必須的 headers
        headers: {
          authority: 'authority',
          Accept: 'Accept',
          Referer: 'Referer',
        },
      })

      await wait(0.1)

      const filePath = path.join(__dirname, `img_${String(i + 1).padStart(2, '0')}.jpg`)

      const writer = fs.createWriteStream(filePath)
      res.data.pipe(writer)

      writer.on('finish', () => {
        console.log('圖片下載完成！')
      })
    } catch (error) {
      console.error(error)
    }
  }

  console.log('done')
}

main()
```
