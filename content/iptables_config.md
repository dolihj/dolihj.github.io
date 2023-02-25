# iptable options 

## basic command 
-N, --new-chain
-L, --list 
-X, --delete-chain
-P, --policy 
-F, --flush
-A, --append
-I, --insert chain [rulenum] rulenum (default 1= first )
-R, --replace
-D, --delete -D chain [rulenum]

## control option 
-s, -d : source , destination address 
--sport, --dport
-p, --protocol 
-i, --in-interface 
-o, --out-interface
-f, --fragment 
-j, --jump 




![[BPP Config#BPP-OTHER-2]]

## A2 
>iptables -A INPUT -p tcp  --sport bgp -i eth1,eth0 -j DROP
>iptables -A INPUT -p tcp  --dport bgp -i eth1,eth0 -j DROP
>iptables -A INPUT -p ip  -i eth1,eth0 -j ACCEPT
>