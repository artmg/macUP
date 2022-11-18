# macOS terminal

This part of the [macUP macOS setup process](https://github.com/artmg/macUP/) focuses on: 

* using different terminal software
* local and remote command line interpreter (CLI) access

see also:

* [application_installation.md](application_installation.md) 
	* how to install some of the utilities mentioned here
	* the configuration is described below

[macUP]() is a public repository 
used to install and configure a macOS device in an efficient, uniform and comprehensive way.Any sensitive or personal settings should be kept separately in your own *Private Configuration Notes*.


## Introduction

### iTerm2

* open-source project that is currently active
* available via package managers
* extensible
* 

### Zee-shell (zsh)

`zsh` is now the default on macOS

For help getting used to the move from bash?

* switching from bash https://scriptingosx.com/2019/06/moving-to-zsh/
* keybindings https://github.com/ohmyzsh/ohmyzsh/blob/master/lib/key-bindings.zsh
    * using macos style combinations https://coderwall.com/p/a8uxma/zsh-iterm2-osx-shortcuts


### Oh-my-Zsh

This terminal theming software is mainly about appearance, but in a way that is designed to help developers. Instead of always facing a monchrome terminal, this tries to use colour and symbols to add visual clues for:

* where you are, in which repo, and in which context
* what is the git status of that location
* how long commands took and whether they succeded

There are also useful shortcuts and features in the myriad plugins you can add according to what software you code with, in and for. It gained so much popularity that it spawned _oh-my-bash_ for Linux and _oh-my-posh_ for Windows Terminal and PowerShell 

## Usage

### Multiplexing

Probably the greatest benefit of terminal multiplexers like `screen` and `tmux` is that 
they keep a session going when you loose the connection. If, for instance, your `ssh` shell 
gets disconnected by the network, then you don't 
have to start all over again. 

Although `screen` is simple, popular, lightweight, 
and often bundled in with an operationg system, 
we will look at **tmux** here because a) it is more actively developed, but mainly b) it integrates with iTerm2. 

NB: many of the commands shown here are for Debian Linux, as you will tend to run them on your remote servers, not the macOS client you connect from. 

```
#### examples
# create new named session
tmux new -s mysession
# show existing sessions
tmux ls
# attach to named target session
tmux attach -t mysession
```

To send commands to the multiplexer you use 
its attention keystroke (Ctrl-A for screen, 
Ctrl-B for tmux) followed by commands. 

#### iTerm2 tmux integration

if you create your session using tmux -CC or 

`tmux -CC new -S myname`

then iTerm integration will split this into a separate-looking windows, which avoids some of 
the issues of keystroke clashes. It does not however 
have the status bar at the bottom. 


#### late muxing

But what if you have already started a long running 
process, and realise you should have muxed (multiplexed) it?

You can use the **reptyr** utility to pick up processes running in a regular shell, and move them
into a muxed shell.

`sudo apt install reptyr`

If you are running multiple processes (e.g. piped commands) you can steal the entire session using **-T**

```
# start a new session called myname
tmux new -s myname

# find the PID of the bash command that started the shell
ps -A | grep pts

# now grab that session
sudo reptyr -T 98765

```

## Installing and Configuring your Terminal

These setup steps have been moved out from [application_installation.md](application_installation.md)

If you haven't already you might need to install...

```
# the terminal emulator
brew install --cask iterm2
# unix-like commands for familiar syntax 
brew install coreutils findutils gnu-sed
```


### Terminal

* iTerm2
	* Launch App / Right click in Dock icon / Options / Keep in Dock
	* ⌘ ,  for Preferences
	    * Keys / Hotkey / Create dedicated
	        * **double-tap Control**
        * Profile / Hotkey
            * Window / Keep background colours opaque
	    * Profile / Basic / Badge
	        * `  \(session.username)@\(session.hostname)  `
    * Relaunch
    * SysPrefsPriv / Accesibility / Allow iTerm
    * 


### Zsh basics

```
setopt interactive_comments

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

restart the terminal to use the new version

### XDG Base directory structure

This makes the [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec) accessible by macOS Zsh by creating the variables for use by code

```
setopt interactive_comments

#### Set ZDG variables for use during this install

export XDG_CACHE_HOME="${XDG_CACHE_HOME:-$HOME/.cache}"
export XDG_CONFIG_HOME="${XDG_CONFIG_HOME:-$HOME/.config}"
export XDG_DATA_HOME="${XDG_DATA_HOME:-$HOME/.local/share}"
# this is more PEP than XDG but its one we will use
export PEP_SCRIPT_USER="${PEP_SCRIPT_USER:-$HOME/.local/bin}"
# and this one is for Zsh itself
export ZDOTDIR=${XDG_CONFIG_HOME}/zsh

#### create the structure
mkdir -p $XDG_CACHE_HOME
mkdir -p $XDG_CONFIG_HOME
mkdir -p $XDG_DATA_HOME
mkdir -p $PEP_SCRIPT_USER
mkdir -p $ZDOTDIR

#### Back up old zshenv
mv ~/.zshenv ~/.config/zsh/.zshenv.old

# This is the most minimal .dotfile presence to bootstrap the rest under XDG
# credit https://www.reddit.com/r/zsh/comments/3ubrdr/
cat >> ~/.zshenv <<EndOfHomeZshEnv
export ZDOTDIR=$ZDOTDIR
source \$ZDOTDIR/.zshenv

EndOfHomeZshEnv

cat > ${ZDOTDIR}/.zshenv <<EndOfConfigZshEnv
# First lets make sure that the ZDG variables are available to all code
# unlikely they are already set, but let's allow override
export XDG_CACHE_HOME="\${XDG_CACHE_HOME:-\$HOME/.cache}"
export XDG_CONFIG_HOME="\${XDG_CONFIG_HOME:-\$HOME/.config}"
export XDG_DATA_HOME="\${XDG_DATA_HOME:-\$HOME/.local/share}"
export PEP_SCRIPT_USER="\${PEP_SCRIPT_USER:-\$HOME/.local/bin}"

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

### iTerm shell integration

You can integrate your shell (e.g. zsh) with the iTerm app for a series of features like command and output history, and other that integrate with ssh like right-click to download files and drag and drop to upload, 

```
setopt interactive_comments
# credit https://www.iterm2.com/documentation-shell-integration.html

#### Set this up on your local shell

mkdir -p ${PEP_SCRIPT_USER}/iterm2
curl -L https://iterm2.com/shell_integration/${SHELL:5} \
-o ${PEP_SCRIPT_USER}/iterm2/iterm2_shell_integration.${SHELL:5}
source ${PEP_SCRIPT_USER}/iterm2/iterm2_shell_integration.${SHELL:5}
echo "source ${PEP_SCRIPT_USER}/iterm2/iterm2_shell_integration.${SHELL:5}" | cat >> $ZDOTDIR/.zshrc



#### Set this up on other servers you commonly ssh to

curl -L https://iterm2.com/shell_integration/${SHELL:5} \
-o ~/.iterm2_shell_integration.${SHELL:5}
source ~/.iterm2_shell_integration.${SHELL:5} 
echo "source ~/.iterm2_shell_integration.${SHELL:5}" | cat >> ~/.profile
# not using .bash_profile as that prevents .profile from being run


#### set up iTerm2 shell integration remotely

# use this if they are behind a firewall 
# or simply because its quick to set up multiple servers

REMOTE_SERVER=user@myserver
curl -L https://iterm2.com/shell_integration/bash | ssh ${REMOTE_SERVER} 'cat > ~/.iterm2_shell_integration.bash'
echo "source ~/.iterm2_shell_integration.bash" | ssh ${REMOTE_SERVER} 'cat >> ~/.profile'
# not using .bash_profile as that prevents .profile from being run
```

When you log onto a server with Shell Integration active, the prompt will have a small light-blue triangle at the start. 

### install OhMyZsh

```
setopt interactive_comments

#### check the current version and that its default shell
# help https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH
zsh --version
echo $SHELL
which zsh
chsh -s (`which zsh`)
# NB this will not change $SHELL until you log back in

#### install options
# https://github.com/ohmyzsh/ohmyzsh/blob/master/tools/install.sh
export ZSH=${PEP_SCRIPT_USER}/oh-my-zsh
export ZSH_CUSTOM=${ZSH}/custom
export KEEP_ZSHRC=yes

#### install OMZ
# credit https://github.com/ohmyzsh/ohmyzsh/#via-curl
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

#### install theme
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM}/themes/powerlevel10k


#### customise Terminal startup
cat >> ${ZDOTDIR}/.zshrc <<EndOfConfigZshRC

##### ZSHRC entries for OhMyZsh
# if you want to look at the latest version of the zshrc template 
# https://github.com/ohmyzsh/ohmyzsh/blob/master/templates/zshrc.zsh-template

# Path to your oh-my-zsh installation.
export ZSH=$ZSH

# name of the theme to load
ZSH_THEME="powerlevel10k/powerlevel10k"

# Which plugins would you like to load?
# Standard plugins can be found in $ZSH/plugins/
# Custom plugins may be added to $ZSH_CUSTOM/plugins/
# Add wisely, as too many plugins slow down shell startup.
plugins=(git iterm2 macos)

# No longer required
## 'avoid' for now the issue with perms on /usr/local/share/zsh
## mentioned during OMZ install
#ZSH_DISABLE_COMPFIX=true

source \$ZSH/oh-my-zsh.sh

# You are encouraged to put your custom aliases into the ZSH_CUSTOM folder



##### Other terminal personalisation

# allow the bash-like hash comments on the prompt
setopt interactivecomments

# add the iTerm shell integration you just downloaded
source \${PEP_SCRIPT_USER}/iterm2/iterm2_shell_integration.zsh

# don't page if less than one screen, e.g. for commands like git branch
export LESS="$LESS --no-init --quit-if-one-screen"
# credit https://stackoverflow.com/q/48341920#comment92614882_48370253


EndOfConfigZshRC


# if you prefer to have the string $HOME instead of the actual 
# folder name in this file then see the sed trick in 
# https://github.com/ohmyzsh/ohmyzsh/blob/master/tools/install.sh#L333

```


* close and reopen the terminal to see it all working
	* this may ask you to configure your preferred powerlevel10k settings
	* this may make some changes to the zshrc file
* OMZ will automatically check fortnightly for updates
	* to upgrade manually for the latest code use `omz update`



### Shell personalisation

```
#### ^L to clear screen also clears scroll-back buffer

cat >> ${ZDOTDIR}/.zshrc <<EndOfConfigZshRC

# include shell personalisation through Zle Zsh Line Editor
source \${ZDOTDIR}/.zle

EndOfConfigZshRC

cat >> ${ZDOTDIR}/.zle <<EndOfConfigZle

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

```

### Gnu Utils

The coreutils and findutils have been installed by brew, 
but you can use them instead of macOS / FreeBSD equivalents, 
e.g. ls instead of gls or find instead of gfind, 
to allow seemless switching between mac and linux hosts. 
Also take the latest openssh so that you can generate ed25519 keys

```
brew install coreutils findutils gnu-sed openssh

cat >> ${ZDOTDIR}/.zshrc <<EndOfConfigZshRC

# prefer GNU utilities over FreeBSD variety that come with macOS
export PATH="/usr/local/opt/coreutils/libexec/gnubin:/usr/local/opt/findutils/libexec/gnubin:/usr/local/opt/gnu-sed/libexec/gnubin:/usr/local/bin:\$PATH"

EndOfConfigZshRC
```

If you find that putting /usr/local/bin in front causes any other contentions, then you may need another hack to put the openssh executables ssh* in front. You can test ssh utility versions with `ssh -V`.

