# 什么情况下算跨域  
两个URL的协议，域名，IP，端口有任意一个不同认为跨域了
gray.ubank365.com - ahtest.ubank365.com 跨域，跟域名一致但是子域名不同
两者之间无法直接通过ajax请求获取数据
两者之间不能通过js互相操作

# 常见跨域解决方案  
## JSONP  
跨域情况下是不能通过JS或者AJAX请求直接获取到数据的，但是在html中img，iframe和script可以通过src属性请求到其他服务器上的数据。JSONP实质就是在html中动态添加script标签，通过设置src属性获取非同源网站的执行脚本。
只支持get请求。

crossorigin="anonymous"不需要验证，不能携带cookie  
当使用验证模式的时候  
![crossorigin="use-credentials"](https://github.com/Namicici/web-tech/blob/master/cross-domain/script.png)  
这个时候需要后端（代理）设置允许跨域的配置否则不允许跨域， 验证模式下是允许携带cookie信息的，如果不是use-credentials, cookie信息不被允许发送的。  
![后端设置跨域策略否则不允许跨域](https://github.com/Namicici/web-tech/blob/master/cross-domain/cross.png)   

![后端设置](https://github.com/Namicici/web-tech/blob/master/cross-domain/serverConfig.png)  

## CORS
通过ajax请求的方式实现跨站资源共享，web端请求方式与普通ajax请求一样，但是分简单请求和复杂请求。  
### 简单请求：  
1. 允许请求的方法：GET、HEAD、POST  
2. 除了浏览器自动添加的header外只允许添加一下几种其他的头：  
Accept  
Accept-Language  
Content-Language  
Content-Type (but note the additional requirements below)  
DPR  
Downlink  
Save-Data  
Viewport-Width  
Width  
3. Content-Type允许取以下几种值：  
application/x-www-form-urlencoded  
multipart/form-data  
text/plain  

这种请求不能携带cookie和自定义header信息，使用时注意。  
![这种模式下只需要在后端设置](https://github.com/Namicici/web-tech/blob/master/cross-domain/simpleCors.png)

### 复杂请求  
不属于简单请求范畴的都属于复杂请求。
复杂请求时浏览器先发一个预请求OPTIONS，预请求返回允许的METHOD，ORIGIN和HEADER，如果需要携带cookie(浏览器端设置withCredentials=true)
![OPTIONS方法设置](https://github.com/Namicici/web-tech/blob/master/cross-domain/complexCors.png)  
![接口中设置](https://github.com/Namicici/web-tech/blob/master/cross-domain/simpleCors.png)   

# postMessage  
不同源的两个iframe之间的通信。
比如a.com的iframe可以通过window.postMessage('I am from a.com', 'http://b.com')  
b.com域下可以通过window.addEventListener('message', function(data){})来接受信息。可以参考张善祥写的安全键盘组件

# 注意  
android6.X以及ios7开始手机端坐了安全策略，setAcceptThirdPartyCookies/setAcceptCookie默认值由true改为了false，导致跨域情况下cookie不允许携带, 类似浏览器对cookie设置策略一样![chrome cookie setting](https://github.com/Namicici/web-tech/blob/master/cross-domain/chromeCookie.png)
