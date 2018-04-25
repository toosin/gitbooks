在开发中我们需要一些工具辅助我们进行开发，这些工具能够很好的帮助的我们

比如在开发swoole的时候，一般的编辑器并没有相应的提示。需要借助外部工具，在官方文档中提到了一个工具，利用php的反射

[https://github.com/flyhope/php-reflection-code](https://github.com/flyhope/php-reflection-code) 进行自动提示



使用步骤：clone github上的代码，获取其中的php-reflection-code,将该文件夹放入我们开发的项目中，一般我会在TP的核心目录下的Library/Com/中创建一个Plugs文件夹，将项目代码放入其中。

该反射代码还支持：

* Memcached

* Redis
* Swoole
* Yaf
* Yar
* Yac
* Yaconf



