# <b>Install Basic For DWM</b>
1.  <span style="color: red "><b>Install Basic Libraries And Programs for DWM</b></span>

    Run setup_for_dwm.sh

    ```
    sudo sh ./setup_for_dwm.sh
    ```

----------------------------------------------------------------------

# <span style="color: red "><b>Install & Setup Network (Wifi) Driver :</b></span>

0. <b>Document Link : </b><https://wiki.debian.org/WiFi/HowToUse#wpa_supplicant>

    <span style="color: yellow "><b>There is Two (2) Options/Methods to Setup....</b></span>
    
    ## i. <code><b>fireware-iwlwifi</b></code>   </br>
    ## ii. <code><b>NetworkManager</b></code>
    

1. <span style="color: red "><b>Using <code>Firmware-iwlwifi</code></b></span>

    <b><span style="color: yellow ">a.</span> Install firmware-iwlwifi & wpasupplicant</b>
    ```
    sudo apt install firmware-iwlwifi wpasupplicant

    ```
    <b><span style="color: yellow ">b.</span>  Setting up wpa_supplicant.conf file</b>

    WPA-PSK and WPA2-PSK
        
    i. Also known as "WPA Personal" and "WPA2 Personal" respectively.

    <span style="color: red "><b><u>Restrict the permissions</u></b></span> of <code>/etc/network/interfaces</code>, to prevent pre-shared key (PSK) disclosure (alternatively use a separate config file such as <code>/etc/network/interfaces.d/wlan0</code> on newer Debian versions):
    ```
    chmod 0600 /etc/network/interfaces
    ```
    <b><span style="color: yellow ">c.</span> Use the WPA passphrase to calculate the correct WPA PSK hash for your SSID by altering the following example:</b>
    ```
    $ su -l -c "wpa_passphrase myssid my_very_secret_passphrase > /etc/wpa_supplicant/wpa_supplicant.conf"
    ```

    If you don't put the passphrase on the command line, it will be prompted for. The above command gives the following output and pipe(write) it to <code>"/etc/wpa_supplicant/wpa_supplicant.conf"</code>:
    ```
    network={
            ssid="myssid"
            #psk="my_very_secret_passphrase"
            psk=ccb290fd4fe6b22935cbae31449e050edd02ad44627b16ce0151668f5f53c01b
    }
    ```
    Since wpa_supplicant v2.6, you need to add following in your <code>/etc/wpa_supplicant/wpa_supplicant.conf</code> in order to function <code>sudo wpa_cli</code>:
    ```
    ctrl_interface=/run/wpa_supplicant 
    update_config=1
    ```
    you'll need to copy from "psk=" to the end of the line, to put in your /etc/network/interfaces file.

    Quick connect to the configured network (doesn't require ifupdown):
    ```
    sudo systemctl reenable wpa_supplicant.service
    sudo service wpa_supplicant restart
    sudo service dhcpcd restart
    sudo wpa_supplicant -B -Dwext -i <interface> -c/etc/wpa_supplicant.conf
    ```
    Now you should have connected to the internet.



    WPA-EAP
    For networks using EAP-TLS, you are required to establish a wpa_supplicant configuration file and provide the client-side certificate. An example WPA2-EAP configuration file can be found at /usr/share/doc/wpasupplicant/examples/wpa2-eap-ccmp.conf.

    Once available, reference your configuration file in <code>/etc/network/interfaces</code>. For example:
    ```
    allow-hotplug wlan0
    iface wlan0 inet dhcp
        wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
    ```


2. <span style="color: red "><b>Using NetworkManager</b></span>

    a. <b>Install Network Manager</b>
    ```
    sudo apt install NetworkManager

    ```

    Enabling Interface Management

    If you want NetworkManager to handle interfaces that are enabled in <code>/etc/network/interfaces</code>:

    Set <code>managed=true</code> in <code>/etc/NetworkManager/NetworkManager.conf</code>.

    Restart NetworkManager: 
    ```
    sudo service network-manager restart
    ```

    Starting with Debian 11 (bullseye), use:
    ```
    sudo service NetworkManager restart
    ```

    "Auto Ethernet" and "Auto eth0"

    Auto Ethernet means "Select an Ethernet interface automatically"

    Auto eth0 means "autoconfigure the eth0 interface". 

    b.  <b>Start and Enable NetworkManager</b>
    ```
    sudo systemctl start NetworkManager.service

    sudo systemctl enable NetworkManager.service
    
    ```
    c.  <b>To view NetworkManager Status :</b>
    ```
    sudo systemctl status NetworkManager
    ```

    <span style="color: red "><b>Connect to available network using <code>nmcli</code></b></span>

    a.  <b>View System Available Network Driver</b>
    ```
    nmcli d

    ```
    b.  <b>Enable Wifi ...</b>
    ```
    nmcli r wifi on      
    ```
    c.  <b>To view Available Wifi network :</b>
    ```
    nmcli d wifi list
    ```
    d.  <b>To connect to wifi n/w  :</b>
    ```
    nmcli d wifi connect "ssid" password --your-password
    ```

----------------------------------------------------------------

#  <span style="color: red "><b>Install & Setup Audio Driver :</b></span>
0. <b>Document Link : </b><https://wiki.debian.org/PulseAudio#Installing_PulseAudio>
1. <span style="color: red "><b>Install <code>Pulse Audio</code> with Alsa as core Linux sound Architecture </b></span>

    <b><span style="color: yellow ">a.</span> Install <code>pulseaudio</code> & for GUI control <code>pavucontrol</code></b>
    ```
    sudo apt install pulseaudio pavucontrol

    ```
    <b><span style="color: yellow ">b.</span>  Restarting the Pulseaudio Daemon</b>

    To reread the config files <code>~/.config/pulse/daemon.conf</code> and <code>/etc/pulse/daemon.conf</code>, one can restart pulse:
        
    ```
    $ systemctl --user restart pulseaudio.service
    ```

    Or on systems that do not use systemd (including Debian 8 and earlier):

    ```
    $ pulseaudio --kill
    $ pulseaudio --start
    ```

    <b><span style="color: yellow ">c.</span> Interrupting play in Amarok when running Skype</b>

    Comment out or remove the line in the <code>/etc/pulse/default.pa</code>

    ```
    load-module module-cork-music-on-phone
    ```

    <b><span style="color: yellow ">c.</span> Sound level is low or suddenly becomes too loud</b>

    Add a line to <code>/etc/pulse/daemon.conf</code>:

    ```
    flat-volumes = no
    ```

    <b><span style="color: yellow ">c.</span> Missing playback devices or audio capture</b>

    If Pulseaudio does not correctly detect your input / output devices ("sources" and "sinks" in Pulseaudio parlance), you can try deleting the configuration files and restarting pulseaudio. This is probably unnecessary overkill, but might help some people.

    ```
    $ rm -r ~/.config/pulse /tmp/pulse-*
    $ systemctl --user restart pulseaudio.service
    ```

    Pulseaudio, in its default configuration, will likely want exclusive access to the hardware. It will therefore skip devices already in use by other applications, eg. web browsers and volume control applets. lsof /dev/snd/* will help identify processes you may need to kill off before PulseAudio will use your audio device.

-----------------------------------------------------------------
# <span style="color: red "><b>Install Intel Drivers :</b></span>
- <b>Document Link : </b><https://wiki.debian.org/GraphicsCard#Intel>
- Debian 11 "Bullseye"
    <p>If your graphic card was manufactured in 2007 and newer
    uninstalling the <code>xserver-xorg-video-intel</code> package and use the builtin modesetting driver (<code>xserver-xorg-core</code>) instead.
    </p>
    ```
    sudo apt-get install xserver-xorg-core
    ```

------------------------------------------------------------------
# <span style="color: red "><b>Install Nvidia Drivers :</b></span>
- <b>Document Link : </b><https://wiki.debian.org/NvidiaGraphicsDrivers#bullseye-470>
- <p>Debian 11 "Bullseye"</p>
    Version 470.103.01
    For support of GeForce 600 series and newer GPUs (supported devices). For older devices, see Version 390 (legacy GPUs).

    1. Add <code>"contrib"</code> and <code>"non-free"</code> components to <code>/etc/apt/sources.list</code>, for example:
    ```
    # Debian Bullseye
    deb http://deb.debian.org/debian/ bullseye main contrib non-free
    ```

    2. Add bullseye-backports as an additional new line to your /etc/apt/sources.list, for example:
    ```
    # bullseye-backports
    deb http://deb.debian.org/debian bullseye-backports main contrib non-free
    ```

    3. Update the list of available packages, then we can install the nvidia-driver package, plus the necessary firmware, from the backports repository:
    ```
    # apt update
    # apt install -t bullseye-backports nvidia-driver firmware-misc-nonfree
    ```

    DKMS will build the nvidia module for your system, via the <code>nvidia-kernel-dkms</code> package.

    - Optional : if you have SecureBoot enabled, you need to sign the resulting modules. Detailed instructions are available here.

    4. Reboot your system to load the updated driver.