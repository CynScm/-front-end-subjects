## 弹幕
#### 核心
```javascript
$(function(){
    var pageW=parseInt($(document).width());
    var pageH=parseInt($(document).height());
    var boxDom=$("#boxDom");
    var btnDom=$("#btn");
    var Top,Right;
    var width;
    width=pageW;
    var colorArr=["#cfaf12","#12af01","#981234","#adefsa","#db6be4","#f5264c","#d34a74"];
     btnDom.bind("click",auto);
    document.onkeydown=function(e){
        if(e.keyCode == 13){
            auto();
        }
    }//按下“发射”按钮或者按下enter键，都可以执行auto函数
    function auto(){
        var creSpan=$("<span class='string'></span>");
        var text=$("#text").val();
        creSpan.text(text);
        $("#text").val("");//输出弹幕后，清空输入框的内容，为下次输入弹幕做准备
        Top=parseInt(pageH*(Math.random()));//是输出的弹幕输出到页面的随机高度
        
        if(Top>pageH-90){
            Top=pageH-90;
        }
        creSpan.css({"top":Top,"right":-300,"color":getRandomColor()});
        boxDom.append(creSpan);
        var spanDom=$("#boxDom>span:last-child");
        spanDom.animate({"right":pageW+300},10000,"linear");
    }
    function getRandomColor(){
        return '#' + (function(h){
            return new Array(7 - h.length).join("0") + h//凑齐六位色彩,例：h的长度为5,则Array(2).join("0")=0，再加上h，则凑齐六位数
        })((Math.random() * 0x1000000 << 0).toString(16));//其他：Array(7).join("0")=000000，而Array(0).join("0")和Array(1).join("0")为空
    }//0x1000000<<0=16777216;Math.random() * 0x1000000 << 0=0~16777216;.toString(16)是转化成16进制，h就是16进制数
    //如果h为0x1000000时，没颜色
   });
```
