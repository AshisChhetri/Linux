# install Basic For DWM
1.  <span style="color: red "><b>Install Basic Libraries And Programs for DWM</b></span>
    
        Run setup_for_dwm.sh

##  <span style="color: red "><b>Install & Setup Network (Wifi) Driver :</b></span>
<b>Document Link : </b><https://wiki.debian.org/WiFi/HowToUse#wpa_supplicant>
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
    b.  <b>Start and Enable NetworkManager</b>
    ```
    sudo systemctl start NetworkManager.service

    sudo systemctl enable NetworkManager.service
    
    ```
    c.  <b>To view NetworkManager Status :</b>
    ```
    sudo systemctl status NetworkManager
    ```

- <span style="color: red "><b>Connect to available network using <code>nmcli</code></b></span>

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