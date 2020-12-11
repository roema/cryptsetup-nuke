 ## cryptsetup-nuke

A simple patch to add NukeKey feature to cryptsetup (Ubuntu 18.04 / 18.10 / 20.04)


## Requirements

* build-essential libgcrypt11-dev libdevmapper-dev libpopt-dev uuid-dev libtool automake autopoint debhelper xsltproc docbook-xsl dpkg-dev

## Installation

Don't forget to change version number...

  - Ubuntu 18.04 - 2.0.2
  - Ubuntu 18.10 - 2.0.4
  - Ubuntu 20.04 - 2.2.2
    - ***Has not yet been adequately tested!***

```
sudo apt build-dep cryptsetup
git clone  https://github.com/roema/cryptsetup-nuke
cd cryptsetup-nuke
apt source cryptsetup
cd cryptsetup-<2.0.4>
patch -p1 < ../cryptsetup-<2.0.4>.patch
dpkg-buildpackage -b -uc
cd ..
sudo dpkg -i libcryptsetup*.deb
sudo dpkg -i cryptsetup*.deb
sudo apt-mark hold cryptsetup-bin libcryptsetup12
```

## Usage examples

First, Backup LUKS header

	 sudo cryptsetup luksHeaderBackup /dev/<sda5> --header-backup-file <file>

Encrypt backup file and store it in a save place

	 openssl enc -aes-256-cbc -salt -in <luks_backup> -out <luks_backup>.enc

Add NukeKey

	 cryptsetup luksAddNuke /dev/<sda5>

To check key slots after nuking, start bootable usb and use the following command

	cryptsetup luksDump /dev/<sda5>

	#return Key Slot 0: DISABLED
	#return Key Slot 1: DISABLED
	#retrun Key Slot 2: DISABLED
	#return Key Slot 3: DISABLED
	#return Key Slot 4: DISABLED
	#return Key Slot 5: DISABLED
	#return Key Slot 6: DISABLED
	#return Key Slot 7: DISABLED

To restore the header, use the following command

	 openssl aes-256-cbc -d -a -in <file>.enc -out <file>
	 cryptsetup luksHeaderRestore /dev/<sda5> --header-backup-file <file>
