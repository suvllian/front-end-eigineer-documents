# Linux系统下Apache服务器的配置

## 一、设置404页面
* 修改httpd文件，设置AllowOverride All
* 在服务器配置的网站根目录下添加.htacess文件，添加内容ErrorDocument 404 /404.html(404.html为编辑好的404页面)
* 重启服务器即可

## 二、设置403Forbidden禁止访问目录，关闭目录浏览权限
有些情况下，如果没有对某些目录设置403权限，用户可直接根据路径查看到当前文件夹下所有文件，这对于网站安全来说是不利的，所以需要设置403，让用户不能直接读取文件夹下所有文件。

* 在文件.htaccess中添加：ErrorDocument 403 /403.html
* 修改httpd文件

将httpd文件中的：
``` 
<Directory />
    Options Indexes FollowSymLinks #主要是这句中的Indexs
    AllowOverride None
    Order deny,allow
    Deny from all
</Directory>
```
修改为：
```
<Directory />
    Options FollowSymLinks
    AllowOverride All
    Order deny,allow
    Allow from all
</Directory>
```
下面还有一处也是Options Indexes FollowSymLinks
改为 Options FollowSymLinks

* 重启服务器