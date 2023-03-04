It is similar to [[200a1 configure interface using group_vars as default]] but I will use for SVI-L3vlans in this example. Vlans are selective not like interfaces/ports in the same model device. 

In the same group, you can configure SVI with default values such as subnetmask or vrf. 

## Group1.yml : default variable in group_vars
```yml

# Default SVI Setting 

vl10:
  use: false
  ip: ""
  desc: ""
vl20:
  use: false
  ip: ""
  desc: ""
vl30:
  use: false
  ip: ""
  desc: ""

vlans: 
- id: 10 
  use: "{{vl10.use}}"
  vrf: 10
  ip: "{{vl10.ip}}"  
  submask: 255.255.255.0
  desc: "{{vl10.desc}}"
- id: 20 
  use: "{{vl20.use}}"
  vrf: 20
  ip: "{{vl20.ip}}"  
  submask: 255.255.0.0
  desc: "{{vl10.desc}}"
- id: 30 
  use: "{{vl30.use}}"
  vrf: 10
  ip: "{{vl30.ip}}"  
  submask: 255.255.255.128
  desc: "{{vl10.desc}}"

```

Now SVI's IP address or description in host_vars 
## host1 : host_vars

```yml 
vl10:
 use: true
 ip: "1.1.1.1"
 desc: "This is Vlan 10 in Host1"

vl30:
  use: true
  ip: "3.3.3.3"
  desc: "Vlan 30 used in Host1 "

```

## host2 : host_vars
```yml 
vl20:
  use: true
  ip: "2.2.2.2"
  desc: "Vlan 20 used in Host2 "
  
vl30:
  use: true
  ip: "3.3.3.4"
  desc: "Vlan 30 used in Host2 "
```

## svi : jinja template 
```j2
{% for vlan in vlans %}
{% if vlan.use %}
interface Vlan {{vlan.id}}
 description {{vlan.desc}}
 ip vrf forwarding {{vlan.vrf}}
 ip address {{vlan.ip}} {{vlan.submask}}
 no ip redirects 
 no ip unreachables
 no ip proxy-arp
!
{% endif %}
{% endfor %}
```

## var_svi_jinja playbook 
```yml 
---
- name:  PLAY WITH ANSIBLE 
  hosts: Group1
  gather_facts: no
  connection: local 

  tasks: 
  - name: SVI CREATION WITH JINJA 
    template:
      src: svi.j2
      dest: ./{{inventory_hostname}}.md
```


## Result 

$ cat host1.md 
```shell

interface Vlan 10
 description This is Vlan 10 in Host1
 ip vrf forwarding 10
 ip address 1.1.1.1 255.255.255.0
 no ip redirects 
 no ip unreachables
 no ip proxy-arp
!
interface Vlan 30
 description This is Vlan 10 in Host1
 ip vrf forwarding 10
 ip address 3.3.3.3 255.255.255.128
 no ip redirects 
 no ip unreachables
 no ip proxy-arp
!

```
$ cat host2.md
```shell

interface Vlan 20
 description Vlan 20 used in Host2 
 ip vrf forwarding 20
 ip address 2.2.2.2 255.255.255.128
 no ip redirects 
 no ip unreachables
 no ip proxy-arp
!
interface Vlan 30
 description Vlan 30 used in Host2
 ip vrf forwarding 10
 ip address 3.3.3.4 255.255.255.128
 no ip redirects 
 no ip unreachables
 no ip proxy-arp
!
```

