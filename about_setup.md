# macOS setup

This is about getting the installation process started, 
including important stuff you want to consider before a reinstall.

It is part of the [macUP macOS setup process](https://github.com/artmg/macUP/) – a public repository 
used to install and configure a macOS device in an efficient, uniform and comprehensive way. Any sensitive or personal settings should be kept separately in your own *Private Configuration Notes*, for details you would not want to share publicly. 

This article looks at ways you should 
inspect any old setup you may have, to 
ensure these private notes contain all 
the relevant application configuration, 
accounts used and data locations 
you will need to start again on a fresh (or freshly rebuilt) machine.

For an index of this repo,
which also provides an overview of the steps involved, please see [README.md](https://github.com/artmg/macUP/blob/main/README.md). _NB: This file is called `'about' setup.md` as it's the starting point, so we wanted it to be alphabetically first._


# Setup

This presumes that you will NOT be using any macOS install-time technique to bring forward your old data, apps and settings, such as recovering them from Time Machine. This presumption is based on the idea that you want a mostly fresh start on the new (-ly rebuilt) machine. 

## full backup

Before doing any work on reinstallation you need:

* a complete backup of any data
	* this could be a time machine backup
	* or it could be some other backup 
	* or simply a full copy of your master data
* Seriously, **do this now** before you even think about carrying on
* Do you have **more than one copy** in-case the media is corrupt?
* list of applications
	* will you remember which apps you will want to reinstall?
	* check using `brew bundle dump` to create a new Brewfile based on what you have asked to be installed
		* compare this to [application_installation](application_installation.md)
* 

### Structured data

#### Configuration data backup

If you have a Private Configuration Note for your own data locations, check this as part of your pre-install backup

* Accounts used on apps and data sync
* Repositories cloned

Consider copying config files from well-known locations – you might not want to recover all of these, but you want to avoid loosing the useful detail in case:

* should we copy the generic User Preferences folder?
	* Lib / Prefs
	* there are Megabytes of stuff in there, most of no value
* take a copy of some dotfolders (~/.*)
* e.g. SAVE
	* local config anydesk gitconfig
	* * .config / .local / .*
	* .ssh
	* * ~/Library/Services
	* custom automations
* e.g. DON'T save
	* Trash cache dropbox npm

```zsh
# check size vs value of home dotfolders you might want to copy
du -hd 0 ~/.*
```

##### Dot Config Backup

Here is an example of typical config locations identified using steps above, to capture your config states for later reinstallation or review...

```zsh
# escaping the ! works in bash interactive shell, 
# but from zsh you **MUST** start a bash shell
cd $HOME
7z a -t7z "DotConfigBackup.Partial.$USER-$(hostname -s).`date +%y%m%d`.7z" \
  .zshenv .gitconfig .anydesk/*.conf \
  .ssh -x\!.ssh/*/ \
  .config/configstore .config/git \
  .config/zsh/ -xr0\!.zcompdump* \
  Library/Services \
  Library/Application\ Support/obsidian/obsidian.json 

# note the syntax to take .ssh files but no subfolders!
# Library/Preferences would be overkill for most cases
```

For git repository config backup see https://github.com/artmg/lubuild/blob/master/help/use/git-source-control.md#config-backup

## Other Procedures

### Clean install

This will wipe you whole identity and any data 
from your device, to render it like new. 
You would normally ONLY want to do this AFTER your backup 
and before you give the device away. 
See help - https://support.apple.com/en-au/HT201065

* Signout and Deauthorise
* Clear NVRAM (start with Opt-Cmd-R-P)
	* appears to boot normally
	* but it will ask for keyboard layout
* Use macOS recovery to erase disk and reinstall
	* Cmd-R during power up 
		* Option-R to reinstall latest version of macOS
	* Erase Disk (accept suggested format)
		* help - https://support.apple.com/en-au/HT208496
	* Reinstall MacOS
		* help - https://support.apple.com/en-au/HT204904
* If you want to allow someone else to do set up
	* Quit reinstall with Cmd-Q
	* reclear NVRAM
	* leave for them to do the install

### Base install

* Language
* Country
* Confirm languages - Continue
* Accessibility or Not Now
* Data & Privacy
* Migration Assistant or Not Now
* Sign in with Apple iCloud or Set up later / Skip
* Terms / Agree
* Create computer account
* Enable location services (optional, or Don't Use)
* Time Zone
* Analytics and crash sharing (both optional)
* Screen time or Set up later
* Siri - enable or uncheck
* Look - light, dark or auto

# Configuration

## Security

### Disk protection

You may have some personal notes on setting up with Disk Encryption that you wish to refer to. You might also consider leaving this step until later, when you have any private data store available, so that you can safely store the recovery key.

* Encrypt Startup disk: 
	* System Prefs / Security and Privacy / FileVault / Turn on /
	* Display Recovery Key - store in safe place
* Show hard drives: 
	* Finder / Preferences (from top menu or ⌘-,) / 
	* General / Show / Check Hard Disks 
 
### Firewall

* Turn the firewall on: 
* System Prefs / Network / Firewall
	* in earlier macOS versions was System Prefs / Security and Privacy / Firewall
* Turn On 
	* … leave default options for now

* Turn OFF Sharing
* System Prefs / (General /) Sharing

Set Computer Name as per Devices and Configs

## Devices 

### Printer

If you have a printer with extra options (like paper trays) that don’t show up with AirPrint, 
Then add a new printer with the vendor’s own driver…

* System Preferences / Printers and Scanners 
* Under the list of printers click the [ + ] button
* Select your device, then click Use: 
* Choose. Select Software ….  And scroll down to find the vendor printer driver
* Now you should be allowed to choose any installed options for your device

Refer to your private configuration notes for details


