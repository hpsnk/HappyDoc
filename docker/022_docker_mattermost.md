## 参考
* [公式サイトの手順](https://docs.mattermost.com/install/install-docker.html)
* [ubuntuにmattermostを構築](https://dev-daikichi.hatenablog.com/entry/2018/12/28/165531)

* その他
  * https://syachiku.net/mattermostslackdocker/
  * https://qiita.com/nanbuwks/items/b20e2df483f6806909ab
  * https://qiita.com/Hikery/items/3cc0789de9eba7eacafb

---

## volume权限
* https://blog.csdn.net/Castlehe/article/details/124704393

* http://www.qince.net/dockerqlq.html
  ```
  docker run -it -v {本地文件夹}:{容器文件夹} -e HOST_PERM={权限设置} {容器名称} 

  docker run -it -v C:\Users\user\app:/app -e HOST_PERM="0777" myapp:latest
  ```

* https://www.coder.work/article/973231