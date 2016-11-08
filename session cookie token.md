# session cookie token的区别

## cookie

> cookie是保存在本地终端的数据。cookie由服务器生成，发送给浏览器，浏览器把cookie以kv形式保存到某个目录下的文本文件内，下一次请求同一网站时会把该cookie发送给服务器。由于cookie是存在客户端上的，所以浏览器加入了一些限制确保cookie不会被恶意使用，同时不会占据太多磁盘空间，所以每个域的cookie数量是有限的。

> cookie的组成有：名称(key)、值(value)、有效域(domain)、路径(域的路径，一般设置为全局:"\")、失效时间、安全标志(指定后，cookie只有在使用SSL连接时才发送到服务器(https))。下面是一个简单的js使用cookie的例子:

~~~
document.cookie = "id="+result.data['id']+"; path=/";
document.cookie = "name="+result.data['name']+"; path=/";
document.cookie = "avatar="+result.data['avatar']+"; path=/";
~~~
用户登录时产生cookie:

~~~
var cookie = document.cookie;
var cookieArr = cookie.split(";");
var user_info = {};
for(var i = 0; i < cookieArr.length; i++) {
    user_info[cookieArr[i].split("=")[0]] = cookieArr[i].split("=")[1];
}
$('#user_name').text(user_info[' name']);
$('#user_avatar').attr("src", user_info[' avatar']);
$('#user_id').val(user_info[' id']);
~~~

> cookie的缺点：

- cookie的大小和数量是有限制的。

- cookie是有域名限制的，域名一多就会产生管理混乱。

- cookie安全令人担忧，虽然可以通过设置 HttpOnly 属性防止一些私密 Cookie 被客户端访问，但是仍然不能保证 Cookie 无法被篡改。

## session

> 前面已经介绍了 Cookie 可以让服务端程序跟踪每个客户端的访问，但是每次客户端的访问都必须传回这些 Cookie，如果 Cookie 很多，这无形地增加了客户端与服务端的数据传输量，而 Session 的出现正是为了解决这个问题

> session的中文翻译是“会话”，当用户打开某个web应用时，便与web服务器产生一次session。服务器使用session把用户的信息临时保存在了服务器上，用户离开网站后session会被销毁。这种用户信息存储方式相对cookie来说更安全。

> 以数组形式保存用户登录的信息（id、name、avatar），session是全局的，在一个地方保存了，其他地方就可以用。

> 下面详细讲一下 Session 如何基于 Cookie 来工作。实际上有三种方式能可以让 Session 正常工作：

- 基于 URL Path Parameter，默认支持。
- 基于 Cookie，如果没有修改 Context 容器的 cookies 标识，默认也是支持的。
- 基于 SSL，默认不支持，只有 connector.getAttribute("SSLEnabled") 为 TRUE 时才支持。

> session的缺点：

- 如果web服务器做了负载均衡，那么下一个操作请求到了另一台服务器的时候session会丢失。

- session会增大服务端的压力

## cookie和session的区别
1. cookie数据存放在客户的浏览器上，session数据放在服务器上。
2. cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗，考虑到安全应当使用session。
3. session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能，考虑到减轻服务器性能方面，应当使用COOKIE。
4. 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。

## token

> token的意思是“令牌”，是用户身份的验证方式，最简单的token组成:uid(用户唯一的身份标识)、time(当前时间的时间戳)、sign(签名，由token的前几位+盐以哈希算法压缩成一定长的十六进制字符串，可以防止恶意第三方拼接token请求服务器)。还可以把不变的参数也放进token，避免多次查库。

使用基于 Token 的身份验证方法，在服务端不需要存储用户的登录记录。大概的流程是这样的：

1. 客户端使用用户名跟密码请求登录
2. 服务端收到请求，去验证用户名与密码
3. 验证成功后，服务端会签发一个 Token，再把这个 Token 发送给客户端
4. 客户端收到 Token 以后可以把它存储起来，比如放在 Cookie 里或者 Local Storage 里
5. 客户端每次向服务端请求资源的时候需要带着服务端签发的 Token
6. 服务端收到请求，然后去验证客户端请求里面带着的 Token，如果验证成功，就向客户端返回请求的数据
