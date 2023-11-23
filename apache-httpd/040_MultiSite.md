# 使用 `VirtualHost` 托段多个站点

* `site1` : 占用`8801` 端口

* `site2` : 占用`8802` 端口

* `httpd.conf` 设置的例

    ```apache
    Listen 8801
    <VirtualHost *:8801>
        ServerName   site1
        ServerAlias  site1
        DocumentRoot "D:/site1"
        <Directory "D:/site1">
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>
        ErrorLog "logs/site1.error.log"
        CustomLog "logs/site1.access.log" common
    </VirtualHost>

    Listen 8802
    <VirtualHost *:8802>
        ServerName   site2
        ServerAlias  site2
        DocumentRoot "D:/site2"
        <Directory "D:/site2">
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>
        ErrorLog "logs/site2.error.log"
        CustomLog "logs/site2.access.log" common
    </VirtualHost>
    ```

