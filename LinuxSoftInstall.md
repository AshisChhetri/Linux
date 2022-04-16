# System Update

- <b>In Debian - MX xfce :</b>
	```bash
		sudo apt update -y && sudo apt upgrade -y
	```



# Conky Install
- <b>In Debian11 :</b>
    ```
    sudo app-get install conky-all
    ```
-   <b>Install Conky Manager.. </b>
    ```
    wget --no-check-certificate https://github.com/teejee2008/conky-manager/releases/download/v2.4/conky-manager-v2.4-amd64.deb

    ```
    ```
    chmod +x conky-manager-v2.4-amd64.deb
    ```
    ```
    sudo ./conky-manager-v2.4-amd64.deb
    ```
    ```
    sudo apt-get install conky p7zip imagemagick rsync
    ```


- <b>In Debian :</b>
	command:
	To install Conky:

	```bash
	sudo apt-get install conky conky-all
	```
	-	To Download StatConky File:

	```bash
	cd && wget -O .start-conky http://drive.noobslab.com/data/conky/start-conky
	```
	```bash
	chmod +x .start-conky
	```

# Latte Dock install

- <b>In Kali Linux - KDE :</b>
	```bash
		sudo apt install latte-dock
	```

# Nautilus File Manager install
	```
	sudo apt install nautilus
	``` 


# How To Setup Flutter in Kali Linux ...
<br>
Extract flutter inside home folder and follow steps..


- <b>1. Add flutter path in (.bashrc) File located in $HOME/ folder</b>
    ```
    $ export PATH="$PATH:[PATH_OF_FLUTTER_GIT_DIRECTORY]/bin"
    ```
- <b>Run terminal and type</b>
    ```
    $ sudo su
    ```
    Enter root password<br><br>
    <b>Add flutter tool to your path  </b>
    ```
    # export PATH="$PATH:`pwd`/flutter/bin"
    ```

    - <b>Direct Way to Add Path <code>/etc/profile</code> and ADD the line</b>
    ```
    if [ "$(id -u)" -eq 0 ]; then
       
    else
        PATH="**....**:/home/AshisThapa/AndroidFlutter/flutter/bin"
    fi
    export PATH
    ```

- <b>2. To Check Path Added or Not. </b>
    ```
    # echo $PATH
    ```
- <b>3. Optionally, pre-download developmant binaries</b>
    ```
    flutter precache
    ```

- <b>4. Check  Flutter is working or not.. </b>
    ```
    flutter doctor
    ```
    ![](images/flutter_doctor.png)

-   <b>To fix permission denied</b>:
    ```
    sudo chown -R <UserName>: /path/to/Flutter/folder
    ```
    ex:-
    ```bash
    sudo chown -R AshisThapa: /home/AshisThapa/AndroidFlutter/flutter
    ```

<!-- ############################################################# -->
# Install Android Studio in Kali Linux ...
    
- Extract .tar.gz file in a folder
    ```
    sudo tar -xvf android-studio-2021.1.1.21-linux.tar.gz -C /opt/
    ```

-   <b>Install Main Menu </b>
    ```
    apt-get install alacarte
    ```
    ![](images/apt_get_install_alacarte.png)

-   <b>Search Main Menu in Application and Set Android Studio launcher ... </b>
    ```
    ```

<!-- ############################################################# -->

# MPV player install 

-   <b> </b>
    ```
    sudo apt-get install mpv
    ```

<!-- ############################################################# -->

# New Efi entry in efi partition Using (grub-efi) :

-   <b>1.Install grub-efi:</b>
    ```
    sudo apt install grub-efi
    ```
-   <b>2.Mount EFI system partition(ESP):</b><br>
    under <code>/boot/efi/</code> directory.In this example, <code>/dev/sda7</code> is the ESP partition.
    ```
    sudo mount /dev/sda7 /boot/efi
    ```
-   <b>3.Then install Grub boot loader to ESP :</b>
    ```
    sudo grub-install /dev/sda --target=x86_64-efi --efi-directory=/boot/efi/
    ```
<!-- ############################################################# -->

# Install Python 3.10 on debian :
-   <b>Web Site link:</b> https://computingforgeeks.com/
how-to-install-python-on-ubuntu-linux-system/
----------------------------------------------------------------------
-   <b>Install the required dependencies to build Python 3.10 from the source:</b>
    ```
    $ sudo apt update

    $ sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev
    ```

-   <b>Download <code>Pyton3.10.* (.tgz)</code> file</b>
  
    
-   <b>1.Extract the <code>.tgz</code> file:</b>
    ```
    $ tar xzf Python-3.10.2.tgz 
    ```
-   <b>2.Change to the extracted directory with cd command, then prepare the Python source code for the compilation on your system:</b>
    ```
    $ cd Python-3.10.2 
    $ ./configure --enable-optimizations 
    ```
    ![](images/python_install_configure.png)

-   <b>Now initiate the Python 3.10 build process as below.</b>
    ```
    make -j 4
    ```

-   <b>3.Finally, run the following command to complete the Python installation on the Debian system. The altinstall prevents the compiler to override default Python versions :</b>
    ```
    $ sudo make altinstall 
    ```
    
-   <b>Check Python Version</b>
    ```
    $ python3.10 -V 
    ```
-   <b>Install pip3</b>
    ```
    $ sudo apt install python3-pip
    ```
<!-- ############################################################# -->

# Install JDK on debian :

-   Download <code>JDK-*.deb</code> from oracle and Install it.
-   <b>Check Java installed path</b>
    ```
    $ ls  /usr/lib/jvm/
    ```

-   <b>To Update <code>JDK</code> Path:</b>
    ```
    // java for running java
    $ sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-14/bin/java 1
    
    // javac for java compiler
    $ sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk-14/bin/javac 1
    ```

- to update symlink
    Open folder where symlink exist and change its link
    ```
    //new link
    $ sudo ln -s /path/to/file /path/to/symlink
    
    //To update existing link.
    $ sudo ln -sf /usr/lib/jvm/jdk-17/bin/java java

    ```
<!-- ############################################################# -->

# ShortCut key <code>f4</code> for terminal :

-   <code></code>
-   <b>Check Settings/Keyboard/Application Shortcuts.</b>
    ```
    xfce4-terminal --hide-menubar --hide-borders --drop-down
    ```

<!-- ############################################################# -->
- [X]  select
- [ ] unselect


    

