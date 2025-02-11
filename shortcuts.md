# macOS shortcuts.md

This is an annexe to the [macUP macOS setup process](https://github.com/artmg/macUP/) that offers: 

* Keyboard combinations
* recipe ideas for the macOS Shortcuts.app

see also:

* [misc.md](misc.md) 
	* keyboard quick combos
	* generic tips for smooth running 
	* and help for troubleshooting


## keyboard shortcuts

These 'kbd combo tips' were correct in High Sierra (version 10.13)

https://support.apple.com/en-gb/HT201236

Legend: 

	⌘ = cmd ≍ Win  
	⌥ = opt ≍ Alt   
	^ = Ctrl  
	⇧ = Shift


| Feature      | Shortcut                                     |
| ------------ | -------------------------------------------- |
| Symbols      | ⌃ ⌘ space	(Settings / add Technical Symbols) |
| Lock Screen  | ⌃ ⌘ Q                                        |
| Spotlight    | ⌘ space                                      |
| Filer search | ⌥ ⌘ space                                    |
| Screenshot   | ⇧ ⌘ 3                                        |
| Crop shot    | ⇧ ⌘ 4                                        |

See also [Mac startup key combinations](https://support.apple.com/en-gb/HT201255)

### Selection

Feature | Shortcut
--- | ---
Multiple select | ⌘ click


### App Windows

| Shortcut       | Feature                                                            |
| -------------- | ------------------------------------------------------------------ |
| ⌘ ⇥            | Switch apps                                                        |
| ⌃ ← or →       | Switch ‘desktops’ (maximised apps)                                 |
| ⌘ `            | Switch instances of app (only if neither minimised nor maximised)  |
| F11            | Show Desktop (Exposé)                                              |
| ?              | http://www.virtualbox.org/manual/ch08.html#vboxmanage-guestcontrol |
| ⌘  ⌥  ⎋  (Esc) | Force Quit Applications                                            |
| ⌃ space        | for Spotlight to search "Activity Monitor"                         |

If you really want to make a shortcut directly into Activity Monitor then see https://appleinsider.com/articles/18/03/14/how-to-create-keyboard-shortcuts-to-launch-apps-in-macos-using-automator

### in many apps

* ⌘ , 		open current application's Preferences or Settings dialog


### in Finder

* Enter	Rename the selected file
* ⌘ ⌥ C	Copy file path as UNC
* ⌘ ⇧ . 	Show / Hide hidden items
* 
* ⌘ ⇧ C	Computer folder
* ⌘ ⇧ H	Home folder

* ⌘ ⇧ G	Go to Folder
	* this will also work when you are in an app that uses the File Open dialog
	* It will allow you to paste in a path, from the clipboard, to go straight to that location

## touchpad

Multi-touch gestures https://support.apple.com/en-gb/HT204895
Force click examples https://support.apple.com/en-gb/HT204352


## Characters and accents

An easy way that usually works is to hold the key for a second and release it before it starts repeating.

Option Key gives the others:

* \` grave
* e acute
* i  circumflex
* n tilde
* u umlaut

then type the next letter

For other common characters use Option and the letter (o ø - a å etc)

See the table at https://forlang.wsu.edu/help-pages/help-pages-keyboards-os-x/


## suggested tweaks

apple dictate - for TTS, change the key cmd to option+tilde


## Shortcuts in Terminal

⌃ space (for Spotlight) then   terminal  

https://support.apple.com/en-lamr/guide/terminal/keyboard-shortcuts-trmlshtcts

## Shortcuts in LibreOffice

https://help.libreoffice.org/Writer/Shortcut_Keys_for_Writer

Generally CTRL becomes ⌘

* Headings
	* ⌘ 1  Heading 1
	* ⌘ 2  Heading 2 etc
* 

## Shortcuts in Chrome

* Autofill
	* ⇧ fn ⌫  Shift Fn Backsp  remove hightlighted autofill entry
* .

## Using the macOS Shortcuts app

### System Preferences

on iOS devices there is a `prefs:root=Devices&path=` etc URL scheme that allows you quickly open the System Preferences app in a particular place. This is not (yet?) present in macOS so you have alternatives.

* You can use `x-apple.systempreferences` scheme
	* this has been deprecated since 10.13, but still works for some settings sections
	* e.g. `open x-apple.systempreferences:com.apple.Desktop-Settings.extension`
	* for a list see https://github.com/bvanpeski/SystemPreferences/blob/main/macos_preferencepanes-Ventura.md
* Or you can use AppleScript

```perl
tell application "System Preferences"
    reveal anchor "input" of ¬
        pane id "com.apple.Sound-Settings.extension"
    activate
end tell
```

* Notes on using AppleScript for System Settings shortcuts
	* See the list of panes in https://developer.apple.com/documentation/devicemanagement/systempreferences
	* see [SuperUser](https://superuser.com/a/1642983) for a more robust and varied script that actually tweaks settings for you
	* a more extensive [reddit Guide to System Settings with System Events automation templates](https://www.reddit.com/r/applescript/comments/118ivys/updateguide_macos_ventura_system_settings_with/)
	* to identify anchors in the pane use `Script Editor` to run the following whilst the pane is open:
		* ` tell application "System Settings" to get anchors of current pane `
		* credit https://stackoverflow.com/q/24701362#comment38330235_24703872
	* 

