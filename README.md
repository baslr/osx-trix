osx-trix
========

Compilation of Patches, Fixes, Tips and Tricks for Apples OS X Platform.

**Pull Requests are welcome.**

# Notes
``$`` means your normal user account.

``#`` means your root account.

# Finder
After any change you made here you have to type *killall Finder*
## show 'Quit' in the Menubar
This will enable CMD + Q and in the menubar Finder -> Quit Finder

    $ defaults write com.apple.Finder QuitMenuItem 1

tested for 10.9

## show hidden files in Finder
This wil show all files in the Finder.

```
$ defaults write com.apple.finder AppleShowAllFiles TRUE
```

tested for 10.4 - 10.9

## show a specific hidden file/directory in Finder
This will show a specific file/directory in the Finder. e.g. The directory ~/Library/.

```
$ chflags nohidden ~/Library/
```

necessary since 10.7

## show full POSIX Path in title
This will enable the full POSIX path in any Finder menu.

    $ defaults write com.apple.finder _FXShowPosixPathInTitle -bool true
    
tested for 10.7 - 10.9

## prevent .DS_Store file creation over network connections
This will prevent the Finder from creating .DS_Store over network connections.

    $ defaults write com.apple.desktopservices DSDontWriteNetworkStores true
    
tested for 10.4 - 10.9

# Testing your Memory
Source: http://rampagedev.wordpress.com/os-x-tweaks/run-memtest-under-mac-os-x/
## with normal RAM usage
1. Download Memtest and install
2. type *memtest all 2* this will test your whole unused memory 2 times

## with minimal RAM usage (recommend)
1. Download Memtest and install
2. restart your Mac and hold down cmd+s while starting
3. Type *memtest all 6* into prompt. To save the results into .txt file you need to mount your volume with 
*# /sbin/mount -uw /* and then run *memtest all 6 > output.txt* this will save all results to the root of the volume.

# Networking

## SMB (samba)
### speedup connection
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

tested for 10.9

## throttle TCP/IP Port throughput
* (src|dst) means src-port xor dst-port
* replace {port} with the port number you want to throttle

```
# ipfw pipe 1 config bw 500KByte/s
# ipfw add 1 pipe 1 (src|dst)-port {port}
# ipfw delete 1
```
tested for 10.8 - 10.9

## disable delayed tcp ack
Source: <http://www.jeremycole.com/blog/2010/01/13/delayed-ack-in-os-x-is-incomprehensible/>
```
# echo 'sysctl -w net.inet.tcp.delayed_ack=0' >> /etc/sysctl.conf
```

# Editing Apple-Apps

## codesign
As soon as you edit any .plist etc. in an original Apple-App it crashes after editing the .plist, because every App is signed by Apple. To sign the edited App (e.g *Boot Camp Assistant*) get root and type:

    # codesign -fs - /Applications/Utilities/Boot\ Camp\ Assistant.app

only necessary for 10.9

## Make BootCamp Assistant create a bootable USB-drive on Mac's with optical drive
Source: https://www.youtube.com/watch?v=5A5FG8EMEGc

1. Edit the info.plist located in: */Applications/Utilities/Boot Camp Assistant.app/Contents*.
2. Search for the string *<key>PreESDRequiredModels</key>* and paste your Model Identifier e.g. *<string>MacBookPro7,2</string>* (You can find the Identifier in Mactracker or by left clicking the -Menu while holding down the "alt" Key and clicking "Systeminformation")
3. Copy your Boot-Rom-Version form the System-Profiler, which you start by left clicking the -Menu while holding down the "alt" Key and clicking "Systeminformation" and paste it in the info.plist at *<key>DARequiredROMVersions</key>*
4. To make your Mac boot from USB just delete the "Pre" from *<key>PreUSBBootSupportedModels</key>* and add your Model Identifier
5. Codesign your BootCamp Assistant

# Animations

## disable Mission Control animation
This disables the swoosh animation when you start Mission Control.
```
$ defaults write com.apple.dock expose-animation-duration -float 0
$ killall Dock
```

# Useful Apps (free)
## Boot Camp 5
http://support.apple.com/kb/dl1638

## flux
Changes the kelvin-temperature of your display when the sun goes down.

## img2icns
Transforms images to icons.

## TextMate
Improved text displaying with syntax highlighting. It's the Notepad++ equal for OS X.

## 0xED, HexEditor
http://www.suavetech.com/0xed/
> 0xED is a native OS X hex editor based on the Cocoa framework.


## BetterTouchTool
http://www.boastr.de
> BetterTouchTool is a great, feature packed FREE app that allows you to configure many gestures for your Magic Mouse, Macbook Trackpad and Magic Trackpad. It also allows you to configure actions for keyboard shortcuts, normal mice and the Apple Remote. In addition to this it has an iOS companion App (BTT Remote) which can also be configured to control your Mac the way you want.

> BetterTouchTool includes many goodies, like window snapping or an integrated window switcher.

## Memtest
http://cdn.command-tab.com/2008/memtest_422.zip (direct)

## buildin http server
source: <http://osxdaily.com/2010/05/07/create-an-instant-web-server-via-terminal-command-line/>

just run
```
$ python -m SimpleHTTPServer 4104
```
in your terminal. It will serve the files and directories in the current directory.


# Peripherals
## Make the SuperDrive work on any Mac
Source: http://www.maclife.de/tipps-tricks/hardware/externes-superdrive-fast-jedem-mac-verwenden

1. Copy the file */Library/Preferences/SystemConfiguration/com.apple.Boot.plist* to your desktop and open it with TextEdit.
2. paste "mbasd=1" bewteen *&lt;string&gt;* and *&lt;/string&gt;*. Save the document and paste it into */Library/Preferences/SystemConfiguration/* replace the old file. Root is needed.
3. Restart your Mac.

## Keyboard changes &gt;/&lt; with ^° Keys
```
# rm /Library/Preferences/com.apple.keyboardtype.plist
```
then reboot

# Clearing Caches
## Removing User-Cache
Just delete the content of the directory: *~/Library/Caches*

## Clearing Font-Cache
Source: http://www.extensis.com/support/de/knowledge-base/clearing-the-mac-os-x-font-cache/
```
# atsutil databases -remove
```

## Clearing DNS-Cache
Source: http://reviews.cnet.com/8301-13727_7-57493543-263/how-to-reset-the-dns-cache-in-os-x/

```
# dscacheutil -flushcache
# killall -HUP mDNSResponder
```

# Terminal

## show my group memberships
```
$ groups {username}
```
The admin user has among others the group ``admin``

The root user has among others the groups ``wheel`` ``daemon`` ``kmem`` ``sys`` ``operator``
