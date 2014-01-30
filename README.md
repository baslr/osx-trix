osx-trix
========

Compilation of Patches, Fixes and Tricks for Apples OS X Platform

# Finder
## show 'Quit' in the Menubar
This will enable CMD + Q and in the menubar Finder -> Quit Finder

    $ defaults write com.apple.Finder QuitMenuItem 1

testet for 10.9

## show full POSIX Path in title
This will enable the full POSIX path in any Finder menu.

    $ defaults write com.apple.finder _FXShowPosixPathInTitle -bool true
    
testet for 10.7 - 10.9

## prevent .DS_Store file creation over network connections
This will prevent the Finder from creating .DS_Store over network connections.

    $ defaults write com.apple.desktopservices DSDontWriteNetworkStores true
    
testet for 10.4 - 10.9


# Networking

## SMB (samba)
This will speedup smb (samba) connections. Write a file (as root) with:

```
[default]
minauth=none
streams=no
soft=yes
notify_off=yes
port445=no_netbios
```

to
```
/private/etc/nsmb.conf
```

testet for 10.9

## throttle TCP/IP Port throughput
* (src|dst) means src-port xor dst-port
* replace {port} with the port number you want to throttle

```
# ipfw pipe 1 config bw 500KByte/s
# ipfw add 1 pipe 1 (src|dst)-port {port}
# ipfw delete 1
```
