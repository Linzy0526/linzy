# sharp.js 实现图片压缩

> sharp.js 用于处理图像和进行图像压缩。它提供了一组功能强大的方法，可以帮助开发人员在 Web 应用程序中对图像进行处理和优化。
>
> 英文文档：[sharp.pixelplumbing.com/](sharp.pixelplumbing.com/)
>
> 中文文档：[https://blog.lcddjm.com/sharp-documents-cn/](https://blog.lcddjm.com/sharp-documents-cn/)

### 安装

```shell
npm install sharp
```

安装条件：

- Node v4.5.0+
- 全局安装的 libvips
- 对于当前平台和 node 版本，不存在预编译的二进制文件 或当命令 npm install --build-from-source 执行时
- C++11 稳定版本编译工具例如 gcc 4.8+、 clang 3.0+ 或 MSVC 2013+
- node-gyp 和它的依赖(包括 Python 2.7)

注：[libvips 安装注意事项](https://blog.lcddjm.com/sharp-documents-cn/article/charp2.html#%E6%9D%A1%E4%BB%B6)

### 实现图片压缩

```js
// 根据临时文件，压缩文件并导出
const tempFileUrl = fileRelativeDir + "/" + "temp" + fileName;
await sharp(tempFileUrl)
  .tiff({
    quality: 50,
  })
  .resize(750)
  // 必须要转格式不然浏览器打不开，且建议格式为jpeg,png压缩效果不佳
  .toFormat("jpeg")
  .toFile(fileRelativeDir + "/" + fileName);
```

sharp.js 是根据目录中已有的文件对文件进行压缩或者其他图片的处理，所以在 sharp 的构造函数内传入需要处理的文件路径，通过 tiff 格式中的 quality 可以将图片的质量降低，如需将文件的大小压缩到极致，还可以通过改变图片的尺寸大小 resize 去压缩带下，最后再通过 toFile 方法将压缩后的文件输出到指定目录下。

### sharp.js 常用方法

#### sharp 构造函数

sharp(input | options | 无参数)

- input `Buffer | String` 可以是包含 JPEG、PNG、WebP、GIF、SVG、TIFF 或原始像素图像数据的 Buffer，或包含 JPEG、PNG、WebP、GIF、SVG 或 TIFF 图像文件路径的字符串
- options `Object` 是具有可选属性的对象

一般通过 input 参数，传入文件 buffer 或者文件路径去加载图片

#### 输出文件

##### toFile

toFile(fileOut, callback)

- fileOut (String) 写入图像数据的路径
- callback (Function) 在完成时调用，带有两个参数(err, info)。info 包含输出图像格式、大小(字节)、宽度、高度、通道和预乘(指示是否使用预乘)。在使用裁剪策略时还包含 cropOffsetLeft 和 cropOffsetTop。

将输出图像数据写入文件， 如果没有选择输出格式，将从扩展中推断它，支持 JPEG、PNG、WebP、TIFF、DZI 和 libvip 的 V 格式。注意，原始像素数据只支持缓冲区输出。

##### toBuffer

参数：

toBuffer({ resolveWithObject: true })

resolveWithObject 使用包含 data 和 info 属性的对象解析 Promise，而不是仅使用 data 解析。 \*callback 回调

回调:

toBuffer().then(err , data , info)

- err 一个错误(如果有的话)。
- data 输出的图像数据
- info 包含输出图像格式、大小(字节)、宽度、高度、通道和预乘(指示是否使用预乘)。在使用裁剪策略时还包含 cropOffsetLeft 和 cropOffsetTop。

##### jpeg、png、webp、tiff

输出对应格式的数据

共有属性: quality (Number) 质量，整数 1-100(可选，默认 80)

[jpeg 属性](https://blog.lcddjm.com/sharp-documents-cn/article/charp3.3.html#jpeg)

[png 属性](https://blog.lcddjm.com/sharp-documents-cn/article/charp3.3.html#png)

[webp 属性](https://blog.lcddjm.com/sharp-documents-cn/article/charp3.3.html#webp)

[tiff 属性](https://blog.lcddjm.com/sharp-documents-cn/article/charp3.3.html#tiff)
