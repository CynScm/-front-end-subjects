## 轮播图
方法：bootstrap前端框架的carousel插件
#### 1.html
轮播插件就是将几张同等大小的大图，按照顺序依次播放。
//基本实例。
```html
<div id="myCarousel" class="carousel slide">
	<ol class="carousel-indicators">
		<li data-target="#myCarousel" data-slide-to="0" class="active"></li>
		<li data-target="#myCarousel" data-slide-to="1"></li>
		<li data-target="#myCarousel" data-slide-to="2"></li>
	</ol>
	<div class="carousel-inner">
		<div class="item active" style="background:#750291;"><!--设置过渡颜色-->
			<img src="img/slide1.jpg" alt="第一张">
		</div>
		<div class="item" style="background:#B8866E;">
			<img src="img/slide2.jpg" alt="第二张">
		</div>
		<div class="item" style="background:#CCE49E;">
			<img src="img/slide3.jpg" alt="第三张">
		</div>
	</div>
	<a href="#myCarousel" data-slide="prev" class="carousel-control left">&lsaquo;</a>
	<a href="#myCarousel" data-slide="next" class="carousel-control right">&rsaquo;</a>
</div>
```
data 属性解释：
1.data-slide 接受关键字 prev 或 next，用来改变幻灯片相对于当前位置的位置；
2.data-slide-to 来向轮播底部创建一个原始滑动索引， data-slide-to="2"将把滑
动块移动到一个特定的索引，索引从 0 开始计数。
3.data-ride="carousel"属性用户标记轮播在页面加载时开始动画播放。
轮播插件有三个自定义属性：

| 属性名称       | 描述           |
|--------------| ------------- |
|data-interval| 默认值 5000，幻灯片的等待时间(毫秒)。<br>如果为false，轮播将不会自动开始循环。|
|data-pause|默认鼠标停留在幻灯片区域(hover)即暂停<br>轮播， 鼠标离开即启动轮播|
|data-wrap|默认值 true，轮播是否持续循环。|
#### js部分
如果在 JavaScript 调用就直接使用键值对方法，并去掉 data-；
```javascript
//设置自定义属性
$('#myCarousel').carousel({
	//设置自动播放/2 秒
	interval : 2000,
	//设置暂停按钮的事件
	pause : 'hover',
	//只播一次
	wrap : false,
});

//调整轮播器
$(window).resize(function () {
	var $height = $('.carousel-inner img').eq(0).height() || 
				  $('.carousel-inner img').eq(1).height() || 
				  $('.carousel-inner img').eq(2).height();//获取当前图片是第几张图，
				  //然后获取当前图片高度，然后设置line-height。
	$('.carousel-control').css('line-height', $height + 'px');
});
```
#### css部分
```css 
@charset "utf-8";

.logo {
	padding:0;
}
avbar-collapse ul {
	margin-top:0;
}
.carousel-inner img {
	margin: 0 auto;/*是轮播图居中*/
}
.carousel-control {
	font-size: 100px;
}
```
