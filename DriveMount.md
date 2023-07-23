# Auto mount Drivers in startup:


- Use KDE Partition Manager

# To Enable Write Permission in ArchLinux on NTFS drive:

- Remove & Re-Install ntfs dependencies:-
    ```bash
    sudo pacman -Rs ntfsprogs
    sudo pacman -Rs ntfs-3g
    sudo pacman -S ntfs-3g
    ```
- Check out the NTFS partitions:-
    ```bash
    sudo blkid | grep "ntfs"
    ```
- FIX the NTFS partitions:
    eg: 
    ```bash
    sudo ntfsfix /dev/sda3
    ```
## Sample :
```bash
sudo ntfsfix /dev/nvme1n1p2       #Data Drive
sudo ntfsfix /dev/sda2            #Media
sudo ntfsfix /dev/sda3            #Software
```
## Edit /etc/fstab :

add this and change UUID with yours
```bash
UUID=<UUID>	/media/Software	ntfs-3g	rw,auto,user,fmask=0111,dmask=0000,noatime,nodiratime	0	0
UUID /media/software ntfs nosuid,nodev,nofail,x-gvfs-show,nobootwait,
```


## Windows Key Menu Fix :

- Latte Dock command: 
```bash
kwriteconfig5 --file ~/.config/kwinrc --group ModifierOnlyShortcuts --key Meta  "org.kde.lattedock,/Latte,org.kde.LatteDock,activateLauncherMenu"
qdbus org.kde.KWin /KWin reconfigure
```
