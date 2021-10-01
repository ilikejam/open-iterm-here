# Open iTerm here
Modified from [Deep-blue](https://hohonuuli.blogspot.com/2016/02/iterm2-version-3-open-iterm-here-script.html)'s version. I don't use tabs in shells, so this version always opens a new iTerm window.

Open ScriptEditor from Launchpad and paste this in:
```
(*
    Open Terminal Here
    A toolbar script for Mac OS X 10.3/10.4
    Written by Brian Schlining
    Modified by David Stark
 *)

property debug : false

-- when the toolbar script icon is clicked
--
on run
    tell application "Finder"
        try
            set this_folder to (the target of the front window) as alias
        on error
            set this_folder to startup disk
        end try
        my process_item(this_folder)
    end tell
end run

-- This handler processes folders dropped onto the toolbar script icon
--
on open these_items
    repeat with i from 1 to the count of these_items
        set this_item to item i of these_items
        my process_item(this_item)
    end repeat
end open

-- this subroutine processes does the actual work
-- this version can handle this weirdo case: a folder named "te'st"ö te%s`t"

on process_item(this_item)
    set thePath to quoted form of POSIX path of this_item
    set theCmd to "cd " & thePath & ";clear;"
    if application "iTerm" is running then
        tell application "iTerm"
            activate
            set aWindow to (create window with default profile)
            tell current session of aWindow
                write text "cd " & thePath & ";clear;"
            end tell
        end tell
    else
        tell application "iTerm"
            activate
            set aWindow to current window
            tell current session of aWindow
                write text "cd " & thePath & ";clear;"
            end tell
        end tell
    end if
end process_item
```

Export it as an Application, then drag the application icon up to the Finder toolbar while holding down `option` and `command`.

Then you can click the icon in the Finder toolbar and iTerm will open at that folder.
