# ZFS
I use ZFS to manage my hard drives outside of the boot drive, so this is everything I've learnt about ZFS. Very incomplete.

## Creating a volume
1. We first need to create the volume
    ```
    zfs create path/of/volume
    ```
    We can then set the size of the volume via the next guide, if needed.


## Set size for volumes
1. Check the current quota for each volume.
   ```
   zfs get quota
   ```
2. Increase the quota.
   ```
   zfs set quota=<new quota> path/of/volume
   zfs set reservation=<new quota> path/of/volume
   ```

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