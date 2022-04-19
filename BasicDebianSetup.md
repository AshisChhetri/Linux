file location:- E:\Debian11Os

[//]: <> ( comment)

# install Basic For DWM
1.  <span style="color: red "><b>Install Basic Libraries And Programs for DWM</b></span>
    
        Run setup_for_dwm.sh

2.  <span style="color: red "><b>Install & Setup Network (Wifi) Driver</b></span>

    a.  <b>Install Network Manager</b>
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

3.  <span style="color: red "><b>Connect to available network using <code>nmcli</code></b></span>

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
