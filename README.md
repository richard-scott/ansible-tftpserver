Role Name
=========

Builds out a fully functional PXE/TFTP Server for OS deployments..
(Ubuntu included using Netboot)
Pre-Seed file template included and ESXi scripted install ready.

Requirements
------------

If ESXi scripted installs are needed you MUST upload the appropriate ISO's
to /var/lib/tftpboot/images and configure them in `defaults/main.yml`
```
tftpserver_vmware_iso_images:
  - file: VMware-VMvisor-Installer-6.0.0.update01-3029758.x86_64.iso
    folder: ESXi/6.0U1
```

Role Variables
--------------
```
---
# defaults file for ansible-tftpserver
tftpserver_apache_links:
  - 'ESXi_VIBS'
  - 'ESXi_boot'
  - 'images'
  - 'KS'
tftpserver_apache_root: '/var/www/html'
tftpserver_apt_cacher_server: 'apt-cache.{{ tftpserver_domain_name }}'
tftpserver_apt_mirror_dir: '/ubuntu'  #defines the mirror directory of webserver if using local apt repo mirror (apt-mirror) server.
tftpserver_apt_mirror_server: 'apt-mirror.{{ tftpserver_domain_name }}'  #define your local apt repo mirror (apt-mirror) server if you are using one.
tftpserver_bind_address: '{{ ansible_default_ipv4.address }}'
tftpserver_boot_menu:  #menu_default has been disabled to allow boot from local HD by default
  - label: 'local'
    menu_label: '^Boot from hard drive'
    menu_default: true
    localboot: true
#  - label: 'install'
#    menu_label: Install
#    menu_default: false
#    kernel: 'ubuntu-installer/amd64/linux'
#    append: 'vga=788 initrd=ubuntu-installer/amd64/initrd.gz -- quiet'
#  - label: 'cli'
#    menu_label: 'Command-line install'
#    menu_default: false
#    kernel: 'ubuntu-installer/amd64/linux'
#    append: 'tasks=standard pkgsel/language-pack-patterns= pkgsel/install-language-support=false vga=788 initrd=ubuntu-installer/amd64/initrd.gz -- quiet'
  # - label: 'auto-install Ubuntu Netboot (Latest)'
  #   menu_label: 'Automated install Ubuntu (Latest)'
  #   menu_default: false
  #   kernel: 'ubuntu-installer/amd64/linux'
  #   append: 'auto=true priority=critical vga=788 initrd=tftp://{{ tftpserver_bind_address }}/ubuntu-installer/amd64/initrd.gz locale=en_US.UTF-8 kbd-chooser/method=us netcfg/choose_interface=auto url=tftp://{{ tftpserver_bind_address }}/preseed.cfg'
#  - label: 'CentOS 7 (Manual)'
#    menu_label: 'CentOS 7 (Manual)'
#    menu_default: false
#    kernel: 'images/CentOS/7/images/pxeboot/vmlinuz'
#    append: 'auto=true priority=critical vga=normal initrd=tftp://{{ tftpserver_bind_address }}/images/CentOS/7/images/pxeboot/initrd.img ip=dhcp inst.repo=http://{{ tftpserver_bind_address }}/images/CentOS/7'
#  - label: 'Ubuntu 12.04.5 (Manual)'
#    menu_label: 'Install Ubuntu 12.04.5 (Manual)'
#    menu_default: false
#    kernel: 'images/Ubuntu/12.04/install/netboot/ubuntu-installer/amd64/linux'
#    append: 'auto=true priority=critical vga=788 initrd=tftp://{{ tftpserver_bind_address }}/images/Ubuntu/12.04/install/netboot/ubuntu-installer/amd64/initrd.gz locale=en_US.UTF-8 kbd-chooser/method=us netcfg/choose_interface=auto live-installer/net-image=http://{{ tftpserver_bind_address }}/images/Ubuntu/12.04/install/filesystem.squashfs'
#  - label: 'Ubuntu 12.04.5 (Pre-Seed)'
#    menu_label: 'Install Ubuntu 12.04.5 (Pre-Seed)'
#    menu_default: false
#    kernel: 'images/Ubuntu/12.04/install/netboot/ubuntu-installer/amd64/linux'
#    append: 'auto=true priority=critical vga=788 initrd=tftp://{{ tftpserver_bind_address }}/images/Ubuntu/12.04/install/netboot/ubuntu-installer/amd64/initrd.gz locale=en_US.UTF-8 kbd-chooser/method=us netcfg/choose_interface=auto url=tftp://{{ tftpserver_bind_address }}/ubuntu_12.04_preseed.cfg live-installer/net-image=http://{{ tftpserver_bind_address }}/images/Ubuntu/12.04/install/filesystem.squashfs'
#  - label: 'Ubuntu 14.04.2 (Manual)'
#    menu_label: 'Install Ubuntu 14.04.2 (Manual)'
#    menu_default: false
#    kernel: 'images/Ubuntu/14.04/install/netboot/ubuntu-installer/amd64/linux'
#    append: 'auto=true priority=critical vga=788 initrd=tftp://{{ tftpserver_bind_address }}/images/Ubuntu/14.04/install/netboot/ubuntu-installer/amd64/initrd.gz locale=en_US.UTF-8 kbd-chooser/method=us netcfg/choose_interface=auto live-installer/net-image=http://{{ tftpserver_bind_address }}/images/Ubuntu/14.04/install/filesystem.squashfs'
  - label: 'Ubuntu 14.04.5 (Pre-Seed)'
    menu_label: 'Install Ubuntu 14.04.5 (Pre-Seed)'
    menu_default: false
    kernel: 'images/Ubuntu/14.04.5/install/netboot/ubuntu-installer/amd64/linux'
    append: 'auto=true priority=critical vga=788 initrd=tftp://{{ tftpserver_bind_address }}/images/Ubuntu/14.04.5/install/netboot/ubuntu-installer/amd64/initrd.gz locale=en_US.UTF-8 kbd-chooser/method=us netcfg/choose_interface=auto url=tftp://{{ tftpserver_bind_address }}/ubuntu_14.04.5_preseed.cfg live-installer/net-image=http://{{ tftpserver_bind_address }}/images/Ubuntu/14.04.5/install/filesystem.squashfs'
tftpserver_build_images: true  #defines if images folder(s) and isos should be added
tftpserver_config_tftp: true  #defines if tftp services should be configured
tftpserver_domain_name: 'example.org'  #defined here or in group_vars/all/network
tftpserver_enable_apt_caching: false  #defines if apt-cacher-ng is setup and added to preseed.cfg
tftpserver_enable_apt_mirror: false  #defines if using a local mirror (apt-mirror) and configures preseed.cfg as such. Do not use both apt_caching and apt_mirror...
tftpserver_esxi_addl_settings:  #define additional esxli commands to run in order to configure each host
  - desc: 'Set default domain lookup name'
    command: 'esxcli network ip dns search add --domain={{ tftpserver_domain_name }}'
#  - desc: 'Add vmnic3 to vSwitch0'
#    command: 'esxcli network vswitch standard uplink add --uplink-name vmnic3 --vswitch-name vSwitch0'
#  - desc: 'SATP configurations'
#    commands:
#      - 'esxcli storage nmp satp set --satp VMW_SATP_SVC --default-psp VMW_PSP_RR'
#      - 'esxcli storage nmp satp set --satp VMW_SATP_DEFAULT_AA --default-psp VMW_PSP_RR'
tftpserver_esxi_create_host_kickstart_configs: false  #defines if individual kickstart configs should be created
tftpserver_esxi_enable_snmp: false
tftpserver_esxi_enable_ssh_and_shell: false  #defines if SSH and ESXi shell are to be enabled
tftpserver_esxi_exit_maint_mode: false  #defines if during TFTP/PXE load of ESXi hosts maintenance mode should be exited.
tftpserver_esxi_global_network_options:
  - bootproto: 'dhcp'
    create_default_portgroup: false
    interface: 'vmnic0'
tftpserver_esxi_hosts: []
#  - name: 'esxi01'
#    bootproto: 'dhcp'
#    create_default_portgroup: false
#    interface: 'vmnic0'
#  - name: 'esxi02'
#    bootproto: 'static'
#    create_default_portgroup: false
#    interface: 'vmnic0'
#    ip: '10.0.106.22'
#    netmask: '255.255.255.0'
#    gateway: '10.0.106.1'
#    nameservers: ''
tftpserver_esxi_install_disk_options: 'install --firstdisk --overwritevmfs'  #example options are... install --firstdisk=usb --overwritevmfs --novmfsondisk ... or ... install --firstdisk --overwritevmfs
tftpserver_esxi_install_vibs: false
tftpserver_esxi_root_pw: 'vmware1'  #define here or in group_vars/all/accounts (preferred)
tftpserver_esxi_root_pw_encrypted: false  #defines if tftpserver_esxi_root_pw has been set to an already encrypted password
tftpserver_esxi_snmp_options:
  - community: 'PUBLIC'
    allowed_from: '10.0.0.0/24'
tftpserver_esxi_vibs:
  - 'http://{{ ansible_fqdn }}/ESXi_VIBS/Dell/cross_oem-dell-openmanage-esxi_7.3.0.2.ESXi550-0000.vib'
  - 'http://{{ ansible_fqdn }}/ESXi_VIBS/Dell/cross_oem-dell-iSM-esxi_1.0.ESXi550-0000.vib'
  - 'http://{{ ansible_fqdn }}/ESXi_VIBS/PernixData/PernixData_bootbank_pernixcore-vSphere5.5.0_1.5.0.2-25498.vib'
tftpserver_images_folders:
  - 'CentOS/7'
  # - 'ESXi/5.1'
  # - 'ESXi/5.1U1'
  # - 'ESXi/5.1U2'
  # - 'ESXi/5.5'
  # - 'ESXi/5.5U1'
  # - 'ESXi/5.5U2'
  - 'ESXi/5.5U3'
  # - 'ESXi/6.0'
  - 'ESXi/6.0U1'
  # - 'Ubuntu/12.04'
  - 'Ubuntu/14.04'
  - 'Ubuntu/16.04'
tftpserver_iso_images: []
#  - url: 'http://www.gtlib.gatech.edu/pub/centos/7/isos/x86_64'
#    file: 'CentOS-7-x86_64-Minimal-1503-01.iso'
#    folder: 'CentOS/7'
#  - url: 'http://mirror.pnl.gov/releases/12.04'
#    file: 'ubuntu-12.04.5-server-amd64.iso'
#    folder: 'Ubuntu/12.04'
#  - url: 'http://mirror.pnl.gov/releases/14.04'
#    file: 'ubuntu-14.04.2-server-amd64.iso'
#    folder: 'Ubuntu/14.04'
tftpserver_netboot_file: 'netboot.tar.gz'
tftpserver_netboot_url: 'http://archive.ubuntu.com/ubuntu/dists/trusty-updates/main/installer-amd64/current/images/netboot'
tftpserver_preseed_expert_recipe_partitioning: false  #defines if custom partitioning is required
tftpserver_preseed_expert_recipe_partitions:  #define the partitions to create during pre-seed
  - name: 'boot'
    mountpoint: '/boot'
    bootable: true
    filesystem: 'ext4'
###    lv_name: boot  #This is an example only for /boot  do not assign an lv_name for boot.../boot will not be part of LVM.
    max_size: '1000'
    method: 'format'
    min_size: '500'
    priority: '50'
    use_filesystem: true
  - name: 'swap'
#    mountpoint:  #not needed for swap
#    bootable: false  #not needed for swap.
    filesystem: 'linux-swap'
    lv_name: 'swap'  #defines the LVM name to use if tftpserver_preseed_partitioning_method: lvm and tftpserver_preseed_expert_recipe_partitioning: true
    max_size: '2048'
    method: 'swap'
    min_size: '500'
    priority: '512'
#    use_filesystem: true
  - name: 'root'
    mountpoint: '/'
#    bootable: false  #not needed for root
    filesystem: 'ext4'
    lv_name: 'root'  #defines the LVM name to use if tftpserver_preseed_partitioning_method: lvm and tftpserver_preseed_expert_recipe_partitioning: true
    max_size: '10000'
    method: 'format'
    min_size: '5000'
    priority: '10000'
    use_filesystem: true
tftpserver_preseed_files: # Defines preseed.cfg to generate
  - distro: 'ubuntu'
    version: '12.04.5'
  - distro: 'ubuntu'
    version: '14.04.5'
  - distro: 'ubuntu'
    version: '16.04.1'
tftpserver_preseed_mirror: '{{ ansible_hostname }}' # Define mirror...(us.archive.ubuntu.com)
tftpserver_preseed_mirror_hostname: '{{ ansible_hostname }}' # Define mirror...(us.archive.ubuntu.com)
tftpserver_preseed_ntp_sync: true  #defines if system clock is synced from NTP during install
tftpserver_preseed_packages:  #define packages to install during pre-seed installation(s)
  - 'openssh-server'
  # - 'open-vm-tools'
tftpserver_preseed_partition_disk: '/dev/sda'  #defines disk to install to during pre-seed TFTP/PXE install
tftpserver_preseed_partitioning_method: 'lvm'   #defines partitioning method....lvm, regular or crypto

# To generate passwords use (replace P@55w0rd with new password).... echo "P@55w0rd" | mkpasswd -s -m sha-512
# Define password as encrypted password and set encrypted_password=true
tftpserver_preseed_poweroff_after_install: false  #defines if host should be shutdown after installing...good for mass PXE deployments when the option to do a one-time boot to PXE is not an option.
tftpserver_preseed_user_info:
  encrypted_password: false # Defines if password has been entered as encrypted or not
  full_username: 'Ubuntu User'
  password: 'ubuntu'
  sudo_password: true # Defines if password prompt for sudo
  username: 'ubuntu'
tftpserver_tftpboot_dir: '/var/lib/tftpboot'
tftpserver_vmware_iso_images: []
#  - file: VMware-VMvisor-Installer-6.0.0.update01-3029758.x86_64.iso
#    folder: ESXi/6.0U1
```

Dependencies
------------
Install the required Ansible roles by running:
`ansible-galaxy install -r requirements.yml`

Example Playbook
----------------
```
---
- hosts: tftpserver-nodes
  become: true
  vars:
    tftpserver_enable_apt_caching: false
  roles:
    - role: ansible-apt-cacher-ng
      when: tftpserver_enable_apt_caching
    - role: ansible-dnsmasq
    - role: ansible-tftpserver
```

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com
