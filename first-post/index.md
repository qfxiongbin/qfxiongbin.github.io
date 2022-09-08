# 主题文档 - 第一次使用Hugo搭建个人博客


<!-- {{< version 0.2.0 >}} -->

搭建个人博客的初衷是想把以前零散的输出整理到一起，另外一个重要的原因是有很多想学、想做的事儿，当我开始做这些事儿后，过程中需要沉淀，无论是问题也好，成果也好，记录下来。
我也有其他写作平台的账号，为什么还要自己搭建一个？

很重要的一个原因是这块地方仅仅属于我自己，一切我说了算。本文对搭建过程做一下简单记录，对于一般的使用做一下说明。

<!--more-->

## 安装Hugo

刚开始以为hugo需要通过服务器环境安装，我尝试用自己的阿里云ECS服务器安装，找的教程中都是说要先安装Homebrew,于是，我就被带到了坑里面，费了很大劲安装Homebrew,安装时遇到问题，需要升级服务器的其他软件，升级完了又执行安装Homebrew脚本，又识别不了新版本，于是，本次的行动卒。

打开Hugo官方github账号后发现，有windows环境的安装包，欣喜过望。开始第二次行动。

> 这是官方仓库：<https://github.com/gohugoio/hugo/releases>,我的电脑系统环境是 windows 11家庭中文版 64位操作系统，下载了hugo_extended_0.102.3_Windows-64bit.zip 安装包。

解压后，如下图所示：

![](/images/hugo001.png)

配置环境变量：

![](/images/hugo002.png)

查看hugo版本

```
hugo version

```
![](/images/hugo003.png)

到此为止，hugo就在windows上面搭建好了

## 项目创建

```
$ hugo new site /path/to/site

```

执行完命令后，生成项目结构如下：

![](/images/hugo004.png)

至此，通过hugo脚手架创建好了博客的初始项目。接下来我们需要挑选自己喜欢的主题，做相关的设置，部署运行啦。

## 主题选择

<https://hugothemesfree.com/> 站点中提供了大量主题供我们选择。我选的是[LoveIt](https://github.com/dillonzq/LoveIt.git)这个主题。

选好主题后，进入主题github仓库，克隆到上面搭建好项目的themes目录中。

```
cd themes
git clone https://github.com/xxx.git

```

主题clone到本地后，可以在主题目录中的exampleSite下找到 config.toml这个配置文件，拷贝替换项目根目录下的config.toml（非首次换皮肤时，记得备份，不过用git做版本管理，就不需要啦，历史版本都能找回）。

这个配置文件是站点的设置文件，示例站点中的配置很全面了，注释也很清晰。对于首次使用者而言，需要配置以下内容：

* 站点的header配置

包括站点名称、菜单栏配置等。

* 个人头像配置

* 社交账号

* 搜索

* 文章页的共通设置

* 站点的footer配置

设置项目很多，第一次不必一下子配置好，先试着创建自己的第一篇博文，设置项可以后面慢慢研究。第一篇博文的内容可以参考exampleSite目录content中的示例。配置项很全，看一眼应该知道每个项目干啥用的。
## github action + page 部署

需要在自己的github中创建两个仓库，一个用来管理站点源码，另外一个用来管理构建后的静态站点。

创建仓库过程略过了。要特别说明的一点是，部署静态站点的仓库名称命名是**your githubname.github.io** 这样的格式。举个例子，我自己的名称是qfxiongbin.github.io。


源码仓库中，需要设置action 脚本，每次本地推送时，会触发 action的 workflow,对增量内容进行构建，构建成功后，推送到your githubname.github.io仓库中，这个仓库中接收到推送内容后会触发gitpage action,大概过个几十秒，就会部署好，刷新页面就能看到最新的内容。

## Github action 配置

```
# This is a basic workflow to help you get started with Actions

name: CI

# Controls when the action will run.
on:
  # Triggers the workflow on push or pull request events but only for the master branch
  push:
    branches: [main]
  pull_request:
    branches: [main]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

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
        with:
          submodules: false # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0 # Fetch all history for .GitInfo and .Lastmod

      - name: Hugo setup
        # You may pin to the exact commit or the version.
        # uses: peaceiris/actions-hugo@2e89aa66d0093e4cd14751b3028fc1a179452c2e
        uses: peaceiris/actions-hugo@v2.4.13
        with:
          # The Hugo version to download (if necessary) and use. Example: 0.58.2
          hugo-version: latest # optional, default is latest
          # Download (if necessary) and use Hugo extended version. Example: true
          extended: false # optional, default is false

      - name: Build
        run: hugo

      - name: Pushes to another repository
        uses: cpina/github-action-push-to-another-repository@main
        env:
          API_TOKEN_GITHUB: XXXXXX
        with:
          source-directory: "public"
          destination-github-username: "qfxiongbin"
          destination-repository-name: "qfxiongbin.github.io"
          user-email: qfxiongbin@163.com

```

### 脚本说明

  * 上面脚本中，最后三行需要配置成自己的，**destination-github-username** 、 **destination-repository-name(这个需要配置构建后的静态站点仓库名)**、**user-email**。

  * API_TOKEN_GITHUB ：https://github.com/settings/profile，Settings/Developer settings 下Personal access tokens下创建。



## 参考资料
https://ibrights.github.io/post/blog20210527/

