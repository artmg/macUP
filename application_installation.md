# macOS application installation

This installs applications and adds a very basic config. 
It also outlines a number of optional packages and some maintenance. 

It is part of the [macUP macOS setup process](https://github.com/artmg/macUP/) – a public repository 
used to install and configure a macOS device in an efficient, uniform and comprehensive way. Any sensitive or personal settings should be kept separately in your own *Private Configuration Notes*.

Included with those details, which you would not want to share publicly, 
are likely to be specifics about how these applications are configured 
and any accounts used and data locations. If you don't have this comprehesively noted when you start to follow this process, you might do well 
to make more ample notes as you go along. 

**TTD:** move terminal setup out into [terminal.md](terminal.md) 

## Brew package manager

Others were available (see Choices section near the end) 
but brew has a good set of features and is popular so attracts many packages

```
# Homebrew

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

### Xcode 

The homebrew installation also now includes a version of Xcode CLI tools. 
Xcode is Apple's IDE and the CLI package is c. 130 MB of tools like make, gcc, etc
which go into /Library/Developer/CommandLineTools/
(credit http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/ )

The version delivered might be out of date, but you can upgrade 
with other Apple system components when you check System Preferences / Updates later


## Basic Apps

```
# create a bundle of apps in a Brewfile
cat >> Brewfile <<EndOfBrewfile

# install commands and scripts
brew 'f3'             # flash speed checker
brew 'git'
brew 'jq'             # query contents from json files 
#brew 'md5sha1sum'     # checksum hash validator - conflicts with coreutils
brew 'coreutils'      # gnu equivalents e.g. gls for ls
brew 'findutils'      # gnu find as gfind etc 
brew 'gnu-sed'.       # gnu sed
brew 'pandoc'         # convert formats incl markdown
brew 'transmission'   # torrent client

brew 'mas'            # cli interface to Mac App Store

# install apps
cask 'cantata'
cask 'dropbox'
cask 'dupeguru'         # find and remove duplicate files
cask 'balenaetcher'     # write boot image to usb
cask 'evernote'
cask 'firefox'          # still some peoples favourite browser
cask 'freac'            # rip audio cds - alternative is xld
cask 'geany'            # capable editor or lite ide
cask 'gitahead'         # foss gui for git workflows
cask 'gimp'             # raster image manipulation
cask 'google-chrome'
cask 'gramps'           # genealogy database
cask 'grandperspective' # visual display of disk size used by files in tree
cask 'hex-fiend'        # hex viewer / editor / compare
cask 'inkscape'         # fully supported v>1.0ß
cask 'iterm2'           # feature-rich terminal emulator
cask 'keepassx'
cask 'keka'             # monolithic cabinet decompression (extract all files from zip 7z)
cask 'libreoffice'
cask 'macdown'          # markdown editor with preview
cask 'meld'             # diff or merge files and folders
cask 'macfuse'          # filesystems incl SSHFS (formerly called osxfuse)
brew 'ext4fuse'         # Ext2/3/4 RO access, mount via cli, depends macfuse
cask 'veracrypt'        # transparent volume encryption, depends macfuse
cask 'qownnotes'        # markdown notes editor / manager / syncer
cask 'spotify'
cask 'skype'
cask 'stats'            # menu bar system monitor
cask 'virtualbox'       # VM environment
cask 'virtualbox-extension-pack'
cask 'vlc'
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

## Some basic config

### XDG Base directory structure

This makes the [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec) accessible by macOS Zsh by creating the variables for use by code

```
# create the structure
mkdir -p ~/.config/
mkdir -p ~/.cache/
mkdir -p ~/.local/share/
# this is more PEP than XDG but its one we will use
mkdir -p ~/.local/bin/

mkdir -p ~/.config/zsh/
mv ~/.zshenv ~/.config/zsh/.zshenv.old

# This is the most minimal .dotfile presence to bootstrap the rest under XDG
# credit https://www.reddit.com/r/zsh/comments/3ubrdr/
cat >> ~/.zshenv <<EndOfHomeZshEnv
export ZDOTDIR=\$HOME/.config/zsh
source \$ZDOTDIR/.zshenv
EndOfHomeZshEnv

cat > ~/.config/zsh/.zshenv <<EndOfConfigZshEnv
# First lets make sure that the ZDG variables are available to all code
# unlikely they are already set, but let's allow override
export XDG_CACHE_HOME="\${XDG_CONFIG_HOME:-\$HOME/.cache}"
export XDG_CONFIG_HOME="\${XDG_CONFIG_HOME:-\$HOME/.config}"
export XDG_DATA_HOME="\${XDG_DATA_HOME:-\$HOME/.local/share}"
export PEP_USER_SCRIPT="\${PEP_USER_SCRIPT:-\$HOME/.local/share}"
EndOfConfigZshEnv

# if needed in .zshrc ?
#SSH_CONFIG="-F \${XDG_CONFIG_HOME}/ssh/config"
# credit https://superuser.com/a/874924
# but this would also need the alias
# so you may as well put it all into the alias
# but you still need to manually specify the path to key files in config
```

**Make sure to close** the terminal so these take effect next time you reopen it. 

see also other ideas towards a dotless home:

* ArchWiki's useful explanation of XDG Base Dirs https://wiki.archlinux.org/index.php/XDG_Base_Directory
* PEP 370 that proposes other .local locations https://www.python.org/dev/peps/pep-0370/
* more ideas on managing dotfiles https://github.com/webpro/awesome-dotfiles
* cross-platform locations of XDG Base Directories or their equivalents https://github.com/folder/xdg
* macOS default folders https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html#//apple_ref/doc/uid/TP40010672-CH2-SW1
* 

## Command line

`zsh` is now the default on macOS, so this will 
upgrade and configure it, make it work well with iTerm 
and add Oh My Zsh to keep up with the trendy devs ;) 

For help see: 

* switching from bash https://scriptingosx.com/2019/06/moving-to-zsh/
* keybindings https://github.com/ohmyzsh/ohmyzsh/blob/master/lib/key-bindings.zsh
    * using macos style combinations https://coderwall.com/p/a8uxma/zsh-iterm2-osx-shortcuts

### Terminal

* iTerm2
	* Launch App / Right click in Dock icon / Options / Keep in Dock
	* ⌘-, Preferences
	    * Keys / Hotkey / Create dedicated
	        * **double-tap Control**
        * Profile / Hotkey
            * Window / Keep background colours opaque
	    * Profile / Basic / Badge
	        * \(session.username)@\(session.hostname)
    * Relaunch
    * SysPrefsPriv / Accesibility / Allow iTerm
    * 


### Zsh basics

* NB next time: Change this to use $ZDOTDIR (`~/.config/zsh/`) 
and move .iterm* and .oh-my-zsh into  `~/.local/bin/` subfolders


```
ZDOTDIR=${XDG_CONFIG_HOME}/zsh
ZSH=${PEP_USER_SCRIPT}/oh-my-zsh
ZSH_CUSTOM=${ZSH}/custom
```


```
setopt interactivecomments

### Install the latest version of the shell

# see current version
zsh --version

# compare with the latest on brew
brew info zsh
# more detail on comparing at https://rick.cogley.info/post/use-homebrew-zsh-instead-of-the-osx-default/

# install the latest
brew install zsh
# note the caveats on ncurses if that is an important library for your work 
```

### iTerm shell integration

```
# credit https://www.iterm2.com/documentation-shell-integration.html


curl -L https://iterm2.com/shell_integration/${SHELL:5} \
-o ~/.iterm2_shell_integration.${SHELL:5}
source ~/.iterm2_shell_integration.${SHELL:5} 
echo "source ~/.iterm2_shell_integration.${SHELL:5}" | cat >> ~/.profile

# should be cat >> $ZDOTDIR/.zshrc


### Set this up on other servers you commonly ssh
### if they are behind a firewall, 

REMOTE_SERVER=user@myserver
curl -L https://iterm2.com/shell_integration/bash | ssh ${REMOTE_SERVER} 'cat > ~/.iterm2_shell_integration.bash'
echo "source ~/.iterm2_shell_integration.bash" | ssh ${REMOTE_SERVER} 'cat >> ~/.bash_profile'
```

### OhMyZsh

```
# credit https://github.com/ohmyzsh/ohmyzsh/#via-curl

sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

#### and theme
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k


nano ~/.zshrc
# nano $ZDOTDIR/.zshrc


```

find these two lines and replace them

```
ZSH_THEME="powerlevel10k/powerlevel10k"

plugins=(git iterm2 osx)
```

add this just before sourcing OMZ

```
# 'avoid' for now the issue with perms on /usr/local/share/zsh
# mentioned during OMZ install
ZSH_DISABLE_COMPFIX=true
```

then add this to the end

```
# add the iTerm shell integration you just downloaded
source ~/.iterm2_shell_integration.zsh

# allow the bash-like hash comments on the prompt
setopt interactivecomments

# don't page if less than one screen, e.g. for commands like git branch
export LESS="$LESS --no-init --quit-if-one-screen"
# credit https://stackoverflow.com/q/48341920#comment92614882_48370253
```

* close and reopen the terminal to see it all working
* to upgrade later use `omz update`

### Shell personalisation

```
#### ^L to clear screen also clears scroll-back buffer

cat >> ~/.config/zsh/.zle <<EndOfConfigZle

function clear-scrollback-buffer {
  # Behavior of clear: 
  # 1. clear scrollback if E3 cap is supported (terminal, platform specific)
  # 2. then clear visible screen
  # For some terminal 'e[3J' need to be sent explicitly to clear scrollback
  clear && printf '\e[3J'
  # .reset-prompt: bypass the zsh-syntax-highlighting wrapper
  # https://github.com/sorin-ionescu/prezto/issues/1026
  # https://github.com/zsh-users/zsh-autosuggestions/issues/107#issuecomment-183824034
  # -R: redisplay the prompt to avoid old prompts being eaten up
  # https://github.com/Powerlevel9k/powerlevel9k/pull/1176#discussion_r299303453
  zle && zle .reset-prompt && zle -R
}

zle -N clear-scrollback-buffer
bindkey '^L' clear-scrollback-buffer

# credit https://unix.stackexchange.com/a/531178

EndOfConfigZle

cat >> ~/.config/zsh/.zshrc <<EndOfConfigZshRC

# include shell personalisation through Zle Zsh Line Editor
source ~/.config/zsh/.zle

EndOfConfigZshRC

```

### Gnu Utils

The coreutils and findutils have been installed by brew, 
but you can use them instead of macOS / FreeBSD equivalents, 
e.g. ls instead of gls or find instead of gfind, 
to allow seemless switching between mac and linux hosts

```
cat >> ~/.config/zsh/.zshrc <<EndOfConfigZshRC

# prefer GNU utilities over FreeBSD variety that come with macOS
export PATH="/usr/local/opt/coreutils/libexec/gnubin:/usr/local/opt/findutils/libexec/gnubin:/usr/local/opt/gnu-sed/libexec/gnubin:$PATH"
EndOfConfigZshRC
```

### Powerlink 10k 

NB: **OPTIONAL**

in zshrc

```
# Enable Powerlevel10k instant prompt. Should stay close to the top of ~/.config/zsh/.zshrc.
# Initialization code that may require console input (password prompts, [y/n]
# confirmations, etc.) must go above this block; everything else may go below.
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi

# To customize prompt, run `p10k configure` or edit ~/.config/zsh/.p10k.zsh.
[[ ! -f ~/.config/zsh/.p10k.zsh ]] || source ~/.config/zsh/.p10k.zsh
```


#### No Longer Needed

```
## get brew cask for apps
#tap 'caskroom/cask'
## do you want to opt out of anonymous profiling analytics?
# -----------------------
```


### Candidates

Pick any of these as you need or want

```
brew install --cask android-file-transfer # obtain data from phones and tablets
brew install --cask anydesk               # remote desktop control without the nags of TeamViewer
brew install        audacity              # sound file manipulation, works with ffmpeg
brew install        ffmpeg                # add on features for audacity
brew install --cask calibre               # eBook file converter
brew install --cask clipy                 # clipboard buffer (alternatively 'flycut')
brew install --cask dashlane              # Chrome credentials manager
brew install --cask eqmac                 # eqMac2 audio equaliser 
brew install        exiftool              # display metadata, e.g. from image files
brew install --cask db-browser-for-sqlite # sql lite browser
brew install --cask freecad               # CAD design in 3d
brew install --cask github                # desktop app aligned to GitHub repo hosting service
brew install --cask klogg                 # log viewer, supplanted glogg
brew install --cask google-backup-and-sync # sync cloud data to archive
brew install        lnav                  # terminal log viewer
brew install        iperf                 # local bandwidth tests
brew install        iproute2mac           # python wrapper to emulate linux 'ip' command
brew install        nmap                  # local network probe
brew install        jq                    # json handler for bash scripting
brew install        libpst                # includes `readpst` for Outlook mailbox archives
brew install --cask lingon-x              # view launchd services - cannot edit!!
brew install --cask microsoft-teams       # video meeting client
brew install --cask monitorcontrol        # adjust brightness etc via HDMI DCC
brew install --cask musicbrainz-picard    # music metadata tagger like mp3tag
brew install        neovim                # vim-like editor
brew install --cask obsidian              # markdown notes front-end
brew install --cask openlp                # projection manager, e.g. song lyrics for worship
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
brew install        qrencode              # generate QR code images
brew install --cask sequential            # folder image browser - very out of date, but works - alternatives depend on widget system e.g. qView or gThumb
brew install --cask skype-for-business    # if required for client
brew install        smartmontools         # disk diagnostics where supported by drive
brew install --cask speedcrunch           # calc
brew install --cask sshfs                 # mount filesystems over ssh # may require macfuse, pulls lots of depends (python,sqlite, etc)
brew install --cask sublime-text          # text and markdown editor with ToC - nags a little but powerful
brew install --cask openvpn-connect       # basic client for OpenVPN protocol
brew install --cask tunnelblick           # more popular VPN client needs moving to Applications
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

### enumerate packages installed

```
brew bundle dump
```

This will create a `Brewfile` in the current folder listing the apps installed


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

