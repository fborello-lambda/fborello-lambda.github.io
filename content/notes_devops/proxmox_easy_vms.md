+++
title = "Proxmox Easy VMs"
date = 2026-01-09

[taxonomies]
tags=["devops", "proxmox"]
+++

## How to Create a VM Template - PVE 9.1.2

Being able to have a ready to go image to fire VMs up is extremely useful for quick and easy to use sandboxes for isolated projects.

Creating Templates from an already existing VM is really easy from the proxmox's GUI. If, for instance, an `ubuntu-server` image is used to generate the VM, it can be just set to be a `Template`, but there are some things to take into account.

## Problem - Duplicated IP

There's a catch, the `machine-id`, when using the template to create a cloned instance, the `machine-id` is duplicated, also the MAC address and consequently the IP address will not differ between instances.

A way of fixing this issue is by manually removing the `machine-id` ([systemd - Is it OK to change /etc/machine-id? - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/402999/is-it-ok-to-change-etc-machine-id)) and then creating a template out of that instance:

```sh
rm -f /etc/machine-id && dbus-uuidgen --ensure=/etc/machine-id && rm /var/lib/dbus/machine-id && dbus-uuidgen --ensure
```

Then the cloned instance should regenerate the `machine-id` the first time it boots... Well, it doesn't, the instance cannot boot properly

A second approach would be changing the IP for each new clone, but it's an ugly solution and tedious to do.

With the `ip a` command we can get the network link associated with our ip address.
In this case it's `ens18`.

So, using `vim` to write the config file:

```sh
sudo vim /etc/netplan/50-cloud-init.yaml
```

The following:

```yaml
network:
  version: 2
  ethernets:
    ens18:
        dhcp4: false
        addresses:
          - 192.168.0.254/24
        nameservers:
          addresses:
            - 8.8.8.8
            - 8.8.4.4
        routes:
          - to: default
            via: 192.168.0.1
```

And applying the changes with:

```sh
sudo netplan apply
```

The static IP address should be set. Take into account that the router plays a big role in this, it may differ quite a lot. Ideally a sub network could be made for all the VM instances so the static IP addresses doesn't alter the "main" network.

It may work but it doesn't scale well. `cloud-init` gives more flexibility.

## Solution - Cloud Init

To make it work, an ubuntu server vm can be created and configured with `cloud-init` or even simpler, an already created image can be used from [Ubuntu Cloud Images](https://cloud-images.ubuntu.com/). A QCow2 UEFI/GPT Bootable disk image is needed, it can be downloaded from the proxmox's GUI with `Download from URL` button (Also the sha256sum can be validated with the same tool)

Then, to create the template the docs have a dedicated section: [Cloud-Init Support - Proxmox VE](https://pve.proxmox.com/wiki/Cloud-Init_Support). There are some extra tweaks that may be worth mentioning:

- Set the CPU to `host`.
- Set the disk to `SSD emulation` (if an SSD is being used).
- Enlarge the template's boot disk, by doing so the cloned instances will have more disk available with no extra steps.
- Set the cloud-init file, the user, password, enable dhcp and set the ssh-keys of the PCs that need to access the VM.

There is an excellent video tutorial that explains the steps from the documentation in detail:
[Use Proxmox Cloud-Init to Deploy Your Virtual Machines! Kubernetes At Home - Part 2 - YouTube](https://www.youtube.com/watch?v=Kv6-_--y5CM)

## Resources

- [IP Address duplicates/conflicts when cloning from a template | Proxmox Support Forum](https://forum.proxmox.com/threads/ip-address-duplicates-conflicts-when-cloning-from-a-template.84237/)
- [How to Configure Static IP Address on Ubuntu 22.04 – TecAdmin](https://tecadmin.net/how-to-configure-static-ip-address-on-ubuntu-22-04/)
- [How do I request a new IP address from my DHCP server using Ubuntu Server? - Server Fault](https://serverfault.com/questions/24515/how-do-i-request-a-new-ip-address-from-my-dhcp-server-using-ubuntu-server)
- [How to Clone a VM and Assign a Different IP Address to the Clone? | Proxmox Support Forum](https://forum.proxmox.com/threads/how-to-clone-a-vm-and-assign-a-different-ip-address-to-the-clone.161232/)
- [Proxmox Home Lab - Using Cloud Init to speed up VM creation - YouTube](https://www.youtube.com/watch?v=E8heMmnS9Wc)
