title: npm install 失败解决方案
date: 2016-01-19 23:07:22
tags: npm

---

使用npm install 时经常卡住很久，运气不好会报红字错误。原因在于npm服务器在美国，还有就是某强大的防火墙作用！网上整理了解决方案如下

* 设置代理
* 更换国内镜像

## 确认环境

* 确定一下npm config情况

``` bash
npm config ls
```

config内容

> registry npm默认源地址
> userconfig 用户配置文件所在地址
> proxy 代理服务器
> registry 用户配置的源地址

如果用户没有配置过上述的配置内容都不会显示，可以直接使用命令行工具命令修改配置，如下
``` bash
npm config set proxy http://127.0.0.1:8086 //设置代理地址 http://www.xxxx.com:port格式
npm config set registry http://registry.npmjs.org //设置npm源地址，当前显示的为美国默认的源地址
```
也可以在安装过程中临时修改npm源地址
``` bash
npm install express --registry=http://registry.npmjs.org //临时修改registry
```

## 配置方法

国内有几个镜像站点可以供我们使用，速度很快

* 通过 config 配置指向国内镜像源

国内镜像速度是快，不过我有强迫症必须要用最新的，还好npm官方站点并没有完全被墙，速度稍慢也能接受，重置一下源地址，一般重置一次就可以安装了，虽然速度慢，不过为了强迫症忍了。
``` bash
npm config set registry http://registry.npmjs.org //配置指向源，这是美国的官方地址
```
下面是国内的镜像源
``` bash
npm config set registry http://registry.cnpmjs.org //配置指向源
npm info express  //下载安装第三方包
```

* 通过 npm 命令指定下载源

``` bash
npm --registry http://registry.cnpmjs.org info express //注意源地址，自行修改
```

* 在配置文件 ~/.npmrc 文件写入源地址

``` bash
nano ~/.npmrc   //打开配置文件
registry =https://registry.npm.taobao.org   //写入配置文件，注意源地址，这是淘宝的源地址
```

推荐使用最后一种方法，一劳永逸，前面2钟方法都是临时改变包下载源。
如果你不像使用国内镜像站点，只需要将写入 ~/.npmrc 的配置内容删除即可。

## 总结

* 如果大家有稳定的代理服务器还是使用代理最好
* 没有代理条件就只能使用更换源地址的方式打游击了
* npm在国外不是很稳定，安装失败以后使用npm cache clean 清一下缓存，多安装试试。

> npm源地址集合的站点
> [http://blog.modulus.io/npm-mirrors](http://blog.modulus.io/npm-mirrors)


