# macOS terminal

This part of the [macUP macOS setup process](https://github.com/artmg/macUP/) focuses on: 

* using different terminal software
* local and remote command line interpreter (CLI) access

see also:

* [application_installation.md](application_installation.md) 
	* how to install and configure some of the utilities mentioned here

[macUP]() is a public repository 
used to install and configure a macOS device in an efficient, uniform and comprehensive way.Any sensitive or personal settings should be kept separately in your own *Private Configuration Notes*.

**TTD:** move terminal setup out from [application_installation.md](application_installation.md)


## iTerm2

* open-source project that is currently active
* available via package managers
* extensible
* 

## Zee-shell (zsh)

* how to get used to the move from bash?

mention of oh-my-zsh 

## Multiplexing

Probably the greatest benefit of terminal multiplexers like `screen` and `tmux` is that 
they keep a session going when you loose the connection. If, for instance, your `ssh` shell 
gets disconnected by the network, then you don't 
have to start all over again. 

Although `screen` is simple, popular, lightweight, 
and often bundled in with an operationg system, 
we will look at **tmux** here because a) it is more actively developed, but mainly b) it integrates with iTerm2. 

NB: many of the commands shown here are for Debian Linux, as you will tend to run them on your remote servers, not the macOS client you connect from. 

To send commands to the multiplexer you use 
its attention keystroke (Ctrl-A for screen, 
Ctrl-B for tmux) followed by commands. 

### iTerm2 tmux integration

if you create your session using tmux -CC or 

`tmux -CC new -S myname`

then iTerm integration will split this into a separate-looking windows, which avoids some of 
the issues of keystroke clashes. It does not however 
have the status bar at the bottom. 


### late muxing

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

