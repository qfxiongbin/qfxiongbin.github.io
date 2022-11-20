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
