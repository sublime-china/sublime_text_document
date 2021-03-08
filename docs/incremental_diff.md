# Incremental Diff

Added in:3.2

Sublime Text includes a built-in diff functionality that tracks changes to files being edited. The diff calculation is incremental, tracking each buffer modification as it is performed. It does not require the file be stored in a version control system, such as Git.

The exact location of each addition, modification and deletion is tracked. This is used to display markers in the gutter, allow navigation between each change, display inline diffs and allow for reverting changes.

Diffs are calculated against the version of the file on disk, but[Git repositories](git_integration.html#diff_markers)can be configured to diff against HEAD, and the API allows files to be diffed against any content.

*   [Diff Markers](incremental_diff.html#diff_markers)
*   [Navigation](incremental_diff.html#navigation)
*   [Inline Diffs](incremental_diff.html#inline_diffs)
*   [Reverting](incremental_diff.html#reverting)
*   [Settings](incremental_diff.html#settings)

## Diff Markers

The following is an example of diff markers displayed when using the Mariana color scheme:

2728A line that was added2930A modified line31followed by another modified line3233The line before this was deleted34

See the[color schemes documentation](color_schemes.html#global_settings-diff)for information on customizing the colors and width of the diff markers.

## Navigation

Users can jump to the next or previous modification using the following methods:

*   **Ctrl***+***.**
*   **Ctrl***+***,**
*   Goto![▶](images/right.svg)Next Modification
*   Goto![▶](images/right.svg)Previous Modification

*The keyboard shortcuts tend to be a very natural way to jump around a file being edited.*

## Inline Diffs

In addition to tracking which lines have been modified, the incremental diff also tracks the exact changes. This allows displaying the original version of the text.

### TOGGLING

When right-clicking on a modified region of a file, a menu entryShow Diff Hunkwill be available. This menu item will display the previous content inline beneath the current content. Right-clicking again will show a menu itemHide Diff Hunkto hide the inline diff.

Toggling an inline diff may be performed via theEdit![▶](images/right.svg)Text![▶](images/right.svg)Toggle Hunk Diffmenu.

In addition to menu-based activation, diffs may also be toggled via keyboard shortcut:

*   Windows/Linux:**Ctrl***+***K**,**Ctrl***+***/**
*   Mac:**⌘***+***K**,**⌘***+***/**

To toggle the diff for a region, while hiding all other diffs, press:

*   Windows/Linux:**Ctrl***+***K**,**Ctrl***+***;**
*   Mac:**⌘***+***K**,**⌘***+***;**

### STYLING

The styles used for displaying inline diffs are automatically generated for color schemes that have not created their own rules. For custom styles, add rules with the following selectors:

*   `diff.deleted`
*   `diff.deleted.char`
*   `diff.inserted`
*   `diff.inserted.char`

Generally each rule will set the`background`and`foreground_adjust`properties.

## Reverting

A modification may be reverted to the original text by the keyboard shortcut:

*   Windows/Linux:**Ctrl***+***K**,**Ctrl***+***Z**
*   Mac:**⌘***+***K**,**⌘***+***Z**

Alternatively, the menuEdit![▶](images/right.svg)Text![▶](images/right.svg)Revert Modificationmay be used.

## Settings

mini\_diffboolean, string

If the incremental diff functionality should be enabled. Valid values include:

*   `true`– always enable incremental diff
*   `"auto"`– enable incremental diff for files in a Git repository
*   `false`– disable incremental diff

Default:`true`

git\_diff\_targetstring

Controls the behavior of incremental diff for files in a Git repository. Valid values include:

*   `"index"`– diff against the Git index
*   `"head"`– diff against the file at HEAD

Default:`"index"`