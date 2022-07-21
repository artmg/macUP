# macOS networking

This part of the [macUP macOS setup process](https://github.com/artmg/macUP/) focuses on: 

* initial configuration settings
* diagnostics
* advanced configuration

[macUP](https://github.com/artmg/macUP/) is a public repository 
used to install and configure a macOS device in an efficient, uniform and comprehensive way.Any sensitive or personal settings should be kept separately in your own *Private Configuration Notes*.

See also:

* [misc](misc.md) for more general tips

## Configuration

### Reset all network settings

If all else fails, you can wipe and restart your whole network configuration using the plist files mentioned here:

http://otago.custhelp.com/app/answers/detail/a_id/3543/~/network-reset-instructions

This technique leaves them in trash so you can recover them if it doesn't fix the issue.

## Diagnostics

Includes some workarounds in the form of advanced configuration

### General network diagnostics

If you just want to show your IP address, or check the basic wireless or ethernet configuration...

```
# check your connections
ifconfig

# look at the AP cache for MAC addresses of recent connections
arp -a
```

### Wireless

Try holding ⌥ (Option / Alt) key whilst clicking on the wireless icon in your toolbar

```
# show your wifi interfaces
networksetup -listallhardwareports

# show the wifi networks available
airport -s

# or 
/System/Library/PrivateFrameworks/Apple80211.framework/
Versions/Current/Resources/airport -s

# if you can't see the airport command then make a link to it
ln -s /System/Library/PrivateFrameworks/Apple80211.framework/
Versions/Current/Resources/airport /usr/bin/airport
```

### Routing

```
# check the current routes
netstat -rn

# just show net routes for IPv4
netstat -rn -f inet

networksetup -listnetworkserviceorder
```

#### Routing order

Note that macOS routing choices is not weighted by a route priority so much as by range-specificity (prefer a route containing less addresses) and adapter priority

An example use case for changing the order: you have an ethernet cable plugged into a new router whilst you configure it using local-only access, 
but you still want to be able to access internet via your regular wifi connection


```
# check the current service order
networksetup -listnetworkserviceorder

# check routes 
netstat -rn -f inet

# rearrange (needs full service name, not just en0 device id)
networksetup -ordernetworkservices "Apple USB Ethernet Adapter" "USB 10/100/1000 LAN" "Wi-Fi" "Bluetooth PAN"
```

This is the equivalent of System Prefs / Network / (service list drop down) Set Service Order...

Onc you reconnect (e.g. turn the wifi off and on again) the default routes will now be added in the 'right order'.

### Local services

See what is listening on which ports

```
sudo lsof -i -P | grep LISTEN
```

### Comprehensive diagnostic dump

```
NET_IF=en0
ifconfig $NET_IF
ifconfig
arp -a
networksetup -listallhardwareports
netstat -rn -f inet
networksetup -listnetworkserviceorder

NET_SVC=Wi-fi
networksetup -getinfo $NET_SVC
networksetup -getadditionalroutes $NET_SVC
networksetup -getv6additionalroutes $NET_SVC
networksetup -getdnsservers $NET_SVC
networksetup -getsearchdomains $NET_SVC
networksetup -getwebproxy $NET_SVC
ipconfig getpacket $NET_IF

ping 8.8.8.8 -c 4 -t 3

TEST_HOST=www.meter.net
nslookup $TEST_HOST
curl $TEST_HOST
curl -v $TEST_HOST
```

The value for TEST_HOST is not massively significant. Preferably it could be a service you do not normally use, so that the hostname is NOT already in the DNS cache. 

Most hosts, if you get their default page on http, should simply return a 301 redirect to their https site. 

## Issues

### rename on wake

`Computer hostmame already on network`

In some circumstances macOS can automatically change the hostname. It does this to avoid duplicates, for instance if you 
multi-home your device by using both wifi and ethernet onto the 
same subnet (especially if you are using DHCP). 

However it can also do this inadvertently when it comes out of resume.
You may see the message 

```
This computer's hostname is already in use on this network. 
The name has been changed to ...
```

If you are sure that there is no reason why there might be 
another device with that name, you can fix it using **scutil**

```
sudo echo 

originalhostname=`scutil --get ComputerName | cut -f1 -d" "`
echo Setting hostname back to $originalhostname

# check the current settings
for key in LocalHostName ComputerName  HostName  ; do scutil --get $key; done

# set the local host and computer - for now the HostName is unset!
for key in LocalHostName ComputerName  ; do sudo scutil --set $key $originalhostname; done

for key in LocalHostName ComputerName  HostName  ; do scutil --get $key; done
# credit https://apple.stackexchange.com/a/301258
```

### Lost connectivity on wifi change

First noticed in 11.5.2

Changing wireless connection, **especially** at the same time as coming out of suspend, would give connectivity issues. 

Possible issues and checks:

* Connectivity - ping
* routes - 
* DNS - flush
* DHCP - wrong settings e.g. DNS

Even when connectivity is there (e.g. ping 8.8.8.8 works) no name resolution is possible (cannot connect to new sites and nslookup example.com does not get response).

Rebooting fixes the issue, but there must be a more direct method?!

Tried:

* Disable/reenable wifi in menu bar
* chrome://net-internals/#dns - clear host cache
* sudo killall -HUP mDNSResponder
* Network Diagnostics Report cannot be created
* 

to try:

* networksetup -setairportpower en0 off && networksetup -setairportpower en0 on
	* should do same as ifconfig but without sudo
* sudo ifconfig en0 down;sleep 1;sudo ifconfig en0 up
	* sudo networksetup -setnetworkserviceenabled en0 off; sleep 10; networksetup -setnetworkserviceenabled en0 on
	* try flushing route whilst down
	* sudo route -n flush
	* https://apple.stackexchange.com/a/325483
* /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport
* Renew in DHCP
* SysPrefs / Nwck / each service (...) Make Service Inactive
* Create a new Location with all network services disabled, and switch using SysPrefs or `scutil`
	* `man scutil`
* Look at kernel kld options - https://serverfault.com/a/220611


The most useful discussion of working around or diagnosing such issues I have found is at https://serverfault.com/q/140327

