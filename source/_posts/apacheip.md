title: Apache禁止域名访问自己的服务器
date: 2018-06-21 13:49:58
tags: [Apache, PHP]
---
用Apache搭建的WEB服务器，让网友只能通过设定的域名访问，而不能直接通过服务器的IP地址访问，修改`/etc/httpd/conf/httpd.conf`文件。  
##### 方法一  
在httpd.conf文件最后面，加入以下代码
```
# 下面代码是实现拒绝直接通过221.*.*.*这个IP的任何访问请求，这时如果你用221.*.*.*访问，会提示拒绝访问。
NameVirtualHost 221.*.*.*
<VirtualHost 221.*.*.*>
    ServerName 221.*.*.*
    <Location />
        Order Allow,Deny
        Deny from all
    </Location>
</VirtualHost>
# 下面代码就是允许通过www.linuxidc.com这个域名访问，主目录指向/var/www/html（这里假设你的网站的根目录是/var/www/html）
<VirtualHost 221.*.*.*>
    DocumentRoot "/var/www/html"
    ServerName www.flygrub.github.io.com
</VirtualHost>
```
##### 方法二  
在httpd.conf文件最后面，加入以下代码
```
# 下面代码是把通过221.*.*.*这个IP直接访问的请求指向/var/www/error目录下，这可以是个空目录，也可以在里面建一个首页文件，如index.hmtl，首面文件内容可以是一个声明，说明不能通过IP直接访问。
NameVirtualHost 221.*.*.*
<VirtualHost 221.*.*.*>
    DocumentRoot "/var/www/error"
    ServerName 221.*.*.*
</VirtualHost>
# 下面代码就是允许通过www.linuxidc.com这个域名访问，主目录指向/var/www/html（这里假设你的网站的根目录是/var/www/html）
<VirtualHost 221.*.*.*>
    DocumentRoot "/var/www/html"
    ServerName www.flygrub.github.io.com
</VirtualHost>
```
> 保存httpd.conf之后，需要重启Apache
```
service httpd restart
```