Struts2中dispatcher、chain、redirectAction等返回结果的区别
-----------------------------

&emsp;&emsp;Action处理完用户请求后，返回一个普通字符串，整个普通字符串就是一个逻辑视图名。通过配置逻辑视图名和物理视图资源之间的映射关系，系统在收到Action返回的逻辑视图名后就会把响应的物理视图资源呈现给浏览者。

&emsp;&emsp;对于返回结果，需要在struts.xml的<result .../>元素中配置。result元素有两个任务：
* 提供一个逻辑名，一个action返回类似"success"或"error"，而任何其他细节信息并不知道
* 提供一个result type，默认为dispatcher，另外可选的结果有redirect、redirectaction、action。
>1. 跳转方式：dispatcher和chain是服务器端跳转，所以客户端只发起一次请求，产生一个action；redirect和redirectAction是客户端跳转，所以客户端发起两次请求。
>2. 跳转内容:dispatcher和redirect跳转的是views（一般是jsp页面）；chain和redirectAction跳转的是一个action。


|**名称**|**说明**|
|:--|:-|
|dispatcher|将请求forward（转发）到指定JSP|
|chain|处理Action链|
|redirect|重定向至一个URL|
|redirectaction|重定向至一个Action|
|freeMarker|处理freeMarker模板|
|httpHeader|控制特殊的Http行为|
|steam|向浏览器发送InputStream对象，用来处理文件下载|
|velocity|处理Velocity模板|
|xslt|处理XML/XLST模板|
|plaintext|显示原始文件内容|
|tiles|结合Tile使用|


<br>

https://blog.csdn.net/sosous/article/details/40950203
