# 

[DOCUMENTATION](index)[TOC](api_environments#toc)[TOP](api_environments#)

API Environments

Version:  
[Dev](api_environments#ver-dev)[3.2](api_environments#ver-3.2)[3.1](api_environments#ver-3.1)[3.0](api_environments#ver-3.0)

Plugins in Sublime Text are Python files located in the root of a[package](packages). The following document describes the Python environment the plugins are run in.

*   [Overview](api_environments#overview)
*   [Python Version](api_environments#python_version)
    *   [Selecting the Python Version](api_environments#selecting_python_version)4050
*   [Modules](api_environments#modules)
*   [Pre-Installed Packages](api_environments#preinstalled_packages)4050

## Overview

Sublime Text runs plugins in a separate process from the main editor UI. This process runs an executable namedplugin\_host.

Running plugins in a separate process ensures the entire editor will not crash due to a poorly written plugin. If a plugin does cause theplugin\_hostto crash, a user may still save their work before re-starting Sublime Text.

All plugins are run in a singleplugin\_hostprocess, and share a single Python environment. Each plugin is loaded as a sub-module of a module named after the package. For example, a plugin in the fileMyPackage/my\_plugin.pywill be loaded as the Python module`MyPackage.my_plugin`.

Theplugin\_hostprocess contains an embedded version of the Python programming language, and exposes an[API](api_reference)to plugins. Theplugin\_hostexecutable always uses its own embedded version of Python, even if the end-user has Python installed on their machine.

## Python Version

By default, all plugins are run using Python 3.3.6.*Sublime Text‘s build of Python 3.3.6 includes a handful of patches backported from Python 3.4 to fix issues with unicode paths and crashes with the`ctypes`module on 64bit versions of Windows.*

Starting in build 4050, plugins may also be run using Python 3.8. Python 3.8 features many improvements to the language, better performance and continued support and bug fixes from the Python Software Foundtion.4050

### SELECTING THE PYTHON VERSION4050

To provide for backward compatibility, Sublime Text 4050 will continue to run all plugins using Python 3.3.

Any package that wishes to use Python 3.8 must create a file named.python-versionin the root of the packages. This file should contain either the text`3.3`or`3.8`to select the version of Python to use.*If a file named.python-versionis not present, or it contains any value other than`3.8`, then Python 3.3 will be used.*

All plugins in a package will use the same version of Python.*Any package with a.python-versionfile containing`3.8`loaded in older builds of Sublime Text will try to run the plugins using Python 3.3.*

## Modules

The Python environment withinplugin\_hostcontains all of the modules in[The Python Standard Library](https://docs.python.org/3.3/library/), except for:

*   audioop
*   cryptLINUX
*   curses
*   cProfileLINUX
*   fpectl
*   readline

*   lzma
*   msilib
*   nis
*   ossaudiodev
*   resource
*   spwd

*   syslog
*   test
*   tkinter
*   turtle
*   wave

## Pre-Installed Packages4050

The following the packages are pre-installed in both the Python 3.3 and 3.8 environments:

*   [certifi](https://pypi.org/project/certifi/): A collection of SSL root certs for use with urllib