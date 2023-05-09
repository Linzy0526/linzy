- 微信小程序开发中遇到的问题集合

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

4. 微信小程序点击事件参数获取的问题

   情景：父节点用 bindtap 绑定 FnA 事件，在父节点上绑定所需的属性值

   ```html
   <view class="father" data-value="a" bindtap="FnA">
     <view class="child"></view>
   </view>
   ```

   为了获取节点上绑定的 data,一般我们可以从回调函数参数 event 对象中获取

   event.target.dataset 当前点击元素上的属性，如果是子元素则是子元素上的属性，如果是父节点则是父节点上的属性值

   event.currentTarget.dataset 获取事件绑定元素上的属性值（上面代码会回去父节点上的属性）
