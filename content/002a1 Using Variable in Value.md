[[002a Ansible group_vars and host_vars precedence]]
How to use ansible variable with same name and default variable manipulation. 


_As a network engineer, similar config can be applied to multiple device. their variable structure is the same like interfaces for the same model but its value would vary. In that case, using group_vars with variable in VALUE would be handy trick for hosts.

reference link : Ansible document  https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable
key board is not working ? 

host_1## hosts.ini
```ini
[Group1]
host1
host2 
```

## Group1.yml - default variable

```yml

# Default Interface Setting 

int1_desc: NOT IN USE
int1_ip : ""
int1_mask: "255.255.255.248"

int2_desc: NOT IN USE
int2_ip : ""
int2_mask: "255.255.255.248"

interfaces: 
  int1:
    name: Gi1/0/1
    desc: "{{int1_desc}}"
    ip: "{{int1_ip}}"
    mask: "{{int1_mask}}"
  int2:
    name: Gi1/0/2
    desc: "{{int2_desc}}"
    ip: "{{int2_ip}}"
    mask: "{{int1_mask}}"
 
```

## host1.yml  variable 

```yml 

int1_desc: "HOST1 CONNECTED TO SITE AAA "
int1_ip : "10.1.1.1"

```

## host2.yml variable
```yml 

int1_desc: "HOST2 CONNECTED TO SITE BBB "
int1_ip : "10.2.2.1"
int1_mask: "255.255.255.0"
```


## pa_var_20230215.yml 
```yml 
---
- name:  PLAY WITH ANSIBLE 
  hosts: Group1
  gather_facts: no
  connection: local 

  tasks: 
  - name: VAR PRECEDENCE TEST 
    debug: 
      msg:   
      - "{{interfaces}}"
```


## Result 
```bash

TASK [VAR PRECEDENCE TEST] ***************************************************************************************************************************************************************
ok: [host1] => {
    "msg": [
        {
            "int1": {
                "desc": "HOST1 CONNECTED TO SITE AAA ",
                "ip": "10.1.1.1",
                "mask": "255.255.255.248",
                "name": "Gi1/0/1"
            },
            "int2": {
                "desc": "NOT IN USE",
                "ip": "",
                "mask": "255.255.255.248",
                "name": "Gi1/0/2"
            }
        }
    ]
}
ok: [host2] => {
    "msg": [
        {
            "int1": {
                "desc": "HOST2 CONNECTED TO SITE BBB ",
                "ip": "10.2.2.1",
                "mask": "255.255.255.0",
                "name": "Gi1/0/1"
            },
            "int2": {
                "desc": "NOT IN USE",
                "ip": "",
                "mask": "255.255.255.0",
                "name": "Gi1/0/2"
            }
        }
    ]
}

```


