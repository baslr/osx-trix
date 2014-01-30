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

    /private/etc/nsmb.conf

testet for 10.9

# Editing Apple-Apps

## codesign
As soon as you edit any .plist etc. in an original Apple-App it crashes after editing the .plist, because every App is signed by Apple. To sign the edited App (e.g *Boot Camp Assistant*) get root and type:

    $ sudo codesign -fs - /Applications/Utilities/Boot\ Camp\ Assistant.app

only necessary for 10.9

## Make BootCamp Assistant create a bootable USB-drive on Mac's with optical drive.
1. Edit the info.plist located in: */Applications/Utilities/Boot Camp Assistant.app/Contents*.
2. Search for the string *<key>PreESDRequiredModels</key>* and paste your Model Identifier e.g. *<string>MacBookPro7,2</string>* (You can find the Identifier in Mactracker or by left clicking the Apple while holding down the "alt" Key and clicking "Systeminformation")
3. Copy your Boot-Rom-Version form the System-Profiler, which you start by left clicking the Apple while holding down the "alt" Key and clicking "Systeminformation" and paste it in the info.plist at *<key>DARequiredROMVersions</key>*
4. To make your Mac boot from USB just delete the "Pre" from *<key>PreUSBBootSupportedModels</key>* and add your Model Identifier
5. Codesign your BootCamp Assistant
