# 【Javascript】——简单cookie操作     
　　cookie是web开发中常用的存储技术，与其类似的还有localStorage、sessionStorage。这三者的差别可以阅读[【掘金】把cookie聊清楚](https://juejin.im/post/59d1f59bf265da06700b0934),这篇文章详细的说明了这三者的异同和使用注意事项。这里来点干货，说说cookie的基本操作：**设置cookie、获取cookie和删除cookie**。       

　　**1.设置cookie。**原生JS可以通过<span class="singlecode">document.cookie</span>来操作cookie。设置cookie主要包含三个参数：*名称、值、过期时间*。我们用setCookie函数来设置cookie。
```
 var setCookie = function(name,value,expiretime){
	        expiretime = expiretime || 30;
	        var date = new Date();
	        date.setTime(date.getTime() + (expiretime*24*60*60*1000));
	        var expires = "expires="+date.toUTCString();
	        document.cookie = name + "=" + value + "; " + expires;
	    }
```
设置cookie非常简单，这里设置了过期时间的单位是天，过期时间的的设置是“当前时间”+过期时间阈值，默认的阈值是30(天)。这里的toUTCString() 方法可根据世界时 (UTC) 把 Date 对象转换为字符串，最后拼接成完整的cookie。     

　　**2.获取cookie。** cookie的获取同样可以通过<span class="singlecode">document.cookie</span>来获取。如：
```
setCookie("name","TOM",30);
console.log(document.cookie);   //name=TOM
```
上边的结果可能会奇怪，为什么设置了过期时间，却没有获取到呢？其实过时时间已经设置好了，可以通过F12 ==>Aplication ==> Cookies中查看。
　　当我们设置多个cookie时，获取结果是怎样呢？
```
setCookie("name","TOM");
setCookie("age",12);
console.log(document.cookie)   // name=TOM;age=12
```
获取的结果是cookie所有cookie的字符串。这样的结果并不令人满意，如果可以通过类似键值对的形式获取就好了。getCookie函数就做了这件事。
```
var getCookie = function(name) {
            var storage = document.cookie;
            storage = storage.split(";");
            var cookieValue = {};
            storage.forEach(element => {
                let temp = element.split("=");
                cookieValue[`${temp[0]}`] = temp[1]
            });

            return name ? cookieValue[name]:cookieValue;
        },
```
首先把所有cookie存储起来，然后将其分割成数组，因为我们想要的到对象的形式，所以定义一个对象作为返回值，遍历storage数组，将每一个cookie依据“=”分割，分割后的每个元素是一个包含名称和值的数组，然后将数组转换成对象返回。这里的返回值会根据传入的name(cookie的“键”)来判断返回的是指定的值还是所有的cookie，也就是说，当传入name参数时，获取的是name名称的cookie的值，省略参数时，获取所有的cookie对象。
```
setCookie("name","TOM");
setCookie("age",12);

console.log(getCookie(name));    // TOM
console.log(getCookie())   // {"name":"TOM","age":"12"}
```
　　**3.删除Cookie。**删除cookie非常简单，只需让对应的cookie的有效期小于当前时间即可。deleteCookie实现的方法与getCookie一样，可以删除指定cookie或者删除全部cookie。
```
var deleteCookie = function (name){
            if(name){
                tool.setCookie(name,"",-1)
            }
            else{
                let cookies = getCookie();
                Object.keys(cookies).forEach(element=>{
                    setCookie(element,"",-1);
                })
            }
        }
```
删除cookie用到了getCookie和setCookie，这里设置cookie为负数，当expires为负数时，表示cookie为临时数据，这样就达到了删除cookie的目的。同样，传入参数，删除指定cookie，省略参数表示删除所有cookie。     

　　到此，简单的cookie操作就完成了。[Github传送门](https://github.com/CiroLee/jsLab)