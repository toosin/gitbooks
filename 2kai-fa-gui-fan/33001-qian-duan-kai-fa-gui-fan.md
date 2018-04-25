### 前端目前以小程序为例

**文件目录结构**

component/ 无状态组件：复用、不能有业务逻辑

pagecomponent/ 有状态组件：引用无状态组件、包含业务逻辑、只在一个项目内有用

page/ 入口

modal/ 数据、请求等：暴露接口给有状态组件

proxy.js 请求：

> 1Proxy类的status方法用于验证200状态
>
> 2一个接口对应一个方法
>
> 3.url是域名前缀，urls是接口
>
> 4.有个token变量和localstorage有token、timeout、uid
>
> 5.设置独立的proxy的目的是：不想在组件里看到大量的wx.request,但是回调逻辑要在pagecomponent，少放在proxy

store.js 数据：用了状态管理架构的放这，如Redux的reducer。或者其他不想放appdata的数据。

resource/ 资源

font/ 字体

image/ 图片

mp3/ 音频

until/

behavior/ 公用函数库

behavior.js 配置库统一出口如分享函数，```init(this, { onShareAppMessage: true, onsharecof: true })``\`表示注入分享朋友和分享朋友圈逻辑。

lib/ 三方库

until.js 工具函数库

app.wxss 公用样式：布局类、抽象类

**代码规范**

方法命名：`SlideFinish`首字母大写的驼峰

缩进：就用微信开发者默认的`shift+alt+F`

注释：每个组件最上面写上组件的用途，调用方式。

导入模块用 `import from` 不用`require`

用`let/const`不用`var`

`appdata`表示`app`全局变量

`that`表示当前组件

