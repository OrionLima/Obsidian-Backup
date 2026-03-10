
### Partition Disks

#### Ventoy

	Go to https://sourceforge.net/projects/ventoy/files/v1.1.10/
	Download
	Extract
	
	Open Ventoy2Disk.exe
	Select Generic MassStorageClass
	Options
	Partition Configuration
	Preserve some space at the end of the disk
	192 GB
#### Drive Names

	/dev/nvme0n1 - NVMe SSD
	/dev/sda - External SSD
	/dev/sdc - USB Drive

#### Prepartition setup

	wipefs -a /dev/sda
	wipefs -a /dev/nvme0n1
#### Partition Disks with cfdisk

##### NVMe SSD

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


##### External SSD

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


##### USB (MicroSD Card Reader):

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


### Formatting The Drives:

##### Windows:

	mkfs.fat -F32 /dev/sda1
	mkfs.ntfs -f /dev/sda3
	mkfs.ntfs -f /dev/sda5

##### Linux:

*****NOT DONE YET***

[Next -->](Windows%20Installation.md)