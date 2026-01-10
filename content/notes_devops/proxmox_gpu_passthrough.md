+++
title = "Proxmox GPU passthrough"
date = 2026-01-09

[taxonomies]
tags=["devops", "proxmox"]
+++

## GPU passthrough - PVE 9.1.2

1. Run the post-install script
2. Enable IO-MMU

In the BIOS menu:
AMD CBS > NBIO Common Options > IOMMU: Enabled.

In the proxmox's shell check if it's enabled:

```sh
dmesg | grep IOMMU
```

Add amd_iommu=on iommu=pt to GRUB_CMDLINE_LINUX_DEFAULT in /etc/default/grub:

```sh
vi /etc/default/grub
```

Update and reboot:

```sh
update-grub && update-initramfs -u -k all && reboot
```

1. Blacklist the driver

```sh
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
echo "blacklist nvidia*" >> /etc/modprobe.d/blacklist.conf
```

Then reboot. After reboot, the GPU could be selected as PCIe device by a VM instance.

### Ollama

- NOTICE the orange changes in the VM->Hardware gui means that a reboot is needed.
- install `nvidia-drivers-580` and `nvtop`
- install `ollama`
- May be needed -> [Forcing Ollama to bind to 0.0.0.0 instead of localhost · Issue #5905 · ollama/ollama](https://github.com/ollama/ollama/issues/5905)
  - If running local VMs and want to connect them with ollama without hassle.

## Resources

- [Host a Private AI Server at Home with Proxmox Ollama and OpenWebUI - YouTube](https://www.youtube.com/watch?v=y5-6qww8uKk)
- [Run Ollama with NVIDIA GPU in Proxmox VMs and LXC containers - Virtualization Howto](https://www.virtualizationhowto.com/2025/05/run-ollama-with-nvidia-gpu-in-proxmox-vms-and-lxc-containers/)

- [PCI Passthrough - Proxmox VE](https://pve.proxmox.com/wiki/PCI_Passthrough)
