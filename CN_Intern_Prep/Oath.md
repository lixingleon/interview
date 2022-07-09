# 基于session的认证

1. 客户端首次将认证信息发送给服务端
2. 服务端生成session，并返回包含sessionid的cookie返回给客户端
3. 客户端保存cookie
4. 客户端再次请求服务端时，把包含sessionid的cookie带着
5. 服务端通过校验sessionid让用户登录

# 基于token的认证

1. 客户端首次将认证信息发送给服务端
2. 服务端生成token，并把token返回给客户端
3. 客户端保存token到cookie或localStorage
4. 客户端再次请求服务端时，把token带着
5. 服务端通过校验token，让用户登录

# Oauth2.0

作用：给第三方应用授权。

流程：

1. 用户访问客户端，后者将前者导向认证服务器。
2. 用户给予客户端权限。（扫码）
3. 认证服务器生成access code，附在客户端事先设置的回调URI上，将用户导向回调URI。
4. 客户端通过URL收到了access code， 在后台向认证服务器请求token
5. 认证服务器核对access code和URI，确认无误后，向客户端发送token
6. 客户端拿到token，向资源服务器请求资源。

优点：

可以只授权部分功能给第三方应用，且便于管理。

一定要access code吗：

能不能直接将token作为回调URL的一部分呢？不能，因为这样很不安全。

能不能直接把token发给客户端后台呢？不能，这样用户就一直留在认证界面了。

reference:

https://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html