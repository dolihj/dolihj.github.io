# VxLAN EVPN terminology 

[ARISTA BGP EVPN – OVERVIEW AND CONCEPTS](https://overlaid.net/2018/08/27/arista-bgp-evpn-overview-and-concepts/)
VTEP/ Leaf 설명이 잘 되어 있음. 

[Configuration and Verification VXLAN with MP-BGP EVPN Control Plane](https://www.cisco.com/c/en/us/support/docs/ip/border-gateway-protocol-bgp/200952-Configuration-and-Verification-VXLAN-wit.html)

[VXLAN Design with Cisco Nexus 9300 Platform Switches](https://www.cisco.com/c/en/us/products/collateral/switches/nexus-9000-series-switches/white-paper-c11-732453.html#_Toc401870709)

[Configuring VXLAN BGP EVPN](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/7-x/vxlan/configuration/guide/b_Cisco_Nexus_9000_Series_NX-OS_VXLAN_Configuration_Guide_7x/b_Cisco_Nexus_9000_Series_NX-OS_VXLAN_Configuration_Guide_7x_chapter_0100.pdf)


ENCOR 350-401 book
traditional L2 problem
vlan 4094 limit: not enough
necessary of large mac addr : more vm
STP - large num of disabled links : waste of resource
ECMP not supported
Host mobility not suported
terminology
vxlan segment
Vlan_id vs VNI 12bit(4094) : 24bit (16m)


Terminology
Equal-cost multi-path routing (ECMP)
NLRI : Network Layer Reachability Info.
NVE: 
   NVO: Network Virtualization Overlay

   NVE: Network Virtualization Endpoint

   VNI:  Virtual Network Identifier (for VXLAN)

   VSID: Virtual Subnet Identifier (for NVGRE)

   EVPN: Ethernet VPN
   
   EVI: An EVPN instance spanning the Provider Edge (PE) devices
   participating in that EVPN.

   MAC-VRF: A Virtual Routing and Forwarding table for Media Access
   Control (MAC) addresses on a PE.

   Ethernet Segment (ES): When a customer site (device or network) is
   connected to one or more PEs via a set of Ethernet links, then that
   set of links is referred to as an 'Ethernet segment'.

   Ethernet Segment Identifier (ESI): A unique non-zero identifier that
   identifies an Ethernet segment is called an 'Ethernet Segment
   Identifier'.

   Ethernet Tag: An Ethernet tag identifies a particular broadcast
   domain, e.g., a VLAN.  An EVPN instance consists of one or more
   broadcast domains.

   PE: Provider Edge device.

   Single-Active Redundancy Mode: When only a single PE, among all the
   PEs attached to an Ethernet segment, is allowed to forward traffic
   to/from that Ethernet segment for a given VLAN, then the Ethernet
   segment is defined to be operating in Single-Active redundancy mode.

   All-Active Redundancy Mode: When all PEs attached to an Ethernet
   segment are allowed to forward known unicast traffic to/from that
   Ethernet segment for a given VLAN, then the Ethernet segment is
   defined to be operating in All-Active redundancy mode.
