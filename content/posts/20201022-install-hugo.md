---
title: "通过github Actions自动部署hugo到VPS"
date: 2020-10-22T23:19:18+08:00

tags: ["hugo"]
categories: ["文档"]

lightgallery: true
---

#### 安装并运行hugo(window)

[hugo下载地址](https://github.com/gohugoio/hugo/releases)   解压文件 并添加path到环境变量中

```
# 新建站点
hugo new blog
# 创建页面
hugo new about.md
hugo new posts/first.md
```

安装主题 `git clone https://github.com/dillonzq/LoveIt.git themes/LoveIt`

[主题文档说明](https://hugoloveit.com/zh-cn/)

####  创建git仓库

把hugo整个文件推到仓库，到github创建Atcions工作流模板

完整工作流模板

````
# .github/workflows/main.yml
# This is a basic workflow to help you get started with Actions

name: Deploy on push events

# Controls when the action will run. Triggers the workflow on push events
# but only for the master branch
on:
  push:
    branches: [master]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2
        # with:
        #   submodules: 'recursive'

      # Use ssh-agent to cache ssh keys
      - uses: webfactory/ssh-agent@v0.2.0
        with:
          ssh-private-key: |
            ${{ secrets.BLOG_DEPLOY_KEY }}
      - name: Scan public keys
        run: |
          ssh-keyscan www.linpx.cn >> ~/.ssh/known_hosts
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "0.75.1"
          extended: true

      - name: Build
        run: |
          hugo --minify
      - name: Deploy
        run: |
          rsync -av --delete public root@www.linpx.cn:/www/lppx-on-hugo

````

####  生成SSH密钥

```
ssh-keygen -t ed25519 -f ~/.ssh/blog_deploy_key
```

将公钥`~/.ssh/blog_deploy_key.pub`内容贴到VPS的`~/.ssh/authorized_keys`，将私钥`~/.ssh/blog_deploy_key`内容添加到GitHub仓库的Secrets中并命名为`BLOG_DEPLOY_KEY`

####   相关资料

[部署hugo到VPS](https://blog.lancitou.net/using-github-actions-to-deploy-hugo-blog-to-self-hosted-vps/)

