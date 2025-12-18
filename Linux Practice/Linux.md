<div style="text-align:center;">

# First modul - **Linux**
</div>

<div align="center">

## 1. Unix-based OS’s

</div>

### Introduction to Unix-based Operating Systems

- **1969–1970** – Ken Thompson and Dennis Ritchie implemented and released the **Unix** operating system.  
- **1977** – Berkeley Software Distribution (BSD) released a free operating system project containing Unix code.  
- **1983** – Richard Stallman started the **GNU** project with the goal of creating a free Unix-like operating system.  
- **1986** – Maurice J. Bach published *The Design of the UNIX Operating System*.  
- **1987** – **MINIX**, a Unix-like system intended for academic use, was released by Andrew S. Tanenbaum.  
- **1991** – Linus Torvalds began a project that later became the **Linux kernel**, and on 25 August 1991 he announced it in the Usenet newsgroup "comp.os.minix". Development was done on MINIX using the GNU C Compiler.  
- **1992** – The Linux kernel was released under the **GNU GPL** license.  
- **1996** – The official Linux mascot, a penguin , was introduced.  

### Linux

- A Unix-like computer operating system.  
- Developed under the model of **free and open-source software**.  
- The defining component of Linux is the **Linux kernel**.  

### Kernel

The kernel is a computer program that serves as the **core of the operating system**, with full control over everything in the system:  

- It is one of the first programs loaded after the bootloader.  
- Handles the rest of the system startup.  
- Processes software requests, translating them into instructions for the CPU.  
- Manages memory and peripherals such as keyboards, monitors, printers, and speakers.  

---

Unix-based OS’s are built on **Unix principles** and serve as the foundation for Linux and other Unix-like systems.
---

<div align="center">

## 2. Linux Distributions

</div>

A Linux distribution is a member of the family of **Unix-like software distributions** built on top of the Linux kernel. There are currently over **three hundred Linux distributions**. Most of them are in active development, constantly being revised and improved.  

![Linux Distribution Timeline]https://upload.wikimedia.org/wikipedia/commons/1/1b/Linux_Distribution_Timeline.svg)

For a detailed list of distributions, see [Wikipedia: List of Linux distributions](https://en.wikipedia.org/wiki/List_of_Linux_distributions).

<div align="center">

## 3. Shell

</div>

### What is a Shell

A **shell** is a program that provides an interface between a user and an operating system (OS) kernel.

---

### Basic Architecture of the Shell

The fundamental architecture of a shell is not complex. It works like a **pipeline**:  

1. Input is analyzed and parsed.  
2. Symbols are expanded using methods such as:  
   - Brace expansion  
   - Tilde expansion  
   - Variable and parameter expansion and substitution  
   - Filename generation  
3. Commands are executed using **shell built-in commands** or **external commands**.

---

### Types of Shells

- **The Bourne Shell (sh)**  
- **The C Shell (csh)**  
- **The Korn Shell (ksh)**  
- **The GNU Bourne-Again Shell (bash)**  

---

### Comparison of Shells

| Shell                     | Path           | Default Prompt (non-root) | Default Prompt (root) |
|----------------------------|----------------|--------------------------|---------------------|
| The Bourne Shell (sh)      | /bin/sh, /sbin/sh | $                        | #                   |
| The C Shell (csh)          | /bin/csh       | %                        | #                   |
| The Korn Shell (ksh)       | /bin/ksh       | $                        | #                   |
| GNU Bourne-Again Shell (bash) | /bin/bash   | bash-x.xx$               | bash-x.xx#          |

---

### Additional Materials

For more details, see: [Difference Between Bash, Zsh, and Other Linux Shells](https://www.howtogeek.com/68563/htg-explains-what-are-the-differences-between-linux-shells/)



<div align="center">

## 4. File Systems

</div>

### Unix/Linux vs Windows File Systems

**Unix/Linux:**
- Supports many file system types: ext2, ext3, ReiserFS, etc.  
- Uses a **single hierarchy (tree) concept** – one root directory (`/`), and every file exists somewhere under it.  
- No strict `filename.extension` concept – the system doesn’t assume anything about a file’s content based on its extension (extensions are optional).  

**Windows:**
- Supports only NTFS, FAT16 & FAT32 file systems.  
- Uses **drive letter abstraction** – files are located on disks or partitions represented by letters (A:, B:, C:, … Z:).  
- System behavior is influenced by file extensions (`.exe` is executable, `.txt` is a text file, etc.).

---

### Inodes

An **inode (index node)** is a data structure in a Unix-style file system that describes a file-system object (file or directory).  

- Each inode stores:  
  - Attributes (metadata: last modification, access, change times)  
  - Owner and permission data  
  - Disk block locations of the object’s data  
- **Directories** are lists of names assigned to inodes.  
  - Each directory contains an entry for itself (`.`), its parent (`..`), and each of its children.

---

### Hard Links

- A **hard link** is a direct pointer to an inode.  
- Cannot cross file systems (inodes are unique only within a file system).  
- Indistinguishable from the original file – changes in one reflect in all.  
- The file remains accessible as long as at least one hard link exists.  
- Created using `ln` (without options) or the `link` function.  

---

### Soft Links (Symbolic Links)

- A **soft link** (symlink) is a pointer to a **file name**.  
- If the original file is moved, the link becomes invalid.  
- Deleting the link does **not** delete the original file.  
- Usually used to create shortcuts to directories.  
- Created using `ln -s` or the `symlink` function.

<div align="center">

## 5. LVM (Logical Volume Manager)

</div>

### What is LVM

LVM (Logical Volume Manager) is a system for managing disk space in Linux that allows **more flexible volumes** than normal disk partitions.

---

### Advantages

- Combine multiple physical disks into **one large logical volume**.  
- Logical volumes can **span multiple disks**.  
- Resize logical volumes **dynamically** without worrying about their position on the disk.  
- Create, delete, or resize logical and physical volumes **online**.  
- **Snapshots** allow backups of the filesystem with minimal downtime.  
- Live migration of volumes between disks **without stopping services**.

---

### Disadvantages

- Mostly **Linux only**; other OSs (Windows, FreeBSD) have little or no support.  
- More steps are required during setup, making it more complicated.  
- If using **Btrfs** with Subvolumes, LVM may be unnecessary.

---

### Steps to Create an LVM Logical Volume

1. **Initialize Physical Volumes (PV)**  
   ```bash
   sudo pvcreate /dev/sdb1
   sudo pvscan
   sudo pvdisplay
2. **Create a Volume Group (VG)**  
   ```bash
   sudo vgcreate vg_newlvm /dev/sdb1
   sudo vgcreate vg_newlvm /dev/sdb1 /dev/sdc1 /dev/sdc2
3. **Create a Logical Volume (LV)**  
   ```bash
   sudo lvcreate --name centos7_newvol -l 100%FREE vg_newlvm
   sudo lvdisplay
4. **Create a Filesystem on the LV**  
   ```bash
   sudo mkfs.ext4 /dev/vg_newlvm/centos7_newvol   # ext4
   sudo mkfs.btrfs /dev/vg_newlvm/centos7_newvol  # btrfs
   sudo mkfs.ntfs /dev/vg_newlvm/centos7_newvol   # ntfs
<div align="center">

## 6. SWAP

</div>

### What is SWAP

**Swap space** in Linux is used when the amount of physical memory (RAM) is full. If the system needs more memory resources and RAM is full, **inactive pages in memory are moved to swap space**.  

- Swap helps systems with limited RAM, but **it does not replace RAM** because disks are much slower than memory.  
- Swap can be:  
  - A **dedicated swap partition** (recommended)  
  - A **swap file**  
  - A **combination** of swap partitions and swap files  

> Note: The **Btrfs filesystem does not support swap space**.

---

### Swap Files vs Swap Partitions

- In Linux kernel 2.6.x and later, **swap files are almost as fast as swap partitions**, as long as the swap file is **contiguously allocated**.  
- Swap partitions on HDDs can be placed on **contiguous areas**, giving slightly higher throughput and faster seek times.  
- Swap files provide **administrative flexibility**:  
  - Can be placed on any mounted filesystem  
  - Can be resized or created as needed  
- Swap partitions cannot easily be resized without repartitioning or using volume management tools.

---

### Swappiness

**Swappiness** is a Linux kernel parameter that controls **how aggressively the system uses swap** versus keeping data in RAM page cache.  

- Values range from **0 to 100**:  
  - Low value → system prefers evicting pages from the cache and keeps RAM usage high  
  - High value → system prefers swapping “cold” memory pages to disk  
- Default value: **60**  
- Adjusting swappiness:  
  - Desktops with plenty of RAM → lower value for consistent low latency  
  - Servers or batch systems → higher value for better overall throughput  

---

### Checking SWAP
    To check if swap is active:  
    ```bash
    cat /proc/swaps
    free -h

### Adding SWAP as an LVM Logical Volume  
1. #### Create a 2 GB logical volume for swap:
   ```bash 
   sudo lvcreate VolGroup00 -n LogVol02 -L 2G
2. #### Format it as swap 
   ```bash
   sudo mkswap /dev/VolGroup00/LogVol02
3. #### Add it to /etc/fstab:
   ```bash
   /dev/VolGroup00/LogVol02 swap swap defaults 0 0
4. #### Reload systemd units:
   ```bash
   sudo systemctl daemon-reload
5. #### Activate swap immediately:
   ```bash
   sudo swapon -v /dev/VolGroup00/LogVol02
### Creating a Swap File
1. #### Determine size (e.g., 64 MB → 65536 blocks of 1024 bytes):
   ```bash
   sudo dd if=/dev/zero of=/swapfile bs=1024 count=65536
2. #### Set up the swap file
   ```bash
   sudo mkswap /swapfile
3. #### Restrict permissions:
   ```bash
   sudo chmod 0600 /swapfile
4. #### Add to **/etc/fstab** for aautomatic activation at boot
   ```bash
    /swapfile swap swap defaults 0 0
5. #### Reloade systemd units:
   ```bash
   Sudo systemctl daemon-reload
6. #### Activate swap immediately:
   ```bash
   sudo swapon /swapfile
7. #### Verify:
   ```bash
   cat /proc/swaps
   free -h
### Removing a Swap File
1. #### Disable the swap file:
   ```bash
   sudo swapoff -v /swapfile
2. #### Remove entry from **/etc/fstab**.
3. #### Reload systemd units:
   ```bash
   sudo systemctl daemon-reload
4. #### Delete the actual file:
   ```bash
   sudo rm /swapfile
<div align="center">

## 7. Disk Quotas

</div>

### What are Disk Quotas

**Disk quotas** allow a system administrator to **restrict and manage disk usage** for users or groups. They help prevent a single user from consuming all disk space or filling up a partition completely.  

- Quotas can be configured for **individual users** or **user groups**.  
- This allows managing **user-specific files** (e.g., emails) separately from **project files** if projects are assigned their own groups.  
- Quotas can control:  
  - **Number of disk blocks used** (space)  
  - **Number of inodes used** (number of files), since inodes store metadata about files in Unix/Linux file systems  

> Controlling inodes ensures that a user cannot create unlimited numbers of small files even if disk space is available.  

---

### Benefits of Disk Quotas

- Prevents a single user from filling up the disk  
- Helps manage resources for multi-user systems  
- Provides fine-grained control over storage usage per user or group  

---

### Reference

For more detailed configuration and examples, see:  
[Red Hat Disk Quotas Documentation](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/ch-disk-quotas.html)


<div align="center">

## 8. Boot Loaders

</div>


### What is a Boot Loader

When a computer is powered off, its software (including operating systems, application code, and data) remains stored on **non-volatile memory**. When the computer is powered on, it usually does **not have an operating system in RAM** yet.  

- The computer first executes a small program stored in **ROM** (Read-Only Memory) along with minimal data needed to access the non-volatile storage.  
- This small program is called the **bootstrap loader**, **bootstrap**, or **boot loader**.  
- Its main job is to **load other programs and data into RAM**, so the operating system can run.  

Often, **multi-stage boot loaders** are used, where several programs of increasing complexity are loaded one after another, a process called **chain loading**.

---

### Common Linux Boot Loaders

#### LILO (Linux Loader)

- Comes standard on older Linux distributions  
- Older and less powerful  
- Originally **did not include a GUI menu**  
- Simple, reliable for single-boot systems  

#### GRUB (GRand Unified Bootloader)

- Easier to administer than LILO  
- Supports **command-line interface**, **network boot**, **MD5 passwords**  
- Allows multi-boot systems with menu selection  
- Configuration files:  
  - `/boot/grub/grub.conf`  
  - `/etc/grub.conf`  


<div align="center">

## 9. Runlevels

</div>

### What is a Runlevel

A **runlevel** defines the **state of the system after boot**.  
Each runlevel determines **which services are started or stopped** and how the system behaves.

In traditional Unix/Linux systems (SysVinit), runlevels are numbered from **0 to 6**.

---

### Classic SysVinit Runlevels

| Runlevel | Description |
|--------|-------------|
| **0** | Halt (shutdown the system) |
| **1** | Single-user mode (maintenance / rescue, no network) |
| **2** | Multi-user mode (varies by distribution) |
| **3** | Multi-user mode with networking, **no GUI** |
| **4** | Custom / unused |
| **5** | Multi-user mode with **graphical interface (GUI)** |
| **6** | Reboot |

- Runlevels **3 and 5** are most commonly used
- Lower runlevels are useful for **system maintenance and recovery**

---

### SysVinit Service Management

Services were controlled using directories:

```bash
/etc/rc[0-6].d/
```
#### Example
    S10network -> ../init.d/network
    K20nfs     -> ../init.d/nfs
* S = start service
* K = stop service
* Numbers define execution order

#### The default runlevel was defined in:
    /etc/inittab
#### Example:
    id:3:initdefault:
> This means the system boots into runlevel 3 by default.
