---
title: deploy docusaurus
tags:
  - docusaurus
  - deploy
  - github
---

## 在GitHub上部屬Docusaurus

Docusaurus使用版本為 2.2.0

### 前置作業

1. 在github建立repository，這裡假設命名為`myBlog`，github帳號為`myAccount`，license可以考慮選擇MIT

2. 把`my-blog` git clone到本地，執行docusaurus指令安裝
```console
// /my-blog
npx create-docusaurus@latest . classic --typescript
```

3. **務必**要用下面的設定
```jsx title="docusaurus.config.js"
module.exports = {
  // ...
  url: 'https://myAccount.github.io', // Your website URL
  baseUrl: '/myBlog/',
  projectName: 'myBlog',
  organizationName: 'myAccount',
  trailingSlash: false,
  // ...
};
```

4. 完成上列步驟後，可以選擇使用[指令](https://docusaurus.io/docs/deployment#deploy)部屬
```console
// 指令
GIT_USER=<GITHUB_USERNAME> npm run deploy

// 例子
GIT_USER=myAccount npm run deploy
```

### 使用github action部屬：

1. 在github repo Settings > Pages 把 Source改為 GitHub Actions

![](https://i.imgur.com/NZdJt6y.png)

2. 建立yml檔案，名稱可以隨意

```yml title="myBlog\.github\workflows\deploy.yml"
name: Deploy static content to Pages

on:
  # push到main分支就會觸發自動部屬
  push:
    branches: [main]

  # 允許手動部屬
  workflow_dispatch:

# 設定GitHub權限
permissions:
  contents: read
  pages: write
  id-token: write

# Allow one concurrent deployment
concurrency:
  group: 'pages'
  cancel-in-progress: true

jobs:
  deploy:
    environment:
      name: github-pages
      # 這裡對應到step id等於deployment所產生的網址
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18
          cache: npm
      - name: Install dependencies
        run: npm ci
      - name: Build
        run: npm run build
      - name: Setup Pages
        uses: actions/configure-pages@v1
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          # 這裡選擇資料夾進行部屬，也就是npm run build產生的資料夾，名為build
          path: build
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v1
```

3. 當main更新時，自動更新就會觸發

### 使用github token與github action部屬：

1. 參照[Triggering deployment with GitHub Actions](https://docusaurus.io/docs/deployment#triggering-deployment-with-github-actions)的same repo設定，設定部屬yml檔案

```yml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main
    # Review gh actions docs if you want to further define triggers, paths, etc
    # https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#on

jobs:
  deploy:
    name: Deploy to GitHub Pages
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
          cache: npm

      - name: Install dependencies
        run: npm ci
      - name: Build website
        run: npm run build

      # Popular action to deploy to GitHub Pages:
      # Docs: https://github.com/peaceiris/actions-gh-pages#%EF%B8%8F-docusaurus
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GH_TOKEN }}
          # Build output to publish to the `gh-pages` branch:
          publish_dir: ./build
          # The following lines assign commit authorship to the official
          # GH-Actions bot for deploys to `gh-pages` branch:
          # https://github.com/actions/checkout/issues/13#issuecomment-724415212
          # The GH actions bot is used by default if you didn't specify the two fields.
          # You can swap them out with your own user credentials.
          user_name: github-actions[bot]
          user_email: 41898282+github-actions[bot]@users.noreply.github.com
```

2. github token生產：
![](https://i.imgur.com/Tg3MBI3.png)

![](https://i.imgur.com/NAx138c.png)

![](https://i.imgur.com/zaqrEWj.png)

![](https://i.imgur.com/uN447Xn.png)

3. repo設定secret

![](https://i.imgur.com/fYnluL2.png)

![](https://i.imgur.com/gVtfYXH.png)

4. yml設定token
```yaml
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          # 這裡就會取得secrets的GH_TOKEN值
          github_token: ${{ secrets.GH_TOKEN }}
          publish_dir: ./build
```

5. 當main更新時，自動更新就會觸發

### 參考

* [GitHub After Dark: Deploying Docusaurus to GitHub Pages using GitHub Actions](https://www.youtube.com/watch?v=ImSK0nv7KfY)
* [docusaurus - TypeScript Support](https://docusaurus.io/docs/typescript-support)
* [docusaurus - Deployment](https://docusaurus.io/docs/deployment)
* [starter-workflows (部屬yml參考)](https://github.com/actions/starter-workflows/blob/main/pages/static.yml)

### 其他

* [LOGO製作](https://www.freelogodesign.org/manager/signin?r=https%3A%2F%2Fwww.freelogodesign.org%2Fmanager)