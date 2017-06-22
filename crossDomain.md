###什么情况下算跨域  
两个URL的协议，域名，IP，端口有任意一个不同认为跨域了
gray.ubank365.com ahtest.ubank365.com 跨域，跟域名一致但是子域名不同
两者之间无法直接通过ajax请求获取数据
两者之间不能通过js互相操作

###常见跨域解决方案  
####JSONP  
跨域情况下是不能通过JS或者AJAX请求获取到数据的，但是在html中img，iframe和script可以通过src属性请求到其他服务器上的数据。JSONP实质就是在html中动态添加script标签，通过设置src属性获取非同源网站的执行脚本。
只支持get请求。
credentials的有nginx的情况下如何配置？？？？？？

####CORS  



####注意  
android6.X以及ios7开始手机端坐了安全策略，acceptThirdPartySetCookies默认值由true改为了false，导致跨域情况下cookie不允许携带



