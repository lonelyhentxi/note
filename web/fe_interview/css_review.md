>  

1.  语法和形式

    1.  声明：\`property: value\`

    2.  声明块：\`{declaration;\[declaration;…\]}\`

    3.  规则：

        1.  在声明块前放置选择器；

        2.  选择器与声明块统称为规则集，简称为规则

    
    1.  at 规则：\`@规则\`

        1.  说明：覆盖了 meta 信息（@charset、@import），条件信息（@media @document），描述信息（@font-face）


1.  优先级：

    1.  前后顺序：同级由 CSS 的先后顺序控制

    2.  4位优先级：内联 &gt; \#id &gt; .class/\[title=""\]/:psedo-class &gt; tag name/::psedo-element

    3.  继承控制：

        1.  一些属性可以继承

        2.  inherit/initial/unset

        3.  all 属性可以用来重设所有属性

    
    1.  !important 能够覆盖普通规则的层叠；除非有比它优先级更高的 !important

    2.  特殊：

        1.  :not 否定伪类不会被看做伪类，:is 伪类同样

    
    1.  注意：

        1.  优先级基于形式来计算

        2.  计算中同级之间无视 DOM 树之间的距离


1.  CSS 的值

    1.  

<table><thead><tr class="header"><th>数值类型</th><th>描述</th></tr></thead><tbody><tr class="odd"><td>&lt;integer&gt;</td><td>&lt;integer&gt; 是一个整数，比如1024或-55</td></tr><tr class="even"><td>&lt;number&gt;</td><td>&lt;number&gt;表示一个小数——它可能有小数点后面的部分，也可能没有</td></tr><tr class="odd"><td>&lt;dimension&gt;</td><td>&lt;dimension&gt;是一个&lt;number&gt;，它有一个附加的单位</td></tr><tr class="even"><td>&lt;percentage&gt;</td><td>&lt;percentage&gt;表示一些其它值的一部分</td></tr></tbody></table>

1.  长度：

    1.  绝对长度单位：

        1.  

<table><thead><tr class="header"><th>单位</th><th>名称</th><th>等价换算</th></tr></thead><tbody><tr class="odd"><td>cm</td><td>厘米</td><td>1cm = 96px/2.54</td></tr><tr class="even"><td>mm</td><td>毫米</td><td>1mm = 1/10th of 1cm</td></tr><tr class="odd"><td>Q</td><td>四分之一毫米</td><td>1Q=1/40th of 1cm</td></tr><tr class="even"><td>in</td><td>英寸</td><td>1in=2.54cm=96px</td></tr><tr class="odd"><td>pc</td><td>十二点活字</td><td>1pc=1/16th of 1in</td></tr><tr class="even"><td>pt</td><td>点</td><td>1pt=1/72th of 1in</td></tr><tr class="odd"><td>px</td><td>像素</td><td>1px=1/96th of 1in</td></tr></tbody></table>

1.  相对长度单位：

    1.  

<table><thead><tr class="header"><th>单位</th><th>相对于</th></tr></thead><tbody><tr class="odd"><td>em</td><td>父元素的字体大小</td></tr><tr class="even"><td>ex</td><td>字符“x”的高度</td></tr><tr class="odd"><td>ch</td><td>字符“0”的高度</td></tr><tr class="even"><td>rem</td><td>根元素的字体大小</td></tr><tr class="odd"><td>lh</td><td>元素的 line-height</td></tr><tr class="even"><td>vw</td><td>视窗宽度的 1%</td></tr><tr class="odd"><td>vh</td><td>视窗高度的 1%</td></tr><tr class="even"><td>vmin</td><td>视窗较小尺寸的 1%</td></tr><tr class="odd"><td>vmax</td><td>视图较大尺寸的 1%</td></tr></tbody></table>

1.  颜色：

    1.  RGB 色彩：

        1.  颜色关键字：&lt;见MDN&gt;

        2.  16进制二进制值: \#aaaaaa, \#aaa

        3.  rgb(0-255, 0-255, 0-255) , rgba(0-255,0-255,0-255,0.0-1.0)

    
    1.  HSL 色彩：

        1.  属性：

            1.  色调：颜色的底色 0-360 之间，表示色轮周围的角度

            2.  饱和度：颜色多饱和，0-100%，其中0为无颜色（显示灰色阴影），100%为全色饱和度

            3.  亮度：颜色多亮，从0-100%，其中0表示没有光（它将显示为黑色），100%表示完全亮（它将完全显示为白色）

        
        1.  HSLA：附加透明度

    
    1.  图片：

        1.  background-image: linear-gradient(90deg, rgba(119, 0, 255, 1) 39%, rgba(0, 212, 255, 1) 100%);


1.  字符串和标识符

    1.  使用“”包裹来表明是一个字符串


1.  函数：如 calc 函数


1.  选择器：

    1.  类选择器；元素选择器；唯一选择器；属性选择器；略

    2.  &lt;空格&gt; 表示后代选择器；&gt; 表示直接子孙选择器；+邻接兄弟选择器；~通常兄弟选择器；

    3.  伪类和伪元素：

        1.  伪类：

            1.  :first-child，:last-child, :only-child, :nth-child(x) 选择特定儿子

            2.  :invalid input 和 form 的非法状态

            3.  :hover, :focus, :active, :link

        
        1.  伪元素：

            1.  ::first-line 选择第一行

            2.  ::before, ::after 向前后插入元素（需要在 CSS 中设置 content：&lt;string&gt;）

        
        1.  其它：见 [https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Pseudo-classes_and_pseudo-elements](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Pseudo-classes_and_pseudo-elements)


1.  盒模型：

    1.  组成部分：

        1.  元素：margin-edge, border-edge, padding-edge, content-edge

        2.  <img src="media/css_review/image1.png" style="width:5.76806in;height:3.86806in" />

    
    1.  属性：

        1.  内容区域：

            1.  box-sizing:

                1.  分类：

                    1.  content-box（默认）：width 等参数影响 content-width，其它累加至总宽度

                    2.  border-box：width 等参数影响 content + padding + border，background-color/image 设定此时会衍生到边框（边框覆盖，可通过 background-clip 调整）

                
                1.  注意：

                    1.  不建议使用 padding-box 属性了

            
            1.  background-clip:

                1.  分类：

                    1.  border-box/padding-box/content-box

                    2.  text: 背景被裁减为文字的前景色

            
            1.  width/height/min-width/max-width/min-height/max-height

        
        1.  内边距区域：padding/padding-left/padding-right/padding-top/padding-bottom

        2.  边框区域：border-width/border

        3.  外边距区域：margin/margin-top/margin-left/margin-right/margin-bottom

    
    1.  外边距重叠：

        1.  描述：

            1.  块级元素的上外边距和下外边距同时都有设定时，只会保留最大边距，称为边界折叠

            2.  float 和 position: absolute 设定的元素不会外边距重叠

            3.  同级相邻/同级中间距离为空/没有内容将父元素和后代元素分隔开

    
    1.  visibility：显示或者隐藏元素而不改变文档的布局

        1.  分类：

            1.  visible：正常显示

            2.  hidden：隐藏元素，但是其他元素不改变；若子元素不隐藏，则该元素不隐藏

            3.  collapse：

                1.  用于 table 行/行组和列，不占用任何空间

                2.  快速删除行列，不重新计算整个表

                3.  折叠的弹性项目被隐藏，占用的空间将删除

                4.  对于其他元素，折叠与隐藏相同

    
    1.  overscroll-behavior：浏览器过度滚动时的表现

        1.  类似：overscroll-behavior-x/y/inline/block

        2.  分类：

            1.  auto（默认）

            2.  contain：默认滚动边界行为不变，临近区域不会被滚动链影响

            3.  none：临近区域不受影响，而且默认的滚动到边界的表现也被阻止

            4.  组合


1.  视觉格式化模型：

    1.  决定因素：

        1.  盒子的尺寸：精确指定、约束指定、没有指定

        2.  盒子类型：inline、inline-level、atomic inline-level、block

        3.  定位方案：普通流（static、relative）、浮动(float）、绝对（absolute、fixed、sticky)

        4.  文档树中的其它元素：当前盒子的子元素和兄弟元素

        5.  视口尺寸和位置

        6.  包含图片的尺寸

        7.  其它的某些外部因素

    
    1.  术语和概念：

        1.  block 块在文档流上占据一个独立的区域

        2.  containing block 包含其它盒子的块

        3.  box 由 CSS 引擎根据文档中的内容所创建

        4.  block-level element 元素 display 为 block、list-item、table 时，该元素成为块级元素

        5.  block-level box 块级元素生成，一个元素会生成一个或者多个块级盒子，每个块级盒子都一定会参与块格式化上下文的创建

        6.  block box 同时是块级盒子和块容器盒子的盒子

            1.  具名块盒子

            2.  匿名块盒子 - 无法被 CSS 选择符选中

        
        1.  block container box 或者 block containing box，块容器盒子主要用于确定其子元素的定位和布局

        2.  inline-level element 元素 display 为 inline、inline-block、inline-table 的元素称为行内级元素

        3.  inline-level box

        4.  inline box 参与行内格式化上下文创建的行内级盒子

            1.  具名行内盒子

            2.  匿名行内盒子（CSS 选择器无法选中）

        
        1.  atomic inline-level box 不参与行内格式化上下文创建的行内级盒子。其内容不会拆分为多行显示

    
    1.  块级元素和块级盒子：

        1.  block-level element 1 -&gt; 1-n block-level box(the one is principal block-level box)

        2.  principal block-level box 包含由后代元素生成的盒子以及内容，同时参与定位方案

        3.  有些块级盒子不是块容器盒子，比如表格；有些块容器盒子不是块级盒子，比如非替换行内块和非替换表格单元

        4.  匿名块盒子：类似 &lt;div&gt;xxx&lt;p&gt;yyy&lt;/p&gt;xxx&lt;/div&gt; 时，为了便于排布，会在 &lt;p&gt;标签外部前后生成匿名块，其可继承属性为 inherit、不可继承属性为 initial, 且无法被 CSS 选择器选择

    
    1.  行内级元素和行内盒子

        1.  行内级元素在显示时不会生成内容块，但是可以与其他行内级内容一起显示为多行

        2.  具有 display: inline 样式的非替换盒子是行内盒子；display: inline-block/inline-table 或者替换元素创建的盒子不会像行内盒子一样被拆分为多个盒子

        3.  匿名行内盒子：其它类似匿名块级盒子；一种常见的情况是 CSS 引擎会自动为直接包含在块盒子中的文本创建一个行内格式化上下文（如果仅包含空格可能并非如此，空格可能会被 white-space 的设置移除）

    
    1.  其它类型的盒子：

        1.  行盒子：行内上下文创建，显示一行文本；在行盒子内部，行盒子总是从块盒子的一边延伸到另一边；当有浮动元素时，行盒子会从左浮动的元素的右边延伸到向右浮动的元素的左边缘

        2.  Run-in 盒子：display: run-in 取决于紧随其后的盒子的类型

        3.  表格内容模型：表格包装器盒子和表格盒子、表格标题盒子等

        4.  多列内容模型：在容器盒子和内容之间创建多个列盒子

        5.  网格内容模型和弹性内容模型


1.  布局和包含块：

    1.  position:

        1.  absolute: 元素移出文档流，并不为元素预留空间，通过指定元素相对于最近非 static 定位祖先元素的偏移，来确定元素位置

        2.  static：元素使用正常的布局行为

        3.  relative：元素先 static 定位，在不改变页面布局的前提下调整元素位置

        4.  fixed：移出正常文档流，并不为元素预留空间，指定相对视口的位置来指定元素位置

        5.  sticky：先进行 static 定位，相对于最近滚动祖先和块级祖先进行偏移，不会影响其它元素的位置

            1.  常用于 dl，dt，dd

    
    1.  确定包含块：

        1.  如果 position 为 static 或者 relative，包含块就是最近祖先的块元素（inline-block, block, list-item）或者格式化上下文（table container, flex container, grid container, block container）的 content edge

        2.  如果 position 为 absolute，包含块就是最近的不是 static 的 position 的祖先元素的 padding edge

        3.  如果 position 为 fixed，在连续媒体（continuous media）的情况下包含块是 viewport，在分页媒体（page media）的情况下包含块是分页区域

        4.  如果 position 为 fixed 或者 absolute，包含块也可能是由满足以下条件的最近父元素的 padding edge 组成的：

            1.  transform perspective 不为 none

            2.  will-change value of transform perspective

            3.  contain: paint

    
    1.  display:

        1.  外部：block、inline、run-in

        2.  内部: flow、flow-root（将子浮动元素计算高度）、table、flex、grid、ruby(用于旁注)

        3.  内外：block flow、inline table、flex run-in

        4.  列表：list-item，将外部变为 block 盒、内部显示类型变为多个 list-item inline 盒

        5.  internal：table\*、ruby\*，复杂且只在特定布局模型内部才有意义

        6.  盒子：contents（从可访问性树中移除, 让子元素和父元素布局方式一样）、none（从可访问性树中移除, 消失），决定是否使用盒模型

        7.  传统：inline-block、inline-table、inline-flex、inline-grid CSS2 对于 display 属性单关键字语法，inline 级变体需要单独使用

        8.  全局

    
    1.  流布局模型：

        1.  普通流：按照次序依次定位每个盒子

            1.  要求：position: relative/static; float: none;

            2.  块格式化上下文中 - 依次垂直排列；行内格式化上下文中 - 盒子依次水平排列

        
        1.  浮动：将盒子从普通流中单独拎出来，将其放到外层盒子的某一边

            1.  要求：position: static/relative；float: &lt;!"none"&gt;

> 在浮动定位中，浮动盒子会浮动到当前行的开始或尾部位置。这会导致普通流中的文本及其他内容会“流”到浮动盒子的边缘处，除非元素通过 Clear 清除了前面的浮动。

1.  行盒子会伸缩以适应盒子的大小


1.  绝对定位：按照某种绝对坐标系来计算


1.  块格式化上下文：

    1.  元素：html、float、position=absolute|fixed、inline-block、table-cell（表格单元格默认值）、table-caption（表格标题默认值）、table/table-row/table-row-group、table-header-group、table-footer-group（tbale、row、tbody、thead、tfoot 的默认属性）或者 inline-table、!overflow: visible、display: flow-root、contain：layout/content/paint 的元素、弹性元素、网格元素、多列元素（column-count 或者 column-width 部位 auto 包括 column-count 为 1）

    2.  overflow: 当一个元素的内容太大而无法适应块级格式化上下文的做法

        1.  分类：overflow、overflow-x、overflow-y

        2.  值：

            1.  visible（默认值）：不修剪内容，呈现到元素框之外。其它值会创建新的块级格式化上下文；若浮动和滚动条相交，会在每个滚动步骤后强行重新包装内容，导致慢滚动体验

            2.  hidden：修剪内容，其余不可见

            3.  auto：浏览器定夺

            4.  scroll：内容被修剪，浏览器显示滚动条

    
    1.  contain：该属性允许开发者声明当前元素和它的内容尽可能独立于 DOM 树的其它部分。使得重新计算布局、样式、绘图和他们组合的时候，只会影响到有限的 DOM 区域

        1.  值：

            1.  none（默认） 无布局包含

            2.  strict 包含 layout、style、paint、size

            3.  content 包含 layout、paint、size

            4.  size 该元素不依赖于它的子孙元素

            5.  layout 没有外部元素可以影响布局

            6.  style 同时会影响这个元素和子孙元素的属性，都在该元素的包含范围内

            7.  paint 子孙不在边缘外显示

    
    1.  clear:

        1.  行元素无视自动伸缩，而找一个可以占满父元素位置的点开始

        2.  值：left、right、both、none、inline-start、inline-end

        3.  案例：::after { content: ''; clear: both; display: table; }


1.  多列布局：

    1.  属性：

        1.  column 综合

        2.  column-count：lenth/auto（由其他属性指定）

        3.  column-width: 多列宽度

        4.  column-gap: 多列间隔；

        5.  column-rule\[-width/style/color\]：多列之间的直线

        6.  column-span：none; all; inherit; 子元素跨越多列

    
    1.  注意：

        1.  break-inside: avoid 防止多列布局、分页媒体和多区域上下文中的意外中断


1.  弹性布局：

    1.  容器属性：

        1.  flex-direction: row（默认） | row-reverse | column | column-reverse 方向

        2.  flex-wrap: nowrap（默认） | wrap | wrap-reverse(前面的行在下方)

        3.  flex-flow: &lt;flex-direction&gt; || &lt;flex-wrap&gt; 组合

        4.  justify-content: 主轴对齐方向

            1.  flex-start 前对齐 flex-end 后对齐

            2.  center 中间密集对齐

            3.  space-between 两侧“铁板”，内容块视为斥性磁极，之间间隔相等

            4.  space-around 有一定空间的间隔松散排布，之间间隔比两侧间隔大一倍

        
        1.  align-items: 在交叉轴上的对齐方式

            1.  flex start | flex-end | center 同上

            2.  baseline 项目第一行文字的基线对齐

            3.  stretch（默认值）拉伸

        
        1.  align-content: 交叉轴多根轴线的对齐方式

            1.  flex-start | flex-end | center | space-between | space-around | stretch(默认值，轴线占满整个交叉轴)

    
    1.  项目属性：

        1.  order: 项目的排列顺序，越小越靠前

        2.  flex-grow: 项目放大比例，默认为 0，有空间也不放大，不同项的相对放大比例形成效果

        3.  flex-shrink：项目的缩小比例，默认为 1，即空间不足，项目将缩小

        4.  flex-basis: 项目占据的主轴空间，按默认 auto， 可选为 length

        5.  flex: none(0 0 auto) | auto(1 1 auto) | &lt;flex-grow&gt; || &lt;flex-shrink&gt; || &lt;flex-basis&gt;

        6.  align-self：auto | flex-start | flex-end | center | baseline | stretch 覆盖父亲的交叉轴对齐方式


1.  网格布局：

    1.  属性：

        1.  grid-template-columns/rows:

            1.  值：xxx 传统单位 | 百分比 | auto-fill | fr 片段 | auto 浏览器决定长度 | \[cn\] \[rn\] (网格线名称，允许多个名字，如 \[fifth-line row-5\])

            2.  函数：

                1.  repeat(count, 值)

                2.  minmax(值， 值)

        
        1.  grid\[-rows/columns\]-gap 列间距（也可以写成 row-gap/column-gap/gap）

        2.  grid-template-area: 'a a c'\\n'd . f';

            1.  为单元格命名

            2.  同名即合并单元格

            3.  不利用区域用点表示

            4.  区域的命名会影响网格线，每个区域的起始网格线，会自动命名为区域名-start，结束会自动命名为区域名-end

        
        1.  grid-auto-flow ：( row | column ) \[dense\]

            1.  dense 表示尽量填满，不出现空格

        
        1.  justify-items: start | end | center | stretch 横轴对齐方式

        2.  align-items: start | end | center | stretch 纵轴对齐方式

        3.  place-items: &lt;align-items&gt; &lt;justify-items&gt;

        4.  justify-content/align-content/place-content: start | end | center | stretch | space-around | space-between | space-evenly(边框间隔和之间间隔相等) 整个区域在容器的水平位置

        5.  grid-auto-columns：当不指定列宽和行高时，浏览器根据内容大小确定新增网格的列宽和行高

        6.  grid/grid-templates 是前面的缩写，建议不要简写

    
    1.  项目属性：

        1.  grid-row/column-start/end: 如果项目重叠，使用 z-index 决定

            1.  number

            2.  \[r/cn\] 网格线名字

            3.  区域名-start/end

            4.  span 跨越多少网格

        
        1.  grid-column/row:

            1.  number / number (start/end)

            2.  number 第几行、第几列

        
        1.  grid-area:

            1.  number 区域编号

            2.  string 区域值

            3.  &lt;row-start&gt; / &lt;column-start&gt; / &lt;row-end&gt; / &lt;column-end&gt;

        
        1.  justify-self/align-self/place-self: start | end | center | stretch


1.  Ruby 布局（旁注布局）：

    1.  属性（ruby 标签的属性）：

        1.  ruby-align: start center space-between space-around

        2.  ruby-position: over under inter-character

    
    1.  元素：

> &lt;ruby&gt;  
> &lt;rb&gt;超電磁砲&lt;/rb&gt;  
> &lt;rp&gt;（&lt;/rp&gt;&lt;rt&gt;レールガン&lt;/rt&gt;&lt;rp&gt;）&lt;/rp&gt;  
> &lt;/ruby&gt;

1.  层叠上下文：

    1.  元素：

        1.  html

        2.  position: absolute/relative 且 index 不为 auto

        3.  position 为 fixed、sticky 的元素

        4.  flex/grid 且 z-index 部位 auto

        5.  opacity 属性值小于 1

        6.  mix-blend-mode 不为 normal

        7.  transform、filter、perspective、clip-path、mask/mask-image/mask-border 不为 none

        8.  isolation 为 isolate

        9.  will-change 设定任意在 non-initial 值时会创建层叠上下文

        10. contain 为 layout、paint 或包含之一的合成值

    
    1.  特点：

        1.  子集 z-index 只有在父级中才有意义

        2.  层叠上下文可以包含在其它层叠上下文中，并且一起创建层叠上下文层级

        3.  每个层叠上下文都独立于兄弟元素

        4.  每个层叠上下文都是自包含的：一个元素发生层叠，该元素作为整体在父级层叠上下文中按顺序进行层叠

    
    1.  isolation: 和 background-blend-mode 混合使用时，可以只混合一个指定元素栈的背景；允许使一组元素从它后面的背景中独立出来，只混合这组元素的背景

        1.  auto（默认）需要时候才创造一个层叠栈环境

        2.  isolate 强制创造一个层叠栈环境

    
    1.  mix-blend-mode: 混合模式 [see at](https://developer.mozilla.org/zh-CN/docs/Web/CSS/mix-blend-mode)

    2.  background-blend-mode：背景如何混合


1.  使用 CSS 变换：

    1.  transform：物体变换

        1.  特点：

            1.  只能转换由盒模型定位的元素

        
        1.  分类：

            1.  图形学矩阵映射：matrix(x1,x2,x3,x4,x5,x6)

            2.  平移：translate(x, y)/ translateX(x)/translateY(y)

            3.  缩放：scale(x, y)/scaleX(x)/scaleY(y)

            4.  旋转：rotate(θ)/rotate(x)/rotate(y)/rotate(z)

            5.  拉伸：skew(x,y)/skew(x)/skew(y)

            6.  图形学矩阵映射 3D: matrix3d(x1…x16)

            7.  平移 3D：translate3d(x,y,z)

            8.  平移 Z：translateZ(z)

            9.  缩放 3D：scale3d( r )

            10. 缩放 Z：scaleZ(z)

            11. 透视：perspective(x)

            12. 组合

            13. 全局值：inherit、initial、unset

    
    1.  transfrom-origin：

        1.  特点：只能转换由盒模型定位的元素

        2.  分类：

            1.  通常情况：translate-origin: x/y/z-offset\[-keywords\]

    
    1.  transform-style:

        1.  特点：试验性

        2.  分类：

            1.  preserve-3d/flat

    
    1.  perspective：

        1.  语法：

            1.  通常：none | &lt;length&gt;

    
    1.  persepective-origin:

        1.  语法：

            1.  通常：x/y-offset\[-keyword\]

    
    1.  backface-visibility：

        1.  后面可见性

        2.  语法：

            1.  visible/hidden

    
    1.  filter:

        1.  url: 接收一个文件（支持锚点）来指定一个具体的路径元素

        2.  blur(radius) 高斯模糊，标准差

        3.  brightness(number) 给图片运用线性乘法使得看起来更亮或者更按

        4.  contrast(number) 对比度

        5.  drop-shadow(offset-x, offset-y, blur-radius, spread-raius, color) 图像阴影

        6.  grayscale(number) 转换为灰度图像

        7.  hue-rotate 色相旋转

        8.  invert(number) 反转颜色

        9.  opacity(number) 透明度

        10. saturate(number) 饱和度

        11. sepia(number) 褐色化

        12. 符合函数

    
    1.  mask: 允许使用者通过部分或者完全隐藏一个元素的可见区域

        1.  属性：image, mode, position, size, repeat, origin, clip, composite

    
    1.  clip-path: 裁减区域

        1.  none 不裁剪

        2.  url 背景图片

        3.  集合盒子：margin/border/padding/content/fill(对象边界框)/stroke(笔触边界框)/view box(SVG视口)/none

        4.  基本形状：

            1.  inset(length-percentage, round)

            2.  circle(radius at x y)

            3.  ellipse(r1 r2 at positon)

            4.  polygon(nonzero/evenodd xi,yi…)

            5.  path(nonezero/evenodd, xi, yi…)

    
    1.  opacity: 将该元素与其子元素当成整体看待

        1.  number

    
    1.  will-change:

        1.  auto（默认）没指定哪些属性会变化

        2.  scroll-position 改变滚动条产生动画

        3.  contents 改变元素中某些内容产生动画

        4.  transform / opacity / left / top 产生动画


1.  简写属性：

    1.  边界情况：

        1.  联合属性中，未指定的属性将设置为初始值，因此建议将更细化的属性（单独属性）置于后方覆盖

        2.  关键词 inherit 只能用于单独属性

    
    1.  案例：

        1.  margin 和 padding 属性：

            1.  顺序：top right bottom left / top&bottom right&left / top&bottom&right&left

        
        1.  border: width style color;

        2.  border-radius: all/left-top&right-bottom right-top&left-bottom/left-top right-top right-bottom right-bottom

        3.  font: style(italic) weight size\[/line-height\] font-family

        4.  background: color url(image) repeat-mode position


1.  样式化文本：

    1.  基本文本和文字样式

        1.  样式文本的 CSS 属性：

            1.  字体样式：字体的属性，直接应用到文本中 - 大小/粗体/斜体……

            2.  文本布局风格：作用于文本的间距以及其他布局的功能 - 操纵行与字之间的空间，在文本框中的对齐方式

        
        1.  注意：

            1.  包含在元素中的文本是单一的实体，不能将文字的某一部分选中或者添加样式，而要使用某些标签包装

        
        1.  文本属性：

            1.  颜色：color

            2.  装饰：text-decoration:

                1.  text-decoration-line: underline 下划线，line-through 删除线, overline 上方的修饰线，none 无文本修饰效果

                2.  text-decoration-color: &lt;color&gt; | currentColor(默认)

                3.  text-decoration-style: solid/double/dotted/dashed/wavy(波浪线)

            
            1.  字体种类：font-family

                1.  网页安全字体：

                    1.  

<table><thead><tr class="header"><th>字体名称</th><th>泛型</th><th>注意</th></tr></thead><tbody><tr class="odd"><td>Arial</td><td>sans-serif</td><td>通常认为的最佳做法是添加 Helvetica 作为 Arial 的首选替代品，尽管它们字体层面基本相同，但是 Helvetica 被认为具有更好地形状</td></tr><tr class="even"><td>Courier New</td><td>monospace</td><td> </td></tr><tr class="odd"><td>Times New Roman</td><td>serif</td><td> </td></tr><tr class="even"><td>Georgia</td><td>serif</td><td> </td></tr></tbody></table>

1.  默认字体：

<table><thead><tr class="header"><th><strong>名称</strong></th><th><strong>定义</strong></th><th><strong>示例</strong></th></tr></thead><tbody><tr class="odd"><td>serif</td><td>有衬线的字体 （衬线一词是指字体笔画尾端的小装饰，存在于某些印刷体字体中）</td><td>My big red elephant</td></tr><tr class="even"><td>sans-serif</td><td>没有衬线的字体。</td><td>My big red elephant</td></tr><tr class="odd"><td>monospace</td><td>每个字符具有相同宽度的字体，通常用于代码列表。</td><td>My big red elephant</td></tr><tr class="even"><td>cursive</td><td>用于模拟笔迹的字体，具有流动的连接笔画。</td><td>My big red elephant</td></tr><tr class="odd"><td>fantasy</td><td>用来装饰的字体</td><td>My big red elephant</td></tr></tbody></table>

1.  字体栈：

    1.  按顺序尝试使用，最后回退到后备字体

    2.  不只一个单词的字体名称需要使用“ ”包裹


1.  字体大小：font-size

    1.  px 像素 / em 父元素字体大小的相对值 / rem 相对于 HTML 根元素字体大小


1.  字体样式：font-style

    1.  normal（默认）普通字体/italic 斜体/oblique 斜体的模拟版本


1.  字体粗细：font-weight

    1.  normal（默认）bold 加粗

    2.  ligher bolder 更细或者更粗

    3.  100-900 的数值


1.  字体转换：text-transform

    1.  none 防止任何转型/uppercase 大写/lowercase 小写/capitalize 首字母大写/full-width 全角


1.  文字阴影：text-shadow

    1.  &lt;x-offset&gt; &lt;y-offset&gt; &lt;shadow-width&gt; &lt;shadow-blur&gt; &lt;shadow-color&gt;


1.  文本布局：

    1.  文本对齐：text-align

        1.  left 左对齐文本/right 右对齐文本/center 居中文本/justify 文本展开

    
    1.  行高：line-height：

        1.  文本每行之间的高，推荐为 1.5 - 2 倍

    
    1.  字母间距：letter-spacing

    2.  单词间距：word-spacing

    3.  缩进：text-indent（第一个块元素首行文本内容之前的缩进量）

    4.  文本溢出：text-overflow （如何向用户发出未显示的溢出内容信号）

        1.  clip 在内容区域的极限处截断文本，可能在字符的中间发生截断

        2.  ellipsis 用一个省略号表示被截断的文本（如空间太小容不下省略号，省略号也会被截断）

        3.  &lt;string&gt; 用一个字符串表示被截断的文本

    
    1.  处理元素中的空白的方式：white-space

        1.  normal (默认)连续的空白符会被合并，换行符会被当做空白符来处理

        2.  nowrap 连续的空白符被合并，但文本内的换行无效

        3.  pre 连续的空白符被保留；在遇到换行符或者 &lt;br&gt; 时

        4.  pre-wrap 和前述类似，或者为了填充行盒子才会换行

        5.  pre-line 连续的空白符会被合并，在遇到换行符或者 &lt;br&gt; 或者填充行框盒子时才会执行

        6.  break-spaces: 与 pre-wrap 行为相同，

            1.  任何保留的空白序列总是占用空间，包括在行尾

            2.  每个保留的空格字符都存在换行机会，包括空格字符之间

            3.  这些保留的空间占用空间而不会挂起，影响盒子的固有尺寸

    
    1.  指定如何在单词内断行 word-break：

        1.  normal（默认）使用默认的断行规则；break-all 对于 non-CJK 文本，可在任意字符间断行；keep-all CJK 文本不断行、Non-CJK 文本表现同 normal

    
    1.  字形：font-variant

        1.  small-caps/common-ligaures/small-caps

    
    1.  字距：font-kerning

        1.  auto 浏览器来决定是否使用字体字距/normal 使用字体包含的字距信息/none 禁用字体中的字距信息

    
    1.  文本方向：direction

        1.  设置文本、表列水平溢出的方向

        2.  值：

            1.  ltr(默认），设置文本和其它元素的默认方向从左到右

            2.  rtl 反向

    
    1.  CJK 断开：line-break

        1.  auto 使用默认的断行规则分解文本； loose 使用尽可能松散的规则分解文本（用于短行的情况，例如报纸）；normal 使用最一般的断行规则分解文本； strict 使用严格的断行原则分解文本；anywhere 在每个印刷字符单元的周围，都有一个自动换行（soft wrap）的机会，包括标点符号、保留的空白字符、单词之间。忽略任何阻止换行的字符，或是由 word-break 属性强制的字符

    
    1.  换行连字符：hypens

        1.  none 只会在空白时换行/manual 在换行点处换行/auto 根据换行设置自动处理换行

        2.  可选换行点： &HYPEN; &shy;

    
    1.  文字溢出 （word-wrap）overflow-wrap 当一个不能被分开的字符串太长而不能填充其包裹盒时，为防止溢出，浏览器是否允许这样的单词中断换行

        1.  normal 单词结束时换行；break-word 允许换行

    
    1.  书写模式：writing-mode

        1.  描述：块级内容堆叠的方向，行内内容在块级容器中流动的方向

        2.  值：

            1.  horizontal-tb / vertical-rl / vertical-lr / sidewars-rl（内容从上到下垂直流动，所有字形朝左侧设置）/ sideways-lr

    
    1.  其它参见：https://developer.mozilla.org/zh-CN/docs/Learn/CSS/%E4%B8%BA%E6%96%87%E6%9C%AC%E6%B7%BB%E5%8A%A0%E6%A0%B7%E5%BC%8F/Fundamentals


1.  样式列表

    1.  列表的默认设置：

        1.  ul 和 ol 元素设置 margin 的顶部和底部：16px(1em) 0; 和 padding-left: 40px(2.5em); 假设浏览器默认字体大小为 16 px

        2.  li 默认没有设置间距

        3.  dl 元素设置 margin 的顶部和底部： 16px(1em)，无内边距设定

        4.  dd 元素设置为： margin-left 40px(2.5em)

        5.  p 元素设置 margin 的顶部和底部：16px（1em），和其它列表类型相同

    
    1.  处理列表间距离：

        1.  line-height：设置多行元素的空间量，对于块级元素，它指定元素行盒的最小高度。对于非替代的 inline 元素，它计算行盒的高度。

        2.  使用 margin-bottom 调整列表和 P 元素的空间一致

    
    1.  列表特定样式：

        1.  list-style-type 设置用于列表的项目符号的类型，例如无序列表的方形或圆形项目符号，或者有序列表的数字，字母或罗马数字

            1.  disc 实心原点；cricle 空心原点；square 实心方块；decimal 十进制阿拉伯数字；cjk-decimal 中日韩十进制数；decimal-leading-zero 前位填 0 的十进制数；lower-roman 小写罗马；upper-roman 大写罗马；lower-greek 小写希腊；lower-alpha 小写字母；lower-latin 小写拉丁；upper-alpha；upper-latin；…

        
        1.  list-style-postion 项目符号的位置：

            1.  inside / outside

        
        1.  list-style-image 允许为项目使用自定义图片

    
    1.  管理列表计数：

        1.  &lt;html 属性&gt; 开始计数的的位置 &lt;ol start="4"&gt;

        2.  &lt;html 属性&gt; reversed 将列表倒计数

        3.  &lt;html 属性&gt; value 直接指定 li 项目的值


1.  样式化链接：

    1.  伪类：:link（默认）未访问；:visited 访问过了；:hover 悬浮中; :focus 链接被聚焦的时候； ：active 链接被激活的时候

    2.  默认样式：下划线；未访问过的链接为蓝色的；访问过的链接是紫色的；悬停的时候鼠标的光标会变成小手的图标；选中的时候，链接周围有轮廓；激活的时候变成红色

    3.  调整样式：

        1.  color 调整颜色

        2.  cursor 鼠标指针悬浮在元素上方显示的鼠标光标

            1.  auto （默认）根据当前内容决定鼠标样式

            2.  default 默认指针

            3.  none 无指针渲染

            4.  context-menu 指针下有可用内容目录

            5.  help 指示帮助

            6.  pointer 悬浮为链接上时

            7.  progress 程序后台繁忙，用户可以交互

            8.  wait 程序繁忙，不可交互

            9.  cell 单元格可被选中

            10. crosshair 交叉指针，指示位图中的选框

            11. text 文字可被选中

            12. vertical-text 垂直文字可被选中

            13. alias 复制或者快捷方式将被创建

            14. copy 指示可复制

            15. move 被悬浮的物体可被移动

            16. no-drop 当前位置不能扔下

            17. not-allowed 不能执行

            18. grad 可抓取

            19. grabbing 抓取中

            20. all-scroll 元素可任意方向滚动

            21. col-resize 重设宽度

            22. row-resize 重设高度

            23. n/e/s/w/nw/ne/se/sw/ew/ns/nesw/nwse-resize 某条边或者双向重新设置大小

            24. zoom-in/out 可被放大或者缩小

        
        1.  text-decoration: none 取消下划线（见前文）

        2.  outline 边线框设置 &lt;width&gt; &lt;style&gt; &lt;color&gt;


1.  Web 字体

    1.  动态加载字体：

        1.  @font-face 块，可以指定要下载的字体文件

        2.  @front-face { font-family: "myFont"; src: url("myFont.ttf") \[format('woff2')\]; font-weight: normal; font-style: normal; }

    
    1.  支持：

        1.  大多数现代浏览器都支持 WOFF 和 WOFF2; 其它的支持还有 embedded-opentype truetype svg

        2.  字体一般都不能自由使用，必须为他们付费，或者遵循其它许可条件


1.  样式化替换元素 - 图像、媒体和表格元素：图像和视频被描述为替换元素

    1.  调整图像大小：

        1.  在网格、弹性布局的情况下，会有不同的默认可替换元素布局方式

        2.  设置 width 和 height 可能会无条件的改变可替换元素的属性

        3.  object-fit:

            1.  指定可替换元素内容的填充方式

            2.  值：

                1.  fill 拉伸并填充

                2.  cover 裁减并填充

                3.  contain 保持宽高比缩放适应内容框

                4.  none(默认) 保持原有尺寸

                5.  scale-down 内容尺寸与 none 或 contain 中的一个相同，取决于那个更小

        
        1.  object-position:

            1.  指定可替换元素内容的调整方式

            2.  值：

                1.  &lt;position&gt; values


1.  样式化 HTML 表单：

    1.  优缺点：

        1.  优点：form、fieldset、label、output 等元素在跨平台时很少出现问题

        2.  缺点：legend 元素不能在所有平台上正确定位，checkbox 和 radio 按钮不能直接应用样式（CSS3前），placeholder 不能以任何标准方式应用样式（可以以私有的CSS伪元素或者伪类定义样式）

        3.  丑陋：select、option、optgroup、datalist、progress、meter、文件选择等部件不可样式化；想要定制这些小部件，必须依靠 JavaScript 来构建一个你能够应用样式的 DOM 树

    
    1.  实践经验：

        1.  对于 Webkit 核心的浏览器，可以使用 -webkit-appearance: none 来手动关闭不一致渲染

        2.  集成和表单元素：在一些浏览器中，表单元素默认不会继承字体样式，如果需要使用继承字体样式，需要手动设置继承

        3.  form 元素与 box-sizing：确保在给 form 元素设定宽度和高度时有统一的体验，使用 box-sizing: border-box 并将内外边距设为 0

        4.  其它有用的设置：在 textarea 上设置 overflow: auto 避免在不需要滚动的时候显示滚动条

        5.  legend 元素定位是其 fieldset 父元素上边框的最顶端，可以使用 position 属性将其位置设为绝对或者相对

        6.  默认情况下，textarea 是 inline-block, 与文本底线对齐；如果在保持 inline 的情况下想要使用它，可以使用 vertical-align 来改变竖直对齐方式


1.  样式化表格：

    1.  间距和布局：

        1.  在表格上，使用 table-layout 属性设置一个 fixed 值通常是一个好主意，因为它使得表的行为在默认情况下更可预测

            1.  table-layout: auto（默认）根据包含的内容确定宽度; fixed：表格和列的宽度通过表格首行的宽度来设置，且该布局方式可以加速渲染，任何包含溢出内容单元格的内容可以使用 overflow 属性控制是否允许溢出

        
        1.  使用 border-collapse 的属性的 collapse 对于任何样式表的工作来说都是最佳实践。这样在表设置边框时，将会有没有间隔

            1.  border-collapse：collapse 相邻的单元格共用一条边框；separate（默认值）每个单元拥有独立的边框

        
        1.  对于整个表设置 border 是有必要的

        2.  在 th 和 td 元素上设置一些 padding，使得数据项有一些空间，表格看起来更加清晰

    
    1.  简单的排版：

        1.  &lt;thead&gt; 和 &lt;tfoot&gt; 上可以设置自定义字体

        2.  视情况可以对 tbody 中的表格单元中文本进行居中对齐（默认情况 text-align=left, 标题为 center）

    
    1.  图形和颜色：

        1.  斑马条纹实现 tbody tr:nth-child(odd/even)


1.  背景和边框：

    1.  背景颜色：background-color: 使用任何 color 值

    2.  背景图片：

        1.  控制背景平铺 background-repeat: repeat/repeat-x/repeat-y/no-repeat

        2.  调整背景大小

            1.  大小 background-size: xxx

            2.  填充方式：cover 剪裁填充；fill 拉伸填充；contain 等比缩放适应；none 无；scale-down contain 和 none 较小者

        
        1.  背景定位：默认（0，0），使用关键字或者离上、左的距离

        2.  可以设置多个背景图像，逗号分隔

        3.  背景附加 background-attachment: scroll 使得元素的背景在页面滚动时滚动；fixed 背景固定在视口上；local 将背景固定在设置的元素上

        4.  渐变：linear-gradient 和 radial-gradient

    
    1.  边框：

        1.  通常：&lt;width&gt; &lt;style&gt; &lt;color&gt;

        2.  圆角 border-radius


1.  媒体查询：

    1.  简介：

        1.  语法：@media at-rule 可以根据媒体查询的结果有条件的应用样式表的一部分；@import 可以有条件的应用整个样式表

        2.  HTML 中的媒体查询：

            1.  &lt;link&gt; 中的 media 属性，定义了待应用链接资源（通常是CSS）的媒体

            2.  &lt;source&gt; 元素的 media 属性中，定义待应用源的媒体

            3.  &lt;style&gt; 元素的 media 中，定义待应用样式的媒体

        
        1.  在 JavaScript 中使用媒体查询

            1.  使用 Window.matchMedia 方法可以根据媒体查询测试窗口

            2.  MediaQueryList.addListener 可以在查询状态发生变化时收到通知

    
    1.  使用媒体查询：

        1.  语法：媒体查询由可选的媒体类型和任意数量的媒体要素表达式组成，使用逻辑运算符可以以各种方式组合多个查询，媒体查询不区分大小写；当媒体类型与显示文档的设备匹配且所有要素表达式为 true 时，媒体查询为 true。涉及未知媒体类型的查询始终为 false（link始终下载，为false仅仅是不可用）

        2.  设计：

            1.  媒体类型：

                1.  all 所有设备

                2.  print 适用于在打印预览模式下屏幕上查看的分页材料和文档

                3.  screen 主要用于屏幕

                4.  speech 主要用于语音合成器

            
            1.  媒体功能：any-hover, any-pointer, aspect-ratio, color … &lt;见MDN [<span class="underline">https://developer.mozilla.org/en-US/docs/Web/CSS/Media\_Queries/Using\_media\_queries</span>](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries)&gt;

            2.  逻辑运算符：

                1.  and xx&&yy

                2.  not (!xxx)

                3.  only 因为不存在的项返回 false，用于支持早期浏览器，在不存在某些属性的时候，仅返回前面属性

                4.  , ||

        
        1.  实践：

            1.  定位媒体类型

                1.  @media print {..}

                2.  @media print, screen {..}

            
            1.  定位媒体功能：

                1.  @media (hover: hover) {..}

                2.  @media (max-width: 12450px) { ... }

                3.  @media (color) { ... }

                4.  @media speech and (aspect-ratio: 11/5) { ... }

            
            1.  级别四的改进：

                1.  支持 max min

                2.  支持 &lt;= &lt;

                3.  not 可以对子项使用

    
    1.  使用编程方法测试媒体查询

        1.  创建媒体查询列表：const mediaQueryList = window.matchMedia("(orientation: portrait)")

        2.  检查查询结果：if(mediaQueryList.matches)

        3.  接收查询提醒：mediaQueryList.addListener(callback)

        4.  终止查询：mediaQueryList.removeListener(callback)

    
    1.  分页媒体：

        1.  介绍：属性控制打印或者能够将内容拆分为离散页面的任何其他媒体内容的呈现。允许以不同的方式设置分页符，控制可打印区域以及左边和右边不同的页面样式，还控制元素内部的分割符。

        2.  属性：

            1.  page-break-before

            2.  page-break-after

            3.  page-break-inside

            4.  @page：用于在打印时，修改某些 CSS 属性

                1.  可以通过 CSS 对象模型接口 CSSPageRule 访问

                2.  size 指定页面盒模型所在的容器大小和方向

                3.  marks 向文档添加剪切或者注册标记

                4.  bleed 指定一个页面盒模型的区域，在该区域的页面内容将被裁减


1.  动画：CSS Animation 是 CSS 的一个模块，它定义了如何用关键帧来随时间推移对 CSS 属性的值进行动画处理。关键帧动画的行为可以通过指定它们的持续时间，它们的重复次数以及他们如何重复来控制。

    1.  使用 CSS 动画：

        1.  简介：

            1.  用处：CSS animations 使得可以将从一个 CSS 样式配置转换到另一个 CSS 样式配置

            2.  组成：描述动画的样式规则和用于指定动画开始、结束以及中间点样式的关键帧

            3.  优点：

                1.  能够非常容易地创建简单动画

                2.  动画运行效果良好（低性能系统上可能会运用跳帧）

                3.  让浏览器控制动画序列，允许浏览器优化性能和效果

        
        1.  配置动画：

            1.  animtation-delay 设置延时，元素加载完成之后到动画序列开始执行的这段时间

                1.  语法：&lt;time&gt;\#;

            
            1.  animation-direction 动画每次运行完后是反向执行还是回到原位置执行

                1.  normal（默认）回到起点重新开始；alternate 交替反向，反向运行时，动画按步后退，同时，带函数功能的函数也反向；reverse 反向运行动画，每周期结束动画由头到尾运行；alternate-reverse 第一次反向，第二次正向，依次循环

            
            1.  animation-duration 动画一个周期的时长

                1.  语法：&lt;time&gt;\#

            
            1.  animation-iteration-count 动画重复次数，infinite 可以指定无限次动画

                1.  语法：&lt;number&gt; | &lt;infinite&gt;

            
            1.  animation-name 指定由 @keyframes 描述的关键帧名称

            2.  animation-play-state 允许暂停和恢复动画:

                1.  running 正在运行; paused 停止；其值可以设置为运行停止停止的交替

            
            1.  animation-timing-function 设置动画速度，即通过建立加速度曲线，设置动画在关键帧之间如何变化：

                1.  见 &lt;timing-function&gt;

            
            1.  animation-fill-mode 动画执行前后如何为目标元素应用样式：

                1.  none 动画未执行时，不会将任何样式应用于目标，而是已经赋予该元素的 CSS 规则；forwards 目标将保留有执行期间遇到的最后一个关键帧计算值。最后一个关键帧取决于 animation-direction 和 animation-iteration-count 值；backwards 应用第一个关键帧中定义的值，并在 animation-delay 期间保留此值；both 将遵循 forwards、backwards 的规则，在两个方向上拓展动画属性

        
        1.  使用 keyframes 定义动画序列：

            1.  当时间定义完毕后，关键帧使用 percentage 来指定动画发生的时间点。0% （from）指定动画的第一时刻，100%（to）指定动画的最终时刻。

            2.  使用动画事件：

                1.  const e = document.getElementId("watchme"); e.addEventListener("animationstart", listener, false); e.className = "slidein";

        
        1.  timing-function

            1.  定时函数：

                1.  cubic-bezier() 定义了一条立方贝塞尔曲线，一般用于动画的平滑变换，也被称为缓动函数（见图形学知识，或见 MDN [<span class="underline">https://developer.mozilla.org/zh-CN/docs/Web/CSS/timing-function</span>](https://developer.mozilla.org/zh-CN/docs/Web/CSS/timing-function)），语法 cubic-bezier(x1, y1, x2, y2)，由于是时间-完成状态函数，P0、P3 点被固定在时间轴的开始和结束

                2.  step() 时间函数，定义了一个以等距步长划分值域的函数，它是一个阶跃函数，其子类有时被称为阶梯函数；step(number\_of\_steps, direction), direction 的 left、right 表示向左、右连续

                3.  常用定时函数关键字

                    1.  Linear 表示定时函数 cubic-bezier(.0, .0, 1., 1.)

                    2.  ease 表示定时函数 cubic-bezier(0.25, 1, 0.25, 1)

                    3.  ease-in 表示定时函数 cubic-bezier(0.42, 0.0, 1.0, 1.0)

                    4.  ease-in-out 表示定时函数 cubic-bezier(0.0, 0.0, 0.58, 1.0)

                    5.  step-start 表示定时函数 steps(1, start)

                    6.  step-end 表示定时函数 steps(1, end)

    
    1.  CSS 动画的提示和技巧

        1.  重新播放动画的唯一效果是删除动画效果，让文档重新计算样式以使得知道重新设计了它，再将动画效果加会该元素；

        2.  调用两次 requestAnimationFrame 第一次等待原效果消失，第二次等待样式重新计算


1.  响应式设计：

    1.  其它略

    2.  视口元标记

        1.  举例：&lt;meta name="viewport" content="width=device-width, initial-scale=1"&gt; 告诉移动浏览器，应将视口宽度设置为设备宽度，并将文档缩放到预期大小的 100%，从而以预期移动优化的大小显示文档

        2.  其它：

            1.  initial-scale 设置页面的初始缩放，设置为 1

            2.  height 设置视口的特定高度

            3.  minimum-scale 设置最小缩放级别

            4.  maximum-scale 设置最大缩放级别

            5.  user-scalable 如果设置为 no，则防止缩放
