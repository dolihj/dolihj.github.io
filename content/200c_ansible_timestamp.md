
# using Timestamp variable

When you want to use time or date info as variable in your playbook
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


# Using timestamp in the output filename 

## 200c_timestamp.yml

You can also make an output file name with date/time info. this is jus easy {{'%y%m%d-%H%M' | strftime}}
```yml 
---
- name:  200c_ANSIBLE_TIMESTAMP
  hosts: A2_1
  gather_facts: no
  connection: local 

  tasks: 
  - name: 
    template:
      src: nesting.j2
      dest: ./{{inventory_hostname}}_{{'%y%m%d-%H%M' | strftime}}.md
```

## output
{{inventory_hostname}}_{{year-month-date_Hour_min}}

```shell
$ ls -al A2_1*
-rw-r--r--  1 dolihj  staff   240B Mar  4 14:46 A2_1_230304-1446.md
```

