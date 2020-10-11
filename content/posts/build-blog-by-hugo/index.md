---
title: "利用 Github Actions 和 Hugo 搭建自动化构建的静态博客"
date: 2020-10-11T16:05:21+08:00
draft: false
image: "build-blog-by-hugo-logo.png"
---

## 前言

既然刚折腾完 Hugo，蹭着记忆还清晰，梳理一下搭建过程吧

## 工具和网站介绍

- [Hugo](https://gohugo.io):

    号称是世界上最快静态网页构建框架(framework)

    由 Go 编写而成，用于生成我们博客所需的静态页面

- [Git](https://git-scm.com/):
  
    Git 是一个分散式版本控制软件

- [Github](https://github.com):
  
    ~~最大的同性交友网站~~
    GitHub是通过Git进行版本控制的软件源代码托管服务平台

    本教程利用的是 Github 提供的名为 Github Pages 的免费网站托管服务


## 教程正文
---

### Step 0: 安装 Git 和 注册 Github 

- 安装 Git:
        
    打开 [Git](https://git-scm.com/downloads) 页面，按照你的操作系统，下载安装程序，并安装

- 注册 Github:
  
    打开 [Github](https://github.com/) 填写屏幕右侧的表单，跟随步骤进行即可

### Step 1: 安装 Hugo

- Windows 用户:

    - 下载:
        
        前往[Releases](https://github.com/gohugoio/hugo/releases)页面，查找名称类似于 **hugo_0.76.3_Windows-64bit.zip** 的压缩包，下载解压即可

        请自行确定使用 x64/x86 版本

    

    - 配置环境变量:

        右键开始菜单按钮

        选择系统

        在新打开的页面左侧中选择高级系统设置

        点击环境变量按钮

        在用户变量部分， 找到名为 PATH 的部分

        在 PATH 上双击

        点击添加按钮

        在弹出的窗口中选择含有 hugo.exe 文件的文件夹

        点击完成即可

- Macos 用户:

    - 如果你安装了 Brew：

        >brew install hugo
    
        在你喜欢的 Terminal 中执行上述命令即可

    - 如果你未安装 Brew:

        前往[Releases](https://github.com/gohugoio/hugo/releases)页面，查找名称类似于 **hugo_0.76.3_macOS-64bit.tar.gz** 的压缩包并下载

        解压即可

### Step 2: 利用 Hugo 生成你的网站

> 如果你进行此步骤遇到麻烦，请确认上一步骤是否正确进行

- 打开 Terminal 

    - Windows:

        按下 win + x 键，选取 windows powershell

    - Macos:

        根据喜好使用即可，笔者这里用的是 iTerm

- 生成新网站

    > {var} 这样的标记，表示将此处替换成内部描述的字符，注意去掉{}

    在 Terminal 下执行

            >hugo new site {此处输入你要生成的网站目录名}

    不出意外, 目录中已出现你指定名称的文件夹

- 选取 Blog 主题

  - 下载主题

    可以在这个网站[Themes](https://themes.gohugo.io/)或通过搜索引擎下载你喜欢的主题

  - 安装主题

      下载后，主题应为一个压缩包，解压后放至网站目录内的 themes 文件夹内即可

  - 配置主题

      鉴于不同主题配置方法不一，请参阅你选择的主题提供的文档

- 开始创作

    在 Terminal 内执行

        >hugo new post/{文章名}.md

    来创建文章文件

    >注意：Hugo所使用的是名为 Markdown 的轻量型标记语言来撰写文章，如果你未接触过，这里提供一篇由少数派发布的[入门教程](https://sspai.com/post/25137)

    用你喜欢的编辑器打开该 md 文件，进行你的创作

    你应该已经注意到，md 文件开头有 以---分割的一段信息(Meta)

    这段信息至少包含了三个部分

    - title

    - date

    - draft

    后面跟随的值分别代表了

    - 文章标题
    - 生成日期
    - 是否为草稿

    你可以自由修改前两部分

    如果你想让这篇文章显示在你的网站上的话，请将 

        draft: true

    改为

        draft: false

    >这个表示表明了这篇文章是否为草稿

    创作完成后，保存文件即可

### 部署

  - 在本地预览你的博客

      在 Terminal 中执行

          >hugo server
      
      这样，hugo 会在本地搭建一个静态服务器来让你能够看到当前博客的样子
  
  - 发布博客至 Github Pages

      仅让自己看到博客，不够尽兴，让大家都能一览你的作品吧

      - 创建 Repo: 

          要发布你的博客，首先需要在 Github 上创建一个库(repo)

          打开你的 Github 主页, 选择 New

          ![](/imgs/build-blog-by-hugo/blog-1.png)

          在新页面中，填入必要信息

          ![](/imgs/build-blog-by-hugo/blog-2.png)

          - Repositor name: 库的名称，此处建议使用 {github 用户名}.github.io 作为库名

          - Description: 用一段简短的话描述这个库，可省略

          - Public: 为了部署我们的网站，这个库的类型应该是公开的

          - Initialize this repository with: 此处的选项不在介绍，建议不勾选

          点击绿色的 Create repository 来创建你的库

      - 初始化本地 Git 库并 push 至 github

          回到 Terminal, 要想将网站发布，需要在本地创建 Git 库

          在网站根目录下依次执行

              >git init
              >git add .
              >git commit -m "first commit"
              >git remote add origin https://github.com/{你的 github 用户名}/{你的 github 用户名}.github.io.git
              >git push -u origin master

          这样，你的网站已经被上传到了 Github 上
      
      - 配置 Github Actions

          hugo 的页面是需要静态生成后才能直接查看的，下面我们来配置 Github Actions

          在本帖网站根目录创建文件夹.github/workflows

          新建文件deploy.yml

          向文件中写入

          ```yml
          # This is a basic workflow to help you get started with Actions

          name: deploy-hugo

          # Controls when the action will run. Triggers the workflow on push or pull request
          # events but only for the master branch
          on:
            push:
              branches: [ master ]
            pull_request:
              branches: [ master ]

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

                - name: Setup Hugo # Step name
                  uses: peaceiris/actions-hugo@v2 # Step 使用的 actions
                  with: # 參數設定
                    hugo-version: '0.75.1'
                    extended: true

                - name: Build
                  run: hugo --gc --minify --cleanDestinationDir

                - name: Deploy
                  uses: peaceiris/actions-gh-pages@v3
                  with:
                    github_token: ${{ secrets.GITHUB_TOKEN }}
                    publish_dir: ./public

          ```

          保存文件, 在 Terminal 中输入

              >git add .
              >git commit -m "init Actions"
              >git push -u origin master

          此时，Github Actions 会帮你生成文件后发布到 gh-pages 分支下

      - 配置 Github Pages

          打开 Repo Setting 页面 https://github.com/{你的 github 用户名}/{你的 github 用户名}.github.io/settings

          ![](/imgs/build-blog-by-hugo/blog-3.png)

          找到图示部分，将原有的Branch: master 切换为 gh-pages 点击右侧 Save 保存即可

  - 大功告成

      打开 https://{你的 github 用户名}.github.io 来查看你的网站吧


### 后续创作

- 直接在 Github 上在线编辑

    打开你的 Repo 页面

    ![](/imgs/build-blog-by-hugo/blog-4.png)

    点击 Add file 选择 Create new file

    在打开的页面中

    ![](/imgs/build-blog-by-hugo/blog-5.png)

    向 Name your file 中输入 content/post/{文章名字}.md

    在下面的编辑框中创作，别忘了加上 Meta 信息

    创作完成后在页脚点击绿色的 commit new file 即可

- 在本地编辑器中创作

    回顾上文创作过程，创作完成后在 Terminal 中执行

        >git add .
        >git commit -m "{此处输入提示信息}"
        >git push -u origin master

    即可

## 感谢

恭喜你学会了搭建博客的方法，其中夹杂了 CLI, Git, Markdown 等知识，如果你对这些感兴趣，可以自行了解

感谢你阅读我的文章!


  
      