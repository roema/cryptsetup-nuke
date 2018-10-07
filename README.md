# cryptsetup-nuke

A simple patch to add NukeKey feature to cryptsetup 2:2.0.2-1ubuntu1.1 (Ubuntu 18.04)


## Requirements

* libgcrypt11-dev libdevmapper-dev libpopt-dev uuid-dev libtool automake autopoint debhelper xsltproc docbook-xsl dpkg-dev

## Installation

	# sudo apt-get install libgcrypt11-dev libdevmapper-dev libpopt-dev uuid-dev libtool automake autopoint debhelper xsltproc docbook-xsl dpkg-dev
	# apt-get source cryptsetup
	# git clone https://github.com/roema/cryptsetup-nuke
	# cd cryptsetup-2.0.2
	# patch -p1 < ../cryptsetup-nuke/cryptsetup-2.0.2.patch
	# dpkg-buildpackage -b -uc
	# cd ..
	# sudo dpkg -i ../libcryptsetup*.deb
	# sudo dpkg -i ../cryptsetup*.deb

## Usage examples

First, Backup LUKS header

	# sudo cryptsetup luksHeaderBackup /dev/<sda5> --header-backup-file <file>

Encrypt backup file and store it in a save place

	# openssl enc -aes-256-cbc -salt -in <luks_backup> -out <luks_backup>.enc

Add NukeKey

	# cryptsetup luksAddNuke /dev/<sda5>

To restore the header, start with bootable usb and execute following command

	# openssl aes-256-cbc -d -a -in <file>.enc -out <file>
	# cryptsetup luksHeaderRestore /dev/<sda5> --header-backup-file <file>

