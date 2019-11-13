---
layout: post
title:  "misadventures in arch"
date:   2019-11-11 23:06:33 +0530
tags: arch security OS disk encryption
---


A few days ago, I hit Control-C in the middle of a `pacman -Syu` by mistake. And of course, as luck would have it, it was during the kernel build step. 
I reran the command and thought nothing of it.

Until yesterday; when I rebooted my system, I decrypted grub and ran into this error 
`Error loading \vmlinuz-linux: not found`


My setup follows the ArchWiki's LUKS on LVM [entry](https://wiki.archlinux.org/index.php/Dm-crypt/Encrypting_an_entire_system#LUKS_on_LVM) to create an [encrypted bootloader](https://wiki.archlinux.org/index.php/Dm-crypt/Encrypting_an_entire_system#Encrypted_boot_partition_(GRUB)) (grub), root and swap disks.
The error showed up after decrypting grub, and was resolved with a live USB running [chroot](https://wiki.archlinux.org/index.php/Chroot).

This involved 
* decrypting the boot and LVM partitions 
```
cryptsetup open /dev/sdaX cryptmapper
```
* mounting them and the efi partition,
```
mount /dev/MyVolGroup/root /mnt 
```
* accessing chroot,
* reinstalling the kernel with a `pacman -S linux`

And voila, I had my system restored to its previous splendor. It was also good to know that the full disk encryption didn't lock me out (although I did have a few seconds of panic).

For my future self to remember how the mount points work -
```
NAME                  MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
sda                     8:0    0 223.6G  0 disk  
├─sda1                  8:1    0   550M  0 part  /boot/efi
├─sda2                  8:2    0   200M  0 part  
│ └─encryptedBOOT     254:4    0   198M  0 crypt /boot
└─sda3                  8:3    0 222.9G  0 part  
  └─cryptoluks        254:0    0 222.9G  0 crypt 
    ├─MyVolGroup-root 254:1    0    40G  0 lvm   /
    ├─MyVolGroup-swap 254:2    0     8G  0 lvm   [SWAP]
    └─MyVolGroup-home 254:3    0 174.9G  0 lvm   /home
```
