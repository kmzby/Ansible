Role Name
=========

Installs keepalived http://www.keepalived.org/

Requirements
------------

None...unless using GlusterFS

Role Variables
--------------

````
---
# defaults file for ansible-keepalived
config_keepalived: false
keepalived_router_info:
  - name: vrrp_1
    check_script:
      - name: chk_haproxy
        script: 'killall -0 haproxy'
        interval: 2
        weight: 2
    master_node: node0  #Define inventory_hostname of node to be considered Master
    router_id: 50
    router_pri_backup: 100
    router_pri_master: 150
    vip_int: '{{ ansible_eth1.device }}'
    vip_addresses:
      - 192.168.202.100
      - 192.168.202.101
  - name: vrrp_2
    check_script:
      - name: chk_nginx
        script: 'pidof nginx'
        interval: 2
        weight: 2
    master_node: node1
    router_id: 51
    router_pri_backup: 100
    router_pri_master: 150
    vip_int: '{{ ansible_eth1.device }}'
    vip_addresses:
      - 192.168.202.102

#### <-------Below here are old legacy variables which will eventually go away------>
keepalived_router_id: 55  #defines router id...must be unique per subnet
keepalived_router_pri: 101  #defines router priority....each node must be different....best to define in host_vars
keepalived_scripts_mnt: ''  #define if using GlusterFS
keepalived_vip: 192.168.1.200  #defines the VIP to use
keepalived_vip_int: '{{ ansible_default_ipv4.interface }}'  #defines interface to use for VIP
notify_backup_script: backup.sh
notify_fault_script: fault.sh
notify_master_script: master.sh
primary_gfs_server: ''  #define if using GlusterFS
scripts_home: ''  #define if using GlusterFS
secondary_gfs_server: ''  #define if using GlusterFS
sync_keepalived: false  #defines if keepalived should be synced when using with GlusterFS
````

Dependencies
------------

None...unless using GlusterFS


Example Playbook
----------------
#### GitHub
````
- hosts: all
  remote_user: remote
  become: true
  vars:
  roles:
    - role: ansible-keepalived
  tasks:
````
#### Galaxy
````
- hosts: all
  remote_user: remote
  become: true
  vars:
  roles:
    - role: mrlesmithjr.keepalived
  tasks:
````

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com
