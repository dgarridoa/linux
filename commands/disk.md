# Disk Management

Partitions
- Disk can be divided into parts, called parttions. Partitions allow you to separate data.
- Partitions schemes
  - 1) OS, 2) Application, 3) User, 4) Swap
  - 1) OS, 2) User home directories
  - etc
- Can protect the overall system
- Keep users from creating outages by using a home directory partition

MBR
- MBR = Master Boot Record
- Can only address 2 TB of disk space
- being phased out by GPT
- 4 primary partitions
- Extended partitions allow you to create logical partitions

GPT
- GPT = GUID Partition Table
- GUID = Global Unique Identifier
- Replacing the MBR partitionin scheme
- Part of UEFI
- UEFI = Unified Extensible Firmware Interface
- UEFI is replacing BIOS
- Supports up to 128 partitions and 9.4 ZB disk sizes
- Not supported by older operating systems

Mount Points
- A directory used to access the data on a partition
- /(slash) is always a mount point
- /home
- /export/home
- you can mount over existing data
  - `mkdir /home/sarah`
  - `mount /dev/sdb2 /home`
  - you will not be able to see /home/sarah
  - `unmount /home`
  - you can now see /home/sarah again

Partitioning tools
- `fdisk`
- `gdisk`
- `parted`

- `fdisk `
  - `device`: is usually /dev/sda, /dev/sdb or so. A device name refers to the entire disk.
  - `-l`: list the partition tables.

Filesystems
- `mkfs -t TYPE DEVICE`: to create a file systems. Example: `mkfs -t ext3 /dev/sdb2`.
- `ls -1 /sbin/mkfs**`: to see all type of file systems.

Mounting
- `mount DEVICE MOUNT_POINT`: `mount /dev/sdb4 /opt`
- `unmount DEVICE_OR_MOUNT_POINT`: `unmount /opt` or `/dev/sdb4 `
- `df -h`: report the filesystem usage
- `/etc/fstab`: add an entry to the file to make mounts persist between reboots.

Swap Area: in place of creating a filesystem and mounting it. 
- `mkswap DEVICE`: to prepares a swap partition for use.
- `swapon DEVICE`: to enable a swap partition.
- `swapon -s`: to see swap devices.

The File System Table
- `/etc/fstab`
- Controls what devices get mounted and where on boot.


# LVM: Logical Volume Manager

LVM introduces layers of abstraction including:
- Physical Volumes (PVs).
- Volume Groups (VGs).
- Logial Volumes (LVs)

Create one or more PVs from unused devices, create a VG from those one or more PVs, create one or more LVs from the VG. A LV is like a disk partition.

Uses:
1. **Flexible Capacity**: create file systems that extend across multiple storage devices.
2. **Easily Resize Storage While Online**: expand or shrink file systems in real-time while the data remains online and fully accessible.
3. **Online Data Relocation**: easily migrate data from one to anothe while online.
4. **Convenient Device Naming**: human-readable device names of your choosing.
5. **Disk Striping**: increase throughput by allowing your system to read data in parallel.
6. **Data Redundancy/Data Mirroring**: increase fault tolerance and reliability by having more than one copy of your data.
7. **Snapshots**: create point-in-time snapshots of your file systems.

- `lvmdiskscan`: list devices that may be used as physical volumes.
- `lsblk`: list block devices.
  - `-p`: print full device paths.
- `df`: report file system disk space usage.
  - `-h`: print sizes in a human readable format, in porwers of 1024.
- `fdisk`: manipulate disk partition table.
  - `-l`: list the partition tables for the specified devices and then exit.

## Physical Volume (PV)
- `pvcreate device`: create a physical volume. LVM will only create a pv label on a device if it is not currently in use.
  - `pvcreate /dev/sdb1`
- `pvremove path`: remove physical volume.
- `pvmove pv1_path pv2_path`: migrate data from PV1 to PV2. This task can be done online.
- `pvs`: list of physical volumes.
- `pvdisplay`: display physical volumes information.
  - `-m`: display the mapping of physical extents to LVs and logical extents.

## Volume Group (VG)
- `vgcreate vg_name pv1_name ... pvn_name`: create a volume group.
- `vgremove vg_name`: remove a volume group.
- `vgextend vg_name pv1_name ... pvb_name`: extend a volume group.
- `vgreduce vg_name pv1_name ... pvb_name`: reduce a volume group.
- `vgs`: list of volume groups.
- `vgdisplay`: display volume groups information.

## Logical Volume (LV)
- `lvcreate -L Size -n lv_name vg_name`: create a logical volume from a volume group.
  - `lvcreate -L 20G -n lv_data vg_app`
  - `lvcreate -l 10%FREE -n lv_logs vg_app`
- `lvremove lv_path`: remove a logical volume.
- `-m NumberCopies`: specifies the number of mirror images in addition to the original LV image. The mirror are allocated in different partitions/devices.
- `lvextend -L +Size -r lv_path`: extend size of a logical volume.
  - `resize2fs lv_path`: if `-r` argument is not given this solve the mistake.
- `lvs`: list of logical volumes.
  - `-a`: display all logical volumes, including mirrors.
- `lvdisplay`: display information about a logical volume. Shows the attribute of LVs, like size, path, read/write status, snapshot, information, etc.
  - `-m`: display the mapping of logical extents to PVs and physical extents (PEs). For each 5 GB are attaching 1280 MB  to metadata.
- `mkfs -t type device`: build a linux filesystem.
  - `mkfs -t ext4 /dev/vg_app/lv_data`
- `mount device /dir`: mounts a storage device or filesystem, making it accessible and attaching it to an existing directory structure.
  - `mount /dev/vg_app/lv_daa /data`
  - ```
    # /etc/fstab:
    /dev/vg_app/lv_app /app ext4 defaults 0 0

    hostname:/# mount /app
    ```
- `umount /dir`: unmount file systems. Must be executed before to remove the LV.