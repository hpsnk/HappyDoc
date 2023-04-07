
## 查看防火墙状态：
```
systemctl status firewalld
```

## 开启防火墙：
```
systemctl start firewalld
```

## 关闭防火墙：（系统启动时会启动防火墙服务）
```
systemctl stop firewalld
```

## 禁用防火墙（系统启动时不启动防火墙服务）
```
systemctl disable firewalld
```
