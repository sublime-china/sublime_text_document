# 

[DOCUMENTATION](index)[TOC](themes#toc)[TOP](themes#)

主题

Version:  
[Dev](themes#ver-dev)[3.2](themes#ver-3.2)[3.1](themes#ver-3.1)[3.0](themes#ver-3.0)

The look of the Sublime Text interface is controlled by themes. The term*theme*refers strictly to the look of the UI – buttons, select lists, the sidebar, tabs and so forth. The highlighting of source code, markup and prose is controlled by a[color scheme](color_schemes).

The theme engine for Sublime Text is based on raster graphics. PNGs are used to prevent texture degradation and provide full alpha control. Each element in the UI can have up to four layers of textures or fills applied, with properties to control opacity and padding. The properties set on each element can be conditionally changed based on user interaction and settings.

Sublime Text themes are implemented via the.sublime-themeformat. It is a JSON format that specifies rules for matching elements and modifying their appearance.

*   [Format](themes#format)
*   [Terminology](themes#terminology)
*   [General Information](themes#general_information)
*   [Inheritance](themes#inheritance)
*   [Variables](themes#variables)
*   [Colors](themes#color_values)
*   [Font Sizes](themes#font_sizes)
*   [Attributes](themes#attributes)
*   [Settings](themes#settings)
*   [Properties](themes#properties)
*   [Elements](themes#elements)
*   [Deprecated](themes#deprecated)
*   [Obsolete](themes#obsolete)
*   [Customization](themes#customization)

## Format

A.sublime-themefile contains a single JSON document. The document should bean object containing a key"rules"with the value of3179an array of rules.An optional key"variables"with an object containing variable/value pairs may be added.3179

*The following is an example of a.sublime-themefile, showing the format. A complete theme will have many more rules to cover all elements used in the UI.*

~~~
{
    "variables":
    {
        "light_gray": "rgb(240, 240, 240)"
    },
    "rules":
    [
        // Set up the textures for a button
        {
            "class": "button_control",
            "layer0.tint": "#000",
            "layer0.opacity": 1.0,
            "layer1.texture": "Theme - Example/textures/button_background.png",
            "layer1.inner_margin": 4,
            "layer1.opacity": 1.0,
            "layer2.texture": "Theme - Example/textures/button_highlight.png",
            "layer2.inner_margin": 4,
            "layer2.opacity": 0.0,
            "content_margin": [4, 8, 4, 8]
        },
        // Show the highlight texture when the button is hovered
        {
            "class": "button_control",
            "attributes": ["hover"],
            "layer2.opacity": 1.0
        },
        // Basic text label style
        {
            "class": "label_control",
            "fg": "var(light_gray)",
            "font.bold": true
        },
        // Brighten labels contained in a button on hover
        {
            "class": "label_control",
            "parents": [{"class": "button_control", "attributes": ["hover"]}],
            "fg": "white"
        }
    ]
}
~~~

## Terminology

The primary contents of a theme is an array of*rules*. Each*rule*object contains a"class"key used to match to an element. In addition to the"class", matching can be further restricted by specifying"attributes","settings","parents"and"platforms"keys.*Properties*affect the look or behavior of the element.

*Variables*allow reusing values throughout different*rules*. Variables may contain any type of syntax, but may only be referenced by top-level keys in a*rule*.3179

Most elements have a single class name, although a few have more than one to allow for both generic, and specific styling. For example, thepopup\_controlclass can be used to set styles for the auto complete and HTML popups, howeverpopup\_control auto\_complete\_popupmay be used to target just the auto complete popup. Multiple"class"values are separated by a space. When a rule specified multiple class names, all must be present on the element for the rule to be applied.

"attributes"are set by Sublime Text, and indicate the state of user interaction, or other information about the nature of an element. The value is an array of strings. Examples include`"hover"`,`"pressed"`and`"dirty"`.

"settings"uses values from.sublime-settingsfiles to filter rules. This allowing theme authors to give users the ability to tweak a theme. Themes may define their own settings, but there are a handful of "default" settings that should be supported if possible. See[Settings](themes#settings)for more details.

The value for the"settings"key may be one of:

*   **array of strings**  
    Each string is the name of boolean settings. To check for a`false`value, prefix the setting name with a`!`.  
    Example:`["bold_folder_labels","!always_show_minimap_viewport"]`.
*   **object**  
    Each key is the name of a setting. A value may be a boolean, string, or array of strings. If an array of strings is used, the setting will be matched if any of the strings in the array matches the user’s value. When comparing to a string, the user’s setting will be coerced to an empty string when not set.  
    Example:`{"bold_folder_labels":true,"file_tab_style":"rounded"}`.

The"parents"key is an array of objects specifying the"class"and"attributes"that must be matched in a parent element.

The"platforms"key is an array of strings specifying the what operating systems to apply the rule to. Valid options include`"osx"`,`"windows"`, and`"linux"`.

*Properties*refer to all other keys in the JSON objects. Some properties are available on all elements, while others are specific to an individual element.

## General Information

The follow sections discuss information about images and how to specify styles.

### SPECIFICITY

Unlike CSS, a Sublime Text theme does not do specificity matching when applying rules to elements. All rules are tested, in order, against each element. Subsequent rules that match will override properties from previous rules.

### TEXTURE IMAGES

All textures in a theme are specified using PNG images. Each texture should be saved at "normal" DPI, where each pixel in the file will be mapped to one device pixel. All file paths in the theme definition should reference the normal DPI version.

A second version of each texture should also be included at double the DPI, with`@2x`added to the filename right before the extension. Sublime Text will automatically use the`@2x`version when being displayed on a high-DPI screen.It is also possible to specify`@3x`variants of textures for screens running at 300% scale or higher3167.

*SVG images are not currently supported.*

### DIMENSIONS

Integer units in a theme referring to dimensions are always specified in device-independent pixels (DIP). Sublime Text automatically handles scaling UI elements based on the screen density.

### PADDING & MARGINS

Padding and margin may be specified in one of three ways:

*   A single integer value – the same value is applied to the left, top, right and bottom
*   An array of two integers – the first value is applied to the left and right, while the second value is applied to the top and bottom
*   An array of four integers – the values are applied, in order, to the left, top, right and bottom

## Inheritance3179

A theme may extend another theme, appending rules and overriding variables. To extend a theme, add a top-level key"extends"to the JSON object, with a string value of the base theme.

~~~
{
    "extends": "Default.sublime-theme",
    "rules":
    [
        {
            "class": "label_control",
            "parents": [{"class": "button_control", "attributes": ["hover"]}],
            "fg": "red"
        }
    ]
}
~~~

The resulting list of rules will start with the base theme rules followed by the extending theme rules. Any variables from the extending theme will override variables with the same name in the base theme. Variable overrides will affect rules both in the base theme and the extending theme.

## Variables3179

Reusable variables may be defined by a JSON object under the top-level key"variables". Variable names are strings, however the value may be a string, number, boolean, array or object. Using a variable requires specifying a string in the format`var(example_variable_name)`.

~~~
{
    "variables":
    {
        "light_gray": "rgb(240, 240, 240)"
    },
    "rules":
    [
        {
            "class": "label_control",
            "fg": "var(light_gray)"
        }
    ]
}
~~~

Variables may be used as the value for any[property](themes#properties), but the variable must be the entire value, it may not be embedded within another variable. The only exception to this rule is that variables may be used as the base color for the CSS`color()`mod function.

## Colors

Colors may be specified by CSS or legacy color syntax:

### CSS COLOR SYNTAX3179

Since Sublime Text build 3177, colors in themes may now be specified using CSS syntax, as supported by[minihtml](minihtml). This includes support for hex,`rgb()`,`hsl()`, variables and the color mod function. Additionally, all[predefined variables](minihtml#predefined_variables)that are derived from the color scheme are available for use.

*The color white, as hex*

~~~
#fff
~~~

*The color white, using`rgb()`functional notation*

~~~
rgb(255, 255, 255)
~~~

*50% opacity white, using`hsla()`functional notation*

~~~
hsla(0, 100%, 100%, 0.5)
~~~

*The closest color to red, as defined in the color scheme*

~~~
var(--redish)
~~~

*50% opacity of the closest color to red, as defined in the color scheme*

~~~
color(var(--redish) a(0.5))
~~~

### LEGACY COLOR SYNTAX

Prior to supporting CSS syntax for colors, themes were only able to specify colors using the following formats, which are now deprecated.

#### RGB

Colors in the RGB color space are specified via an array of 3 or 4 numbers, with the first three being integers ranging from`0`to`255`representing the components red, green and blue. The optional fourth number is a float ranging from`0.0`to`1.0`that controls the opacity of the color.

*The color white, with full opacity*

~~~
[255, 255, 255]
~~~

*The color blue, with 50% opacity*

~~~
[0, 0, 255, 0.5]
~~~

#### HSL

Colors may also be specified using the HSL color space by creating an array of 4 elements, with the first being the string`"hsl"`. The second element is an integer from`0`to`360`specifying the hue. The third is an integer from`0`to`100`specifying the saturation, and the fourth is an integer from`0`to`100`specifying the lightness.

*A dark magenta, with full opacity*

~~~
["hsl", 325, 100, 30]
~~~

A float from`0.0`to`1.0`may be added as a fifth element to control the opacity.

*A bright teal, with 50% opacity*

~~~
["hsl", 180, 100, 75, 0.5]
~~~

#### DERIVED COLORS

It is also possible to derive colors from the current global color scheme. Colors in this format are specified using arrays with specific formats. In all cases, the first element is the base color, which may be`"foreground"`,`"background"`or`"accent"`.

##### Change Opacity of Base Color

To change the opacity of a base color, specify an array of 2 elements, the first being the base color name and the second being a float from`0.0`to`1.0`. The opacity will be set to the float value.

*The color scheme foreground, at 90% opacity*

~~~
["foreground", 0.9]
~~~

##### De-saturate Base Color

To de-saturate a base color, specify an array with 3 elements. The first is the name of the base color, the second is the string`"grayscale"`, and the third is an integer from`0`to`100`which specifies what percentage of the saturation (in HSL color space) of the existing color should be retained. A value of`100`means no change, whereas a value of`0`would cause the color to be completely desaturated.

*The color scheme foreground, with the saturation adjusted to 1/4 of the original value.*

~~~
["foreground", "grayscale", 25]
~~~

##### Tint Base Color

5 and 6-element derived colors allow blending a color into the base color. A 5-element colors uses an RGBA color, whereas a 6-element uses an HSLA. In both cases, the last element, which normally represents the opacity, controls how much of the secondary color is blended into the base.

*The color scheme background, lightened with white*

~~~
["background", 255, 255, 255, 0.1]
~~~

*The color scheme accent, tinted with dark red*

~~~
["accent", "hsl", 0, 100, 30, 0.2]
~~~

Colors derived from the color scheme will always be based on the global color scheme, and will not reflect view-specific color schemes. Certain view-specific controls in the UI have tinting properties that allow using the view-specific color scheme colors.

## Font Sizes

Font sizes may be specified in the formats:

### NUMERIC

An integer or float to specify the size of the font in pixels.  
*Examples:`12`,`13.5`.*

### CSS FORMAT4050

A string of a`px`or`rem`CSS font size.  
*Examples:`12px`,`1.2rem`*

*The`rem`size is based on the global settingfont\_sizefor most elements. Elements that use a different root font size will specify in the description.*

## Attributes

Attributes are specified as an array of strings. Each string is an attribute name. To check for the absence of an attribute, prepend a`!`to the name.

The following attributes are common to all elements:

hover

set whenever the user's mouse is hovered over an element

### LUMINOSITY

Although not available on all elements, many have attributes set based on the approximate luminosity of the current color scheme. Most elements have the attributes set based on the global color scheme. Tabs and the tab background, however, have the attributes based on the color scheme specific to the selected view.

The attributes are assigned based on the`V`value of the background color, when represented as[HSV](https://en.wikipedia.org/wiki/HSL_and_HSV)colors.

file\_light

`V`from`0.60`\-`1.00`

file\_medium

`V`from`0.30`\-`0.59`

file\_medium\_dark

`V`from`0.10`\-`0.29`

file\_dark

`V`from`0.00`\-`0.09`

## Settings

Certain Sublime Text settings are design to influence the UI. Themes should respect these settings and change elements based on them.

overlay\_scroll\_bars

this should affect the style of the scroll bars – generally they should be semi-transparent and theoverlayproperty of thescroll\_area\_controlshould be set to`true`

always\_show\_minimap\_viewport

if the current viewport area should be highlighted on the minimap even when the user is not hovering over the minimap.

bold\_folder\_labels

if folder names in the side bar should have thefont.boldproperty set to`true`.

mouse\_wheel\_switches\_tabs

this is used to control mouse wheel behavior of tabs on Linux. It should be combined with checking for`!enable_tab_scrolling`to change themouse\_wheel\_switchproperty of thetabset\_controlto`false`.

highlight\_modified\_tabs

if the tabs of modified files should be highlighted. This setting should be checked in addition to thedirtyattribute.

show\_tab\_close\_buttons

if tabs should have close buttons

inactive\_sheet\_dimming4095

if sheets other than the one with the attributehighlightedshould be visually de-emphasized usingbackground\_modifier

## Properties

The"rules"key of a.sublime-themefile is a JSON array of of objects describing how UI elements should be styled. Every element in the UI supports the following keys:

layer0.*\**

the bottom-most texture[layer](themes#layer_properties)for the element

layer1.*\**

the second texture[layer](themes#layer_properties)for the element

layer2.*\**

the third texture[layer](themes#layer_properties)for the element

layer3.*\**

the fourth texture[layer](themes#layer_properties)for the element

hit\_test\_level

a float value setting the required opacity of a pixel for a click to be considering a "hit"

### LAYER PROPERTIES

Every element in the UI supports up to four texture layers for displaying fill colors and raster graphics. Each layer has dotted sub-keys in the formatlayer*#*.*sub-key*. Valid sub-keys include:

*layer#.*opacity

a float value from`0.0`to`1.0`that controls the master opacity of the layer.

Example:`0.9`

*layer#.*tint

a[color value](themes#color_values)of a fill color to apply to the layer.

Example:`[255,0,0,127]`

*layer#.*texture

a string of the file path to a PNG image, relative to thePackages/folder.

Example:`"Theme - Default/arrow_right.png"`

*layer#.*inner\_margin

texture images are stretched to fit the element by slicing into a grid of 9 using four lines. See[padding & margins](themes#padding_margin)for valid formats with which to specify the margin used to make the slices.

Example:`[5,2,5,2]`

*layer#.*draw\_center

a boolean that controls if the center rectangle of the 9-grid created via*layer#.*inner\_marginshould be drawn. This is an optimization that allows skipping an unused section of texture.

Example:`false`

*layer#.*repeat

a boolean that controls if the texture should be repeated instead of stretched.

Example:`false`

#### VALUE ANIMATION

Properties specified by floats may be animated over time. Instead of providing a single numeric value, the animation is specified with an object including details of the animation.*Value animation is primarily useful for changing opacity over time.*The object keys are:

target

a float value from`0.0`to`1.0`that controls the destination value

Example:`1.0`

speed

a float value of`1.0`or greater that controls the relative length of time the animation takes

Example:`1.5`

interpolation

an optional string that allow specifying the use of*smoothstep*function instead of the default*linear*function.

Default:`"linear"`  
Example:`"smoothstep"`

#### TEXTURE ANIMATION

The*layer#.*texturesub-key may be an object to specify an animation based on two or more PNG images. The object keys are:

keyframes

an array of strings of the paths to PNG images, in order

Example:`["Theme - Default/spinner.png","Theme - Default/spinner1.png"]`

loop

an optional boolean that controls if the animation should repeat

Default:`false`  
Example:`true`

frame\_time

an optional float specifying how long each frame should be displayed.`1.0`represents 1 second.

Default:`0.0333`(30 fps)  
Example:`0.0166`(60 fps)

### TEXTURE TINTING PROPERTIES

Certain elements have an available tint value set by the background of current color scheme. The tint can be modified and applied to alayer*#*.textureimage.

tint\_index

Controls which layer the tint is applied to. Must be an integer from`0`to`3`.

tint\_modifier

An array of four integers in the range`0`to`255`. The first three are blended into the RGB values from the tint color with the fourth value specifying how much of these RGB modifier values to apply.

### FONT PROPERTIES

Certain textual elements allow setting the following font properties:

font.face

the name of the font face

font.size

the[font size](themes#font_sizes)

font.bold

a boolean, if the font should be bold

font.italic

a boolean, if the font should be italic

color

a[color value](themes#color_values)to use for the text - thefgproperty is an alias for this for backwards compatibility

shadow\_color

a[color value](themes#color_values)to use for the text shadow

shadow\_offset

a 2-element array containing the X and Y offsets of the shadow

opacity4073

a float from`0.0`to`1.0`that is multiplied against the opacity of thecolorandshadow\_colorproperties

### FILTER LABEL PROPERTIES

Labels used in the quick panel have color control based on selection and matching

fg

a[color value](themes#color_values)for unselected, unmatched text

match\_fg

a[color value](themes#color_values)for unselected, matched text

bg

a[color value](themes#color_values)for the background of an unselected row

selected\_fg

a[color value](themes#color_values)for selected, unmatched text

selected\_match\_fg

a[color value](themes#color_values)for selected, matched text

bg

a[color value](themes#color_values)for the background of a selected row

font.face

the name of the font face

font.size

the[font size](themes#font_sizes)

### DATA TABLE PROPERTIES

Row-based tables of data provide the following properties:

dark\_content

if the background is dark – used to set thedarkattribute for scrollbars

row\_padding

padding added to each row, in one of the formats described in[padding & margins](themes#padding_margin)

### STYLED LABEL PROPERTIES

Certain labels allow for additional control over their appearance. They support the properties:

border\_color

a[color value](themes#color_values)for the border of the label

background\_color

a[color value](themes#color_values)for the background of the label

## Elements

The following is an exhaustive list of the elements that comprise the Sublime Text UI, along with supported attributes and properties.

*   [Windows](themes#elements-windows)
*   [Side Bar](themes#elements-side_bar)
*   [Tabs](themes#elements-tabs)
*   [Quick Panel](themes#elements-quick_panel)
*   [Views](themes#elements-views)
*   [Auto Complete](themes#elements-auto_complete)
*   [Panels](themes#elements-panels)
*   [Status Bar](themes#elements-status_bar)
*   [Dialogs](themes#elements-dialogs)
*   [Scroll Bars](themes#elements-scroll_bars)
*   [Inputs](themes#elements-inputs)
*   [Buttons](themes#elements-buttons)
*   [Labels](themes#elements-labels)
*   [Tool Tips](themes#elements-tool_tips)

### WINDOWS

title\_bar

#### Attributes

[luminosity attributes](themes#luminosity_attributes)

#### Properties

fg

a[color value](themes#color_values)to use for the window title text –*Mac 10.10 or newer only*

bg

a[color value](themes#color_values)to use for the title bar background –*Mac 10.10 or newer only*

style4050

the OS style to use for the title bar -`"system"`,`"dark"`*(Mac/Linux only)*or`"light"`*(Mac only)*.

Default:`"system"`

window

This element can not be styled directly, however it can be used in a"parents"key. The luminosity attributes are set based on the global color scheme.

#### Attributes

[luminosity attributes](themes#luminosity_attributes)

#### Properties

*none*

edit\_window

This element contains the main editor window, and is intended for use in a"parents"key.

#### Properties

*none*

switch\_project\_window

This element contains the Switch Project window, and is intended for use in a"parents"key.

#### Properties

*none*

### SIDE BAR

sidebar\_container

The primary sidebar container that handles scrolling

#### Properties

content\_margin

the[margin](themes#padding_margin)around thesidebar\_tree

sidebar\_tree

A tree control containing multipletree\_rows

#### Properties

[data table properties](themes#data_table_properties)

indent

an integer amount to indent each level of the tree structure

indent\_offset

an additional indent applied to every row, for the sake of positioningdisclosure\_button\_controlandclose\_button

indent\_top\_level

a boolean if top-level rows in the tree should be indented

spacer\_rows

a boolean controlling if a blank row should be added between the*Open Files*and*Folders*sections of the sidebar, when both are visible.

tree\_row

A row may contain a header, open file, folder or file

#### Attributes

selectable

when a row is selectable

selected

when an selectable row is selected

expandable

when a row is expandable

expanded

when an expandable row is expanded

sidebar\_heading

One of the "Open Files", "Group #" or "Folders" headings in the sidebar

#### Properties

[font properties](themes#font_properties)

case3179

the case modification to use for the heading -`"upper"`,`"lower"`or`"title"`.

Default:`"upper"`

file\_system\_entry3181

The container holding information about a file or folder in the sidebar. Contains different controls based on which section of the sidebar it is within.

Within the*Open Files*section, this control will contain asidebar\_labelwith the file name, plus possibly avcs\_status\_badge.

Within the*Folders*section, this control will contain a folder or file icon (eithericon\_folder,icon\_folder\_loading,icon\_folder\_duporicon\_file\_type), asidebar\_labelwith the file or folder name, plus possibly avcs\_status\_badge.

#### Attributes

ignored

**Files:**when a file is ignored  
**Folders:**when the entire folder is ignored

untracked

**Files:**when a file is new or not recognized  
**Folders:**when a folder contains one or more untracked files

modified

**Files:**when a file has been changed on disk  
**Folders:**when a folder contains one or more modified files

missing

**Folders:**when one or more of a folder‘s files is no longer on disk

added

**Files:**when a new file has been newly added to the index  
**Folders:**when a folder contains one or more added files

staged

**Files:**when a modified file has been added to the index  
**Folders:**when a folder contains one or more staged files

deleted

**Folders:**when one or more of a folder‘s files has been added to the index for removal

unmerged

**Files:**when a file is in a conflict state and needs to be resolved  
**Folders:**when a folder contains one or more unmerged files

#### Properties

content\_margin

the[margin](themes#padding_margin)around the contained controls

spacing

an integer number of pixels between each contained control

sidebar\_label

Names of open files, folder names and filenames

#### Properties

[font properties](themes#font_properties)

close\_button

A button to the left of each file in the*Open Files*section

#### Properties

content\_margin

for buttons, the[margin](themes#padding_margin)specifies the dimensions

disclosure\_button\_control

An expand/collapse icon present in alltree\_rows that can be expanded

#### Properties

content\_margin

for buttons, the[margin](themes#padding_margin)specifies the dimensions

icon\_folder

Used for a folder once the contents have been fully enumerated

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

icon\_folder\_loading

Used for a folder while the contents are being enumerated

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

icon\_folder\_dup

Used for a folder that has been scanned previously in the sidebar.*This is necessary to prevent a possibly infinite list of files due to recursive symlinks.*

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

icon\_file\_type

The icon for a file. Thelayer0.textureshould not be set since it is determined dynamically based on theiconsetting provided by.tmPreferencesfiles.

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

vcs\_status\_badge3181

An icon contained withinfile\_system\_entrythat is used to display the status of a file or folder with regards to a Git repository it is contained in. This icon will only be shown if the settingshow\_git\_statusis`true`, the file is contained within a Git repository, and the file has some sort of special state within the repository.*A file that is not shown via`git status`and is not ignored via a.gitignorerule will have no icon.*

#### Attributes

ignored

**Files:**when a file is ignored  
**Folders:**when the entire folder is ignored

untracked

**Files:**when a file is new or not recognized  
**Folders:**when a folder contains one or more untracked files

modified

**Files:**when a file has been changed on disk  
**Folders:**when a folder contains one or more modified files

missing

**Folders:**when one or more of a folder‘s files is no longer on disk

added

**Files:**when a new file has been newly added to the index  
**Folders:**when a folder contains one or more added files

staged

**Files:**when a modified file has been added to the index  
**Folders:**when a folder contains one or more staged files

deleted

**Folders:**when one or more of a folder‘s files has been added to the index for removal

unmerged

**Files:**when a file is in a conflict state and needs to be resolved  
**Folders:**when a folder contains one or more unmerged files

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

### TABS

tabset\_control

#### Attributes

[luminosity attributes](themes#luminosity_attributes)

#### Properties

[texture tinting properties](themes#texture_tinting_properties)

content\_margin

the[margin](themes#padding_margin)around thetab\_controls

tab\_overlap

how many DIPs the tabs should overlap

tab\_width

default tab width when space is available

tab\_min\_width

the minimum tab width before tab scrolling occurs

tab\_height

the height of the tabs in DIPs

mouse\_wheel\_switch

if the mouse wheel should switch tabs – this should only be set to`true`if the settingenable\_tab\_scrollingis false

tab\_control

#### Attributes

[luminosity attributes](themes#luminosity_attributes)

dirty

when the associated view has unsaved changed

selected

when the associated view is the active view in its group

transient

when the associate view is a preview and not fully opened

highlighted4050

when the tab is for the sheet with input focus

left4095

when the tab is the left-most tab in the tabset

right4095

when the tab is the right-most tab in the tabset

multiple4095

when the tab is part of a sheet multi-selection

left\_of\_selected4095

when the tab is to the left of a selected tab

left\_of\_hover4095

when the tab is to the left of a hovered tab

right\_of\_selected4095

when the tab is to the right of a selected tab

right\_of\_hover4095

when the tab is to the right of a hovered tab

left\_overhang4095

when the tab is overhanging to the left of its sheet, which can occur during sheet multi-selection

right\_overhang4095

when the tab is overhanging to the right of its sheet, which can occur during sheet multi-selection

#### Properties

[texture tinting properties](themes#texture_tinting_properties)

content\_margin

the[margin](themes#padding_margin)around thetab\_label

max\_margin\_trim

how much of the left and rightcontent\_marginmay be removed when tab space is extremely limited

accent\_tint\_index

Controls which layer the accent tint is applied to. Must be an integer from`0`to`3`. The accent color is specified by the color scheme.

accent\_tint\_modifier

An array of four integers in the range`0`to`255`. The first three are blended into the RGB values from the accent tint color with the fourth value specifying how much of these RGB modifier values to apply.

tab\_label

#### Attributes

transient

when the associate view is a preview and not fully opened

#### Properties

[font properties](themes#font_properties)

tab\_close\_button

#### Properties

content\_margin

for buttons, the[margin](themes#padding_margin)specifies the dimensions

accent\_tint\_index

Controls which layer the accent tint is applied to. Must be an integer from`0`to`3`. The accent color is specified by the color scheme.

accent\_tint\_modifier

An array of four integers in the range`0`to`255`. The first three are blended into the RGB values from the accent tint color with the fourth value specifying how much of these RGB modifier values to apply.

scroll\_tabs\_left\_button

#### Properties

content\_margin

for buttons, the[margin](themes#padding_margin)specifies the dimensions

scroll\_tabs\_right\_button

#### Properties

content\_margin

for buttons, the[margin](themes#padding_margin)specifies the dimensions

show\_tabs\_dropdown\_button

#### Properties

content\_margin

for buttons, the[margin](themes#padding_margin)specifies the dimensions

tab\_connector4095

#### Attributes

left\_overhang4095

when the tab is overhanging to the left of its sheet, which can occur during sheet multi-selection

right\_overhang4095

when the tab is overhanging to the right of its sheet, which can occur during sheet multi-selection

#### Properties

[texture tinting properties](themes#texture_tinting_properties)

### QUICK PANEL

The quick panel is used for the various Goto functionality, the command palette and is available for use by plugins.

overlay\_control

The container for the quick panel, including the input and data table

#### Specializations4050

To allow for targeting theoverlay\_controlwhen the quick panel is being used for specific functionality, the following multi-class selectors are available:

overlay\_control goto\_file

The Goto File quick panel

overlay\_control goto\_symbol

The Goto Symbol quick panel

overlay\_control goto\_symbol\_in\_project

The Goto Symbol in Project quick panel

overlay\_control goto\_line

The Goto Line quick panel

overlay\_control goto\_word

The Goto Anything quick panel, filtering by word

overlay\_control command\_palette

The Command Palette

#### Properties

content\_margin

the[margin](themes#padding_margin)around thequick\_panel

quick\_panel

The data table displayed below the input. Normally the height is dynamic so the layers will not be visible, however the Switch Project window will use layers for the blank space below the filtered options.

#### Properties

[data table properties](themes#data_table_properties)

mini\_quick\_panel\_row

A non-file row inquick\_panel. Contains onequick\_panel\_labelfor each line of text in the row.

#### Attributes

selected

when the row is selected

quick\_panel\_row

A Goto Anything file row inquick\_panel. Also used in the Switch Project window.

Containsquick\_panel\_labelwith the filename, andquick\_panel\_path\_labelfor the file path.

#### Attributes

selected

when the row is selected

kind\_container4050

A container shown to the left of the symbols in the Goto Symbol and Goto Symbol in Project quick panels. It contains thekind\_labeland is used to indicate the kind of the symbol.

*This element is also used inauto\_completeto show the kind of the item being inserted. A"parents"key should be used to distinguish the two uses.*

#### Specializations

The elementkind\_containeris always paired with a second class name indicating the general category the kind belongs to. The categories for usage in the quick panel are as follows:

kind\_container kind\_ambiguous

When the kind of the item is unknown*– no color*

kind\_container kind\_keyword

When the item is a keyword*– typically pink*

kind\_container kind\_type

When the item is a data type, class, struct, interface, enum, trait, etc*– typically purple*

kind\_container kind\_function

When the item is a function, method, constructor or subroutine*– typically red*

kind\_container kind\_namespace

When the item is a namespace or module*– typically blue*

kind\_container kind\_navigation

When the item is a definition, label or section*– typically yellow*

kind\_container kind\_markup

When the item is a markup component, including HTML tags and CSS selectors*– typically orange*

kind\_container kind\_variable

When the item is a variable, member, attribute, constant or parameter*– typically cyan*

kind\_container kind\_snippet

When the item is a snippet*– typically green*

kind\_container kind\_color\_redish

When the plugin author wants to display red

kind\_container kind\_color\_orangish

When the plugin author wants to display orange

kind\_container kind\_color\_yellowish

When the plugin author wants to display yellow

kind\_container kind\_color\_greenish

When the plugin author wants to display green

kind\_container kind\_color\_cyanish

When the plugin author wants to display cyan

kind\_container kind\_color\_bluish

When the plugin author wants to display blue

kind\_container kind\_color\_purplish

When the plugin author wants to display puple

kind\_container kind\_color\_pinkish

When the plugin author wants to display pink

kind\_container kind\_color\_dark

When the plugin author wants to display a dark background

kind\_container kind\_color\_light

When the plugin author wants to display a light background

#### Properties

content\_margin

a[margin](themes#padding_margin)that is added around thekind\_label

kind\_label4050

A label showing a single unicode character, contained within thekind\_container

*This element is also used inauto\_completeto show the kind of the item being inserted. A"parents"key should be used to distinguish the two uses.*

#### Properties

[font properties](themes#font_properties)

symbol\_container4050

A container around thequick\_panel\_labelwhen showing the Goto Symbol or Goto Symbol in Project symbol lists

#### Properties

content\_margin

a[margin](themes#padding_margin)that is added around thequick\_panel\_label

quick\_panel\_label

Filenames inquick\_panel\_rowand all text inmini\_quick\_panel\_row

#### Properties

[filter label properties](themes#filter_label_properties)

quick\_panel\_path\_label

File paths inquick\_panel\_row

#### Properties

[filter label properties](themes#filter_label_properties)

quick\_panel\_label hint4083

Annotations show to the right-hand side ofquick\_panel\_row

#### Properties

[filter label properties](themes#filter_label_properties)

quick\_panel\_detail\_label4083

Detail rows inquick\_panel\_row

#### Properties

color

a[color value](themes#color_values)to use for the text

link\_color

a[color value](themes#color_values)to use for links

monospace\_color

a[color value](themes#color_values)to use for monospace text

monospace\_background\_color

a[color value](themes#color_values)to use for the background of monospace text

### SHEETS

sheet\_contents4095

A sheet can have text, image or HTML contents

#### Attributes

[luminosity attributes](themes#luminosity_attributes)

highlighted4095

when the sheet has input focus

#### Properties

background\_modifier

A string with a space-separated list of adjusters that are supported by the[minihtml`color()`mod function](minihtml#color-mod_function). Used for implementing inactive sheet dimming.

### VIEWS

text\_area\_control

This element can not be styled directly since that is controlled by the color scheme, however it can be used in a"parents"key.

#### Attributes

[luminosity attributes](themes#luminosity_attributes)

#### Properties

*none*

grid\_layout\_control

The borders displayed between views when multiple groups are visible

#### Properties

*no layer support*

border\_color

a[color value](themes#color_values)to use for the border

border\_size

an integer of the border size in DIPs

minimap\_control

Control over the display of the viewport projection on the minimap

#### Properties

*no layer support*

viewport\_color

a[color value](themes#color_values)to fill the viewport projection with

viewport\_opacity

a float from`0.0`to`1.0`specifying the opacity of the viewport projection

fold\_button\_control

Code folding buttons in the gutter

#### Attributes

expanded

when a section of code is unfolded

#### Properties

content\_margin

for buttons, the[margin](themes#padding_margin)specifies the dimensions

popup\_control html\_popup

The primary container for the HTML popups used by*Show Definitions*and third-party packages. The tint of the scroll bar will be set to the background color of the HTML document.

popup\_control annotation\_popup4050

The primary container for the annotations added to the right-hand side of the editor pane by build systems and third-party packages.

annotation\_close\_button4050

The close button shown at the left edge ofannotation\_popup.

#### Properties

content\_margin

for buttons, the[margin](themes#padding_margin)specifies the dimensions

### AUTO COMPLETE

popup\_control auto\_complete\_popup

The primary container for the auto complete popup

auto\_complete

The data table for completion data. The tint is set based on the background color of the color scheme applied to the view the popup is displayed in.

#### Properties

[data table properties](themes#data_table_properties)[texture tinting properties](themes#texture_tinting_properties)

table\_row

A row inauto\_complete

#### Attributes

selected

when the user has highlighted a completion

kind\_container4050

A container shown to the left of an auto complete item, which contains thekind\_labeland is used to indicate the kind of the item

*This element is also used in thequick\_panelfor Goto Symbol and Goto Symbol in Project. A"parents"key should be used to distinguish the two uses.*

#### Specializations

The elementkind\_containeris always paired with a second class name indicating the general category the kind belongs to. The categories for usage in the auto complete window are as follows:

kind\_container kind\_ambiguous

When the kind of the item is unknown*– no color*

kind\_container kind\_keyword

When the item is a keyword*– typically pink*

kind\_container kind\_type

When the item is a data type, class, struct, interface, enum, trait, etc*– typically purple*

kind\_container kind\_function

When the item is a function, method, constructor or subroutine*– typically red*

kind\_container kind\_namespace

When the item is a namespace or module*– typically blue*

kind\_container kind\_navigation

When the item is a definition, label or section*– typically yellow*

kind\_container kind\_markup

When the item is a markup component, including HTML tags and CSS selectors*– typically orange*

kind\_container kind\_variable

When the item is a variable, member, attribute, constant or parameter*– typically cyan*

kind\_container kind\_snippet

When the item is a snippet*– typically green*

#### Properties

content\_margin

a[margin](themes#padding_margin)that is added around thekind\_label

kind\_label4050

A label showing a single unicode character, contained within thekind\_container

*This element is also used in thequick\_panelfor Goto Symbol and Goto Symbol in Project. Aparentskey should be used to distinguish the two uses.*

#### Properties

[font properties](themes#font_properties)

*The`rem`root font size is based on the font size of the editor control the auto complete window is being shown for.*

trigger\_container4050

A container around theauto\_complete\_label

#### Properties

content\_margin

a[margin](themes#padding_margin)that is added around theauto\_complete\_label

auto\_complete\_label

Text in atable\_row

#### Properties

[filter label properties](themes#filter_label_properties)

fg\_blend

a boolean controlling if thefg,match\_fg,selected\_fg, andselected\_match\_fgvalues should be blended onto the foreground color from the color scheme of the current view

auto\_complete\_label auto\_complete\_hint4073

The "annotation" hint displayed at the right-hand-side of atable\_row

#### Properties

[font properties](themes#font_properties)

*The`rem`root font size is based on the font size of the editor control the auto complete window is being shown for.*

fg\_blend

a boolean controlling if thecolorvalue should be blended onto the foreground color from the color scheme of the current view

auto\_complete\_detail\_pane4050

A detail pane displayed below the list of auto complete items, containing theauto\_complete\_infospacer, withauto\_complete\_kind\_name\_labelandauto\_complete\_detailsinside

#### Properties

content\_margin

a[margin](themes#padding_margin)that is added around the child controls

auto\_complete\_info4050

Provides spacing betweenauto\_complete\_kind\_name\_labelandauto\_complete\_details

#### Properties

spacing

an integer number of pixels between each contained control

auto\_complete\_kind\_name\_label4050

A label used to display the name of the auto complete kind

#### Properties

[font properties](themes#font_properties)

*The`rem`root font size is based on the font size of the editor control the auto complete window is being shown for.*

[styled label properties](themes#styled_label_properties)

auto\_complete\_details4050

A single-line HTML control used to display the details of the auto complete item

#### Properties

font.face

the name of the font face

font.size

the[font size](themes#font_sizes)\-*the`rem`root font size is based on the font size of the editor control the auto complete window is being shown for*

color

a[color value](themes#color_values)to use for the text

link\_color

a[color value](themes#color_values)to use for links

monospace\_color

a[color value](themes#color_values)to use for monospace text

monospace\_background\_color

a[color value](themes#color_values)to use for the background of monospace text

### PANELS

panel\_control find\_panel

The container for the Find and Incremental Find panels.

#### Properties

content\_margin

the[margin](themes#padding_margin)around the panel contents

panel\_control replace\_panel

The container for the Replace panel.

#### Properties

content\_margin

the[margin](themes#padding_margin)around the panel contents

panel\_control find\_in\_files\_panel

The container for the Find in Files panel.

#### Properties

content\_margin

the[margin](themes#padding_margin)around the panel contents

panel\_control input\_panel

The container for the input panel, which is available via the API and used for things like file renaming.

#### Properties

content\_margin

the[margin](themes#padding_margin)around the panel contents

panel\_control console\_panel

The container for the Console.

#### Properties

content\_margin

the[margin](themes#padding_margin)around the panel contents

panel\_control output\_panel

The container for the output panel, which is available via the API and used for build results.

#### Properties

content\_margin

the[margin](themes#padding_margin)around the panel contents

panel\_control switch\_project\_panel

The container for the input in the Switch Project window.

#### Properties

content\_margin

the[margin](themes#padding_margin)around the panel contents

panel\_grid\_control

The layout grid used to position inputs on the various panels.

#### Properties

*no layer support*

inside\_spacing

an integer padding to place between each cell of the grid

outside\_vspacing

an integer padding to place above and below the grid

outside\_hspacing

an integer padding to place to the left and right of the grid

panel\_close\_button

The button to close the open panel

#### Properties

content\_margin

for buttons, the[margin](themes#padding_margin)specifies the dimensions

### STATUS BAR

status\_bar

#### Attributes

panel\_visible

when a panel is displayed above the status bar

#### Properties

content\_margin

the[margin](themes#padding_margin)around thesidebar\_button\_control,status\_containerandstatus\_buttonss

panel\_button\_control<4050

The panel switcher button on the left side of the status bar

#### Properties

content\_margin

for buttons, the[margin](themes#padding_margin)specifies the dimensions

sidebar\_button\_control4050

The sidebar/panel switcher button on the left side of the status bar

#### Properties

content\_margin

for buttons, the[margin](themes#padding_margin)specifies the dimensions

status\_container

The area that contains the current status message

#### Properties

content\_margin

the[margin](themes#padding_margin)around the status message

status\_button

The status buttons that display, and allow changing, the indentation, syntax, encoding and line endings

#### Properties

content\_margin

for buttons, the[margin](themes#padding_margin)specifies the dimensions

min\_size

an array of two integers specifying the minimum width and height of a button, in DIPs

vcs\_status3181

The container holding thevcs\_branch\_icon,label\_controlwith the current branch name, andvcs\_changes\_annotationcontrol

#### Properties

content\_margin

the[margin](themes#padding_margin)around the contained controls

spacing

an integer number of pixels between each contained control

vcs\_branch\_icon3181

An icon shown to the left of the current branch name

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

vcs\_changes\_annotation3181

Displays the number of files that have been added, modified or deleted

#### Properties

[font properties](themes#font_properties)[styled label properties](themes#styled_label_properties)

### DIALOGS

dialog

The Indexer Status and Update windows both use this class for the window background

progress\_bar\_control

The progress bar container. The progress bar is displayed in the Update window used for updates on Mac and Windows.

progress\_gauge\_control

The bar representing the progress completed so far

#### Properties

content\_margin

the[margin](themes#padding_margin)specifies the height of the bar

### SCROLL BARS

scroll\_area\_control

The scroll area contains the element being scrolled, along with the bar, track and puck.

#### Attributes

scrollable3186

when the control can be scrolled vertically

hscrollable3186

when the control can be scrolled horizontally

#### Properties

content\_margin

a[margin](themes#padding_margin)that is added around the content being scrolled

overlay

sets the scroll bars to be rendered on top of the content

left\_shadow

a[color value](themes#color_values)to use when drawing a shadow to indicate the area can be scrolled to the left

left\_shadow\_size

an integer of the width of the shadow to draw when the area can be scrolled to the left

top\_shadow

a[color value](themes#color_values)to use when drawing a shadow to indicate the area can be scrolled to the top

top\_shadow\_size

an integer of the height of the shadow to draw when the area can be scrolled to the top

right\_shadow

a[color value](themes#color_values)to use when drawing a shadow to indicate the area can be scrolled to the right

right\_shadow\_size

an integer of the width of the shadow to draw when the area can be scrolled to the right

bottom\_shadow

a[color value](themes#color_values)to use when drawing a shadow to indicate the area can be scrolled to the bottom

bottom\_shadow\_size

an integer of the height of the shadow to draw when the area can be scrolled to the bottom

scroll\_bar\_control

The scroll bar contains the scroll track. The tint is set based on the background color of the element being scrolled.

#### Attributes

dark

when the scroll area content is dark, necessitating a light scroll bar

horizontal

when the scroll bar should be horizontal instead of vertical

#### Properties

[texture tinting properties](themes#texture_tinting_properties)

content\_margin

a[margin](themes#padding_margin)that is added around the scroll track

scroll\_track\_control

The track that the puck runs along. The tint is set based on the background color of the element being scrolled.

#### Attributes

dark

when the scroll area content is dark, necessitating a light scroll bar

horizontal

when the scroll bar should be horizontal instead of vertical

#### Properties

[texture tinting properties](themes#texture_tinting_properties)

scroll\_corner\_control

The dead space in the bottom right of ascroll\_area\_controlwhen both the vertical and horizontal scroll bars are being shown.

#### Attributes

dark

when the scroll area content is dark, necessitating a light scroll bar

#### Properties

[texture tinting properties](themes#texture_tinting_properties)

puck\_control

The scroll puck, or handle. The tint is set based on the background color of the element being scrolled.

#### Attributes

dark

when the scroll area content is dark, necessitating a light scroll bar

horizontal

when the scroll bar should be horizontal instead of vertical

#### Properties

[texture tinting properties](themes#texture_tinting_properties)

### INPUTS

text\_line\_control

The text input used by the Quick Panel, Find, Replace, Find in Files and Input panels.

#### Properties

content\_margin

the[margin](themes#padding_margin)around the text

color\_scheme\_tint

a[color value](themes#color_values)to use to tint the background of the color scheme

color\_scheme\_tint\_2

a[color value](themes#color_values)to use to add a secondary tint to the background of the color scheme

dropdown\_button\_control

The button to close the open panel

#### Properties

content\_margin

for buttons, the[margin](themes#padding_margin)specifies the dimensions

### BUTTONS

button\_control

Text buttons

#### Attributes

pressed

set when a button is pressed down

#### Properties

min\_size

an array of two integers specifying the minimum width and height of a button, in DIPs

icon\_button\_group

A grid controlling the spacing of related icon buttons

#### Properties

*no layer support*

spacing

an integer number of pixels between each button in the group

icon\_button\_control

Small icon-based buttons in the Find, Find in Files, and Replace panels

#### Attributes

selected

when an icon button is toggled on

left

when the button is the left-most button in a group

right

when the button is the right-most button in a group

icon\_regex

The button to enable regex mode in the Find, Find in Files and Replace panels

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

icon\_case

The button to enable case-sensitive mode in the Find, Find in Files and Replace panels

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

icon\_whole\_word

The button to enable whole-word mode in the Find, Find in Files and Replace panels

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

icon\_wrap

The button to enable search wrapping when using the Find and Replace panels

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

icon\_in\_selection

The button to only search in the selection when using the Find and Replace panels

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

icon\_highlight

The button to enable highlighting all matches in the Find and Replace panels

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

icon\_preserve\_case

The button to enable preserve-case mode when using the Replace panel

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

icon\_context

The button to show context around matches when using the Find in Files panel

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

icon\_use\_buffer

The button to display results in a buffer, instead of an output panel, when using the Find in Files panel

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

icon\_use\_gitignore4073

The button to toggle using.gitignoreto filter files in the Find in Files panel

#### Properties

content\_margin

for icons, the[margin](themes#padding_margin)specifies the dimensions

### LABELS

label\_control

Labels are shown in the Find, Replace, Find in File and Input panels. Additionally, labels are used in the Update window, on textual buttons and for the text in thestatus\_container.

*Targeting specific labels can be accomplished by using the"parents"key.*

#### Properties

[font properties](themes#font_properties)

title\_label\_control

The title label is used in the About window.

#### Properties

[font properties](themes#font_properties)

### TOOL TIPS

tool\_tip\_control

Tool tips shown when hovering over tabs and buttons

#### Properties

content\_margin

the[margin](themes#padding_margin)around the tool tip text

tool\_tip\_label\_control

Text shown in a tool tip

#### Properties

[font properties](themes#font_properties)

## Deprecated

### COLOR VALUES

Before build 3127, the only way to specify opacity in colors was by using a 4-element array containing all integers from`0`to`255`. The fourth element controlled the opacity, such that`0`was fully transparent and`255`was fully opaque. The preferred format is now to use a float from`0.0`to`1.0`.

## Obsolete

As the UI of Sublime Text has adapted over time, certain elements and properties are no longer applicable or supported.

### ELEMENTS

Thepanel\_button\_controlelement was removed from the status bar and replaced bysidebar\_button\_control.4050

Thesheet\_container\_controlelement is never visible to users in recent versions of Sublime Text.

An element namedicon\_reverseused to exist in the find panel to control if searching would move forward or backwards in the view. This is now controlled by the*Find*and*Find Prev*buttons.

The element namedquick\_panel\_score\_labelis no longer present in the Goto Anything quick panel.

### PROPERTIES

Theblurproperty used to be supported to blur the pixel data behind an element, however it is not currently supported for implementation reasons.

## Customization

Users can customize a theme by creating a file with new rules that will be appended to the original theme definition.

To create a user-specific customization of a theme, create a new file with the same filename as the theme, but save it in thePackages/User/directory.

For example, to customize the Default theme, create a file namedPackages/User/Default.sublime-theme. Adding the following rules to that file will increase the size of the text in the sidebar.

~~~
[
    {
        "class": "sidebar_heading",
        "font.size": 15,
    },
    {
        "class": "sidebar_label",
        "font.size": 14
    }
]
~~~