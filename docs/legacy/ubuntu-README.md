# Ubuntu PXE Media

This directory documents how to prepare the Ubuntu PXE files used by the deployment server.

## Current media

Ubuntu 26.04.1 Desktop AMD64:

```text
ubuntu-26.04.1-desktop-amd64.iso
```

The PXE server uses the kernel and initrd directly from the `casper/` directory inside the Desktop ISO.

## 1. Place the Ubuntu ISO

Copy the obtained ISO to:

```text
/var/www/html/ubuntu/ubuntu-26.04.1-desktop-amd64.iso
```

## 2. Mount the ISO

```bash
cd /var/www/html/ubuntu

sudo mkdir -p /mnt/iso
sudo mount -o loop,ro ubuntu-26.04.1-desktop-amd64.iso /mnt/iso
```

## 3. Verify the required files

```bash
ls -lh /mnt/iso/casper/vmlinuz
ls -lh /mnt/iso/casper/initrd
```

Both files must exist.

## 4. Copy the kernel and initrd

```bash
sudo cp /mnt/iso/casper/vmlinuz ./vmlinuz
sudo cp /mnt/iso/casper/initrd ./initrd
```

## 5. Unmount the ISO

```bash
sudo umount /mnt/iso
```

## 6. Set permissions

```bash
sudo chmod 644 vmlinuz initrd
sudo chown www-data:www-data vmlinuz initrd
```

## 7. Verify the final files

```bash
ls -lh \
  ubuntu-26.04.1-desktop-amd64.iso \
  vmlinuz \
  initrd
```

Expected files:

```text
ubuntu-26.04.1-desktop-amd64.iso
vmlinuz
initrd
```

## PXE boot paths

The active `boot.ipxe` uses:

```text
http://172.16.110.10/ubuntu/vmlinuz
http://172.16.110.10/ubuntu/initrd
http://172.16.110.10/ubuntu/ubuntu-26.04.1-desktop-amd64.iso
```

## Important

The `vmlinuz` and `initrd` files must come from the **same Ubuntu ISO being served**.

For example:

```text
ubuntu-26.04.1-desktop-amd64.iso
        │
        ├── casper/vmlinuz ──> /var/www/html/ubuntu/vmlinuz
        │
        └── casper/initrd  ──> /var/www/html/ubuntu/initrd
```

This is the procedure used for the working Ubuntu 26.04.1 Desktop PXE deployment.
