# Web Solution with WordPress – Step 1 (Storage Setup)

This project follows the **Web Solution with WordPress-102** guide.  
So far, we have completed the **storage configuration** on an Ubuntu EC2 instance.  

---

## 📌 Steps Completed

### 1. Launched an EC2 Instance
- Created an Ubuntu-based EC2 instance.
- Added **3 EBS volumes** (10 GiB each) in the same Availability Zone as the EC2.

### 2. Verified Attached Volumes
```bash
lsblk
df -h
```

Confirmed that the new volumes (nvme1n1, nvme2n1, nvme3n1) were available.

### 3. Partitioned the Volumes

Used gdisk to create partitions on each disk:

```
sudo gdisk /dev/nvme1n1
sudo gdisk /dev/nvme2n1
sudo gdisk /dev/nvme3n1
```

Set partition type to Linux LVM (8E00).

Verified with lsblk.

### 4. Installed LVM2

```
sudo apt update
sudo apt install lvm2 -y
sudo lvmdiskscan
```

### 5. Created Physical Volumes

```
sudo pvcreate /dev/nvme1n1p1
sudo pvcreate /dev/nvme2n1p1
sudo pvcreate /dev/nvme3n1p1
sudo pvs
```

### 6. Created a Volume Group

```
sudo vgcreate webdata-vg /dev/nvme1n1p1 /dev/nvme2n1p1 /dev/nvme3n1p1
sudo vgs
```

### 7. Created Logical Volumes

```
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg
sudo lvs
```

### 8. Formatted the Logical Volumes

```
sudo mkfs.ext4 /dev/webdata-vg/apps-lv
sudo mkfs.ext4 /dev/webdata-vg/logs-lv
```

### 9. Mounted the Volumes

Created mount points:

```
sudo mkdir -p /var/www/html
sudo mkdir -p /home/recovery/logs
```

Mounted apps-lv to /var/www/html:

```
sudo mount /dev/webdata-vg/apps-lv /var/www/html/
```

Backed up existing logs:

```
sudo rsync -av /var/log/ /home/recovery/logs/
```

Mounted logs-lv to /var/log and restored logs:

```
sudo mount /dev/webdata-vg/logs-lv /var/log
sudo rsync -av /home/recovery/logs/ /var/log
```

### 10. Made Mounts Persistent

Retrieved UUIDs:

```
sudo blkid
```

Updated /etc/fstab:

```
UUID=b6f441f7-aa32-43d4-8033-bbdac0ab2eef /var/www/html ext4 defaults 0 0
UUID=813cdb07-e008-42c9-8e01-b0f2168732af /var/log      ext4 defaults 0 0
```

Applied changes:

```
sudo mount -a
sudo systemctl daemon-reload
```

### 11. Verified Setup

```
df -h
```

Output shows:

```
/dev/mapper/webdata--vg-apps--lv   14G   ...   /var/www/html
/dev/mapper/webdata--vg-logs--lv   14G   ...   /var/log
```
