# 
[DOCUMENTATION](index)[TOC](git_integration#toc)[TOP](git_integration#)

Git Integration

Added in:3.2

Sublime Text's Git integration includes the following components:

*   [Side Bar](git_integration#side_bar)
*   [Status Bar](git_integration#status_bar)
*   [Diff Markers](git_integration#diff_markers)
*   [Sublime Merge Integration](git_integration#sublime_merge)
*   [Settings](git_integration#settings)

*Please note: the following documentation discusses the implementation of the Git integration as seen with the Default and Adaptive themes that are included with Sublime Text. Via the theme engine, it is possible for third-party themes to change the visual presentation of information, in which case the following documentation may not be accurate.*

## Side Bar

Files and folders displayed in the side bar will include a status badge along the right-hand edge, when modified. This includes files and folder in the*Folders*section of the side bar, along with files in the*Open Files*section. Ignored files and folders are de-emphasized in the side bar by reducing the opacity of the name.

When the mouse pauses over a status badge, a tool tip will be displayed indicating the status of the file, or in the case of a folder, the status of the contained files and folders.

### STATUS BADGE KEY

The following table indicates the meaning of each badge.*Please note that the color of the badges will be slightly different as they adapt to the closest hue in the active color scheme.*

*   Untracked
*   Modified
*   Missing
*   Staged Addition
*   Staged Modification
*   Staged Deletion
*   Unmerged

When a folder contains files with multiple statuses, the badge most toward the end of the above list will override all others.

## Status Bar

When the focussed file us containing within the working directory of a Git repository, the status bar will contain the name of the current branch, along with the number of files that are untracked, modified, staged or unmerged. The status bar element will look like:

master3

## Diff Markers

Sublime Text's[incremental diff](incremental_diff)functionality ties in with the Git integration. By default, the incremental diff functionality tracks changes to the file since it was last saved, but it is also possible to diff against HEAD.

Here is an example of what the diff markers look like in action, using the Mariana color scheme:

2728A line that was added2930A modified line31followed by another modified line3233The line before this was deleted34

Changing the settinggit\_diff\_targetto`"head"`will modify the diff markers to display a diff versus the version of the file at the Git repository HEAD, as opposed to the version of the file in the working directory.

See the[incremental diff documentation](incremental_diff)for more information and examples, including instructions for viewing inline diffs, navigating between hunks and reverting changes.

## Sublime Merge Integration

The Git features available in Sublime Text were derived from work that went into our other product,[Sublime Merge](https://www.sublimemerge.com/). Sublime Merge is a full-featured, blazing-fast Git client built upon the technologies from Sublime Text.

Since editing source code and prose requires different tools and workflows than managing a Git repository, we opted to integrate the most appropriate Git functionality into Sublime Text, but leave more advanced features in Sublime Merge. The following integration points make it easy to jump into the appropriate Git context:

#### EDITOR CONTEXT MENU

*   Open Git Repository…
*   File History…
*   Line History…
*   Blame File…

#### SIDE BAR FOLDER CONTEXT MENU

*   Open Git Repository…
*   Folder History…

#### SIDE BAR FILE CONTEXT MENU

*   Open Git Repository…
*   File History…
*   Folder History…
*   Blame File…

#### COMMAND PALETTE

*   Sublime Merge: Open Repository
*   Sublime Merge: Folder History
*   Sublime Merge: File History
*   Sublime Merge: Blame File

## Settings

show\_git\_statusboolean

Enables Git integration

Default:`true`

git\_diff\_targetstring

Controls the behavior of incremental diff for files in a Git repository. Valid values include:

*   `"index"`– diff against the Git index
*   `"head"`– diff against the file at HEAD

Default:`"index"`