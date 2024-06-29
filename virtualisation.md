
This is about using virtualisation technologies on macOS to run software in different environments without impacting your own working desktop environment.

see also:

* https://github.com/artmg/lubuild/blob/master/help/configure/virtual-guest.md
	* Using virtualisation technologies on Linux systems
* https://github.com/artmg/lubuild/blob/master/help/configure/Windows.md
	* Using Windows OS inside a virtual machine

## Podman

Although Docker created many of the standards, and much of the market, for containerisation, installing it and using it on macOS has never been a simple, smooth or pain-free experience. IBM RedHat has produced and is actively developing and marketing its Podman as a Docker-compatible containerisation environment without the shackles, nor the overhead. 

Open source Podman has cross-platform support, and wide package-manager availability. One of the reasons containers are more challenging on macOS and Windows is because the containerisation concept has been built right into the Linux kernel, so the most effective way to use many container stacks is to run them in a Linux OS inside a virtual machine guest. On macOS specifically, Podman uses the underlying [Apple macOS virtualization framework](https://developer.apple.com/documentation/virtualization) via the open-source [vfkit](https://github.com/crc-org/vfkit) command line tool, making it an extremely efficient and surprisingly lightweight implementation compared with other virtualisation and containerisation systems available on macOS. 

### Installing Podman on macos

NB: running on a single machine, you should **deny** the firewall access to `podman-remote`. 

```zsh
brew install podman
# this is a nice lightweight package
# but it will load a 1GB image when you init below

# if you need to change the file locations see
podman machine info
# see under podman's covers: https://www.redhat.com/sysadmin/run-containers-mac-podman

# mv $HOME/.local/share/containers/podman $HOME/Cached/Images/podman
# ln -s $HOME/Cached/Images/podman $HOME/.local/share/containers/podman

# create the linux VM to run podman, then start it
podman machine init
podman machine start

# follow the suggested instructions to default to the docker socket
sudo /opt/homebrew/Cellar/podman/5.0.3/bin/podman-mac-helper install
podman machine stop; podman machine start
```


Optional GUI: ` brew install podman-desktop `

### example usage

Once you have started your machine (see above), to run an interactive vm (one you can shutdown with Cmd-C from command line)

```zsh
podman run --rm -it --shm-size=2g -p 4444:4444 -p 5900:5900 -p 7900:7900 selenium/standalone-firefox
```

NB: If you have not already downloaded the image, podman will do this for you on the fly.


```zsh
# check
podman machine list
# clear up
podman machine stop
# remove all machines and files INCLUDING image caches
# podman machine reset
```


## VirtualBox tips

_this was moved IN from [[macUP/misc|misc]] article_ 

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


