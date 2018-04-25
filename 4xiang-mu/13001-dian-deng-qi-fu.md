### 概述

项目位置：wechat-mimi

后端模块：Diandeng

服务域名：[https://diandeng.seunc.cn](https://diandeng.seunc.cn)

提示：以下接口中只会说明常用参数中的data中的值，非200的错误，请查看msg,另外token必须放入AUTHORIZATION位，请求方式未特殊说明的情况下，请使用post传递。

### 接口

---

* **openid接口：用户进入小程序，识别用户**

接口地址:/user/openid

参数：code

返回值：

```
array(
'token'   => $token,
'timeout' => time() + 7200,
'uid'     => $insertResult['id'],
'status'  => $insertResult['status'],
);
```

参数解释：

token：存储识别用户信息，前端值不需要解析，只需要存储和传递

timeout:过期时间

uid:当前用户的id

status：用户头像和昵称是否完善的标志位

---

* **update接口，用于更新用户的基础信息，此接口需要nickname和avatarurl**

接口地址：/user/update

参数：nickname，avatarurl

返回值：空值

---

* **init接口：配置数据，供前端进行初始化**

接口地址：/deng/init

参数：无

返回值:

data中的config字段返回

| alipay\_code | 支付宝的code值 |
| :--- | :--- |
| alipay\_status | 支付宝的code值的状态 |
| create\_pay\_status | 创建的时候是否需要支付 |
| zan\_pay\_status | 点赞的时候是否需要支付 |
| create\_pay\_money | 创建点灯的时候需要支付的金额 |
| default\_share\_image | 默认分享的图片 |
| default\_share\_title | 默认的分享标题 |
| cdn\_url | cdn的地址 |
| banner\_image | banner图地址 |

data中的type字段返回

| id | 首页类别的id |
| :--- | :--- |
| name | 首页类别的名称 |
| icon | 首页类别的图片 |

data中的money字段

| id | 点赞支付的id |
| :--- | :--- |
| money | 点赞支付的金额 |
| value | 点赞之后获取的成长值 |

---

* **getDengTypeInfo接口：用户从首页进入点击任意祈福，获取的祈福的信息**

接口地址：/deng/getDengTypeInfo

参数:接口init中data中的type字段中的id值中的任意一个

返回值：

返回此灯的描述信息

| duixiang | 适用对象 |
| :--- | :--- |
| yunxiao | 运效 |
| jieyue | 解曰 |
| desc | 描述 |
| tips | 提示 |
| dengxin | 灯型 |
| background | 背景图片地址 |
| right\_light | 右侧灯图片的地址 |

---

* **create接口：创建点灯**

接口地址：/deng/create

参数：typeid，name，sex,birthday,wish

返回值：

此接口存在两种情况的返回值，id表示当前创建灯的id

第一种：init中的config字段中配置需要进行创建支付的

此时返回

```
array(
'id'      => id,
);
```

第二种、无需进行支付

```
array(
'id' => id,
);
```

---

* **createPay接口：创建祈福时候的支付接口，是否需要请求改接口，请根据init中的config中的create\_pay\_status字段进行判定**

接口地址：/deng/createpay

参数：create接口返回的id，{id：id}

返回值：

```
array(
'id'      => $id,
'payinfo' => $data['data'],
);
```

支付信息，和当前的id

---

* **getdenginfo接口：获取灯的信息，在上面create接口请求完成之后会返回灯的id值，获取的是该id值得信息**

接口:/deng/getdenginfo

参数:id

返回值：

此接口返回也存在两种情况

第一种、当前的灯是自己创建的

```
array(
'is_self' => TRUE,
'id'      => $info['id'],
'name'    => $info['name'],
'wish'    => $info['wish'],
'pc'      => $info['zanCount'],
);
```

第二种、当前的灯不是自己创建的

```
array(
'is_self' => FALSE,
'id'      => $info['id'],
'name'    => $info['name'],
'wish'    => $info['wish'],
'pc'      => ‘people zan count’ // 用户点赞数量
);
```

---

* **zanPay接口：用户点赞支付接口**

接口地址:/deng/zanPay

参数：deng_id，money_\_id

参数说明：

| deng\_id | 灯的id |
| :--- | :--- |
| money\_id | init接口中返回的money字段中的id值 |

返回值：

```
array(
'deng_id' => $deng_id,
'payinfo' => $data['data'], // 支付信息
);
```

---

* **selfCreate接口：当前用户自己创建的点灯祈福**

接口地址：/deng/selfWish

参数：offset,limit,其中limit默认为5

返回值：为一个json数组，数组中包含字段为

| id | 祈福的id |
| :--- | :--- |
| title | 祈福的标题【灯型】 |
| wish | 祈福的愿望， |
| addtime | 添加时间 :5-08 |
| count | 点赞人数 |
| image | 左侧的图片 |

---

* **maxZan接口：最亮许愿灯**

接口地址：/deng/maxZan

参数：offset,limit,其中limit默认为5

返回值：为一个json数组，数组中包含字段为

| id | 祈福id |
| :--- | :--- |
| title | 祈福的标题【许愿人的姓名】 |
| wish | 愿望 |
| addtime | 添加时间:05-02 |
| count | 点赞人数 |

---

* **lastWish接口：最新许愿灯**

接口地址：/deng/lastWish

参数：offset,limit,其中limit默认为5

返回值：为一个json数组，数组中包含字段为

|  |
| :--- |


| id | 祈福id |
| :--- | :--- |
| title | 祈福的标题【许愿人的姓名】 |
| wish | 愿望 |
| addtime | 添加时间:05-02 |
| count | 点赞人数 |

* **zanDetail接口：点赞列表**

接口地址：/deng/zanDetail

参数：offset,limit,id,其中默认limit为10，offset为0

返回中：

| nickname | 点赞用户名 |
| :--- | :--- |
| avatar | 点赞用户头像 |
| value | 点赞的幸运值 |
| time | 点赞的时间 |

* **zanNoPay接口：点赞不需要支付**

接口地址：/deng/zanNoPay

参数：deng\_id

返回值：

| value | 帮好友获得的随机好运值 |
| :--- | :--- |


* **getShare接口：获取分享朋友圈的图片**

接口地址：/deng/getshare

参数：id-&gt;当前点灯的id

返回值：正常的情况下返回file

| file | 图片地址 |
| :--- | :--- |




