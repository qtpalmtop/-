# CSS
## 1.position有哪些值，作用分别是什么
答：
- static：正常默认文档流，无法通过定位属性修改位置；
- relative: 正常文档流，可以通过定位属性修改位置； 
- absolute：脱离文档流，独立一层默认相对于 \<html>元素或其最近的定位祖先
- fixed：脱离文档流，独立一层相对于浏览器视口本身
- sticky：相对和固定混合，表现和相对一样，直到他滚动到一定某个阈值点
加分项：

[开始学习](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Positioning)
## 2.CSS选择器有哪些（重点）
答：
- 简单选择器：元素选择器，类选择器，ID选择器，通用选择器
- 属性选择器：存在和值属性选择器（精确匹配属性），字串值属性选择器（类正则匹配属性）
- 伪类和伪元素选择器：指定元素的各种状态的某个部分
- 组合器：空格，>，+， ～来组合各种选择器，分别表示所有后代，后代第一个，兄弟第一个，所有兄弟
- 多用选择器：逗号隔开，公用一套css规则

加分项：

[开始学习](https://segmentfault.com/a/1190000013424772)
## 3.什么是BFC，BFC有什么作用，如何形成BFC
答：
- 是什么：Format Context是W3C CSS2.1中的一种规范，他是一块渲染区域，有一套渲染规则，决定其子元素如何定位，和其他元素如的关系和相互作用，BFC指块级格式化上下文，属于正常文档流。
- 作用：1.元素外边距重叠 2.包含浮动元素（清除浮动） 3.阻止元素被浮动元素覆盖
- 如何形成：1.body根元素 2.position: absolute/fixed 3.float:除了none 4.display：table-cells/flex/inline-block 5.overflow:除了visible
加分项：

[开始学习](https://zhuanlan.zhihu.com/p/25321647)
## 4.flex布局有什么好处
答：
- 简便：只需要几个属性搭配就能实现复杂布局
- 完整：支持传统布局拥有的所有布局方案
- 响应式：flex子元素能根据父容器大小适配
- 性能好：同样的布局相比传统布局性能更好
加分项：

[开始学习](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)
## 5.介绍下盒子模型
答：

在CSS中，所有的元素都被一个个盒子包围着，盒子包括外边距、边框、内边距、元素内容
盒子类型有两种
- 标准模型：box-sizing: content-box，标准模型的width=内容宽度 + 内边距 + border
- IE模型：box-sizing: border-box，IE模型的width=内容宽度

如何获取盒子模型宽高：style，currentStyle，ComputedStyle，getBoundingClientRect

盒子包含或并列排列时，外边距会重叠，取的最大的那个元素外边距，如何解决，设置父级或兄弟元素为BFC

BFC：块级格式化上下文，是一个块级渲染区域，有一套渲染规则规定了其元素如何定位，以及和其他元素的关系和相互作用
如何设置BFC
- position：absolute,fixed
- display：table-cell，flex，inline-block；
- overflow: 不是visible
- float：不是none
- body根元素
BFC还解决了包含浮动元素，防止浮动元素覆盖其他元素，总的来说就是让块中的内容和块外的内容互不关联

加分项：BFC是什么，解决了什么，怎么设置

[开始学习](https://segmentfault.com/a/1190000013069516)
## 6.有哪些方式可以使div居中
答：
- flex: display:flex;align-items:center;justify-content:center;
- grid：display:grid;align-items:center;justify-items：center;
- position: absolute;元素margin-left，margin-top自身宽高50%
- position: absolute;元素transform:translate自身宽高50%
- position:absolute;元素已知宽高，top,left,bottom,right:0,margin:auto；
- table-cell；display:table-cell;vertical-align: center;
加分项：

[开始学习](https://juejin.cn/post/6844903821529841671)
## 7.css优先级是怎么计算的
答：
!important 无限大 > 行内样式1000 > id选择器100 > class选择器10 > 标签选择器1 > 通用选择器 0

如果样式指向同一个元素，权重规则是根据组合器中所有选择器权重的和来计算的
- 1.权重规则生效，权重大的优先
- 2.权重规则生效，权重相同时，离目标元素最近的样式生效

样式指向不同元素，权重规则失效，离目标元素最近的样式生效

加分项：

[开始学习](https://zhuanlan.zhihu.com/p/41604775)
## 8.双飞冀/圣杯布局
答：
- 是什么：都是实现三栏目布局的常用布局方式
- 实现：
  - 双飞翼
  1.父：width:100%;
  2.左中右：float:left,中width:100%；正中margin 0 200px;左margin-left：100%，右margin-left：200px;
  - 圣杯
  1.父：width:100%;
  2.左中右：float:left;中width:100%,左：position:relative;margin-left:100%;left:-200px;右：position:relative;margin-left:200px;right:-200px;
- 优点：1.兼容性好；2.优先加载解析中间重要的内容，优化用户体验。
- 区别：双飞翼中间多了一个div，圣杯左右多了定位属性，结构自然
加分项：

[开始学习](https://juejin.cn/post/6844903817104850952)
## 9.浮动元素会造成什么影响，如何清除浮动
答：
影响：
- 1.块级不认：块级元素认为浮动元素不存在，比如一个块级元素包含浮动元素，上下边就会贴到一起
- 2.占位：会占行内元素的位置，影响行内元素的布局
- 3.影响包含块：通过影响行内元素布局，会间接导致包含块的布局也发生改变

清除浮动
- 1.包含块自身设置浮动，缺点：影响后面节点
- 2.包含块设置成BFC
  - position: absolute,fixed; 缺点：脱离文档流
  - display: flex, gird,table-cell/table, inline-(block/flex/table/grid)
  - overflow: 除了visible
  - float: 除了none
  - body根元素
- 3.伪元素上设置 display: block;clear: both，清除元素两边内容的浮动

加分项：

[开始学习](https://segmentfault.com/a/1190000012739764)
## 10.CSS样式隔离手段
答：
- 动态样式表：应用切换时，替换应用对应的style
- 工程化手段：BEM，CSS Modules, CSS in JS
- shadow dom:
- runtime css transformer: 动态给属性选择器添加随机值
加分项：

[开始学习](https://juejin.cn/post/6844904034281734151#heading-9)
## 11.行内元素、块级元素有哪些，区别是什么
答：
- 行内：span、b、i、sub、sup、a、small、strong、img、input……
- 块级：div、p、table、ol、ul、h1-6、form、pre、dl……

区别
- 1.是否一行
  - 行内元素只能一行显示
  - 块级元素单独占满一行
- 2.是否能改变全部盒模型属性
  - 行内只可以改变内外边距的left和right
  - 块级可以改变所有盒模型属性
- 3.是否可改变宽高
  - 行内：不可以
  - 块级：可以
- 4.宽度是否受内容影响
  - 行内：是
  - 块级：宽度是浏览器宽度
- 5.能包含的元素类型
  - 行内：只能包含行内元素、文本
  - 块级：行内和块级元素都可以
加分项：

[开始学习](https://www.cnblogs.com/yc8930143/p/7237456.html)
## 11.CSS3有哪些新特性
答：
- 过渡+动画（transition、animation、transform）
- 属性选择器、伪类/伪元素选择器
- 边框（box-shadow、border-image、border-radius）
- 背景（background-clip、background-origin、background-size）
- 反射（-webkit-box-reflect）
- 文字(word-break,word-wrap、text-overflow、-webkit-box、text-shadow）
- 颜色（rgba,hsla）
- 渐变
- Filter
- 布局（flex，grid）
- 多列布局(column-count，column-rule)
- 盒模型定义（box-sizing）
- 媒体查询
- 混合模式（background-blend-mode、mix-blend-mode）

[开始学习](https://segmentfault.com/a/1190000010780991)
