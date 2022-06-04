![Arch linux](logo.png)

<h1 style="color:#1793d1">Arch linux installation guide</h1>

<h3 style="color:#1793d1">📋 Initial setup:</h3>

<details>
  <summary>
    <strong>Pre installation setup</strong>
  </summary>

- `ls /usr/share/kbd/keymaps/**/*.map.gz | less` _list all the keymaps_
- default is `us` in case you want to change keymaps do `loadkeyes de-latin1` _example_
- `timedatectl set-ntp true` _enable ntp_
- `timedatectl status` _check ntp status_

</details>

<details>
  <summary>
    <strong>Connecting to wifi</strong>
  </summary>

You can check if you have internet connection by pining any ip address.

- `iwctl` _gets into the wifi iterface_
- `help` _to see available commands_
- `device list` _to see the available devices_
- `station` _< device > scan_
- `station < device > get-networks` _gets all the networks_
- (Connect on your prefered network)
- `station < device > connect < name >` _then input password_
- `station < device > show` _to see if you are connected_
- `exit`
- `ping archlinux.org` _to check if you are connected_

</details>

<h3 style="color:#1793d1">️🗄️️ Filesystem setup:</h3>

<details>
  <summary>
    <strong>Disk Partitioning</strong>
  </summary>

#### Creating the partitions

- `fdisk -l` _to list all the drives_
- `fdisk /dev/sda` _to partition our drive remember flag `m` for help_
- `g` _to create a gpt partition table_
- `n` _to create a new partition_
- `1` _to create a partition 1 (limit 128)_
- `Enter` _to go with default first sector_
- `+550M` _550M for EFI Partition_
- `n` _to create a new partition_
- `2` _to create a partition 2 (limit 128)_
- `Enter` _to go with default first sector_
- `+2G` _2G for Swap partition_
- `n` _to create a new partition_
- `3` _to create a partition 3 (limit 128)_
- `Enter` _to go with default first sector_
- `Enter` _to go with remaining space_

#### Changing partition types

- `t` _to change partition type `L` to list_
- `1` _to select partition 1_
- `1` _to change to EFI partition_
- `t` _to change partition type `L` to list_
- `2` _to select partition 2_
- `19` _to change to Linux Swap partition_
- `w` _to write all previous changes_

> _Note:_ Partition 3 is already Linux File System by default

</details>

<details>
  <summary>
    <strong>Making the File System</strong>
  </summary>

- `mkfs.fat -F32 /dev/sda1` _to make the EFI partition_
- `mkswap /dev/sda2` _to make the swap partition_
- `swapon /dev/sda2` _to activate the swap partition_
- `mkfs.ext4 /dev/sda3` _to make the Linux File System partition_

</details>

<h3 style="color:#1793d1">💿 Installing and config</h3>

<details>
  <summary>
    <strong>Mounting and installing base</strong>
  </summary>

- `mount /dev/sda3 /mnt` _to mount the Linux File System partition_
- `pacstrap /mnt base linux linux-firmware` _to install base_
- `genfstab -U /mnt >> /mnt/etc/fstab` _to create fstab_
- `arch-chroot /mnt` _to enter the new system_
</details>

<details>
  <summary>
    <strong>Configuring the system</strong>
  </summary>

- `ln -sf /usr/share/zoneinfo/Europe/Amsterdam /etc/localtime` _to set the timezone_
- `hwclock --systohc` _to set the hardware clock_

  > Uncomment your locale lines in `/etc/locale.gen`

- `pacman -S nano` _to install nano_
- `nano /etc/locale.gen` _to edit the locale_
- _uncomment the lines (en_US-UTF-8)_
- `locale-gen` _to generate the locale_
- `nano /etc/hostname` _to set the hostname (set the name you want)_
- `nano /etc/hosts` _to set the hosts (set the ip address you want)_
  Recommened list to hosts:

  - 127.0.0.1 localhost
  - ::1 localhost
  - 127.0.1.1 < hostname >.localdomain < hostname >

- `passwd` _to set the root password_
- `useradd -m < username > _to create your user_
- `passwd < username >` _to set the password_
- `usermod -aG wheel,audio,video,optical,power,storage,scanner < username >` _to add the user to the groups_
- `pacman -S sudo` _to install sudo_
- `EDITOR=nano visudo` _to edit the sudoers file_
  > Find the line to uncomment the %wheel group

</details>

<details>
 <summary>
   <strong>Setting up grub</strong>
  </summary>

- `pacman -S grub` _to install grub_
- `pacman -S efibootmgr dosfstools os-prober mtools` _to install the needed packages_
- `mkdir /boot/efi` _to create the efi directory_
- `mount /dev/sda1 /boot/efi` _to mount the efi partition_
- `grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck` _to install grub_
- `grub-mkconfig -o /boot/grub/grub.cfg` _to create the grub.cfg file_
</details>

<details>
  <summary>
    <strong>Installing network</strong>
  </summary>

- `pacman -S networkmanager` _to install networkmanager_
- `systemctl enable NetworkManager` _to enable networkmanager_
</details>

<h3 style="color:#1793d1">🖥️ Desktop environment</h3>

<details>
  <summary>
    <strong>Installing Gnome (optional)</strong>
  </summary>

- `pacman -S xorg gnome gnome-shell gdm` _to install gnome_
- `systemctl enable gdm` _to enable gdm_
</details>

<details>
  <summary>
    <strong>Installing KDE (optional)</strong>
  </summary>

- `pacman -S xorg plasma kde-applications sddm` _to install kde_
- `systemctl enable sddm` _to enable sddm_
</details>

<h3 style="color:#1793d1">👋 Exit and reboot:</h3>

<details>
  <summary>
    <strong>Unmounting and rebooting</strong>
  </summary>

- `exit` _to exit arch chroot_
- `umount -l /mnt` _to unmount the partition_
- `reboot` _to reboot_
</details>

### Enjoy Arch Linux BTW!
