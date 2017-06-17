# Generate-an-image-by-canvas
### 最近在学习canvas就想做些有意思的东西
这是一个类似于网上流传的很多的那种输入汉字然后自动生成表情包的小东西，然后就想要来试一下
### 最终将实现的功能：
最终实现的功能大概有这些：
* 用户可以自由上传图片，自由编辑文字内容，并选择插入文字的位置
* 下载生成的图片
* 原笔迹的书写
* 图片下载寒酸的裁剪功能
* 几种图片滤镜：黑白，负片以及更多
--- 
额。。大概就这些了，后续想到的话再加上来。
最近快要期末考试了，所以写的会有点慢。
### 上传图片
上传图片在这里使用的是HTML5中的filereader来实现的，将图片以DataURL的形式读入页面，在上传前通过输入缩放倍数来选择大小
后期考虑引入滑块式的调整各种参数，直接输入的方法真的是。。
### 下载图片
下载图片时是可以选择保存图片的格式的，但是考虑到在使用时的繁琐，就直接设置为了jpg格式。
在canvas中保存图片是使用的toDataURL来做的。在保存的时候比较关键的一个就是要将MIME类型改为image/object-stream。得到的图片data:image/png;base64需要更改一下
```
var fixtype = function (type) {
      type = type.toLocaleLowerCase().replace(/jpg/i, 'jpeg');
	    var r = type.match(/png|jpeg|bmp|gif/)[0];
	    return 'image/' + r;
		}
  imgdata = imgdata.replace(fixtype(type), 'image/octet-stream')
```
 ### 图片处理
 说是图片处理，我都不好意思了hhh
 * 添加文字：使用是canvas中自有的渲染文字的方法，可以选择字体颜色
 取色器使用getImageData做，得到的数据是一个数组，每四位分别表示一个像素点的RGBA数据。取到该像素点的数据，简单的字符处理就可得到可用的颜色数据。
 * 原笔迹书写：监听鼠标的move up down 使用了line_X,line_Y,line_N三个数组分别储存的轨迹的X Y坐标，和鼠标是否按下的标志信息，当鼠标按下并移动时分别向数组X，Y ，N push该点坐标及标识位信息，然后整个遍历一次数组，标志位为1时lineTo（X，Y），标志位为0时移动坐标，进行下一笔画的渲染。突然发现这样遍历成本很高，回头改改。
 #### 几种辣鸡滤镜
 很简单的滤镜功能：
* 负片：取到图片的数据，循环一次数组，分别将R位 G位 B位数据被255减即可。
* 黑白：同样拿到数据，步阶为4，将r、g、b位求平均值，然后rgb均等该改平均值即可
* 高斯模糊：这里引入了一个StackBlur.js，自己写有点应付不来。。
 大概就是这些吧。
 ## 学到了什么？
 技术方面的不再说，说一些软的东西吧。
 一个完整项目中的一些命名规则，变量的处理，包括全局和私有变量选择，函数的处理，如何解决函数间的通信等
 另外，在整个项目开始之前的大概的规划，就算是练手的小项目，大致要做成什么样子也要有一个底。不然在写的时候很容易越写越乱。
 另外，代码规范！这个东西并未严格的按照家园的前端代码规范来写，因为觉得只是练手吧，直到现在开始接触Vue 工作室要求使用EAlint来规范代码，真的很难受。。这个东西还是从开始就养好习惯比较好。不要因为追求速度而忽视了这些‘软实力’。
 就这些吧~
 
 
 
