# 星星点评方式控制

现在的网站中都会有一个给用户做评分的星星或者是其他什么图案，反正呢方式很多，在很早之前我写过一个方式，不过自己都忘了是什么方式了，然后那个网站也已经关了。最近心血来潮，又稍微折腾一下。大概想到了几个方式来处理，不过还是有失败的。

## 思路

处理这个星星评级的方式，如果只是想通过CSS来处理的话，主要还是利用**宽度**、**背景**等属性来操作，通过`:hover`、`:target`甚至是`:checked`等方式来改变；这里再结合`E ~ F`和`E + E`的选择符方式来影响。

星星呢是一张周围是白色，中间是透明的（可以是有alpha通道的透明）图片，这样的话，只要控制图片下面的背景色即可，图片如下：

![有alpha透明的星星图片](http://linxz.github.io/CSS_Skills/img/picture/star_rating.png)这里是有图片的，只是看不到啊看不到……

## demo展示&简要说明

基本上大概的简要说明已经在[Demo](http://linxz.github.io/CSS_Skills/demo/picture/star_rating.html)页面中说明了，大家可以直接进入[Demo](http://linxz.github.io/CSS_Skills/demo/picture/star_rating.html)看效果，如果愿意的话，也可以再花几分钟时间看下去。

首先在这份[Demo](http://linxz.github.io/CSS_Skills/demo/picture/star_rating.html)中，主要有一个空的标签元素来控制图片下面的背景色，大概的代码是这样的：

	.bg_bar {position: absolute;top: 0;left: -100px;width: 100px;height: 20px;background-color: #c8c8c8;border-left:100px solid #f32600;border-right: 100px solid #3171d2;z-index: 2}

这里通过绝对定位的方式，是为了能够更好的通过定位`left`和`right`来达到最终的效果，宽度和左右两边的边框是相等的，这三个数值都是根据图片的宽度来计算的。

### E~F 选择符结合 a 的 :hover 方式来控制选择星星

这种方式，主要就是还是利用`a`标签的`:hover`来影响某个元素的宽度或者坐标位置，由于之前对`.bg_bar`采用的是绝对定位的方式，那么这里只要考虑控制`left`或者`right`的坐标即可。

	.demo1 a:hover ~ .bg_bar {left: -80px;}
	.demo1 a + a:hover ~ .bg_bar {left: -60px;}
	.demo1 a + a + a:hover ~ .bg_bar {left: -40px;}
	.demo1 a + a + a + a:hover ~ .bg_bar {left: -20px;}
	.demo1 a + a + a + a + a:hover ~ .bg_bar {left: 0;}
	
就这样，一个简单的鼠标滑过时，星星的颜色也就改变了……

### E~F 选择符结合 :checked 控制星星

这种方式，其实跟上面一种是类似的，不同的是，`input type="radio"`类型我们不仅可以让它有`:hover`，还可以有`:checked`来作为选择符来改变样式。所以，这里我们得到三种颜色，**默认**、**鼠标经过的颜色**、**点击选中后的颜色**，而且这三种颜色就是之前我们所设置的宽度（背景色）、左右两边边框色。

	.demo2 input {position: relative;float: left;width: 20px;height: 20px;border:0 none;background: none;z-index: 3;-webkit-appearance:none;cursor: pointer;}
	.demo2 input:checked ~ .bg_bar {left: auto;right: 80px}
	.demo2 input + input:checked ~ .bg_bar {right: 60px;}
	.demo2 input + input + input:checked ~ .bg_bar {right: 40px;}
	.demo2 input + input + input + input:checked ~ .bg_bar {right: 20px}
	.demo2 input + input + input + input + input:checked ~ .bg_bar {right: 0;}
	
	.demo2 input:hover ~ .bg_bar {left: -80px}
	.demo2 input + input:hover ~ .bg_bar {left: -60px;}
	.demo2 input + input + input:hover ~ .bg_bar {left: -40px;}
	.demo2 input + input + input + input:hover ~ .bg_bar {left: -20px;}
	.demo2 input + input + input + input + input:hover ~ .bg_bar {left: 0;}
	
这里需要注意的一点是，`:hover`时的颜色是要覆盖在`:chekced`选中后的颜色之上，所以，定位的层叠顺序以及选择符的优先级需要考虑。更关键的一点是，要把`input`的默认样式给去掉，这里仅用了针对`webkit`内核的一个私有属性：`-webkit-appearance:none;`。

### input type="range" 的方式来展示星星的数量

好吧，本来我设想这种方式是在拖动`range`的那个进度条时，同时改变宽度或者其他什么的来达到最终的目的，不过暂时没得到我想要的结果，唉～

现在如果用鼠标在上面拖动的话，仅仅是一颗星星在滑动。当然，如果结合`onchange`的话，或许也可以改变点什么，但这样的话，就跟我最初的设想不一样了，不是用CSS来装逼了……

而且改变`range`里面的元素也是很折腾的事情，就先这样吧……

### a标签中的锚点跳转结合:target选择符方式实现

这个就简单了，直接利用`:target`来跳，跳到需要的元素上，让它显示出来。也就因为是这样，HTML结构有点复杂，N个`a`对应着N个`span`……

	.demo4 .bg_bar {position: static;display: none;width: 0;border: 0 none;background-color: #f32600;}
	.demo4 #s1:target {display: block;width: 20px;}
	.demo4 #s2:target {display: block;width: 40px;}
	.demo4 #s3:target {display: block;width: 60px;}
	.demo4 #s4:target {display: block;width: 80px;}
	.demo4 #s5:target {display: block;width: 100px;}
	
## 结语

终于又折腾了一个……感觉自己又是在应付，不写不折腾怕忘记了一些东西（还真的是会忘记），写呢又发觉写了之后是没什么用处的东西，真实一个纠结的人啊！