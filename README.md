# PiBookPro
Here are instructions for installing everything you need to run the PiBookPro on a Raspberry Pi 3B or 4, using the latest Raspberry Pi OS dated August 2020, and the latest DisplayLink 5.3.1 drivers for Ubuntu.*





Installing on Pi 2B/3B/4, Raspberry Pi OS March 4 2021, DisplayLink 5.4
There have been a few Raspberry Pi OS updates, and some issues with the DisplayLink software, since I originally wrote these instructions. Unfortunately, the current version of Raspberry Pi OS and the previous and current DisplayLink driver appear to be incompatible out of the box. You'll need to do a little extra work to keep your Pi Book Pro running until everything matches back up again.

Here are what I believe to be accurate instructions for installing everything you need to run the Pi Book Pro on a Raspberry Pi 2B, 3B or 4, using the latest Raspberry Pi OS dated March 4 2021, and the legacy DisplayLink 5.3.1 drivers for Ubuntu.

Plug an external HDMI display, USB keyboard, and USB mouse into your Raspberry Pi. You cannot use the Pi Book Pro to set up the Raspberry Pi for the first time.

Format a Micro SD card with "Raspberry Pi OS (Legacy)". Using the official Raspberry Pi Imager is recommended (it's under "Raspberry Pi OS (other)"). 
Follow the official instructions to set up your Raspberry Pi, including making sure your networking is working (wifi or ethernet). Restart when prompted.

Next, we need to enable an option called "kernel mode-setting." Click on the Terminal icon () to open a console window, and type sudo raspi-config. This opens the text-based raspi-config utility. Navigate the menus from Advanced Options to GL Driver and enable Fake KMS. See the official raspi-config documentation for more detailed instructions if you need help. If it asks you to reboot, say Yes.

Then, we need to roll back the Raspberry Pi OS kernel version to a compatible version. Raspberry Pi OS now ships with 5.10.x, and we need 5.4.x.

Click on the Terminal icon again, and enter the following commands to install a previous kernel, install supporting software, install the source code for that kernel, and configure the source code for that kernel. I've included the full output of the commands for you to compare to, as this is an atypical process:



$ sudo rpi-update 43022f5adbbf5d6e05ea0e022a3090c2c9feff7c
 *** Raspberry Pi firmware updater by Hexxeh, enhanced by AndrewS and Dom
 *** Performing self-update
 *** Relaunching after update
 *** Raspberry Pi firmware updater by Hexxeh, enhanced by AndrewS and Dom
 *** We're running for the first time
 *** Backing up files (this will take a few minutes)
 *** Backing up firmware
 *** Backing up modules 5.10.17-v7l+
.#############################################################
WARNING: This update bumps to rpi-5.4.y linux tree
See: https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=269769

'rpi-update' should only be used if there is a specific
reason to do so - for example, a request by a Raspberry Pi
engineer or if you want to help the testing effort
and are comfortable with restoring if there are regressions.

DO NOT use 'rpi-update' as part of a regular update process.

.##############################################################
Would you like to proceed? (y/N)
 *** Downloading specific firmware revision (this will take a few minutes)
. % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   168  100   168    0     0   1105      0 --:--:-- --:--:-- --:--:--  1112
100  117M    0  117M    0     0  6254k      0 --:--:--  0:00:19 --:--:-- 5135k
 *** Updating firmware
 *** Updating kernel modules
 *** depmod 5.4.83+
 *** depmod 5.4.83-v8+
 *** depmod 5.4.83-v7+
 *** depmod 5.4.83-v7l+
 *** Updating VideoCore libraries
 *** Using HardFP libraries
 *** Updating SDK
 *** Running ldconfig
 *** Storing current firmware revision
 *** Deleting downloaded files
 *** Syncing changes to disk
 *** If no errors appeared, your firmware was successfully updated to 43022f5adbbf5d6e05ea0e022a3090c2c9feff7c
 *** A reboot is needed to activate the new firmware
Then, type sudo reboot to reboot into the new (old) kernel. Next, five more commands (apt, wget, chmod, and rpi-source twice):

$ sudo apt install git bc bison flex libssl-dev
Reading package lists... Done
Building dependency tree       
Reading state information... Done
bc is already the newest version (1.07.1-2).
bc set to manually installed.
git is already the newest version (1:2.20.1-2+deb10u3).
The following additional packages will be installed:
  libbison-dev libfl-dev libsigsegv2 m4
Suggested packages:
  bison-doc flex-doc libssl-doc m4-doc
The following NEW packages will be installed:
  bison flex libbison-dev libfl-dev libsigsegv2 libssl-dev m4
0 upgraded, 7 newly installed, 0 to remove and 0 not upgraded.
Need to get 3,662 kB of archives.
After this operation, 10.3 MB of additional disk space will be used.
Do you want to continue? [Y/n] 
Get:1 http://archive.raspberrypi.org/debian buster/main armhf libssl-dev armhf 1.1.1d-0+deb10u6+rpt1 [1,584 kB]
Get:2 http://raspbian.mirrors.lucidnetworks.net/raspbian buster/main armhf libsigsegv2 armhf 2.12-2 [32.3 kB]
Get:3 http://raspbian.mirrors.lucidnetworks.net/raspbian buster/main armhf m4 armhf 1.4.18-2 [185 kB]
Get:4 http://raspbian.mirrors.lucidnetworks.net/raspbian buster/main armhf flex armhf 2.6.4-6.2 [427 kB]
Get:5 http://raspbian.mirrors.lucidnetworks.net/raspbian buster/main armhf libbison-dev armhf 2:3.3.2.dfsg-1 [500 kB]
Get:6 http://raspbian.mirrors.lucidnetworks.net/raspbian buster/main armhf bison armhf 2:3.3.2.dfsg-1 [829 kB]
Get:7 http://raspbian.mirrors.lucidnetworks.net/raspbian buster/main armhf libfl-dev armhf 2.6.4-6.2 [104 kB]
Fetched 3,662 kB in 4s (1,015 kB/s)
Selecting previously unselected package libsigsegv2:armhf.
(Reading database ... 98611 files and directories currently installed.)
Preparing to unpack .../0-libsigsegv2_2.12-2_armhf.deb ...
Unpacking libsigsegv2:armhf (2.12-2) ...
Selecting previously unselected package m4.
Preparing to unpack .../1-m4_1.4.18-2_armhf.deb ...
Unpacking m4 (1.4.18-2) ...
Selecting previously unselected package flex.
Preparing to unpack .../2-flex_2.6.4-6.2_armhf.deb ...
Unpacking flex (2.6.4-6.2) ...
Selecting previously unselected package libbison-dev:armhf.
Preparing to unpack .../3-libbison-dev_2%3a3.3.2.dfsg-1_armhf.deb ...
Unpacking libbison-dev:armhf (2:3.3.2.dfsg-1) ...
Selecting previously unselected package bison.
Preparing to unpack .../4-bison_2%3a3.3.2.dfsg-1_armhf.deb ...
Unpacking bison (2:3.3.2.dfsg-1) ...
Selecting previously unselected package libfl-dev:armhf.
Preparing to unpack .../5-libfl-dev_2.6.4-6.2_armhf.deb ...
Unpacking libfl-dev:armhf (2.6.4-6.2) ...
Selecting previously unselected package libssl-dev:armhf.
Preparing to unpack .../6-libssl-dev_1.1.1d-0+deb10u6+rpt1_armhf.deb ...
Unpacking libssl-dev:armhf (1.1.1d-0+deb10u6+rpt1) ...
Setting up libbison-dev:armhf (2:3.3.2.dfsg-1) ...
Setting up libsigsegv2:armhf (2.12-2) ...
Setting up libssl-dev:armhf (1.1.1d-0+deb10u6+rpt1) ...
Setting up m4 (1.4.18-2) ...
Setting up bison (2:3.3.2.dfsg-1) ...
update-alternatives: using /usr/bin/bison.yacc to provide /usr/bin/yacc (yacc) in auto mode
Setting up flex (2.6.4-6.2) ...
Setting up libfl-dev:armhf (2.6.4-6.2) ...
Processing triggers for libc-bin (2.28-10+rpi1) ...
Processing triggers for man-db (2.8.5-2) ...
Processing triggers for install-info (6.5.0.dfsg.1-4+b1) ...
$ sudo wget https://raw.githubusercontent.com/RPi-Distro/rpi-source/master/rpi-source -O /usr/local/bin/rpi-source
--2021-04-14 21:35:25--  https://raw.githubusercontent.com/RPi-Distro/rpi-source/master/rpi-source
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.108.133, 185.199.111.133, 185.199.110.133, ...
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14222 (14K) [text/plain]
Saving to: ‘/usr/local/bin/rpi-source’

/usr/local/bin/rpi- 100%[===================>]  13.89K  --.-KB/s    in 0.001s  

2021-04-14 21:35:25 (9.44 MB/s) - ‘/usr/local/bin/rpi-source’ saved [14222/14222]

$ sudo chmod +x /usr/local/bin/rpi-source
$ /usr/local/bin/rpi-source -q --tag-update
$ rpi-source

 *** SoC: BCM2711

 *** rpi-update: https://github.com/Hexxeh/rpi-firmware

 *** Firmware revision: 43022f5adbbf5d6e05ea0e022a3090c2c9feff7c

 *** Linux source commit: 76c49e60e742d0bebd798be972d67dd3fd007691

 *** Download kernel source
--2021-04-14 21:35:47--  https://github.com/raspberrypi/linux/archive/76c49e60e742d0bebd798be972d67dd3fd007691.tar.gz
Resolving github.com (github.com)... 140.82.113.4
Connecting to github.com (github.com)|140.82.113.4|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://codeload.github.com/raspberrypi/linux/tar.gz/76c49e60e742d0bebd798be972d67dd3fd007691 [following]
--2021-04-14 21:35:48--  https://codeload.github.com/raspberrypi/linux/tar.gz/76c49e60e742d0bebd798be972d67dd3fd007691
Resolving codeload.github.com (codeload.github.com)... 140.82.114.9
Connecting to codeload.github.com (codeload.github.com)|140.82.114.9|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [application/x-gzip]
Saving to: ‘/home/pi/linux-76c49e60e742d0bebd798be972d67dd3fd007691.tar.gz’

/home/pi/linux-76c4     [<=>                 ] 167.12M  6.15MB/s    in 27s     

2021-04-14 21:36:16 (6.09 MB/s) - ‘/home/pi/linux-76c49e60e742d0bebd798be972d67dd3fd007691.tar.gz’ saved [175242951]


 *** Unpack kernel source
...............................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................
 *** Add '+' to kernel release string

 *** Create symlink: /home/pi/linux

 *** Create /lib/modules/<ver>/{build,source} symlinks

 *** .config

 *** Module.symvers

 *** make modules_prepare
  HOSTCC  scripts/basic/fixdep
  HOSTCC  scripts/kconfig/conf.o
  HOSTCC  scripts/kconfig/confdata.o
  HOSTCC  scripts/kconfig/expr.o
  LEX     scripts/kconfig/lexer.lex.c
  YACC    scripts/kconfig/parser.tab.[ch]
  HOSTCC  scripts/kconfig/lexer.lex.o
  HOSTCC  scripts/kconfig/parser.tab.o
  HOSTCC  scripts/kconfig/preprocess.o
  HOSTCC  scripts/kconfig/symbol.o
  HOSTLD  scripts/kconfig/conf
scripts/kconfig/conf  --syncconfig Kconfig
  SYSHDR  arch/arm/include/generated/uapi/asm/unistd-common.h
  SYSHDR  arch/arm/include/generated/uapi/asm/unistd-oabi.h
  SYSHDR  arch/arm/include/generated/uapi/asm/unistd-eabi.h
  HOSTCC  scripts/dtc/dtc.o
  HOSTCC  scripts/dtc/flattree.o
  HOSTCC  scripts/dtc/fstree.o
  HOSTCC  scripts/dtc/data.o
  HOSTCC  scripts/dtc/livetree.o
  HOSTCC  scripts/dtc/treesource.o
  HOSTCC  scripts/dtc/srcpos.o
  HOSTCC  scripts/dtc/checks.o
  HOSTCC  scripts/dtc/util.o
  LEX     scripts/dtc/dtc-lexer.lex.c
  YACC    scripts/dtc/dtc-parser.tab.[ch]
  HOSTCC  scripts/dtc/dtc-lexer.lex.o
  HOSTCC  scripts/dtc/dtc-parser.tab.o
  HOSTLD  scripts/dtc/dtc
  HOSTCC  scripts/genksyms/genksyms.o
  YACC    scripts/genksyms/parse.tab.[ch]
  HOSTCC  scripts/genksyms/parse.tab.o
  LEX     scripts/genksyms/lex.lex.c
  HOSTCC  scripts/genksyms/lex.lex.o
  HOSTLD  scripts/genksyms/genksyms
  HOSTCC  scripts/kallsyms
  HOSTCC  scripts/pnmtologo
  HOSTCC  scripts/conmakehash
  HOSTCC  scripts/recordmcount
  HOSTCC  scripts/sortextable
  HOSTCC  scripts/asn1_compiler
  HOSTCC  scripts/extract-cert
  WRAP    arch/arm/include/generated/uapi/asm/kvm_para.h
  WRAP    arch/arm/include/generated/uapi/asm/bitsperlong.h
  WRAP    arch/arm/include/generated/uapi/asm/bpf_perf_event.h
  WRAP    arch/arm/include/generated/uapi/asm/errno.h
  WRAP    arch/arm/include/generated/uapi/asm/ioctl.h
  WRAP    arch/arm/include/generated/uapi/asm/ipcbuf.h
  WRAP    arch/arm/include/generated/uapi/asm/msgbuf.h
  WRAP    arch/arm/include/generated/uapi/asm/param.h
  WRAP    arch/arm/include/generated/uapi/asm/poll.h
  WRAP    arch/arm/include/generated/uapi/asm/resource.h
  WRAP    arch/arm/include/generated/uapi/asm/sembuf.h
  WRAP    arch/arm/include/generated/uapi/asm/shmbuf.h
  WRAP    arch/arm/include/generated/uapi/asm/siginfo.h
  WRAP    arch/arm/include/generated/uapi/asm/socket.h
  WRAP    arch/arm/include/generated/uapi/asm/sockios.h
  WRAP    arch/arm/include/generated/uapi/asm/termbits.h
  WRAP    arch/arm/include/generated/uapi/asm/termios.h
  WRAP    arch/arm/include/generated/asm/compat.h
  WRAP    arch/arm/include/generated/asm/current.h
  WRAP    arch/arm/include/generated/asm/early_ioremap.h
  WRAP    arch/arm/include/generated/asm/emergency-restart.h
  WRAP    arch/arm/include/generated/asm/exec.h
  WRAP    arch/arm/include/generated/asm/extable.h
  WRAP    arch/arm/include/generated/asm/flat.h
  WRAP    arch/arm/include/generated/asm/irq_regs.h
  WRAP    arch/arm/include/generated/asm/kdebug.h
  WRAP    arch/arm/include/generated/asm/local.h
  WRAP    arch/arm/include/generated/asm/local64.h
  WRAP    arch/arm/include/generated/asm/mm-arch-hooks.h
  WRAP    arch/arm/include/generated/asm/mmiowb.h
  WRAP    arch/arm/include/generated/asm/msi.h
  WRAP    arch/arm/include/generated/asm/parport.h
  WRAP    arch/arm/include/generated/asm/preempt.h
  WRAP    arch/arm/include/generated/asm/seccomp.h
  WRAP    arch/arm/include/generated/asm/serial.h
  WRAP    arch/arm/include/generated/asm/trace_clock.h
  WRAP    arch/arm/include/generated/asm/simd.h
  UPD     include/config/kernel.release
  UPD     include/generated/uapi/linux/version.h
  UPD     include/generated/utsrelease.h
  SYSNR   arch/arm/include/generated/asm/unistd-nr.h
  GEN     arch/arm/include/generated/asm/mach-types.h
  SYSTBL  arch/arm/include/generated/calls-oabi.S
  SYSTBL  arch/arm/include/generated/calls-eabi.S
  CC      scripts/mod/empty.o
  HOSTCC  scripts/mod/mk_elfconfig
  MKELF   scripts/mod/elfconfig.h
  HOSTCC  scripts/mod/modpost.o
  CC      scripts/mod/devicetable-offsets.s
  UPD     scripts/mod/devicetable-offsets.h
  HOSTCC  scripts/mod/file2alias.o
  HOSTCC  scripts/mod/sumversion.o
  HOSTLD  scripts/mod/modpost
  CC      kernel/bounds.s
  UPD     include/generated/bounds.h
  UPD     include/generated/timeconst.h
  CC      arch/arm/kernel/asm-offsets.s
  UPD     include/generated/asm-offsets.h
  CALL    scripts/checksyscalls.sh
  CALL    scripts/atomic/check-atomics.sh

 *** ncurses-devel is NOT installed. Needed by 'make menuconfig'. On Raspberry Pi OS,
run 'sudo apt install libncurses5-dev' to install it.

 *** Help: https://github.com/RPi-Distro/rpi-source/blob/master/README.md
Then, we'll download the legacy DisplayLink drivers suitable for the Pi Book Pro screen. Click on the Web Browser icon () and visit the URL https://www.synaptics.com/products/displaylink-graphics/downloads/ubuntu. Click the + alongside "Legacy Drivers", then Download alongside the "Release 5.3.1" drivers, then Accept to start the download onto your Raspberry Pi.

Once the download completes, we'll need to uncompress and install it. Click on the Terminal icon again and type in cd Downloads to change to the Downloads directory. Then, unzip DisplayLink\ USB\ Graphics\ Software\ for\ Ubuntu5.3.1-EXE.zip to uncompress it (you may be able to autocomplete the file name by typing just unzip Display and then pressing Tab). Run the installer by typing sudo bash ./displaylink-driver-5.3.1.34.run (again, you may be able to autocomplete the file name by typing just sudo bash ./displaylink and pressing Tab).

The DisplayLink driver will automatically download all the dependencies it needs. Hit Enter at the download prompt to confirm.

When the installer completes successfully, it will prompt you to reboot. We've a couple changes to make still, so press N and Enter to decline.

On the Pi 3B, the DisplayLink driver seems to occasionally load too slowly, and isn't ready by the time the Pi wants to display the desktop, so we need to force it to restart the desktop display manager in those cases. This is less of an issue on the faster Pi 4, but these changes are still safe to make. In the Terminal window, type sudo nano /lib/systemd/system/displaylink-driver.service. This will start a text editor. Use the arrow keys to move down to the [Service] section, and add a new line after the ExecStart line that reads ExecStartPost=/bin/systemctl try-restart display-manager. (If you're copy-and-pasting from the web browser, you can paste using CtrlShiftV.) That whole section should look like this:

[Service]
ExecStartPre=/bin/sh -c 'modprobe evdi || (dkms install $(ls -t /usr/src | grep evdi | head -n1  | sed -e "s:-:/:") && modprobe evdi)'
ExecStart=/opt/displaylink/DisplayLinkManager
ExecStartPost=/bin/systemctl try-restart display-manager
Restart=always
WorkingDirectory=/opt/displaylink
RestartSec=5
Press CtrlO, Enter to save, then CtrlX to exit. Enable the changes by typing sudo systemctl daemon-reload in the Terminal window. (If you don't make these changes, and your Pi ever boots up but nothing shows on the screen, you can blindly reboot it by pressing CtrlAltDel to bring up the shutdown menu, then Down arrow to select Reboot, then Spacebar to press it.)

We also need to accommodate the fact that the bezel covers up some of the pixels of the display. In the Terminal window, type sudo nano /usr/share/X11/xorg.conf.d/10-monitor.conf to create a new monitor configuration file. In it, type or paste (CtrlShiftV) the following:

Section "Monitor"
    Identifier "DVI-I-1"
    # Warning: Aspect Ratio is not CVT standard.
    # 1800x1000 59.91 Hz (CVT) hsync: 62.19 kHz; pclk: 148.75 MHz
    Modeline "1800x1000_60.00"  148.75  1800 1912 2096 2392  1000 1003 1013 1038 -hsync +vsync

    # Warning: Aspect Ratio is not CVT standard.
    # 1904x1048 59.94 Hz (CVT) hsync: 65.15 kHz; pclk: 165.75 MHz
    Modeline "1904x1048_60.00"  165.75  1904 2024 2224 2544  1048 1051 1061 1087 -hsync +vsync

    # Warning: Aspect Ratio is not CVT standard.
    # 1864x1048 59.90 Hz (CVT) hsync: 65.11 kHz; pclk: 162.00 MHz
    Modeline "1864x1048_60.00"  162.00  1864 1984 2176 2488  1048 1051 1061 1087 -hsync +vsync

    Option "PreferredMode" "1904x1048_60.00"
EndSection
Again, CtrlO, Enter to save, then CtrlX to exit. See the Display, overscan, bezel section above for details and additional configuration options.

Now we can reboot, and the Pi Book Pro should be the primary display for the Raspberry Pi desktop. In the Terminal window, type sudo reboot. You can unplug your HDMI display, your USB keyboard and USB mouse.



                        ____________
                        
Soure: http://vitor.io/notes-pibookpro-2020#installing-on-pi-3b4-raspberry-pi-os-august-2020-displaylink-531
