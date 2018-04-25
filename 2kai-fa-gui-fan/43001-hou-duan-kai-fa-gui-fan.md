# 后端开发

开发规范：主要遵循[https://www.kancloud.cn/thinkphp/php-fig-psr/3139](#)

后端开发目前主要使用PHP，框架使用thinkphp，我们主要针对微信进行开发，所有封装了很多针对微信的操作。以下是整个概述。

### 0、服务构架

> 我们目前采用的是openresty+memcache+mysql+linux
>
> linux：使用的是centos6.8或者centos7以上
>
> memcache：可以是本地服务，另外还有一台专门的memcache服务器
>
> openresty：nginx中的一种，把nginx常用的模块集成在一起
>
> mysql：我们使用的是5.6

> **git：我们使用的是oschina的码云，需要预先注册账号**
>
> 所有需要了解以上服务的基本命令
>
> 本地开发一般使用的是集成环境，自由选择即可

目录结构：

项目名

--- Application       ==&gt; 模块目录

```
 ---Admin           ==&gt;后台模块

 ---Common      ===&gt;公共模块

 ---Crontab        ===&gt;定时任务模块

     ---Conf

        ----项目配置文件

     ---Controller

         --- TaskController.class.php   ==&gt;任务控制器，此控制器中包含，access\_token的获取

        ---- CronController.class.php   ==&gt; 用于处理项目数据库中配置的定时任务

     ---入口文件.php

 ---其他业务模块
```

---avatar                ==&gt;后台入口

---Public                ==&gt; 前台入口

---ThinkPHP          ==&gt;tp核心库

#### 服务器目录

1、发布脚本

> 我们一般会在oschina git上创建项目，创建项目的时候请把欧总，codebean加入到项目中，否则服务器无法pull code，
>
> 发布脚本我们命名为git\__update\_xxx.sh，脚本的目录位于/opt/bin/下_
>
> 发布脚本的功能，是从git下拉代码到/data/git/目录中，然后同步代码到/data/www/目录中

2、/data目录下的文件概述

> git：用于存放oschina git下拉的代码
>
> logs：nginx的日志目录
>
> app：openresty和php的位置
>
> www：项目的目录

### 1、Module的加载

我们通过配置文件去加载对应的Module中对应的方法，在index.php中你可以看到这样的一段代码

```
// NGINX环境变量TP_ENV , 定义APP_STATUS应用状态
if (isset($_SERVER['TP_ENV'])) {
    $tp_env = strtolower(trim($_SERVER['TP_ENV']));
    define('APP_STATUS', $tp_env);
}
```

此功能是实现配置加载Module的关键代码，此功能需要与服务器相互配合使用

#### Example

**nginx**：我们设置fastcgi的参数

```
location ~ \.php$ {
fastcgi_pass   127.0.0.1:9000;
fastcgi_index  index.php;        
include        fastcgi_params;
fastcgi_param  TP_ENV  justtest_online;
}
```

这里加载的是justtest\_online.php的文件

**Apache**：我们直接设置apache的环境变量即可，一般是在本地开发

```
<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host2.example.com
    DocumentRoot "C:/xampp/htdocs/acome/avatar"
    ServerName www.duoma-article.com
    SetEnv TP_ENV duoma_local
    CustomLog "logs/dummy-host2.example.com-access.log" common
</VirtualHost>
```

这里会加载一个duoma\_local.php的配置文件

如有疑问请查阅tp官方手册

### 2、服务器定时任务

我们通过Crontab进行定时任务，一般的需求点在于，定时获取access\_token

当进行开发的时候涉及的微信的时候可能需要配置crontab。需要了解crontab的基本配置的含义

在配置的时候需要建立一个crontab的入口文件，一般在Crontab模块中的demo.php同级，建立一个针对当前项目的Crontab的配置文件。

配置定时任务的时候，可能需要在/tmp/目录下手动创建一个存放当前项目的定时任务日志的文件夹，crontab配置参考如下

```
*/20 * * * * /usr/local/bin/php /data/www/acome/Application/Crontab/laozhao1.php task/gettoken >> /tmp/laozhao1/cron.log 2>&1
```

具体参考demo.php

### 3、活动的发布

> 活动发布的规范
>
> 活动发布中可能需要域名，我们公司的域名在aliyun和dnspod中
>
> nginx配置：cd /data/app/openresty/nginx/conf/vhost/中
>
> 以上配置目前可能需要手动创建，不久后将会开发自动脚本自动构建。



