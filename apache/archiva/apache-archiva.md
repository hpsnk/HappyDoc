# 安装
  * Standalone版
    ```
    cd/d apache-archiva\bin
    archiva install
    archiva start
    ```
  * WAR版

## 避坑指南
  * 使用jdk 1.6~1.8，不需要额外导入第三方jar包

## 配置
  * 修改服务的端口号
  
    conf/jetty.xml
    ```
    <Set name="port"><SystemProperty name="jetty.port" default="8080"/></Set>
    ↓
    <Set name="port"><SystemProperty name="jetty.port" default="8082"/></Set>
    ```
  * 修改JDK路径

    conf/wrapper.conf

    ```
    wrapper.java.command=java
    ↓
    wrapper.java.command=C:/jdk/jdk1.8.0_121/bin/java
    ```

# 初回使用

### Repository作成

| 項目            | バリュー  |
| -------         | --------      |
| Id              | maven2
| Name            | Maven2 Mirrow
| Directory       | e:/repository/apache-archiva
| Index Directory | e:/repository/apache-archiva    
| Type            | Maven 2.x Repository
| Cron Expression | 0 0 * * * ?
| Days Older      | 30
| Retention Count | 2

# 工程设置

* gradle配置

  build.gradle
  ```
  repositories {
    jcenter()
  }
  
  ↓
  
  repositories {
    maven {
      url 'http://192.168.1.1:8082/repository/internal/'
    }
  }
  ```
 

