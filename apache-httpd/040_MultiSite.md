
## 用途

## 设定内容

### 对象文件 : httpd.conf

1. 设置监听端口
    ```apache
    #Listen 12.34.56.78:80
    Listen 80
    ```
    ↓
    ```apache
    #Listen 12.34.56.78:80
    Listen 80
    Listen 8901    # site1
    Listen 8902    # site2
    ```

2. 不同站点，`Directory` 和 `VirtualHost` 的导入
    ```apache

    ```

