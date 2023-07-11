# Nexus構築

## docker-compose.ymlの作成
```
version: '3'
services:
  nexus:
    image: 'sonatype/nexus3'
    container_name: nexus
    restart: always
    environment:
      - TZ=Asia/Shanghai
    ports:
      - '8931:8081'
```

## Nexus image download & startup
```
docker-compose up -d
```



## 初期パスワードの確認
```
docker exec -it nexus bash
cat /nexus-data/admin.password
```