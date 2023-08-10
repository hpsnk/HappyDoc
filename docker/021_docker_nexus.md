# Nexus構築

* docker-compose.ymlの作成
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
      volumes:
        - ./nexus-data:/nexus-data
  volumes:
    nexus-data:
  ```

## docker関連コマンド
* 起動
  ```
  docker-compose up -d
  ```

* 停止
  ```
  docker-compose stop
  ```

* 終了
  ```
  docker-compose down -v
  ```


## Nexus関連メモ
* アクセス
  ```
  http://localhost:8931/
  ```

* adminユーザの初期パスワード
  ```
  docker exec -it nexus bash
  cat /nexus-data/admin.password
  ```


* proxy経由
  ```
  proxyの裏にnexusサーバーを立てた。

  HTTP proxyの設定は Administration > System > HTTP の HTTP proxy のチェックボックスをチェックすると、設定が入れれるようになる。
  ```

## 利用する際にリポジトリ関連

* pythonリポジトリ
  ```
  pip install [package] -i http://[ip address]:[port]/repository/[リポジトリ]/simple --trusted-host [ip address]
  ```
