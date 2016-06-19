# Dlink DSR-500N OpenBSD Upgrade on OSX
A quick guide to getting OpenBSD 5.9 running on the Dlink DSR-500N using an Apple Mac as the workstation.

We'll use the built-in Apple tftp server, files from the OpenBSD 5.9 Octeon release and some happy thoughts.  

Commands
========

On your Dlink:

```
setenv ipaddr 192.168.1.230
setenv serverip 192.168.1.100
tftpboot 0 bsd.rd
```

On your Mac:

```
sudo launchctl load -F /System/Library/LaunchDaemons/tftp.plist
sudo launchctl start com.apple.tftpd
sudo cp ~/Downloads/bsd.rd /private/tftpboot
sudo launchctl restart com.apple.tftpd
```

Notes
=====

Errors:

```
Attempting to allocate memory for ELF segment: addr: 0xffffffff81000000 (adjusted to: 0x0000000001000000), size 0x504890
```

Build ZRouter
=============

```
brew install vagrant
vagrant init kaorimatz/openbsd-5.9-amd64
vagrant up --provider virtualbox
vagrant ssh
```

On an OpenBSD box (or VM):

```
#!/bin/sh
# What should work, but does not `exec !! lsz -X bsd.rd`

pkg_add mercurial
mkdir -p ZRouter/
cd ZRouter
hg clone http://zrouter.org/hg/zrouter/
hg clone http://zrouter.org/hg/FreeBSD/head FreeBSD
./menu.sh
```
