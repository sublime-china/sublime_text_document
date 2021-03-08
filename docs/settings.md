# 

[DOCUMENTATION](index)[TOC](settings#toc)[TOP](settings#)

Settings

Sublime Text has many different settings to customize its behavior. Settings are changed by editing text files: while this is a little trickier than using a GUI, you're rewarded with a flexible system.

Settings are accessed via thePreferences![▶](https://www.sublimetext.com/images/right.svg)Settingsmenu item. The left-hand pane contains all of the default settings, along with a description of each. The right-hand pane is where customization can be saved.

*   [Categories](settings#categories)
*   [Settings Files](settings#settings_files)
*   [Syntax-Specific Settings](settings#syntax-specific_settings)
*   [Project Settings](settings#project_settings)
*   [Distraction Free Settings](settings#distraction_free_settings)
*   [Key Bindings](settings#key_bindings)
*   [Troubleshooting](settings#troubleshooting)

## Categories

The settings in Sublime Text are organized into three categories. The default settings file organizes the settings into sections for easier distinction.

*   **Editor Settings**: These settings affect the behavior and functionality presented when editing text in a file. Examples include thefont\_face,tab\_sizeandspell\_check. These settings are presented in the first section of the default settings file.
*   **User Interface Settings**: These settings affect the general user interface, across all open windows. Examples include thetheme,animation\_enabledandoverlay\_scroll\_bars. These settings are presented in the second section of the default settings file.
*   **Application Behavior Settings**: These settings affect the behavior of the application, across all open windows. Examples include thehot\_exit,index\_filesandignored\_packages. These settings are presented in the third section of the default settings file.

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

In general, you should place your settings inPackages/User/Preferences.sublime-settings, which is opened in the right-hand pane when selecting the menu itemPreferences![▶](https://www.sublimetext.com/images/right.svg)Settings. If you want to specify settings for a certain file type, for example, Python, you should place them inPackages/User/Python.sublime-settings. This can be accessed via the right-hand pane when a Python file is open, and the menu itemPreferences![▶](https://www.sublimetext.com/images/right.svg)Settings – Syntax Specificis selected.

## Syntax-Specific Settings

Settings may be specified on a per-syntax basis. Common uses for this are to have different indentation settings or the color scheme vary by file type.

You can edit the settings for the syntax of the current file by selecting thePreferences![▶](https://www.sublimetext.com/images/right.svg)Settings – Syntax Specificmenu item.

*Note that only Editor Settings can be specified in syntax-specific settings.*

## Project Settings

Settings can be set on a per-project basis, details are in the[Project Documentation](projects).

*Note that only Editor Settings can be specified in project settings.*

## Distraction Free Settings

[Distraction Free Mode](distraction_free)has an additional settings file applied (Distraction Free.sublime-settings). You can place file settings in here to have them only apply when in Distraction Free Mode – access it from thePreferences![▶](https://www.sublimetext.com/images/right.svg)Settings – Distraction Freemenu item.

## Changing Settings with a Key Binding

Thetoggle\_settingcommand can be used to toggle a setting. For example, to make a key binding that toggles theword\_wrapsetting on the current file, you can use (inPreferences![▶](https://www.sublimetext.com/images/right.svg)Key Bindings):

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

Theset\_settingcommand can be used to set a setting to a specific value. For example, this key binding makes the current file use the Cobalt color scheme:

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

The settings modified here are buffer specific settings: they override any settings placed in a settings file, but apply to the current file only.

## Troubleshooting

As settings can be specified in several different places, sometimes in can be helpful to view the applied setting that's actually being used by the current file. You can do this by using the console:

`view.settings().get('font_face')`