# macOS application installation

This installs applications and adds a very basic config. 
It also outlines a number of optional packages and some maintenance. 

It is part of the [macUP macOS setup process](https://github.com/artmg/macUP/) – a public repository 
used to install and configure a macOS device in an efficient, uniform and comprehensive way. Any sensitive or personal settings should be kept separately in your own *Private Configuration Notes*.

Included with those details, which you would not want to share publicly, 
are likely to be specifics about how these applications are configured 
and any accounts used and data locations. If you don't have this comprehesively noted when you start to follow this process, you might do well 
to make more ample notes as you go along. 

## Brew package manager

Others were available (see Choices section near the end) 
but brew has a good set of features and is popular so attracts many packages

```
# Homebrew

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

### Zsh compatibility

If you get the error `zsh: brew: command not found` then you need to integrate brew into that shell's environment

```
echo "export PATH=/opt/homebrew/bin:$PATH" >> ~/.zshrc
```

and reopen the prompt for it to take effect.

### Xcode 

The homebrew installation also now includes a version of Xcode CLI tools. 
Xcode is Apple's IDE and the CLI package is c. 130 MB of tools like make, gcc, etc
which go into /Library/Developer/CommandLineTools/
(credit http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/ )

The version delivered might be out of date, but you can upgrade 
with other Apple system components when you check System Preferences / Updates later


## Basic Apps

NB: Before you install these apps, make sure you have [cleaned up your launchpad](https://github.com/artmg/macUP/blob/main/personalisation.md#clean-up-base-apps) first, else it's a pain to drag all the icons around after.

```
# create a bundle of apps in a Brewfile
cat >> Brewfile <<EndOfBrewfile

# install commands and scripts
brew 'f3'               # flash speed checker
brew 'git'
brew 'jq'               # query contents from json files 

brew 'coreutils'        # gnu equivalents e.g. gls for ls
brew 'findutils'        # gnu find as gfind etc 
brew 'gnu-sed'          # gnu sed
brew 'pandoc'           # convert formats incl markdown
brew 'transmission-cli' # torrent client

brew 'mas'              # cli interface to Mac App Store

# install apps
cask 'anydesk'          # remote desktop control without the nags
cask 'dropbox'          # cloud file sync service
cask 'dupeguru'         # find and remove duplicate files
cask 'firefox'          # still some peoples favourite browser
cask 'freac'            # rip audio cds - alternative is xld
cask 'geany'            # capable editor or lite ide
cask 'gimp'             # raster image manipulation
cask 'gittyup'          # foss gui for git workflows, fork of gitahead
cask 'google-chrome'    # fully featured browser if you have no objections
cask 'grandperspective' # visual display of disk size used by files in tree
cask 'hex-fiend'        # hex viewer / editor / compare
cask 'inkscape'         # fully supported v>1.0ß
cask 'iterm2'           # feature-rich terminal emulator
cask 'keepassxc'        # password manager using local storage
cask 'keka'             # monolithic cabinet decompression (extract all files from zip 7z)
cask 'libreoffice'      # open source and compatible office applications
cask 'macdown'          # markdown editor with preview
cask 'veracrypt'        # transparent volume encryption, depends macfuse
cask 'meld'             # diff or merge files and folders
cask 'obsidian'         # markdown notes front-end
cask 'skype'            # common conferencing service
cask 'spotify'          # music streaming service
cask 'stats'            # menu bar system monitor
cask 'vlc'              # media player for audio and video files and streams
cask 'zoom'             # was zoomus

# to get the ID search for the apps.apple.com entry and look at id in url
mas 'Discovery', id: 1381004916
# mas 'Spotify', id: 324684580 - only iOS client!
mas 'Neptunes', id: 1006739057



##
## check in your private application configs
## for any list of additional apps to add here
## 


EndOfBrewfile

# now install the bundle in the Brewfile
brew bundle install
```

#### No Longer Needed

```
## get brew cask for apps
#tap 'caskroom/cask'
## do you want to opt out of anonymous profiling analytics?
# -----------------------
```


### App Candidates

Pick any of these as you need or want

```
brew install --cask android-file-transfer # obtain data from phones and tablets
brew install        audacity              # sound file manipulation, works with ffmpeg
brew install --cask balenaetcher          # write boot image to usb
brew install        ffmpeg                # add on features for audacity
brew install --cask calibre               # eBook file converter
brew install --cask cantata               # music player daemon (mpd) client
brew install --cask clipy                 # clipboard buffer (alternatively 'flycut')
brew install --cask dashlane              # Chrome credentials manager
brew install --cask eqmac                 # eqMac2 audio equaliser 
brew install --cask evernote              # popular freemium note sync client
brew install        exiftool              # display metadata, e.g. from image files
brew install --cask db-browser-for-sqlite # sql lite browser
brew install --cask freecad               # CAD design in 3d
brew install --cask github                # desktop app aligned to GitHub repo hosting service
brew install --cask gramps                # genealogy database
brew install --cask klogg                 # log viewer, supplanted glogg
brew install --cask google-backup-and-sync # sync cloud data to archive
brew install        lnav                  # terminal log viewer
brew install        iperf                 # local bandwidth tests
brew install        iproute2mac           # python wrapper to emulate linux 'ip' command
brew install        nmap                  # local network probe
brew install        jq                    # json handler for bash scripting
brew install        libpst                # includes `readpst` for Outlook mailbox archives
brew install --cask lingon-x              # view launchd services - cannot edit!!

# some macfuse dependers were disabled in core when it went closed source
brew install --cask macfuse               # filesystems api (formerly osxfuse)
brew install gromgit/fuse/ext4fuse-mac    # Ext2/3/4 RO access, mount via cli
brew install gromgit/fuse/sshfs-mac # mount filesystems over ssh # pulls lots of depends (python,sqlite, etc)

brew install --cask microsoft-teams       # video meeting client
brew install --cask monitorcontrol        # adjust brightness volume etc via HDMI DCC
brew install --cask musicbrainz-picard    # music metadata tagger like mp3tag
brew install        neovim                # vim-like editor
brew install --cask openlp                # projection manager, e.g. song lyrics for worship
brew install --cask openvpn-connect       # basic client for OpenVPN protocol
brew install        p7zip                 # command line compression –    7z --help

brew install --cask pdfsam-basic          # freemium PDF manip, only FOSS macos option since 2011?
brew install        poppler               # some pdf manipulation commands
brew install        ghostscript           # some pdf manipulation commands
brew install        podofo                # pdf manipulation library & tools
brew install        qpdf                  # pdf manipulation tool

# workaround for pdftk
brew install https://raw.githubusercontent.com/turforlag/homebrew-cervezas/master/pdftk.rb
# https://gist.github.com/jvenator/9672772a631c117da151#gistcomment-3224117

brew install --cask qlmarkdown            # Quick Look renderer for markdown, enables indexing of MDs
brew install --cask qownnotes.            # markdown notes editor / manager / syncer
brew install        qrencode              # generate QR code images
brew install --cask sequential            # folder image browser - very out of date, but works - alternatives depend on widget system e.g. qView or gThumb
brew install --cask skype-for-business    # if required for client
brew install        smartmontools         # disk diagnostics where supported by drive
brew install --cask speedcrunch           # calc
brew install --cask sublime-text          # text and markdown editor with ToC - nags a little but powerful
brew install        tesseract tesseract-lang  # OCR - CLI engine only

# for GUI see https://github.com/artmg/lubuild/blob/master/help/manipulate/photos-and-images.md#optical-character-recognition-ocr

brew install --cask tunnelblick           # popular VPN client needs sudo 1st run
brew install --cask virtualbox            # VM environment
brew install --cask virtualbox-extension-pack 
brew install --cask wireshark             # packet sniffer for network diagnostics
brew install --cask yujitach-menumeters   # system tray activity indicator
brew install        zbar                  # zbarimg reads QR and barcode from images (no java but uses old 2011 repo not mchehab fork)

#brew install --cask aptanastudio          # web publishing IDE not quite wysiwyg (needs Java)
```

For other old apps see Deprecated and No Longer Used sections below


### Other task app candidates

Out to: ????

#### archive manager

* Built in “Archive Utility”:
    * Only expands doesn’t view
    * Can’t handle 7z files
* Keka
    * Only expands
    * not OSS?
    * Ditto “The Unarchiver”
* P7zip
    * In brew candidates above
* Zipeg?
* Quick Look addins
    * about ?
    * BetterZipQL - gratis


## Command line

### see Terminal article

Next you should follow the instructions in [terminal # Installing and Configuring your Terminal](terminal.md#Installing%20and%20Configuring%20your%20Terminal)


## Maintenance

### Validate downloads

If you download packages and need to validate 
them against files such as .txt.sha256sum 

```
# specify the algorithm with -a 
shasum -a 256 -c path/to/checksum.file
```

### App Updates

```
# help https://docs.brew.sh/FAQ

# get latest version info
brew update

# see what can be upgraded
brew outdated

# upgrade them all
brew upgrade
```

If some packages seem to be lagging...

```
brew outdated --cask --greedy
brew upgrade caskName1 caskName2
```

The GUI updater `cakebrew` is no longer maintained, 
so `brewlet`

#### new versions

If a new version of an app has not yet appeared in brew or cask, 
you can use the cask-repair script to raise a PR to update:

```
# xcode should have been installed by homebrew
# xcode-select --install

# credit https://github.com/Homebrew/homebrew-cask/blob/master/CONTRIBUTING.md#updating-a-cask
brew install vitorgalvao/tiny-scripts/cask-repair

# github credentials to create a personal access token for managing repos etc
(cd $(brew --repository) && hub issue)

cask-repair --help
```

### Brew diagnostics

#### enumerate packages installed

```
brew bundle dump
```

This will create a `Brewfile` in the current folder listing the apps installed

#### Check dependencies

```zsh
# show the installed packages in a dependency tree
brew deps --tree --installed

# check for ANY package that uses this one
brew uses --eval-all mypackage

```
#### Filesystem locations

On Intel Silicon the locations were under `/usr/local/`

* /usr/local/Cellar
* /usr/local/Caskroom

but now these have moved on Apple Silicon to `/opt/homebrew`. 
To be safe, refer to any brew location using, e.g. 

```
brew --prefix
ls -la $(brew --prefix)
```


### DEPRECATED

```
# In future use Linux VMs to run these packages

# Docker and Vagrant container platforms
# They use a VirtualBox VM, so cut out the 'middleman' and spin up an Ubuntu VM

#cask 'vagrant*'
#brew 'docker*'

# The following are still only 32-bit packages
# due to limitations in dependencies and 
# number of mac developers
# and soon Macos will not support them
# (e.g. Macos 10.15) 

# Need to run in VM Desktop instead!
#cask 'scribus'               # Desktop Publishing (DTP)  **32 bit!**

# replaced or no longer needed
#cask 'disk-inventory-x'      # analyse storage consumed **32 bit!**
#cask 'xquartz'               # x server for macOS required by inkscape, feh, ...



### NOT OR NO LONGER USED

# brew ’ntfs-3g’               # NTFS filesystem support  # Could not symlink lib/pkgconfig/libntfs-3g.pc /usr/local/lib/pkgconfig is not writable.
# cask 'utorrent'             # replace commericial with FOSS
# cask 'bonjour-browser'        # mDNS service finder - now mas Discovery
# brew install --cask aptanastudio          # web publishing IDE not quite wysiwyg (needs Java)
# brew 'md5sha1sum'      # checksum hash validator - conflicts with coreutils


#mas 'Numbers', id: 409203825
#mas 'Pages', id: 409201541


### Other candidates

brew install        feh                   # command line driven image viewer, depends xquartz x window server


# example list from https://www.perpetual-beta.org/weblog/setting-up-new-mac.html
#  brew install --cask appcleaner archey arq backblaze-downloader backuploupe bartender bittorrent-sync coconutbattery codekit commandq dropbox firefox flip4mac fluid gfxcardstatus github google-chrome iterm2 jumpcut little-snitch p4merge paparazzi perian pupil secrets sequel-pro sequential shortcat sophos-anti-virus-home-edition sublime-text superduper thisservice

## See also:
# https://github.com/serhii-londar/open-source-mac-os-apps
# http://www.freesmug.org/review:dev
```

## choice of macOS package manager

Aside from the App Store, 
Apple's default interface for finding and installing software, 
or downloading binaries from application developers' websites, 
what alternative ways are there to install packages

Homebrew, and it's **brew** command, 
with *casks* for graphical applications, 
is the most popular 'alternative package manager' by mentions. 
It is simple and effective, although some people criticise the 
fact it focusses on installs for the current user only, 
and that it requires no special privilege like using **sudo**. 
It also has the *mas* CLI for ‘Mac App Store’

Macports is often recommended by more technical folk, 
partly as being more secure, and partly because of the way it 
installs software properly for all users of a PC.
However it does have to compile packages, 
rather than downloading pre-built binaries.

pkgsrc, with its pkgin installer that is very similar to **apt** or **yum**, 
sounds like it's the best of all worlds. It can build from source, 
but also has many prebuilt packages. 
It works across many OSes in the netbsd family, 
and it does things 'the proper way'.
Unfortunately, in looking for macos / darwin packages, 
many of my chosen apps are not amongst the packages available. 

Maybe I'll just stick to 'imperfect' homebrew - it works for what I need, 
and has a pretty extensive library. 

# Application specifics

If this section grows, consider moving this out to a separate 
application_specifics.md article. 

## Dropbox client

The the official Dropbox macOS client, 
whether you install in manually by download 
or view the homebrew package, 
has historically requested privilege escalations 
that not everyone agrees with. 
Some users are concerned that asking for too high permissions 
or opening firewall access could be concerns for 
security and privacy. 

You can find some forum articles on Dropbox for macOS's 
desire for excessive permissions like [Allow incoming connections in Firewall Options](https://www.dropboxforum.com/t5/Apps-and-Installations/MacOS-X-Security-Is-it-normal-to-allow-Dropbox-app-quot-to/td-p/60167) and for [root password](https://www.dropboxforum.com/t5/Create-upload-and-share/Why-does-Dropbox-ask-for-your-computer-password/td-p/188955/page/2)

Some suggested ways to avoid these are
* do not allow Accessibility unless you need the 'badges' notifications
* When you are prompted for your Admin password just CANCEL
* in System Settings / Network / Firewall / Options
	* change Dropbox to not accept incoming connections

Doing these might disable features that some users in some cases 
might find desirable, but until you need them you can 
leave the permissions inactive for safety.

### Dropbox password dialog

(_this section might want integrating into the section above_)

After installing / upgrading dropbox using the brew formula, 
when you restart your Mac you may find yourself presented with an 
odd 'Dropbox' dialog box prompting you to

`Please enter your computer password for Dropbox to work properly`
and enter you username and password.

This is a component of the application which is trying to elevate 
privilege to install extra components in a rather non-transparent way. 

There is a slightly ranty reveal of what is going on at 
https://medium.com/@dmxt/dropbox-is-a-backdoor-for-your-macintosh-2fcdf3b211e6 
but considering the Dropbox official forums gloss over the issue, 
merely telling you to do a full manual re-install, or mentioning 
Accessibility options, you might understand the frustration.
The article https://groups.google.com/g/munki-dev/c/ooE-f2JTXxM 
goes into some explanation of steps to work around this, pointing to 
https://github.com/Ginja/Admin_Scripts/blob/master/dropbox_helper.rb
as a way to disable this behaviour.

However the offending files may have changed.
Watch this space. 

```
rm $HOME/Library/LaunchAgents/com.dropbox.DropboxMacUpdate.agent.plist
```

