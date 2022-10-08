# GIT QA


<!--more-->

GIT是我目前最常用的版本管理工具，使用过程中还是会遇到一些小问题，这些问题都是因为配置不当等导致。记录下来，释放大脑内存。

* fatal: unable to access 'xxx': OpenSSL SSL_read: Connection was reset, errno 10054

> SOL: git config --global http.sslVerify "false"
