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
# user Drive
# Workspace
UUID=2EF43160F4312C09 /mnt/Workspace ntfs rw,uid=ashisthapa,gid=ashisthapa,fmask=0111,dmask=0000,noatime,nofail,nobootwait,x-gvfs-show,nodiratime 0 0

# Other

UUID=08602DAD602DA284 /mnt/Other ntfs rw,uid=ashisthapa,gid=ashisthapa,fmask=0111,dmask=0000,noatime,nofail,nobootwait,x-gvfs-show,nodiratime 0 0

# Software
UUID=56DA21A3DA21807B /mnt/Software ntfs rw,uid=ashisthapa,gid=ashisthapa,fmask=0111,dmask=0000,noatime,nofail,nobootwait,x-gvfs-show,nodiratime 0 0
# Media
UUID=BA9AE0AE9AE067F9 /mnt/Media ntfs rw,uid=ashisthapa,gid=ashisthapa,fmask=0111,dmask=0000,noatime,nofail,nobootwait,x-gvfs-show,nodiratime 0 0
```


## Change permission :
```
sudo chown -R ashisthapa:ashisthapa /mnt/Workspace
sudo chmod -R 755 /mnt/Workspace
```


## Windows Key Menu Fix :

- Latte Dock command: 
```bash
kwriteconfig5 --file ~/.config/kwinrc --group ModifierOnlyShortcuts --key Meta  "org.kde.lattedock,/Latte,org.kde.LatteDock,activateLauncherMenu"
qdbus org.kde.KWin /KWin reconfigure
```
