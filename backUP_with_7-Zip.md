# backUP with 7-Zip

This article contains a list of useful commandline examples for the 7z utility, 
most of which are focussed on grabing your configs in a handy archive file. 
Although backup is the headline focus, it also considers a variety of use cases 
and suggests relevant 7z commands for them. 

## about 7z

7-Zip was initially developed by Igor Pavlov at the turn of the millenium as open-source file compression software that also introduced the new '7z' format for cabinets or containers (single destination filesystem objects that contain multiple source files from multiple folders). It brought in a general purpose data compression algorhythm known as LZMA that was free, open and sufficiently efficient that it still remains a popular choice for assorted data-types. Orignally aimed at Windows, with the `7za` command, 7-zip was ported for POSIX systems as p7zip, so these examples show the `7z` command.

Note that on Linux filesystems, 7z will not store filesystem owner or group information, so you should not rely on it for a full backup and restore mechanism. 

The [Japanese mirror](https://sevenzip.osdn.jp/chm/cmdline/) has a good depth of documentation on switches and options. 


## Compressed Selective Backup


```sh
# NB this has been tested by pasting into a terminal NOT by executing as a shell script
# escaping the ! works in bash, so from zsh run bash first

# create an archive, 7z format, with date in the filename, excluding various paths
7z a -t7z "My Backup.`date +%y%m%d`.7z" "$HOME/Folder/Path" \
 -xr\!'*/Exclude/This/Folder' \
 -xr\!'*/Exclude/This one too' \
 -xr\!'*/And this' \
 justfolder/ -x\!justfolder/*/ \

# -x recursive, escape the ! for bash, single quotes for spaces in names
# not sure if we need the * at front if recursive ?? 
#
# -r- might not work as you expect for not recursing
# see syntax for copying justfolder contents with no subfolders
#
# There is no obvious test only / dry-run mode available
#
# help, maybe - http://sourceforge.net/p/p7zip/discussion/
```

## Back up over shh

Although it doesn't use 7z the command show in 
https://github.com/artmg/lubuild/blob/master/help/configure/Secure-SHell-SSH.md#pipe-files-back-from-ssh
will make a gzip from a tar file piped over an ssh secure shell. 
A simple yet powerful way to grab configs from a remote server. 
And it's particularly effective if your authentication and authorisation is already sorted. 


## Show contents

To save you having to remember specfic syntax you could make a 
**zipShow.sh** script somewhere in your path

```sh
if [ $# -eq 1 ]
then 
  7za l "$*"
else 
  echo $0
  echo
  echo \  - Show all contents of the cabinet without extracting them
  echo \  - should would for .zip tar.gz .7z etc
  echo \  - allows for spaced file names to be passed unquoted \(as copied from finder\)
  echo \  - assumes 7z installed \(brew install p7zip\)
  echo
  echo usage:
  echo zipShow.sh pathtocabinet.xxx
fi
```

### with symlinked configs

The **zipShow.sh** script could be in your path if 
you use `SymLinked configs` and `Shell scripts on path` sections from Private Config Note for apps (needs bringing OUT here)

### manual commands

```
#### list specific folder only

7za l myarchive.7z -ir'!Exact folder name'
# bash expansion requires ! to be wrapped in quotes
# have not yet worked out how to do partial names or wildcards

```


## Other usage

### List

To view contents of cabinet:

    7z l myarchive.7z

If you want to list just specific files 
then add `-i` with options such as

    7z l myarchive.7z '-ir!*.sh*'

where:

* `r` recurses internal subfolder paths, 
* `!` indicates a wildcard is coming and 
* `' '` quotes are to prevent bash or zsh interpreting the `!` bang character 
for command hißstory expansion

### Add

To add a pattern of named files to an existing cabinet...

    7z a myarchive.7z pattern*

or instead you can specify a folder to add, and if you include a trailing slash it will add the folder name to the path

    mv pattern* archived/

once they are zipped you can archive them, 
or move them to the bin, or even use the 
`-sdel` switch to automatically delete compressed files. 

Notes: 

* unless overridden, type comes from extension, defaulting to .7z
* for rules on whether folder is included 
	* see https://sevenzip.osdn.jp/chm/cmdline/commands/add.htm


For extract, use e for filenames only or x to extract with full paths


