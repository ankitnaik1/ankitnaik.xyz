+++
title = "Proxmox Install"
date = 2024-10-30
+++

# Guide to Setup Proxmox

This guide assume proxmox have been already installed on zfs with systemd boot manager.

## 1. Initial setup 

Open webui by going to proxmox_host_ip:8006.

Login as root, disable enterprise repository and enable no-subscription reposity

Head to Shell and update repos with 

`apt update`

and upgrade the whole system with

`apt dist-upgrade`

then reboot

`reboot`

## 2. Enable intel gvt-g to have VMs slice of gpu

### 2.1 Edit kernel Boot Option

Open /etc/kernel/cmdline with your text editor

Add following line after boot=zfs

`quiet intel_iommu=on i915.enable_gvt=1 drm.debug=0`

### 2.2 Add modules

Open /etc/modules and add

    # Modules required for PCI passthrough
    vfio
    vfio_iommu_type1
    vfio_pci
    vfio_virqfd
    
# Modules required for Intel GVT-g Split
    kvmgt

### 2.3 To update configuration

Run `update-initramfs -u -k all`

and `reboot`


