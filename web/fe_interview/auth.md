## 认证、授权、凭证

1.  认证、授权、凭证

    1.  认证是 authentication，指用户当前的身份，当用户登录过系统后便能追踪到它的身份作出符合响应业务逻辑的操作。

    2.  授权是 authorization，指何种身份被允许访问某些资源，在获取到用户身份后继续检查用户的权限。

    3.  实现认证和授权的基础是需要一种媒介 —— 凭证 credientials，来标记访问者的身份或者权限。

<!-- -->

1.  HTTP Basic Authentication

    1.  基本概念：

        1.  组合用户名、密码然后 Base64 编码

        2.  给编码后的字符串添加 Basic 前缀，然后设置名称为 Authorization 的 header 头部

    <!-- -->

    1.  细节：

        1.  用户名密码使用冒号连接，两者中故不能出现冒号

        2.  为了防止用户名密码中出现超出 ASCII 码范围的字符，推荐使用 UTF-8 编码

        3.  将上面的字符串使用 Base 64 编码

        4.  在 HTTP 请求头中加入 Basic\[空格\]编码后的字符串

    <!-- -->

    1.  性质：

        1.  实现简单

        2.  如果明文在网络传输中泄露，必须通过修改密码的方式保证安全。

<!-- -->

1.  HMAC（AK/SK）认证

    1.  概念：

        1.  生成 access key 和 secure key，通过签名的方式完成认证需求，避免传输 secure key。

        2.  利用散列的消息码

    <!-- -->

    1.  流程：

        1.  客户端预先在认证服务器中设置 access key 和 secure key。

        2.  在调用 api 时，客户端需要对参数和 access key 进行自然排序后并使用 secure key 进行签名生成一个额外的参数 digest。

        3.  服务器根据预先设置的 secure key 进行同样的摘要计算，并要求结果完全一致。

        4.  secure key 不能在网络中传输，以及在不受信任的位置存放。

    <!-- -->

    1.  避免重放：

        1.  质疑/应答算法

            1.  客户端先请求一次服务器，获得一个 401 未认证的返回，并得到一个随机的字符串 nonce。

            2.  将 nonce 附加到以上方法进行 HMAC 签名。

            3.  服务器对 nonce 进行校验。

        <!-- -->

        1.  基于时间的一次性密码认证：

            1.  利用时间戳作为验证的时间窗口。

            2.  客户端服务器共享密钥然后根据时间窗口和 HAMC 算法算出相同的验证码。

<!-- -->

1.  Oauth 2 和 Open ID

    1.  概述：

        1.  Oauth 是一个开放标准，允许用户授权第三方网站访问他们存储在另外服务器者上的信息，而不需要提供用户名密码。

        2.  Oauth 是一个授权标准，而非认证标准；资源服务器不需要准确直到确切的用户身份 session，只需要验证授权服务器授予的权限。

    <!-- -->

    1.  组成：

        1.  验证 access token：

            1.  方式一：在完成授权流程后，资源服务器使用 Oauth 服务器提供的 Introspection 接口来验证 access token，Oauth 服务器会返回 access token 的状态和过期时间。

            2.  方式二：使用 JWT 验证。授权服务器使用私钥签发 JWT 形式的 access token，资源服务器需要使用预先配置的公钥检验 JWT Token，并得到 token 状态和一些包含在 token 中的信息。

        <!-- -->

        1.  refresh token 和 access token

            1.  服务器会在第一次授权时返回 access token 和 refresh token，后刷新 access token 时只会需要 refresh token。

            2.  access token 用来客户端和资源服务器之间的交互，refresh token 和授权服务器之间的交互。

            3.  保证 refresh token 泄露，系统仍然相对安全，因为需要 secure key 验证。

    <!-- -->

    1.  Oauth、Open ID、Open ID Connect

        1.  Oauth 负责解决分布式系统之间的授权问题。

        2.  Open ID 解决分布式系统之间的身份认证问题。

        3.  Open ID Connect 是在 Oauth 这套体系下的用户认证问题，实现的基本原理是将 ID 作为资源处理。

<!-- -->

1.  JWT

    1.  概念：

        1.  JWT 是一种包含令牌/值令牌 —— 关联到 session 上的 hash 值叫做引用令牌。

    <!-- -->

    1.  组成：

        1.  JWT Token = part1.part2.part3

        2.  header json 的 base 64 为令牌的第一部分

        3.  payload json 为 base 64 为令牌的第二部分

        4.  拼装第一、第二部分编码后的 json 以及 secret 进行签名的令牌为第三部分

    <!-- -->

    1.  特点：

        1.  只需要签名的 secret key 就能够校验 JWT 令牌

        2.  只需要从消息体中读取授权需要的信息，不需要外部存储

        3.  使用了加密算法，修改的内容无法通过验证

        4.  JWT Token 的部分，不是加密，不应当存放敏感信息

        5.  JWT Token 自包含，无法被撤回

        6.  JWT 的签名算法可以自己拟定

            1.  本地调试可以使用对称算法，便于调试

            2.  生产环境使用非对称加密算法，保证安全

    <!-- -->

    1.  性质：

        1.  在微服务系统中优势突出，多层的 API 调用中，可以直接传输 JWT Token，利用自包含特性，减少外部查询次数。

        2.  JWT 撤回困难在对撤销 Token 要求很高的系统中不适用。

        3.  负载可能过长，在短消息和流数据等应用中不适合使用。

<!-- -->

1.  Cookie、Token in Cookie、Session Token 依然被使用

    1.  以下种类的认证仍然被使用：

        1.  使用 Cookie，在 web 项目中的 ajax 方式

        2.  session ID 或者 hash 作为 token，但 token 放入 header 中传递

        3.  将生成的 token 放入 cookie 传递，利用 HTTPSonly 或者 Secure 标签保护 Token

<!-- -->

1.  选择合适的认证方式

    1.  终端用户参与的通信，需要更高的安全性。并且由于客户端不受控制，由于无法自主分发密钥，认证通信的安全高度依赖 HTTPS。

    2.  第一方服务端之间可以使用基础认证，减小负载。

    3.  第一方和第三方服务器之间也需要高安全性认证。

>  
