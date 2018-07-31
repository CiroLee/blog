## 【小设计】——krpano轻提示
> 文章概述
 > 1. 文字轻提示的设计和使用
 > 2. 常用button的设计和使用
 > 3. 代码分析

　　说起前端UI框架，想必大家能说出很多来，比如很火的基于Vue的UI框架就有很多：饿了么团队开发的Element、Mint(移动端)、Vux、Muse、iView等。想到这里，研究krpano的程序员也想打造一个属于krpano的UI框架，虽然这些通过HTML+CSS的知识完全可以做到，或者修改使用成熟的前端UI框架，但是，可但是，我就是想再造个轮子，造轮子虽然没有什么实用意义，但是造轮子的过程确实学习进步的必经之路，所以接下来的balabala~，就是使用krpano的语法，造一个简易的基于krpano的UI库，设计宗旨就是：无序关注代码，拿来就用。  
DEMO地址如下[点击预览](http://nanhaibo.com/bbt_tech/panowork/pano_toast)
  
　　核心文件：**panotoast.xml**,使用时在主文件中引入:  
```
<include url="panotoast.xml" />
```

　　panotoast.xml是一个轻便的文字提示的插件。日常使用过程中，往往会遇到一些文字提示框，比如进入一个场景，弹出该场景的名字的简单文字提示，这个功能虽然简单，但是能做到模块化就好了，panotoast做的就是这件事，把文字提示模块化。   
　　一个简单的使用场景，点击一个按钮，弹出一个文字提示，提示的内容可以动态修改，弹窗的位置也是可变的，比如出现在上边、下边等。在panotoast里，实现以上功能，只需要一个函数：  
##### <p align="center" style="color:#F3750F;font-size:18px">**create_toast(*html*,*style*,*delaytime*)**</p>   
在onclick事件里写入：   
```
onclick="create_toast('简单提示'，basic|popup,5)"
```
就会创建一个从屏幕上方出现的文字提示。现在来了解一下这个函数的用法。   
　　　**create_toast(*html*,*style*,*delaytime*)**有三个参数：html、style、delaytime；    
- **html： 提示文字(必选)**
- **style：提示框样式(必选)**
- **delaytime：延迟时间，提示框几秒后自动消失，默认为2s(可选)**
   
①.这里需要注意的是*style*参数，style参数虽然为一个整体，但是由两部分组成，由上边的例子可以知道，两部分由“|”相隔，没错，就像在krpano中使用样式style一样，“|”表示使用了多个style属性，*style*属性是：“颜色”+“出现位置” 的顺序构成(顺序不可更改)。这里预设了4种常规颜色、两种特殊颜色和7个位置，六种颜色分别是：  
     
基本色 ：

- basic ===> 基础色（黑）
- warn ==> 警告
- success ==> 成功
- error ==> 错误               
********   
特殊色：      
- transgray ==> 半透明灰色
- transcircle ==> 半透明灰色圆角

②.出现位置。style参数的第二个部分就是出现位置部分，综合实际使用场景，把toast的出现位置列为7个：popup(上方)、popdown(下方)、popcenter(中央)、poplefttop(左上角)、popleftbottom(左下角)、poprighttop(右上角)、poprightbottom(右下角)；两个部分结合，形成完整的toast的样式。      
　　到此似乎toast的用法就讲完了，NO! 还得继续，我们来剖析一下toast是怎样设计的，来看看代码把！首先是最重要的**create_toast()**函数，代码如下：    
```
<!--注释：
		%1 ==> _html:提示文字(必须)
		%2 ==> _style:样式(必须)
		%3 ==> _delaytime:延迟事件，默认2s(可选)
-->

	<action name="create_toast" >
		set(_style,%2);
		txtsplit(%2,'|',__style);
		copy(special,__style[0].value);


		ifnot(%3,
			set(_delaytime,2),
			set(_delaytime,%3)
		);

		removelayer(toastlayer);

		addlayer(toastlayer);
		tween(layer[toastlayer].alpha,1,0.3);
		layer[toastlayer].loadstyle(%2);
		set(layer[toastlayer],
			keep='true',
			type='text',
			padding='10 40',
			bgroundedge='2',
			css='font-family:Arial; font-size:16px; color:#fff;text-align:left',
			html=%1,
			onclick="destory_toast(_style)"

		);
		<!--特殊样式设置，覆盖默认设置-->
		if(special == 'transcircle',
			set(layer[toastlayer].bgroundedge,40);
		);
		
		delayedcall(get(_delaytime),destory_toast(_style));
	</action>
```
设计思路是：调用函数时，根据传入的参数，创建一个layer，这里用到了krpano的action——[addlayer()](https://krpano.com/docu/actions/#addlayer)，在创建layer时，会固定一些不会变化的属性，这些属性用set()来设置(圆角样式需要修改属性，后边会说明)，并为layer绑定onclick属性——点击toast时销毁这个layer；当然，不操作toast时，toast在一定时间delaytime后会自动消失，这时调用的也是销毁函数；这时需要设置delaytime一个默认值，当不传值时，取默认值；     
```
delayedcall(get(_delaytime),destory_toast(_style));
```
上边位于末尾的代码就是根据设置(或默认)的时间来销毁toast。上边提到一个特殊，就是圆角属性部分，为此我们把第二个参数%2存储起来，来剥离础style的第一部分并存储到special变量中，对于特殊的圆角属性，当然是特殊处理，当函数判断sepcial为transcircle时，重置bgroundedge属性；如果您对代码进行修改，添加了自己的特殊样式，这里也是很好修改的。
```
<!--特殊样式设置，覆盖默认设置-->
		if(special == 'transcircle',
			set(layer[toastlayer].bgroundedge,40);
		);
```
这里有人要问，为什么不把重置的圆角属性写到style中呢？这时因为属性的设置是有优先级的，写到layer或者是通过set设置元素的属性优先级最高，而style中的同名属性不会覆盖元素标签内的属性，所以需要在函数内重新set，这里有个小tips，通过“|”附加的属性，后边的覆盖前边的同名属性。    
　　销毁函数**destory_toast(style)**接受style参数，根据style的类型，来实现不同的退出效果。
　　细心的同学在demo中还看到了一些button样式，没错，我还添加了一些常用的按钮样式，算不算是锦上添花呢<(￣︶￣)>，下边就来说说button部分。    
　　之前在krpano中要放置一些特殊的按钮，通常是用图片的形式来代替，就是在layer中添加图片url，这样的好处就是， 你想用什么样的按钮就可用什么样的按钮；但是这样的弊端就是一些简单的异形按钮也用图片，似乎有些浪费，还得麻烦UI设计师帮你切切图。因此，我把最常用的两种按钮样式抽离成通用样式来使用。常用的button样式无非就两种，**圆角矩形按钮**和**圆角按钮**；     
```
<!--button style-->

	<style name="btn_default" 
		keep="true"
		type="text"
		padding="16 50"
		bgroundedge="4"
		css="font-family:Arial; font-size:16px; color:#fff;text-align:center"
		/>
	<style name="btn_circle"
		keep="true"
		type="text"
		padding="16 50"
		bgroundedge="50"
		css="font-family:Arial; font-size:16px; color:#fff;text-align:center"
		/>

	<style name="btn_block"
		keep="true"
		type="text"
		width="100%"
		padding="16 50"
		bgroundedge="4"
		css="font-family:Arial; font-size:16px; color:#fff;text-align:center"

		/>
```
以上是三种常用的按钮样式。    
- **btn_default** ：圆角矩形按钮；
- **btn_circle**  ：圆角按钮；
- **btn_block** ：块级按钮，宽度为100%，一般用于重要提示，独占一行；     

其次，对按钮的size也做了统一，分别是default、large、small三种；至于按钮的颜色，可以使用之前toast使用的预设颜色，所以，一个完整的button的使用如下：     
```
<layer name="btn_popup" style="btn_default|basic|small" align="left" html="popup" x="6" onclick="create_toast('简单提示',basic|popup)" />
```
这里仍需要注意style部分，style部分分成三个部分，“**按钮样式|颜色|尺寸**”，当按钮的size为默认时，可省略第三部分，即为“**按钮样式|颜色**”，上边的例子中。更多的详细用法详见[DEMO](http://nanhaibo.com/bbt_tech/panowork/pano_toast)代码。     
　　自此，这个小的krpanoUI库算是完成了，样式虽然不多，但是很实用，今后我也会不断扩充的，各位同学喜欢的话，多多支持哦，支持按钮就在下边，没事多点点哦~~      
　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　
　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　[Github传送门](https://github.com/CiroLee/pano_toast)