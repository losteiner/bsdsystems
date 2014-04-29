freebsdsystem
=============

The repository aims to describe and help a minimal FreeBSD installation with docs and scripts

Install
=======

ZFS pool with default settings.

With services:
- sshd
- moused
- lib32

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

NOTE: Problem with ncurses can cause compilation errors later. Check here!

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

References
==========

- Minimal FreeBSD desktop: https://forums.freebsd.org/viewtopic.php?t=35308
- FreeBSD 9 How-to: https://cooltrainer.org/2012/01/02/a-freebsd-9-desktop-how-to/
