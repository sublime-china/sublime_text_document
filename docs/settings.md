# 

[DOCUMENTATION](index)[TOC](settings#toc)[TOP](settings#)

设置

Sublime Text有许多不同的设置来自定义其行为。通过编辑文本文件来更改设置: 虽然这比使用图形用户界面要复杂一点，但你会得到一个灵活的系统。

通过首选项访问![▶](images/right.svg) 设置菜单项目。左侧窗格包含所有默认设置以及每个设置的说明。右侧窗格可保存自定义。

*   [Categories](settings#categories)
*   [Settings Files](settings#settings_files)
*   [Syntax-Specific Settings](settings#syntax-specific_settings)
*   [Project Settings](settings#project_settings)
*   [Distraction Free Settings](settings#distraction_free_settings)
*   [Key Bindings](settings#key_bindings)
*   [Troubleshooting](settings#troubleshooting)

## Categories
 
分类 
Sublime Text 中的设置 组织成为三个分类。 默认设置文件将设置组织成几个部分，以便于区分。

*   **编辑设置**: 这些设置会影响编辑文件中的文本时呈现的行为和功能。示例包括 font\_face, tab\_size 和 spell\_check。这些设置显示在默认设置文件的第一部分中。 
*   **用户界面设置**: 这些设置会影响所有打开的窗口的常规用户界面。示例包括主题、动画启用和覆盖滚动条。这些设置显示在默认设置文件的第二部分。 
*   **应用行为设置**: 这些设置会在所有打开的窗口中影响应用程序的行为。示例包括 hot\_exit、index\_files 和 ignored\_packages。这些设置显示在默认设置文件的第三部分。

*The User Interface Settings and Application Behavior Settings are global to the entire application and can not be controlled by a syntax specific settings file, nor thesettingskey in a.sublime-project.*

## Settings Files

Settings files are consulted in this order:

1.  Packages/Default/Preferences.sublime-settings
2.  Packages/Default/Preferences (**).sublime-settings
3.  **Packages/User/Preferences.sublime-settings**
4.  **
5.  Packages/**/**.sublime-settings
6.  **Packages/User/**.sublime-settings**
7.  **

In general, you should place your settings inPackages/User/Preferences.sublime-settings, which is opened in the right-hand pane when selecting the menu itemPreferences![▶](images/right.svg)Settings. If you want to specify settings for a certain file type, for example, Python, you should place them inPackages/User/Python.sublime-settings. This can be accessed via the right-hand pane when a Python file is open, and the menu itemPreferences![▶](images/right.svg)Settings – Syntax Specificis selected.

## Syntax-Specific Settings

Settings may be specified on a per-syntax basis. Common uses for this are to have different indentation settings or the color scheme vary by file type.

You can edit the settings for the syntax of the current file by selecting thePreferences![▶](images/right.svg)Settings – Syntax Specificmenu item.

*Note that only Editor Settings can be specified in syntax-specific settings.*

## Project Settings

Settings can be set on a per-project basis, details are in the[Project Documentation](projects).

*Note that only Editor Settings can be specified in project settings.*

## Distraction Free Settings

[Distraction Free Mode](distraction_free)has an additional settings file applied (Distraction Free.sublime-settings). You can place file settings in here to have them only apply when in Distraction Free Mode – access it from thePreferences![▶](images/right.svg)Settings – Distraction Freemenu item.

## Changing Settings with a Key Binding

toggle\_setting 命令可用于切换设置。例如，要创建一个键绑定来切换当前文件的 word\_wrap 设置，您可以使用

 (在首选项[▶](images/right.svg)快捷键设置):

~~~
{
    "keys": ["alt+w"],
    "command": "toggle_setting",
    "args":
    {
        "setting": "word_wrap"
    }
}

~~~

set\_settings 命令可用于将设置设置为特定值。例如，此键绑定使当前文件使用钴色方案:

~~~
{
    "keys": ["ctrl+k", "ctrl+c"],
    "command": "set_setting",
    "args":
    {
        "setting": "color_scheme",
        "value": "Packages/Color Scheme - Default/Cobalt.tmTheme"
    }
}

~~~

此处修改的设置是特定于缓冲区的设置: 它们覆盖设置文件中的所有设置，但仅应用于当前文件。

## 故障排除

由于可以在多个不同的位置指定设置，因此有时可以帮助查看当前文件实际使用的应用设置。您可以使用控制台执行此操作:

`view.settings().get('font_face')`