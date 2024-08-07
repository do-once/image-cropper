# @doonce/image-cropper

> 改造自[image-cropper](https://github.com/1977474741/image-cropper)

**支持非全屏裁剪，传入container_height、container_offset_top、container_width、container_offset_left参数即可**

- 源码在`demo/component`目录
- `demo`基于`iphone12/13 mini (375x812)`编写，其它模拟尺寸，需适配`container_height、container_offset_top、container_width、container_offset_left`参数

- ![图片](./.github/image.png)

## 一款高性能的小程序图片裁剪插件，支持旋转。
###### `1.功能强大。`
###### `2.性能超高超流畅，大图毫无卡顿感。`
###### `3.组件化，使用简单。`
###### `4.点击中间窗口实时查看裁剪结果。`

## 初始准备
#### 1.json文件中添加image-cropper
```json
    "usingComponents": {
       "do-image-cropper": "../image-cropper/image-cropper"
    },
    "navigationBarTitleText": "裁剪图片",
    "disableScroll": true
```
#### 2.wxml文件
```html
<do-image-cropper
    id="image-cropper"
    bindload="cropperload"
    bindimageload="loadimage"
    bindtapcut="clickcut"
    limit_move="{{limit_move}}"
    disable_rotate="{{disable_rotate}}"
    width="{{width}}"
    height="{{height}}"
    imgSrc="{{src}}"
    angle="{{angle}}"
    disable_width="{{disable_width}}"
    max_width="{{max_width}}"
    max_height="{{max_height}}"
    disable_height="{{disable_height}}"
    disable_ratio="{{disable_ratio}}"
    container_width="{{container_width}}"
    container_height="{{container_height}}"
    container_offset_top="{{container_offset_top}}"
    container_offset_left="{{container_offset_left}}"
  >
</do-image-cropper>
```
#### 3.简单示例
- 详细参考`demo`目录
```javascript
    Page({
        data: {
            src:'',
            width:250,//宽度
            height: 250,//高度
        },
        onLoad: function (options) {
	    //获取到image-cropper实例
            this.cropper = this.selectComponent("#image-cropper");
            //开始裁剪
            this.setData({
                src:"https://github.com/do-once/image-cropper/blob/main/.github/image.png",
            });
            wx.showLoading({
                title: '加载中'
            })
        },
        cropperload(e){
            console.log("cropper初始化完成");
        },
        loadimage(e){
            console.log("图片加载完成",e.detail);
            wx.hideLoading();
            //重置图片角度、缩放、位置
            this.cropper.imgReset();
        },
        clickcut(e) {
            console.log(e.detail);
            //点击裁剪框阅览图片
            wx.previewImage({
                current: e.detail.url, // 当前显示图片的http链接
                urls: [e.detail.url] // 需要预览的图片http链接列表
            })
        },
    })
```

## 参数说明<font color=#C39178 size=2>（高亮属性表示对于上个版本有修改，请注意）</font>
| 属性           | 类型   | 缺省值  | 取值  | 描述  | 必填 |
| ------------- |:------:|:------:|:-----:|:-----:|:-----:|
| container_width 	| Number | wx.getSystemInfo().windowWidth	   |无限制|非全屏裁切时容器宽度|否|
| container_height 	| Number | wx.getSystemInfo().windowHeight	   |无限制|非全屏裁切时容器宽度|否|
| container_offset_left 	| Number | 0	   |无限制|非全屏裁切时容器距离视口左侧的距离|否|
| container_offset_top 	| Number | 0	   |无限制|  非全屏裁切时容器距离视口顶部的距离；**自定义导航栏时，视口顶部在屏幕顶部；原生导航栏时，视口顶部在原生导航栏底部;**|否|
| imgSrc      	| String | 无	   |无限制|图片地址(如果是网络图片需配置安全域名)|否|
| disable_rotate| Boolean| false    |true/false|禁止用户旋转(为false时建议同时设置limit_move为false)|否|
| limit_move	| Boolean| false    |true/false|限制图片移动范围(裁剪框始终在图片内)(为true时建议同时设置disable_rotate为true)|否|
| width 	| Number | 200      |超过屏幕宽度自动转为屏幕宽度|裁剪框宽度|否|
| height        | Number | 200      |超过屏幕高度自动转为屏幕高度|裁剪框高度|否|
| max_width 	| Number | 300      |裁剪框最大宽度|裁剪框最大宽度|否|
| max_height        | Number | 300      |裁剪框最大高度|裁剪框最大高度|否|
| min_width 	| Number | 100      |裁剪框最小宽度|裁剪框最小宽度|否|
| min_height        | Number | 100      |裁剪框最小高度|裁剪框最小高度|否|
| disable_width 	| Boolean | false      |true/false|锁定裁剪框宽度|否|
| disable_height    | Boolean | false      |true/false|锁定裁剪框高度|否|
| disable_ratio    | Boolean | false      |true/false|锁定裁剪框比例|否|
| export_scale  | Number | 3        |无限制|输出图片的比例(相对于裁剪框尺寸)|否|
| quality 	| Number | 1        |0-1|生成的图片质量|否|
| ~~cut_top~~ 	| Number | 居中     |始终在屏幕内 |裁剪框上边距|否|
| ~~cut_left~~ 	| Number | 居中     |始终在屏幕内 |裁剪框左边距|否|
| `img_width` 	| Number | 宽高都不设置，最小边填满裁剪框 |支持%(不加单位为px)(只设置宽度，高度自适应)|图片宽度|否|
| `img_height` 	| Number | 宽高都不设置，最小边填满裁剪框 |支持%(不加单位为px)(只设置高度，宽度自适应)|图片高度|否|
| scale 	| Number | 1	   |无限制|图片的缩放比|否|
| angle 	| Number | 0	   |(limit_move=true时angle=n*90)|图片的旋转角度|否|
| min_scale 	| Number | 0.5	   |无限制|图片的最小缩放比|否|
| max_scale 	| Number | 2	   |无限制|图片的最大缩放比|否|
| bindload 	| Function | null   |函数名称|cropper初始化完成|否|
| bindimageload | Function | null  |函数名称|图片加载完成,返回值Object{width,height,path,type等}|否|
| bindtapcut 	| Function | null  |函数名称|点击中间裁剪框,返回值Object{src,width,height}|否|
## 函数说明
| 函数名         | 参数   	       | 返回值  |描述|参数必填|
| ------------- |:------:	   |:------:|:------:|:------:|
| upload      	|  无    	  |   无   |调起wx上传图片接口并开始剪裁|否|
| pushImg       |  src   	   |   无   |放入图片开始裁剪|是|
| getImg        |Function(回调函数) |   `Object{url,width,height}`  |裁剪并获取图片(图片尺寸 = 图片宽高 * export_scale)|是|
| ~~setCutXY~~     	|  X、Y  	  |   无    |设置裁剪框位置|是|
| setCutSize    |  width、height   |   无    |设置裁剪框大小|是|
| setCutCenter  |  无   	   	  |   无    |设置裁剪框居中|否|
| setScale      |  scale   	   |   无    |设置图片缩放比例（不受min_scale、max_scale影响）|是|
| setAngle      |  deg   	   |   无    |设置图片旋转角度（带过渡效果）|是|
| setTransform  |{x,y,angle,scale,cutX,cutY}|   无    |图片在原有基础上的变化(scale受min_scale、max_scale影响)|根据需要传参|
| imgReset      |无	          |   无    |重置图片的角度、缩放、位置(可以在onloadImage回调里使用)|否|


如果有什么好的建议欢迎提issues或者提pr
