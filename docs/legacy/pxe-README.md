# PXE / iPXE

This documents the PXE bootloader setup used by the deployment server.

## TFTP root

The TFTP root is:

```text
/srv/tftp
```

The final PXE configuration expects these two files:

```text
/srv/tftp/ipxe.efi
/srv/tftp/undionly.kpxe
```

## Install iPXE

The deployment server installed the Ubuntu iPXE package together with the other PXE services:

```bash
sudo apt-get install -y ipxe dnsmasq nginx python3-yaml
```

## UEFI iPXE loader

The UEFI loader was copied from the installed iPXE package:

```bash
sudo mkdir -p /srv/tftp
sudo cp -L /usr/lib/ipxe/ipxe.efi /srv/tftp/ipxe.efi
sudo chmod 644 /srv/tftp/ipxe.efi
```

Verify:

```bash
file /srv/tftp/ipxe.efi
```

## BIOS iPXE loader

The BIOS loader was obtained directly from the official iPXE download location:

```bash
sudo curl -fL https://boot.ipxe.org/undionly.kpxe \
  -o /srv/tftp/undionly.kpxe
sudo chmod 644 /srv/tftp/undionly.kpxe
```

Verify:

```bash
file /srv/tftp/undionly.kpxe
```

## dnsmasq integration

dnsmasq serves `/srv/tftp` and selects the loader based on the client architecture and whether the client is already running iPXE.

The relevant configuration is in:

```text
configs/dnsmasq/pxe.conf
```

The final configuration uses:

```text
enable-tftp
tftp-root=/srv/tftp

dhcp-boot=tag:!ipxe,tag:efi64,ipxe.efi
dhcp-boot=tag:!ipxe,tag:!efi64,undionly.kpxe
dhcp-boot=tag:ipxe,http://172.16.110.10/boot.ipxe
```

This produces the two-stage boot process:

```text
PXE client
    ↓
dnsmasq DHCP
    ↓
TFTP
    ↓
ipxe.efi / undionly.kpxe
    ↓
iPXE DHCP
    ↓
HTTP
    ↓
http://172.16.110.10/boot.ipxe
```

## Verify TFTP files

```bash
ls -lh /srv/tftp
file /srv/tftp/ipxe.efi
file /srv/tftp/undionly.kpxe
```

## Important

The iPXE binaries are deployment-server files under `/srv/tftp`.

The repository stores the configuration and this procedure, not the binary files themselves.
