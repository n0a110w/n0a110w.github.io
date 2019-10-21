---
layout: default
title: LVM
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

## How to create a Logical Volume
```bash
# initialize the physical volumes to be used with LVM. (Note: devices must be partition type 0x8E 'LVM')
pvcreate /dev/sda2 /dev/sdb5

# create a Volume Group and define the size of Physical Extents (default is 4Mb, max is 8Mb)
vgcreate -s 8M myvg0 /dev/sda2 /dev/sdb5
# or you can add the physical volumes to an existing volume group:
vgextend myvg0 /dev/sda2

# create a Logical Volume
lvcreate -L 1024M -n mystuff myvg0
# mkfs on the new logical volume
mkfs -t ext4 /dev/myvg0/mystuff
# mount the logical volume , it is ready to be used
mount /dev/myvg0/mystuff /mystuff
```

## How to increase the size of a Logical Volume
<small>Only if the underlying filesystem permits it<small>
```bash
# extend the Volume Group using the space in another disk
vgextend myvg0 /dev/sdc

# now extend the Logical Volume
lvextend -L 2048M /dev/myvg0/mystuff
# or use lvresize
lvresize -L+2048M /dev/myvg0/mystuff

# now extend the filesystem
resize2fs /dev/myvg0/mystuff
```

## How to decrease the size of a Logical Volume
<small>Only if the underlying filesystem permits it<small>
```bash
# shrink the filesystem
resize2fs /dev/myvg0/mystuff 500M

# shrink the logical volume
lvreduce -L 500M /dev/myvg0/mystuff
# or use lvresize
lvresize -L-500M /dev/myvg0/mystuff
```

## How to create snapshot and backup a Logical Volume
```bash
# create the snapshot (very similar to creating a Logical Volume)
lvcreate -s -L 1024M -n snapshot0 /dev/myvg0/mystuff

# create a compressed tarball of the snapshot (this is the your backup)
tar czvf snapshot0.tar.gz snapshot0

# delete the snapshot
lvremove /dev/myvg0/snapshot0
```

## Other Commands
### Physical Volumes
| command     | description     |
| :------------- | :------------- |
| `pvs` | display information about physical volumes |
| `pvck` | check physical volume metadata |
| `pvdisplay` | display physical volume attributes |
| `pvscan` | scan all disk for physical volumes |
| `pvremove` | remove a physical volume
| `pvmove` | move the logical extents on a physical volume |

### Volume Groups
| command | description     |
| :------------- | :------------- |
| `vgs` | display information about volume groups |
| `vgck` | check volume group metadata |
| `vgmerge` | merge two volume groups |
| `vgimport` | import a volume group into a system |
| `vgexport` | export a volume group from a system |
| `vgchange` | change volume group attributes |
| `vgextend` | add a physical volume to a volume group |
| `vgreduce` | remove a physical volume from a volume group |

### Logical Volumes
| command | description     |
| :------------- | :------------- |
| `lvs` | display information about logical volumes |
| `lvchange` | change logical volume attributes |
| `lvremove` | remove a logical volume |
| `lvscan` | scan all disks for logical volumes |


## Resources and More
{: .no_toc }
