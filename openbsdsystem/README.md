openbsdsystem
=============

The repository aims to describe and help a minimal OpenBSD (currently 6.0) installation with docs and scripts

piccolo machine:
HP EliteBook 2530p - L9400 Core2Duo CPU - 4GB RAM - 120GB HDD - BIOS F.22


Requirements
============
- Notebook features (sleep, awake, CD/DVD mount, USB mount)
- Lightweight Desktop Environmrnt (Openbox, Fluxbox, i3, Lumina)
- File manager (Xfe, PCManFM, Insight, etc...)
- Browsing (Firefox/Chromium, Midori, dwb)
- Modern console solutions (terminal emulation, shell)
- Development environment (C/C++, Python, Matlab/Octave)
- Cross compiler for embedded systems (ARM)
- WLAN capable (various networks)
- Multimedia playback (VLC, ffmpeg/avconv)
- Office and Publication (LibreOffice, LaTeX, printing)


Install
=======


Post-install
============
<pre>
# export PKG_PATH=http://ftp.OpenBSD.org/pub/OpenBSD/6.0/packages/$(uname -m)
# pkg_add -Iv nano mc
</pre>

Add the user to <b>doas</b> in order to make the setup faster:
<pre>
# user mod -G wheel <USERNAME>
</pre>

Setup the Slim as desktop manager:
<pre>
# pkg_add -Iv slim slim-themes
</pre>

Then add the following lines to <b>/etc/rc.local</b>
<pre>
+ /usr/local/bin/slim -d
</pre>

... and add the following lines to <b>/etc/rc.conf.local</b> DO WE NEED DBUS??
<pre>
+ pkg_scripts="dbus_daemon avahi_daemon"
+ dbus_enable=YES
</pre>


User-space install
==================

Let's pick a desktop environment:
<pre>
# pkg_add -Iv lumina
</pre>

<pre>
# pkg_add -Iv firefox vlc git
</pre>

References
==========

- https://news.ycombinator.com/item?id=13223351
- http://sohcahtoa.org.uk/openbsd.html
- https://sivers.org/openbsd
- http://daemonforums.org/showthread.php?p=60188
- http://www.bsdnow.tv/tutorials/the-desktop-obsd
