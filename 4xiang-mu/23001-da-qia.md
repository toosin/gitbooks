### 概述

此文档描述打卡相关的逻辑和api接口

打卡产品图![](/assets/daka.png)

#### 广告相关接口

---

##### 关于data的返回值约定

```
array(
    'type'=>'mini', // 打开小程序
    'img'=>'', // 展示图片
    'appid'=>'',//小程序appid
),
array(
    'type'=>'session', // 打开客服消息
    'img'=>'', // 图片
    'session_name'=>'cash', // 会话名称
),
array(
    'type'=>'article', // 打开本小程序文章
    'title'=>'', // 文章展示的标题
    'id'=>'', // 文章id
),

//广告只存在两种状态，打开小程序和打开客服会话，打开小程序的时候type为mini，客服的时候为session
```

| 公用返回 |
| :--- |


| 参数 | 参数值 |
| :--- | :--- |
| status | 请求状态，200代表成功，其他的代码 |
| msg | 如果状态不为200时的错误信息 |
| data | 请求数据，包含参数 |

##### 

##### getLaunch

> 获取app初始化相关的信息，包含是否展示初始广告已经广告内容

参数

| 参数 | 参数值 |
| :--- | :--- |
| 无 | 无 |

请求地址

/ad/getLaunch

| data返回值 | 参数值 |
| :--- | :--- |
| status | 1表示开启launch广告，0表示不开启 |
| type | 广告类型，可选值：mini，session |
| appid | 当type为mini时会返回该参数 |
| session\_name | 当type为session时返回该参数 |
| img | launch加载的图片 |

**getIndex**

> 获取首页的广告相关的数据

参数

| 参数 | 参数值 |
| :--- | :--- |
| 无 | 无 |

请求地址

/ad/getIndex

返回值

| data返回值 | 返回值 |
| :--- | :--- |
| type | 类型，取值有article,mini,session |
| title | 文章的标题 |
| id | 当type为article时返回该值 |
| appid | 当type为mini的时候返回该值 |
| session\_name | 当type为session时返回该值 |
| status | 1表示开启广告，0表示不开启广告 |

**getCash**

> 获取提现页面的广告信息

参数：无

请求地址：

/ad/getCash

返回值

| data返回值名 | 返回值 |
| :--- | :--- |
| status | 1表示开启广告，0表示不开启广告 |
| type | 类型，取值mini,session |
| img | 广告的图 |
| appid | 当type为mini时候展示 |
| session\_name | 当type为session的时候展示 |



