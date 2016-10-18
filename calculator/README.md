## 计算器
核心为javascript
```javascript
function getClass(d,f)
{
	if(document.getElementsByClassName)
		{
			return d.getElementsByClassName(f)
		}
};
window.onload=function()
{
	var aNum=getClass(document,'num');//使用getClass函数获取class
	var oText=document.getElementById('text');
	var aPer=getClass(document,'oper');
	var oPer=document.getElementById('per');
	var oText1=document.getElementById('text1');
	var oDeng=getClass(document,'deng')[0];
	var oSq=getClass(document,'sq')[0];
	var oRec=getClass(document,'rec')[0];
	var oZheng=getClass(document,'zheng')[0];
	var oOn=getClass(document,'on')[0];
	var oOff=getClass(document,'off')[0];
	var oClea=getClass(document,'clea')[0];	
	var bOnOrOffClick=false;	
	
	function fnNum(a)
	{		
		var bClear=false;
		oText.value='0'	;
		for(var i=0;i<aNum.length;i++)
		{						
			aNum[i].onclick=function()
			{	
				if(!bOnOrOffClick)return;//如果点“关机”，bOnOrOffClick为false,当输入值时，此时运行到这里，返回,无法输入
	                                                 //其实在输符号时也应该加这句，但是因为本身符号不会显示， 所以可以不写。
				if(bClear)
				{					
					
					bClear=false;
				}
				if(oText.value.indexOf('.')!=-1)
				{
					if(this.innerHTML=='.')
					{
						return;	
					}	
				}//当输入的值中有“.”号，则不再允许输入“.”
				if(oPer.value&&oText.value&&oText1.value=='')
				{//只有当一次运算在输入符号后，接着输第二个值时才会true,
				 //才会执行(比如“12+122=”，当输‘12’和‘+’时为空（初次输入）或者false（连续运算时），
				 //当输‘122’时，才会运行，即把‘12’放入oText1,然后好让后面的程序，把‘122’赋值给oText)
					oText1.value=oText.value;	
					oText.value='';																
				}		
				var re1=/^([0]\d+)$/;//以0开头的整数，类似012,02	
				oText.value+=this.innerHTML;//持续输入数值（包括“.”，但不重复“.”）
				if(re1.test(oText.value))
				{	
					oText.value=this.innerHTML;				
				}//oText已经有0，因此当输入值后，如果符合正则re1，则去掉0
			}	
			for(var j=0;j<aPer.length;j++)//符号部分的添加
			{			
				aPer[j].onclick=function()
				{						
					oPer.value=this.innerHTML;		
				}						
			}//将符号赋值给符号缓冲器oPer			
			oDeng.onclick=function()//点击等号的时候
			{			
				//+-*/%的情况
				if(oText1.value==''&&oPer.value==''&&oText.value=='')
				{
					return;	
				}
				var n=eval(oText1.value+oPer.value+oText.value);			
				oText.value=n;
				oText1.value='';
				oPer.value='';	
				bClear=true;																
			}//计算结果为n，并且付给显示器oText,同时清空oPer和oText1
			oSq.onclick=function()//点击开根号的时候
			{
				var m=Math.sqrt(oText.value);	
				oText.value=m;
			}
			oRec.onclick=function()//点击倒数的时候
			{
				var a=1/oText.value;
				
				if(oText.value=='0')
				{
					a='正无穷'	
				}
				oText.value=a;	
			}
			oZheng.onclick=function()//正负号的时候
			{
				if(oText.value>0)
				{
					oText.value=-oText.value;
				}	
				else
				{
					oText.value	=-oText.value;	
				}
			}	
			oClea.onclick=function()//清屏的时候
			{
				oText.value='0';
				oText1.value='';
				oPer.value='';	
			}	
		}	
	}
	//on时
	oOn.onclick=function()
	{
		bOnOrOffClick=true;
		fnNum(bOnOrOffClick);	
	}	
	//off时
	oOff.onclick=function()
	{		
		bOnOrOffClick=false;
		fnNum(bOnOrOffClick);
		oText.value='';
	}
}
```
