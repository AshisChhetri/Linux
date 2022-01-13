
# 1.To Mount NTFS Drive with read/write Permission

 - Copy Drive (path or UUID):
 
      - sudo apt install ntfs-3g fuse
      - sudo ntfsfix /dev/sda2
      - sudo mount -o rw -t ntfs /dev/sda2 /media/ashischhetri/Media

#Screenshots

![](/linux/mounting2.png)
