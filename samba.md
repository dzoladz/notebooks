A basic setup using samba on a raspberry pi 4 for time machine backups

## --------------------------
## Set up Storage Block Disk
## --------------------------

# list block devices
lsblk

# Delete partitions and reboot
sfdisk --delete /dev/sda

# Write Files system
mkfs.ext4 -j -L SambaStorage /dev/sda

# List block ids
blkid

# update file system table
vim /etc/fstab

# mount all volumes in fs table
mount -a

# reboot
reboot

## -------------------------
## Install Samba
## -------------------------

# Install Samba
apt-get install samba

# Update Samba Configuration to listen only to local network
/etc/samba/smb.conf

```bash
[global]
#### macOS definitions #####
min protocol = SMB2
spotlight = yes
fruit:metadata = stream
fruit:model = MacSamba
fruit:posix_rename = yes
fruit:delete_empty_adfiles = yes

# SAMBA USER ACCOUNTS
[derek]
    path = /samba/users/derek
    browseable = yes
    writeable = yes
    read only = no
    fruit:aapl = yes
    fruit:time machine = yes
    vfs objects = catia fruit streams_xattr
    force create mode = 0660
    force directory mode = 2770
    valid users = derek
```

# Test Config after Changes
testparm

# Restart services
systemctl restart smbd
systemctl restart nmbd

## -----------------------
## Create users
## -----------------------

useradd -M -d /samba/users/derek -s /usr/sbin/nologin -G sambashare derek
mkdir /samba/users/derek
chown derek:sambashare /samba/users/derek
chmod 2770 /samba/users/derek
smbpasswd -a derek
smbpasswd -e derek

## ------------------------
## Connect from macOS
## ------------------------

# Connecting from macOS on local network static IP
# LAN DNS via PiHole
smb://backup.derekzoladz.com/derek
