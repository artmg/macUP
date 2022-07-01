# macOS disks and data

This is an annexe to the [macUP macOS setup process](https://github.com/artmg/macUP/) that offers: 

* configuration guides for disk storage drives
* how to use and manipulate data files

see also:

* [misc](misc.md)
	* other generic tips and tricks

You may also want to record details of 
specific cases or issues that occur, 
along with any *Private Configuration Notes* you hold.


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


### Troubleshooting attached devices

If you want to know what accessories have been plugged in via USB 

* `ioreg -p IOUSB -w0`
	* add ` -l` to see loads of detail
* `system_profiler SPUSBDataType`
	* equivalent of the "System Information" UI app


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
	* e.g.   ./BootVolume/Store-V2/(GUID)
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

