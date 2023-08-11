# Personal Notes For Linux Recovery

## Recover Grub from Live Image

```
$ # Use Live CD to boot
$ sudo su # Switch to root
$ fdisk -l # Get names of root, boot & EFI partition names. you can also use blkid
$ mount /dev/mapper/fedora_localhost--live-root /mnt  # mount root partition
$ cat /mnt/etc/fedora-release
Fedora release 31 (Thirty One)
$ mount /dev/nvme0n1p2 /mnt/boot  # mount boot partition
$ mount /dev/nvme0n1p1 /mnt/boot/efi  # mount EFI partition
# Note: If you are not able to mount EFI partition ('Input/Output error'),
# You may have to repair ESP file system or format ESP.
# fsck.vfat /dev/nvme0n1p1
# mkfs.vfat /dev/nvme0n1p1 
# If formatted then we may have to update UUID at /etc/fstab
$ ls /mnt/boot/efi # should show all OS names under EFI

$ # mount the virtual filesystems that the system uses to communicate with processes and devices
$ for dir in /dev /proc /sys /run; do mount --bind $dir /mnt/$dir ; done

$ # enter chroot
$ chroot /mnt

$ # Now you can do all the work e.g. fix grub
$ dnf reinstall grub2-efi shim -y
$ grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg # Regenerate grub2

$ # Check /etc/fstab UUID, update if necessary [Hint: lsblk -f would show partition UUIDs]

$ # All things done; now exit from chroot
$ exit

$ # Check BIOS boot details [ Note: this command won't work if you are inside chroot. ]
$ efibootmgr -v
$ # In case you need to create new entry in BIOS
$ efibootmgr -c -d /dev/nvme0n1p1 -p 1 -L Fedora -l '\EFI\fedora\grubx64.efi' # or, shimx64.efi
$ # To delete entry from efibootmgr use: efibootmgr -b <#entry> -B
$ # Copy grubx64.efi from Live USB, if required
$ cp -p /boot/efi/EFI/grubx64.efi /mnt/boot/efi/EFI/fedora 

$ exit
$ # Now you can reboot
```