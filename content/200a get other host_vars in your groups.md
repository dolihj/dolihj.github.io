If you would like to print out Group 'GROUP1' variables each host _vars variable 

- Group Name : GROUP1
- Host_vars : sitenum  
```ini
# Inventory file 
[GROUP1]
host_101
host_102
host_103
```

```yml 
#host_101.yml 
sitenum: 101

#host_102.yml 
sitenum: 102

#host_103.yml 
sitenum: 103

```

```yml 

  - name: Groupmembers variable print out 
    debug: 
      msg:   
      - "{{hostvars[item].sitenum}}" 
    loop: "{{groups['GROUP1']}}" 


    #hostvars[inventory_hostname].keys()
```

# Example) MPLS ping in the group 

```yml 

    tasks:

      - name: PING MPLS  
        ios_command:
          commands: 
            - ping mpls ipv4 {{ hostvars[item].ansible_host }} 255.255.255.255 source {{ansible_host}}
        with_items: "{{groups['GROUP1']}}" 
        register: output
        when: hostvars[item].ansible_host != ansible_host

```