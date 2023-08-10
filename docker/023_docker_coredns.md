## docker-compose.yml
```
version: '3.7'
services:

  coredns:
    image: coredns/coredns:1.10.0
    container_name: coredns
    ports:
      - 53:53/udp
    volumes:
      - ./coredns/hostsfile:/etc/coredns/hostsfile
      - ./coredns/Corefile:/Corefile
```
## Corefile 文件内容如下：
```
.:53 {
    hosts {
        192.168.1.11 test.com
        192.168.1.12 test1.com
        fallthrough
    }
    forward .8.8.8.8:53 114.114.114.114:53
    log
}
```
其中 forward 指向上级 dns 服务

## 我们还可以将 hosts 独立出来为一个单独的文件，如下所示：
```
.:53 {
    hosts /etc/coredns/hostsfile {
        fallthrough
    }
    forward . 8.8.8.8:53 114.114.114.114:53
    log
}
```

## hostsfile 文件内容如下：
```
192.168.1.11 test.com
192.168.1.12 test1.com
```