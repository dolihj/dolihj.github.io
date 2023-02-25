# RD and RT difference

[PacketLife_explanation](http://packetlife.net/blog/2013/jun/10/route-distinguishers-and-route-targets/)

Route-Distinguisher (RD) & Route-Target (RT) are two different concepts that are both used in an MPLS VPN. The RD is used to keep all prefixes in the BGP table unique, and the RT is used to transfer routes between VRF’s/VPNS. Let’s take a look at an example.

RD는 그냥  VRF/ customer 구분인자 Unique Id이고. 
RT는 routing table의 BGP ext. community 로  VRF간 route을 서로 learn 할 수 있다.

RT import 는 learn routing table from Network or
RT export 는 advertise this routing to Network 

[[BGP]]