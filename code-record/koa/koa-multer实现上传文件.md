# koa-multer 实现上传文件

### 安装

```shell
npm install --save koa-multer
```

### 使用

#### 单文件上传

```javascript
const Koa = require("koa");
const multer = require("@koa/multer");
const app = new Koa();
const upload = multer({ dest: "uploads/" });

app.use(async (ctx, next) => {
  if (ctx.path === "/upload" && ctx.method === "POST") {
    await upload.single("file")(ctx, next);
    ctx.body = "文件上传成功！";
  } else {
    await next();
  }
});

app.listen(3000);
```

在上面的示例中，我们首先引入了 koa 和 @koa/multer 模块，并创建了一个 Koa 应用和一个 multer 实例。然后，我们在应用中定义了一个中间件，用于处理文件上传请求。在该中间件中，我们通过判断请求路径和请求方法来确定是否进行文件上传操作。如果是文件上传请求，则调用 upload.single 方法处理文件上传操作。upload.single 方法接受一个参数 file，表示上传文件的字段名。最后，我们返回一个成功信息来响应客户端。

需要注意的是，使用 multer 中间件上传文件时，文件会被保存到指定的目录下（在本例中是 uploads/ 目录下）。在处理上传文件之前，我们必须确保该目录已经存在，否则会抛出异常。

#### 多文件上传

如果需要上传多个文件，可以使用 upload.array 方法。该方法接受两个参数：name 和 maxCount。其中，name 表示上传文件的字段名，maxCount 表示最大上传文件数。

```javascript
const Koa = require("koa");
const multer = require("@koa/multer");
const app = new Koa();
const upload = multer({ dest: "uploads/" });

app.use(async (ctx, next) => {
  if (ctx.path === "/upload" && ctx.method === "POST") {
    await upload.array("files", 10)(ctx, next);
    ctx.body = "文件上传成功！";
  } else {
    await next();
  }
});

app.listen(3000);
```

### 配置

在使用 koa-multer 时，我们可以通过传递一个对象来进行配置。以下是 multer 配置参数的说明：

#### dest

上传文件的目录（必须是一个已经存在的目录）。如果未设置该参数，则文件不会被保存到磁盘上，而是保存在内存中。

```javascript
const multer = require("koa-multer");
const upload = multer({ dest: "uploads/" });
```

#### fileFilter

选项用于过滤上传的文件。您可以使用以下代码来指定文件过滤器

```javascript
const upload = multer({
  dest: "uploads/",
  fileFilter: function (req, file, cb) {
    if (!file.mimetype.startsWith("image/")) {
      cb(new Error("只能上传图片类型的文件！"));
    } else {
      cb(null, true);
    }
  },
});
```

#### limits

用于指定上传文件的大小限制。您可以使用以下代码指定文件大小限

```javascript
const multer = require("koa-multer");
const upload = multer({
  limits: {
    fileSize: 1024 * 1024, // 1 MB
  },
});
```

#### preservePath

用于保留上传文件的路径。默认情况下，上传文件的路径将被截断，只保留文件名。您可以使用以下代码来保留上传文件的路径

```javascript
const multer = require("koa-multer");
const upload = multer({ preservePath: true });
```

#### storage

用于指定文件的存储方式。默认情况下，文件将被保存在 Node.js 进程的临时文件夹中。您可以使用以下代码指定文件的存储方式

```javascript
const multer = require("koa-multer");
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, "uploads/");
  },
  filename: (req, file, cb) => {
    cb(null, file.originalname);
  },
});
const upload = multer({ storage: storage });
```

在上面的代码中，我们使用 `multer.diskStorage` 函数指定文件的存储方式。`diskStorage` 函数接受一个对象，该对象包含 `destination` 和 `filename` 选项。`destination` 选项用于指定上传文件的目录，`filename` 选项用于指定上传文件的文件名。在上面的代码中，我们将上传的文件保存在 `uploads/` 目录下，并使用原始文件名作为文件名。
