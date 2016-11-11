# Charles使用

## 抓包(https)

1. 电脑和手机在同一个网域中（iyaya系列，waiguoyueliang系列皆可）
2. 电脑端打开Charles
3. 获取电脑端的IP地址（可以通过cmd中输入ipconfig查看）
4. 手机端设置网络的代理，代理地址填电脑端的IP地址，端口填8888（Charles fiddler 等抓包工具默认端口8888，也可以在设置中修改）
5. 手机端在浏览器中输入http://charlesproxy.com/getssl 下载并安装证书  
**注：一个Charles客户端对应一个手机端的证书，换电脑（Charles客户端）或者手机设备都需要重新安装证书**

## 篡改(Breakpoints)

> 主要用于篡改request请求参数或者response返回内容，达到一些异常场景的测试

1. 在Structure中右键点击需要设置断点的path，在弹出窗口中勾选Breakpoints
2. 在客户端进行请求，就会看到对应path的请求进入了断点模式（向上的红色箭头）
3. 点击Edit request，可以对请求的参数内容等进行修改（注：部分接口需要对参数进行sign校验的话就不太好改了）
4. 点击Execute，发送修改过的请求给服务端
5. 等服务端返回接口内容后，就会直接到断点模式中（向下的红色箭头）
6. 点击Edit response，可以对返回的内容进行修改
7. 点击Execute，发送修改过的返回给客户端

缺点：客户端处理一个请求如果15秒没有返回就会触发重试机制，导致篡改返回的时候速度要快，否则会失败。  
后期这类型的测试考虑通过anyproxy设置rule进行mock。

## 模拟请求

> 右键点击某一个请求可以进行 repeat,edit等请求的重发，如果只需要看服务端返回，不需要客户端接收的话，可以考虑这种

## 限速(Throttle)

> 模拟不同上下行网速，丢包率等进行弱网测试

1. proxy --> Throttle settings --> Enable Throttle