
## Windows版

* zipファイル解凍

* httpdフォルダ配布

* configファイル修正

* Windows Service Install


# configファイル修正

conf/httpd.conf

Define SRVROOT "c:/Apache24"
⇒
Define SRVROOT "C:/Program Files/Apache24"

ServerName "aaa.bbb.ccc.ddd:80"


# Windows Service Install

```
httpd.exe -k install
httpd.exe -k install -n "MyServiceName"
```

```
httpd.exe -k uninstall
httpd.exe -k uninstall -n "MyServiceName"
```
---
# 参考ページ

https://httpd.apache.org/docs/2.4/platform/windows.html
