# QEMU, VirtManager And GPU passthrough

## Arch Linux Setup

1. <b>Install package for qemu virtual machine setup.
    use any packageManager( yay or pacman )</b>
    ```
    yay -Sy --needed qemu dhclient openbsd-netcat virt-viewer \
    libvirt dnsmasq dmidecode ebtables virt-install \
    virt-manager bridge-utils
    ```
    2) method
    ```
    sudo pacman -S qemu libvirt edk2-ovmf virt-manager virt-viewer swtpm  dnsmasq
    
    // iptables-nft
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
    
    d. Regenerate grub config.
    ```sh
    grub-mkconfig -o /boot/grub/grub.cfg
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
  
   > booster

    Similar to mkinitcpio you need to specify modules to load early:
    ```
    $sudo vim /etc/booster.yaml
    
    modules_force_load: vfio_pci,vfio,vfio_iommu_type1,vfio_virqfd
    ```
    >dracut
    
    Following the same idea, we need to ensure all vfio drivers are in the initramfs. Add the following file to:
    ``` 
    $sudo vim /etc/dracut.conf.d/10-vfio.conf

    force_drivers+=" vfio_pci vfio vfio_iommu_type1 vfio_virqfd "
    ```

    g. Generate initramfs
    ```
    sudo mkinitcpio -P
    ```


## VM Storage type and address
In the config window for your VM, open the XML tab for the storage device and change the bus type from sata to “virtio” and the address to “pci”. When you click “Apply” it should look like this 


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

    # Royal Kludge
    <input type="evdev">
    <source dev="/dev/input/by-id/usb-SINO_WEALTH_RK_Bluetooth_Keyboard-event-kbd" grab="all" grabToggle="ctrl-ctrl" repeat="on"/>
    </input>


    # wireless id
    <input type="evdev">
    <source dev="/dev/input/by-id/usb-Compx_2.4G_Wireless_Receiver-event-kbd" grab="all" grabToggle="ctrl-ctrl" repeat="on"/>
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


## win10 VM xml file..
```
<domain type="kvm">
  <name>win10-test</name>
  <uuid>b5b2cf11-de66-4a88-a77a-a59c984b4818</uuid>
  <metadata>
    <libosinfo:libosinfo xmlns:libosinfo="http://libosinfo.org/xmlns/libvirt/domain/1.0">
      <libosinfo:os id="http://microsoft.com/win/10"/>
    </libosinfo:libosinfo>
  </metadata>
  <memory unit="KiB">8388608</memory>
  <currentMemory unit="KiB">8388608</currentMemory>
  <vcpu placement="static">12</vcpu>
  <os firmware="efi">
    <type arch="x86_64" machine="pc-q35-8.1">hvm</type>
    <firmware>
      <feature enabled="no" name="enrolled-keys"/>
      <feature enabled="no" name="secure-boot"/>
    </firmware>
    <loader readonly="yes" type="pflash">/usr/share/edk2/x64/OVMF_CODE.fd</loader>
    <nvram template="/usr/share/edk2/x64/OVMF_VARS.fd">/var/lib/libvirt/qemu/nvram/win10-test_VARS.fd</nvram>
  </os>
  <features>
    <acpi/>
    <apic/>
    <hyperv mode="custom">
      <relaxed state="on"/>
      <vapic state="on"/>
      <spinlocks state="on" retries="8191"/>
    </hyperv>
    <vmport state="off"/>
  </features>
  <cpu mode="host-passthrough" check="none" migratable="on">
    <topology sockets="1" dies="1" cores="2" threads="6"/>
  </cpu>
  <clock offset="localtime">
    <timer name="rtc" tickpolicy="catchup"/>
    <timer name="pit" tickpolicy="delay"/>
    <timer name="hpet" present="no"/>
    <timer name="hypervclock" present="yes"/>
  </clock>
  <on_poweroff>destroy</on_poweroff>
  <on_reboot>restart</on_reboot>
  <on_crash>destroy</on_crash>
  <pm>
    <suspend-to-mem enabled="no"/>
    <suspend-to-disk enabled="no"/>
  </pm>
  <devices>
    <emulator>/usr/bin/qemu-system-x86_64</emulator>
    <disk type="file" device="disk">
      <driver name="qemu" type="qcow2" discard="unmap"/>
      <source file="/var/lib/libvirt/images/win10-test.qcow2"/>
      <target dev="vda" bus="virtio"/>
      <boot order="2"/>
      <address type="pci" domain="0x0000" bus="0x03" slot="0x00" function="0x0"/>
    </disk>
    <disk type="block" device="disk">
      <driver name="qemu" type="raw" cache="none" io="native" discard="unmap"/>
      <source dev="/dev/sda4"/>
      <target dev="vdb" bus="virtio"/>
      <boot order="3"/>
      <address type="pci" domain="0x0000" bus="0x04" slot="0x00" function="0x0"/>
    </disk>
    <controller type="usb" index="0" model="qemu-xhci" ports="15">
      <address type="pci" domain="0x0000" bus="0x01" slot="0x00" function="0x0"/>
    </controller>
    <controller type="pci" index="0" model="pcie-root"/>
    <controller type="pci" index="1" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="1" port="0x10"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x0" multifunction="on"/>
    </controller>
    <controller type="pci" index="2" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="2" port="0x11"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x1"/>
    </controller>
    <controller type="pci" index="3" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="3" port="0x12"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x2"/>
    </controller>
    <controller type="pci" index="4" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="4" port="0x13"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x3"/>
    </controller>
    <controller type="pci" index="5" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="5" port="0x14"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x4"/>
    </controller>
    <controller type="pci" index="6" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="6" port="0x15"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x5"/>
    </controller>
    <controller type="pci" index="7" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="7" port="0x16"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x6"/>
    </controller>
    <controller type="pci" index="8" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="8" port="0x17"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x7"/>
    </controller>
    <controller type="pci" index="9" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="9" port="0x18"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x03" function="0x0" multifunction="on"/>
    </controller>
    <controller type="pci" index="10" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="10" port="0x19"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x03" function="0x1"/>
    </controller>
    <controller type="pci" index="11" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="11" port="0x1a"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x03" function="0x2"/>
    </controller>
    <controller type="pci" index="12" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="12" port="0x1b"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x03" function="0x3"/>
    </controller>
    <controller type="pci" index="13" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="13" port="0x1c"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x03" function="0x4"/>
    </controller>
    <controller type="pci" index="14" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="14" port="0x1d"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x03" function="0x5"/>
    </controller>
    <controller type="pci" index="15" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="15" port="0x1e"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x03" function="0x6"/>
    </controller>
    <controller type="pci" index="16" model="pcie-to-pci-bridge">
      <model name="pcie-pci-bridge"/>
      <address type="pci" domain="0x0000" bus="0x06" slot="0x00" function="0x0"/>
    </controller>
    <controller type="sata" index="0">
      <address type="pci" domain="0x0000" bus="0x00" slot="0x1f" function="0x2"/>
    </controller>
    <controller type="virtio-serial" index="0">
      <address type="pci" domain="0x0000" bus="0x02" slot="0x00" function="0x0"/>
    </controller>
    <interface type="network">
      <mac address="52:54:00:e8:d6:a5"/>
      <source network="default"/>
      <model type="e1000e"/>
      <address type="pci" domain="0x0000" bus="0x09" slot="0x00" function="0x0"/>
    </interface>
    <serial type="pty">
      <target type="isa-serial" port="0">
        <model name="isa-serial"/>
      </target>
    </serial>
    <console type="pty">
      <target type="serial" port="0"/>
    </console>
    <channel type="spicevmc">
      <target type="virtio" name="com.redhat.spice.0"/>
      <address type="virtio-serial" controller="0" bus="0" port="1"/>
    </channel>
    <input type="tablet" bus="usb">
      <address type="usb" bus="0" port="1"/>
    </input>
    <input type="mouse" bus="ps2"/>
    <input type="keyboard" bus="ps2"/>
    <input type="evdev">
      <source dev="/dev/input/by-id/usb-Logitech_G102_Prodigy_Gaming_Mouse_1068385D3734-event-mouse"/>
    </input>
    <input type="evdev">
      <source dev="/dev/input/by-id/usb-Compx_2.4G_Wireless_Receiver-event-kbd" grab="all" grabToggle="ctrl-ctrl" repeat="on"/>
    </input>
    <graphics type="spice" autoport="yes">
      <listen type="address"/>
      <image compression="off"/>
    </graphics>
    <sound model="ich9">
      <address type="pci" domain="0x0000" bus="0x00" slot="0x1b" function="0x0"/>
    </sound>
    <audio id="1" type="spice"/>
    <video>
      <model type="qxl" ram="65536" vram="65536" vgamem="16384" heads="1" primary="yes"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x01" function="0x0"/>
    </video>
    <hostdev mode="subsystem" type="pci" managed="yes">
      <source>
        <address domain="0x0000" bus="0x01" slot="0x00" function="0x1"/>
      </source>
      <address type="pci" domain="0x0000" bus="0x07" slot="0x00" function="0x0"/>
    </hostdev>
    <hostdev mode="subsystem" type="pci" managed="yes">
      <source>
        <address domain="0x0000" bus="0x01" slot="0x00" function="0x0"/>
      </source>
      <address type="pci" domain="0x0000" bus="0x08" slot="0x00" function="0x0"/>
    </hostdev>
    <redirdev bus="usb" type="spicevmc">
      <address type="usb" bus="0" port="2"/>
    </redirdev>
    <redirdev bus="usb" type="spicevmc">
      <address type="usb" bus="0" port="3"/>
    </redirdev>
    <watchdog model="itco" action="reset"/>
    <memballoon model="none"/>
    <shmem name="looking-glass">
      <model type="ivshmem-plain"/>
      <size unit="M">64</size>
      <address type="pci" domain="0x0000" bus="0x10" slot="0x01" function="0x0"/>
    </shmem>
  </devices>
</domain>


```
