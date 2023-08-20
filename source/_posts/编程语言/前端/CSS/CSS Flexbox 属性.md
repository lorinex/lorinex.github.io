---
categories:
  - 编程语言
  - 前端
  - CSS
---
|display|规定用于HTML元素的盒类型。|
| -----------------| ------------------------------------------------------------------------------------|
|flex-direction|规定弹性容器内的弹性项目的方向。|
|justify-content|当弹性项目没有用到主轴上的所有可用空间时，水平对齐这些项目。|
|align-items|当弹性项目没有用到主轴上的所有可用空间时，垂直对齐这些项。|
|flex-wrap|规定弹性项目是否应该换行， 若一条flex线上没有足够的空间容纳它们。|
|align-content|修改flex-wrap属性的行为。与align-items相似， 但它不对齐弹性项目， 而是对齐flex线。|
|flex-flow|flex-direction和flex-wrap的简写属性。|
|order|规定弹性项目相对于同一容器内其余弹性项目的顺序。|
|align-self|用于弹性项目。覆盖容器的align-items属性。|
|flex|flex-grow、flex-shrink以及flex-basis属性的简写属性。|

[CSS Flexbox (w3school.com.cn)](https://www.w3school.com.cn/css/css3_flexbox.asp)

用来对齐的工具

如何如何利用FlexBox实现垂直居中？

完美的居中

```css
.flex-container {
  display: flex;
  height: 300px;
  justify-content: center;
  align-items: center;
}
```

垂直居中

**align-items** 属性用于垂直对齐 flex 项目。

```css
.flex-container {
  display: flex;
  height: 200px;
  align-items: center;
}
```
