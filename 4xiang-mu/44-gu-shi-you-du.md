### 故事有毒小程序

概述：故事有毒是在2016年就开始了的，最开始我们做的事h5上的有毒故事，后面看见别人把这个活动坐在了小程序上。所有我们也做一个类似工具类的小程序

#### 后端

后端为了方便对有毒故事进行扩展，对tp原来的方式进行了扩展，添加某个故事只需要添加相应的文件即可。

代码结构：

```
Youdu
--- Controller
    --- IndexController.class.php
    --- EmptyController.class.php
    --- BaseController.class.php
    --- UserController.class.php
--- Module
    --- Female --->dir
        --- 1.php
        --- 2.php
        --- 3.php --->一个php文件对应一个故事的合理，没有使用面向对象
        ...
    --- Male --->dir
        --- 1.php
        ....

    --- IndexModel.class.php
    --- UserModel.class.php
```

#### 前端接口

合成图像接口

> 接口地址：/index/build
>
> 参数：name=用户的名称，sex=用户的性别（male和female）
>
> 请求方式：post
>
> 返回格式如下：

```
{
    "status": 200,
    "msg": "",
    "data": {
        "img": "http://www.youdu.com/static/youdu/build/5ab072081c1a8.jpg"
    }
}
```

用户进入后期用户的openid

> 接口地址：/user/openid
>
> 参数：code=wx.login\(\)返回的code，fid：用户在生产结果页面分享出去的自己的id.
>
> 请求方式：post
>
> 返回格式：

```
{
    "status": 200, 
    "msg": "",
    "data": {
        "token": "**********************",
        "timeout":token超时时间，
        "uid":77
    }
}
```

[资源](/assets/youdu.zip)

