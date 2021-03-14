# [SUBLIME TEXT中文文档之](index)minihtml参考

Sublime Text包含一个名为minihtml的自定义HTML和CSS引擎 ，用于在编辑器窗格中显示样式化内容。HTML内容可以在弹出窗口和幻像中显示。

minihtml提供了大多数Web浏览器中的HTML和CSS功能的有限子集。虽然只能实现某些CSS和HTML功能，但它们的设计符合标准。实现的任何功能都应该在minihtml中以与在浏览器中相同的方式运行。

*   [HTML](minihtml#html)
    *   [最佳实践](minihtml#best_practices)
    *   [预定义类](minihtml#predefined_classes)
*   [CSS](minihtml#css)
    *   [单位](minihtml#units)
    *   [颜色](minihtml#colors)
    *   [变量](minihtml#variables)

## HTML

以下标记由默认样式表设置样式：

*   `<html>`
*   `<head>`，`<style>`
*   `<body>`
*   `<h1>`，`<h2>`，`<h3>`，`<h4>`，`<h5>`，`<h6>`
*   `<div>`
*   `<p>`
*   `<ul>`，`<ol>`，`<li>`
*   `<b>`，`<strong>`
*   `<i>`，`<em>`
*   `<u>`
*   `<big>`，`<small>`
*   `<a>`
*   `<code>`，`<var>`，`<tt>`

为几个标签实现了特殊行为：

*   `<a>`\- 可以通过API指定回调来处理锚标记的点击
*   `<img>`\-支持PNG，JPG和GIF的图片`file://`，`res://`和`data:`网址
*   `<ul>`\- 显示`<li>`标签的项目 符号

其他具有特殊行为的HTML标记未实现。这包括标记，如`<input>`，`<button>`，`<table>`等。

### 最佳实践

为了让配色方案作者能够调整弹出窗口和幻像的外观，最好在插件的HTML标签中添加一个唯一`id=""`属性`<body>`。

在`<body>`标记内，添加`<style>`包含不使用选择器的 标记`id`。保留颜色方案中的选择器，以便能够覆盖插件。

~~~html
<body id="my-plugin-feature">
    <style>
        div.error {
            background-color: red;
            padding: 5px;
        }
    </style>
    <div class="error"></div>
</body>
~~~

### 预定义类

当minihtml处理HTML标记时，它会自动向标记添加单个类名`<html>`。类名将是`dark`或`light`，旨在允许在样式幻像和弹出窗口中高级使用CSS。

添加哪个类是基于HSL颜色空间中当前颜色方案的背景颜色的亮度。如果亮度小于0.5，`dark`则添加。如果亮度大于或等于0.5，`light`则将添加。

## CSS

以下列表提供了受支持的属性和值的概述：

*   `display`:*`inline`,`block`,`list-item`,`none`*
*   `margin`:*positive[units](minihtml#units)*  
    `margin-top`:*positive[units](minihtml#units)*  
    `margin-right`:*positive[units](minihtml#units)*  
    `margin-bottom`:*positive[units](minihtml#units)*  
    `margin-left`:*positive[units](minihtml#units)*
*   `position`:*`static`,`relative`*
*   `top`:*positive and negative[units](minihtml#units)*  
    `right`:*positive and negative[units](minihtml#units)*  
    `bottom`:*positive and negative[units](minihtml#units)*  
    `left`:*positive and negative[units](minihtml#units)*
*   `background-color`:[colors](minihtml#colors)
*   `font-family`:*comma-separated list of font families*  
    `font-size`:*positive[units](minihtml#units)*  
    `font-style`:*`normal`,`italic`*  
    `font-weight`:*`normal`,`bold`*  
    `line-height`:*positive[units](minihtml#units)*  
    `text-decoration`:*`none`,`underline`*
*   `color`:[colors](minihtml#colors)
*   `padding`:*positive[units](minihtml#units)*  
    `padding-top`:*positive[units](minihtml#units)*  
    `padding-right`:*positive[units](minihtml#units)*  
    `padding-bottom`:*positive[units](minihtml#units)*  
    `padding-left`:*positive[units](minihtml#units)*
*   `border`:*positive[units](minihtml#units)*||*[border-style](minihtml#border-style)*||*[colors](minihtml#colors)*  
    `border-top`:*positive[units](minihtml#units)*||*[border-style](minihtml#border-style)*||*[colors](minihtml#colors)*  
    `border-right`:*positive[units](minihtml#units)*||*[border-style](minihtml#border-style)*||*[colors](minihtml#colors)*  
    `border-bottom`:*positive[units](minihtml#units)*||*[border-style](minihtml#border-style)*||*[colors](minihtml#colors)*  
    `border-left`:*positive[units](minihtml#units)*||*[border-style](minihtml#border-style)*||*[colors](minihtml#colors)*
*   `border-style`:*`none`,`solid`*  
    `border-top-style`:*[border-style](minihtml#border-style)*  
    `border-right-style`:*[border-style](minihtml#border-style)*  
    `border-bottom-style`:*[border-style](minihtml#border-style)*  
    `border-left-style`:*[border-style](minihtml#border-style)*
*   `border-width`:*positive[units](minihtml#units)*  
    `border-top-width`:*positive[units](minihtml#units)*  
    `border-right-width`:*positive[units](minihtml#units)*  
    `border-bottom-width`:*positive[units](minihtml#units)*  
    `border-left-width`:*positive[units](minihtml#units)*
*   `border-color`:[colors](minihtml#colors)  
    `border-top-color`:[colors](minihtml#colors)  
    `border-right-color`:[colors](minihtml#colors)  
    `border-bottom-color`:[colors](minihtml#colors)  
    `border-left-color`:[colors](minihtml#colors)
*   `border-radius`:*positive[units](minihtml#units)*  
    `border-top-left-radius`:*positive[units](minihtml#units)*  
    `border-top-right-radius`:*positive[units](minihtml#units)*  
    `border-bottom-right-radius`:*positive[units](minihtml#units)*  
    `border-bottom-left-radius`:*positive[units](minihtml#units)*

### 单位

支持的计量单位包括：

*   `rem`
*   `em`
*   `px`
*   `pt`

`rem`建议使用单位，因为它们基于用户的`font_size`设置，并且不会级联。

### 颜色

可通过以下方式指定颜色：

*   命名的颜色：`white`，`tan`，等
*   速记十六进制：`#fff`
*   带alpha的速记十六进制：`#fff8`
*   全十六进制：`#ffffff`
*   完整的十六进制与alpha：`#ffffff80`
*   RGB功能表示法：`rgb(255, 255, 255)`
*   RGBA功能表示法：`rgba(255, 255, 255, 0.5)`
*   HSL功能表示法：`hsl(0, 100%, 100%)`
*   HSLA功能表示法：`hsla(0, 100%, 100%, 0.5)`
*   `transparent`

另外，彩色值可使用CSS颜色模块级别4被修改[颜色-MOD函数](https://drafts.csswg.org/css-color-4/#modifying-colors)与以下调节器：
*   `alpha()`/`a()`
*   `saturation()`/`s()`
*   `lightness()`/`l()`
*   `blend()`
*   `blenda()`

~~~css
.error {
    background-color: color(var(--background) alpha(0.25));
}
~~~

~~~css
.error {
    background-color: color(var(--background) blend(red 50%));
}
~~~

color-mod函数与[变量](minihtml#variables)结合使用最为有用。

### 变量

使用自定义属性和`var()`功能表示法也支持CSS变量。*自定义属性是以那些开头的`--`。*

一个限制是表示`var()`法不能用于多数值的一部分，例如`padding`或`margin`。使用这些聚合属性时，`var()`必须使用符号表示完整值。

#### 预定义变量

加载颜色方案时，背景和前景颜色设置为CSS变量，以及与少数基本颜色相近的最接近颜色。这些都是`html { }`在默认CSS样式表中的规则集中设置的。

*   `var(--background)`
*   `var(--foreground)`
*   `var(--redish)`
*   `var(--orangish)`
*   `var(--yellowish)`
*   `var(--greenish)`
*    `var(--cyanish)`
*   `var(--bluish)`
*   `var(--purplish)`
*   `var(--pinkish)`

选择颜色的算法使用HSL颜色空间，并使用几种启发法来尝试选择适合前景使用的颜色。在不希望自动颜色选择的情况下，颜色方案作者可以使用`html { }`包含在`popupCss`或`phantomCss`设置中的它们自己的规则集 来覆盖适当的值。


如果在选定名称 .sublime-color-scheme 中设置了具有预定义名称之一的变量 ，该值将被使用，而不是试图从配色方案中使用的颜色中找到匹配项。


~~~
html {
    --fg: #f00;
}
.error {
    background-color: var(--fg);
}
~~~