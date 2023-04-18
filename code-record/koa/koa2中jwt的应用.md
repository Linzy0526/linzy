# koa2 中 jwt 的应用

> JWT（JSON Web Token）实现用户认证和授权的代码示例。JWT 是一种轻量级的安全令牌，用于在客户端和服务器之间传递身份信息。
>
> JWT 的优点是可以避免在服务器端保存用户的登录状态，降低了服务器的压力和复杂度。

### 安装所需依赖

`npm install koa koa-jwt jsonwebtoken`

### 实现认证和授权

#### 生成 JWT

```typescript
import * as jwt from "jsonwebtoken";

const JWT_SECRET = "your jwt secret"; // 秘钥
const JWT_TIME = "1h"; // 有效时长
jwt.sign({ username: "user123" }, JWT_SECRET, {
  expiresIn: JWT_TIME,
});
```

在上面的代码中，定义了一个 secret 来加密 JWT 的内容。使用 jwt.sign 方法生成 JWT，其中第一个参数是要加密的信息（在这里是{ username: 'user123' }），第二个参数是我们的 secret，第三个参数是一些选项，如过期时间等。这将生成一个 JWT

#### 验证 JWT

##### 正常验证方式

在 Koa2 中验证 JWT。使用 koa-jwt 中间件来实现这一点

```typescript
import Koa from "koa";
import * as jwt from "jsonwebtoken";
import jwtKoa from "koa-jwt";

const app = new Koa();

const secret = "mysecret";

app.use(jwtKoa({ secret }));

app.use((ctx) => {
  const { username } = ctx.state.user;
  ctx.body = `Hello, ${username}!`;
});

app.listen(3000);
```

##### 装饰器模式

装饰器模式表示 koa 引入了 routing-controller 业务层的路由都是通过装饰器的写法去注入，所以 jwt 也是通过装饰器去注入。主要是在@UseBefore 去注入 JWT

```typescript
const authMiddleware = jwtKoa({ secret: CONSTANTS.JWT_SECRET }).unless({
  path: [/^\/public/],
});

@JsonController("/user-invite-record")
@Service()
@UseBefore(authMiddleware)
export class UserInviteRecordController {
  @Post("/create")
  create(@HeaderParams("Authorization") token: string) {
    // 获取http中header的token并解析
    const authUser = verifyToken(token);
  }
}

@JsonController("/user")
@Service()
export class UserController {
  @Post("/create")
  create() {}

  @Post("/detail")
  @UseBefore(authMiddleware) // 也可以在函数装饰器单独对某个路由做校验
  detail(@HeaderParams("Authorization") token: string) {}
}
```

#### 客服端验证

JWT 主要是获取 Http 的 Header 中 Authorization 的 token 值去验证合法性。

```typescript
axios.get("/create", {
  headers: { Authorization: `Bearer ${token}` },
});
```

注：token 值必须有`Bearer `前缀

如果过期的话，http 的 status 直接返回 401
