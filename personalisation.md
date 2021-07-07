# macOS personalisation

This sets out the general intefrace, including luanchpad, finder with actions, and spotlight search. 

It is part of the [macUP macOS setup process](https://github.com/artmg/macUP/) – a public repository 
used to install and configure a macOS device in an efficient, uniform and comprehensive way. Any sensitive or personal settings should be kept separately in your own *Private Configuration Notes*.


## Rename

* SysPrefs / Sharing / Computer Name:
    * dynamic: uncheck

If you later get issues with the computer automatically renaming itself see [macUP / network.md](https://github.com/artmg/macUP/blob/main/network.md)


## Launchpad cleanup

Launchpad is the Rocket Icon in the dock, which shows the apps:

```
# Decrease Launchpad Icons Size (by setting 8x6 grid)
defaults write com.apple.dock springboard-rows -int 6
defaults write com.apple.dock springboard-columns -int 8;killall Dock
# credit https://www.webnots.com/how-to-increase-or-decrease-launchpad-icons-size-in-macos/

# Allow downloaded apps to run without going into SysPrefs
defaults write com.apple.LaunchServices LSQuarantine -bool false

# Do not reopen all apps
defaults write com.apple.systempreferences NSQuitAlwaysKeepsWindows -bool false

```

### Clean up base apps:

* On Dock / right-click Options / Remove from Dock
	* Mail, FaceTime, Messages, Maps, Photos, Contacts, Calendar, Reminders, Music, Podcasts, TV, News, 
	* ones marked ^ can go because their also pinned to Dock
* Move into Productivity folder
	* Mail, Contacts, Calendar, Reminders, FaceTime, Messages, Maps, Find My, PhotoBooth, Photos, Music, Podcasts,  Safari ^ , Finder ^ , TV, VoiceMemos, News, Stock, Books, Dictionary, Home, Siri, Mission Control, Quick Time Player, TextEdit, Chess, Stickies, 
* Open Launcher folder Other, rename to Technical and move in: 
	* AppStore, System Prefs ^ , Time Machine, Font Book, Image Capture, Automator, 

### Other general layout preferences


```
# credit https://www.taniarascia.com/setting-up-a-brand-new-mac-for-development/#defaults
# Show Library folder
chflags nohidden ~/Library

# Show hidden files
defaults write com.apple.finder AppleShowAllFiles YES
# Show path bar
defaults write com.apple.finder ShowPathbar -bool true
# Show status bar
defaults write com.apple.finder ShowStatusBar -bool true

# credit https://www.cultofmac.com/646404/secret-mac-settings/
# expand Save panel
defaults write NSGlobalDomain NSNavPanelExpandedStateForSaveMode -bool true
defaults write NSGlobalDomain NSNavPanelExpandedStateForSaveMode2 -bool true
# Disable Notification Center and remove the menu bar icon
launchctl unload -w /System/Library/LaunchAgents/com.apple.notificationcenterui.plist 2> /dev/null
# Use list view in all Finder windows by default
defaults write com.apple.finder FXPreferredViewStyle -string "Nlsv"

# To modify items in the finder sidebar, 
# consider https://github.com/robperc/FinderSidebarEditor


```
# Finder - STILL MANUAL...
	* (consider instead https://github.com/robperc/FinderSidebarEditor)
* Preferences / Sidebar
	* Remove AirDrop, Applications, Documents, iCloud Drive, 
	* Add Home (`user`), Root (`user`s MacBook) 
* Preference / Settings
	* Check Show extensions
	* Uncheck Warn when changing
* Tags / on right / Hide
* drag into Favourites:
	* Cached / Repos
	* 

```
# credit https://apple.stackexchange.com/a/337179
defaults write com.apple.systemuiserver menuExtras -array \
"/System/Library/CoreServices/Menu Extras/AirPort.menu" \
"/System/Library/CoreServices/Menu Extras/Battery.menu" \
"/System/Library/CoreServices/Menu Extras/Bluetooth.menu" \
"/System/Library/CoreServices/Menu Extras/Clock.menu" \
"/System/Library/CoreServices/Menu Extras/Displays.menu" \
"/System/Library/CoreServices/Menu Extras/Volume.menu"
killall SystemUIServer
```

* Sound issues
	* preventing autoswitch to monitor might not be straightforward
		* try https://apple.stackexchange.com/q/285311 or https://superuser.com/q/733314 
		* last resort is to plug in a dummy headphone cable



## Automations


Note: some people might prefer an alternative file manager to Finder, 
instead of adding these features manually. 
If you want a cross-platform, open source candidate try 
`brew install --cask double-commander`

Note: these will get saved as a couple of XML files in the folder ~/Library/Services/name\ I\ saved\ as.workflow/


### New File in Folder

This adds the feature you find in File Managers of many other OSes 
to Right-click to add new file in current folder

* Cmd-Space a-u-t to run Automator
* New: Quick Action
* Workflow receives current: folders
* in: Finder
* image: + Add
* Actions / Library / Utility / Run Shell Script (double-click to add)
* Shell: zsh
* Pass input: as arguments

```
#!/bin/zsh
# add a space before `New` to make it go to the top
touch "$1/ New File"
# pass the new filename to the next step that highlights it
echo "$1/ New File"
```

* Actions / Library / Files & Folders / Reveal Finder Items (double-click to add)
* Save: New File in Folder


### Copy as URL

To use Finder to get a URL like file:///System/Applications/Font%20Book.app

* Cmd-Space a-u-t to run Automator
* New: Quick Action
* Workflow receives current: files or folders
* in: Finder
* image: Globe
* Actions / Library / Utility / Copy To Clipboard (double-click to add)
* Actions / Library / Utility / Run Shell Script (double-click to add)
* Shell: zsh
* Pass input: as arguments

```
#!/bin/zsh
sed -e 's/:/\//g' -e 's/\ /%20/g' -e 's/^/file:\/\//g' | pbcopy
# credit http://hints.macworld.com/article.php?story=20091122183451908
```

* Save: Copy as URL



## Search indexing

to prevent an external disk indexing see
[macOS misc.md#prevent-external-disk-indexing]

### allow markdown indexing

By default MarkDown .md documents are not included in the spotlight indexes. 
Here is a simple fix to include them. Note that MD in mdimporter means MetaData. 

```
# credit https://stackoverflow.com/a/33873422
cp -r /System/Library/Spotlight/RichText.mdimporter .

vi RichText.mdimporter/Contents/Info.plist
# insert a new line, four tabs, then the string ...
# <string>net.daringfireball.markdown</string>

sudo cp -R RichText.mdimporter /Library/Spotlight
mdimport -r /Library/Spotlight/RichText.mdimporter
```


