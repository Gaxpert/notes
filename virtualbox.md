---
title: virtualbox
date: 2020-04-08 04:59:12+01:00
author: Gaxpert
---

# virtualbox

## Arch host and guest

NOTE: virtualbox and virtualbox modules must be the same version on host and guest

### In Host

for linux standard kernel -> 'virtualbox-host-modules-arch'

In case of manually load module `modprobe vboxdrv`

To use USB ports of host machine in vms, add users to 'vboxusers' group

### In Guest

for linux standard kernel --> 'virtualbox-guest-utils' and 'xf86-video-vmware'

enable vbox modules 'vboxservice.service'

In case of manually load needed `modprobe -a vboxguest vboxsf vboxvideo`

Launch virtualbox guest services 
```
$ VBoxClient --clipboard
$ VBoxClient --draganddrop
$ VBoxClient --seamless
$ VBoxClient --display
$ VBoxClient --checkhostversion
$ VBoxClient --vmsvga-x11
```

### Shared folders

Manual mounting

`mount -t vboxsf -o gid=vboxsf shared_folder_name mount_point_on_guest_system`

ex:  `mount -t vboxsf -o gid=vboxsf SHARED_VMS ~/SHARED_VMS`



