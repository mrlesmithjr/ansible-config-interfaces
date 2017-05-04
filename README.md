Role Name
=========

An [Ansible] role to configure network interfaces
- Static, DHCP or Manual.
- Ability to create VLAN, Bond and Bridge interfaces as well.

Requirements
------------

See [Example Playbook](#example-playbook) for examples of how to define specific
network configurations.

Role Variables
--------------

```
---
# defaults file for ansible-config-interfaces

# Defines if network bonds should be configured as defined
config_network_bonds: false

# Defines if network bridges should be configured as defined
config_network_bridges: false

# Defines if interfaces should be configured as defined
config_network_interfaces: false

# Defines if vlans should be configured as defined
config_network_vlans: false

# Defines all dns servers to configure
dns_nameservers:
  - '8.8.8.8'
  - '8.8.4.4'

# Defines your global dns suffix search
dns_search: '{{ pri_domain_name }}'

# Defines if interfaces, bonds, bridges and vlans should be brought up after defining.
enable_configured_interfaces_after_defining: false

network_bonds: []
  # - name: 'bond0'
  #   address: '192.168.1.10'
  #   netmask: '255.255.255.0'
  #   configure: true
  #   comment: 'Bond Group 0'
  #   method: 'static'
  #   slaves:
  #     - 'enp0s9'
  #     - 'enp010'
  #   primary: 'enp0s9'
  #   addl_settings:
  #     - 'bond_mode active-backup'
  #     - 'bond_miimon 100'
network_bridges: []
  # - name: 'br0'
  #   configure: true
  #   comment: 'Bridge 0'
  #   method: 'static'
  #   address: '192.168.1.11'
  #   netmask: '255.255.255.0'
  #   netmask_cidr: '24'
  #   # gateway: '192.168.1.1'
  #   ports: 'enp0s16'
  #   addl_settings:
  #     # - 'up route add default gw 10.0.106.1'
  #     - 'bridge_stp off'
  #     - 'bridge_fd 0'
network_interfaces: []
  # - name: 'enp0s3'
  #   configure: true
  #   method: 'dhcp'
  #   addl_settings:
  #     - 'pre-up sleep 2'
  # - name: 'enp0s8'
  #   configure: true
  #   method: 'static'
  #   address: '192.168.250.10'
  #   netmask: '255.255.255.0'
  # - name: 'enp0s9'
  #   configure: true
  #   comment: 'bond0 member'
  #   method: 'manual'
  #   addl_settings:
  #     - 'bond_master bond0'
  # - name: 'enp0s10'
  #   configure: true
  #   comment: 'bond0 member'
  #   method: 'manual'
  #   addl_settings:
  #     - 'bond_master bond0'
  # - name: 'enp0s16'
  #   configure: true
  #   comment: 'br0 member'
  #   method: 'manual'
  #   # addl_settings:
  #   #   - 'bond_master bond0'

network_vlans: []
  # - name: 'enp0s8.100'
  #   configure: true
  #   comment: 'VLAN 100'
  #   method: 'manual'
  #   address:
  #   netmask:
  #   netmask_cidr:
  #   gateway:
  #   vlan_device: 'enp0s8'
  #   addl_settings:

pri_domain_name: 'example.org'
```

Dependencies
------------

If interface is wireless you will need to define as such as well as provide the
SSID and key.

Example Playbook
----------------

```
---
- hosts: all
  become: true
  vars:
    network_bonds:
      - name: 'bond0'
        address: '192.168.1.10'
        netmask: '255.255.255.0'
        configure: true
        comment: 'Bond Group 0'
        method: 'static'
        slaves:
          - 'enp0s9'
          - 'enp010'
        primary: 'enp0s9'
        addl_settings:
          - 'bond_mode active-backup'
          - 'bond_miimon 100'
    network_bridges:
      - name: 'br0'
        configure: true
        comment: 'Bridge 0'
        method: 'static'
        address: '192.168.1.11'
        netmask: '255.255.255.0'
        netmask_cidr: '24'
        # gateway: '192.168.1.1'
        ports: 'enp0s16'
        addl_settings:
          # - 'up route add default gw 10.0.106.1'
          - 'bridge_stp off'
          - 'bridge_fd 0'
    network_interfaces:
      - name: 'enp0s3'
        configure: true
        method: 'dhcp'
        addl_settings:
          - 'pre-up sleep 2'
      - name: 'enp0s8'
        configure: true
        method: 'static'
        address: '192.168.250.10'
        netmask: '255.255.255.0'
      - name: 'enp0s9'
        configure: true
        comment: 'bond0 member'
        method: 'manual'
        addl_settings:
          - 'bond_master bond0'
      - name: 'enp0s10'
        configure: true
        comment: 'bond0 member'
        method: 'manual'
        addl_settings:
          - 'bond_master bond0'
      - name: 'enp0s16'
        configure: true
        comment: 'br0 member'
        method: 'manual'
        # addl_settings:
        #   - 'bond_master bond0'
    network_vlans:
      - name: 'enp0s8.100'
        configure: true
        comment: 'VLAN 100'
        method: 'manual'
      #  address:
      #  netmask:
      #  netmask_cidr:
      #  gateway:
        vlan_device: 'enp0s8'
      #  addl_settings:

  roles:
    - role: ansible-config-interfaces
  tasks:
```

Example `/etc/network/interfaces`:
----------------------------------

```
# Ansible managed
# Any changes made here will be lost

auto lo
iface lo inet loopback

########## Network Interfaces
auto enp0s3
iface enp0s3 inet dhcp
  pre-up sleep 2

auto enp0s8
iface enp0s8 inet static
  address 192.168.250.10
  netmask 255.255.255.0

# bond0 member
auto enp0s9
iface enp0s9 inet manual
  bond_master bond0

# bond0 member
auto enp0s10
iface enp0s10 inet manual
  bond_master bond0

# br0 member
auto enp0s16
iface enp0s16 inet manual

########## End of Network Interfaces

########## Network Bonds
# Bond Group 0
auto bond0
iface bond0 inet static
  address 192.168.1.10
  netmask 255.255.255.0
  bond_slaves enp0s9 enp010
  bond_primary enp0s9
  bond_mode active-backup
  bond_miimon 100

########## End of Network Bonds


########## Network Bridges
# Bridge 0
auto br0
iface br0 inet static
  address 192.168.1.11
  netmask 255.255.255.0
  bridge_stp off
  bridge_fd 0
  bridge_ports enp0s16

########## End of Network Bridges

dns-nameservers 8.8.8.8 8.8.4.4
dns-search test.vagrant.local
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

[Ansible]: <https://www.ansible.com>
