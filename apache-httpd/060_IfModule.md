
## 用途

## 设定内容

### 对象文件 : httpd.conf

```apache

<IfModule alias_module>
    Alias /pad "D:/workspace_git_padmst/PADDashFormation/"
</IfModule>
```

```apache
<Directory "D:/workspace_git_padmst/PADDashFormation/">
    Options None
    AllowOverride None
    Require all granted
</Directory>
```
