# 通过SSH的方式克隆github中的仓库


<!--more-->

在macOS上我想克隆自己的github中的仓库，刚开始使用的是https的方式在本地执行clone命令，发现输入用户密码后，一直报一个403的错误。

换成SSH的方式就没有这个问题，但需要提前做一些配置，主要步骤如下：

## 步骤1

本地生成SSH key

```
ssh-keygen -t rsa -C 'youmail'

三次回车

查看复制key

cat ～/.ssh/id_isa.pub

从ssh-rsa字符开始复制，到邮箱地址字符结束

```
## 步骤2

在github中创建sshkey ，并将上面本地创建的key复制到github的配置中

![](/images/githubsshkey.png)
