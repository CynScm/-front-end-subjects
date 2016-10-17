## 滚动条回顶
方法1：jquery
```html
<div class="scroll" id="scroll" style="display: none;">回到顶部</div>
<!--这句话生成一个回顶的点击区，并且让他隐藏-->
```
```css
 .scroll{
    width: 70px;
    height: 70px;/*点击区大小*/
    background: #64bfae;
    color: #fff;
    line-height: 70px;
    text-align: center;
    position: fixed;
    right:30px;
 	bottom:50px;
 	/*设置position: fixed;然后设置right:30px;bottom:50px;使点击区置于页面右下角*/
 	cursor: pointer;
 	font-size:14px;
	 }
```
```javascript
$(function(){
	$(window).scroll(function(){
		var scrollValue=$(window).scrollTop();//使用scrollTop获取滚动条到页面顶的距离
		scrollValue>100?$('div.scroll').fadeIn():$('div.scroll').fadeOut();
	})//如果距离大于100，则使点击区渐渐显示，如果小于100，则渐渐消失
	$('#scroll').click(function(){
		$('html,body').animate({scrollTop:0},200)
	})//经历200ms，到达顶端
})
```
#### 注意：jquery中fadeIn()和fadeOut()函数只适用于display:none 的元素，与js不一样，js中想要通过transition来慢慢显示或慢慢隐藏元素，元素只能是 visibility:hidden。这与display破坏transition，因为display会导致reflow。
