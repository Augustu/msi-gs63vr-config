### Installation

```bash
# download
# https://archlinux.org/download/

# Verify signature
gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig
# or using
pacman-key -v archlinux-version-x86_64.iso.sig

# Prepare an installation medium
sudo dd if=archlinux-version-x86_64.iso of=/dev/sdx

# Verify the boot mode
ls /sys/firmware/efi/efivars

# Connect to the internet
iwctl

# Partition the disks
fdisk -l

# for efi already exists, have installed windows first
# encrypting ref: https://wiki.archlinux.org/index.php/Dm-crypt/Encrypting_an_entire_system#LVM_on_LUKS
# --pbkdf-force-iterations=76384 about 1s for grub unlock on i7-7700HQ
cryptsetup luksFormat /dev/sda1 --pbkdf-force-iterations=76384 --tries=3
cryptsetup open /dev/sda1 Augustu

pvcreate /dev/mapper/Augustu
vgcreate Augustu /dev/mapper/Augustu

lvcreate -L 8G Augustu -n Swap
lvcreate -l 100%FREE Augustu -n Root

# mkfs.f2fs for ssd
mkfs.ext4 /dev/Augustu/Root
mkswap /dev/Augustu/Swap

mount /dev/Augustu/root /mnt
swapon /dev/Augustu/swap
mkdir /mnt/efi
mount /dev/sda1 /mnt/efi
# check if issue 1 occur, fix it now

pacstrap /mnt base linux-hardened linux-firmware

arch-chroot /mnt

# Configuring mkinitcpio
vi /etc/mkinitcpio.conf
# add udev keyboard keymap encrypt lvm2 hooks
HOOKS=(base udev autodetect keyboard keymap consolefont modconf block encrypt lvm2 filesystems fsck)

# do this later if you want a keyfile to avoid enter passphrase twice
mkinitcpio -P

# Avoiding having to enter the passphrase twice
dd bs=512 count=4 if=/dev/random of=/root/cryptlvm.keyfile iflag=fullblock
chmod 000 /root/cryptlvm.keyfile
cryptsetup -v luksAddKey /dev/sda3 /root/cryptlvm.keyfile

vi /etc/mkinitcpio.conf
FILES=(/root/cryptlvm.keyfile)

mkinitcpio -P

chmod 600 /boot/initramfs-linux*

# efi grub boot
# ref: https://wiki.archlinux.org/index.php/Dm-crypt/Encrypting_an_entire_system#Encrypted_boot_partition_(GRUB)
vi /etc/default/grub
GRUB_ENABLE_CRYPTODISK=y
# blkid to get uuid
# use uuid with type: crypto_LUKS
# blkid | grep /dev/nvme0n1p2 >> /etc/default/grub
# y36l p for vim copy uuid
GRUB_CMDLINE_LINUX="... cryptdevice=UUID=device-UUID:cryptlvm ..."

vi /etc/default/grub
GRUB_CMDLINE_LINUX="... cryptkey=rootfs:/root/cryptlvm.keyfile"


grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=ArchLinux --recheck

grub-mkconfig -o /boot/grub/grub.cfg

# Configure the system
# out of arch-chroot with Ctrl+d
genfstab -U /mnt >> /mnt/etc/fstab

arch-chroot /mnt
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

hwclock --systohc

# fix 8 hours different from windows when two systems switch
timedatectl set-local-rtc true

vi /etc/locale.gen
en_US.UTF-8 UTF-8
zh_CN.GB18030 GB18030  
zh_CN.GBK GBK  
zh_CN.UTF-8 UTF-8  
zh_CN GB2312

vi /etc/locale.conf
LANG=en_US.UTF-8
LC_CTYPE="zh_CN.UTF-8"

vi /etc/hostname
Augustu

vi /etc/hosts
127.0.0.1	localhost
::1		localhost
127.0.1.1	myhostname.localdomain	myhostname

# Root password
passwd

# Install gnome
# or try install gnome-tweak only
# pacman -S gnome gnome-tweak
pacman -S gnome gnome-extra
# check if issue 2 occur :)
# if using intel cpu
pacman -s intel-ucode

systemctl enable gdm
systemctl enable NetworkManager

# blacklist nouveau module
vi /etc/modprobe.d/blacklist.conf
blacklist nouveau

exit
reboot

```

ref: https://wiki.archlinux.org/index.php/Installation_guide





#### Issues

1. **Partition table entries are not in disk order**

   ```bash
   fdisk /dev/sdb
   x
   f fix partition order
   p
   w
   ```

   

2. **Pacman -S signature from xxx is unknown trust**

   ```bash
   pacman-key --init
   pacman-key --populate archlinux
   
   # or
   vi /etc/pacman.conf
   SigLevel Never
   ```

   

3. 







