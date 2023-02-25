# using Timestamp variable


```yml
---
- name:  Ansible ios_command on Cisco IOS XE
  hosts: host1  #iosxe 
  gather_facts: no
  connection: local 

  vars: 
    timestamp: "{{ lookup('pipe', 'date +\"%D%H%M\"')}}" 

  tasks: 
  - name: Get timestamp from the system
    shell: "date +%Y-%m-%d%H-%M-%S"
    register: tstamp

  - debug: 
      msg: "SHELL_TIMESTAMP {{tstamp}}"
      
```


# Using ansible factor for timestamp 

```yml 
---


```