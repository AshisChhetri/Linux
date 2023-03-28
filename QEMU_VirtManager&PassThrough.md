# QEMU, VirtManager And GPU passthrough

## Arch Linux Setup

1. <b>Install package for qemu virtual machine setup.
    use any packageManager( yay or pacman )</b>
    ```
    yay -Sy --needed qemu dhclient openbsd-netcat virt-viewer \
    libvirt dnsmasq dmidecode ebtables virt-install \
    virt-manager bridge-utils
    ```

2. Configuration of libvirt and Virtual Machine Manager 

    2.1 Edit <code>/etc/libvirt/libvirtd.conf</code></br>
    Read/Write permissions and group
    ```
    unix_sock_group = "libvirt"
    ```
    ```
    unix_sock_rw_perms = "0770"
    ```
    2.2 Add these lines at the end of the file:
    ```
    log_filters="3:qemu 1:libvirt"
    log_outputs="2:file:/var/log/libvirt/libvirtd.log"
    ```
    2.3 Adding your user to the libvirt group
    ```
    sudo usermod -a -G kvm,libvirt $(whoami)
    ```
     2.4 enable and start libvirt service
    ```
    sudo systemctl enable libvirtd
    ```
    ```
    sudo systemctl start libvirtd
    ```
    2.5 You can verify libvirt has been added to your users groups by using the following command.
    ```
    sudo groups $(whoami)
    ```
    It should return your current users groups like so.</br>
    <code>
    username libvirt etc...
    </code>
    
    </br>
    </br>
    
3. Qemu configuration

    3.1 The second config file to edit is qemu.conf, located at <code>/etc/libvirt/qemu.conf</code>. Again, use your terminal editor to edit this file:
    ```
    sudo nano /etc/libvirt/qemu.conf
    ```

    3.2 Uncomment the following lines (between line 518 and 524) :
    ```
    user = "root"

    group = "root"
    ```
    And replace root with your username:
    ```
    user = "your username"

    group = "your username"
    ```
    
    3.3 Then restart the libvirt daemon with the following.
    ```
    sudo systemctl restart libvirtd
    ```


4. Update Grub file to <code>enable IOMMU</code>

    a. view PCI ID's
    ```sh
    lspci -nnk
    ```
    b. open grub config
    ```sh
    sudo vim /etc/default/grub
    ```
    c. Add following to GRUB_CMDLINE_LINUX arguments

    old
    ```
    intel_iommu=on iommu=pt rd.driver.pre=vfio-pci pci-stub.ids=10de:2191,10de:1aeb"

    #pci-stub.ids=10de:2191,10de:1aeb,10de:1aec,10de:1aed rd.driver.pre=vfio-pci"

    ```
    (New)
    ```sh
    intel_iommu=on iommu=pt rd.driver.pre=vfio-pci rd.driver.blacklist=nouveau modprobe.blacklist=nouveau nvidia-drm.modeset=1 vfio-pci.ids=10de:2191,10de:1aeb module_blacklist=nouveau
    ```
    d. Regenerate grub config.
    ```sh
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

    e. create new file
    ```
    sudo vim /etc/modprobe.d/vfio.conf
    ```
    add following to new file.
    ```
    options vfio-pci ids=10de:2191,10de:1aeb,10de:1aec,10de:1aed
    ```
    f. Update initramfs
    ```
    sudo vim /etc/mkinitcpio.conf
    ```
    add following to MODULES
    ```
    vfio vfio_iommu_type1 vfio_pci vfio_virqfd
    ```
    should look like
    ```
    MODULES=(vfio vfio_iommu_type1 vfio_pci vfio_virqfd)
    BINARIES=()
    FILES=()
    HOOKS=(base udev autodetect keyboard keymap modconf block filesystems fsck)
    ```
    g. Generate initramfs
    ```
    sudo mkinitcpio -P
    ```

## Share mouse and Keyboard
1. to get ID's
    ```
    ls /dev/input/by-id
    ```
2. add following to VM XML.
    ```
    # Change with your mouse id
    <input type="evdev">
    <source dev="/dev/input/by-id/usb-Logitech_G102_Prodigy_Gaming_Mouse_1068385D3734-event-mouse"/>
    </input>

    # Change with your keyboard id
    <input type="evdev">
    <source dev="/dev/input/by-id/usb-Logitech_USB_Keyboard-event-kbd" grab="all" grabToggle="ctrl-ctrl" repeat="on"/>
    </input>

    ```
    This add function to switch mouse and keyboard control b/w VM and Host System.


<!-- ############################################################# -->

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
    
3. Install looking-glass for in windows system.

4. run looking-glass client using command.
    ```
    looking-glass-client
    ```