 [[IPSec]]

ref : https://gns3vault.com/tunneling/site-to-site-ipsec-vpn
ref2: https://www.cisco.com/c/ko_kr/support/docs/security-vpn/ipsec-negotiation-ike-protocols/119425-configure-ipsec-00.html#anc15
[[101_What_is_IPSec]]

# Site-to-Site IPSEC VPN

## Scenario:
Your network colleagues were very enthusiastic when you showed them that a GRE tunnel makes it possible to tunne routing protocols across VPN connections, and after configuring the previous “GRE Tunnel Basic” lab (see our lab section) your colleagues now ask you to configure a basic IPSEC Site-to-Site VPN so they can configure encrypted GRE tunnels later.

## Goal:
All IP addresses have been preconfigured as specified in the topology picture.
Router Godzilla and Nessie have a loopback interface:
Godzilla: Loopback0: 1.1.1.1 /24
Nessie: Loopback0: 3.3.3.3 /24
Configure OSPF on all 3 routers and advertise the following networks:
192.168.12.0 /24
192.168.23.0 /24
1.1.1.0 /24
3.3.3.0 /24
Ensure that Godzilla and Nessie can ping each other.

Ensure you can ping 3.3.3.3 from Godzilla, sourced from it’s Loopback0 interface.
We are going to configure an IPSEC connection between Router Godzilla and Nessie.

## Create a ISAKMP policy:
Authentication: pre-shared-key
Encryption: AES 256
Hashing: SHA
DH: Group 5
Lifetime: 3600

```
D2(config)#do sho run | sec isakmp
crypto isakmp policy 1
 encr aes
 hash sha256
 authentication pre-share !default is RSA 
 group 2
 lifetime 3600
```

## Pre-Shared-Key 
Configure the pre-shared-key : “OurKey” which you will use for the IPSEC connection.
### Set peer ISAKMP identity
```diff
+ D2
crypto isakmp identity address 
-D2(config)#crypto isakmp key OurKey address 8.8.3.2
crypto isakmp key OurKey address 4.0.0.1

+ D4 
crypto isakmp identity address 
-D4(config)#crypto isakmp key OurKey address 8.8.0.2

```
# CONFIG IPSEC 
````diff
## 1. ACL 
!D2
ip access-list ext STS-VPN-D2D4 
permit ip 10.0.0.0 0.0.0.255 10.0.4.0 0.0.0.255

!D4
ip access-list extended STS-VPN-D4 
permit ip 10.0.4.0 0.0.0.255 10.0.0.0 0.0.0.255
## 2. IPSEC transform-set: (ISAKMP Phase2 policy)

!Cipher: AES 256
!ESP (Encapsulating Security Protcol)
!Hashing: SHA

crypto ipsec transform-set STS-D2D4 esp-aes esp-sha-hmac 

D4(config)#crypto ipsec transform-set STS-VPN-D2 esp-aes esp-sha-hmac
D4(cfg-crypto-trans)#


## 3. CryptoMAP 
crypto map D2MAP 10 ipsec-isakmp 
  set peer 8.8.3.2
  set transform-set STS-D2D4
  match address STS-VPN-D2D4 


D4(config)#crypto ipsec transform-set STS-VPN-D2 esp-aes esp-sha-hmac
! D4
crypto map D4MAP 10 ipsec-isakmp 
 ! Incomplete
 set peer 8.8.0.2
 set transform-set STS-VPN-D2 
 match address STS-VPN-D2

## 4. Apply to Interface 
interface eth1/0
  crypto map STS-VPN 

## Change the IPSEC security association lifetime to 1800 seconds.

You need to encrypt traffic from Router Godzilla’s Loopback0 interface destined to Nessie’s Loopback0 interface, create the correct access-list.
Ensure you have a correct access-list on both Routers.

## Crypto-Map 
Create the correct crypto-map to finish the IPSEC configuration.
```
crypto map MYMAP 10 ipsec-isakmp 
match address 100  
set peer 
set pfs group5 

inteface 
crypto map 

Verify the IPSEC configuration, you can use the following show/debug commands:
show crypto ipsec transform-set
show crypto map
show crypto ipsec sa
debug crypto isakmp

Try a ping from Router Godzilla’s Loopback0 interface destined to Router Nessie’s Loopback0 interface, if your configuration is correct then traffic should be encrypted.
