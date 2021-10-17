# ZFS
I use ZFS to manage my hard drives outside of the boot drive, so this is everything I've learnt about ZFS. Very incomplete.

## Creating a volume
1. We first need to create the volume
    ```
    zfs create path/of/volume
    ```
    We can then set the size of the volume via the next guide, if needed.


## Set size for volumes
1. Check the current quota & reservation for each volume.
   ```
   zfs get quota
   zfs get reservation
   ```
2. Increase the quota & reservation.
   ```
   zfs set quota=<new quota> path/of/volume
   zfs set reservation=<new quota> path/of/volume
   ```
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