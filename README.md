osx-trix
========

Patches, Fixes and Tricks for Apples OS X Platform

# Finder
## show 'Quit' in the Menubar
This will enable CMD + Q and in the menubar Finder -> Quit Finder

    $ defaults write com.apple.Finder QuitMenuItem 1

testet for 10.9

# Networking

## SMB (samba)
This will speedup smb (samba) connections. Write a file with:

```
[default]
minauth=none
streams=no
soft=yes
notify_off=yes
port445=no_netbios
```

to

    /private/etc/nsmb.conf

testet for 10.9
