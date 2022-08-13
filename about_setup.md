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
you will need to start again on a fresh machine.

For an index of this repo,
which also provides an overview of the steps involved, please see [README.md](https://github.com/artmg/macUP/blob/main/README.md). _NB: This file is called `'about' setup.md` as it's the starting point, so we wanted it to be alphabetically first._


# Setup

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

Consider copying config files from well-known locations:

* Lib / Prefs
* .config / .local / .*
* ~/Library/Services
	* custom automations

```
# check size vs value of home dotfolders you might want to copy
du -hd 0 ~/.*
```

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



# Configuration

## Security

### Disk protection

Encrypt Startup disk: System Prefs / Security and Privacy / FileVault / Turn on / Display Recovery Key - store in safe place
Show hard drives: Finder / Preferences (from top menu or ⌘-,) / General / Show / Check Hard Disks 
See also Service.IN encryption
 
### Firewall

Turn on: System Prefs / Security and Privacy / Firewall / Turn On 

… leave default options for now

Turn OFF: System Prefs / Sharing

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


