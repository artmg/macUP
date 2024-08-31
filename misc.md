# macOS miscellaneous tips and help

This is an annexe to the [macUP macOS setup process](https://github.com/artmg/macUP/) that offers: 

* generic tips for smooth running 
* and help for troubleshooting

see also:

* [shortcuts.md](shortcuts.md) 
	* keyboard quick combos
* [iOS_Apple_iPad_iPhone_reset.md](iOS_Apple_iPad_iPhone_reset.md)
	* how to wipe iOS devices (often need a Mac connected!)
* [disks_and_data](disks_and_data.md)
	* useful tips on storing and arringing your information on disk storage drives


You may also want to record details of 
specific cases or issues that occur, 
along with any *Private Configuration Notes* you hold.


## why … ?

A highly abridged history lesson on macOS… 

* Originally derived from BSD
* A lot of the UI components came forward from the NeXTSTEP platform, and you'll still see NSxxxx under the covers (NSDockTile, NSServices, etc)
* Darwin is the open-source OS core, but other key components that make up macOS are closed
    * Like Cocoa OO-API and Aqua GUI
    * Darwin is also the root of iOS, watchOS etc
* 

## Reinstall

Before reinstalling (especially if you are transferring ownership) you might like to 

* Turn off Find My Mac (as well as disassociate your AppleID) 
* Consider erasing the NVRAM
	* There could be a firmware password to unlock this https://support.apple.com/en-gb/HT204455
	* This _might_ also disable Find My Mac
	* see https://support.apple.com/en-gb/HT204063
* Erase the Mac HD (System) and Data volumes https://support.apple.com/en-gb/HT208496

Your options now include:

* Use Command-R at power on to boot into MacOS recovery.
	* This will reinstall with the most recently installed version. 
* Option-Command-R will allow you to use Internet Recovery
	* which will try to install the latest version. 
* See [How to reinstall macOS from macOS Recovery](https://support.apple.com/en-gb/HT204904) or [About macOS Recovery](https://support.apple.com/en-gb/HT201314)
* 

So a TOTAL WIPE, where you will DESTROY ALL DATA, and reinstall of a device might be

* remove FindMyMac and Sign Out of Applle ID
* Shutdown
* Power and and Hold Opt-Cmd-R-P
* Wait 20 secs for second light up of display then release
* Shutdown
* Power on and Hold Opt-Cmd-R
* Enter your wifi PSK
* Wait a few minutes for latest installer
* Language
* when you see macOS Utilities
	* Disk Utility
	* select Macintosh HD
	* Erase
	* Accept proposed filesystem
		* as its likely to be the most suited to your circumtance
		* if you wish you can choose `with Encryption` now instead of waiting to turn FileVault on later
	* Repeat for any other disks present
	* Quit Disk Utility
* Reinstall macOS
* Choose Macintosh HD and continue
* Leave it running until the Welcome Screen
    * with the annoying voice where you should NOT press Esc 
* If you want to put the installation on hold (e.g. to pass it over to its new owner)
    * Press Cmd-Q to Shutdown cleanly 



### Bootable installer

If you have no recovery partition left on your machine, 
and the internet is not available, then you will need:

* a bootable installer
	* recommended as a 8GB or larger USB (-C?) flash drive

```
# find the exact command to use
ls /Applications/Install\ macOS*/Contents/Resources/cre*

# now append the options
#   --volume /Volumes/uXX_MediaName
```

* may need to enter your password
* no need to install xcode if prompted
* Use Option ⌥ at boot to select boot device




## Disable lid sleep

If you want your Mac to keep running when you put down the lid, you could plug in a display, 
install a special piece of software like caffeine, 
or simply use the built in command:

```
# Display global power settings:
pmset -g

# To stop sleep entirely:
sudo pmset -a disablesleep 1

# To revert, allowing sleep again:
sudo pmset -a disablesleep 0

# credit https://apple.stackexchange.com/a/361398
```

Remember to allow sleep again before you unplug and 
put your mac into a backpack, 
else it might get rather toasty!


## Firefox

### Already running

```
# Make sure it’s not still running
ps -a | grep -i firefox

# remove semaphore
cd ~/Library/Application\ Support/Firefox/Profiles/
$ ls -l
# if you have multiple profiles, list the one with the most recent date 
$ cd rAnd0m.default
$ ls -la
# remove the hidden parentlock file 
$ rm -v .parentlock

# credit https://apple.stackexchange.com/a/238169



```


## iCloud 

Note that this article is primarily written from a viewpoint that uses a Mac as the only Apple device, and therefore eschews the majority of the 'Apple-for-everything' ecosystem. If you have the whole range of Apple products, tied into a single AppleID, some of this might seem pointless to your approach to computing. 

### Key transaction settings

There are some things where you're very limited as to how you control payments and subscription, almost obliged to dedicate a macOS or iOS device to an AppleID in order to do these...

* Change **iCloud Storage** subscription level
	* can do on macOS, iOS, or iCloud for Windows
	* can't do via browser
	* see https://support.apple.com/en-gb/HT201318
* Change **payment methods**
	* using macOS or iOS https://support.apple.com/en-gb/HT201266
	* using Apple Apps: **iTunes** for Windows or  **Apple Music** for Chromebook/Android https://support.apple.com/en-gb/HT210755
		* I couldn't manage via tv.apple.com in browser
* Other things you can't change without a payment method set up
	* https://support.apple.com/en-gb/HT203905

See the section [Cached Keychain](#cached-keychain) (consider renaming) below to 'home' multiple AppleIDs on a single device.

### Drive folder choices

If you use the default iCloud Drive settings, 
it will back up the following folders:

* Documents
* Desktop 
* Downloads 

* To change this, choose System Preferences / AppleID / iCloud
* Next to iCloud Drive click Options
* you can deselect Desktop and Documents Folders
	* but as at April 2021 you cannot yet deselect a specific folder (e.g. Downloads)

### Managing Shared Albums

Even if you have not set Photos to sync from iCloud to your device
(Settings and Preferences / AppleID / iCloud / left Photos unticked) 
then you can still allow the Photos client app to control
Shared Albums.
* Photos / Preferences / iCloud / tick Shared Albums
* this will place a tick in the main AppleID / Photos line
	* but selecting it in Photos stops it from also selecting the options: iCloud Photos and Photo Stream

### Keychain

The macOS keychain is where passwords are stored, for Safari browser and other apps. You can access its contents through, the following - the first two are essentially the same management interface, which also supports Two Factor Authentication (2FA) and the third is a more technical way to view and edit contents.

* Safari / Preferences / Passwords
* Preferences / System / Passwords
* Keychain Access.app in the launchpad Utilities folder

The latter shows you, and allows you to search within, the individual encrypted keychain files such as:

* iCloud - synced between your devices
* System - wifi networks and system roots
* login - automatically unlocked by user logon

Items within the keychains include types:

* passwords - also synced to iOS devices
* Secure Notes - macos only manual entries
* Certificates and Keys - for asynchronously encrypted services

Individual iCloud items can be shared from iOS via AirDrop. For instance if you wish to share a single set of login credentials use Preferences / Passwords / Share / AirDrop. If however you wish to share a copy of the entire keychain it is usually more complex. 

#### Cached keychain

(consider renaming section) 

As a simpler workaround, consider adding a new macOS user, and signing into AppleID. That way you get to choose which iCloud feature data is cached into the local folders, including of course the Keychain. The keychain _should_ be encrypted using the principle device passcode for that account, but do consider ensuring the using FileVault full disk encryption on the drive holding the cache. 

Holding a device signed into that account also confers the other privileges, such as allowing other sign-ins to be trusted, so protect the device and local account well. 

For more keep reading.

#### Create a standard user

* System Settings / Users & Groups
* Add User (then you may need to authenticate yourself)
* leave it set as standard user
* set the account name and password
* Log on as the user
* set Accessibility and Privacy as you prefer
* do NOT use AppleID just yet, Skip For Now
* Skip through most of the rest
* then set just the basic services below.

##### AppleID basics only

If you only want to use your AppleID to enable basic services like App Store, manage your iCloud subscription, or Find My Mac:

* System Settings / Sign in with your AppleID / iCloud / Start using iCloud
	* enter your AppleID and password
	* use a Trusted Device to get your user verification code, if requested
	* Accept the Terms of Service
	* you will probably have to enter your local user password too
		* so that it may also unlock your iCloud keychains
		* If you have changed your local password 
			* you may need to enter the old password, 
			* as it unlocked your iCloud keychain
* You will want to be quite quick to disable the rest
	* before they all sync down to your device
* System Settings / AppleID / iCloud / Apps using iCloud
	* UNcheck ALL iCloud options (including ‘Show More Apps’) except:
	* Keychain
	* (_optionally_) FindMy
		* you will also need to enable Location Services if you want to locate as well as be able to wipe
			* Enable Location Services
				* you may leave most apps turned off
				* after System Services, click details
					* UNcheck all except Find My Mac and Networking & Wireless
* unless there are other specific services you wish to retain on this device


Review the devices that are now tied to this account, and take especial note of those that are Apple devices. Check the details against this newly set up device, to ensure it mentions `This device is trusted and can receive AppleID verification codes`.

You may, if you wish, also set up Safari on this user account to have access to iCloud via the web, but you can do that anytime you need it.


##### Trusted Devices

When signing in with an AppleID account that has two-factor authentication (2FA, MFA) enabled, you will also need a six-digit verification code. As explained in [https://support.apple.com/en-ca/HT204974](https://support.apple.com/en-ca/HT204974) the limitation is that Trusted Devices can only be items running Apple 'OS' variants, like Macs, iPads etc. 

This is another reason why you might need to keep extra local users authenticated with any 'legacy' AppleIDs that still hold some data or services. As you decommission legacy devices, ensure that you are tracking which are still Trusted Devices for the relevant six-digit user verification codes.


## Images

### Photos app from Apple

The Photos app, introduced on OS X Yosemite 10.10, replaced iPhoto and Aperture apps, that have not been supported since macOS Mojave 10.14. On iOS devices, the similarly branded Photos app replaced the Camera Roll app/

The [Memories feature](https://support.apple.com/en-gb/guide/photos/pht96259d626/) (formerly 'Moment') on the Photos app can be used to categorise by metadata

The Photos app, and other related app components like the Media Browser, rely on the [System Photo Library being configured](https://support.apple.com/en-gb/HT204414) e.g. Photos / Preferences / General. 

By default, in your user folder, the Pictures / Photos Library contains your local library for the Photos app. You can export the entire thing by copying the folder using Finder. If you only want to export a portion: within the Photos app, select the photos/albums you want then File / Export / choose destination / Export Originals.


#### Managing Photo Libraies

Before you start making changes to Photos Libraries and their locations locally, see below to turn off iCloud Photo moving Libraries. 

If you launch Photos app whilst holding Option button, you will be prompted for a new Photos Library location. Note that Photos Library folders on macOS have the `.photoslibrary` filename extension. The Option on startup will also allow you to create an additional local Photos Library, allowing you to have multiple locations (including external drives).  credit https://www.makeuseof.com/how-to-move-photos-library-to-external-drive-on-mac/

If you have Multiple Photo Libraries, only the one that you have nominated as the System Photo Library (Photos / Preferences / General) can connect with iCloud features.

### iCloud Photos

The **iCloud Photo Library** is the portion of your photos that are stored within the iCloud paid (if above 5GB) subscription. The photos stored on your device locally can be a subset of these, with the 'Optimise Storage' feature: Photos / Preferences / iCloud / Optimise. 

Alternatively, if you want to store all the images locally, for instance is you want to back them up, then use: Photos / Preferences / iCloud / Download Originals.

You can use Shared Albums for photo sharing, to allow other iCloud users to view photos in your iCloud Library: Photos / Preferences / iCloud / Shared Albums – this turns on photo sharing.

You can also create a private iCloud.com website for those without iCloud accounts.

The System Photo Library (nominated via Photos / Preferences / General) is the one that connects with iCloud features. This is only relevant if you have Multiple Photo Libraries, otherwise the default system library is connected automatically. 

When you import items, they will only sync with iCloud (if it is your System Photo Library and) if you check the Copy items to the Photo Library.

Disconnecting iCloud when moving a library on your local device (ref https://www.imore.com/how-use-photos-mac-external-hard-drive or https://www.macworld.co.uk/how-to/move-photos-hard-drive-3681943/ )

The processes involved in the synchronisation are `cloudd` that directs `cloudphotod` to fetch the content and `photolibraryd` that organises it on arrival.


## VirtualBox tips

moved OUT to [[virtualisation]]

## Protection

### Crash diagnostics

Check the Console in Launchpad / Other 

From Terminal run   sudo sysdiagnose   

Some people recommend independent open source diagnostic tool EtreCheck http://etrecheck.com

### XProtect

Apple's antimalware XProtectService is built into macOS, but other than File Quarentine, there is no obvious interface. It runs when supporting software (such as the major browsers) download files, and is updated with new signature definitions as long as App Store Updates allow `Install system data files and security updates`. 

When you first try to execute a downloaded file you get a warning 

    This application was downloaded from the Internet
    Are you sure you want to open it

If the XProtect matched it to a file Apple marked as malware it will
be far more direct in warning you, and it will name the offending malware. 
It is therefore more like Windows SmartScreen than Defender –  
You need third party software for features like advanced heueristic detection, periodic scans of your disk, protection from bundled adware.
Opinions differ on whether XProtect alone is enough – 
depending on how vicariously you surf, click, download, and install, 
you may or may not wish to add some extra paid protection. 


## Audio

### Audio routing driver

If you want a 'jack'-style audio router to send sounds between one app and another, look at BlackHole (which stepped in since SoundFlower was abandoned) 

### per-app audio control

If you want to diagnose which application is the 
sound source program that is currently sending audio, 
like `EarTrumpet` on Windows, 
consider the advanced volume controller app BackgroundMusic 
(`brew install --cask background-music`).

### Burn CD / DVD

to prepare the audio track see Lubuild / films-and-songs.md#normalise-peak-amplitude

* save your audio.wav as 16-bit 44,100Hz
* Open Music (iTunes)
* Create a new playlist called CD
* Drop the wav file onto the Playlist
* Menu / File / Burn Playlist to Disc
	* Audio CD
	* Burn
* Wait a couple of minutes for it to finish
* Eject the CD and test out the disc in an ordinary player

According to [this Apple Music guide](https://support.apple.com/en-gb/guide/music/mus03854c438/mac) CDs you create with Music (iTunes) _will_ play in normal CD players, but DVDs will **only** play in computers. 
If you want a non-data DVD, one that will play in a regular DVD player, then you could try one of these open-source programs available via homebrew:

* burn
    - simple, old favourite
    - not updated since 2010 (but still in use) 
* dvdstyler
    - mature, cross-platform, in c++ with wxWidgets
    - last mac build 2019, although X+Win updated 2021
    - includes menu creation features

You can use VLC to find the temporary download URL if your media originates from YouTube, Vimeo, etc. See https://github.com/artmg/lubuild/blob/master/help/manipulate/films-and-songs.md#online-video-caching
DVD uses 576i resolution, so it's not worth going for
higher than 720p downloads. 

The video be automatically converted by the burning program to use the MPEG-2 encoding at the correct resolution and framerate for the DVD standard, 
put you might need to choose PAL format for Europe. 


## Troubleshooting

### Attached USB devices

If you want to know what accessories have been plugged in via USB 

* `ioreg -p IOUSB -w0`
	* add ` -l` to see loads of detail
* `system_profiler SPUSBDataType`
	* equivalent of the "System Information" UI app
* manual GUI: menu bar Apple button / About this Mac / System Report

### files in use

If you want to know which process is using (locking) a given file, or preventing a volume from being dismounted, you can show it/them using the list open files (lsof) command...

``` sh
# basic list in process order
sudo lsof

# just containing keyword
sudo lsof | grep myFileContains

# specific file or volume
sudo lsof /Volumes/MyDiskUnit

# any file in a folder or subfolder
sudo lsof +D /path/to/folder
```


### startup processes

See what apps and executables launch automatically on startup. 

* Per-user apps
	* System Prefs / Users & Groups / select user / Login Items
	* Finder / Go / Library / Launch Agents
		* though this is not always the safest place to remove things
* System-wide
	* via Finder: 
		* /Library/StartupItems
		* /Library/LaunchDaemons
		* /Library/LaunchDaemons
		* The /System versions of these are unlikely to have any user-changeable apps

Although this is a diagnostic way to identify apps, 
deleting items here is not a reliable method to start them from starting up. Consider uninstalling components you no longer require, 
or look in the startup options inside the application's Preferences menu. 


## Manipulating plist config files

May apps' preferences can be found in the plist files in 
`$HOME/Library/Preferences`, but not all of these plists are in XML format, some of them are binary plists (a hex editor shows the magic id `bplist00`).

For the commands see:

* using `plutil` to convert binary property lists into XML and back
	* https://osxdaily.com/2016/03/10/convert-plist-file-xml-binary-mac-os-x-plutil/
* more in depth manipulation 
	* https://scriptingosx.com/2016/11/editing-property-lists/
* you can also use `/usr/libexec/PListBuddy`
* you could read binary plists in Visual Studio Code by using the extension `dnicolson.binary-plist`





## Development tips

Moved OUT to [coding#misc development tips](macUP/coding#misc%20development%20tips)
