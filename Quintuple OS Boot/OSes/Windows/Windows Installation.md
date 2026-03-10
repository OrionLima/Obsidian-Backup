# Windows Install ISO

### Boot Into Windows ISO

	Spam Boot Key
	Find Generic MassStorageClass Partition 2
	Select Windows 11.iso

### Diskpart

	Open CMD (Shift + F10)
	
	list disk
	sel disk x (1TB)
	
	list part
	sel part 1 (1 GiB)
	ass letter S
	
	sel part 3 (850 GiB)
	ass letter W
	
	sel part 4 (2 GiB)
	ass letter R
	
	list vol
	Take note of the letter that coresponds to the label CCCOMA_X64_FRE (Let J)
	
	exit

### DISM

#### Install Windows

	 dir J:\sources\install.wim
	
	dism /Get-ImageInfo /imagefile:J:\sources\install.wim
	Take note of the index that coresponds to Windows 11 Pro (Let 6)
	
	dism /Apply-Image /ImageFile:J:\sources\install.wim /Index:6 /ApplyDir:W:\
	bcdboot W:\Windows /s S:\ /f UEFI
	wpeutil reboot / shutdown -r -t 0 / Ctrl + Alt + Delete / Press X on window


# OOBE

	Language/Region: United States
	Keyboard: United States
	Network: (If not connected) Connect to Network / Ethernet
	PC Name: LimaMSI-Win
	Accept EULA
	Use a School Or Work account
	Sign In Options
	Domain Join instead
	Name: orion
	Password: [Set Your Password]
	Answer Security Questions
	Disable all privacy options
	Wait for updates

