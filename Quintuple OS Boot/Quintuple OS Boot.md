
# Part 1: Why a Quintuple Boot

In a Dual Boot, you know exactly what you need. Usually the 2 dual booted OSes are Windows and a Linux Distro. However in a Quintuple Boot OS, you can explore your system much more deeply. As an example, rather than having only Windows to can game, and any Linux distro to do your daily work, You can have Windows for your gaming, Debian (Or another stable distro) as a way to know nothing will go wrong, Arch as a daily driver, and any other OSes you so choose.

# Part 2: What OSes

I personally chose these following OSes:

[Windows](Windows%20Installation.md) -- Windows is by far my most reluctant OS to use. However, it is the one OS that has the most support by far, even compared to second place, MacOS. Gaming on Windows is also the best and is still incomparable to Linux (Even after Steam)

[Arch Linux](Arch.md) -- Arch Linux is my daily driver OS. This is what I'll use to rice Linux, do some light gaming. Work, Use LibreOffice, etc. I chose it because it's the most customizable Linux machine without it being a nuisance.

[Debian](Debian.md) -- Debian is an incredibly stable OS. In case something goes wrong with an Arch Update, Debian has my back. Along with that it's also an amazing OS to have alongside Arch and can help accomplish some amazing things.

[Gentoo](Gentoo.md) -- Gentoo is the most customizable OS that isn't Linux From Scratch. As many issues as Gentoo might have, it is still an amazing OS to run. It isn't my Daily Driver but it is going to have amazing performance. Also this is largely going to be an experimental OS so I can learn how to do things like compiling kernels

[Linux From Scratch (LFS)](<Linux From Scratch (LFS).md>) -- Linux from Scratch is entirely an educational distro here. It is BY FAR the most lightweight and customizable OS, however, that comes with a few caveats. It is by and large the most difficult OS to set up and it takes the longest. However, the final reward is worth it as you get an amazing OS incomparable to anything else.

# Part 3: Partitioning The Drives

I have 3 Main Drives. Those are:

My NVMe SSD - 2TB
My External SSD - 1TB
My USB Stick - 256 GB

### NVMe M.2 SSD:

 2 TB = 1.81899 TiB

	BOOT - 1 GiB - Fat32 - EFI
	SWAP - 20 GiB - Swap
	ARCH - 180 GiB - BTRFS - Arch
	DEBIAN - 160 GiB - BTRFS - Debian
	GENTOO - 280 GiB - BTRFS - Gentoo
	LFS - 140 GiB - BTRFS - LFS
	SHARED - 1038 GiB - BTRFS - Shared Linux Data

### External SSD:

1 TB = 0.909495 TiB

	WINBOOT - 1 GiB - Fat32 - EFI
	WINRESERVE - 16 MiB - Windows Reserved
	WINDOWS - 850 GiB - NTFS - Windows
	WINPE - 2 GiB - Windows Recovery
	WINBACKUP - 79 GiB - Backup

### USB (MicroSD Card Reader):

256 GB = 238.419 GiB

	VTOYEFI - 32 MiB - Fat32 - EFI
	RECOVERY - 60 GiB - exFAT - ISO
	PRIVATE - 20 GiB - luksFormat - Private (Password Protected)
	BACKUP - 140 GiB - ext4 - Backup
	FILES - 19 GiB - NTFS - File Transfer

### BTRFS Subvolumes:

##### Arch

	 @
	 @home_arch
	 @snapshots_arch
	 @var_log_arch
	 @var_cache_arch
	 @pkgcache_arch
	 @tmp_arch

##### Debian

	@
	@home_debian
	@snapshots_debian
	@var_log_debian
	@var_cache_debian
	@tmp_debian

##### Gentoo

	@
	@home_gentoo
	@snapshots_gentoo
	@var_log_gentoo
	@var_cache_gentoo
	@portage_tmp
	@distfiles
	@ccache

##### Linux From Scratch

	 @
	 @home_lfs
	 @snapshots_lfs
	 @build_lfs
	 @src_lfs
	 @var_log_lfs

##### Shared

	@shared_docs
	@shared_projects
	@shared_media
	@shared_iso
	@shared_backups
	@shared_dotfiles
	@shared_vm
	@shared_src_archive


[Next -->](<Pre Installation.md>) 