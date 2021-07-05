# arch-installation-guide
this is an arch installation guide
this is based on the arch wiki installation guide
**note: this installation will only guide you to install the vanila arch linux with CLI.
![image](https://user-images.githubusercontent.com/51907689/111893773-03218300-8a40-11eb-9343-d3cc47674d55.png)
# The start of installation
## the installation must have ethernet connected

- ping google.com "you must know if you have an internet connection"

- timedatectl set-ntp true " this timedatectl is to configure the time and date"
- timedatectl status
- fdisk /dev/sda "fdisk use to modify the disk partion. We will create a new partition table."
- G
    - create new gpt partition table
- N
    - add new partition
- 1
    - first partitions - efi partitions
        - first -default
        - last - +550M
- 2
    - second partions
        - first - default
        - last- +2G
- 3
    - 3rd
        - first default
        - last - all the space "you can just hit enter"
- t
    - first partition must be efi file system
        - 1 is for efi file system
    - second partition muse be linx swap
        - 19 is for linux swap
- w
    - to write the partition table
- mkfs.fat -F32 /dev/sda1
- mkswap /dev/sda2
- swapon /dev/sda2
- mkfs.ext4 /dev/sda3
- mount /dev/sda3 /mnt
- pacstrap /mnt base linux linux-firmware
- genfstab -U /mnt >> /mnt/etc/fstab
- arch-chroot /mnt
- ln -sf /usr/share/zoneinfo/Asia/Manila /etc/localtime
- hwclock --systohc
- pacman -S nano
    - nano is an text editor
- nano /etc/locale.gen
    - uncomment `en_US.UTF-8 UTF-8` and en_PH
- locale-gen
- nano /etc/hostname
    - type the hostname `wesley`
- sudo nano /etc/hosts
    - input *you can change the wesley into your hostname*
        

```
127.0.0.1 localhost
::1 localhost
127.0.1.1 wesley.localdomain wesley
```

- psswd
	- add new password
	- retype new password
- useradd -m wesley
- passwd wesley
	- add new password
	- retype new password
- usermod -aG wheel,audio,video,optical,storage wesley
- pacman -S sudo
- EDITOR=nano visudo
	- uncoment %wheel ALL=(ALL) ALL
- pacman -S grub
- pacman -S efibootmgr dosfstools os-prober mtools
- mkdir /boot/EFI
- mount /dev/sda1 /boot/EFI
- grub-install -- target=x86_64-efi --bootloader-id=grub_uefi --recheck
- grub-mkconfig -o /boot/grub/grub.cfg
- pacman -S networkmanager vim firefox git 
- systemctl enable NetworkManager
- exit
- umount -l /mnt
- sudo reboot
# this installation does not have graphical interface, it only have CLI(command line input) interface
## In the new update they will have an installation guide
## just type this:
python -m archinstall guided
