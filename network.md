# macOS networking

This part of the [macUP macOS setup process](https://github.com/artmg/macUP/) focuses on: 

* initial configuration settings
* diagnostics
* advanced configuration

[macUP]() is a public repository 
used to install and configure a macOS device in an efficient, uniform and comprehensive way.Any sensitive or personal settings should be kept separately in your own *Private Configuration Notes*.

## Configuration

## Diagnostics

Includes some workarounds in the form of advanced configuration

### General network diagnostics

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

