#switching 
## what is STP Dispute ? 

In Spanning Tree Protocol, two switches will decide who will be the root switch. 
Today's project in vlan 507,  Switches are connected M--- MSPP-----E  and M and E both were insisting itself as a root bridge. 

E switch ports are successfully set as Designate port / Forwarding status but 
M switch trunk ports was set as Designate / Blocking status ( Dispute ) for vlan 506, 507 

STP dispute usually happens when inferior BPDU is received from the peer even though  the same interface is sending Superior BPDU. 
Then switch blocks the port as the peer is not agree on root switch election and keeps sending BPDU. 

## Why  and how to resolve 

-  UDLD - it can be unidirectional link failure - thus failing sending Superior BPDU while receiving BPDU.  - In this specific case, M's port ( trunk) allowing vlan 501, 506,507 has no issue on vlan 501. thus link might not be the issue. 
-  native vlan :  M's port native vlan is 1 in the trunk ( default native vlan ) . if MSPP  peer switch has different native vlan config, it can cause an issue to STP, BPDU packet exchange. 
-  Cisco OS bug. - it is reported in old ios version






