### gdclient函数库合成图片

自建的图片合成库，主要是针对GD库进行开发，将常用的功能基本上都进行了封装，方便调用。

实现了以下功能：

1、创建指定宽度、高度的图片，支持alpha，也就是说支持生成空白png图

2、文字水印，支持任意坐标点文字，可指定文字的大小，角度，延时，支持alpha

3、文字水印，支持x轴居中的文字水印，Y轴居中的水印

4、图片合成

5、图片压缩

6、生成圆角图片

7、支持高斯模糊

8、支持直接输出浏览器或文件中

9、无须关系图片的后缀，无须关系图片的destory，自动释放

10、目前支持png和jpg

### Example

1、创建一张白色空白的图片

```
$gdclient = new gdclient()
$gdclient->createEmptyImage(400,400,'000')
```

2、创建一张白色透明的图片

```
$gdclient = new gdclient()
$gdclient->createEmptyImage(400,400,'000',127)
```

3、从图片中创建并在图片中创建一个水平居中的文章和用户的头像并输出到文件中，一般的功能都是在底图上添加不同的图片或者文章

```
$bg = 'static/xinyunci/resource/'.$rd.".jpg";
$gdclient->imageCreate($bg);
$gdclient->imagecopy($icon,313,180,0,0,1,1);
$gdclient->imageTextXCenter(30,0,340,'static/xinyunci/fonts/PingFang Bold.ttf',$nickname,'161419');
$outfile = "static/xinyunci/"."build_".$this->openid.'jpg';
$gdclient->ouputFile($outfile);
```

具体的参数，请参看lib中的源码源码位置位于

/ThinkPHP/Library/Com/NLGD



