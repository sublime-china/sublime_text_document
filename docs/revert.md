# 

[DOCUMENTATION](index)

Reverting to a Freshly Installed State

Version:  
[Dev](revert#ver-dev)[3.2](revert#ver-3.2)[3.1](revert#ver-3.1)[3.0](revert#ver-3.0)

Sublime Text can be reverted to a freshly installed state by removing your data directory. Depending on your operating system, this directory is located in:

*   **Windows:**%APPDATA%\\Sublime Text 3
*   **Mac:**~/Library/Application Support/Sublime Text 3
*   **Linux:**~/.config/sublime-text-3

To revert to a freshly installed state, you can:

1.  Exit Sublime Text
2.  Move the data directory to a backup location
3.  Start Sublime Text

When restarted, a fresh data directory will be created, just as it was the first time you ran Sublime Text. Keep in mind that this will also remove all of your settings and packages. The backup copy of your data directory can be used to retrieve configuration, or custom packages that can not be reinstalled.

### WINDOWS

In Windows, cache files are stored in a separate location,%LOCALAPPDATA%\\Sublime Text 3, to improve performance with roaming profiles.

### MAC

On Mac, the~/Librarydirectory is hidden by default. To navigate there, select theGo![▶](images/right.svg)Go to Foldermenu item in Finder, and type in~/Library.