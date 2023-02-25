# cisco_ios ssh config

```
ip domain-name hj.org
crypto key generate rsa
1024

  
! ssh user
username admin privilege 15 password 0 @dmin!@3
aaa new-model
ip ssh ver 2

  
! directly login privilege mode
line vty 0 15
privilege level 15
```

## Legacy KeyExchange and Crypto enable

>parkhj1@hjubuntu:~/BAATS$ sudo vim /etc/ssh/ssh_config
!
Host 192.168.122.*
   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,aes128-cbc,3des-cbc
   KexAlgorithms +diffie-hellman-group14-sha1


**![](https://lh3.googleusercontent.com/Ldi54luLfu-1JoE4Of1S1_jSf7tagL_peHzJOQBJ17KEr8v09GSa7g7hIagCoNoQsaozMf3f0uC6zMUxu3cCO3Y7o5md2Vtd82IKrNApJqiJU_sLCvhLHy0Rd3RxKdXbkUVp6iSZJZQXb75RcA)


## ssh public key 

```
ip ssh pubkey-chain
username parkhj1
key-string
AAAAB3NzaC1yc2EAAAADAQABAAAAgQCzjD/3VXZFzEVkEJ6CoPi0+1Q0sZtQwPpy2qOKmXwoSkn5pPT8duc8bwrBVf1aBAIODz1DRPm8N39eoVBbhEkDq23lR3Eu7188TP2Q6R7LR3VIJtb8BW4sacilxcABQp24CBkE4Q4Wi8fRb6ul/E5jVT0YKpQRtHKxsa12OH0ufw==
exit
exit

```