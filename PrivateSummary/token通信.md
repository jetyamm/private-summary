#基于 Token 的身份验证方法
使用基于 Token 的身份验证方法，在服务端不需要存储用户的登录记录。大概的流程是这样的：

1、客户端使用用户名跟密码请求登录
2、服务端收到请求，去验证用户名与密码
3、验证成功后，服务端会签发一个 Token，再把这个 Token 发送给客户端
4、客户端收到 Token 以后可以把它存储起来，比如放在 Cookie 里或者 Local Storage 里
5、客户端每次向服务端请求资源的时候需要带着服务端签发的 Token
6、服务端收到请求，然后去验证客户端请求里面带着的 Token，如果验证成功，就向客户端返回请求的数据

Token生成之后，客户端每次访问时，都要带上这个字符串。在后端对Token做验证，验证方式就是将Token字符串按照既有方式解密，然后判断Token中存储的信息，包括用户UID，过期时间等，是否符合条件。