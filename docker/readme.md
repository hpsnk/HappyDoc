### Stop a docker container which started with --restart=always

```
docker update --restart=no <container_id>
```

### 状态一览
```
docker ps
```

### 进入环境
```
docker exec -it containers_db_1 bash
```

### copy文件
```
docker cp test.sh containers_db_1:/
```
