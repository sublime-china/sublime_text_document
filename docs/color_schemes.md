# 

[DOCUMENTATION](https://www.sublimetext.com/docs/index.html)[TOC](https://www.sublimetext.com/docs/color_schemes.html#toc)[TOP](https://www.sublimetext.com/docs/color_schemes.html#)

Color Schemes

Version:  
[Dev](https://www.sublimetext.com/docs/color_schemes.html#ver-dev)[3.2](https://www.sublimetext.com/docs/color_schemes.html#ver-3.2)[3.1](https://www.sublimetext.com/docs/color_schemes.html#ver-3.1)[3.0](https://www.sublimetext.com/docs/color_schemes.html#ver-3.0)

The highlighting of source code and prose in Sublime Text is controlled by a color scheme. A*color scheme*assigns colors and font styles to*scopes*, which are assigned to the text by the syntax. The rest of the look of the user interface is controlled by the[theme](https://www.sublimetext.com/docs/themes.html). The theme controls such elements as buttons select lists, the sidebar and tabs.

Sublime Text color schemes are implemented using.sublime-color-schemefiles, containing JSON. Sublime Text also supports a subset of features using the TextMate[.tmTheme format](https://www.sublimetext.com/docs/color_schemes_tmtheme.html).*Before Sublime Text 3.1, only the .tmTheme format was supported.*

*   [Example](https://www.sublimetext.com/docs/color_schemes.html#example)
*   [Colors](https://www.sublimetext.com/docs/color_schemes.html#colors)
*   [Variables](https://www.sublimetext.com/docs/color_schemes.html#variables)
*   [Global Settings](https://www.sublimetext.com/docs/color_schemes.html#global_settings)
    *   [Accents](https://www.sublimetext.com/docs/color_schemes.html#global_settings-accents)
    *   [CSS](https://www.sublimetext.com/docs/color_schemes.html#global_settings-css)
    *   [Gutter](https://www.sublimetext.com/docs/color_schemes.html#global_settings-gutter)
    *   [Diff](https://www.sublimetext.com/docs/color_schemes.html#global_settings-diff)
    *   [Selection](https://www.sublimetext.com/docs/color_schemes.html#global_settings-selection)
    *   [Find](https://www.sublimetext.com/docs/color_schemes.html#global_settings-find)
    *   [Rulers](https://www.sublimetext.com/docs/color_schemes.html#global_settings-rulers)
    *   [Guides](https://www.sublimetext.com/docs/color_schemes.html#global_settings-guides)
    *   [Brackets](https://www.sublimetext.com/docs/color_schemes.html#global_settings-brackets)
    *   [Tags](https://www.sublimetext.com/docs/color_schemes.html#global_settings-tags)
    *   [Shadows](https://www.sublimetext.com/docs/color_schemes.html#global_settings-shadows)
*   [Scope Rules](https://www.sublimetext.com/docs/color_schemes.html#scope_rules)
    *   [Matching](https://www.sublimetext.com/docs/color_schemes.html#matching)
    *   [Naming](https://www.sublimetext.com/docs/color_schemes.html#naming)
    *   [Style Rules](https://www.sublimetext.com/docs/color_schemes.html#style_rules)
    *   [Hashed Syntax Highlighting](https://www.sublimetext.com/docs/color_schemes.html#hashed_syntax_highlighting)
    *   [Examples](https://www.sublimetext.com/docs/color_schemes.html#examples)
*   [Customization](https://www.sublimetext.com/docs/color_schemes.html#customization)
*   [Appendix: CSS Colors](https://www.sublimetext.com/docs/color_schemes.html#css_colors)

## Example

*The following is an example of the format of a.sublime-color-schemefile. A complete color scheme will have many more rules to cover the standard scope names.*

~~~
{
    "name": "Example Color Scheme",
    "globals":
    {
        "background": "rgb(34, 34, 34)",
        "foreground": "#EEEEEE",
        "caret": "white"
    },
    "rules":
    [
        {
            "name": "Comment",
            "scope": "comment",
            "foreground": "#888888"
        },
        {
            "name": "String",
            "scope": "string",
            "foreground": "hsla(50, 100%, 50%, 1)",
        },
        {
            "name": "Number",
            "scope": "constant.numeric",
            "foreground": "#7F00FF",
            "font_style": "italic",
        }
    ]
}
~~~

## Colors

Colors in color schemes may be specified using one of seven formats:

*   **Hex RGB**: A`#`followed by six hex characters, with the first two specifying the red channel, second tow the green channel and the final two the blue channel. Red is written as`#FF0000`. An abbreviated form is available when each of the three pairs use the same value for both characters. Red is written as`#F00`.
*   **Hex RGBA**: Same as Hex RGBA, but with an extra pair of hex characters at the end to specify the alpha channel. Red with 67% opacity is written as`#FF0000AA`. The abbreviated form would be`#F00A`.
*   **RGB functional notation**: A function named`rgb`that accepts three integers in the range 0 to 255. The first integer specifies the red channel, the second the green channel and the third the blue channel. Red is written as`rgb(255, 0, 0)`.
*   **RGBA functional notation**: Identical to the RGB function format, except the name of the function is`rgba`and a fourth parameter is added accepting a value from`0.0`to`1.0`specifying the alpha channel. Red with 50% opacity is written as`rgba(255, 0, 0, 0.5)`.
*   **HSL functional notation**: A function named`hsl`that accepts three values. The first is an integer in the range`0`to`360`specifying the hue. The second is a percentage specifying the saturation. The third is a percentage specifying the lightness. Red is written as`hsl(0, 100%, 50%)`.
*   **HSLA functional notation**: Identical to the HSL function format, except the name of the function is`hsla`and a fourth parameter is added accepting a value from`0.0`to`1.0`specifying the alpha channel. Red with 50% opacity is written as`hsla(0, 100%, 50%, 0.5)`.
*   **HWB functional notation**3181: A function named`hwb`that accepts three or four values. The first is an integer in the range`0`to`360`specifying the hue. The second is a percentage specifying the percentage of white mixed in. The third is a percentage specifying the black mixed in. The optional fourth parameter is a value from`0.0`to`1.0`that controls the opacity. Examples include:`hwb(0, 20%, 20%)`and`hwb(0, 20%, 20%, 0.5)`.
*   **Named**:[CSS color names](https://www.sublimetext.com/docs/color_schemes.html#css_colors).*Please note that while some share names with X11 named colors used in.tmThemefiles, the actual colors tend to differ.*

Additionally, colors may be specified as a[variable](https://www.sublimetext.com/docs/color_schemes.html#variables), and then referenced via the syntax`var(example_var_name)`. Variable references are particularly useful when combined with the[minihtml`color()`mod function](https://www.sublimetext.com/docs/minihtml.html#color-mod_function)and the supported`blend()`,`blenda()`,`alpha()`,`saturation()`,`lightness()`and`min-contrast()`adjusters.

*   **blend() adjuster**: Blends a color into the base. To blend equal parts grey and a base color referenced via variable, in RGB space:`color(var(base_green) blend(#888 50%))`. If colors should be blended in HSL space, use the following form:`color(var(base_green) blend(#888 50% hsl))`. The resulting alpha value is always the alpha channel of the base color.
*   **blenda() adjuster**: Functions the same way as the`blend()`adjuster, but blends the alpha channel of the two colors instead of just using the alpha channel from the base. An example of the blending a partially transparent grey into a green:`color(var(base_green) blenda(#8888 50% hsl))`
*   **alpha() adjuster**: Changes the alpha channel of the base color to the value specified, from`0.0`to`1.0`. Setting the alpha channel to 90%:`color(var(base_green) alpha(0.9))`.*A shorthand name of`a()`is also available for this adjuster.*
*   **saturation() adjuster**3179: Changes the saturation channel of the base color, in the HSL color space, to the value specified, from`0%`to`100%`. Setting the saturation to 90%:`color(var(base_green) saturation(0.9))`. Increasing the saturation by 10%:`color(var(base_green) s(+ 10%))`.*A shorthand name of`s()`is also available for this adjuster.*
*   **lightness() adjuster**3179: Changes the lightness channel of the base color, in the HSL color space, to the value specified, from`0%`to`100%`. Setting the lightness to 90%:`color(var(base_green) lightness(0.9))`. Decreasing the lightness by 10%:`color(var(base_green) l(- 10%))`.*A shorthand name of`l()`is also available for this adjuster.*
*   **min-contrast() adjuster**3181PROPRIETARY: Modifies a color to ensure a minimum contrast ratio against a "background" color. The first parameter is the color to calculate the contrast again, the "background", and the second is a decimal number specifying the minimum contrast ratio. Typical values for the contrast ratio range from`2.0`to`4.5`. Ensure a contrast ratio of 2.5 against the background:`color(var(base_green) min-contrast(var(bg_color) 2.5))`

## Variables

Reusable color definitions may be created in the`variables`key. The names may be any string utilizing the characters`a-z`,`A-Z`,`0-9`,`_`and`-`. The values may be any valid color format.

Variables may be referenced in the global settings and rules, via the syntax`var(example_var_name)`. The following example shows basic variable usage:

~~~
{
    "name": "Example Color Scheme",
    "variables":
    {
        "green": "hsla(153, 80%, 40%, 1)",
        "black": "#111",
        "white": "rgb(242, 242, 242)"
    },
    "globals":
    {
        "background": "var(black)",
        "foreground": "var(white)",
        "caret": "color(var(white) alpha(0.8))"
    },
    "rules":
    [
        {
            "name": "Comment",
            "scope": "comment",
            "foreground": "color(var(black) blend(#fff 50%))"
        },
        {
            "name": "String",
            "scope": "string",
            "foreground": "var(green)",
        },
        {
            "name": "Number",
            "scope": "constant.numeric",
            "foreground": "#7F00FF",
            "font_style": "italic",
        }
    ]
}
~~~

## Global Settings

The following global settings go in the object with the"globals"key.

"background"

The default background color

"foreground"

The default color for text

"invisibles"

The color for whitespace, when rendered.*When not specified, defaults to`foreground`with an opacity of`0.35`.*

"caret"

The color of the caret

"block\_caret"3190

The color of the caret when using a block caret

"block\_caret\_border"4086

The color of the border for a block caret

"block\_caret\_underline"4086

The color of the underline the block caret is drawn as when overlapping with a selection

"block\_caret\_corner\_style"4086

The style of corners to use for block carets. Options include:`round`(the default),`cut`or`square`.

"block\_caret\_corner\_radius"4086

The radius to use when the`block_caret_corner_style`is`round`or`cut`.

"line\_highlight"

The background color of the line containing the caret.*Only used when the`highlight_line`setting is enabled.*

### ACCENTS

"misspelling"

The color to use for the squiggly underline drawn under misspelled words.

"fold\_marker"

The color to use for the marker that indicates content has been folded.

"minimap\_border"

The color of the border drawn around the viewport area of the minimap when the setting`draw_minimap_border`is enabled.*Note that the viewport is normally only visible on hover, unless the`always_show_minimap_viewport`setting is enabled.*

"accent"

A color made available for use by the theme.*The Default theme uses this to highlight modified tabs when the`highlight_modified_tabs`setting is enabled.*

### CSS

CSS is applied to[minihtml](https://www.sublimetext.com/docs/minihtml.html)content created via the popups and phantoms functionality that is exposed through the API. Supported CSS properties are discussed in the[minihtml CSS reference](https://www.sublimetext.com/docs/minihtml.html#css).

Plugins that use minihtml are encouraged to set a unique`id`attribute on the`<body>`tag of generated HTML to allow color schemes to override default plugin styles.

"popup\_css"

CSS passed to popups.

"phantom\_css"

CSS passed to phantoms.*If not specified, uses`popup_css`.*

"sheet\_css"4065

CSS passed to HTML sheets.

### GUTTER

"gutter"

The background color of the gutter

"gutter\_foreground"

The color of line numbers in the gutter

"gutter\_foreground\_highlight"4050

The color of line numbers in the gutter when a line is highlighted

### DIFF

The diff functionality is displayed in the gutter as colored lines for added and modified lines, and a triangle where lines were deleted.

"line\_diff\_width"3186

The width of the diff lines, between 1 and 8

"line\_diff\_added"3189

The color of diff markers for added lines

"line\_diff\_modified"3186

The color of diff markers for modified lines

"line\_diff\_deleted"3189

The color of diff markers for deleted lines

### SELECTION

"selection"

The background color of selected text

"selection\_foreground"

A color that will override the scope-based text color of the selection

"selection\_border"

The color for the border of the selection

"selection\_border\_width"

The width of the selection border, from`0`to`4`.

"inactive\_selection"

The background color of a selection in a view that is not currently focused

"inactive\_selection\_border"4074

The color for the border of the selection in a view that is not currently focused

"inactive\_selection\_foreground"

A color that will override the scope-based text color of the selection in a view that is not currently focused

"selection\_corner\_style"

The style of corners to use on selections. Options include:`round`(the default),`cut`or`square`.

"selection\_corner\_radius"

The radius to use when the`selection_corner_style`is`round`or`cut`.

### FIND

"highlight"

The border color for "other" matches when the*Highlight matches*option is selected in the Find panel. Also used to highlight matches in Find in Files results.

"find\_highlight"

The background color of text matched by the Find panel

"find\_highlight\_foreground"

A color that will override the scope-based text color of text matched by the Find panel

"scroll\_highlight"4050

The color search result positions drawn on top of the scroll bar.

"scroll\_selected\_highlight"4050

The color of the selected search result position drawn on top of the scroll bar.

### RULERS

Ruler locations are set by the`rulers`setting.

"rulers"

The color used to draw rulers.

### GUIDES

Guides are controlled globally by the`draw_indent_guides`setting.

"guide"

The color used to draw indent guides.*Only used if the option`"draw_normal"`is present in the setting`indent_guide_options`.*

"active\_guide"

The color used to draw the indent guides for the indentation levels containing the caret.*Only used if the option`"draw_active"`is present in the setting`indent_guide_options`.*

"stack\_guide"

The color used to draw the indent guides for the parent indentation levels of the indentation level containing the caret.*Only used if the option`"draw_active"`is present in the setting`indent_guide_options`.*

### BRACKETS

Bracket matching is controlled globally by the`match_brackets`setting.

"brackets\_options"

How brackets are highlighted when the caret is next to one. Accepts a space-separated list from the following:

*   `underline`
*   `stippled_underline`
*   `squiggly_underline`
*   `foreground`
*   `bold`
*   `italic`

"brackets\_foreground"

The color to use when drawing the style specified by`brackets_options`.

"bracket\_contents\_options"

How brackets are highlighted when the caret is positioned in between a pair of brackets. Accepts a space-separated list from the following:

*   `underline`
*   `stippled_underline`
*   `squiggly_underline`
*   `foreground`

"bracket\_contents\_foreground"

The color to use when drawing the style specified by`brackets_contents_options`.

### TAGS

Tag matching is controlled globally by the`match_tags`setting.

"tags\_options"

How tags are highlighted when the caret is inside of one. Accepts a space-separated list from the following:

*   `underline`
*   `stippled_underline`
*   `squiggly_underline`
*   `foreground`

"tags\_foreground"

The color to use when drawing the style specified by`tags_options`.

### SHADOWS

"shadow"

The color of the shadow used to show when a text area can be horizontally scrolled

"shadow\_width"

The width of the shadow in device-independent pixels

## Scope Rules

Color schemes interact with the text in a file via scopes. Scopes are set to code or prose tokens via the*syntax*. Scopes are dotted strings, specified from least-to-most specific. For example, the`if`keyword in PHP could be specified via the scope name`keyword.control.php`.

### MATCHING

Color schemes apply colors and font styles to the scopes by matching the dotted labels, starting with the first. Prefix matching is the standard way to have a color scheme apply to multiple syntaxes. Instead of matching`keyword.control.php`, most color schemes will instead assign a color to`keyword`. Matching the first one or two labels in a scope is most common. Including the final label, the syntax name, is uncommon unless a syntax-specific override is desired.

### NAMING

Author of syntaxes can assign whatever scopes they want to a given token. This combined with the fact that there are hundreds of community-maintained syntaxes means that is can be hard to know what scopes to target. The[official Scope Naming guidelines](https://www.sublimetext.com/docs/scope_naming.html)were established to help syntax and color scheme authors use a common set, for better interoperability. The[Usage in Color Schemes](https://www.sublimetext.com/docs/scope_naming.html#color_schemes)section provides a baseline set of scopes that color scheme authors should strive to handle.

### STYLE RULES

Each scope style rule consists of an object containing a"scope"key, along with one or more of the following optional keys:

"name"

The (arbitrary) name for the scope rule

"foreground"

The text color

"background"

The background color

"foreground\_adjust"3179

An adjustment to the"foreground"color, only valid with"background"

"selection\_foreground"

The text color when selected

"font\_style"

Zero or more of the following, separated by spaces:

*   `bold`
*   `italic`
*   `glow`4050
*   `underline`4074
*   `stippled_underline`4075
*   `squiggly_underline`4075

The"foreground\_adjust"key accepts a space-separated list of adjusters that are supported by the[minihtml`color()`mod function](https://www.sublimetext.com/docs/minihtml.html#color-mod_function). It is only supported when the"background"key is also specified, and thus allows modifying all foregrounds used in combination with the background, without having to create different rules for every permutation.

### HASHED SYNTAX HIGHLIGHTING

The"foreground"key supports a special mode called*Hashed Syntax Highlighting*, where each token matching the scope specified will receive a unique color from one, or more, gradients. Some editors refer to this style of highlighting as*Semantic Highlighting*.

To use hashed syntax highlighting, the"foreground"key must have a value that is an array of two or more colors. Sublime Text will create 256 different colors that are linear interpolations (lerp) between the colors provided. The interpolation is done in HSL space.

As Sublime Text highlights the tokens in a file, it will create a hashed value of the token, and use that to pick one of the 256 linear interpolations. Every instance of a given token will use the same color. For instance, each instance of`first_name`would have the same color, but every instance of`name`would have a different color.

For hashed syntax highlighting to be most obvious, the hue difference between the start and end points should be as far apart as possible. Here is an example of using blues, purples and pinks for variable names:

~~~
{
    "scope": "source - punctuation - keyword",
    "foreground": ["hsl(200, 60%, 70%)", "hsl(330, 60%, 70%)"]
}
~~~

### EXAMPLES

The following scope style rule will color all strings as green:

~~~
{
    "name": "Strings",
    "scope": "string",
    "foreground": "#00FF00"
}
~~~

To style all numbers as bold, italic red, use:

~~~
{
    "name": "Numbers",
    "scope": "constant.numeric",
    "foreground": "#FF0000",
    "font_style": "bold italic"
}
~~~

## Customization

Color schemes based on the.sublime-color-schemeformat are specified by filename only, not a package-based file path. This allows users to customize a color scheme by overriding variables or globals, and adding rules.

To create a user-specific customization of a color scheme, create a new file with the same filename as the color scheme, but save it in thePackages/User/directory.

For example, to customize the default Monokai color scheme, create a file namedPackages/User/Monokai.sublime-color-scheme. The following settings will change the background color to be a fully-desaturated grey, the yellow to be more vibrant, and will add a new rule change Python docstrings to be colored the same as strings.

~~~
{
    "variables":
    {
        "yellow": "hsl(54, 100%, 50%)",
    },
    "globals":
    {
        "background": "hsl(70, 0%, 15%)",
    },
    "rules":
    [
        {
            "name": "Python docstrings",
            "scope": "comment.block.documentation.python",
            "foreground": "var(yellow)"
        },
    ]
}
~~~

The contents of the"variables"and"globals"keys are merged, with the user's copy overwriting keys with the same name. For the"rules"array, the user's rules are appended.

## Appendix: CSS Colors

aliceblue  
antiquewhite  
aqua  
aquamarine  
azure  
beige  
bisque  
black  
blanchedalmond  
blue  
blueviolet  
brown  
burlywood  
cadetblue  
chartreuse  
chocolate  
coral  
cornflowerblue  
cornsilk  
crimson  
cyan  
darkblue  
darkcyan  
darkgoldenrod  
darkgray  
darkgreen  
darkgrey  
darkkhaki  
darkmagenta  
darkolivegreen  
darkorange  
darkorchid  
darkred  
darksalmon  
darkseagreen  
darkslateblue  
darkslategray  
darkslategrey  
darkturquoise  
darkviolet  
deeppink  
deepskyblue  
dimgray  
dimgrey  
dodgerblue  
firebrick  
floralwhite  
forestgreen  
fuchsia  
gainsboro  

ghostwhite  
gold  
goldenrod  
gray  
green  
greenyellow  
grey  
honeydew  
hotpink  
indianred  
indigo  
ivory  
khaki  
lavender  
lavenderblush  
lawngreen  
lemonchiffon  
lightblue  
lightcoral  
lightcyan  
lightgoldenrodyellow  
lightgray  
lightgreen  
lightgrey  
lightpink  
lightsalmon  
lightseagreen  
lightskyblue  
lightslategray  
lightslategrey  
lightsteelblue  
lightyellow  
lime  
limegreen  
linen  
magenta  
maroon  
mediumaquamarine  
mediumblue  
mediumorchid  
mediumpurple  
mediumseagreen  
mediumslateblue  
mediumspringgreen  
mediumturquoise  
mediumvioletred  
midnightblue  
mintcream  
mistyrose  
moccasin  

navajowhite  
navy  
oldlace  
olive  
olivedrab  
orange  
orangered  
orchid  
palegoldenrod  
palegreen  
paleturquoise  
palevioletred  
papayawhip  
peachpuff  
peru  
pink  
plum  
powderblue  
purple  
rebeccapurple  
red  
rosybrown  
royalblue  
saddlebrown  
salmon  
sandybrown  
seagreen  
seashell  
sienna  
silver  
skyblue  
slateblue  
slategray  
slategrey  
snow  
springgreen  
steelblue  
tan  
teal  
thistle  
tomato  
turquoise  
violet  
wheat  
white  
whitesmoke  
yellow  
yellowgreen