# 

[DOCUMENTATION](index.html)[TOC](minihtml.html#toc)[TOP](minihtml.html#)

minihtml Reference

Version:  
[Dev](minihtml.html#ver-dev)[3.2](minihtml.html#ver-3.2)[3.1](minihtml.html#ver-3.1)[3.0](minihtml.html#ver-3.0)

Sublime Text contains a custom HTML and CSS engine, namedminihtml, for displaying stylized content in editor panes. HTML content can be displayed in both popup windows and phantoms.

minihtmlprovides a limited subset of HTML and CSS features found in most web browsers. While only certain CSS and HTML features may be implemented, they are designed to be standards compliant. Any feature implemented should function the same way inminihtmlas in a browser.

*   [HTML](minihtml.html#html)
    *   [Tags](minihtml.html#tags)
    *   [Attributes](minihtml.html#attributes)
    *   [Protocols](minihtml.html#protocols)4073
    *   [Inline Formatting](minihtml.html#inline_formatting)4073
    *   [Best Practices](minihtml.html#best_practices)
    *   [Predefined Classes](minihtml.html#predefined_classes)
*   [CSS](minihtml.html#css)
    *   [Units](minihtml.html#units)
    *   [Colors](minihtml.html#colors)
    *   [Variables](minihtml.html#variables)

## HTML

### TAGS

The following tags are styled by the default style sheet:

*   `<html>`
*   `<head>`,`<style>`
*   `<body>`
*   `<h1>`,`<h2>`,`<h3>`,`<h4>`,`<h5>`,`<h6>`
*   `<div>`
*   `<p>`
*   `<ul>`,`<ol>`,`<li>`
*   `<b>`,`<strong>`
*   `<i>`,`<em>`
*   `<u>`
*   `<big>`,`<small>`
*   `<a>`
*   `<code>`,`<var>`,`<tt>`

Special behavior is implemented for a few tags:

*   `<a>`– a callback can be specified via the API to handle clicks on anchor tags
*   `<img>`– supports PNG, JPG and GIF images from`file://`,`res://`and`data:`URLs
*   `<ul>`– bullets are displayed for`<li>`tags

Other HTML tags with special behavior are not implemented. This includes tags such as`<input>`,`<button>`,`<table>`, etc.

### ATTRIBUTES

The following attributes are supported:

*   `class`\- for CSS selectors
*   `id`\- for CSS selectors
*   `style`\- inline CSS
*   `href`\- callback-based navigation,[protocols](minihtml.html#protocols)support4073
*   `title`\- tooltips4085

### PROTOCOLS4073

In HTML sheets, popups, annotations and completion item details, the`href=""`attribute of`<a>`tags automatically supports three protocols:

*   `http:`\- standard URL, will use system default browser to open
*   `https:`\- standard URL, will use system default browser to open
*   `subl:`\- a command name and args to invoke a command

Valid`subl:`URL formats include:

*   `subl:*command_name*`
*   `subl:*command_name*{"*arg_name*":*arg_value*,*…*}`

*For popups and annotations it is possible to define an`on_navigate`callback via the API that will be called for any URL except for the`subl:`protocol. This API-based approach was the only way to handle URLs before build4073.*

### INLINE FORMATTING4073

In the details field of completion items, quick panel items and list input items, there is support for a limited, inline formatting subset of minihtml. This allows for some basic text formatting, that respects the theme and color scheme the user has selected.

The supported tags are:

*   `<ahref="">`–[protocols](minihtml.html#protocols)
*   `<b>`
*   `<strong>`
*   `<i>`
*   `<em>`
*   `<u>`
*   `<tt>`
*   `<code>`

### BEST PRACTICES

To allow color scheme authors to tweak the look of popups and phantoms, it is best to add a unique`id=""`attribute to the`<body>`tag of your plugin's HTML.

Within the`<body>`tag, add a`<style>`tag containing selectors that do not use the`id`. Leave that for selectors in color schemes to be able to override the plugin.

~~~
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

### PREDEFINED CLASSES

When minihtml processes the HTML markup, it will automatically add a single class name to the`<html>`tag. The class name will be`dark`or`light`, and is designed to allow for advanced use of CSS in styling phantoms and popups.

Which class is added is based on the lightness, in the HSL color space, of the background color of the current color scheme. If the lightness is less than 0.5,`dark`will be added. If the lightness is greater than or equal to 0.5,`light`will be added.

## CSS

The following list provides an overview of supported properties and values:

*   `display`:*`inline`,`inline-block`4085,`block`,`list-item`,`none`*
*   `margin`:*positive[units](minihtml.html#units)*  
    `margin-top`:*positive[units](minihtml.html#units)*  
    `margin-right`:*positive[units](minihtml.html#units)*  
    `margin-bottom`:*positive[units](minihtml.html#units)*  
    `margin-left`:*positive[units](minihtml.html#units)*
*   `position`:*`static`,`relative`*
*   `top`:*positive and negative[units](minihtml.html#units)*  
    `right`:*positive and negative[units](minihtml.html#units)*  
    `bottom`:*positive and negative[units](minihtml.html#units)*  
    `left`:*positive and negative[units](minihtml.html#units)*
*   `background-color`:[colors](minihtml.html#colors)
*   `font-family`:*comma-separated list of font families*  
    `font-size`:*positive[units](minihtml.html#units)*  
    `font-style`:*`normal`,`italic`*  
    `font-weight`:*`normal`,`bold`*  
    `line-height`:*positive[units](minihtml.html#units)*  
    `text-decoration`:*`none`,`underline`*  
    `text-align`:*`left`,`right`,`center`*4085
*   `color`:[colors](minihtml.html#colors)
*   `padding`:*positive[units](minihtml.html#units)*  
    `padding-top`:*positive[units](minihtml.html#units)*  
    `padding-right`:*positive[units](minihtml.html#units)*  
    `padding-bottom`:*positive[units](minihtml.html#units)*  
    `padding-left`:*positive[units](minihtml.html#units)*
*   `border`:*positive[units](minihtml.html#units)*||*[border-style](minihtml.html#border-style)*||*[colors](minihtml.html#colors)*  
    `border-top`:*positive[units](minihtml.html#units)*||*[border-style](minihtml.html#border-style)*||*[colors](minihtml.html#colors)*  
    `border-right`:*positive[units](minihtml.html#units)*||*[border-style](minihtml.html#border-style)*||*[colors](minihtml.html#colors)*  
    `border-bottom`:*positive[units](minihtml.html#units)*||*[border-style](minihtml.html#border-style)*||*[colors](minihtml.html#colors)*  
    `border-left`:*positive[units](minihtml.html#units)*||*[border-style](minihtml.html#border-style)*||*[colors](minihtml.html#colors)*
*   `border-style`:*`none`,`solid`*  
    `border-top-style`:*[border-style](minihtml.html#border-style)*  
    `border-right-style`:*[border-style](minihtml.html#border-style)*  
    `border-bottom-style`:*[border-style](minihtml.html#border-style)*  
    `border-left-style`:*[border-style](minihtml.html#border-style)*
*   `border-width`:*positive[units](minihtml.html#units)*  
    `border-top-width`:*positive[units](minihtml.html#units)*  
    `border-right-width`:*positive[units](minihtml.html#units)*  
    `border-bottom-width`:*positive[units](minihtml.html#units)*  
    `border-left-width`:*positive[units](minihtml.html#units)*
*   `border-color`:[colors](minihtml.html#colors)  
    `border-top-color`:[colors](minihtml.html#colors)  
    `border-right-color`:[colors](minihtml.html#colors)  
    `border-bottom-color`:[colors](minihtml.html#colors)  
    `border-left-color`:[colors](minihtml.html#colors)
*   `border-radius`:*positive[units](minihtml.html#units)*  
    `border-top-left-radius`:*positive[units](minihtml.html#units)*  
    `border-top-right-radius`:*positive[units](minihtml.html#units)*  
    `border-bottom-right-radius`:*positive[units](minihtml.html#units)*  
    `border-bottom-left-radius`:*positive[units](minihtml.html#units)*
*   `list-style-type`:*`circle`*,*`square`*and*`disc`*4050

### UNITS

Supported units of measurement include:

*   `rem`
*   `em`
*   `px`
*   `pt`

`rem`units are recommended because they are based on the user's`font_size`setting, and they will not cascade.

### COLORS

Colors may be specified via:

*   Named colors:`white`,`tan`, etc
*   Shorthand hex:`#fff`
*   Shorthand hex with alpha:`#fff8`
*   Full hex:`#ffffff`
*   Full hex with alpha:`#ffffff80`
*   RGB functional notation:`rgb(255, 255, 255)`
*   RGBA functional notation:`rgba(255, 255, 255, 0.5)`
*   HSL functional notation:`hsl(0, 100%, 100%)`
*   HSLA functional notation:`hsla(0, 100%, 100%, 0.5)`
*   HWB functional notation:`hwb(0, 20%, 20%)`,`hwb(0, 20%, 20%, 0.5)`3181
*   `transparent`

#### `color()`MOD FUNCTIONPROPRIETARY

Additionally, color values may be modified using the CSS Color Module Level 4 (*05 July 2016*)[color-mod function](https://www.w3.org/TR/2016/WD-css-color-4-20160705/#modifying-colors)with the following adjusters.*Unfortunately in a later draft of CSS Color Module Level 4, the color-mod function was removed.*

*   `alpha()`/`a()`
*   `saturation()`/`s()`3179
*   `lightness()`/`l()`3179
*   `blend()`
*   `blenda()`
*   `min-contrast()`3181PROPRIETARY

~~~
.error {
    background-color: color(var(--background) alpha(0.25));
}
~~~

~~~
.error {
    background-color: color(var(--background) blend(red 50%));
}
~~~

The color-mod function will be most useful in combination with[variables](minihtml.html#variables).

##### `min-contrast()`Adjuster

***The`min-contrast()`adjuster for the`color()`mod function is a non-standard addition that is custom to minihtml.***At the time of implementation, the CSS Color Module Level 4 spec had a`contrast()`adjuster, but it only allowed taking a color and modifying it to provide contrast with itself, as opposed to taking a second color (typically a background) and making sure the foreground has sufficient contrast with the background.

`min-contrast()`accepts two parameters: a background color to measure the contrast against, and a minimum contrast ratio between the "base" color and the background color. The ratio will be a decimal number, typically between`2.0`and`4.5`.

~~~
.error {
    background-color: color(var(--redish) min-contrast(var(--background) 2.5));
}
~~~

Please see the documentation for the[`contrast()`adjuster](https://www.w3.org/TR/2016/WD-css-color-4-20160705/#contrast-adjuster)for details about how the contrast ratio is calculated and how the color is modified to meet it.

### VARIABLES

CSS variables are also supported using custom properties and the`var()`functional notation.*Custom properties are those starting with`--`.*

~~~
html {
    --fg: #f00;
}
.error {
    background-color: var(--fg);
}
~~~

The one limitation is that the`var()`notation can not be used for part of a multi-number value, such as`padding`or`margin`. With those aggregate properties, the`var()`notation must be used for the complete value.

#### PREDEFINED VARIABLES

When a color scheme is loaded, the background and foreground colors are set to CSS variables, along with the closest color found to a handful of basic colors. These are all set in an`html{ }`rule set in the default CSS style sheet.

*   `var(--background)`
*   `var(--foreground)`
*   `var(--accent)`3179
*   `var(--redish)`
*   `var(--orangish)`
*   `var(--yellowish)`
*   `var(--greenish)`
*   `var(--cyanish)`3179
*   `var(--bluish)`
*   `var(--purplish)`
*   `var(--pinkish)`

The algorithm to pick the colors uses the HSL color space and uses several heuristics to try and pick colors that are suitable for foreground use. In the case that the automatic color selection is undesirable, color scheme authors may override the appropriate values with their own`html{ }`rule set contained in the`popupCss`or`phantomCss`settings.

If a variable with one of the predefined names is set in the selected.sublime-color-scheme, that value will be used instead of trying to find a match from the colors used in the color scheme.