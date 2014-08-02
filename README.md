freebsdsystem
=============

The repository aims to describe and help a minimal FreeBSD installation with docs and scripts

piccolo machine:
HP EliteBook 2530p - L9400 Core2Duo CPU - 4GB RAM - 120GB HDD - BIOS F.22


Install
=======

ZFS pool with default settings.

With services:
- sshd
- moused
- lib32

The default groups for user should be: wheel, operator. Later can be set with:
<pre># pw groupmod [group_name] -m [user_name]</pre>

NOTE: In order to make it bootable I had to manually set the system bootable before installer reboot.
<pre># gpart set -a active ada0</pre>

Post-install
============
<pre>
# portsnap fetch
# portsnap extract
# portsnap update
</pre>
later: portsnap fetch update

Installing portmaster in order to handle ports:

As superuser
<pre># cd /usr/ports/ports-mgmt/portmaster && make install clean</pre>

<pre>
#cp /usr/local/etc/portmaster.rc.sample /usr/local/etc/portmaster.rc
#ee /usr/local/etc/portmaster.rc
</pre>

And uncomment the followings:
<pre>
# Always delete stale distfiles without prompting (-d)
ALWAYS_SCRUB_DISTFILES=dopt
# Be verbose (-v)
PM_VERBOSE=vopt
# Install packages for build-only dependencies (--packages-build)
PM_PACKAGES_BUILD=pmp_build
# Delete build-only dependencies when finished (--delete-build-only)
PM_DEL_BUILD_ONLY=pm_dbo
#Suppress the build confirmation message (--no-confirm)
PM_NO_CONFIRM=pm_no_confirm
</pre>

NOTE: Problem with ncurses can cause compilation errors later. Temp solution is pkg install ncurses!

Install Xorg
============
<pre>
# portmaster x11-servers/xorg-server</pre>
[hal] unselect [devd] selected

<pre># portmaster x11-drivers/xorg-drivers</pre>
Here the used video driver(s) and mouse+keyboard must be selected

<pre>
# portmaster x11/xinit
# portmaster x11/xauth
# portmaster x11-fonts/xorg-fonts
# portmaster x11-fonts/webfonts <NOTE: enable WIN LICENSE>
# portmaster x11-fonts/inconsolata-ttf
# portmaster x11-fonts/terminus-font
# portmaster x11-fonts/urwfonts-ttf
# portmaster x11-fonts/ubuntu-font
</pre>

Now comes the Xorg configuration:
<pre># Xorg -configure</pre>

Then copy to the common etc directory:
<pre># cp /root/xorg.conf.new /etc/X11/xorg.conf</pre>

Setting up the moused driver: [not necesary]
<pre>
# echo "moused_port=\"/dev/psm0\"" >> /etc/rc.conf
# echo "moused_enable=\"YES\"" >> /etc/rc.conf
</pre>


Xorg Environment
================

First of all the mindnight commander:
<pre># portmaster misc/mc</pre>

The zip archive extractor:
<pre># portmaster archive/unzip</pre>

Terminal emulator URXVT and shell:
<pre>
#portmaster x11/rxvt-unicode
#portmaster shells/zsh
</pre>

At the first start the zsh can be configured with a guide script and set as default shell:
<pre># chsh -s zsh</pre>

TODO: Configuration files HERE.

<pre>
# portmaster x11-wm/i3
# portmaster x11/i3status
# portmaster x11/dmenu
</pre>


Applications
============

First the web browser Opera:
<pre># portmaster www/opera</pre>
Here I had to make a little hack with libfreetype.so.9 issue. See /etc/libmap.conf.

A very simple graphical file manager Xfe:
<pre># portmaster x11-fm/xfe</pre>

GIT version control system (with gitk, subversion):
<pre># portmaster devel/git</pre>

MPlayer for music and video (GUI and skin switched off):
<pre># portmaster multimedia/mplayer</pre>

Lightweight PDF viewer:
<pre># portmaster graphics/mupdf</pre>
With JavaScript(V8) support

The mandatory Vim editor only for X11:
<pre># portmaster -m WITH_X11_ONLY=YES editors/vim</pre>

Hardware support
================

Sound
-----
The HDA based audio cards should work out of the box however on HP notebooks there is a little hack.
In /boot/device.hints there is a little pin 'magic' which solved the problem.

In /boot/loader.conf:
<pre>snd_hda_load="YES"</pre>


Suspend/Resume
--------------
Under heavy testing with acpiconf -s 3

Issues:
- DRI crashes after resume
- hdac crash after resume

Mount USB/CD/DVD
----------------
<pre># portmaster sysutils/automount</pre>
With FAT, ext4 and ntfs3g. This will install the fusefs-ntfs 

<pre>#cp /usr/local/etc/automount.conf.sample /usr/local/etc/automount.conf</pre>
This file should be edited according to needs. The more important changes are in the 
devfs.rules, devfs.conf, rc.conf, loader.conf and automount_devd.conf (this must be patched)
 

Development libraries
=====================
<pre>
# portmaster devel/boost-all
# portmaster graphics/opencv

# portmaster devel/sdl12
# portmaster graphics/sdl_gfx
# portmaster graphics/sdl_ttf
# portmaster graphics/sdl_image
# portmaster net/sdl_net
# portmaster audio/sdl_mixer
</pre>


References
==========

- Minimal FreeBSD desktop: https://forums.freebsd.org/viewtopic.php?t=35308
- FreeBSD 9 How-to: https://cooltrainer.org/2012/01/02/a-freebsd-9-desktop-how-to/
