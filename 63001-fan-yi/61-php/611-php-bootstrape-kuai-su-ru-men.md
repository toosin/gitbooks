# PHP bootstrape 快速入门

As your PHP applications become larger and more complex, managing dependencies via a list of includes becomes a chore, and it's also seen as bad practice today. In addition, many of the scripts only include business logic \(i.e. they don't render HTML, and shouldn't be accessed directly\). Once you've finished this crash course in bootstrapping, you'll have a basic -- yet powerful -- modern application template with simplified script dependency built in.

You can download the full source code for this tutorial [here](https://github.com/Binpress/php-bootstrapping-crash-course).

当你的php项目变得越来越大、越来越复杂的时候，通过文件包含列表来管理依赖就会变得异常的繁重，这种在老式的管理方式在现在看来已经是一种非常不好的实践。另外，大部分的脚步仅仅包含业务逻辑（i.e. 它们不渲染html，不被外部直接访问）。一旦你完成了这个快速课程你就能创建一个基础、有力、现代的应用模板来处理脚本之间的依赖。

## 什么是bootstrapping

Bootstrapping refers to the process of**loading the environment**a program \(or a script, in the case of PHP\) needs to operate. In the context of PHP development, it also means funneling all web requests through a single script that performs the bootstrapping process, also called "front controller."

In many PHP systems you might have encountered, such as WordPress or PHPMyAdmin, requests are handled ad-hoc, sometimes allowing direct access to scripts, sometimes funneling requests to an`index.php`file. Aside from security concerns \(for example, accessing`wp-config.php`while the server PHP process is down, showing it as plain text\), it also makes it hard to follow the include chain to understand what is loaded for each script. Not to mention how much of a pain it becomes to properly set up the environment for new scripts as they're added to the system. This is especially true if you're not the original developer of the app you're working with.

Bootstrapping是指加载php执行环境中所有需要的操作的一个程序。该程序运行在整个php的上下文中，这也意味着所有的web请求都通过这个脚本进行引导，也被称作为“前端控制器”。在大多数php系统中你可能已经遇到过，比如wordpress或者phpmyadmin所有的请求都是临时处理的，有时允许通过指令访问脚本、有时所有的请求都通过index.php文件。安全问题例如（访问服务器的PHP程序wp-config.php而下，显示为纯文本。）它也使得很难遵循包含链来理解每个脚本加载的内容。更不用说当新脚本添加到系统中时，如何正确地为新脚本设置环境。如果您不是正在开发的应用程序的原始开发人员，这一点尤其适用。

Bootstrapping alleviates those problems in the following fashion

1. **All requests are processed by a front controller**, usually`index.php`, which bootstraps the application, including all the relevant dependencies \(functions, classes, configuration\) and executing the appropriate script to return the response to the originating client.
2. All PHP files aside from the front controller are placed **outside of the publicly accessible folder **\(the document root on Apache\), so they can't be accessed regardless of the status of the server.
3. This setup allows the use of **routing in PHP **-- parsing the request URL to call the correct script\(s\) and returning the response to the client. This is how most PHP frameworks implement their MVC structure.

So, how do we successfully bootstrap our applications using real world best practices?

Bootstrapping通过以下后端解决这些问题

1. 所有请求的处理都通过一个前端控制器，这通常是index.php，当应用程序启动的时候，将所有相关的依赖包含进来（比如：functions,classes,configuration）并且执行适当的脚本返回给元素客户端
2. 除了前端控制器之外的所有PHP文件都放在公共可访问文件夹（Apache上的文档根目录）之外，因此无论服务器的状态如何都不能访问它们。
3. 此设置允许在PHP中使用路由——解析请求URL以调用正确的脚本并将响应返回给客户机。这就是大多数PHP框架如何实现它们的MVC结构。

so，我们如何使用bootstrap来构建我们的application?

## Bootstrapping in the real world

The first step is to start with a meaningful directory structure for your application, here's an example.

第一步是创建一个有意义的项目结构比如

```
/path/to/myapp/
  app/
    bootstrap.php
    lib/
    public/
      .htaccess
      index.php
  share/
  vendor/
```

All the code relative to the application itself is self-contained in the`app`directory. Inside this, the`app/public`

directory is the only directory that must be published to the web, so we can write the following in our virtual host file.

与应用程序本身相关的所有代码在应用程序目录中都是自包含的。在这里，应用app/public目录是必须发布到Web上的唯一目录，因此我们可以在虚拟主机文件中编写以下内容。

```
DocumentRoot /path/to/myapp/app/public
<Directory "/path/to/myapp/app/public">
  # other setting here
</Directory>
```

The`.htaccess`file then redirects all non-existing URLs to the front controller`index.php`:

下面的脚本是用户 重写index.php的

```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php [QSA,L]
```

```
<?php
// Including global autoloader
require_once dirname(__FILE__) . '/../vendor/autoload.php';

// Init config data
$config = array();

// Basic config for Slim Application
$config['app'] = array(
    'name' => 'My Awesome Webapp',
    'log.enabled' => true,
    'log.level' => Slim\Log::INFO,
    'log.writer' => new Slim\Extras\Log\DateTimeFileWriter(array(
        'path' => dirname(__FILE__) . '/../share/logs'
    )),
    'mode' => (!empty($_ENV['SLIM_MODE'])) ? $_ENV['SLIM_MODE']: 'production'
);

// Load config file
$configFile = dirname(__FILE__) . '/../share/config/default.php';

if (is_readable($configFile)) {
    require_once $configFile;
}

// Create application instance with config
$app = new Slim\Slim($config['app']);

// Get logger
$log = $app->getLog();

// Only invoked if mode is "production"
$app->configureMode('production', function () use ($app) {
    $app->config(array(
        'log.enable' => true,
        'log.level' => Slim\Log::WARN,
        'debug' => false
    ));
});

// Only invoked if mode is "development"
$app->configureMode('development', function () use ($app) {
    $app->config(array(
        'log.enable' => true,
        'log.level' => Slim\Log::DEBUG,
        'debug' => true
    ));
});
```

通过index.php加载bootstrape.php 然后bootstrap中处理处理自动加载，包含配置文件，执行路由等操作。



源地址：[https://www.binpress.com/tutorial/php-bootstrapping-crash-course/146](https://www.binpress.com/tutorial/php-bootstrapping-crash-course/146)

