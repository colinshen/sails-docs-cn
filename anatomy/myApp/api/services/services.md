# myApp/api/services
### 作用
该文件夹存放应用所需的service（服务）。‘service（服务）’和控制器中的方法很像，但是它通常用与不必要发生的当用户发送一个请求和服务器返回一个响应 的时间点上。任何不依赖与'.req()'和'.res()'的逻辑都可以成为一个servic（服务）.在没有其他原因的情况 下，尽可能的保证你的控制器的干净性和可管理性。

设想我们创建一个service（服务）用于：
- 发送Email
- 自动tweets名人
- 从第三方API中检索数据，然后发送这些数据到客户端当服务已完成（通过websockets）
 
在该文件夹中，service（服务）被写成一个或多个.js文件。


### Example Email.js

```
module.exports = {
	send: function(to,from,body){
		// fancy code that sends an email
	}
}

```

You would call this service with ` Email.send('rick','bill','lol') `


> Mind your case.  Email.send !== email.send


<docmeta name="uniqueID" value="servicesmd572453">
<docmeta name="displayName" value="services">

