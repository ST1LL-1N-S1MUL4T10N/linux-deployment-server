# Linux Deployment Server

PXE-based automated Ubuntu Desktop deployment server.

## Server

Hostname: lns-ds-001
Address: 172.16.110.10/24
Gateway: 172.16.110.1
Network: 172.16.110.0/24

## Services

dnsmasq
  DHCP + TFTP

nginx
  PXE HTTP server
  Ubuntu installer media
  NoCloud autoinstall

Technitium DNS
  172.16.110.10:53
  Domain: lds.lan

apt-cacher-ng
  172.16.110.10:3142

## Target

Ubuntu 26.04.1 Desktop AMD64

## Boot flow

DHCP
→ TFTP
→ iPXE
→ boot.ipxe
→ Ubuntu kernel/initrd
→ Ubuntu ISO
→ NoCloud autoinstall
→ Technitium DNS
→ apt-cacher-ng
→ automatic installation
→ proxy removed
→ reboot

