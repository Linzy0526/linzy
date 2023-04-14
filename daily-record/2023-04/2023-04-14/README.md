# 2023年04月14日

> weather: ⛅️
>
> emoji: 😀


### 问题记录

1.项目环境Koa+Prisma,项目中UserInviteRecord表中存在inviterId和inviteeId字段, 两字字段都是关联User表id字段

解决：[Prisma 表数据关系记录](https://github.com/Linzy0526/linzy/tree/master/code-record/prisma/02.表数据关联查询.md)  
需要在prisma/schema.prisma模型关联中需要指定关联字段名
``` prisma
// /prisma/schema.prisma
model User {
  id                  Int                @id @default(autoincrement())
  inviteeInviteRecord UserInviteRecord[] @relation("invitee")
  inviterInviteRecord UserInviteRecord[] @relation("inviter")
}

model UserInviteRecord {
  id        Int      @id @default(autoincrement())
  inviterId Int
  inviter   User     @relation("inviter", fields: [inviterId], references: [id])
  inviteeId Int
  invitee   User     @relation("invitee", fields: [inviteeId], references: [id])
}

```





