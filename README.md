
# 1.To Mount NTFS Drive with read/write Permission

 - Copy Drive (path or UUID):
 
      - sudo apt install ntfs-3g fuse
      - sudo ntfsfix /dev/sda2
      - sudo mount -o rw -t ntfs /dev/sda2 /media/Mount/Media

#Screenshots

![](/linux/mounting2.png)


---

# 2.To Upload File in Git hub..
- Steps:
  - git init
  - git add filename.type
  - git commit -m "first commit"
  - git branch -M main
  - git remote add origin https://github.com/username/repo-name.git
  - git push -u origin main

# Screenshot
![](/Images/gitUpload.png)

# 3.To Install and Setup Flutter in Linux....
- Steps :
  - Extract the file in the desired location : <strong> tar xf ~/Downloads/flutter_linux_2.8.1-stable.tar.xz </strong>
  - Go to Home/.bashrc file and Add : <strong> export PATH="$PATH:/home/ashischhetri/flutter/bin" </strong>
  - And Run : <strong> export PATH="$PATH:`pwd`/flutter/bin" </strong>
  - Add the flutter tool to your path: <strong> export PATH="$PATH:`pwd`/flutter/bin"  </strong>
  - Optionally, pre-download development binaries: <strong> flutter precache </strong>


# 4.To Remove And Order boot entry in Linux Using efibootmgr....
- Steps :
  - To Display Details : <strong> efibootmgr -v </strong>
  - To Arrange: <strong> efibootmgr -o 0000,0002,0003,0001 </strong>
  - To delete: <strong> efibootmgr -b[4_digitcode_of_boot_entry] -B </strong>
