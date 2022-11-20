# macOS 降低node版本


<!--more-->

## 降低版本

目前安装的最新版本V18.10.0，以前的老项目运行报错，无法启动。

我的node 是用homebrew安装的，所以，网上使用n 切换 node 版本无效。

> 解决方案

* 卸载
  ```
  brew uninstall node@18
  ```
* 安装14版本
  ```
  brew install node@14
  ```
* link 
  如果不做这一步，node -v 会提示找不到node命令
```
brew link node@14
```
执行命令会报错
```
Error: Could not symlink bin/npm
Target /opt/homebrew/bin/npm
already exists. You may want to remove it:
  rm '/opt/homebrew/bin/npm'

To force the link and overwrite all conflicting files:
  brew link --overwrite node@14

To list all files that would be deleted:
  brew link --overwrite --dry-run node@14
```
解决方法就是 强制覆盖，照着命令提示操作，是不是很方便
```
  brew link --overwrite node@14
```

## npm ERR! node-sass@6.0.0 postinstall: `node scripts/build.js`

一个比较老的vue项目中，用到了 node-sass。在windows环境中运行正常。现在我在mac上做开发，这个项目有些问题需要解决，安装node依赖各种报错。尝试了网上说的各种办法，无果。

解决过程中，我想到mac上有一个vue项目是可以正常运行的，就看了一下package.json中node-sass 是什么版本，结果发现，这个项目引入的是  "sass": "^1.39.0"。

我抱着试一试的心态把前一个启动不了的项目中的node-sass替换成sass，依赖安装成功。
