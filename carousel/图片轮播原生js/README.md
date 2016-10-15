## 轮播图
方法：js原生
## html部分：
　　把轮播图（序号ul和轮播图banner_list）设置在id="banner"里面,代码如下
```html
<div id="banner">    
   <ul id="list"></ul>
   <div id="banner_list">
        <a href="#" target="_blank"><img src="imgs/p1.jpg" title="近水楼台的blog1" alt="近水楼台的blog1"/></a>
        <a href="#" target="_blank"><img src="imgs/p2.jpg" title="近水楼台的blog2" alt="近水楼台的blog2"/></a>
        <a href="#" target="_blank"><img src="imgs/p3.jpg" title="近水楼台的blog3" alt="近水楼台的blog3"/></a>
        <a href="#" target="_blank"><img src="imgs/p4.jpg" title="近水楼台的blog4" alt="近水楼台的blog4"/></a>
    </div>
</div>
```
## css部分：
1、选择轮播图的素材4张图，图的尺寸为478*286，给banner设置css，设置它的width为478px,height为286px,并且让超出banner的其他图隐藏起来，用```overflow:hidden```，并且为了让序号ul放在框的右下角，则让banner为```position:relative```,让#banner ul设置为```position:absolute```,然后可以设置，bottom,right,还有z-index。让ul序号横着排，因此要用到```float:left```。
2、导航ul序号，背景透明阴影，用```position:absolute```,然后设置```buttom:0```,让ul沉底。
## js部分：
单独一个carousel.js。
#### 1.文档碎片
首先使用```frag=document.createDocumentFragment()```生成文档碎片，这样的好处是：节约使用DOM。因为每次JavaScript对DOM的操作都会改变页面的变现，并重新刷新整个页面，从而消耗了大量的时间。为解决这个问题，可以创建一个文档碎片，把所有的新节点附加其上，然后把文档碎片的内容一次性添加到document中。

然后通过一个循环里加```this.num[i]=frag.appendChild(document.createElement("li"))).innerHTML=i+1```给ul添加相应的序号。

然后通过```document.getElementById("list").appendChild(frag);```真正给把文档碎片加到ul中。
#### 2.将除了第一张外的所有图片设置为透明
利用自定义函数each();
```javascript
function each(arr, callback, thisp) {
 for (var i = 0, len = arr.length; i < len; i++) callback.call(thisp, arr[i], i, arr);}
	//callback.call里的四个参数分别是：指定的对象（即thisp指向的对象）和其他执行的参数。
}

function setOpacity(elem, level) {
	if (elem.filters) {
		elem.style.filter = "alpha(opacity=" + level + ")";//低版本浏览器
	} else {
		elem.style.opacity = level / 100;//高版本浏览器
	}
}//考虑到跨浏览器兼容性问题。

each(this.img,function(elem,idx,arr){
//第一个参数this.img对应一个数组对象arr;第二个参数是函数
//function，对应回调函数callback,而function（也就是callback）有三个参数，分别代表	
//数组元素的值（elem）、数组元素的数字索引(idx)、包含该元素的数组对象(arr)
	if (idx!=0) setOpacity(elem,0);//设置为"0"就是使图片透明
});

```
#### 3.为所有li添加事件
```javascript
each(this.num,function(elem,idx,arr){//又使用了each,按下哪个，就把那张图的序号赋值
                                     //给curIndex和targetIdx;
	elem.onclick=function(){
		self.fade(idx,curIndex);//将按下去的序号与当前的序号传给fade
		curIndex=idx;
		targetIdx=idx;
	}
});

fade:function(idx,lastIdx){
	if(idx==lastIdx) return;
	var self=this;
	fadeOut(self.img[lastIdx]);//接受当前的序号，并且使当前图片慢慢消失
	fadeIn(self.img[idx]);//接受按下去的序号，使相应的图片慢慢显示
	each(self.num,function(elem,elemidx,arr){
		if (elemidx!=idx) {
			self.num[elemidx].className='';//使当前序号取消高亮
		}else{
			id("list").style.background="";
			self.num[elemidx].className='on';//使目标序号高亮
			}
	});
	this.info.innerHTML=self.img[idx].firstChild.title;//显示相应的文字说明
}
```
#### 4.渐渐消失，渐渐显现函数
```javascript
function fadeIn(elem) {//渐渐显示
		setOpacity(elem, 0)
		for ( var i = 0; i < 20; i++) {
			(function() {
				var pos = i * 5;
				setTimeout(function() {
					setOpacity(elem, pos)
				}, i * 25);
			})(i);
		}
	}
	function fadeOut(elem) {//渐渐消失
		for ( var i = 0; i <= 20; i++) {
			(function() {
				var pos = 100 - i * 5;
				setTimeout(function() {
					setOpacity(elem, pos)
				}, i * 25);
			})(i);
		}
	}
```
#### 5.自动轮播效果
```javascript
var itv=setInterval(function(){
	if(targetIdx<self.num.length-1){
		targetIdx++;
	}else{
		targetIdx=0;
		}
	self.fade(targetIdx,curIndex);
	curIndex=targetIdx;
	},2000);
	id(ulId).onmouseover=function(){ clearInterval(itv)};//当鼠标在图上时，停止轮播
	id(ulId).onmouseout=function(){//当鼠标移出时，继续轮播
	itv=setInterval(function(){
		if(targetIdx<self.num.length-1){
			targetIdx++;
		}else{
			targetIdx=0;
			}
		self.fade(targetIdx,curIndex);
		curIndex=targetIdx;
	},2000);
}
```
