### 1. 关闭https

打开elasticsearch.yml文件，它通常位于Elasticsearch安装目录的config文件夹下。

找到以下设置：
```
xpack.security.transport.ssl.enabled: true
xpack.security.http.ssl.enabled: true
```
将其修改为：
```
xpack.security.transport.ssl.enabled: false
xpack.security.http.ssl.enabled: false
```
注释掉或者移除与SSL相关的其他配置项，例如：
```
xpack.security.http.ssl.keystore.path: ...
xpack.security.http.ssl.truststore.path: ...
```
保存并关闭elasticsearch.yml文件。

重启Elasticsearch服务。

### 2. 关闭认证

打开elasticsearch.yml文件，它通常位于Elasticsearch安装目录的config文件夹下。

```
xpack.security.enabled: false
```