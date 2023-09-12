
## RDP setup

1. Install freerdp package on your linux host machine..
```
yay -S freerdp	
```

2. Enable remote desktop option.
	>enable remote desktop option in your windows 10 vm and allow in firewall.
	
3. run below commands to start connection..
```
xfreerdp -grab-keyboard /v:YOURIP /u:ashis /p:YOURPASSWORD /size:100% /d: /dynamic-resolution /gfx-h264:avc444 +gfx-progressive 
```
Here,
/v : enter your virtual machine network IP address.
/u : enter windows user name.
/p : enter windows user password.

________________________________________________________________________
## Looking Glass setup

1. Install looking glass.
    ```sh
    yay -s looking-glass
    ```
2. Add to Vm xml file 
    ```sh
    <shmem name="looking-glass">
      <model type="ivshmem-plain"/>
      <size unit="M">32</size>
      <address type="pci" domain="0x0000" bus="0x10" slot="0x01" function="0x0"/>
    </shmem>
    ```

3. Next create a configuration file to create the shared memory file on boot. 
   ```
   sudo vim /etc/tmpfiles.d/10-looking-glass.conf
   ```
   added in file
   ```
   f	/dev/shm/looking-glass	0660	ashisthapa	kvm	-
   ```
   Ask systemd-tmpfiles to create the shared memory file now without waiting to next boot
   ```
   # systemd-tmpfiles --create /etc/tmpfiles.d/10-looking-glass.conf
   ```
   
4. Install looking-glass for in windows system.

5. run looking-glass client using command.
    ```
    looking-glass-client
    ```
