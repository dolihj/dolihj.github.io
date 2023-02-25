# EVE-NG 

## Tired of this Error \[%AMDP2_FE-6-EXCESSCOLL\]

**If you are using IOU/UNL then you are pretty sure aware of this error.**

**%AMDP2_FE-6-EXCESSCOLL: Ethernet0/1 TDR=0, TRC=0**

**In order to filter this error to prevent it from continuously appear on your console without disabling logging.**

**you can apply the below commands.**

**#router(config)#logging discriminator EXCESS severity drops 6 msg-body drops EXCESSCOLL**  
**router(config)#logging buffered 50000**  
**router(config)#logging console discriminator EXCESS**

## SVI ping is not working in IOU/IOL image 
bug of i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423.bin 



# Secann Dell server bootiing issue
booting sequence

F11,  internal SD: IDSDM


## VMware vSphere Hypervisor 7 License

HJ682-4C353-H8DG8-0LAAM-CTR44


# Ansible2.13 

## cisco.ios ssh connection failed: kex error: no match for method kex alogs: server 
>
fatal: [G1_Z_L3S]: FAILED! => {"ansible_facts": {}, "changed": false, "failed_modules": {"cisco.ios.ios_facts": {"failed": true, "invocation": {"module_args": {"available_network_resources": false, "gather_network_resources": null, "gather_subset": ["min"]}}, "msg": "ssh connection failed: ssh connect failed: kex error : no match for method kex algos: server [diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1], client [ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group-exchange-sha256,diffie-hellman-group14-sha256,diffie-hellman-group16-sha512,diffie-hellman-group18-sha512]"}}, "msg": "The following modules failed to execute: cisco.ios.ios_facts\n"}


## [WARNING]: ansible-pylibssh not installed, falling back to paramiko
[pmcsystem@hjalpha playbooks]$ pip3 install --user ansible-pylibssh
Requirement already satisfied: ansible-pylibssh in /usr/local/lib64/python3.6/site-packages


### Reason
 python 3.6 was default and ansible-pylibssh is installed in python3.6 library 
 Ansible uses python3.9 and its library is in /usr/lib/python3.9/site-packages/ansible/ 

[pmcsystem@hjalpha playbooks]$ ansible --version
ansible [core 2.13.3]
  config file = /home/pmcsystem/playbooks/ansible.cfg
  configured module search path = ['/home/pmcsystem/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.9/site-packages/ansible
  ansible collection location = /home/pmcsystem/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.9.13 (main, Jun 15 2022, 11:24:50) [GCC 8.5.0 20210514 (Red Hat 8.5.0-13)]
  jinja version = 3.1.2
  libyaml = True


### Solution  - install anisble-pylibssh in python3.9 version 
[pmcsystem@hjalpha playbooks]$ python3.9 -m pip install ansible-pylibssh
Defaulting to user installation because normal site-packages is not writeable
Collecting ansible-pylibssh
  Downloading ansible_pylibssh-1.0.0-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (2.5 MB)
     |████████████████████████████████| 2.5 MB 13.4 MB/s
Installing collected packages: ansible-pylibssh

