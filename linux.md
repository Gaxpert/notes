---
title: MyLinux
date: 2020-03-31 11:02:19+01:00
author: Gaxpert
---

### MyLinux

Find keys to set / swap  ` grep -E "alt" /usr/share/X11/xkb/rules/base.lst`  

Swaps `setxkbmap -option ctrl:rctrl_alt`.
Reset `setxkbmap -option`

Find key codes `xev`

Activate xmodmap `xmodmap ~/.Xmodmap`

Remove beep sound `xset -b`   
OR
  `echo "blacklist pcspkr"  >> /etc/modprobe.d/blacklist.conf`

##Backup

rsync. a: to archive --delete: to delete files deleted on original aswell
`rsync /path/to/file /backup/folder -a `

#Files

gnome-disk for hdds encript/mount etc

filelight to visualize data

    Encrypt:
gpg --cipher-algo AES256 --symmetric filename.tar.gz

    Decrypt:
gpg --output filename.tar.gz --decrypt filename.tar.gz.gpg

    Destroy file in hdd
shred -zvu -n  5 FILE

or better

semem for ram wipe
secure-erase

Swap keys:
Get list of keys to swap
	> grep -E '(ctrl|caps):' /usr/share/X11/xkb/rules/base.lst
Swap control to caps
	> /usr/bin/setxkbmap -option 'ctrl:nocaps'

##Linux general

Clean weird filenames in folder `detox -r -v /path/to/your/files`

Curl post req with json
curl -d '{"key1":"value1", "key2":"value2"}' -H "Content-Type: application/json" -X POST http://localhost:3000/data

###Swap

###Create

Allocate swap `sudo fallocate -l 1G /swapfile`

Set permissions `sudo chmod 600 /swapfile`

Format as swap `sudo mkswap /swapfile`

Activate as swap `sudo swapon /swapfile`

If want to make changes permanent, edit /etc/fstab and add

`/swapfile swap swap defaults 0 0`

###Remove swap

`sudo swapoff -v /swapfile`

`sudo rm /swapfile`

or remove in /etc/fstab

###Modify swap file size

`sudo fallocate -l 4G /swapfile`

##Faster read/write with ram

`mkdir -p /mnt/ram`

`mount -t tmpfs tmpfs /mnt/ram -o size=8192M`

###Dont save comman on history

Leading space at start `ls` vs  ` ls `

###Open last command in editor

In case of wanting to modify a long command, `fc` opens the previous command on editor


###Check disk usage of every dir

`du`

###Check file system erros

Note: Never use fsck on a mounted filesystem since it may modify data and crash

`fsck`

###Check file or file system info

`stat`

###passwd or shadow

Second field is password
- 'x' indicates the password is stored in /etc/shadow instead of passwd
- '*' indicates user cannot login
- :: empty. Anyone can login

Third field is user id
Fourth is group id

###Groups

Located in 
> /etc/groups

To see groups user belongs 
`groups`

###Time

Pc hardware has a battery-backed real-time clock (RTC)
Reset RTC kernel UTC clock
`hwclock --hvtosys --utc`

###Cron
`15 09 * * * /bin/command`
1. Minute
2. Hour
3. Day of month
4. Month
5. Day of week
6. Command to run


###PAM authentication
Pluggable Authentication Module, located at /etc/pam.conf
`auth requisite pam_shells.so`

1. Function type (auth, account, session or password)
2. Control Argument
	* sufficient: If it succeeds, auth successful. If it fails, proceed to additional rules
	* requisite: If it succeeds, proceed to additional rules. If it fails, auth is unsuccesful and doesn't look at more rules
	* required: If it succeeds, proceed to additional rules. If it fails, proceeds to additional rules BUT will auth will be unsucesful regardless
3. Module


#Kernel 

###Check kernel boot settings

`cat /proc/cmdline`

###Memory Management Unit (MMU)
Kernel assist MMU by breaking down the memory into "pages". The kernel maintains a data structure called a "page table" that containts a mapping of a process virtual page address.
Page work on demand, or on-demand paging, loads and allocates pages only when needed.

###Page Faults
####Minor
Occurs when the desired page is in the main memory but MMU doesn't know where.
Usually the process can continue without problem.
####Major
Occurs when the desired page isn't in memory at all

###Monitor system performance
`vmstat`
By partition
`vmstat -d`
###Monitor I/O performance
`iotop`
###Packages for monitoring and performance analysis
sar (System Activity Reporter)
acct(Process accounting)
Quotas: Limit system resources per-process or per-user (see /etc/security/limits.conf)
##init

###systemd

Is goal oriented. You define a target to achieve along with its dependencies.

####Show current systemd configuration search path

`sudo systemctl -p UnitPath show`

####Check if service is enabled 

`sudo systemctl is-enabled tor.service`

####List all services

`sudo systemctl  list-unit-files --type service`

###Upstart

Is reactionary. Receives events and runs jobs based on those events, which in turn generate more events. 	

###Processes
####lsof

List open files and processes using them
`lsof -p PID`
####Adjusting process priorities
The kernel runs each process with a priority level from -20 to 20 (-20 is the highest prio)

NI --> Nice value, hint at kernel scheduler
Default is 0, it might be useful to swap it in case of big computations so system doesn't lag
`renice 20 PID`

Increasing to 20, PID will have LOWEST prio, not interfering with system functions

##Xorg
The X server uses X input extension to manage input from different devices.
The X input extension creates a "virtual core" that funnels device input to the X server. The core is called the master; the physical devices that you plug in are the slaves

Information about window
`xwininfo`

Get a list of all window IDs and clients
`xlsclients -l`

Get events, input from mouse and keyboard(get keycodes)
`xev`
Attach to window
`xev -id ID`

List inputs
`xinput --list`
List input properties
`xinput --list-props ID`
Change properties
`xinput --set-prop`

###Manipulate keyboard
X internal keyboard mapping
`xmodmap`
The more recent is xkbcomp
`setxkbmap`

##Utils
base64 decode
`base64 -d FILE`
Peak inside archive
`file -z FILE`

##Network
Check public ip
`curl ifconfig.co`



##openssl
Generate self-sugned(x509)
Rsa key
	> openssl genrsa -out server.key 2048
Or ECDSA key
	> openssl ecparam -genkey -name secp384r1 -out server.key

Generation of self-signed(x509) public key (PEM-encodings .pem|.crt) based on the private (.key)
	> openssl req -new -x509 -sha256 -key server.key -out server.crt -days 3650 

#Nvidia

Nvidia tips
Check status
	> nvidia-smi 

#convert mkv to mp4
ffmpeg -i input.mkv -codec copy output.mp4

SOME PROBLEM WITH COLORS (ex vim)
export TERM=xterm-color

#check installed nvidia driver
nvidia-smi

    change default app
xdg-mime default .desktop FILES
#Using mimeo
mimeo -m FILE.jpeg
mimeo --add FILETYOE APP.desktop


    .desktop
Type=Application
Name=Brave
Path=/usr/bin/X
Exec=/usr/bin/X
Icon=X
Terminal=false

validate entry with
desktop-file-validate X.desktop

    No unit found error
#find .service file of unit
find / -name UNIT.service
#enable unit
systemctl enable /usr/lib/UNIT/UNIT.service

    SSh permissions
Typically you want the permissions to be:

.ssh directory: 700 (drwx------)
public key (.pub file): 644 (-rw-r--r--)
private key (id_rsa): 600 (-rw-------)
lastly your home directory should not be writeable by the group or others (at most 755 (drwxr-xr-x)).

    Open huge txt files
glogg
    Huge csv on cmd
xsv


###Permission denied error in copy file
Check group of file. To change group (in case its in a different user)
`chown USER FILE`

#From scratch

With NVidia 2060 RTX 
`sudo pacman -S nvidia` 

`nvidia-xconfig`

`sudo pacman -S lib32-nvidia-utils lib32-nvidia-libgl lib32-mesa-demos libva-vdpau-driver

`

### Exploitation
In linux, if we get different addresses for our stack, it probably means that we have the `grsecurity patch``
