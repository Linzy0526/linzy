# YYYY 年 MM 月 DD 日

> weather: 🌧️
>
> emoji: 😅

### 问题记录

1. 微信小程序开发中不支持?.语法

   在微信小程序的开发过程中，在开发环境时支持?.语法，但是在提交代码到微信小程序后台审
   核时，会报语法异常

   解决：目前暂无解决方案，尽量避免使用不支持的语法

2. 微信小程序 scrow-view 内容未撑满，右边的滚动条仍然显示

   原因：scrow-view 元素内容子元素设置 margin 导致了内部异常的**高度塌陷**

   解决：在子元素最前端添加一段解决高度塌陷的元素

   ```xml
   <scroll-view scroll-y="{{true}}">
      <view style="content: '';overflow: hidden;" />
      <view class="list-item" wx:for="item in list"></view>
   </scroll-view>
   ```

3. 微信小程序 image 图片资源无法加载问题

   原因：开发环境和生产环境的资源链接解析不一致，导致开发能正常显示，生产无法正常显示

   图片资源链接例子：`/images//Home/banner-img/HomeCover01.jpg`

   解决：`/images/Home/banner-img/HomeCover01.jpg`

   注：生产环境的图片资源链接避免资源链接书写不规范

### 学习记录

这里写今天学习记录，可以包括阅读的技术文档、代码、生活。
