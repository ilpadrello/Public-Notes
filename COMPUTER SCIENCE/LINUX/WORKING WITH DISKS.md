### LSBLK :  List connected drives
```
lsblk
```
`lsblk` lists information about all available or the specified block devices. 
The `lsblk` command reads the `sysfs` filesystem and `udev` db to gather information. 
If the `udev` db is not available or `lsblk` is compiled without `udev` support, then it tries to read LABELs, UUIDs and filesystem types from the block device. In this case root permissions are necessary.

The command prints all block devices (except RAM disks) in a tree-like format by default. Use `lsblk --help` to get a list of all available columns.

The default output, as well as the default output from options like `--fs` and `--topology`, is subject to change. So whenever possible, you should avoid using default outputs in your scripts. Always explicitly define expected columns by using `--output` columns `-list` and `--list` in environments where a stable output is required.

Note that `lsblk` might be executed in time when `udev` does not have all information about recently added or modified devices yet. In this case it is recommended to use udevadm settle before lsblk to synchronize with udev.

The relationship between block devices and filesystems is not always one-to-one. The filesystem may use more block devices, or the same filesystem may be accessible by more paths. This is the reason why lsblk provides MOUNTPOINT and MOUNTPOINTS (pl.) columns. The column MOUNTPOINT displays only one mount point (usually the last mounted instance of the filesystem), and the column MOUNTPOINTS displays by multi-line cell all mount points associated with the device.

### DF : Getting used spaces
```
df
df -h //for humain readable
```

## 📌 **Understanding `lsblk` Output**

`lsblk` lists all block devices (disks, partitions, and LVM volumes). Here's what your output tells us:

- **You have 5 disks**:
    
    - `sda` (2.7TB)
    - `sdb` (1.8TB, has 1 partition: `sdb1`)
    - `sdc` (465GB, has 3 partitions: `sdc1`, `sdc2`, `sdc3`)
    - `sdd` (2.7TB, has 2 partitions: `sdd1`, `sdd2`)
    - `sde` (2.7TB, has 2 partitions: `sde1`, `sde2`)
- **Your system is running from `sdc`**:
    
    - `sdc1` → `/boot/efi` (512MB, EFI boot partition)
    - `sdc2` → `/boot` (488MB, stores Linux kernel and boot files)
    - `sdc3` → Contains **LVM** (Logical Volume Manager), split into:
        - `vox--vg-root` (root `/` filesystem)
        - `vox--vg-swap_1` (swap space)

---

## 📌 **Understanding `df -h` Output**

The `df -h` command shows mounted filesystems and their disk usage.

### **What is the "Filesystem" column?**

- It lists the "logical" storage devices that your system recognizes.
- Some are **actual disks**, while others are **virtual filesystems**.

|Filesystem|Meaning|
|---|---|
|`/dev/mapper/vox--vg-root`|Your root filesystem (`/`), managed by **LVM** (more on that later).|
|`/dev/sdc2`|The `/boot` partition (contains Linux kernel & boot files).|
|`/dev/sdc1`|The `/boot/efi` partition (needed for UEFI boot).|
|`udev`|A virtual filesystem for device management.|
|`tmpfs`|A temporary filesystem stored in RAM.|

---

## 📌 **What Are `udev` and `tmpfs`?**

These are **virtual filesystems**, not actual disks.

- **`udev`** → Manages device files in `/dev/`.
    
    - It dynamically creates device nodes when hardware is plugged in.
    - Example: `/dev/sda` appears when a new disk is connected.
- **`tmpfs`** → A temporary in-memory filesystem.
    
    - It's used for performance optimization.
    - Example: `/run/` and `/dev/shm/` store temporary files.

---

## 📌 **What Are Partitions?**

A single disk can be split into **multiple partitions**.

- `sdc` has **3 partitions**: `sdc1`, `sdc2`, `sdc3`
    - `sdc1` → EFI partition (`/boot/efi`)
    - `sdc2` → Boot partition (`/boot`)
    - `sdc3` → Contains the main Linux system (inside an **LVM volume**)

A partition allows a disk to be used for multiple purposes (e.g., dual-booting Windows & Linux).

---

## 📌 **What Is LVM (`/dev/mapper/vox--vg-root`)?**

LVM (Logical Volume Manager) is an abstraction layer over physical disks.

- Instead of using raw partitions, **LVM creates flexible "logical volumes"**.
- This allows you to **resize**, **merge**, or **move** storage more easily.
- Your root filesystem `/` is inside an LVM volume called `vox--vg-root`.

Think of LVM like a **virtual hard drive inside your real hard drive**.

---

## 📌 **Why Aren’t All Disks Automatically Mounted?**

By default, Linux **only mounts essential filesystems** at boot.

- Other disks (`sda`, `sdb`, `sdd`, `sde`) **are detected but not mounted**.
- This is to prevent issues (e.g., data corruption, unwanted auto-mounting).
- You must **manually mount** them or configure automatic mounting (`/etc/fstab`).

---

## 📌 **What Does "Mounting" Mean?**

**Mounting** means **attaching a storage device to a specific folder in your filesystem**.

- In Windows, drives appear as **C:, D:, E:**.
- In Linux, they appear **inside folders** (e.g., `/mnt/disk1`).

### 🔧 **Example: Mounting `sdb1`**

1️⃣ **Create a mount point:**

bash

CopyEdit

`sudo mkdir -p /mnt/disk1`

2️⃣ **Mount the partition:**

bash

CopyEdit

`sudo mount /dev/sdb1 /mnt/disk1`

3️⃣ **Check if it’s mounted:**

bash

CopyEdit

`df -h | grep /mnt/disk1`

4️⃣ **Access your files:**

bash

CopyEdit

`ls /mnt/disk1`

### 🔄 **Unmounting a Disk**

If you want to **detach the disk**, run:

bash

CopyEdit

`sudo umount /mnt/disk1`

---

## 📌 **Final Summary**

|Concept|Meaning|
|---|---|
|`sda`, `sdb`, `sdc`|Physical disks|
|`sdc1`, `sdc2`, `sdc3`|Partitions on a disk|
|`udev`|Virtual filesystem for device management|
|`tmpfs`|RAM-based temporary filesystem|
|`df -h`|Shows mounted filesystems & usage|
|`mount`|Attaches a disk to a folder|
|`umount`|Detaches a disk|
|`LVM`|A flexible storage management system|

Would you like help setting up **automatic mounting** for your extra drives? 🚀