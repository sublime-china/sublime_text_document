# 

[DOCUMENTATION](index)[TOC](api_reference#toc)[TOP](api_reference#)

API Reference

Version:  
[Dev](api_reference#ver-dev)[3.2](api_reference#ver-3.2)[3.1](api_reference#ver-3.1)[3.0](api_reference#ver-3.0)

### GENERAL INFORMATION

*   [Example Plugins](api_reference#example_plugins)
*   [Plugin Lifecycle](api_reference#plugin_lifecycle)
*   [Threading](api_reference#threading)
*   [Units and Coordinates](api_reference#units_coordinates)
*   [Types](api_reference#types)

### CORE COMPONENTS

*   [`sublime`Module](api_reference#sublime)
*   [`Sheet`Class](api_reference#sublime.Sheet)
*   [`Buffer`Class](api_reference#sublime.Buffer)4081
*   [`View`Class](api_reference#sublime.View)
*   [`Selection`Class](api_reference#sublime.Selection)
*   [`Region`Class](api_reference#sublime.Region)
*   [`Phantom`Class](api_reference#sublime.Phantom)
*   [`PhantomSet`Class](api_reference#sublime.PhantomSet)
*   [`Edit`Class](api_reference#sublime.Edit)
*   [`Syntax`Class](api_reference#sublime.Syntax)4081
*   [`TextChange`Class](api_reference#sublime.TextChange)4050
*   [`HistoricPosition`Class](api_reference#sublime.HistoricPosition)4050
*   [`completion`Value](api_reference#type-completion)
*   [`CompletionItem`Class](api_reference#sublime.CompletionItem)4050
*   [`event`Dict](api_reference#type-event_dict)
*   [`kind`Tuple](api_reference#type-kind_tuple)4050
*   [`CompletionList`Class](api_reference#sublime.CompletionList)4050
*   [`QuickPanelItem`Class](api_reference#sublime.QuickPanelItem)4083
*   [`SymbolRegion`Class](api_reference#sublime.SymbolRegion)4085
*   [`Window`Class](api_reference#sublime.Window)
*   [`SymbolLocation`Class](api_reference#sublime.SymbolLocation)4085
*   [`Settings`Class](api_reference#sublime.Settings)
*   [`ListInputItem`Class](api_reference#sublime.ListInputItem)4095

### PLUGIN EXTENSION POINTS

*   [`sublime_plugin`Module](api_reference#sublime_plugin)
*   [`EventListener`Class](api_reference#sublime_plugin.EventListener)
*   [`ViewEventListener`Class](api_reference#sublime_plugin.ViewEventListener)
*   [`TextChangeListener`Class](api_reference#sublime.TextChangeListener)4081
*   [`ApplicationCommand`Class](api_reference#sublime_plugin.ApplicationCommand)
*   [`WindowCommand`Class](api_reference#sublime_plugin.WindowCommand)
*   [`TextCommand`Class](api_reference#sublime_plugin.TextCommand)
*   [`TextInputHandler`Class](api_reference#sublime_plugin.TextInputHandler)3154
*   [`ListInputHandler`Class](api_reference#sublime_plugin.ListInputHandler)3154

## General Information

### EXAMPLE PLUGINS

Several pre-made plugins come with Sublime Text, you can find them in theDefaultpackage:

*   Packages/Default/exec.pyUses phantoms to display errors inline
*   Packages/Default/font.pyShows how to work with settings
*   Packages/Default/goto\_line.pyPrompts the user for input, then updates the selection
*   Packages/Default/mark.pyUsesadd\_regions()to add an icon to the gutter
*   Packages/Default/show\_scope\_name.pyUses a popup to show the scope names at the caret
*   Packages/Default/arithmetic.pyAccepts an input from the user when run via the Command Palette

### PLUGIN LIFECYCLE

At importing time, plugins may not call any API functions, with the exception of:

*   `sublime.version()`
*   `sublime.platform()`
*   `sublime.arch()`
*   `sublime.channel()`
*   `sublime.executable_path()`4081
*   `sublime.packages_path()`4081
*   `sublime.installed_packages_path()`4081
*   `sublime.cache_path()`4081

If a plugin defines a module level functionplugin\_loaded(), this will be called when the API is ready to use. Plugins may also defineplugin\_unloaded(), to get notified just before the plugin is unloaded.

### THREADING

All API functions are thread-safe, however keep in mind that from the perspective of code running in an alternate thread, application state will be changing while the code is running.

### UNITS AND COORDINATES

API functions that accept or return coordinates or dimensions do so using device-independent pixel (dip) values. While in some cases these will be equivalent to device pixels, this is often not the case. Per the CSS specification,[minihtml](minihtml)treats the`px`unit as device-independent.

### TYPES

This documentation generally refers to simply Python data types. Some type names are classes documented herein, however there are also a few custom type names that refer to construct with specific semantics:

location

a tuple of`(str,str,(int,int))`that contains information about a location of a symbol. The first string is the absolute file path, the second is the file path relative to the project, the third element is a two-element tuple of the row and column.

point

anintthat represents the offset from the beginning of the editor buffer. TheViewmethodstext\_point()androwcol()allow converting to and from this format.

value

any of the Python data typesbool,int,float,str,listordict.

dip

afloatthat represents a device-independent pixel.

vector

a tuple of`(dip,dip)`representing`x`and`y`coordinates.

CommandInputHandler

a subclass of either[TextInputHandler](api_reference#sublime_plugin.TextInputHandler)or[ListInputHandler](api_reference#sublime_plugin.ListInputHandler).

## sublimeModule

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| set\_timeout(callback, delay) | `None` | Runs thecallbackin the main thread after the givendelay(in milliseconds). Callbacks with an equal delay will be run in the order they were added. |  |
| set\_timeout\_async(callback, delay) | `None` | Runs thecallbackon an alternate thread after the givendelay(in milliseconds). |  |
| error\_message(string) | `None` | Displays an error dialog to the user. |  |
| message\_dialog(string) | `None` | Displays a message dialog to the user. |  |
| ok\_cancel\_dialog(string, ) | bool | Displays an*ok / cancel*question dialog to the user. Ifok\_titleis provided, this may be used as the text on the ok button. Returns`True`if the user presses the ok button. |  |
| yes\_no\_cancel\_dialog(string, , ) | int | Displays a*yes / no / cancel*question dialog to the user. Ifyes\_titleand/orno\_titleare provided, they will be used as the text on the corresponding buttons on some platforms. Returns`sublime.DIALOG_YES`,`sublime.DIALOG_NO`or`sublime.DIALOG_CANCEL`. |  |
| open\_dialog(callback, , , , ) | `None` | 
Presents the user with a file dialog for the purpose of opening a file, and passes the resulting file path tocallback.

callback

A callback to accept the result of the user’s choice. If the user cancels the dialog,`None`will be passed. If a file is selected, astrcontaining the path will be passed. If the parametermulti\_selectis`True`, alistofstrfile paths will be passed.

file\_types

A specification of allowable file types. This parameter should be alistcontaining 2-elementtuples:

*   Astrcontaining a description
*   Alistofstrwith valid file extensions

*Example:*

~~~
[
    ('Python source files', ['py', 'py3', 'pyw']),
    ('C source files', ['c', 'h'])
]
~~~

directory

Astrof the directory to open the file dialog to. If not specified, will use the directory of the current view.

multi\_select

Aboolto indicate that the user should be allowed to select multiple files

allow\_folders

Aboolto indicate that the user should be allowed to select folders

 | 4075 |
| save\_dialog(callback, , , , ) | `None` | 

Presents the user with file dialog for the purpose of saving a file, and passes the result tocallback.

callback

A callback to accept the result of the user’s choice. If the user cancels the dialog,`None`will be passed. If a file is selected, astrcontaining the path will be passed.

file\_types

A specification of allowable file types. This parameter should be alistcontaining 2-elementtuples:

*   Astrcontaining a description
*   Alistofstrwith valid file extensions

*Example:*

~~~
[
    ('Python source files', ['py', 'py3', 'pyw']),
    ('C source files', ['c', 'h'])
]
~~~

directory

Astrof the directory to open the file dialog to. If not specified, will use the directory of the current view.

name

Astrwith the default file name

extension

Astrcontaining the default file extension

 | 4075 |
| select\_folder\_dialog(callback, , ) | `None` | 

Presents the user with a file dialog for the purpose of selecting a folder, and passes the result tocallback.

callback

A callback to accept the result of the user’s choice. If the user cancels the dialog,`None`will be passed. If a folder is selected, astrcontaining the path will be passed. If the parametermulti\_selectis`True`, alistofstrfolder paths will be passed.

directory

Astrof the directory to open the file dialog to. If not specified, will use the directory of the current view.

multi\_select

Aboolto indicate that the user should be allowed to select multiple folders

 | 4075 |
| load\_resource(name) | str | Loads the given resource. Thenameshould be in the format`"Packages/Default/Main.sublime-menu"`. |  |
| load\_binary\_resource(name) | bytes | Loads the given resource. Thenameshould be in the format`"Packages/Default/Main.sublime-menu"`. |  |
| find\_resources(pattern) | \[str\] | Finds resources whose file name matches the givenpattern. |  |
| ui\_info() | dict | Returns information about the user interface, including top-level keyssystem,themeandcolor\_scheme | 4096 |
| list\_syntaxes() | \[[Syntax](api_reference#sublime.Syntax)\] | Returns a list of all available syntaxes | 4081 |
| syntax\_from\_path(path) | [Syntax](api_reference#sublime.Syntax)*or*`None` | Returns the theSyntax, if any, at the unicode stringpathspecified | 4081 |
| find\_syntax\_by\_name(name) | [Syntax](api_reference#sublime.Syntax)*or*`None` | Returns the theSyntax, if any, with the unicode stringnamespecified | 4081 |
| find\_syntax\_by\_scope(scope) | [Syntax](api_reference#sublime.Syntax)*or*`None` | Returns the theSyntax, if any, with the unicode string basescopespecified | 4081 |
| find\_syntax\_for\_file(fname, ) | [Syntax](api_reference#sublime.Syntax)*or*`None` | Returns the theSyntaxthat will be used when opening a file with the namefname. Thefirst\_lineof file contents may also be provided if available. | 4081 |
| encode\_value(value, ) | str | Encode a JSON compatiblevalueinto a string representation. Ifprettyis set to`True`, the string will include newlines and indentation. |  |
| decode\_value(string) | [value](api_reference#type-value) | Decodes a JSON string into an object. If thestringis invalid, aValueErrorwill be thrown. |  |
| expand\_variables(value, variables) | [value](api_reference#type-value) | Expands any variables in the stringvalueusing the variables defined in the dictionaryvariables.valuemay also be alistordict, in which case the structure will be recursively expanded. Strings should use snippet syntax, for example:`expand_variables("Hello, ${name}",{"name":"Foo"})` |  |
| format\_command(cmd, ) | str | Create a "command string" from astrcmdname, and an optionaldictofargs. This is used when constructing a command-based[CompletionItem](api_reference#sublime.CompletionItem). | 4075 |
| command\_url(cmd, ) | str | Creates a[`subl:`\-protocol](minihtml#protocols)URL for executing a command in a[minihtml](minihtml)link. | 4075 |
| load\_settings(base\_name) | [Settings](api_reference#sublime.Settings) | Loads the named settings. The name should include a file name and extension, but not a path. The packages will be searched for files matching thebase\_name, and the results will be collated into the settings object. Subsequent calls toload\_settings()with thebase\_namewill return the same object, and not load the settings from disk again. |  |
| save\_settings(base\_name) | `None` | Flushes any in-memory changes to the named settings object to disk. |  |
| windows() | \[[Window](api_reference#sublime.Window)\] | Returns a list of all the open windows. |  |
| active\_window() | [Window](api_reference#sublime.Window) | Returns the most recently used window. |  |
| packages\_path() | str | Returns the path where all the user's loose packages are located. |  |
| installed\_packages\_path() | str | Returns the path where all the user's.sublime-packagefiles are located. |  |
| cache\_path() | str | Returns the path where Sublime Text stores cache files. |  |
| get\_clipboard() | str | *DEPRECATED - use`get_clipboard_async()`when possible.*Returns the contents of the clipboard.size\_limitprotects against unnecessarily large data, and defaults to 16,777,216 bytes. If the clipboard is larger thansize\_limit, an empty string will be returned. |  |
| get\_clipboard\_async(callback, ) | `None` | Callscallbackwith the contents of the clipboard.size\_limitprotects against unnecessarily large data, and defaults to 16,777,216 bytes. If the clipboard is larger thansize\_limit, an empty string will be passed. | 4075 |
| set\_clipboard(string) | `None` | Sets the contents of the clipboard. |  |
| score\_selector(scope, selector) | int | Matches theselectoragainst the givenscope, returning a score. A score of`0`means no match, above`0`means a match. Different selectors may be compared against the same scope: a higher score means the selector is a better match for the scope. |  |
| run\_command(string, ) | `None` | Runs the named[ApplicationCommand](api_reference#sublime_plugin.ApplicationCommand)with the (optional) givenargs. |  |
| get\_macro() | \[dict\] | Returns a list of the commands and args that compromise the currently recorded macro. Eachdictwill contain the keys"command"and"args". |  |
| log\_commands(flag) | `None` | Controls command logging. If enabled, all commands run from key bindings and the menu will be logged to the console. |  |
| log\_input(flag) | `None` | Controls input logging. If enabled, all key presses will be logged to the console. |  |
| log\_result\_regex(flag) | `None` | Controls result regex logging. This is useful for debugging regular expressions used in build systems. |  |
| log\_control\_tree(flag) | `None` | When enabled, clicking with**Ctrl***+***Alt**will log the control tree under the mouse to the console. | 4064 |
| log\_fps(flag) | `None` | When enabled, logs the number of frames per second being rendered for the user interface | 4075 |
| version() | str | Returns the version number |  |
| platform() | str | Returns the platform, which may be`"osx"`,`"linux"`or`"windows"` |  |
| arch() | str | Returns the CPU architecture, which may be`"x32"`or`"x64"` |  |
| channel() | str | Returns the release channel this build of Sublime Text is from:`"dev"`or`"stable"` |  |

## `sublime.Sheet`Class

Represents a content container, i.e. a tab, within a window. Sheets may contain a[View](api_reference#sublime.View), or an image preview.

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| id() | int | Returns a number that uniquely identifies this sheet. |  |
| window() | [Window](api_reference#sublime.Window)*or*`None` | Returns the window containing the sheet. May be`None`if the sheet has been closed. |  |
| view() | [View](api_reference#sublime.View)*or*`None` | Returns the view contained within the sheet. May be`None`if the sheet is an image preview, or the view has been closed. |  |
| file\_name() | str*or*`None` | The full name file the file associated with the buffer, or`None`if it doesn't exist on disk. | 4050 |
| close() | `None` | Closes the sheet | 4088 |

## `sublime.Buffer`Class4081

Represents a text buffer. Multiple[View](api_reference#sublime.View)objects may share the same buffer.

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| id() | int | Returns a number that uniquely identifies this buffer. | 4083 |
| file\_name() | str*or*`None` | The full name file the file associated with the buffer, or`None`if it doesn't exist on disk. | 4083 |
| views() | \[[View](api_reference#sublime.View)\] | Returns a list of all views that are associated with this buffer |  |
| primary\_view() | [View](api_reference#sublime.View) | The primary view associated with this buffer. |  |

## `sublime.View`Class

Represents a view into a text buffer. Note that multiple views may refer to the same buffer, but they have their own unique selection and geometry.

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| id() | int | Returns a number that uniquely identifies this view. |  |
| buffer\_id() | int | Returns a number that uniquely identifies the buffer underlying this view. |  |
| is\_primary() | bool | If the view is the primary view into a file. Will only be`False`if the user has opened multiple views into a file. |  |
| file\_name() | str*or*`None` | The full name file the file associated with the buffer, or`None`if it doesn't exist on disk. |  |
| close() | `None` | Closes the view |  |
| name() | str | The name assigned to the buffer, if any |  |
| set\_name(name) | `None` | Assigns a name to the buffer |  |
| is\_loading() | bool | Returns`True`if the buffer is still loading from disk, and not ready for use. |  |
| is\_dirty() | bool | Returns`True`if there are any unsaved modifications to the buffer. |  |
| is\_read\_only() | bool | Returns`True`if the buffer may not be modified. |  |
| set\_read\_only(value) | `None` | Sets the read only property on the buffer. |  |
| is\_scratch() | bool | Returns`True`if the buffer is a scratch buffer. Scratch buffers never report as being dirty. |  |
| set\_scratch(value) | `None` | Sets the scratch property on the buffer. |  |
| settings() | [Settings](api_reference#sublime.Settings) | Returns a reference to the view's settings object. Any changes to this settings object will be private to this view. |  |
| meta\_info(key, pt) | [](api_reference#sublime.Settings)[value](api_reference#type-value) | Returns data from.tmPreferencesfiles. Looks up the preference namedkeyfor the scope of the token at the[point](api_reference#type-point)pt. Preference names may be any string defined in a.tmPreferencesfile. Some examples include:`TM_COMMENT_START`,`TM_COMMENT_END`,`showInSymbolList`and`showInIndexedSymbolList`. |  |
| element() | str*or*`None` | Returns`None`for normal views, for views that comprise part of the UI, astris returned from the following list:
*   `"console:input"`: The console input
*   `"goto_anything:input"`: The input for the Goto Anything
*   `"command_palette:input"`: The input for the Command Palette
*   `"find:input"`: The input for the Find panel
*   `"incremental_find:input"`: The input for the Incremental Find panel
*   `"replace:input:find"`: The Find input for the Replace panel
*   `"replace:input:replace"`: The Replace input for the Replace panel
*   `"find_in_files:input:find"`: The Find input for the Find in Files panel
*   `"find_in_files:input:location"`: The Where input for the Find in Files panel
*   `"find_in_files:input:replace"`: The Replace input for the Find in Files panel
*   `"find_in_files:output"`: The output panel for Find in Files (buffer or output panel)
*   `"input:input"`: The input for the Input panel
*   `"exec:output"`: The output for the exec command
*   `"output:output"`: A general output panel

The console output, indexer status output and license input controls are not accessible via the API. | 4050 |
| window() | [Window](api_reference#sublime.Window) | Returns a reference to the window containing the view. |  |
| sheet() | [Sheet](api_reference#sublime.Sheet)*or*`None` | Returns the Sheet for this View, if displayed in a sheet | 4083 |
| sheet\_id() | int | Returns the ID of the Sheet for this View, or`0`if not part of a sheet | 4083 |
| run\_command(string, ) | `None` | Runs the named[TextCommand](api_reference#sublime_plugin.TextCommand)with the (optional) givenargs. |  |
| size() | int | Returns the number of character in the file. |  |
| substr(region) | str | Returns the contents of theregionas a string. |  |
| substr(point) | str | Returns the character to the right of thepoint. |  |
| insert(edit, point, string) | int | Inserts the givenstringin the buffer at the specifiedpoint. Returns the number of characters inserted: this may be different if tabs are being translated into spaces in the current buffer. |  |
| erase(edit, region) | `None` | Erases the contents of theregionfrom the buffer. |  |
| replace(edit, region, string) | `None` | Replaces the contents of theregionwith the givenstring. |  |
| sel() | [Selection](api_reference#sublime.Selection) | Returns a reference to the selection. |  |
| line(point) | [Region](api_reference#sublime.Region) | Returns the line that contains thepoint. |  |
| line(region) | [Region](api_reference#sublime.Region) | Returns a modified copy ofregionsuch that it starts at the beginning of a line, and ends at the end of a line. Note that it may span several lines. |  |
| full\_line(point) | [Region](api_reference#sublime.Region) | Asline(), but the region includes the trailing newline character, if any. |  |
| full\_line(region) | [Region](api_reference#sublime.Region) | Asline(), but the region includes the trailing newline character, if any. |  |
| lines(region) | \[[Region](api_reference#sublime.Region)\] | Returns a list of lines (in sorted order) intersecting theregion. |  |
| split\_by\_newlines(region) | \[[Region](api_reference#sublime.Region)\] | Splits theregionup such that each region returned exists on exactly one line. |  |
| word(point) | [Region](api_reference#sublime.Region) | Returns the word that contains thepoint. |  |
| word(region) | [Region](api_reference#sublime.Region) | Returns a modified copy ofregionsuch that it starts at the beginning of a word, and ends at the end of a word. Note that it may span several words. |  |
| classify(point) | int | 

Classifiespoint, returning a bitwise OR of zero or more of these flags:

*   `sublime.CLASS_WORD_START`
*   `sublime.CLASS_WORD_END`
*   `sublime.CLASS_PUNCTUATION_START`
*   `sublime.CLASS_PUNCTUATION_END`
*   `sublime.CLASS_SUB_WORD_START`
*   `sublime.CLASS_SUB_WORD_END`
*   `sublime.CLASS_LINE_START`
*   `sublime.CLASS_LINE_END`
*   `sublime.CLASS_EMPTY_LINE`

 |  |
| find\_by\_class(point, forward, classes, ) | [Region](api_reference#sublime.Region) | Finds the next location afterpointthat matches the givenclasses. Ifforwardis`False`, searches backwards instead of forwards.classesis a bitwise OR of the`sublime.CLASS_XXX`flags.separatorsmay be passed in, to define what characters should be considered to separate words. |  |
| expand\_by\_class(point, classes, ) | [Region](api_reference#sublime.Region) | Expandspointto the left and right, until each side lands on a location that matchesclasses.classesis a bitwise OR of the`sublime.CLASS_XXX`flags. separators may be passed in, to define what characters should be considered to separate words. |  |
| expand\_by\_class(region, classes, ) | [Region](api_reference#sublime.Region) | Expandsregionto the left and right, until each side lands on a location that matchesclasses.classesis a bitwise OR of the`sublime.CLASS_XXX`flags.separatorsmay be passed in, to define what characters should be considered to separate words. |  |
| find(pattern, start\_point, ) | [Region](api_reference#sublime.Region) | Returns the first region matching the regexpattern, starting fromstart\_point, or`Region(-1,-1)`if it can't be found. The optionalflagsparameter may be`sublime.LITERAL`,`sublime.IGNORECASE`, or the two ORed together. |  |
| find\_all(pattern, , , ) | \[[Region](api_reference#sublime.Region)\] | Returns all (non-overlapping) regions matching the regexpattern. The optionalflagsparameter may be`sublime.LITERAL`,`sublime.IGNORECASE`, or the two ORed together. If aformatstring is given, then all matches will be formatted with the formatted string and placed into the extractions list. |  |
| rowcol(point) | (int,int) | Calculates the 0-based line and column numbers of thepoint.*Column numbers are returned as number of Unicode characters.* |  |
| rowcol\_utf8(point) | (int,int) | Calculates the 0-based line and column numbers of thepoint.*Column numbers are returned as UTF-8 code units, i.e. bytes.* | 4069 |
| rowcol\_utf16(point) | (int,int) | Calculates the 0-based line and column numbers of thepoint.*Column numbers are returned as UTF-16 code units, i.e. byte pairs.* | 4069 |
| text\_point(row, col, ) | int | 

Calculates the character offset of the given, 0-based,rowandcol.*colis interpreted as the number of Unicode characters to advance past the beginning of the row.*

clamp\_column4075

Abool, ifcolshould be restricted to valid values for the givenrow

 |  |
| text\_point\_utf8(row, col, ) | int | 

Calculates the character offset of the given, 0-based,rowandcol.*colis interpreted as the number of UTF-8 code units, i.e. bytes, to advance past the beginning of the row.*

clamp\_column4075

Abool, ifcolshould be restricted to valid values for the givenrow

 | 4069 |
| text\_point\_utf16(row, col, ) | int | 

Calculates the character offset of the given, 0-based,rowandcol.*colis interpreted as the number of UTF-16 code units, i.e. byte pairs, to advance past the beginning of the row.*

clamp\_column4075

Abool, ifcolshould be restricted to valid values for the givenrow

 | 4069 |
| assign\_syntax(syntax) | `None` | Changes the syntax used by the view.syntaxmay be a[Syntax](api_reference#sublime.Syntax)object, packages path to a syntax file, or`scope:`specifier string. |  |
| set\_syntax\_file(syntax\_file) | `None` | *DEPRECATED - use`assign_syntax()`when possible.*Changes the syntax used by the view.syntax\_fileshould be a name along the lines of`"Packages/Python/Python.tmLanguage"`. To retrieve the current syntax, use`view.settings().get('syntax')`. |  |
| syntax() | [Syntax](api_reference#sublime.Syntax)*or*`None` | Returns the syntax for this view | 4081 |
| extract\_scope(point) | [Region](api_reference#sublime.Region) | Returns the extent of the syntax scope name assigned to the character at the givenpoint. |  |
| scope\_name(point) | str | Returns the syntax scope name assigned to the character at the givenpoint. |  |
| match\_selector(point, selector) | bool | Checks theselectoragainst the scope at the givenpoint, returning a bool if they match. |  |
| score\_selector(point, selector) | int | Matches theselectoragainst the scope at the givenpoint, returning a score. A score of`0`means no match, above`0`means a match. Different selectors may be compared against the same scope: a higher score means the selector is a better match for the scope. |  |
| find\_by\_selector(selector) | \[[Region](api_reference#sublime.Region)\] | Finds all regions in the file matching the givenselector, returning them as a list. |  |
| show(location, , , ) | `None` | 

Scroll the view to show the given location.

location

A[point](api_reference#type-point),[Region](api_reference#sublime.Region)or[Selection](api_reference#sublime.Selection)to scroll the view to.

show\_surrounds

Abool, scroll the view far enough that surrounding conent is visible also

keep\_to\_left4075

Abool, if the view should be kept to the left, if horizontal scrolling is possible

animate4075

Abool, if the scroll should be animated

 |  |
| show\_at\_center(location) | `None` | Scroll the view to center on thelocation, which may be a[point](api_reference#type-point)or[Region](api_reference#sublime.Region). |  |
| visible\_region() | [Region](api_reference#sublime.Region) | Returns the currently visible area of the view. |  |
| viewport\_position() | [vector](api_reference#type-vector) | Returns the offset of the viewport in layout coordinates. |  |
| set\_viewport\_position(vector, ) | `None` | Scrolls the viewport to the given layout position. |  |
| viewport\_extent() | [vector](api_reference#type-vector) | Returns the width and height of the viewport. |  |
| layout\_extent() | [vector](api_reference#type-vector) | Returns the width and height of the layout. |  |
| text\_to\_layout(point) | [vector](api_reference#type-vector) | Converts a text point to a layout position |  |
| text\_to\_window(point) | [vector](api_reference#type-vector) | Converts a text point to a window position |  |
| layout\_to\_text(vector) | [point](api_reference#type-point) | Converts a layout position to a text point |  |
| layout\_to\_window(vector) | [vector](api_reference#type-point) | Converts a layout position to a window position |  |
| window\_to\_layout(vector) | [vector](api_reference#type-vector) | Converts a window position to a layout position |  |
| window\_to\_text(vector) | [point](api_reference#type-point) | Converts a window position to a text point |  |
| line\_height() | [dip](api_reference#type-dip) | Returns the light height used in the layout |  |
| em\_width() | [dip](api_reference#type-dip) | Returns the typical character width used in the layout |  |
| add\_regions(key, \[regions\], , , , , , , ) | `None` | 

Adds visual indicators to regions of text in the view. Indicators include icons in the gutter, underlines under the text, borders around the text and annotations. Annotations are drawn aligned to the right-hand edge of the view and may contain HTML markup.

key

A unicode string identifying the collection ofregions. If a set ofregionsalready exists with the samekey, they will be overwritten.

scope

An optional unicode string used to source a color to draw theregionsin. Thescopeis matched against the color scheme. Examples include:`"invalid"`and`"string"`. See[Scope Naming](scope_naming#alphabetical_reference)for a list of common scopes. If thescopeis empty, theregionswon't be drawn.

Also supports the following pseudo-scopes, to allow picking the closest color from the user‘s color scheme:3148

*   `"region.redish"`
*   `"region.orangish"`
*   `"region.yellowish"`
*   `"region.greenish"`
*   `"region.cyanish"`
*   `"region.bluish"`
*   `"region.purplish"`
*   `"region.pinkish"`

icon

An optional unicode string specifying an icon to draw in the gutter next to each region. Theiconwill be tinted using the color associated with thescope. Standard icon names are`"dot"`,`"circle"`and`"bookmark"`. Theiconmay also be a full package-relative path, such as`"Packages/Theme - Default/dot.png"`.

flags

An optional bitwise combination of zero or more of:

*   `sublime.DRAW_EMPTY`: Draw empty regions with a vertical bar. By default, they aren't drawn at all.
*   `sublime.HIDE_ON_MINIMAP`: Don't show the regions on the minimap.
*   `sublime.DRAW_EMPTY_AS_OVERWRITE`: Draw empty regions with a horizontal bar instead of a vertical one.
*   `sublime.DRAW_NO_FILL`: Disable filling the regions, leaving only the outline.
*   `sublime.DRAW_NO_OUTLINE`: Disable drawing the outline of the regions.
*   `sublime.DRAW_SOLID_UNDERLINE`: Draw a solid underline below the regions.
*   `sublime.DRAW_STIPPLED_UNDERLINE`: Draw a stippled underline below the regions.
*   `sublime.DRAW_SQUIGGLY_UNDERLINE`: Draw a squiggly underline below the regions.
*   `sublime.PERSISTENT`: Save the regions in the session.
*   `sublime.HIDDEN`: Don't draw the regions.

The underline styles are exclusive, either zero or one of them should be given. If using an underline,`sublime.DRAW_NO_FILL`and`sublime.DRAW_NO_OUTLINE`should generally be passed in.

annotations4050

An optional collection of unicode strings containing HTML documents to display along the right-hand edge of the view. There should be the same number ofannotationsasregions. See[minihtml Reference](minihtml)for supported HTML.

annotation\_color4050

A optional unicode string of the CSS color to use when drawing the left border of the annotation. See[minihtml Reference: Colors](minihtml#colors)for supported color formats.

on\_navigate4050

A callback that will be passed thehrefwhen a link in an annotation is clicked.

on\_close4050

A callback that will be called when the annotations are closed.

 |  |
| get\_regions(key) | \[[Region](api_reference#sublime.Region)\] | Return the regions associated with the givenkey, if any |  |
| erase\_regions(key) | `None` | Removed the named regions |  |
| set\_status(key, value) | `None` | Adds the statuskeyto the view. Thevaluewill be displayed in the status bar, in a comma separated list of all status values, ordered by key. Setting thevalueto the empty string will clear the status. |  |
| get\_status(key) | str | Returns the previously assigned value associated with thekey, if any. |  |
| erase\_status(key) | `None` | Clears the named status. |  |
| command\_history(index, ) | (str,dict,int) | 

Returns the command name, command arguments, and repeat count for the given history entry, as stored in the undo / redo stack.

Index`0`corresponds to the most recent command,`-1`the command before that, and so on. Positive values for index indicate to look in the redo stack for commands. If the undo / redo history doesn't extend far enough, then`(None,None,0)`will be returned.

Settingmodifying\_onlyto`True`(the default is`False`) will only return entries that modified the buffer.

 |  |
| change\_count() | int | Returns the current change count. Each time the buffer is modified, the change count is incremented. The change count can be used to determine if the buffer has changed since the last it was inspected. |  |
| change\_id() | (int,int,int) | Returns a 3-element tuple that can be passed totransform\_region\_from()to obtain a region equivalent to a region of theViewin the past. This is primarily useful for plugins providing text modification that must operate in an asynchronous fashion and must be able to handle the view contents changing between the request and response. | 4069 |
| transform\_region\_from(region, change\_id) | [Region](api_reference#sublime.Region) | Transforms a region from a previous point in time to an equivalent region in the current state of theView. Thechange\_idmust have been obtained fromchange\_id()at the point in time the region is from. | 4069 |
| fold(\[regions\]) | bool | Folds the givenregions, returning`False`if they were already folded |  |
| fold(region) | bool | Folds the givenregion, returning`False`if it was already folded |  |
| unfold(region) | \[[Region](api_reference#sublime.Region)\] | Unfolds all text in theregion, returning the unfolded regions |  |
| unfold(\[regions\]) | \[[Region](api_reference#sublime.Region)\] | Unfolds all text in theregions, returning the unfolded regions |  |
| encoding() | str | Returns the encoding currently associated with the file |  |
| set\_encoding(encoding) | `None` | Applies a new encoding to the file. This encoding will be used the next time the file is saved. |  |
| line\_endings() | str | Returns the line endings used by the current file. |  |
| set\_line\_endings(line\_endings) | `None` | Sets the line endings that will be applied when next saving. |  |
| overwrite\_status() | bool | Returns the overwrite status, which the user normally toggles via the insert key. |  |
| set\_overwrite\_status(enabled) | `None` | Sets the overwrite status. |  |
| symbols() | \[([Region](api_reference#sublime.Region), str)\] | *DEPRECATED - use`symbol_regions()`when possible.*Extract all the symbols defined in the buffer. |  |
| symbol\_regions() | \[[SymbolRegion](api_reference#sublime.SymbolRegion)\] | Returns info about symbols that are part of the local symbol list | 4085 |
| indexed\_symbol\_regions() | \[[SymbolRegion](api_reference#sublime.SymbolRegion)\] | Returns info about symbols that are indexed | 4085 |
| show\_popup\_menu(items, on\_done, ) | `None` | 

Shows a pop up menu at the caret, to select an item in a list.on\_donewill be called once, with the index of the selected item. If the pop up menu was cancelled,on\_donewill be called with an argument of`-1`.

itemsis a list of strings.

flagsit currently unused.

 |  |
| show\_popup(content, , , , , , ) | `None` | 

Shows a popup displaying HTML content.

flagsis a bitwise combination of the following:

*   `sublime.COOPERATE_WITH_AUTO_COMPLETE`: causes the popup to display next to the auto complete menu
*   `sublime.HIDE_ON_MOUSE_MOVE`: causes the popup to hide when the mouse is moved, clicked or scrolled
*   `sublime.HIDE_ON_MOUSE_MOVE_AWAY`: causes the popup to hide when the mouse is moved (unless towards the popup), or when clicked or scrolled
*   `sublime.KEEP_ON_SELECTION_MODIFIED`: prevent the popup from hiding when the selection is modified4057
*   `sublime.HIDE_ON_CHARACTER_EVENT`: hide the popup when a character is typed4075

The defaultlocationof`-1`will display the popup at the cursor, otherwise a text point should be passed.

max\_widthandmax\_heightset the maximum dimensions for the popup, after which scroll bars will be displayed.

on\_navigateis a callback that should accept a string contents of the`href`attribute on the link the user clicked.

on\_hideis called when the popup is hidden.

 |  |
| update\_popup(content) | `None` | Updates the contents of the currently visible popup. |  |
| is\_popup\_visible() | bool | Returns if the popup is currently shown. |  |
| preserve\_auto\_complete\_on\_focus\_lost() | `None` | Sets the auto complete popup state to be preserved the next time the View loses focus. When the View regains focus, the auto complete window will be re-shown, with the previously selected entry pre-selected. | 4073 |
| hide\_popup() | `None` | Hides the popup. |  |
| is\_auto\_complete\_visible() | bool | Returns if the auto complete menu is currently visible. |  |
| style() | dict | Returns adictof the global style settings for the view.*All colors are normalized to the six character hex form with a leading hash, e.g.`#ff0000`.* | 3150 |
| style\_for\_scope(scope\_name) | dict | 

Accepts a string scope name and returns adictof style information, includes the keys:

*   "foreground"
*   "background"*(only if set)*
*   "bold"
*   "italic"
*   "glow"4063
*   "underline"4075
*   "stippled\_underline"4075
*   "squiggly\_underline"4075
*   "source\_line"
*   "source\_column"
*   "source\_file"

*The foreground and background colors are normalized to the six character hex form with a leading hash, e.g.#ff0000.*

 | 3149 |
| set\_reference\_document(reference) | `None` | Uses the stringreferenceto calculate the initial diff for the[incremental diff](incremental_diff) | 3186 |
| reset\_reference\_document() | `None` | Clears the state of the[incremental diff](incremental_diff)for the view | 3190 |
| export\_to\_html(, , , , ) | str | 

Generates an HTML string of the current view contents

regions\- restrict the generated HTML to aRegionorlistof regions.

minihtml\- a boolean that causes the exported HTML to be compatiable with[minihtml](minihtml).

enclosing\_tags\- a boolean that adds a`<div>`tag with base styles around the markup.

font\_size\- a boolean that controls adding`font-size`to the markup generated whenenclosing\_tagsis`True`.

font\_family\- a boolean that controls adding`font-family`to the markup generated whenenclosing\_tagsis`True`.

 | 4092 |

## `sublime.Selection`Class

Maintains a set of Regions, ensuring that none overlap. The regions are kept in sorted order.

| Methods | Return Value | Description |
| --- | --- | --- |
| clear() | `None` | Removes all regions. |
| add(region) | `None` | Adds the givenregion. It will be merged with any intersecting regions already contained within the set. |
| add\_all(regions) | `None` | Adds all regions in the givenlistortuple. |
| subtract(region) | `None` | Subtracts theregionfrom all regions in the set. |
| contains(region) | bool | Returns`True`iff the givenregionis a subset. |

## `sublime.Region`Class

Represents an area of the buffer. Empty regions, where`a==b`are valid.

| Constructors | Description |
| --- | --- |
| Region(a, b) | Creates a Region with initial values a and b. |

| Properties | Type | Description |
| --- | --- | --- |
| a | int | The first end of the region. |
| b | int | The second end of the region. May be less that a, in which case the region is a reversed one. |
| xpos | int | The target horizontal position of the region, or`-1`if undefined. Effects behavior when pressing the up or down keys. |

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| begin() | int | Returns the minimum of a and b. |  |
| end() | int | Returns the maximum of a and b. |  |
| size() | int | Returns the number of characters spanned by the region. Always >= 0. |  |
| empty() | bool | Returns`True`iffbegin()\==end(). |  |
| cover(region) | [Region](api_reference#sublime.Region) | Returns a Region spanning both this and the given regions. |  |
| intersection(region) | [Region](api_reference#sublime.Region) | Returns the set intersection of the two regions. |  |
| intersects(region) | bool | Returns`True`iff self ==regionor both include one or more positions in common. |  |
| contains(region) | bool | Returns`True`iff the givenregionis a subset. |  |
| contains(point) | bool | Returns`True`iffbegin()<=point<=end(). |  |
| to\_tuple() | tuple | 
Returns a 2-elementtupleof:

1.  .a, anint
2.  .b, anint

 | 4075 |

## `sublime.Phantom`Class

Represents an HTML-based decoration to display non-editable content interspersed in a View. Used with PhantomSet to actually add the phantoms to the View. Once a Phantom has been constructed and added to the View, changes to the attributes will have no effect.

| Constructors | Description |
| --- | --- |
| Phantom(region, content, layout, ) | 
Creates a phantom attached to aregion. Thecontentis HTML to be processed by[minihtml](minihtml).

layoutmust be one of:

*   `sublime.LAYOUT_INLINE`: Display the phantom in between theregionand the point following.
*   `sublime.LAYOUT_BELOW`: Display the phantom in space below the current line, left-aligned with theregion.
*   `sublime.LAYOUT_BLOCK`: Display the phantom in space below the current line, left-aligned with the beginning of the line.

on\_navigateis an optional callback that should accept a single string parameter, that is the`href`attribute of the link clicked.

 |

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| to\_tuple() | tuple | 
Returns a 4-elementtupleof:

1.  .region, as a 2-elementtuple
2.  .content, astr
3.  .layout, anint
4.  .on\_navigate, acallbackor`None`

 | 4075 |

## `sublime.PhantomSet`Class

A collection that manages Phantoms and the process of adding them, updating them and removing them from the View.

| Constructors | Description |
| --- | --- |
| PhantomSet(view, ) | Creates a PhantomSet attached to aview.keyis a string to group Phantoms together. |

| Methods | Return Value | Description |
| --- | --- | --- |
| update(phantoms) | `None` | 
phantomsshould be a list of phantoms.

The.regionattribute of each existing phantom in the set will be updated. New phantoms will be added to the view and phantoms not inphantomslist will be deleted.

 |

## `sublime.Edit`Class

Edit objects have no functions, they exist to group buffer modifications.

Edit objects are passed to[TextCommand](api_reference#sublime_plugin.TextCommand)s, and can not be created by the user. Using an invalid Edit object, or an Edit object from a different View, will cause the functions that require them to fail.

| Methods | Return Value | Description |
| --- | --- | --- |
| (no methods) |  |  |

## `sublime.TextChangeListener`Class4081

A class that provides event handling about text changes made to a specific[`Buffer`](api_reference#sublime_plugin.Buffer).*Is seperate from[`ViewEventListener`](api_reference#sublime_plugin.ViewEventListener)since multiple views can share a single buffer.*

| Properties | Type | Description |  |
| --- | --- | --- | --- |
| buffer | [](api_reference#sublime.HistoricPosition)[`Buffer`](api_reference#sublime_plugin.Buffer) | The buffer this`TextChangeListener`is listening to |  |

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| on\_text\_changed(\[changes\]) | `None` | 
Called once after changes has been made to a buffer, with detailed information about what has changed.

changesis alistof[TextChange](api_reference#sublime.TextChange)objects.

 | 4081 |
| on\_text\_changed\_async(\[changes\]) | `None` | 

Called once after changes has been made to a buffer, with detailed information about what has changed. Runs in a separate thread, and does not block the application.

changesis alistof[TextChange](api_reference#sublime.TextChange)objects.

 | 4081 |
| on\_revert() | `None` | Called when the buffer is reverted. | 4081 |
| on\_revert\_async() | `None` | Called when the buffer is reverted. Runs in a separate thread, and does not block the application. | 4081 |
| on\_reload() | `None` | Called when the buffer is reloaded. | 4081 |
| on\_reload\_async() | `None` | Called when the buffer is reloaded. Runs in a separate thread, and does not block the application. | 4081 |

## `sublime.Syntax`Class4081

Contains information about a syntax.

| Properties | Type | Description |
| --- | --- | --- |
| path | str | The packages path to the syntax file |
| name | str | The name of the syntax |
| hidden | bool | If the syntax is hidden from the user |
| scope | str | The base scope name of the syntax |

## `sublime.TextChange`Class4050

Represents a change that occured to the text of a[View](api_reference#sublime.View). This is primarily useful for replaying changes to a document.

| Properties | Type | Description |  |
| --- | --- | --- | --- |
| a | [HistoricPosition](api_reference#sublime.HistoricPosition) | The beginning point of the region that was modified |  |
| b | [HistoricPosition](api_reference#sublime.HistoricPosition) | The ending point of the region that was modified |  |
| len\_utf16 | int | The length of the old contents, in UTF-16 code units | 4075 |
| len\_utf8 | int | The length of the old contents, in UTF-8 code units (i.e. bytes) | 4075 |
| str | str | A unicode string of the*new*contents of the region specified by.aand.b. |  |

## `sublime.HistoricPosition`Class4050

Provides a snapshot of the row and column info for a point, before changes were made to a[View](api_reference#sublime.View). This is primarily useful for replaying changes to a document.

| Properties | Type | Description |
| --- | --- | --- |
| pt | [point](api_reference#type-point) | The unicode character offset from the beginning of the[View](api_reference#sublime.View) |
| row | int | The row the.ptwas in when theHistoricPositionwas recorded |
| col | int | The column the.ptwas in when theHistoricPositionwas recorded, in Unicode characters |
| col\_utf16 | int | The value of.col, but in UTF-16 code units | 4075 |
| col\_utf8 | int | The value of.col, but in UTF-8 code units (i.e. bytes) | 4075 |

## `completion`Value

Represents an available auto-completion item.completionvalues may be of serveral formats. The term*trigger*refers to the text matched against the user input,*replacement*is what is inserted into the view if the item is selected. An*annotation*is a unicode string hint displayed to the right-hand side of the trigger.

*   str
    
    A unicode string that is both the trigger and the replacement
    
    ~~~
    return [
        "method1()",
        "method2()"
    ]
    ~~~
    
*   2-elementlist/tuple
    
    A pair of unicode strings, the trigger and the replacement.
    
    ~~~
    return [
        ["me1", "method1()"],
        ["me2", "method2()"]
    ]
    ~~~
    
    If a`\t`is present in the trigger, all subsequent text is treated as an annotation.
    
    ~~~
    return [
        ["me1\tmethod", "method1()"],
        ["me2\tmethod", "method2()"]
    ]
    ~~~
    
    The replacement text may contain dollar-numeric fields such as a snippet does, e.g.`$0`,`$1`.
    
    ~~~
    return [
        ["fn", "def ${1:name}($2) { $0 }"],
        ["for", "for ($1; $2; $3) { $0 }"]
    ]
    ~~~
    
*   [CompletionItem](api_reference#sublime.CompletionItem)object4050
    
    An object containing trigger, replacement, annotation, and kind metadata
    
    ~~~
    return [
        sublime.CompletionItem(
            "fn",
            annotation="def",
            completion="def ${1:name}($2) { $0 }",
            completion_format=sublime.COMPLETION_FORMAT_SNIPPET,
            kind=sublime.KIND_SNIPPET
        ),
        sublime.CompletionItem(
            "for",
            completion="for ($1; $2; $3) { $0 }",
            completion_format=sublime.COMPLETION_FORMAT_SNIPPET,
            kind=sublime.KIND_SNIPPET
        ),
    ]
    ~~~
    

## `sublime.CompletionItem`Class4050

Represents an available auto-completion item.

| Constructors | Description |
| --- | --- |
| CompletionItem(trigger, , , , ) | 
trigger

A unicode string of the text to match against the user's input.

annotation

An optional unicode string of a hint to draw to the right-hand side of the trigger.

completion

An optional unicode string of the text to insert if the completion is specified. If empty, thetriggerwill be inserted.

completion\_format

The format of thecompletion:

*   `sublime.COMPLETION_FORMAT_TEXT`: Plain text –*the default*
*   `sublime.COMPLETION_FORMAT_SNIPPET`: A snippet, with`$`variables. See also the[CompletionItem.snippet\_completion()](api_reference#sublime.CompletionItem.snippet_completion)class method.
*   `sublime.COMPLETION_FORMAT_COMMAND`: A command string, encoded via[format\_command()](api_reference#sublime.format_command). See also the[CompletionItem.command\_completion()](api_reference#sublime.CompletionItem.command_completion)class method.

kind

An optional[kindtuple](api_reference#type-kind_tuple)that controls the presentation in the auto-complete window –*defaults to`sublime.KIND_AMBIGUOUS`*.

details4073

An optional HTML description of the completion, shown in the detail pane at the bottom of the auto complete window. Only supports limited inline HTML, including the tags:

 |
| CompletionItem.snippet\_completion(trigger, snippet, , ) | 

trigger

A unicode string of the text to match against the user's input.

snippet

The snippet text to insert if the item is selected.

annotation

An optional unicode string of a hint to draw to the right-hand side of the trigger.

kind

An optional[kindtuple](api_reference#type-kind_tuple)that controls the presentation in the auto-complete window –*defaults to`sublime.KIND_SNIPPET`*.

details4073

An optional HTML description of the completion, shown in the detail pane at the bottom of the auto complete window. Only supports[limited inline HTML](minihtml.#inline_formatting).

 |
| CompletionItem.command\_completion(trigger, command, , , ) | 

trigger

A unicode string of the text to match against the user's input.

command

A unicode string of the command to execute

args

An optionaldictof args to pass to thecommand

annotation

An optional unicode string of a hint to draw to the right-hand side of the trigger.

kind

An optional[kindtuple](api_reference#type-kind_tuple)that controls the presentation in the auto-complete window –*defaults to`sublime.KIND_AMBIGUOUS`*.

details4073

An optional HTML description of the completion, shown in the detail pane at the bottom of the auto complete window. Only supports limited inline HTML, including the tags:

*   `<ahref="">`–[protocols](minihtml#protocols)
*   `<b>`
*   `<strong>`
*   `<i>`
*   `<em>`
*   `<u>`
*   `<tt>`
*   `<code>`

 |

## `event`Dict

Contains information about a user’s interaction with a menu, command palette selection, quick panel selection or HTML document. The follow methods are used to signal that an eventdictis desired:

*   Commands may opt-in to receive an arg named`event`by implementing the method`want_event(self)`and returning`True`.
*   A call to[`show_quick_panel()`](api_reference#sublime.Window.show_quick_panel)may opt-in to receive a second arg to theon\_donecallback by specifying the flag`sublime.WANT_EVENT`.4096
*   [`ListInputHandler`](api_reference#sublime_plugin.ListInputHandler)classes may opt-in to receive a second arg to the`validate()`and`confirm()`methods by by implementing the method`want_event(self)`and returning`True`.4096

Thedictmay contain zero or more of the following keys, based on the user interaction:

x

A float of the X mouse position when a user clicks on a menu, or in a minihtml document.

y

A float of the Y mouse position when a user clicks on a menu, or in a minihtml document.

modifier\_keys4096

Adictwith zero or more of the following keys:

*   `"primary"`\- indicating**Ctrl**(Windows/Linux) or**Cmd**(Mac) was pressed
*   `"ctrl"`\- indicating**Ctrl**was pressed
*   `"alt"`\- indicating**Alt**was pressed
*   `"altgr"`\- indicating**AltGr**was pressed (Linux only)
*   `"shift"`\- indicating**Shift**was pressed
*   `"super"`\- indicating**Win**(Windows/Linux) or**Cmd**(Mac) was pressed

Present when the user selects an item from a quick panel, selects an item from a[`ListInputHandler`](api_reference#sublime_plugin.ListInputHandler), or clicks a link in a minihtml document.

## `kind`Tuple4050

Metadata about the kind of a symbol,[CompletionItem](api_reference#sublime.CompletionItem),[QuickPanelItem](api_reference#sublime.QuickPanelItem)or[ListInputItem](api_reference#sublime.ListInputItem). Controls the color and letter shown in the "icon" presented to the left of the item.

Options include pre-constructed combinations, or completely custom values.

Pre-contructed options include:

*   `sublime.KIND_AMBIGUOUS`  
    When there source of the item is unknown –*the default*.  
    *Letter: none, theme class:*`kind_ambiguous`
*   `sublime.KIND_KEYWORD`  
    When the item represents a keyword.  
    *Letter:*`k`,*theme class:*`kind_keyword`
*   `sublime.KIND_TYPE`  
    When the item represents a data type, class, struct, interface, enum, trait, etc.  
    *Letter:*`t`,*theme class:*`kind_type`
*   `sublime.KIND_FUNCTION`  
    When the item represents a function, method, constructor or subroutine.  
    *Letter:*`f`,*theme class:*`kind_function`
*   `sublime.KIND_NAMESPACE`  
    When the item represents a namespace or module.  
    *Letter:*`a`,*theme class:*`kind_namespace`
*   `sublime.KIND_NAVIGATION`  
    When the item represents a definition, label or section.  
    *Letter:*`n`,*theme class:*`kind_navigation`
*   `sublime.KIND_MARKUP`  
    When the item represents a markup component, including HTML tags and CSS selectors.  
    *Letter:*`m`,*theme class:*`kind_markup`
*   `sublime.KIND_VARIABLE`  
    When the item represents a variable, member, attribute, constant or parameter.  
    *Letter:*`v`,*theme class:*`kind_variable`
*   `sublime.KIND_SNIPPET`  
    When the item contains a snippet.  
    *Letter:*`s`,*theme class:*`kind_snippet`

Custom kind information may also be passed, via a 3-element tuple, in the following format:

1.  A kind id, which controls the theme class used to contain the letter:
    *   `sublime.KIND_ID_AMBIGUOUS`  
        When there source of the item is unknown
    *   `sublime.KIND_ID_KEYWORD`  
        When the item represents a keyword
    *   `sublime.KIND_ID_TYPE`  
        When the item represents a data type
    *   `sublime.KIND_ID_FUNCTION`  
        When the item represents a function, method, constructor or subroutine
    *   `sublime.KIND_ID_NAMESPACE`  
        When the item represents a namespace or module
    *   `sublime.KIND_ID_NAVIGATION`  
        When the item represents a definition, label or section
    *   `sublime.KIND_ID_MARKUP`  
        When the item represents a markup component, including HTML tags and CSS selectors
    *   `sublime.KIND_ID_VARIABLE`  
        When the item represents a variable, member, attribute, constant or parameter
    *   `sublime.KIND_ID_SNIPPET`  
        When the item contains a snippet
2.  A unicode string containing a single unicode character. This is shown as an "icon" to the left of the`trigger`.
3.  An optional unicode string to describe the kind, shown in the detail pane at the bottom of the auto-complete window, or via tooltip when hovering over the "icon".

For[QuickPanelItem](api_reference#sublime.QuickPanelItem)and[ListInputItem](api_reference#sublime.ListInputItem)objects, a custom tuple may also use one of the following kind ids, which are pulled from the user's global color scheme. These are intended to help build semantic user interactions, such as using green for "Yes" and red for "No".4095

*   `sublime.KIND_ID_COLOR_REDISH`  
    The closest color in the color scheme to red
*   `sublime.KIND_ID_COLOR_ORANGISH`  
    The closest color in the color scheme to orange
*   `sublime.KIND_ID_COLOR_YELLOWISH`  
    The closest color in the color scheme to yellow
*   `sublime.KIND_ID_COLOR_GREENISH`  
    The closest color in the color scheme to green
*   `sublime.KIND_ID_COLOR_CYANISH`  
    The closest color in the color scheme to cyan
*   `sublime.KIND_ID_COLOR_BLUISH`  
    The closest color in the color scheme to blue
*   `sublime.KIND_ID_COLOR_PURPLISH`  
    The closest color in the color scheme to purple
*   `sublime.KIND_ID_COLOR_PINKISH`  
    The closest color in the color scheme to pink
*   `sublime.KIND_ID_COLOR_DARK`  
    A dark grey or black color, as defined by the user's theme
*   `sublime.KIND_ID_COLOR_LIGHT`  
    A light grey or white color, as defined by the user's theme

## `sublime.CompletionList`Class4050

Represents a list of completions, some of which may be in the process of being asynchronously fetched.

| Constructors | Description |
| --- | --- |
| CompletionList(, ) | 
completions

An optional list of[completion values](api_reference#type-completion). If`None`is passed, the methodset\_completions()must be called before the completions will be displayed to the user.

flags

A bitwise OR of:

*   `sublime.INHIBIT_WORD_COMPLETIONS`: prevent Sublime Text from showing completions based on the contents of the view
*   `sublime.INHIBIT_EXPLICIT_COMPLETIONS`: prevent Sublime Text from showing completions based on.sublime-completionsfiles
*   `sublime.DYNAMIC_COMPLETIONS`: if completions should be re-queried as the user types4057
*   `sublime.INHIBIT_REORDER`: prevent Sublime Text from changing the completion order4074

 |

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| set\_completions(completions, ) | `None` | 
Sets the list of completions, allowing the list to be displayed to the user.

The parameterflagsmay be a bitwise OR of:

*   `sublime.INHIBIT_WORD_COMPLETIONS`: prevent Sublime Text from showing completions based on the contents of the view
*   `sublime.INHIBIT_EXPLICIT_COMPLETIONS`: prevent Sublime Text from showing completions based on.sublime-completionsfiles
*   `sublime.DYNAMIC_COMPLETIONS`: if completions should be re-queried as the user types4057
*   `sublime.INHIBIT_REORDER`: prevent Sublime Text from changing the completion order4074

 |  |

## `sublime.QuickPanelItem`Class4083

Represents a row in the quick panel, shown via[`show_quick_panel()`](api_reference#sublime.Window.show_quick_panel).

| Constructor | Description |
| --- | --- |
| QuickPanelItem(trigger, , , ) | 
trigger

A unicode string of the text to match against the user's input.

details

An optional unicode string, or list of unicode strings, containing[limited inline HTML](minihtml#inline_formatting). Displayed below the trigger.

annotation

An optional unicode string of a hint to draw to the right-hand side of the row.

kind

An optional[kindtuple](api_reference#type-kind_info)–*defaults to`sublime.KIND_AMBIGUOUS`*.

 |

## `sublime.SymbolRegion`Class4085

Contains information about a region of a View that contains a symbol

| Properties | Type | Description |
| --- | --- | --- |
| name | str | The symbol |
| region | [Region](api_reference#sublime.Region) | The location of the symbol within the view |
| syntax | str | The name of the syntax for the symbol |
| type | int | 
The type of the symbol location - one of the following values:

*   `sublime.SYMBOL_TYPE_DEFINITION`
*   `sublime.SYMBOL_TYPE_REFERENCE`

 |
| kind | [kindtuple](api_reference#type-kind_tuple) | The kind info for the symbol. References will always be`sublime.KIND_AMBIGUOUS`. |

## `sublime.Window`Class

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| id() | int | Returns a number that uniquely identifies this window. |  |
| new\_file() | [View](api_reference#sublime.View) | Creates a new file. The returned view will be empty, and itsis\_loaded()method will return`True`. |  |
| open\_file(file\_name, , ) | [View](api_reference#sublime.View) | 
Opens the named file, and returns the corresponding view. If the file is already opened, it will be brought to the front. Note that as file loading is asynchronous, operations on the returned view won't be possible until itsis\_loading()method returns`False`.

The optionalflagsparameter is a bitwise combination of:

*   `sublime.ENCODED_POSITION`: Indicates the file\_name should be searched for a`:row`or`:row:col`suffix
*   `sublime.TRANSIENT`: Open the file as a preview only: it won't have a tab assigned it until modified
*   `sublime.FORCE_GROUP`: Don't select the file if it is open in a different group
*   `sublime.ADD_TO_SELECTION`4050: Add the file to the currently selected sheets in this group
*   `sublime.REPLACE_MRU`4096: Causes the sheet to replace the most-recently used sheet in the current sheet selection
*   `sublime.SEMI_TRANSIENT`4096: Valid in combination with`sublime.ADD_TO_SELECTION`and`sublime.REPLACE_MRU`. If a sheet is newly created, it will be set to semi-transient.

The optionalgroupparameter an a 0-based integer of the group to open the file within.`-1`specifies the active group.

 |  |
| find\_open\_file(file\_name) | [View](api_reference#sublime.View) | Finds the named file in the list of open files, and returns the corresponding View, or`None`if no such file is open. |  |
| new\_html\_sheet(name, contents, , ) | [Sheet](api_reference#sublime.Sheet) | 

Constructs a sheet with HTML contents rendered using[minihtml](minihtml).

name

A unicode string of the sheet name, shown in tab and Open Files

contents

A unicode string of the HTML contents

flags

A bitwise combination of:

*   `sublime.TRANSIENT`: If the sheet should be transient
*   `sublime.ADD_TO_SELECTION`: Add the file to the currently selected sheets in this group

group

An integer of the group to add the sheet to,`-1`for the active group

 | 4065 |
| active\_sheet() | [Sheet](api_reference#sublime.Sheet) | Returns the currently focused sheet. |  |
| active\_view() | [View](api_reference#sublime.View) | Returns the currently edited view. |  |
| active\_sheet\_in\_group(group) | [Sheet](api_reference#sublime.Sheet) | Returns the currently focused sheet in the givengroup. |  |
| active\_view\_in\_group(group) | [View](api_reference#sublime.View) | Returns the currently edited view in the givengroup. |  |
| sheets() | \[[Sheet](api_reference#sublime.Sheet)\] | Returns all open sheets in the window. |  |
| selected\_sheets() | \[[Sheet](api_reference#sublime.Sheet)\] | Returns all selected sheets in the window, across all groups | 4083 |
| sheets\_in\_group(group) | \[[Sheet](api_reference#sublime.Sheet)\] | Returns all open sheets in the givengroup. |  |
| selected\_sheets\_in\_group(group) | \[[Sheet](api_reference#sublime.Sheet)\] | Returns all selected sheets in the givengroup. | 4083 |
| select\_sheets(sheets) | `None` | Changes the selected sheets for the entire Windows.sheetsshould be alistof[Sheet](api_reference#sublime.Sheet)objects. | 4083 |
| views() | \[[View](api_reference#sublime.View)\] | Returns all open views in the window. |  |
| views\_in\_group(group) | \[[View](api_reference#sublime.View)\] | Returns all open views in the givengroup. |  |
| num\_groups() | int | Returns the number of view groups in the window. |  |
| active\_group() | int | Returns the index of the currently selected group. |  |
| focus\_group(group) | `None` | Makes the givengroupactive. |  |
| focus\_sheet(sheet) | `None` | Switches to the givensheet. |  |
| focus\_view(view) | `None` | Switches to the givenview. |  |
| focus\_window() | `None` | Switches to the window. | 4066 |
| get\_sheet\_index(sheet) | (int,int) | Returns the group, and index within the group of thesheet. Returns`-1`if not found. |  |
| set\_sheet\_index(sheet, group, index) | `None` | Moves thesheetto the givengroupandindex. |  |
| get\_view\_index(view) | (int,int) | Returns the group, and index within the group of theview. Returns`-1`if not found. |  |
| set\_view\_index(view, group, index) | `None` | Moves theviewto the givengroupandindex. |  |
| status\_message(string) | `None` | Show a message in the status bar. |  |
| bring\_to\_front() | `None` | Brings the window in front of any other windows | 4067 |
| is\_menu\_visible() | bool | Returns`True`if the menu is visible. |  |
| set\_menu\_visible(flag) | `None` | Controls if the menu is visible. |  |
| is\_sidebar\_visible() | bool | Returns`True`if the sidebar will be shown when contents are available. |  |
| set\_sidebar\_visible(flag) | `None` | Sets the sidebar to be shown or hidden when contents are available. |  |
| get\_tabs\_visible() | bool | Returns`True`if tabs will be shown for open files. |  |
| set\_tabs\_visible(flag) | `None` | Controls if tabs will be shown for open files. |  |
| is\_minimap\_visible() | bool | Returns`True`if the minimap is enabled. |  |
| set\_minimap\_visible(flag) | `None` | Controls the visibility of the minimap. |  |
| is\_status\_bar\_visible() | bool | Returns`True`if the status bar will be shown. |  |
| set\_status\_bar\_visible(flag) | `None` | Controls the visibility of the status bar. |  |
| folders() | \[str\] | Returns a list of the currently open folders. |  |
| project\_file\_name() | str | Returns name of the currently opened project file, if any. |  |
| workspace\_file\_name() | str | Returns name of the currently opened workspace file, if any. | 4050 |
| project\_data() | dict | Returns the project data associated with the current window. The data is in the same format as the contents of a.sublime-projectfile. |  |
| set\_project\_data(data) | `None` | Updates the project data associated with the current window. If the window is associated with a.sublime-projectfile, the project file will be updated on disk, otherwise the window will store the data internally. |  |
| run\_command(string, ) | `None` | Runs the named[WindowCommand](api_reference#sublime.WindowCommand)with the (optional) givenargs. This method is able to run any sort of command, dispatching the command via input focus. |  |
| show\_quick\_panel(items, on\_done, , , , ) | `None` | 

Shows a quick panel, to select an item in a list.on\_donewill be called once, with the index of the selected item. If the quick panel was cancelled,on\_donewill be called with an argument of`-1`.

itemsmay be one of the following:

*   a list of unicode strings
*   a list of unicode string lists, where the first is the trigger, and all subsequent strings are details shown below the trigger
*   a list of[QuickPanelItem](api_reference#sublime.QuickPanelItem)items4083

flagsis a bitwise OR of:

*   `sublime.MONOSPACE_FONT`
\- use a monospace font*   `sublime.KEEP_OPEN_ON_FOCUS_LOST`\- keep the quick panel open if the window loses input focus
*   `sublime.WANT_EVENT`4096\- pass a second parameter toon\_done, an[`event`Dict](api_reference#type-event_dict)

on\_highlighted, if given, will be called every time the highlighted item in the quick panel is changed.

placeholderis text displayed in the input until the user enters at least one character4081

 |  |
| show\_input\_panel(caption, initial\_text, on\_done, on\_change, on\_cancel) | [View](api_reference#sublime.View) | Shows the input panel, to collect a line of input from the user.on\_doneandon\_change, if not`None`, should both be functions that expect a single string argument.on\_cancelshould be a function that expects no arguments. The view used for the input widget is returned. |  |
| create\_output\_panel(name, ) | [View](api_reference#sublime.View) | 

Returns the view associated with the named output panel, creating it if required. The output panel can be shown by running theshow\_panelwindow command, with thepanelargument set to the name with an`"output."`prefix.

The optionalunlistedparameter is a boolean to control if the output panel should be listed in the panel switcher.

 |  |
| find\_output\_panel(name) | [View](api_reference#sublime.View)*or*None | Returns the view associated with the named output panel, or`None`if the output panel does not exist. |  |
| destroy\_output\_panel(name) | `None` | Destroys the named output panel, hiding it if currently open. |  |
| active\_panel() | str*or*`None` | Returns the name of the currently open panel, or`None`if no panel is open. Will return built-in panel names (e.g.`"console"`,`"find"`, etc) in addition to output panels. |  |
| panels() | \[str\] | Returns a list of the names of all panels that have not been marked as unlisted. Includes certain built-in panels in addition to output panels. |  |
| lookup\_symbol\_in\_index(symbol) | \[[location](api_reference#type-location)\] | *DEPRECATED - use`symbol_locations()`when possible.*Returns all locations where the symbol is defined across files in the current project. |  |
| lookup\_symbol\_in\_open\_files(symbol) | \[[location](api_reference#type-location)\] | *DEPRECATED - use`symbol_locations()`when possible.*Returns all locations where the symbol is defined across open files. |  |
| symbol\_locations(sym, , , , ) | \[[SymbolLocation](api_reference#sublime.SymbolLocation)\] | 

Finds all locations where the symbolsymis located.

sourcecontrols where definitions are sourced from, and may be one of:

*   `sublime.SYMBOL_SOURCE_ANY`\- locations in the index, or in open files
*   `sublime.SYMBOL_SOURCE_INDEX`\- locations in the index, i.e. files saved to disk in a project folder
*   `sublime.SYMBOL_SOURCE_OPEN_FILES`\- locations in open files, even in unsaved or outside of the project

typecontrols which type of symbol locations are returned, and may be one of:

*   `sublime.SYMBOL_TYPE_ANY`\- locations that are definitions or references
*   `sublime.SYMBOL_TYPE_DEFINITION`\- locations that are symbol definitions
*   `sublime.SYMBOL_TYPE_REFERENCE`\- locations that are symbol references

kind\_idfilters symbol locations by their kind. References will always be`sublime.KIND_AMBIGUOUS`. See[kindtuple](api_reference#type-kind_tuple)for valid values.

kind\_letterfilters symbol locations additionally by the unicode character representing their kind.

 | 4085 |
| extract\_variables() | dict | 

Returns adictof strings populated with contextual keys:

"packages""platform""file""file\_path""file\_name""file\_base\_name""file\_extension""folder""project""project\_path""project\_name""project\_base\_name""project\_extension"

Thisdictis suitable for passing to`sublime.expand_variables()`.

 |  |

## `sublime.SymbolLocation`Class4085

Contains information about a file that contains a symbol

| Properties | Type | Description |
| --- | --- | --- |
| path | str | The filesystem path to the file containing the symbol |
| display\_name | str | The project-relative path to the file containing the symbol |
| row | int | The row of the file the symbol is contained on |
| col | int | The column of the row that the symbol is contained on |
| syntax | str | The name of the syntax for the symbol |
| type | int | 
The type of the symbol location - one of the following values:

*   `sublime.SYMBOL_TYPE_DEFINITION`
*   `sublime.SYMBOL_TYPE_REFERENCE`

 |
| kind | [kindtuple](api_reference#type-kind_tuple) | The kind info for the symbol. References will always be`sublime.KIND_AMBIGUOUS`. |

## `sublime.Settings`Class

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| get(name, ) | [value](api_reference#type-value) | Returns the named setting, ordefaultif it's not defined. If not passed,defaultwill have a value of`None`. |  |
| set(name, value) | `None` | Sets the named setting. Only primitive types,lists, anddicts are accepted. |  |
| erase(name) | `None` | Removes the named setting. Does not remove it from any parent Settings. |  |
| has(name) | bool | Returns`True`iff the named option exists in this set of Settings or one of its parents. |  |
| add\_on\_change(key, on\_change) | `None` | Register a callback to be run whenever a setting in this object is changed. |  |
| clear\_on\_change(key) | `None` | Remove all callbacks registered with the givenkey. |  |
| to\_dict() | dict | Returns a copy of the settings as adict | 40783.8 |
| update(pairs) | `None` | 
Update the settings frompairs, which may be any of the following:

*   Adict
*   An implementation ofcollections.abc.Mapping
*   An object that has akeys()method
*   An object that provides key/value pairs when iterated
*   Keyword arguments

 | 40783.8 |

## `sublime_plugin`Module

| Methods | Return Value | Description |
| --- | --- | --- |
| (no methods) |  |  |

## `sublime_plugin.EventListener`Class

Note that many of these events are triggered by the buffer underlying the view, and thus the method is only called once, with the first view as the parameter.

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| on\_init(\[views\]) | `None` | Called once with a list of views that were loaded before the EventListener was instantiated | 4050 |
| on\_exit() | `None` | Called once after the API has shut down, immediately before theplugin\_hostprocess exits | 4050 |
| on\_new(view) | `None` | Called when a new file is created. |  |
| on\_new\_async(view) | `None` | Called when a new buffer is created. Runs in a separate thread, and does not block the application. |  |
| on\_associate\_buffer(buffer) | `None` | Called when a buffer is associated with a file.bufferwill be a[Buffer](api_reference#sublime.Buffer)object. | 4084 |
| on\_associate\_buffer\_async(buffer) | `None` | Called when a buffer is associated with file. Runs in a separate thread, and does not block the application.bufferwill be a[Buffer](api_reference#sublime.Buffer)object. | 4084 |
| on\_clone(view) | `None` | Called when a view is cloned from an existing one. |  |
| on\_clone\_async(view) | `None` | Called when a view is cloned from an existing one. Runs in a separate thread, and does not block the application. |  |
| on\_load(view) | `None` | Called when the file is finished loading. |  |
| on\_load\_async(view) | `None` | Called when the file is finished loading. Runs in a separate thread, and does not block the application. |  |
| on\_reload(view) | `None` | Called when the[View](api_reference#sublime.View)is reloaded. | 4050 |
| on\_reload\_async(view) | `None` | Called when the[View](api_reference#sublime.View)is reloaded. Runs in a separate thread, and does not block the application. | 4050 |
| on\_revert(view) | `None` | Called when the[View](api_reference#sublime.View)is reverted. | 4050 |
| on\_revert\_async(view) | `None` | Called when the[View](api_reference#sublime.View)is reverted. Runs in a separate thread, and does not block the application. | 4050 |
| on\_pre\_move(view) | `None` | Called right before a view is moved between two windows, passed the[View](api_reference#sublime.View)object. | 4050 |
| on\_post\_move(view) | `None` | Called right after a view is moved between two windows, passed the[View](api_reference#sublime.View)object. | 4050 |
| on\_post\_move\_async(view) | `None` | Called right after a view is moved between two windows, passed the[View](api_reference#sublime.View)object. Runs in a separate thread, and does not block the application. | 4050 |
| on\_pre\_close(view) | `None` | Called when a view is about to be closed. The view will still be in the window at this point. |  |
| on\_close(view) | `None` | Called when a view is closed (note, there may still be other views into the same buffer). |  |
| on\_pre\_save(view) | `None` | Called just before a view is saved. |  |
| on\_pre\_save\_async(view) | `None` | Called just before a view is saved. Runs in a separate thread, and does not block the application. |  |
| on\_post\_save(view) | `None` | Called after a view has been saved. |  |
| on\_post\_save\_async(view) | `None` | Called after a view has been saved. Runs in a separate thread, and does not block the application. |  |
| on\_modified(view) | `None` | Called after changes have been made to a view. |  |
| on\_modified\_async(view) | `None` | Called after changes have been made to a view. Runs in a separate thread, and does not block the application. |  |
| on\_selection\_modified(view) | `None` | Called after the selection has been modified in a view. |  |
| on\_selection\_modified\_async(view) | `None` | Called after the selection has been modified in a view. Runs in a separate thread, and does not block the application. |  |
| on\_activated(view) | `None` | Called when a view gains input focus. |  |
| on\_activated\_async(view) | `None` | Called when a view gains input focus. Runs in a separate thread, and does not block the application. |  |
| on\_deactivated(view) | `None` | Called when a view loses input focus. |  |
| on\_deactivated\_async(view) | `None` | Called when a view loses input focus. Runs in a separate thread, and does not block the application. |  |
| on\_hover(view, point, hover\_zone) | `None` | 
Called when the user's mouse hovers over a view for a short period.

pointis the closest point in the view to the mouse location. The mouse may not actually be located adjacent based on the value ofhover\_zone:

*   `sublime.HOVER_TEXT`: When the mouse is hovered over text.
*   `sublime.HOVER_GUTTER`: When the mouse is hovered over the gutter.
*   `sublime.HOVER_MARGIN`: When the mouse is hovered in whitespace to the right of a line.

 |  |
| on\_query\_context(view, key, operator, operand, match\_all) | bool*or*`None` | 

Called when determining to trigger a key binding with the given contextkey. If the plugin knows how to respond to the context, it should return either`True`or`False`. If the context is unknown, it should return`None`.

operatoris one of:

*   `sublime.OP_EQUAL`: Is the value of the context equal to the operand?
*   `sublime.OP_NOT_EQUAL`: Is the value of the context not equal to the operand?
*   `sublime.OP_REGEX_MATCH`: Does the value of the context match the regex given in operand?
*   `sublime.OP_NOT_REGEX_MATCH`: Does the value of the context not match the regex given in operand?
*   `sublime.OP_REGEX_CONTAINS`: Does the value of the context contain a substring matching the regex given in operand?
*   `sublime.OP_NOT_REGEX_CONTAINS`: Does the value of the context not contain a substring matching the regex given in operand?

match\_allshould be used if the context relates to the selections: does every selection have to match (`match_all==True`), or is at least one matching enough (`match_all==False`)?

 |  |
| on\_query\_completions(view, prefix, locations) | `None`*or*list*or*tuple*or*[CompletionList](api_reference#sublime.CompletionList) | 

Called whenever completions are to be presented to the user. Theprefixis a unicode string of the text to complete.

locationsis a list of[points](api_reference#type-point). Since this method is called for all completions in every view no matter the syntax,`view.match_selector(point,relevant_scope)`should be called to determine if the point is relevant.

The return value must be one of the following formats:

*   `None`: no completions are provided
    
    ~~~
    return None
    ~~~
    
*   A list of[completion](api_reference#type-completion)values
    
    ~~~
    return [
        ["me1", "method1()"],
        ["me2", "method2()"]
    ]
    ~~~
    
*   A 2-element tuple with the first element being a list of[completion](api_reference#type-completion)values, and the second element being flags composed via bitwise OR of:
    
    *   `sublime.INHIBIT_WORD_COMPLETIONS`: prevent Sublime Text from showing completions based on the contents of the view
    *   `sublime.INHIBIT_EXPLICIT_COMPLETIONS`: prevent Sublime Text from showing completions based on.sublime-completionsfiles
    *   `sublime.DYNAMIC_COMPLETIONS`: if completions should be re-queried as the user types4057
    *   `sublime.INHIBIT_REORDER`: prevent Sublime Text from changing the completion order4074
    
    ~~~
    return (
        [
            ["me1", "method1()"],
            ["me2", "method2()"]
        ],
        sublime.INHIBIT_WORD_COMPLETIONS
        | sublime.INHIBIT_EXPLICIT_COMPLETIONS
    )
    ~~~
    
*   A[CompletionList](api_reference#sublime.CompletionList)object4050
    
    ~~~
    cl = sublime.CompletionList(flags=sublime.INHIBIT_WORD_COMPLETIONS)
    start_background_fetch(cl)
    return cl
    ~~~
    

 |  |
| on\_text\_command(view, command\_name, args) | (str,dict) | Called when a text command is issued. The listener may return a`(command,arguments)`tuple to rewrite the command, or`None`to run the command unmodified. |  |
| on\_window\_command(window, command\_name, args) | (str,dict) | Called when a window command is issued. The listener may return a`(command,arguments)`tuple to rewrite the command, or`None`to run the command unmodified. |  |
| on\_post\_text\_command(view, command\_name, args) | `None` | Called after a text command has been executed. |  |
| on\_post\_window\_command(window, command\_name, args) | `None` | Called after a window command has been executed. |  |
| on\_new\_window(window) | `None` | Called when a window is created, passed the[Window](api_reference#sublime.Window)object. | 4050 |
| on\_new\_window\_async(window) | `None` | Called when a window is created, passed the[Window](api_reference#sublime.Window)object. Runs in a separate thread, and does not block the application. | 4050 |
| on\_pre\_close\_window(window) | `None` | Called right before a window is closed, passed the[Window](api_reference#sublime.Window)object. | 4050 |
| on\_new\_project(window) | `None` | Called right after a new project is created, passed the[Window](api_reference#sublime.Window)object. | 4050 |
| on\_new\_project\_async(window) | `None` | Called right after a new project is created, passed the[Window](api_reference#sublime.Window)object. Runs in a separate thread, and does not block the application. | 4050 |
| on\_load\_project(window) | `None` | Called right after a project is loaded, passed the[Window](api_reference#sublime.Window)object. | 4050 |
| on\_load\_project\_async(window) | `None` | Called right after a project is loaded, passed the[Window](api_reference#sublime.Window)object. Runs in a separate thread, and does not block the application. | 4050 |
| on\_pre\_save\_project(window) | `None` | Called right before a project is saved, passed the[Window](api_reference#sublime.Window)object. | 4050 |
| on\_post\_save\_project(window) | `None` | Called right after a project is saved, passed the[Window](api_reference#sublime.Window)object. | 4050 |
| on\_post\_save\_project\_async(window) | `None` | Called right after a project is saved, passed the[Window](api_reference#sublime.Window)object. Runs in a separate thread, and does not block the application. | 4050 |
| on\_pre\_close\_project(window) | `None` | Called right before a project is closed, passed the[Window](api_reference#sublime.Window)object. | 4050 |

## `sublime_plugin.ViewEventListener`Class

A class that provides similar event handling to[EventListener](api_reference#sublime.EventListener), but bound to a specific view. Provides class method-based filtering to control what views objects are created for.

The view is passed as a single parameter to the constructor. The default implementation makes the view available via`self.view`.

| Class Methods | Return Value | Description |
| --- | --- | --- |
| is\_applicable(settings) | bool | A`@classmethod`that receives a[Settings](api_reference#sublime.Settings)object and should return a bool indicating if this class applies to a view with those settings |
| applies\_to\_primary\_view\_only() | bool | A`@classmethod`that should return aboolindicating if this class applies only to the primary view for a file. A view is considered primary if it is the only, or first, view into a file. |

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| on\_load() | `None` | Called when the file is finished loading. | 3155 |
| on\_load\_async() | `None` | Called when the file is finished loading. Runs in a separate thread, and does not block the application. | 3155 |
| on\_reload() | `None` | Called when the file is reloaded. | 4050 |
| on\_reload\_async() | `None` | Called when the file is reloaded. Runs in a separate thread, and does not block the application. | 4050 |
| on\_revert() | `None` | Called when the file is reverted. | 4050 |
| on\_revert\_async() | `None` | Called when the file is reverted. Runs in a separate thread, and does not block the application. | 4050 |
| on\_pre\_move() | `None` | Called right before a view is moved between two windows. | 4050 |
| on\_post\_move() | `None` | Called right after a view is moved between two windows. | 4050 |
| on\_post\_move\_async() | `None` | Called right after a view is moved between two windows. Runs in a separate thread, and does not block the application. | 4050 |
| on\_pre\_close() | `None` | Called when a view is about to be closed. The view will still be in the window at this point. | 3155 |
| on\_close() | `None` | Called when a view is closed (note, there may still be other views into the same buffer). | 3155 |
| on\_pre\_save() | `None` | Called just before a view is saved. | 3155 |
| on\_pre\_save\_async() | `None` | Called just before a view is saved. Runs in a separate thread, and does not block the application. | 3155 |
| on\_post\_save() | `None` | Called after a view has been saved. | 3155 |
| on\_post\_save\_async() | `None` | Called after a view has been saved. Runs in a separate thread, and does not block the application. | 3155 |
| on\_modified() | `None` | Called after changes have been made to the view. |  |
| on\_modified\_async() | `None` | Called after changes have been made to the view. Runs in a separate thread, and does not block the application. |  |
| on\_text\_changed(\[changes\]) | `None` | 
Called once after changes has been made to a view, with detailed information about what has changed.

changesis alistof[TextChange](api_reference#sublime.TextChange)objects.

 | 4050 |
| on\_text\_changed\_async(\[changes\]) | `None` | 

Called once after changes has been made to a view, with detailed information about what has changed. Runs in a separate thread, and does not block the application.

changesis alistof[TextChange](api_reference#sublime.TextChange)objects.

 | 4050 |
| on\_selection\_modified() | `None` | Called after the selection has been modified in the view. |  |
| on\_selection\_modified\_async() | `None` | Called after the selection has been modified in the view. Runs in a separate thread, and does not block the application. |  |
| on\_activated() | `None` | Called when a view gains input focus. |  |
| on\_activated\_async() | `None` | Called when the view gains input focus. Runs in a separate thread, and does not block the application. |  |
| on\_deactivated() | `None` | Called when the view loses input focus. |  |
| on\_deactivated\_async() | `None` | Called when the view loses input focus. Runs in a separate thread, and does not block the application. |  |
| on\_hover(point, hover\_zone) | `None` | 

Called when the user's mouse hovers over the view for a short period.

pointis the closest point in the view to the mouse location. The mouse may not actually be located adjacent based on the value ofhover\_zone:

*   `sublime.HOVER_TEXT`: When the mouse is hovered over text.
*   `sublime.HOVER_GUTTER`: When the mouse is hovered over the gutter.
*   `sublime.HOVER_MARGIN`: When the mouse is hovered in whitespace to the right of a line.

 |  |
| on\_query\_context(key, operator, operand, match\_all) | bool*or*`None` | 

Called when determining to trigger a key binding with the given contextkey. If the plugin knows how to respond to the context, it should return either`True`of`False`. If the context is unknown, it should return`None`.

operatoris one of:

*   `sublime.OP_EQUAL`: Is the value of the context equal to the operand?
*   `sublime.OP_NOT_EQUAL`: Is the value of the context not equal to the operand?
*   `sublime.OP_REGEX_MATCH`: Does the value of the context match the regex given in operand?
*   `sublime.OP_NOT_REGEX_MATCH`: Does the value of the context not match the regex given in operand?
*   `sublime.OP_REGEX_CONTAINS`: Does the value of the context contain a substring matching the regex given in operand?
*   `sublime.OP_NOT_REGEX_CONTAINS`: Does the value of the context not contain a substring matching the regex given in operand?

match\_allshould be used if the context relates to the selections: does every selection have to match (`match_all==True`), or is at least one matching enough (`match_all==False`)?

 |  |
| on\_query\_completions(prefix, locations) | `None`*or*list*or*tuple*or*[CompletionList](api_reference#sublime.CompletionList) | 

Called whenever completions are to be presented to the user. Theprefixis a unicode string of the text to complete.

locationsis a list of[points](api_reference#type-point). Since this method is called for all completions no matter the syntax,`self.view.match_selector(point,relevant_scope)`should be called to determine if the point is relevant.

The return value must be one of the following formats:

*   `None`: no completions are provided
    
    ~~~
    return None
    ~~~
    
*   A list of[completion](api_reference#type-completion)values
    
    ~~~
    return [
        ["me1", "method1()"],
        ["me2", "method2()"]
    ]
    ~~~
    
*   A 2-element tuple with the first element being a list of[completion](api_reference#type-completion)values, and the second element being flags composed via bitwise OR of:
    
    *   `sublime.INHIBIT_WORD_COMPLETIONS`: prevent Sublime Text from showing completions based on the contents of the view
    *   `sublime.INHIBIT_EXPLICIT_COMPLETIONS`: prevent Sublime Text from showing completions based on.sublime-completionsfiles
    *   `sublime.DYNAMIC_COMPLETIONS`: if completions should be re-queried as the user types4057
    *   `sublime.INHIBIT_REORDER`: prevent Sublime Text from changing the completion order4074
    
    ~~~
    return (
        [
            ["me1", "method1()"],
            ["me2", "method2()"]
        ],
        sublime.INHIBIT_WORD_COMPLETIONS
        | sublime.INHIBIT_EXPLICIT_COMPLETIONS
    )
    ~~~
    
*   A[CompletionList](api_reference#sublime.CompletionList)object4050
    
    ~~~
    cl = sublime.CompletionList(flags=sublime.INHIBIT_WORD_COMPLETIONS)
    start_background_fetch(cl)
    return cl
    ~~~
    

 |  |
| on\_text\_command(command\_name, args) | (str,dict) | Called when a text command is issued. The listener may return a`(command,arguments)`tuple to rewrite the command, or`None`to run the command unmodified. | 3155 |
| on\_post\_text\_command(command\_name, args) | `None` | Called after a text command has been executed. | 3155 |

## `sublime_plugin.ApplicationCommand`Class

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| run() | `None` | Called when the command is run. |  |
| is\_enabled() | bool | Returns`True`if the command is able to be run at this time. The default implementation simply always returns`True`. |  |
| is\_visible() | bool | Returns`True`if the command should be shown in the menu at this time. The default implementation always returns`True`. |  |
| is\_checked() | bool | Returns`True`if a checkbox should be shown next to the menu item. The.sublime-menufile must have the"checkboxkey set to`true`for this to be used. |  |
| description() | str | Returns a description of the command with the given arguments. Used in the menu, if no caption is provided. Return`None`to get the default description. |  |
| input(args) | [CommandInputHandler](api_reference#type-CommandInputHandler)*or*`None` | If this returns something other than`None`, the user will be prompted for an input before the command is run in theCommand Palette. | 3154 |
| input\_description() | str | Allows a custom name to be show to the left of the cursor in the input box, instead of the default one generated from the command name | 3154 |

## `sublime_plugin.WindowCommand`Class

WindowCommands are instantiated once per window. The Window object may be retrieved via`self.window`

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| run() | `None` | Called when the command is run. |  |
| is\_enabled() | bool | Returns`True`if the command is able to be run at this time. The default implementation simply always returns`True`. |  |
| is\_visible() | bool | Returns`True`if the command should be shown in the menu at this time. The default implementation always returns`True`. |  |
| description() | str | Returns a description of the command with the given arguments. Used in the menu, if no caption is provided. Return`None`to get the default description. |  |
| input(args) | [CommandInputHandler](api_reference#type-CommandInputHandler)*or*`None` | If this returns something other than None, the user will be prompted for an input before the command is run in the Command Palette. | 3154 |
| input\_description() | str | Allows a custom name to be show to the left of the cursor in the input box, instead of the default one generated from the command name | 3154 |

## `sublime_plugin.TextCommand`Class

TextCommands are instantiated once per view. The View object may be retrieved via`self.view`

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| run(edit, ) | `None` | Called when the command is run. |  |
| is\_enabled() | bool | Returns`True`if the command is able to be run at this time. The default implementation simply always returns`True`. |  |
| is\_visible() | bool | Returns`True`if the command should be shown in the menu at this time. The default implementation always returns`True`. |  |
| description() | str | Returns a description of the command with the given arguments. Used in the menus, and for Undo / Redo descriptions. Return`None`to get the default description. |  |
| want\_event() | bool | Return`True`to receive aneventargument when the command is triggered by a mouse action. The event information allows commands to determine which portion of the view was clicked on. The default implementation returns`False`. |  |
| input(args) | [CommandInputHandler](api_reference#type-CommandInputHandler)*or*`None` | If this returns something other than`None`, the user will be prompted for an input before the command is run in theCommand Palette. | 3154 |
| input\_description() | str | Allows a custom name to be show to the left of the cursor in the input box, instead of the default one generated from the command name | 3154 |

## `sublime_plugin.TextInputHandler`Class3154

TextInputHandlers can be used to accept textual input in theCommand Palette. Return a subclass of this from theinput()method of a command.

*For an input handler to be shown to the user, the command returning the input handler**MUST**be made available in the Command Palette by adding the command to aDefault.sublime-commandsfile.*

| Methods | Return Value | Description |  |
| --- | --- | --- | --- |
| name() | str | The command argument name this input handler is editing. Defaults to`foo_bar`for an input handler named`FooBarInputHandler` |  |
| placeholder() | str | Placeholder text is shown in the text entry box before the user has entered anything. Empty by default. |  |
| initial\_text() | str | Initial text shown in the text entry box. Empty by default. |  |
| initial\_selection() | \[tuple\] | A list of 2-element tuplues, defining the initially selected parts of the initial text | 4081 |
| preview(text) | str*or*sublime | Called whenever the user changes the text in the entry box. The returned value (either plain text or HTML) will be shown in the preview area of the Command Palette. |  |
| validate(text) | bool | Called whenever the user presses enter in the text entry box. Return`False`to disallow the current value. |  |
| cancel() | `None` | Called when the input handler is canceled, either by the user pressing backspace or escape. |  |
| confirm(text) | `None` | Called when the input is accepted, after the user has pressed enter and the text has been validated. |  |
| next\_input(args) | [CommandInputHandler](api_reference#type-CommandInputHandler)*or*`None` | Returns the next input after the user has completed this one. May return`None`to indicate no more input is required, or`sublime_plugin.BackInputHandler()`to indicate that the input handler should be poped off the stack instead. |  |
| description(text) | str | The text to show in the Command Palette when this input handler is not at the top of the input handler stack. Defaults to the text the user entered. |  |

## `sublime_plugin.ListInputHandler`Class3154

ListInputHandler objects can be used to accept a choice input from a list items in the Command Palette. Return a subclass of this from theinput()method of a command.

*For an input handler to be shown to the user, the command returning the input handler**MUST**be made available in the Command Palette by adding the command to aDefault.sublime-commandsfile.*

| Methods | Return Value | Description |
| --- | --- | --- |
| name() | str | The command argument name this input handler is editing. Defaults to`foo_bar`for an input handler named`FooBarInputHandler` |  |
| list\_items() | \[str\]*or*(\[str\], int)*or*\[(str,[value](api_reference#type-value))\]*or*(\[(str,[value](api_reference#type-value))\], int)*or*\[[ListInputItem](api_reference#sublime.ListInputItem)\]*or*(\[[ListInputItem](api_reference#sublime.ListInputItem)\], int) | 
This method should return the items to show in the list.

The returned value may be alistof item, or a 2-elementtuplecontaining a list of items, and anintindex of the item to pre-select.

The each item in the list may be one of:

*   A unicode string - used for both the row text and the value passed to the command
*   A 2-elementtuplecontaining a unicode string for the row text, and a[value](api_reference#type-value)to pass to the command
*   A[ListInputItemobject4095](api_reference#sublime.ListInputItem)
[](api_reference#sublime.ListInputItem)

[](api_reference#sublime.ListInputItem) |  |
| placeholder() | str | Placeholder text is shown in the text entry box before the user has entered anything. Empty by default. |  |
| initial\_text() | str | Initial text shown in the filter box. Empty by default. |  |
| initial\_selection() | \[tuple\] | A list of 2-element tuplues, defining the initially selected parts of the initial text | 4081 |
| preview(value) | str*or*sublime | Called whenever the user changes the selected item. The returned value (either plain text or HTML) will be shown in the preview area of the Command Palette. |  |
| validate(value) | bool | Called whenever the user presses enter in the text entry box. Return`False`to disallow the current value. |  |
| cancel() | `None` | Called when the input handler is canceled, either by the user pressing backspace or escape. |  |
| confirm(value) | `None` | Called when the input is accepted, after the user has pressed enter and the item has been validated. |  |
| next\_input(args) | [CommandInputHandler](api_reference#type-CommandInputHandler)*or*`None` | Returns the next input after the user has completed this one. May return`None`to indicate no more input is required, or`sublime_plugin.BackInputHandler()`to indicate that the input handler should be poped off the stack instead. |  |
| description(value, text) | str | The text to show in the Command Palette when this input handler is not at the top of the input handler stack. Defaults to the text of the list item the user selected. |  |
| want\_event() | bool | If the`validate()`and`confirm()`methods should receive a second parameter, an[`event`Dict](api_reference#type-event_dict) | 4096 |

## `sublime.ListInputItem`Class4095

Represents a row shown via[ListInputHandler](api_reference#sublime_plugin.ListInputHandler).

| Constructor | Description |
| --- | --- |
| ListInputItem(text, value, , , ) | 
text

A unicode string of the text to match against the user's input.

value

A[value](api_reference#type-value)to pass to the command if the row is selected

details

An optional unicode string, or list of unicode strings, containing[limited inline HTML](minihtml#inline_formatting). Displayed below the text.

annotation

An optional unicode string of a hint to draw to the right-hand side of the row.

kind

An optional[kindtuple](api_reference#type-kind_info)–*defaults to`sublime.KIND_AMBIGUOUS`*.

 |