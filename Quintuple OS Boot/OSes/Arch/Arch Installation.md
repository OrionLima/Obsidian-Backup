
# Pre Chroot

## Preformat Commands

	timedatectl set-ntp true
	ls /sys/firmware/efi/efivars
	lsblk -o NAME,SIZE,FSTYPE,LABEL,PARTLABEL,MOUNTPOINTS
	blkid /dev/nvme0n1p1 /dev/nvme0n1p2 /dev/nvme0n1p3 /dev/nvme0n1p7
	ping -c 3 archlinux.org

## Mounting Partitions

### Arch (Partition 3):

#### Creating BTRFS Subvolumes

	mount /dev/nvme0n1p3 /mnt
	
	btrfs subvolume create /mnt/@
	btrfs subvolume create /mnt/@home_arch
	btrfs subvolume create /mnt/@snapshots_arch
	btrfs subvolume create /mnt/@var_log_arch
	btrfs subvolume create /mnt/@var_cache_arch
	btrfs subvolume create /mnt/@pkgcache_arch
	btrfs subvolume create /mnt/@tmp_arch
	
	umount /mnt

#### Mounting Subvolumes

	mount -o noatime,compress=zstd,subvol=@ /dev/nvme0n1p3 /mnt
	
	mkdir -p /mnt/home
	mkdir -p /mnt/.snapshots
	mkdir -p /mnt/var/log
	mkdir -p /mnt/var/cache
	mkdir -p /mnt/var/cache/pacman/pkg
	mkdir -p /mnt/tmp
	mkdir -p /mnt/boot
	
	mount -o noatime,compress=zstd,subvol=@home_arch /dev/nvme0n1p3 /mnt/home
	mount -o noatime,compress=zstd,subvol=@snapshots_arch /dev/nvme0n1p3 /mnt/.snapshots
	mount -o noatime,compress=zstd,subvol=@var_log_arch /dev/nvme0n1p3 /mnt/var/log
	mount -o noatime,compress=zstd,subvol=@var_cache_arch /dev/nvme0n1p3 /mnt/var/cache
	mount -o noatime,compress=zstd,subvol=@pkgcache_arch /dev/nvme0n1p3 /mnt/var/cache/pacman/pkg
	mount -o noatime,compress=zstd,subvol=@tmp_arch /dev/nvme0n1p3 /mnt/tmp


### Shared (Partition 7):

#### Creating BTRFS Subvolumes

	mount /dev/nvme0n1p7 /mnt
	
	btrfs subvolume create /mnt/@shared_docs
	btrfs subvolume create /mnt/@shared_projects
	btrfs subvolume create /mnt/@shared_media
	btrfs subvolume create /mnt/@shared_iso
	btrfs subvolume create /mnt/@shared_backups
	btrfs subvolume create /mnt/@shared_dotfiles
	btrfs subvolume create /mnt/@shared_vm
	btrfs subvolume create /mnt/@shared_src_archive
	
	umount /mnt

#### Mounting Subvolumes

	mkdir -p /mnt/shared
	mkdir -p /mnt/shared/docs
	mkdir -p /mnt/shared/projects
	mkdir -p /mnt/shared/media
	mkdir -p /mnt/shared/iso
	mkdir -p /mnt/shared/backups
	mkdir -p /mnt/shared/dotfiles
	mkdir -p /mnt/shared/vm
	mkdir -p /mnt/shared/src_archive
	
	mount -o noatime,compress=zstd,subvol=@shared_docs /dev/nvme0n1p7 /mnt/shared/docs
	mount -o noatime,compress=zstd,subvol=@shared_projects /dev/nvme0n1p7 /mnt/shared/projects
	mount -o noatime,compress=zstd,subvol=@shared_media /dev/nvme0n1p7 /mnt/shared/media
	mount -o noatime,compress=zstd,subvol=@shared_iso /dev/nvme0n1p7 /mnt/shared/iso
	mount -o noatime,compress=zstd,subvol=@shared_backups /dev/nvme0n1p7 /mnt/shared/backups
	mount -o noatime,compress=zstd,subvol=@shared_dotfiles /dev/nvme0n1p7 /mnt/shared/dotfiles
	mount -o noatime,compress=zstd,subvol=@shared_vm /dev/nvme0n1p7 /mnt/shared/vm
	mount -o noatime,compress=zstd,subvol=@shared_src_archive /dev/nvme0n1p7 /mnt/shared/src_archive

### Mounting Partitions / Verifying

	mount /dev/nvme0n1p1 /mnt/boot
	swapon /dev/nvme0n1p2
	
	findmnt /mnt
	lsblk -o NAME,SIZE,FSTYPE,LABEL,PARTLABEL,MOUNTPOINTS /dev/nvme0n1

## Pacstrap

### Run:

	pacstrap -K /mnt \  
	base base-devel \  
	linux-zen linux-zen-headers \  
	linux-lts linux-lts-headers \  
	linux-firmware intel-ucode \  
	btrfs-progs \  
	grub efibootmgr \  
	networkmanager \  
	sudo git vim neovim nano man-db man-pages texinfo \  
	dkms \  
	nvidia-open-dkms nvidia-utils lib32-nvidia-utils nvidia-prime \  
	mesa vulkan-intel lib32-vulkan-intel \  
	plasma-meta sddm \  
	hyprland kitty \  
	xdg-desktop-portal xdg-desktop-portal-kde xdg-desktop-portal-hyprland \  
	pipewire pipewire-pulse pipewire-alsa pipewire-jack wireplumber \  
	bluez bluez-utils \  
	steam gamemode mangohud \  
	snapper snap-pac btrfs-assistant

## Fstab

	genfstab -U /mnt >> /mnt/etc/fstab
	cat /mnt/etc/fstab
	arch-chroot /mnt

# Chroot

## Locales

	ln -sf /usr/share/zoneinfo/Europe/Tirane /etc/localtime
	hwclock --systohc
	
	sed -i 's/^#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
	locale-gen
	
	printf 'LANG=en_US.UTF-8\n' > /etc/locale.conf
	printf 'KEYMAP=us\n' > /etc/vconsole.conf
	printf 'LimaMSI-Arch\n' > /etc/hostname
	
	cat > /etc/hosts <<'EOF'
	127.0.0.1 localhost
	::1       localhost
	127.0.1.1 LimaMSI-Arch.localdomain arch
	EOF

## Pacman Config

	sed -i '/^\#\[multilib\]/,/^\#Include = \/etc\/pacman.d\/mirrorlist/ s/^#//' /etc/pacman.conf
	pacman -Syy

## Users

	passwd
	
	useradd -m -G wheel,audio,video,storage,input YOURUSERNAME
	passwd YOURUSERNAME
	
	sed -i 's/^# %wheel ALL=(ALL:ALL) ALL/%wheel ALL=(ALL:ALL) ALL/' /etc/sudoers

## Initramfs + GRUB

	SWAPUUID=$(blkid -s UUID -o value /dev/nvme0n1p2)
	echo "$SWAPUUID"
	
	sed -i 's/^MODULES=.*/MODULES=(btrfs nvidia nvidia_modeset nvidia_uvm nvidia_drm)/' /etc/mkinitcpio.conf
	sed -i 's/^HOOKS=.*/HOOKS=(base udev autodetect microcode modconf kms keyboard keymap consolefont block filesystems resume fsck)/' /etc/mkinitcpio.conf
	
	sed -i "s/^GRUB_CMDLINE_LINUX_DEFAULT=.*/GRUB_CMDLINE_LINUX_DEFAULT=\"resume=UUID=${SWAPUUID} nvidia_drm.modeset=1\"/" /etc/default/grub
	sed -i 's/^#\?GRUB_TIMEOUT=.*/GRUB_TIMEOUT=7/' /etc/default/grub
	
	grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=ARCH
	grub-mkconfig -o /boot/grub/grub.cfg
	efibootmgr -v

## Systemctl

	systemctl enable NetworkManager
	systemctl enable sddm
	systemctl enable bluetooth
	systemctl enable fstrim.timer

## Reboot

	exit  
	umount -R /mnt  
	swapoff /dev/nvme0n1p2  
	reboot



# Post Chroot

## Install verify

	uname -r
	findmnt /
	findmnt /boot
	findmnt /home
	findmnt /tmp
	findmnt /shared/docs
	findmnt /shared/projects
	findmnt /shared/media
	nvidia-smi
	systemctl status NetworkManager --no-pager
	systemctl status sddm --no-pager

## BTRFS

	sudo snapper -c root create-config /
	sudo btrfs subvolume delete /.snapshots
	sudo mkdir /.snapshots
	sudo mount -a
	sudo chmod 750 /.snapshots
	sudo snapper -c root create -d "Fresh Arch install"