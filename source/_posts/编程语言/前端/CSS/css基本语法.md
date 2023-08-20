---
categories:
  - 编程语言
  - 前端
  - CSS
---
# css基本语法

选择器、声明块



#### 选择器

通过选择器可以选中页面中的指定元素

- 比如`p`的作用就是选中页面中所有的`p`元素声明块

#### 声明块

通过声明块来指定要为元素设置的样式

- 声明块由一个一个的声明组成，声明是一个**名值对**结构
- 一个样式名对应一个样式值，名和值之间以`:`连接，以`;`结尾

```CSS
p{
    color:aquamarine;
    font-size:40px;
}
```






### 通配选择器（Universal selector）

- 作用：选中页面中的所有元素
- 语法：`*`
- 例子：`*{}`

```CSS
*{
    color: red;
}

```


### 元素选择器（Type selector）

也叫类型选择器、标签选择器

- 作用：根据标签名来选中指定的元素
- 语法：`elementname{}`
- 例子：`p{}` `h1{}` `div{}`

```CSS
p{
  color: red; 
}
h1{
  color: green;
}
```


### 类选择器（Class selector）

- 作用：根据元素的class属性值选中一组元素
- 语法：`.classname`
- 例子：`.blue{}`

```CSS
.bule{
    color:blue;
}
.size{
    font-size:20px;
}

```


`class`是一个标签的属性，它和`id`类似，不同的是`class`

- 可以重复使用，
- 可以通过`class`属性来为元素分组，
- 可以同时为一个元素指定多个`class`属性

<p class="blue size"> 类选择器（Class selector）</p>

### ID选择器（ID selector）

- 作用：根据元素的`id`属性值选中一个元素
- 语法：`#idname{}`
- 例子：`#box{}` `#red{}`

```CSS
#red{
    color:red;
}
```


### 属性选择器（Attribute selector）

- 作用：根据元素的属性值选中一组元素
- 语法1：`[属性名]` 选择含有指定属性的元素
- 语法2：`[属性名=属性值]` 选择含有指定属性和属性值的元素
- 语法3：`[属性名^=属性值]` 选择属性值以指定值开头的元素
- 语法4：`[属性名$=属性值]` 选择属性值以指定值结尾的元素
- 语法5：`[属性名*=属性值]` 选择属性值中含有某值的元素
- 例子：`p[title]{}` `p[title=e]{}` `p[title^=e]{}` `p[title$=e]{}` `p[title*=e]{}`

```CSS
p[title]{
    color: orange;
}
p[title=e]{
    color: orange;
}
p[title^=e]{
    color: orange;
}
p[title$=e]{
    color: orange;
}
p[title*=e]{
    color: orange;
}
```


## 4. 复合选择器

### 交集选择器

- 作用：选中同时复合多个条件的元素
- 语法：`选择器1选择器2选择器3选择器n{}`
- 注意点：交集选择器中如果有元素选择器，必须使用元素选择器开头

```CSS
div.red{
    font-size: 30px;
}

.a.b.c{
    color: blue;
}
```


### 并集选择器（选择器分组）

- 作用：同时选择多个选择器对应的元素
- 语法：`选择器1,选择器2,选择器3,选择器n{}`
- 例子：`#b1,.p1,h1,span,div.red{}`

```CSS
h1,span{
    color: green;
}
```


## 5. 关系选择器

- 父元素：直接包含子元素的元素叫做父元素
- 子元素：直接被父元素包含的元素是子元素
- 祖先元素：直接或间接包含后代元素的元素叫做祖先元素；一个元素的父元素也是它的祖先元素
- 后代元素：直接或间接被祖先元素包含的元素叫做后代元素；子元素也是后代元素
- 兄弟元素：拥有相同父元素的元素是兄弟元素

### 子元素选择器（Child combinator）

- 作用：选中指定父元素的指定子元素
- 语法：`父元素 > 子元素`
- 例子：`A > B`

```CSS
div.box > p > span{
    color: orange;
}
```


### 后代元素选择器（Descendant combinator）

- 作用：选中指定元素内的指定后代元素
- 语法：`祖先 后代`
- 例子：`A B`

```CSS
div span{
    color: skyblue;
}
```


### 兄弟元素选择器（Sibling combinator）

- 作用：选择下一个兄弟
- 语法：`前一个 + 下一个` `前一个 ~ 下一组`（选择下面所有兄弟）
- 例子1：`A1 + A2`（Adjacent sibling combinator）
- 例子2： `A1 ~ An`（General sibling combinator）

```CSS
p + span{
    color: red;
}

p ~ span{
    color: red;
}
```


## 6. 伪类选择器

伪类（不存在的类，特殊的类）

伪类用来描述一个元素的特殊状态，比如：第一个子元素、被点击的元素、鼠标移入的元素.…

伪类一般情况下都是使用`:`开头

- `:first-child` 第一个子元素
- `:last-child` 最后一个子元素
- `:nth-child()` 选中第n个子元素
- n：第n个，n的范围0到正无穷
- 2n或even：选中偶数位的元素
- 2n+1或odd：选中奇数位的元素

以上这些伪类都是根据所有的子元素进行排序的

- `:first-of-type` 同类型中的第一个子元素
- `:last-of-type` 同类型中的最后一个子元素
- `:nth-of-type()` 选中同类型中的第n个子元素

这几个伪类的功能和上述的类似，不同点是他们是在同类型元素中进行排序的

- `:not()`否定伪类，将符合条件的元素从选择器中去除

```CSS
/* ul下所有li，黑色 */
ul>li {
    color: black;
}

/* ul下第偶数个li，黄色 */
ul>li:nth-child(2n) {
    color: yellow;
}

/* ul下第奇数个li，绿色 */
ul>li:nth-child(odd) {
    color: green;
}

/* ul下第一个li，红色 */
ul>li:first-child {
    color: red;
}

/* ul下最后一个li，黄色 */
ul>li:last-child {
    color: orange;
}
```


- `:link` 未访问的链接
- `:visited` 已访问的链接
- 由于隐私的原因，所以`visited`这个伪类只能修改链接的颜色
- `:hover` 鼠标悬停的链接
- `:active` 鼠标点击的链接

```CSS
/* unvisited link */
a:link {
  color: red;
}

/* visited link */
a:visited {
  color: yellow;
}

/* mouse over link */
a:hover {
  color: green;
}

/* selected link */
a:active {
  color: blue;
}
```


## 7. 伪元素选择器

伪元素，表示页面中一些特殊的并不真实的存在的元素（特殊的位置）

伪元素使用`::`开头

- `::first-letter` 表示第一个字母
- `::first-line` 表示第一行
- `::selection` 表示选中的内容
- `::before` 元素的开始
- `::after` 元素的最后
- `::before`和`::after` 必须结合`content`属性来使用

```CSS
/* 段落首字母设置大小为30px */
p::first-letter{
    font-size: 30px;
}

/* 段落第一行设置为黄色背景 */
p::first-line{
    background-color: yellow;
}

/* 段落选中的部分变绿色 */
p::selection{
    background-color: green；
}

/* div前加上内容 */
div::before{
    content: 'BEFORE';
    color: red;
}

/* div后加上内容 */
div::after{
    content: 'AFTER';
    color: blue;
}

```






总结

通配选择器 *{}

元素选择器 elementname{}

类选择器 .classname{}

id选择器 #idname{}

交集选择器 选择器1选择器2选择器3选择器n{}

并集选择器 选择器1,选择器2,选择器3,选择器n{}

子元素选择器 父元素 > 子元素

后代元素选择器 祖先 后代

兄弟元素选择器 A+B 选择下一个B兄弟 A~B 选择下面所有B兄弟

