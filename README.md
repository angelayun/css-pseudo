## 代码当中的一些设计点
#####  伪类实现盒子阴影
* 留个疑问点，感觉transition支持的属性不包含boxshadow??
* 因为使用伪元素，默认情况下透明度为0，hover的时候透明度为1，这样实质上它还是使用的boxshadow来实现盒子阴影，但是这种方式没有重绘消耗，性能要好些
* 再加之使用了will-change:opacity，让GPU和硬件加速
```
will-change: opacity;
```
##### 伪类实现导航栏
* 用伪类及相邻选择符实现各菜单间的斜线
```
    ul.breadcrumb li+li:before {
      padding: 8px;
      color: black;
      content: "/\00a0";
    }
```
##### 伪类实现的按钮动画效果
* 第一个按钮：外层容器的before实现按钮上边框效果，after实现按钮的下边框，里层容器的before实现按钮的左边框，里层容器的after实现按钮的右边框
* 第一个按钮动画效果：上边框向上移动10px，下边框向下移动10px，左右边框分别宽度变成100%（其实也是可以变成50%，但是最好是大于50%，这样不会视觉上看出间隙）
* 第二个按钮使用before伪类盖在左边框上，当hover的时候，width变化即可

##### 伪类实现三角形
* 事实上就是利用boder来实现三角形
* 需要注意的一个点就是，由于放三角形的容器加了position:relative，不仅仅可以使得三角形容易定位，而且使得不管它的html位置在前面还是后面都不会被相邻的图片遮挡

##### 伪类实现图片三角形
* 思路就是底部三角形容器背景色为底部相邻容器的背景颜色，左边div skewX后变成一个菱形，同理右边也是,调整位置即可
* 箭头容器absolute,before及after伪类都是absolute 最点睛笔是bottom 100% 和background-color为inherit
* 里面左右div宽度都是80%，左div right 80%  这个位置似乎我还没想明白是怎么算出来的

###### 伪元素和平移（translate）变换实现的提示框
* 正常情况下，容器是relative并且overflow为hidden，当hover的时候为visible
* tooltip其实是一直存在，只是在正常情况下被hidden不显示罢了
* tooltip为了有动画效果，在正常情况下opacity为0，只有hover的时候opacity才为1

###### 伪元素构建的文章水印背景
* 其实就是用的伪类的content再加上具体颜色，以及配合当前容器的背景色来实现的


#####  index.html  伪元素实现类似word文档的目录
* 将页码设置为html的属性data-page,然后用伪类after显示出来，其中order设置它的位置
```
a::after {
  order: 2;
  content: "p." attr(data-page);
}
```
* 使用伪类before显示目录当中的...，其中order设置...显示的位置  order不设置的话默认为0，也就是第一个显示，flex设置显示的模式
```
a::before {
  height: .1em;
  flex: 1 1 auto;
  order: 1;
  background: left bottom/contain repeat-x url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA3IDIiPjxjaXJjbGUgZmlsbD0iI2ZmZiIgY3g9IjMuNSIgY3k9IjEiIHI9IjEiLz48L3N2Zz4=);
  content: '';
}
```

