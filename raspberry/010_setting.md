# xxx

* wifi，IP
* SSH
* FTP

## SSH

* 初期目录

    TF卡根目录，创建 'SSH' 文件，重启

* GUI开启SSH
    ```
    sudo raspi-config
    5. Interfacing Options -> SSH
    ```

* 命令行开启ssh
    ```
    sudo systemctl enable ssh
    sudo systemctl start ssh
    ```

## FTP
* 安装FTP服务
    ```
    sudo apt-get install vsftpd
    ```

* 修改配置
    ```
    /etc/vsftpd.conf

    anonymous_enable=NO
    local_enable=YES
    write_enable=YES
    local_umask=022
    ```

* 重启FTP服务
    ```
    sudo service vsftpd restart
    ```

* 开放root的FTP权限
    ```
    nano /etc/ftpusers

    root前加#

    按 Ctrl + X, 保存退出nano

    重启FTP服务
    ```


 * 参考
   - https://blog.csdn.net/xiejie0226/article/details/103309741
   - https://www.ihtmlcss.com/archives/612.html
   - https://www.bilibili.com/read/cv10720598/
