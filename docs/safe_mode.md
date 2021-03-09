# 

[DOCUMENTATION](index)[TOC](safe_mode#toc)[TOP](safe_mode#)

Safe Mode

Added in:4.0

Sublime Text includes an execution mode called*safe mode*that disables all customizations. This can be used when attempting to diagnose problems, as it allows easily running the editor in a pristine state, while leaving all preferences and third-party packages unchanged.

*Safe mode is available starting in version 4.0. To accomplish a similar result in earlier versions, please see[Reverting to a Freshly Installed State](revert).*

*   [Behavior](safe_mode#behavior)
*   [Starting](safe_mode#starting)

## Behavior

When starting Sublime Text in safe mode, the application uses an alternate data directory. The data directory is where all preferences, custom key bindings, session data and third-party packages are stored. To ensure that safe mode is not influenced by previous sessions, the safe mode data directory is fully erased when starting.***Do not store any important files or customizations within the safe mode data directory.***

Safe mode can only be entered if the application has been fully closed. If any windows remain open (or on Mac the application itself), then safe mode will not work.

*   **Windows:**%AppData%\\Sublime Text (Safe Mode)\\
*   **Mac:**~/Library/Application Support/Sublime Text (Safe Mode)/
*   **Linux:**~/.config/sublime-text-safe-mode/

To prevent having to re-install any license key each time Sublime Text starts, safe mode will copy the license key from the normal data directory.

## Starting

Sublime Text can be started in safe mode via the[command line interface](command_line):

~~~
subl --safe-mode

~~~

Additionally on Windows and Mac, holding a modifier key while starting the application will open it in safe mode:

*   **Windows:****Shift***+***Alt**
*   **Mac:****Option**