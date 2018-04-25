### 好奇怪小程序-后端接口

##### 概述

###### 后端主要包含两个接口，一个是获取用户的openid的接口存储用户信息为以后的模板消息提供基础，另一个是获取所有图片的接口

后端接口地址：https://api.strange.lha116.cn

##### 详细接口

> 获取用户信息，该接口在用户进入小程序的时候进行调用

接口地址：/user/openid

接口参数：code\(小程序login时候获取的code\)

返回值: data中的返回值包含，在目前的需求中无需处理该接口的返回值

```
array(
'token'=>$this->encode($uid), 
'timeout'=>time() + 7200,
'uid'=>$uid
);
```

> 获取图片信息

接口地址：/image/resource

接口参数：无

返回值：

```
"data": {
"imagesList": 
[
    {
        "id": "toView25",
        "title": "好奇怪",
        "original": "http://cnd.056liu.cn/strange/background/origin/origin_0.png",
        "small": "http://cnd.056liu.cn/strange/background/small/small_0.png"
    },{
        "id": "toView26",
        "title": "水形物语",
        "original": "http://cnd.056liu.cn/strange/background/origin/origin_1.png",
        "small": "http://cnd.056liu.cn/strange/background/small/small_1.png"
    }
],
"topImgList": 
[
    {
        "id": "toView1",
        "title": "空耳朵",
        "original": "",
        "small": "http://cnd.056liu.cn/strange/top/small/small_0.png"
    },
    {
        "id": "toView2",
        "title": "好奇怪刘海",
        "original": "http://cnd.056liu.cn/strange/top/origin/origin_1.png",
        "small": "http://cnd.056liu.cn/strange/top/small/small_1.png"
    }
]
}
```



