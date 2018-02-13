
Debian
====================
This directory contains files used to package Bwbcoind/Bwbcoin-qt
for Debian-based Linux systems. If you compile Bwbcoind/Bwbcoin-qt yourself, there are some useful files here.

## Bwbcoin: URI support ##


Bwbcoin-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install Bwbcoin-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your Bwbcoin-qt binary to `/usr/bin`
and the `../../share/pixmaps/Bwbcoin128.png` to `/usr/share/pixmaps`

Bwbcoin-qt.protocol (KDE)

