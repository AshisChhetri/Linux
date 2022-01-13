
# 1.To Mount NTFS Drive with read/write Permission

 - Copy Drive (path or UUID):
 
      - sudo apt install ntfs-3g fuse
      - sudo ntfsfix /dev/sda2
      - sudo mount -o rw -t ntfs /dev/sda2 /media/ashischhetri/Media

#Screenshots

![](/linux/mounting2.png)


---

# 2.To Upload File in Git hub..
- Steps:
  - git init
  - git add README.md
  - git commit -m "first commit"
  - git branch -M main
  - git remote add origin https://github.com/username/repo-name.git
  - git push -u origin main

# Screenshot
![](/Images/gitUpload.png)

