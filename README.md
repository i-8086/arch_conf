# Arch min non-uefi install and config

## Create partitions
list available disks
`fdisk -l`

select disc you are going to use for install

`fdisk /dev/sda`

	command n - new partition
	
	command p - primary
	
	command 1 - partition number
	
	default first and last sectors to use entire hdd
	
	command w - write to disk
	
	
create filesystem

`mkfs.ext4 /dev/sda1`

## Prepare setup

connect to wifi

`wifi-menu`

select mirror

`pacman -Syy`

`pacman -S reflector`

`cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak`

`reflector -c "Bulgaria" -f 12 -l 10 -n 12 --save /etc/pacman.d/mirrorlist`


## Install Arch

`mount /dev/sda1 /mnt`

`pacstrap /mnt base base-devel linux linux-firmware vim nano`

## Configure installed Arch system

generate fstab file to auto mount partitions

`genfstab -U /mnt >> /mnt/fstab`

enter mounted disk as root

`arch-chroot /mnt`

### set timezone

list timezones

`timedatectl list-timezones`

set timezone

`timedatectl set-timezone Europe/Belgrade`

### set locale

uncoment prefered language from * /etc/locale.gen * file

	en_US.UTF-8
	
	en_US.ISO-something

generate locale config

	locale-gen
	
	echo LANG=en_US.UTF-8 > /etc/locale.conf
	
	export LANG=en.US.UTF-8
	
### network config

create hostname file in /etc dir

`echo arch > /etc/hostname`

`touch /etc/hosts`

`vim /etc/hosts`

```
127.0.0.1		localhost
::1			localhost
127.0.1.1		arch
```

### set up root passwd
`passwd`

## Install grub

`pacman -S grub`

`grub-install /dev/sda`

`grub-mkconfig -o /boot/grub/grub.cfg`

## Create user

uncomment wheel group from sudoers file (wheel ALL=(ALL) ALL)

`sudo EDITOR=nano visudo`

add user

`sudo useradd -m <user_name> -p <password>`

add user to wheel, audio, video group

`sudo usermod -aG wheel,audio,video <user_name>`

install network manager

`sudo pacman -S networkmanager`

enable network manager service

`sudo systemctl enable NetworkManger.service`

install git

`sudo pacman -S git`

### REBOOT SYSTEM

login to your accout

clone repository

`git clone https://github.com/i-8086/arch_conf.git`

navigate to arch_conf dir

`cd arch_conf`

enable execution of install script

`chmod +x install_pkg`

run script

`./install_pkg`

### REBOOT and it's done

