- # Fix flatpak Telegram home folder location access
    ```
    flatpak override --filesystem=home org.telegram.desktop
    ```



#   Fix Kali linux Audio mute on boot issues....
-  <b>Goto <code>/etc/pulse/daemon.conf</code> Change to <code></code></b>
    ```
     daemonize = yes
    ```
    <b>Also remove <code>;</code> in front & Save it and Restart.</b>

# Snap Code 1 error Fix ....
-   <b>Add Below Code To <name>.sh File & move to <code>/etc/init.d/</code></b>
    ```
    #!/bin/bash
    # Snap Code 01 Error Fix...
    apparmor_parser -r /var/lib/snapd/apparmor/profiles/
    ```
-   <b>Add Script on start up</b>
    ```
    sudo chmod +x /etc/init.d/snapcode01error.sh
    ```
    Example:
    
    The procedure to start automatically a daemon in runlevel 3 is easy:

    place your script in <code>/etc/init.d</code>,
    check permissions and ownership of the script,
    create a symbolic link in <code>/etc/rc3.d</code>to the script with the following name <code>S99scriptmame.</code>
    
    The <code>S</code> means start the service. The number is used to sort the service start order from<code> 01</code> (high priority script) to <code>99</code> (last priority script).
    
    Probably, do you need to create a link to the script in <code>/etc/rc0.d</code> (halt) and <code>/etc/rc6.d</code>(reboot) named <code>K01scriptname</code>. Usually last started deamon is the first to be stopped.

    See man init.d^1 for a complete explanation.
    
-   ## <b>Add in Snap Application on user Application</b>
    ```
    sudo cp /var/lib/snapd/desktop/applications/<application-name> /usr/share/applications/


#   Error: Android Studio
-   ```
    The use of Java options environment variables detected. Such variables override IDE configuration files (*.vmoptions) and may cause performance and stability issues. Please consider deleting these variables: _JAVA_OPTIONS.
    ```
    ## solution :-
    ```
    $ printenv _JAVA_OPTIONS
    ```
    <b>output:</b> -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
    ```
    $ unset _JAVA_OPTIONS
    ```
