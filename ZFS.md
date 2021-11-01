# ZFS
I use ZFS to manage my hard drives outside of the boot drive, so this is everything I've learnt about ZFS. Very incomplete.

## Creating a pool
1. First, you need to determine which drives you're using, and which ZFS RAID type you'll be using. For our purposes, we'll be using RAID-Z1
   ```
   lsblk
   ```
2. After determining the drives you'll be using, you can create your pool.
   ```
   zpool create <pool name> <disk1> <disk2> <disk3>
   ```

## Creating a volume
1. We first need to create the volume
   ```
   zfs create path/of/volume
   ```

   Turning on compression is almost always a good idea, since it reduces the space needed, while not having any noticable downsides to it.
   ```
   zfs set compression=lz4 path/of/volume.
   ```
   Note: compression only works on new data, so it won't work if compression is enabled after the fact.

   We can then set the size of the volume via the next guide, if needed.


## Set size for volumes
1. Check the current quota & reservation for each volume.
   ```
   zfs get reservation path/to/pool
   zfs get quota path/to/pool
   ```
2. Increase the quota & reservation.
   ```
   zfs set reservation=<new quota>g path/of/volume
   zfs set quota=<new quota>g path/of/volume
   ```
   The quota amount cannot be less than the currently used or reserved space.  

   You can use the quota property to set a limit on the amount of disk space a file system can use. In addition, you can use the reservation property to guarantee that a specified amount of disk space is available to a file system. Both properties apply to the dataset on which they are set and all descendents of that dataset.

## Mounting volumes
By default, volumes are mounted to `/<poolname>/<volume name>`, but we can change this.
Set the mountpoint
```
zfs set mountpoint=/path/to/mount path/of/volume
```
The new volume should then be mounted to the specified mountpoint.


## Detailed disk usage by pool
```
zfs list -t all -o space -r tank
```

## Delete snapshots
```
zfs destroy tank@snap1
```

## Delete Filesystems
```
zfs destroy path/to/tank
```

`-f` can be used to force the deletion of a tank, in a case where the tank is being used. This option can cause unexpected behaviour.
```
zfs destroy -f path/to/tank
```

`-r` can be used to delete a tank and all of its descendents.
```
zfs destroy -r path/to/tank
```