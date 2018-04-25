### **3.5. 快捷生成tp项目的配置文件**

##### 概述

> 在某些情况下，比如批量做活动的时候需要配置大量的配置文件，手工写入配置文件比较麻烦，特别是不断的切换目录去写入配置文件，
>
> 这个时候需要一些自动化脚本协助我们的工作。于是编写了此快捷脚本。基于php编写，原因在于php处理thinkphp的配置文件的返回数组的形式特别方便。

##### 代码表述

> 在批量配置的时候需要定义我们的模板文件和目标目录，为了方便已经定义基础的模板文件，一般的情况下无需修改，以source开头的变量即使代表模板源文件

```
$source_com_config_file_path  = "src/common_config.php";
$source_cron_config_file_path = "src/crontab_config.php";
$source_cron_index_file_path  = "src/crontab_demo.php";
$dist_com_config_Path         = "dist/"; // common下的文件操作
$dist_cron_file_Path          = "dist/Crontab/"; // crontab目标目录
```

> 定义了配置需要修改的配置项，注意**此config的value为tp配置中key名【重要】**

```
$config        = array(
    'crontab_log_dir' => "CRONTAB_LOG_DIR", // 此项配置请勿修改
    "db_name"         => 'DB_NAME',// session和data cache的前缀,token 公用，prefix前缀
    'appid'           => 'AppID',
    'appsecret'       => 'AppSecret',
    'app_url'         => 'app_url',
);
```

> 此处，请勿修改crontab\_log\_dir该项，该项用于构建crontab的配置文件，此外改脚本使用dbname作为token、session的name、session和data缓存数据的前缀,如果需要添加或者删除某项处理config即可，这里只需要变动可变的配置，不可变的配置可加入到模板源文件中。

##### Example

> 添加一个自动配置AppName的配置项，修改$config

```
$config        = array(
    'crontab_log_dir' => "CRONTAB_LOG_DIR", // 此项配置请勿修改
    "db_name"         => 'DB_NAME',// session和data cache的前缀,token 公用，prefix前缀
    'appid'           => 'AppID',
    'appsecret'       => 'AppSecret',
    'app_url'         => 'app_url',
    "app_name"        => "AppName"
);
```

> 后面就可以交给程序自动完成。

##### Example

```
php php_config.php -v
```

![](/assets/tp_config_1.png)

执行完毕后会在相应的目录创建相应的文件。over

###### 代码下载

> php版本：[php\_config.zip](/assets/thinkphp_config.zip)



