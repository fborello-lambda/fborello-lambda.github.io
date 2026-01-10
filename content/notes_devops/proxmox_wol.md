+++
title = "Proxmox WoL"
date = 2026-01-09

[taxonomies]
tags=["devops", "proxmox"]
+++

## WoL - PVE 9.1.2

1. WakeOnLan enabled in BIOS
2. Easiest way -> `ethtool`. The ubuntu docs provides a good insight.

By changing the interface with an editor:

```sh
vim /etc/network/interfaces
```

And adding the following line to the end of `vmbr0` (At least in this instalation):

```txt
post-up /usr/sbin/ethtool -s nic0 wol g || true
```

Then, after reboot; if the system is shutdown, it should turn on by sending a WoL packet.

## Resources

- [WakeOnLan - Community Help Wiki](https://help.ubuntu.com/community/WakeOnLan)
- [WakeOnLan - Debian Wiki](https://wiki.debian.org/WakeOnLan#enable)
- [Wake-on-LAN - ArchWiki](https://wiki.archlinux.org/title/Wake-on-LAN)
- [How to enable Wake On Lan ( WOL ) with Proxmox ? | Antoine Brossault](https://www.antoinebrossault.com/how-to-enable-wake-on-lan-wol-with-proxmox/)
