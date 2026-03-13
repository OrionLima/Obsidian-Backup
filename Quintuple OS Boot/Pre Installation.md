
# Partition Disks

## Ventoy

	Go to https://sourceforge.net/projects/ventoy/files/v1.1.10/
	Download
	Extract
	
	Open Ventoy2Disk.exe
	Select Generic MassStorageClass
	Options
	Partition Configuration
	Preserve some space at the end of the disk
	192 GB
## Drive Names

	/dev/nvme0n1 - NVMe SSD
	/dev/sda - External SSD
	/dev/sdc - USB Drive

## Prepartition setup

	wipefs -a /dev/sda
	wipefs -a /dev/nvme0n1
## Partition Disks with cfdisk

### NVMe SSD

	 cfdisk /dev/nvme0n1
	
	BOOT:
	
		Create New
		1G
		Type 
		EFI Filesystem
	
	SWAP
	
		Create New
		20G
		Type
		Linux Swap
	
	Arch
	
		Create New
		180G
		Type
		Linux Filesystem
	
	Debian
	
		Create New
		160G
		Type
		Linux Filesystem
	
	Gentoo
	
		Create New
		280G
		Type
		Linux Filesystem
	
	LFS
	
		Create New
		140G
		Type
		Linux Filesystem
	
	Share
	
		Create New
		1038 GiB
		Type
		Linux Filesystem


### External SSD

	cfdisk /dev/sda
	
	WINBOOT:
	
		Create New
		1 GiB
		Type
		EFI Filesystem
	
	WINRESERVE:
	
		Create New
		16 MiB
		Type
		Microsoft Reserved
	
	WINDOWS:
	
		 Create New
		 850 GiB
		 Type
		 Microsoft Basic Data
	
	WINPE:
	
		Create New
		2 GiB
		Type
		Microsoft Basic Data
	
	WINBACKUP:
	
		Create New
		79 GiB
		Type
		Microsoft Basic Data


### USB (MicroSD Card Reader):

	cfdisk /dev/sdc
	DO NOT TOUCH /dev/sdc1 OR /dev/sdc2
	
	PRIVATE:
	
		Create New
		20 GiB
		Type
		Linux Filesystem
	
	BACKUP:
	
		Create New
		140 GiB
		Type
		Linux Filesystem
	
	FILES:
	
		Create New
		19 GiB
		Type
		Microsoft Basic Data


# Formatting The Drives:

### Windows on External SSD (/dev/sda):

	mkfs.fat -F 32 -n WINBOOT /dev/sda1  
	mkfs.ntfs -Q -L WINDOWS /dev/sda3  
	mkfs.ntfs -Q -L WINPE /dev/sda4  
	mkfs.ntfs -Q -L WINBACKUP /dev/sda5  

### Ventoy on MicroSD (/dev/sdc):

	cryptsetup luksFormat /dev/sdc3  
	cryptsetup open /dev/sdc3 private  
	mkfs.ext4 -F -L PRIVATE /dev/mapper/private  
	cryptsetup close private 
	 
	mkfs.ext4 -F -L BACKUP /dev/sdc4  
	mkfs.ntfs -L FILES /dev/sdc5
### Linux on NVME SSD (/dev/nvme0n1):

	mkfs.fat -F 32 -n BOOT /dev/nvme0n1p1  
	mkswap -L SWAP /dev/nvme0n1p2  
	mkfs.btrfs -f -L ARCH /dev/nvme0n1p3  
	mkfs.btrfs -f -L DEBIAN /dev/nvme0n1p4  
	mkfs.btrfs -f -L GENTOO /dev/nvme0n1p5  
	mkfs.btrfs -f -L LFS /dev/nvme0n1p6
	mkfs.btrfs -f -L SHARE /dev/nvme0n1p7  

# File System Labels

## GPT Labels
### Windows on External SSD (/dev/sda):

	parted -s /dev/sda name 1 WINBOOT  
	parted -s /dev/sda name 2 WINRESERVE  
	parted -s /dev/sda name 3 WINDOWS  
	parted -s /dev/sda name 4 WINPE  
	parted -s /dev/sda name 5 WINBACKUP

### Ventoy on MicroSD (/dev/sdc):

	parted -s /dev/sdc name 1 RECOVERY  
	parted -s /dev/sdc name 2 VTOYEFI  
	parted -s /dev/sdc name 3 PRIVATE  
	parted -s /dev/sdc name 4 BACKUP  
	parted -s /dev/sdc name 5 FILES

### Linux on NVME SSD (/dev/nvme0n1):

	parted -s /dev/nvme0n1 name 1 BOOT  
	parted -s /dev/nvme0n1 name 2 SWAP  
	parted -s /dev/nvme0n1 name 3 ARCH  
	parted -s /dev/nvme0n1 name 4 DEBIAN  
	parted -s /dev/nvme0n1 name 5 GENTOO  
	parted -s /dev/nvme0n1 name 6 LFS  
	parted -s /dev/nvme0n1 name 7 SHARE

## Filesystem Labels

### Windows on External SSD (/dev/sda):

	fatlabel /dev/sda1 WINBOOT
	ntfslabel /dev/sda3 WINDOWS  
	ntfslabel /dev/sda4 WINPE  
	ntfslabel /dev/sda5 WINBACKUP

### Ventoy on MicroSD (/dev/sdc):

	exfatlabel /dev/sdc1 RECOVERY
	fatlabel /dev/sdc2 VTOYEFI
	
	cryptsetup open /dev/sdc3 private  
	e2label /dev/mapper/private PRIVATE  
	cryptsetup close private
	
	e2label /dev/sdc4 BACKUP  
	ntfslabel /dev/sdc5 FILES

### Linux on NVME SSD (/dev/nvme0n1):

	fatlabel /dev/nvme0n1p1 BOOT  
	swaplabel -L SWAP /dev/nvme0n1p2  
	btrfs filesystem label /dev/nvme0n1p3 ARCH  
	btrfs filesystem label /dev/nvme0n1p4 DEBIAN  
	btrfs filesystem label /dev/nvme0n1p5 GENTOO  
	btrfs filesystem label /dev/nvme0n1p6 LFS  
	btrfs filesystem label /dev/nvme0n1p7 SHARE

[Next -->](Windows%20Installation.md)