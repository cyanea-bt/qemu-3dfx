# QEMU MESA GL/3Dfx Glide Pass-Through
Copyright (C) 2018-2021  
KJ Liew \<liewkj@yahoo.com\>
## Content
    qemu-0/hw/3dfx      - Overlay for QEMU source tree to add 3Dfx Glide pass-through device model
    qemu-1/hw/mesa      - Overlay for QEMU source tree to add MESA GL pass-through device model
    scripts/sign_commit - Script for stamping commit id
    wrapper             - Glide wrappers for supported guest OS/environment (DOS/Windows/DJGPP/Linux)
    wrapqgl             - MESA GL wrapper for supported guest OS/environment (Windows)
## Patch
    00-qemu520-mesa-glide.patch - Patch for QEMU version 5.2x (MESA & Glide)
    01-qemu411-mesa-glide.patch - Patch for QEMU version 4.xx (MESA & Glide)
    02-qemu311-mesa-glide.patch - Patch for QEMU version 3.xx (MESA & Glide)
    99-3dfx.patch               - Patch for QEMU version 1.6.x to 2.12.1 (deprecated)
    99-oldqemu.patch            - Additional patch for QEMU version < 2.10 (deprecated)
## QEMU Windows Guests Glide/OpenGL/Direct3D Acceleration
- YouTube channel (https://www.youtube.com/channel/UCl8InhZs1ixZBcLrMDSWd0A/videos)
- VOGONS forums (https://www.vogons.org)

Following instructions are based on `MSYS2/mingw-w64` BASH shell environment on Windows 10. It is meant to be simple and minor variations are inevitable due to different flavors of Linux distributions.

## Building QEMU
Simple guide to apply the patch:<br>
(using `00-qemu520-mesa-glide.patch`)

    $ mkdir ~/myqemu && cd ~/myqemu
    $ git clone https://github.com/kjliew/qemu-3dfx.git
    $ cd qemu-3dfx
    $ wget https://download.qemu.org/qemu-5.2.0.tar.xz
    $ tar xf qemu-5.2.0.tar.xz
    $ cd qemu-5.2.0
    $ rsync -r ../qemu-0/hw/3dfx ./hw/
    $ rsync -r ../qemu-1/hw/mesa ./hw/
    $ patch -p0 -i ../00-qemu520-mesa-glide.patch
    $ ../scripts/sign_commit
    $ mkdir ../build && cd ../build
    $ ../qemu-5.2.0/configure && make

## Building Guest Wrappers
**Requirements:**  
 - `base-devel` (make, sed, xxd)  
 - `pexports`  
 - `mingw32` cross-tools (binutils, gcc) for WIN32 DLL wrappers  
 - `Watcom C/C++ 11.0` for DOS OVL wrapper  
 - `i686-pc-msdosdjgpp` cross-tools (binutils, gcc, dxe3gen) for DJGPP DXE wrappers
<br>

    $ cd ~/myqemu/qemu-3dfx/wrapper
    $ mkdir build && cd build
    $ cp ../src/Makefile.in ./Makefile
    $ make && make clean

    $ cd ~/myqemu/qemu-3dfx/wrapqgl
    $ mkdir build && cd build
    $ cp ../src/Makefile.in ./Makefile
    $ make && make clean

## Installing Guest Wrappers
**For Win9x/ME:**  
 - Copy `FXMEMMAP.VXD` to `C:\WINDOWS\SYSTEM`  
 - Copy `GLIDE.DLL`, `GLIDE2X.DLL` and `GLIDE3X.DLL` to `C:\WINDOWS\SYSTEM`  
 - Copy `GLIDE2X.OVL` to `C:\WINDOWS`  
 - Copy `OPENGL32.DLL` to `Game Installation` folders

**For Win2k/XP:**  
 - Copy `FXPTL.SYS` to `%SystemRoot%\system32\drivers`  
 - Copy `GLIDE.DLL`, `GLIDE2X.DLL` and `GLIDE3X.DLL` to `%SystemRoot%\system32`  
 - Run `INSTDRV.EXE`, require Administrator Priviledge  
 - Copy `OPENGL32.DLL` to `Game Installation` folders
 
## Donation
If this project helps you relive the nostalgic memory of old Windows games, you can now donate to the cause of supporting those games preservation with QEMU. Your donation also motivates and encourages me in research of making QEMU a better platform for retro Windows games.

For $59.99 donation, you will deserve the following donor's privileges:
- QEMU binary package built for platform of your choice (choose one: Windows 10, Ubuntu, etc.)
- QEMU-enhanced OpenGlide host binary package built for platform of your choice (choose one: Windows 10, Ubuntu, etc.)
- QEMU-enhanced **WineD3D libraries for Win98/2K/ME/XP VMs** for DirectDraw/Direct3D games up to DirectX 9.0c
- OpenGlide guest binary package for Windows
- Elect up to 3 games for priority support and your name as the honorary sponsor in the supported & tested list of games.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=XE47KTASERX4A)
