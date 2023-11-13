
## 用途

## 设定内容

* 对象文件 : httpd.conf
    ```
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass /wiki http://aaa.bbb.ccc.ddd:8088/JSPWiki/
        ProxyPassReverse /wiki http://aaa.bbb.ccc.ddd:8088/JSPWiki/
        
        ProxyPass /JSPWiki http://aaa.bbb.ccc.ddd:8088/JSPWiki/
        ProxyPassReverse /JSPWiki http://aaa.bbb.ccc.ddd:8088/JSPWiki/
    </VirtualHost>

    ```

# 内容说明

在 Apache HTTP Server 的主配置文件 `httpd.conf` 中，您可以使用 `VirtualHost` 和 `ProxyPass` 指令来配置反向代理（Reverse Proxy），将请求转发给其他服务器或后端应用程序。以下是如何在 `httpd.conf` 中设置 `VirtualHost` 并使用 `ProxyPass` 的示例：

1. 打开 `httpd.conf` 文件并找到 `<VirtualHost>` 部分，例如：

    ```apache
    <VirtualHost *:80>
        ServerName example.com
        DocumentRoot /var/www/html
    </VirtualHost>
    ```

2. 在该 `VirtualHost` 部分内添加 `ProxyPass` 指令来设置反向代理。例如，假设您要将 `/api` 路径下的请求转发到目标服务器上的 `http://localhost:3000`：

    ```apache
    <VirtualHost *:80>
        ServerName example.com
        DocumentRoot /var/www/html

        ProxyPass /api http://localhost:3000
        ProxyPassReverse /api http://localhost:3000
    </VirtualHost>
    ```

    在上述示例中，`ProxyPass` 指令将 `/api` 路径下的请求转发到 `http://localhost:3000`。`ProxyPassReverse` 指令用于修改响应标头，以便正确返回给客户端。

3. 保存并关闭 `httpd.conf` 文件。

4. 重新启动 Apache HTTP 服务器，使配置生效。

通过使用上述配置，当客户端发起对 `example.com/api` 的请求时，Apache 会将请求转发到 `http://localhost:3000/api`。这样您可以在应用中设置反向代理，将特定 URL 路径下的请求传递到其他服务器或后端应用程序。请确保目标服务器或应用程序已经正确配置和运行。

---

* ProxyPreserveHost On
    
    ProxyPreserveHost 指令用于在反向代理过程中保留原始请求的 Host 头信息。当设置为 "On" 时，它将确保后端服务器接收到的请求头中的 Host 信息与原始请求的 Host 相对应。如果不启用 ProxyPreserveHost，则后端服务器可能会将请求视为来自代理服务器，而不是原始客户端。

    如果启用了 ProxyPreserveHost，则后端服务器将能够识别请求的原始主机名，从而提供正确的内容。
