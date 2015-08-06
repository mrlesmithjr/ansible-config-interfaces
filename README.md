Role Name
=========

Configures network interfaces for either static or dhcp.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------
These variables for most occasions should remain as they are. The actual variables should be defined within host_vars/host for each host requiring configurations.
````
config_interfaces: false  #defines if interfaces should be configured...define in host_vars/host
interfaces: [] #define interfaces to configure...define in host_vars/host...unless setting all interfaces for every host to dhcp for example.
#  - name: eth0
#    address:
#    configure: true
#    gateway:
#    method: dhcp
#    netmask:
#    netmask_cidr:
#    network:
#  - name: eth1
#    address:
#    configure:
#    gateway:
#    method:
#    netmask:
#    netmask_cidr: 24
#    network:
#  - name: enp0s3
#    address: 192.168.1.100
#    configure: true
#    gateway: 192.168.1.1
#    method: static
#    netmask: 255.255.255.0
#    netmask_cidr: 24
#    network: 192.168.1.0
dns_nameservers: '{{ pri_dns }} {{ sec_dns }}'  #defines all dns servers to configure...define here or globally in group_vars/all
pri_dns: []  #defines primary dns server...define here or globally in group_vars/all
sec_dns: []  #defines secondary dns server...define here or globally in group_vars/all
````

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: mrlesmithjr.config-interfaces }

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com
