# macOS miscellaneous tips and help

This is an annexe to the [macUP macOS setup process](https://github.com/artmg/macUP/) that offers: 

* generic tips for smooth running 
* and help for troubleshooting

see also:

* [shortcuts.md](shortcuts.md) 
	* keyboard quick combos
* [iOS_Apple_iPad_iPhone_reset.md](iOS_Apple_iPad_iPhone_reset.md)
	* how to wipe iOS devices (often need a Mac connected!)

You may also want to record details of 
specific cases or issues that occur, 
along with any *Private Configuration Notes* you hold.


## why … ?

A little history lesson on macOS… 

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
* 

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

Fixed Disk size is supposed to improve performance - does SSD underneath change that?

Check also Power Management settings - and what should they be in the guest?

Do you want to set up Macos as a guest OS (e.g. for a development or build environment) - it's not supported by VirtualBox but it IS within the terms of your Apple software license. See https://forums.virtualbox.org/viewforum.php?f=22

Troubleshooting hangs

* Look in Activity Monitor to understand what is occurring
* Check VBox.log file (Logs folder under where VM definition and vdi files are  
* Some candidate settings - enable 2/3d video acceleration, modify or disable audio, 
* 

### Example issues from Vbox logs

```
OpenGL Warning: blit from texture to texture not implemented

ERROR [COM]: aRC=E_ACCESSDENIED (0x80070005) aIID={872da645-4a9b-1727-bee2-5585105b9eed} 
aComponent={ConsoleWrap} aText={The object is not ready}, preserve=false aResultDetail=0

```


```
GUI: Passing request to close Runtime UI from machine-logic to UI session.
ERROR [COM]: aRC=E_ACCESSDENIED (0x80070005) aIID={872da645-4a9b-1727-bee2-5585105b9eed} aComponent={ConsoleWrap} aText={The object is not ready}, preserve=false aResultDetail=0

```


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

You can use an online video downloading service if your media originates from YouTube, Vimeo, etc. 
DVD uses 576i resolution, so it's not worth going for
higher than 720p downloads. 

The video be automatically converted by the burning program to use the MPEG-2 encoding at the correct resolution and framerate for the DVD standard, 
put you might need to choose PAL format for Europe. 


## Troubleshooting

### Attached devices

If you want to know what accessories have been plugged in via USB 

* `ioreg -p IOUSB -w0`
	* add ` -l` to see loads of detail
* `system_profiler SPUSBDataType`
	* equivalent of the "System Information" UI app

## Disks

See also:

* how to turn off (Spotlight) Search Indexing on external drives 
	* below
* testing device speed on Mac
	* [https://github.com/artmg/lubuild/blob/master/help/diagnose/disks.md#f3]
* why exFAT is not suitable for small volumes with lots of small files
	* [https://github.com/artmg/lubuild/blob/master/help/understand/disk-layout.md#compatibility]
* .

### Command line disk utility

`diskutil list`

### File system Hierarchy

‘Finder’ is the file, folder and filesystem explorer / file browser / file manager GUI application on macOS. 
Computer: At the very top of the Finder hierarchy is the Computer folder, named after the Computer (Network) name. 
Macintosh HD: Usually the Root filesystem the macOS booted from is called ‘Macintosh HD’.

#### Displayed folders

* Applications - you can run them from here
* Library - files installed to be accessible by all users logged into this device
* System - contains (System) Library subfolder only, leave it alone)
* Users - 
    * Your own folder
    * Other users
    * Shared - data common to everyone on this device

Each user’s own folder is their ‘home’, which contains:

* Desktop; Documents; Downloads; Movies; Music; Pictures; Public
    * The latter is the only one visible to other users


#### Hidden Folders

The quick way to see what these folders are called and used for is to run from a terminal ….

man hier

For full details of the FHS see https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html#//apple_ref/doc/uid/TP40010672-CH2-SW14 

### Hidden folders on USB

* .fseventsd
    * the FileSystem Events Daemon makes a note of changes that should be sent to spotlight search for indexing
* 


### Compression cabinets

compressed files like .zip .7z 

#### Using 7z commands

p7zip has been installed via brew so the 7z commands 
can be used from the CLI

see `Service.IN/Automation/7z command examples.md`


### Secure erase of data

Apple has removed Disk Utility's feature to 'Secure Erase Free Space' 
because so many disks are SSD's and this is no longer necessary/compatible(?).

If you want to secure erase an ATA attached SSD see 
[lubuild/help/??/remove-data#solid-state-flash-drives]

If you are SURE this is a Hard Disk Drive (HDD) using spinning magnetic media 
(you can feel it move when you access data) then you can use:

```
diskutil secureErase freespace 1 /Volumes/mydisk
```

check this command carefully to make sure you ONLY clearing `freespace` 
and that you are using the right volume. 
Back up your data first in you are unsure. 

This utility _might_ be available from the Terminal in Recovery Mode.


### Spotlight search indexing

#### about

* Spotlight Search is the default in-built indexing and search service
* Command Space for a spotlight window
* You can also access via Siri https://support.apple.com/kb/PH25073 
* From Sierra versions, the safari browser also includes SpotLight as a source for matches
* remote volumes are not targeted unless you choose Finder and select the volume
* Apparently it also recognises scenes and expressions in photos! https://www.cultofmac.com/441365/how-to-search-your-photos-by-objects-and-scenery-in-macos-sierra/ 

#### search criteria

To narrow your spotlight search you can add [criteria from metadata attributes](https://support.apple.com/kb/PH25589?locale=en_GB), such as

* kind:document 
* modified:<=18/01/22

See also https://www.lifewire.com/manage-smart-search-safari-for-mac-4103702 

#### CLI utility

Spotlight depends on the Metadata Server to collect file information 
in the background as you work. There are a number of md* utilities including:

```
# show the status of indexing on all current volumes:
mdutil -sav       # show, all vols, verbose

# initiate reindexing of a folder

mdimport /path/to/folder

# perform a command line spotlight search
mdfind
```

#### Technical background

* mds (metedata server) is the parent process for Spotlight workers
* use the ` mdutil -sav ` command above to see volume indexing status
* internal and remote volume indexes are stored:
	* /private/var/db/Spotlight-V100/
	* e.g.   ./BootVolume/Store-V2/<GUID>
	* or   ./Volumes/protocol//user@server_adress/sharepoint
* external volumes are said to be stored in /.Spotlight-V100 on the volume root
* more details in the article...

credit: https://care.qumulo.com/hc/en-us/articles/115008514788-Mac-OS-X-Spotlight-Search-and-Qumulo

* There is a folder /System/Volumes/Data/.Spotlight-V100
	* sudo can't access as it's `root` not `wheel`
* it is not clear where the fuse-mounted volumes are indexed
	* could not hidden find in the root
	* searching `veracrypt fuse spotlight` did not return many results
* 

* As well as via the GUI it can be en/disabled with launchctl
sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.metadata.mds.plist


#### Indexing encrypted disks


```
# current status
mdutil -sav
# turn on
sudo mdutil -i on /Volumes/MyVol
# status - although we use short vol name, the output shows full path
sudo mdutil -svp /Volumes/MyVol
# rebuild
sudo mdutil -E /Volumes/MyVol

# show contents of index directory
sudo mdutil -L /Volumes/MyVol
```

Despite the fact that `sudo ls` won't show it, 
this last command DOES show the locations INSIDE the volume.
Once indexing has completed the size of these 
index directory contents total to around seven percent 
of the actual file data content size on the disk, 
and this addition is reflected in the 'Used on disk' total, 
alongside the other 5-7% already 'occupied' 
by other disk storage overhead. 

##### old

Is macOS sensible enough to create the image for a disk volume that is encypted *inside* the volume, to stop secrets being visible in plaintext _outside_ the volume? Is this also the case with third-party volume encryption? Can we check the index size to see it's really going to the right place?

e.g. 

* `indexes of metadata used by Spotlight for search purposes. They are held in the hidden folder .Spotlight-V100 at the root level of each volume, containing only metadata for that volume`
	* https://eclecticlight.co/2018/06/26/hidden-caches-in-macos-where-your-private-data-gets-stored/


#### prevent external disk indexing

If you plug in an external disk drive that is writable, Spotlight will begin indexing it whether you want it to or not. 
If you want to stop the Spotlight Indexing on a drive use:

* System Preferences / Spotlight / Privacy
* + to add a location to prevent Searching
* Browse to the drive root

##### programmatic methods

you used to be able to `touch .metadata_never_index` 
in a folder to prevent indexing, but now its a 
little more involved

```
# credit https://apple.stackexchange.com/a/255731
sudo defaults write /Volumes/foo/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array-add '/path/to/folder'
sudo launchctl stop com.apple.metadata.mds && sudo launchctl start com.apple.metadata.mds
```

##### Find when not indexed

If you are in Finder, in a filesystem that is excluded from indexing, then how do you quickly and easily perform a unix 'find' for a file from your location?

Answer TBC here please :)

```
# find in names
find . -iname "*string*"

# find in files
grep -R 'string' .
```

#### Rebuild the index

If you want to rebuild the index on a folder or volume:

* use the GUI procedure above
* add the path as a prevented location
* now remove that location
* you can now exit

In the background spotlight will first clear down the old index then rebuild it afresh.

* See also [How to force a re-index](https://appletoolbox.com/2017/10/spotlight-search-not-working-on-macbook-how-to-fix/) 


## Time Machine backup

### Log diagnostics

```
# Show INFO level logs for Time Machine backups for last 48h
log show --style syslog  --predicate 'senderImagePath contains[cd] "TimeMachine"' --info --last 48h

# Show INFO level logs for Time Machine backups for specific date range
log show --style syslog  --predicate 'senderImagePath contains[cd] "TimeMachine"' --info --start '2020-03-17 00:00:00' --end '2020-03-18 00:00:00' 

# watch DEBUG level logs for Time Machine backups as they happen
log stream --style syslog  --predicate 'senderImagePath contains[cd] "TimeMachine"' --debug
```

### Network Connections

Browsing to a Guest share via Finder mounts as follows

    //GUEST:@SERVERNAME._smb._tcp.local/ShareName on /Volumes/ShareName (smbfs, nodev, nosuid, noowners, mounted by user)

Using Time Machine

```
//GUEST:@SERVERNAME._smb._tcp.local./TimeMachineBackup on /Volumes/com.apple.TimeMachine.TimeMachineBackup-95CFF6D1-1ABA-432F-9773-EC3D6B2AF7C7 (smbfs, nobrowse)
/dev/disk2s2 on /Volumes/Time Machine Backups (hfs, local, nodev, nosuid, journaled, nobrowse)
com.apple.TimeMachine.2020-02-16-210112@/dev/disk1s1 on /Volumes/com.apple.TimeMachine.localsnapshots/Backups.backupdb/MacName/2020-02-16-210112/Macintosh HD (apfs, local, read-only, journaled, nobrowse)
com.apple.TimeMachine.2020-02-11-205345@/dev/disk1s1 on /Volumes/com.apple.TimeMachine.localsnapshots/Backups.backupdb/MacName/2020-02-11-205345/Macintosh HD (apfs, local, read-only, journaled, nobrowse)
```



## Manipulating plist config files

May apps' preferences can be found inthe plist files in 
$HOME/Library/Preferences

For the commands see:

* to convert binary property lists into XML and back
	* https://osxdaily.com/2016/03/10/convert-plist-file-xml-binary-mac-os-x-plutil/
* more in depth manipulation 
	* https://scriptingosx.com/2016/11/editing-property-lists/
* 





## Development tips

### Using Valgrind to diagnose memory issues

* seg fault 11
* install
    * brew install valgrind
    * macos 10.14 version not currently supported
    * brew install --HEAD https://raw.githubusercontent.com/sowson/valgrind/master/valgrind.rb



```
2020-03-10 15-51-23.411 [Notice] (playerCallback) No chunk received for 5000ms. Closing Audio Queue.
AddressSanitizer:DEADLYSIGNAL
=================================================================
==27258==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000018 (pc 0x00010084c155 bp 0x700009584b30 sp 0x700009584600 T2)
==27258==The signal is caused by a WRITE memory access.
==27258==Hint: address points to the zero page.
    #0 0x10084c154  (snapclient:x86_64+0x100118154)
    #1 0x10084a7ea  (snapclient:x86_64+0x1001167ea)
    #2 0x10080c6f5 in void* std::__1::__thread_proxy<std::__1::tuple<std::__1::unique_ptr<std::__1::__thread_struct, std::__1::default_delete<std::__1::__thread_struct> >, void (Player::*)(), Player*> >(void*) (snapclient:x86_64+0x1000d86f5)
    #3 0x7fff68dd22ea in _pthread_body (libsystem_pthread.dylib:x86_64+0x32ea)
    #4 0x7fff68dd5248 in _pthread_start (libsystem_pthread.dylib:x86_64+0x6248)
    #5 0x7fff68dd140c in thread_start (libsystem_pthread.dylib:x86_64+0x240c)

==27258==Register values:
rax = 0x00000000000044e8  rbx = 0x00007000095849e0  rcx = 0x0000100000000000  rdx = 0x0000000000000008
rdi = 0x0000000000000018  rsi = 0x0000100000000000  rbp = 0x0000700009584b30  rsp = 0x0000700009584600
 r8 = 0x000060f000007840   r9 = 0x0000700009584801  r10 = 0x000000000000007d  r11 = 0x0000000000000002
r12 = 0x0000700009584600  r13 = 0x00001e00012b08ce  r14 = 0x0000700009584828  r15 = 0x00001e00012b08c0
AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV (snapclient:x86_64+0x100118154)
Thread T2 created by T1 here:
    #0 0x1009d102d in wrap_pthread_create (libclang_rt.asan_osx_dynamic.dylib:x86_64h+0x5402d)
    #1 0x10080c54e  (snapclient:x86_64+0x1000d854e)
    #2 0x100808a86  (snapclient:x86_64+0x1000d4a86)
    #3 0x1008251d6  (snapclient:x86_64+0x1000f11d6)
    #4 0x1007cd40a  (snapclient:x86_64+0x10009940a)
    #5 0x1007ce25b  (snapclient:x86_64+0x10009a25b)
    #6 0x100801505 in void* std::__1::__thread_proxy<std::__1::tuple<std::__1::unique_ptr<std::__1::__thread_struct, std::__1::default_delete<std::__1::__thread_struct> >, void (ClientConnection::*)(), ClientConnection*> >(void*) (snapclient:x86_64+0x1000cd505)
    #7 0x7fff68dd22ea in _pthread_body (libsystem_pthread.dylib:x86_64+0x32ea)
    #8 0x7fff68dd5248 in _pthread_start (libsystem_pthread.dylib:x86_64+0x6248)
    #9 0x7fff68dd140c in thread_start (libsystem_pthread.dylib:x86_64+0x240c)

Thread T1 created by T0 here:
    #0 0x1009d102d in wrap_pthread_create (libclang_rt.asan_osx_dynamic.dylib:x86_64h+0x5402d)
    #1 0x10080135e  (snapclient:x86_64+0x1000cd35e)
    #2 0x1007c2f69  (snapclient:x86_64+0x10008ef69)
    #3 0x10082a8da  (snapclient:x86_64+0x1000f68da)
    #4 0x100747764  (snapclient:x86_64+0x100013764)
    #5 0x7fff68bde3d4 in start (libdyld.dylib:x86_64+0x163d4)

==27258==ABORTING
Abort trap: 6
```

* valgrind --leak-check=yes /usr/local/bin/snapclient
* valgrind --leak-check=yes --main-stacksize=8388608 /usr/local/bin/snapclient
* valgrind --leak-check=yes --main-stacksize=134217728 /usr/local/bin/snapclient
* 

```

==26740== Process terminating with default action of signal 11 (SIGSEGV)
==26740==  Access not within mapped region at address 0x18
==26740==    at 0x10133E808: _pthread_wqthread_setup (in /usr/lib/system/libsystem_pthread.dylib)
==26740==    by 0x10133E497: _pthread_wqthread (in /usr/lib/system/libsystem_pthread.dylib)
==26740==    by 0x10133E3FC: start_wqthread (in /usr/lib/system/libsystem_pthread.dylib)
==26740==  If you believe this happened as a result of a stack
==26740==  overflow in your program's main thread (unlikely but
==26740==  possible), you can try to increase the size of the
==26740==  main thread stack using the --main-stacksize= flag.
==26740==  The main thread stack size used in this run was 8388608.

valgrind: m_scheduler/scheduler.c:1028 (void run_thread_for_a_while(HWord *, Int *, ThreadId, HWord, Bool)): Assertion 'VG_(in_generated_code) == False' failed.

host stacktrace:
==26740==    at 0x258041E10: ???
==26740==    by 0x258042186: ???
==26740==    by 0x258042166: ???
==26740==    by 0x2580B8D47: ???
==26740==    by 0x2580B6E61: ???
==26740==    by 0x2580C81C7: ???
==26740==    by 0x2580C8484: ???

sched status:
  running_tid=5

Thread 1: status = VgTs_WaitSys syscall unix:305 (lwpid 771)
==26740==    at 0x1012EB866: __psynch_cvwait (in /usr/lib/system/libsystem_kernel.dylib)
==26740==    by 0x10134256D: _pthread_cond_wait (in /usr/lib/system/libsystem_pthread.dylib)
==26740==    by 0x100E3AB30: std::__1::condition_variable::__do_timed_wait(std::__1::unique_lock<std::__1::mutex>&, std::__1::chrono::time_point<std::__1::chrono::system_clock, std::__1::chrono::duration<long long, std::__1::ratio<1l, 1000000000l> > >) (in /usr/lib/libc++.1.dylib)
==26740==    by 0x100020504: ??? (in /usr/local/bin/snapclient)
==26740==    by 0x100020427: ??? (in /usr/local/bin/snapclient)
==26740==    by 0x10002C96A: ??? (in /usr/local/bin/snapclient)
==26740==    by 0x10002C451: ??? (in /usr/local/bin/snapclient)
==26740==    by 0x1000462B5: void* std::__1::__thread_proxy<std::__1::tuple<std::__1::unique_ptr<std::__1::__thread_struct, std::__1::default_delete<std::__1::__thread_struct> >, void (Player::*)(), Player*> >(void*) (in /usr/local/bin/snapclient)
==26740==    by 0x1000060E6: ??? (in /usr/local/bin/snapclient)
==26740==    by 0x1010543D4: start (in /usr/lib/system/libdyld.dylib)
client stack range: [0x104149000 0x104948FFF] client SP: 0x104947C48
valgrind stack range: [0x7000009B2000 0x700000AB1FFF] top usage: 9768 of 1048576

Thread 4: status = VgTs_Yielding (lwpid 9731)
==26740==    at 0x10133E808: _pthread_wqthread_setup (in /usr/lib/system/libsystem_pthread.dylib)
==26740==    by 0x10133E497: _pthread_wqthread (in /usr/lib/system/libsystem_pthread.dylib)
==26740==    by 0x10133E3FC: start_wqthread (in /usr/lib/system/libsystem_pthread.dylib)
client stack range: ??????? client SP: 0x700003C79F80
valgrind stack range: [0x700003F4A000 0x700004049FFF] top usage: 3808 of 1048576

Thread 5: status = VgTs_Runnable (lwpid 9475)
==26740==    at 0x10133E3F0: start_wqthread (in /usr/lib/system/libsystem_pthread.dylib)
client stack range: ??????? client SP: 0x700003F45B80
valgrind stack range: [0x70000404E000 0x70000414DFFF] top usage: 8720 of 1048576

```

Note: see also the FAQ in the source distribution.
It contains workarounds to several common problems.
In particular, if Valgrind aborted or crashed after
identifying problems in your program, there's a good chance
that fixing those problems will prevent Valgrind aborting or
crashing, especially if it happened in m_mallocfree.c.

If that doesn't help, please report this bug to: www.valgrind.org

In the bug report, send all the above text, the valgrind
version, and what OS and version you are using.  Thanks.


