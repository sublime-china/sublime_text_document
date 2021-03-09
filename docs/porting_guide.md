# 

[DOCUMENTATION](index)[TOC](porting_guide#toc)[TOP](porting_guide#)

Plugin Porting Guide

Sublime Text 3 contains some important differences from Sublime Text 2 when it comes to plugins, and most plugins will require at least a small amount porting to work. The changes are:

*   [Python 3.3](porting_guide#python_33)
*   [Out of Process Plugins](porting_guide#out_of_process_plugins)
*   [Asynchronous Events](porting_guide#asynchronous_events)
*   [Restricted`begin_edit()`and`end_edit()`](porting_guide#restricted_edit)
*   [Zipped Packages](porting_guide#zipped_packages)
*   [Importing Modules](porting_guide#importing_modules)
*   [Restricted API Usage at Startup](porting_guide#api_at_startup)

## Python 3.3

Sublime Text 3 uses Python 3.3, while Sublime Text 2 used Python 2.6. Furthermore, on Mac, the system build of Python is no longer used, instead Sublime Text is bundled with its own version. Windows and Linux are also bundled with their own version, as they were previously.

## Out of Process Plugins

Plugins are now run in a separate process,`plugin_host`. From a plugin authors perspective, there should be no observable difference, except that a crash in the plugin host will no longer bring down the main application.

## Asynchronous Events

In Sublime Text 2, only the`set_timeout()`method was thread-safe. In Sublime Text 3, every API method is thread-safe. Furthermore, there are now asynchronous event handlers, to make writing non-blocking code easier:

*   `on_modified_async()`
*   `on_selection_modified_async()`
*   `on_pre_save_async()`
*   `on_post_save_async()`
*   `on_activated_async()`
*   `on_deactivated_async()`
*   `on_new_async()`
*   `on_load_async()`
*   `on_clone_async()`
*   `set_timeout_async()`

When writing threaded code, keep in mind that the buffer will be changing underneath you as your function runs.

## Restricted`begin_edit()`and`end_edit()`

`begin_end()`and`end_edit()`are no longer directly accessible, except in some special circumstances. The only way to get a valid instance of an[Edit](api_reference#sublime.Edit)object is to place your code in a[TextCommand](porting_guide#sublime_plugin.TextCommand)subclass. In general, most code can be refactored by placing the code between`begin_edit()`and`end_edit()`in a[TextCommand](porting_guide#sublime_plugin.TextCommand), and then running the command via`run_command()`.

This approach removes the issue of dangling[Edit](api_reference#sublime.Edit)objects, and ensures the repeat command and macros work as they should.

## Zipped Packages

Packages in Sublime Text 3 are able to be run from.sublime-package(i.e., renamed.zipfiles) files directly, in contrast to Sublime Text 2, which unzipped them prior to running.

While in most changes this should lead to no differences, it is important to keep this in mind if you are accessing files in your package.

## Importing Modules

Importing other plugins is simpler and more robust in Sublime Text 3, and can be done with a regular import statement, e.g.,`import Default.comment`will importPackages/Default/Comment.py.

## Restricted API Usage at Startup

Due to the`plugin_host`loading asynchronously, it is not possible to use the API at import time. This means that all top-level statements in your module must not call any functions from the[sublime](api_reference#sublime)module. During startup, the API is in a dormant state, and will silently ignore any requests made.