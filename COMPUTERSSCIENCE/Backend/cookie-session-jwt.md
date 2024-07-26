# Cookie - Session 认证模式

在 **Web** 应用发展的初期，大部分采用基于 **Cookie-Session** 的会话管理方式，逻辑如下

* 客户端使用用户名、密码进行认证
* 服务器验证用户名、密码正确后生成并存储 **Session**，将 **SessionID** 通过 **Cookie** 返回给客户端
* 客户端访问需要认证的接口时在 **Cookie** 中携带 **SessionID**
* 服务端通过 **SessionID** 查找 **Session** 并进行鉴权，返回给客户端需要的数据

基于**Session**的方式存在多种问题

* 服务端需要存储**Session**，并且由于**Session**需要经常快速查找，通常存储在内存或内存数据库中，同时在线用户较多时需要占用大量的服务器资源
* 当需要扩展时，创建**Session**的服务器可能不是验证**Session**的服务器，所以还需要将所有**Session**单独存储并共享
* 由于客户端使用**Cookie**存储**SessionID**，在跨域场景下需要进行兼容性处理，同时这种方式也难以防范**CSRF**攻击



# Token 认证模式

鉴于基于 **Session** 的会话管理方式存在上述多个缺点，基于 **Token** 的无状态会话管理方式诞生了，所谓无状态，就是服务器可以不再存储信息，甚至是不再存储 **Session**，逻辑如下

* 客户端使用用户名、密码进行认证
* 服务端验证用户名、密码正确后生成 **Token** 返回给客户端
* 客户端保存 **Token**，访问需要认证的接口时在 **URL** 参数或 **HTTP Header** 中加入 **Token**
* 服务端通过解码 **Token** 进行鉴权，返回给客户端需要的数据



# JWT

是 **JSON Web Token** 的缩写，是为了在网络应用环境间传递声明而执行的一种基于 **JSON** 的开放标准

它是由`.`分隔的三部分组成，依次是

* 头部（Header）
* 负载（Payload）
* 签名（Signature）

头部和负载以**JSON**形式存在，这就是**JWT**中的**JSON**，三部分的内容都分别单独经过了**Base64**编码，以`.`拼接成一个**JWT Token**

## Header

**JWT**的**Header**中存储了所使用的加密算法和**Token**类型

```json
{
    "alg": "HS256",
    "typ": "JWT"
}
```

## Payload

**Payload** 表示负载，也是一个**JSON**对象，**JWT**规定了**7**个官方字段供选用

```
iss (issusr): 签发人
exp (expiration time): 过期时间
sub (subject): 主题
aud (audience): 受众
nbf (Not Before): 生效时间
iat (Isuued At): 签发时间
jti (JWT ID): 编号
```

除了官方字段，开发者也可以自己指定字段和内容

```json
{
    "sub": "123456789",
    "name": "John Doe",
    "admin": true
}
```

注意，**JWT**默认是不加密的，任何人都可以读到

## Signature

**Signature**部分是对前两部分的签名，防止数据篡改

首先，需要指定一个密钥啊（secret）。这个密钥只有服务器才知道，不能泄露给用户。然后，使用**Header**里面指定的签名算法（默认是**HMAC SHA256**），按照下面的公式产生签名

```
HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)
```





# 基于JWT实现认证实践

在**JWT**的实践中，引入**Refresh Token**，将会话管理流程改进如下

* 客户端使用用户名密码进行认证
* 服务端生成有效时间较短的**Access Token**，和有效时间较长的**Refresh Token**也失效了，用户就只能重新登录了

在**JWT**的实践中，引入**Refresh Token**，将会话管理流程改进如下

* 客户端使用用户名密码进行认证
* 服务端生成有效时间较短的**Access Token**，和有效时间较长的**Refresh Token**
* 客户端访问需要认证的接口是，携带**Access Token**
* 如果**Access Token**没有过期，服务端鉴权后返回给客户端需要的数据
* 如果携带**Access Token**访问需要认证的接口时鉴权失败，则客户端使用**Refresh Token**向刷新接口申请新的**Access Token**
* 如果**Refresh Token**没有过期，服务端向客户端下发新的**Access Token**
* 客户端使用新的**Access Token**访问需要认证的接口
