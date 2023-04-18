# YYYY 年 MM 月 DD 日

> weather: ⛅️
>
> emoji: 😅

### 学习记录

#### 1.koa2 中 jwt 的使用

今日项目主要是 koa2 + routing-controller 搭建的项目，应用 jwt 的方式也是通过装饰器去校验权限

##### 1.创建一个 auth 中间建

```typescript
// configs/auth.middleware.ts
import jwtKoa from "koa-jwt";
import * as jwt from "jsonwebtoken";
import CONSTANTS from "./constants";
import { User } from "@prisma/client";

export const authMiddleware = jwtKoa({ secret: CONSTANTS.JWT_SECRET }).unless({
  path: [/^\/public/],
});

export function createToken(payload: any): string {
  return jwt.sign(payload, CONSTANTS.JWT_SECRET, {
    expiresIn: CONSTANTS.JWT_EXPIRE_TIME,
  });
}

export function verifyToken(
  token: string
): Pick<User, "id" | "cellphone" | "wxOpenId"> {
  try {
    return jwt.verify(token.replace("Bearer ", ""), CONSTANTS.JWT_SECRET);
  } catch (err) {
    return null;
  }
}
```

#### 2.在 controller 业务层中通过装饰器注入到路由中

```typescript
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

#### 3.客户端需要在 header 中添加 token

``` typescript
headers: { Authorization: `Bearer ${token}`}
```

[koa2中jwt的应用](https://github.com/Linzy0526/linzy/tree/master/code-record/koa/koa2中jwt的应用.md)
