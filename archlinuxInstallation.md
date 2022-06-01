fdisk -l

fdisk <your disk>

n  -> for new partition

+512M

t -> mode 1

n -> for new partition and all default

w -> to save changes

mkfs.fat -F32 /dev/sda1

mkfs.ext4 /dev/sda2

ping google.com

pacman -Syy

pacman -S reflector

cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak

reflector -c "US" -f 12 -l 10 -n 12 --save /etc/pacman.d/mirrorlist

mount /dev/sda2 /mnt

pacstrap /mnt base linux linux-firmware vim nano

genfstab -U /mnt >> /mnt/etc/fstab

arch-chroot /mnt

timedatectl set-timezone Europe/Amsterdam

vim /etc/locale.get  -> uncomment the desired one

locale-gen

echo LANG=en_US.UTF-8 > /etc/locale.conf

export LANG=en_US.UTF-8

echo arch-linux /etc/hostname

touch etc/hosts
  - 127.0.0.1 localhost
  - ::1 localhost
  - 127.0.1.1 arch-linux

passwd  -> set root password

pacman -S grub efibootmgr

mkdir /boot/efi

mount /dev/sda1 /boot/efi

grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi

grub-mkconfig -o /boot/grub/grub.cfg

pacman -S xorg sddm

pacman -S --needed plasma kde-applications

pacman -S networkmanager

systemctl enable sddm

systemctl enable NetworkManager.service

vim /usr/lib/sddm/sddm.conf.d/default.conf

  [Theme]
  # current theme name
  Current=breeze

pacman --sync sudo

useradd --create-home abeldlp

passwd abeldlp

usermod --append --groups wheel abeldlp

visudo => uncomment # Uncomment to allow members of group wheel to execute any command %wheel ALL=(ALL) ALL

shutdown now

//Possible solutions for resolution problem on virtual machine
sudo pacman -S xf86-video-vmware xf86-vide-nouveau

Disable Kscreen in background services 
