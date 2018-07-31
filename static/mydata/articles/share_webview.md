# 全景拥抱小程序（二）    

在[全景拥抱小程序（一）](https://cirolee.github.io/website/#/article?url=pano_width_mnapp_01)中讲到如何利用小程序的web-view展示第三方页面，以及如何做到页面和小程序的交互。本篇将继续上一篇，来聊一聊如何在小程序中分享带有定制结果的全景页。      

相对于使用web-view单纯展示，分享定制结果就显得复杂一些。但是，只要分解好逻辑关系，再复杂的东西也会变得简单。     

什么是分享带定制结果的全景页呢？这个概念其实并不复杂。一般情况下，你的全景项目会包含很多场景，当用户浏览某一场景时，觉得这个场景不错，想分享给好友，好友打开链接时（这里是小程序），看到的是你当前浏览的场景。具体一点，以上一期的汽车脚垫定制来说，A用户使用小程序定制脚垫，定制了红色-黑色的搭配，然后A把这个web-view分享给好友B,B打开分享的小程序时，看到的是A定制的结果，即红色-黑色的脚垫搭配，而不是初始的第一个场景。这就是带定制结果的分享页，这样的需求在其他客户端也是很常见的。      

既然概念清晰了，那么怎么实现呢？让我们来捋一下逻辑。     

> 用户分享时可获取当前<span class="singlecode">web-view</span>的URL，即在<span class="singlecode">onShareAppMessage</span>回调中返回<span class="singlecode">webViewUrl</span>参数。     
>

```javascript
Page({
  onShareAppMessage(options) {
    console.log(options.webViewUrl)
  }
})
```

首先就是分享了，web-view的分享同其他小程序页面的分享，都是写在<span class="singlecode">onShareAppMessage</span>中。要想分享出去的小程序的web-view的带有定制结果，显然是要带一个参数了，而这个参数，其实页很简单，就是当前场景的索引。Bingo！第一个要点：**携带定制结果参数**。      

然后，如何获取当前场景索引参数呢？显然，只有全景切换时，当前场景的所以才会变化，所以这个参数应该是由**页面传递给小程序**的。即第二个要点。

最后，当分享带有参数的小程序时，别人打开后，怎样就跳转到定制的场景呢？解决这个问题，需要在小程序和全景页面中共同完成。小程序中需要唉onLoad中获取同样的参数，并做处理，这个后边会讲到。      

在全景的主文件<span class="singlecode">tour.xml</span>中有这样一个<span class="singlecode">action</span>：    

```xml
<action name="startup" autorun="onstart">
    if(startscene === null OR !scene[get(startscene)], copy(startscene,scene[0].name); );
    loadscene(get(startscene), null, MERGE);
    if(startactions !== null, startactions() );
</action>
```

这个action是立即运行的，作用就是确定加载的第一个场景是什么。这样，我们就可以把分享携带的参数拿到，作为加载的第一个场景索引了。     

OK，逻辑已经罗列清楚，让我们一个一个解决。

首先，先解决第二条，向小程序传递索引参数。不管三七二十一，先上代码：       

```javascript
//javascript
var postMessageToMnapp = function(){
	var product_code = krpano.get("scene[get(xml.scene)].index");
		wx.miniProgram.postMessage({
			data:{"result":product_code}
		});
};
```



```xml
<!--xml-->
<events name="custom_event" keep="true" devices="html5"
		onnewscene="jscall(postMessageToMnapp())"
	/>
```

代码非常简单，<span class="singlecode">postMessageToMnapp</span>函数里，首先获取当前场景索引，然后利用小程序接口<span class="singlecode">wx.miniProgram.postMessage</span>向小程序发送参数。然后在自定义的event中的中使用这个函数，<span class="singlecode">onnewscene</span>是新场景加载时调用，即每当场景更新时，就会向小程序发送参数。      

ok，发送者任务已经完成，然后，接收参数就是小程序的事了。而且坑也在这~~。根据官方文档，web-view有属性叫做：<span class="singlecode">bindmessage</span>，当网页向小程序postMessage时，会在**特定时机**收到消息，注意是**特定时机**，即小程序后退、组件销毁、分享时触发并收到消息。不过还好，我们是做分享，不用担心啦~！      

小程序中使用如下：       

```html
<!--index.wxml-->
<web-view src="{{src}}" bindmessage="msgHandler"></web-view>
```

```js
//index.js
data: {
    src:"",
    msgstr:""
  
  },
  //处理页面postMessage传参
  msgHandler(msg) {
    let msgstr = msg.detail.data.pop();
    this.setData({msgstr:msgstr.result});
    
  }
```

当在触发分享时，会调用<span class="singlecode">msghandler</span>，将传入的参数存储在data中。     

小程序分享。官方文档这样描述：

<span class="singlecode">onShareAppMessage</span>中有一个自定义转发字段    

| 字段     | 说明                                                         | 默认值                                | 最低版本 |
| -------- | ------------------------------------------------------------ | ------------------------------------- | -------- |
| title    | 转发标题                                                     | 当前小程序名称                        |          |
| path     | 转发路径                                                     | 当前页面path，必须是以/开头的完整路径 |          |
| imageUrl | 自定义图片路径，可以是本地文件路径、代码包文件路径或者网络图片路径，支持PNG及JPG，不传入 imageUrl 则使用默认截图。显示图片长宽比是 5:4 |                                       | 1.5.0    |

需要注意，web-view会铺满整个页面，并覆盖其他组件，所以触发分享只能通过右上角的 ***三个点*** 来触发。        

```js
 onShareAppMessage:function(e){
    //未使用接口
    let url = e.webViewUrl.split("?")[0];   //获取不带参数的url
    return {
      title:"xxx",
      imageUrl:"xxx.jpg",
      path: `/pages/vrview/index?src=${url}&msgstr=${this.data.msgstr}`
    };    
  }
```

path参数是对应web-view页的路径和拼接的参数，当打开分享的小程序时，进入的就是path的路径，并且携带了参数（这里参数包括src和全景索引参数msgstr）具体的路径参数的使用可以参考小程序官方文档。     

在小程序web-view页的onLoad生命周期函数中写入如下代码：     

```javascript
onLoad: function (option) {
    if(option.msgstr){    //携带参数
      let src = `${option.src}?msgstr=${option.msgstr}`
      this.setData({
        src:src
      });
    }
    else{    //不携带参数
      this.setData({
        src: option.src
      });
    };
  },
```

这里需要区分一下，进入全景页除了通过分享定制结果进入外，还有从首页的具体产品进入，当从首页进入时，时不携带索引参数的。    

接下来就是页面获得索引参数了。因为我们在生命周期函数onLoad中处理了src，并赋值给web-view，所以页面获得参数的时间是晚于小程序的，这样就不会造成获取参数失败的问题。那么页面如何获取参数呢？这个可以通过获取当前的网址来截取参数：   

```xml
<!--小程序获取url参数-->
	<action name="get_option" type="javascript" ><![CDATA[
		var url = window.location.href;
		if(url.indexOf("msgstr") > -1){
			var startindex = url.split("msgstr=")[1];
			krpano.set("skin_settings.startindex",startindex);
		}
	]]>
	</action>
```

根据我的经验，把这段代码写在<span class="singlecode">tour.xml</span>中执行会更快一些，因为如果写在js文件中，js文件必须放在tour.js后，这样势必会增加等待时间。然后，还需要在<span class="singlecode">skin_settings</span>的标签内增加一个全局参数，用来存储参数，因为krpano的action是没有返回值的。最后，需要修改startup这个action中的内容：     

```xml
<action name="startup" autorun="onstart">
		get_option();
		delayedcall(0.1,
			set(firstscene,get(scene[get(skin_settings.startindex)].name));
			loadscene(get(firstscene), null, MERGE);
		);
		if(startactions !== null, startactions() );
		
	</action>
```

​          

至此，整个定制结果分享的小程序功能就已经完成了。最后，我们再来梳理一下。      

1. 全景场景切换时，向小程序传递当前索引参数；

2. 小程序触发分享时，获取参数，用获取的参数拼接跳转路径；

3. 生命周期onLoad中识别参数，全景网页通过地址截取参数，渲染页面。       


