# Linux下安装phpMyAdmin

### 1.下载phpMyAdmin

```
sudo wget  https://files.phpmyadmin.net/phpMyAdmin/4.7.4/phpMyAdmin-4.7.4-all-languages.zip
```

### 2.配置

解压并改名

```shell
 sudo unzip phpMyAdmin-4.7.4-all-languages.zip 
 sudo mv phpMyAdmin-4.7.4-all-languages phpMyAdmin
```

找到 `phpMyAdmin/libraries/config.default.php`文件，将`config.default.php`复制到`phpmyadmin`目录下，然后更名为`config.inc.php`

```
sudo cp  libraries/config.default.php ./config.inc.php
```

**对config.inc.php文件进行vi编辑：**

1. 查找 `$cfg['PmaAbsoluteUri']` 修改为你将上传到空间的phpMyAdmin的网址：

   如：$cfg['PmaAbsoluteUri'] = '[http://192.168.11.115/phpMyAdmin/](http://192.168.1.11/phpMyAdmin/)';

2. 查找 `$cfg['Servers'][$i]['host'] = 'localhost';`（通常用默认，也有例外，可以不用修改）

3. 查找` $cfg['Servers'][$i]['auth_type'] = 'config';`

   　　在自己的机子里调试用config；如果在网络上的空间用cookie，这里我们既然在前面已经添加了网址，就修改成cookie ，这里建议使用cookie。

4. 查找 `$cfg['Servers'][$i]['user'] = 'root';`   MySQL user（mysql用户名，自己机里用root；）

5. 查找` $cfg['Servers'][$i]['password'] = '';`   MySQL password (mysql用户的密码,自己的服务器一般都是mysql用户root的密码)

6. 查找 `$cfg['Servers'][$i]['only_db'] = '';`   If set to a db-name, only（你只有一个数据就设置一下；如果你在本机或想架设服务器，那么建议留空

7. 查找` $cfg['DefaultLang'] = 'zh';` （这里是选择语言，zh代表简体中文的意思,这里不知道填gbk对否）

8. 设置完毕后保存退出即可。

