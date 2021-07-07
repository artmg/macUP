# macOS networking

This part of the [macUP macOS setup process](https://github.com/artmg/macUP/) focuses on: 

* initial configuration settings
* diagnostics
* advanced configuration

[macUP]() is a public repository 
used to install and configure a macOS device in an efficient, uniform and comprehensive way.Any sensitive or personal settings should be kept separately in your own *Private Configuration Notes*.


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
