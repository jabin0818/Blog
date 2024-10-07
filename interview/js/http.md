### CORS简单请求与复杂请求

cors跨域

cors 是一个W3C标准,全称是"跨域资源共享"（Cross-originresource sharing），它允许浏览器向跨源服务器发送XMLHttpRequest请求，从而克服了 AJAX 只能同源使用的限制

cors 需要浏览器和服务器同时支持，整个 CORS通信过程，都是浏览器自动完成不需要用户参与，对于开发者来说，cors的代码和正常的 ajax 没有什么差别，浏览器一旦发现跨域请求，就会添加一些附加的头信息

但是，cors不支持ie10及以下版本。

 

简单请求和复杂请求

浏览器将cors请求分为简单请求和复杂请求。

简单请求则直接发送请求到服务器，只请求一次。

而复杂请求在正式请求前都会有预检请求，在浏览器中都能看到有OPTIONS请求，用于向服务器请求权限信息的，需要请求两次。

 

简单请求

简单请求必须要同时满足下面三个条件：

1.    请求方式只能是：GET、POST、HEAD

2.    HTTP请求头限制这几种字段：                                     Accept、Accept-Language、Content-Language、Content-Type、Last-Event-ID

3.    Content-type只能取：application/x-www-form-urlencoded、multipart/form-data、text/plain