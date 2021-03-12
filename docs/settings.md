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

* 用户界面设置和应用程序行为设置对整个应用程序是全局的，不能由特定于语法的设置文件或.sublime-project中的设置键控制。*

## Settings Files

按以下顺序查阅设置文件:

1.  Packages/Default/Preferences.sublime-settings
2.  Packages/Default/Preferences (**).sublime-settings
3.  **Packages/User/Preferences.sublime-settings**
4.  **
5.  Packages/**/**.sublime-settings
6.  **Packages/User/**.sublime-settings**
7.  **

通常，你应该放置你的设置在 Packages/User/Preferences.sublime-settings, 在选择了首选项[▶](images/right.svg)设置后打开的面板右侧。 如果要指定特定文件类型 (例如Python) 的设置，则应将其放置在package/User/Python.sublime-settings中。这个可以通过打开Python文件时，选择首选项[▶](images/right.svg)设置 – 特定语法选中时打开的面板右侧去定义。

## Syntax-Specific Settings

可以按语法指定设置。此操作的常见用途是具有不同的缩进设置或配色方案因文件类型而异。

你可以通过选择 选择 首选项 [▶](images/right.svg)- 语法 语法指定菜单项来编辑当前文件的语法。

* 请注意，只能在特定于语法的设置中指定编辑器设置。*

## Project Settings

设置可以在每个项目的基础上设置，详细信息在[Project Documentation](projects).

* 请注意，只能在项目设置中指定编辑器设置。*

## Distraction Free Settings

[Distraction Free Mode](distraction_free) 应用了附加设置文件 (Distraction Free.sublime-settings)。您可以在此处放置文件设置，以使它们仅在无干扰模式下应用 – 访问首选项[▶](images/right.svg)无干扰模式。

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