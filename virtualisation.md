
This is about using virtualisation technologies on macOS to run software in different environments without impacting your own working desktop environment.

see also:

* https://github.com/artmg/lubuild/blob/master/help/configure/virtual-guest.md
	* Using virtualisation technologies on Linux systems
* https://github.com/artmg/lubuild/blob/master/help/configure/Windows.md
	* Using Windows OS inside a virtual machine

Since macOS 11 Big Sur, Apple has it's own built in hypervisor, [Apple macOS virtualization framework](https://developer.apple.com/documentation/virtualization), formerly known as HVF (HyperVisor Framework).

## UTM

UTM is a free and open source front-end for QEMU, although it also offers a paid App Store version for those who want to contribute to the project. It offers a modern interface, following Big Sur styling, and is aimed at hosting on macOS only. 

UTM claims close to bare-metal performance for ARM guests on Apple Silicon, as QEMU can now use Apple's Virtualisation Framework as the hypervisor . Although Podman (below) earlier switched from QEMU to vfkit to improve [performance](https://news.ycombinator.com/item?id=33538397) and to avoid the cost of maintaining a [multi-million-line codebase]( [intro deck to vfkit](https://archive.fosdem.org/2023/schedule/event/govfkit/attachments/slides/5847/export/events/attachments/govfkit/slides/5847/fosdem2023_go_devroom_vfkit.pdf)), QEMU now with HVF support is a reasonably efficient solution for the end user. However, many of the using the Apple Virtualization backend means some UTM features are unavailable, like USB and Clipboard sharing, Dynamic display resolution and Save states.

UTM also supports emulation, which you might find fine for lighter x64-86 linux distros. You might find that using  Windows x64-86 [developer Evaluation images](https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/) under this emulation are far too sluggish. See below for using Windows on Apple Silicon.

```zsh
brew install utm
```

* Launch UTM
* Create New VM
* Virtualise
* macOS 12+
* under Import IPSW simply Continue to auto download
* Hardware - adjust if required - Continue
* Storage Size
	* defaults to 64GB
	* APFS sparse file should only take the space actually required 
	* you _might_ be able to [resize](https://github.com/utmapp/UTM/issues/4186)
* Summary - enter a meaningful name
* Save
	* the IPSW download will begin
	* it may be around 15GB so find something else to while away the time

You will find local files in `~/Library/Containers/com.utmapp.UTM/Data/` or `~/Library/Containers/UTM/Data/`. The VMs in and the IPSW files in `./Library/Caches`

* Start the VM
* click OK to Confirm you want to install (overwrite)
* the installation disk copy begins, taking perhaps a minute or three
* [[about_setup#Base install]]


## Podman

Although Docker created many of the standards, and much of the market, for containerisation, installing it and using it on macOS has never been a simple, smooth or pain-free experience. IBM RedHat has produced and is actively developing and marketing its Podman as a Docker-compatible containerisation environment without the shackles, nor the overhead. 

Open source Podman has cross-platform support, and wide package-manager availability. One of the reasons containers are more challenging on macOS and Windows is because the containerisation concept has been built right into the Linux kernel, so the most effective way to use many container stacks is to run them in a Linux OS inside a virtual machine guest. On macOS specifically, Podman uses the underlying [Apple macOS virtualization framework](https://developer.apple.com/documentation/virtualization) via the open-source [vfkit](https://github.com/crc-org/vfkit) command line tool, making it an extremely efficient and surprisingly lightweight implementation compared with other virtualisation and containerisation systems available on macOS. See an [intro deck to vfkit](https://archive.fosdem.org/2023/schedule/event/govfkit/attachments/slides/5847/export/events/attachments/govfkit/slides/5847/fosdem2023_go_devroom_vfkit.pdf).

### Installing Podman on macos

NB: if you run your services purely on one single machine, you should **deny** the firewall access to `podman-remote`. 

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

By default, ports exposed by containers will only be accessible by other containers in the machine. The -p --ports option exposes these to the `localhost` so you can make connections from your pc.

```zsh
# check
podman machine list
# clear up
podman machine stop
# remove all machines and files INCLUDING image caches
# podman machine reset
```


## Rosetta 2

Although not strictly a virtualisation technology, it is often mentioned in conversations around virtualisation. Rosetta in macOS is an architecture translation (supposedly not 'emulation') system, and Rosetta 2 is the component which may be installed to allow x86_64 'intel' software to run on Apple Silicon 'ARM' processors.

Best practice development for macs is to compile a 'universal' (or fat) binary, which contains code for multiple architectures inside, so the OS runs the one that matches the computer's processor architecture. When an application without a universal binary is installed, Rosetta 2 attempts [ahead-of-time compilation](https://en.wikipedia.org/wiki/Ahead-of-time_compilation "Ahead-of-time compilation") (AOT) to fully translate the x86-64 code one single time, which provides the best performance. If that is not possible then it will use [just-in-time](https://en.wikipedia.org/wiki/Just-in-time_compilation "Just-in-time compilation") (JIT) translation on each execution, and users are likely to find its performance more sluggish.

If you install an application that requires translation then [macOS will prompt](https://support.apple.com/en-gb/102527) the user to install Rosetta. Likewise some subsystems (like vfkit that podman uses below) will offer to install it if it might be considered beneficial. Alternatively it can be installed from the Terminal command line. It would appear there is no effective method to completely remove the software, short of reinstalling macOS, but it is unlikely to cause an impact when it does not need to translate applications. There are some [limitations to its abilities](https://apple.stackexchange.com/a/450602) and [some reports of apps that perform badly](https://www.reddit.com/r/MacOS/comments/sgc2vp/how_does_rosetta_2_impact_general_system/) so you might want to consider alternative workarounds for any given application requirement.

An easy way for an application to [detect if it is being translated](https://stackoverflow.com/a/65347893) is `sysctl sysctl.proc_translated`. Under the covers Rosetta 2 is called AOH and you can [read a bit more](https://eclecticlight.co/2021/01/22/running-intel-code-on-your-m1-mac-rosetta-2-and-oah/) or see [some reverse-engineering](https://ffri.github.io/ProjectChampollion/part1/) if you really wish.

## Windows on Apple Silicon

As explained in UTM above, you can easily use Windows Intel images on older Intel Mac computers. If you have the newer Apple Silicon Mac models, you will need Windows Arm. Microsoft have no plans to release developer ARM builds any time soon, but you have some ways to obtain Arm images.

MS allow the Windows 11 ESD files to be downloaded from directly inside commercial macOS virtualisation software like Parallels and VMware Fusion. Now that Broadcom has opened Fusion up to individuals for Personal Use, you can get this quickly and easily:

```zsh
brew install vmware-fusion
```

with a Windows 11 Pro license you can get Windows for ARM images that give great performance. Alternatively, unless you are on the Windows Insider Preview programme for corporates, you would have to trust [download method](https://docs.getutm.app/guides/windows/) whose source for obtaining the ISO does not look reliably legitimate.

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


