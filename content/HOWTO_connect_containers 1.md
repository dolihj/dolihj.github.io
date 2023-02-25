# How to access docker container network neamespace 

## get container process id 

To create namespace of docker container, container's process id needed. docker inspect command can get process id.
It is recommended to have [[docker001_install and basic usage#docker container name| docker container name]] attribute.

````diff
docker inspect [Docker] --format '{{.State.Pid}}'
pid = "$(docker inspect -f '{{.State.Pid}}' "CONTAINER_NAME | UUID")"
````

````diff
! Example  
pid1="$(docker inspect -f '{{.State.Pid}}' "CoreParser1")"
echo $pid1 
````


## Create Network Namespace 

After having container's process id, it will be linked to namespace. mount is recommended rather than symbolic link. Unlike symbolic link, mount is not persistent thus it would be added in init process to keep mount point. 

### option1 : mount (RECOMMENDED)
````diff
! CREATE FILE 
touch /run/netns/<NETWORK_NAMESPACE>
! PERMIT MOUNT POINT DIRECTORY 
chmod 777 /run/netns/
!chmod  /run/netns/<NETWORK_NAMESPACE>

! CREATE MOUNT POINT FOR CONTAINER 
mount --bind /proc/$pid/ns/net /run/netns/<NETWORK_NAMESPACE>

! CHECK NETEWORK NAMESPACE 
ip netns list 
````
````diff
! Example 
ns1=<NETWORK_NAMESPACE> 
touch /run/netns/$ns1
chmod 777 /run/netns
mount --bind /proc/$pid/ns/net /run/netns/$ns1
ip netns list

````

### option2 : symbolic link ( NOT RECOMMEDNDED )
````
sudo ln -s /proc/$pid/ns/net cpns 
ip netns 
sudo ip netns exec cpns ip addr 
````


## Create veth peer in host and assign to the namespace 

`````diff
! OPTION 1 

! Virtual ethernet (veth) is created as a pair  HOST_VETH and CONTAINER_VETH is connected as created. 
ip link add <HOST_VETH> type veth peer name <CONTAINER_VETH>  


! Assign CONTAINER_VETH to network name space 
ip link set <CONTAINER_VETH> netns <NETWORK_NAMESPACE>

! OPTION2 
! While creating veth pair, one veth can be assigned to namespace in one cli 
ip link add <HOST_VETH> type veth peer name <CONTAINER_VETH> netns <NETWORK_NAMESPACE>


! BRING INTERFACES UP in default namespace(host)
ip link set dev <HOST_VETH> up 

! Bring interface up in user-defined namespace 
ip netns exec <NETWORK_NAMESPACE> ip link set dev <HOST_VETH> up 

`````

`````diff
! EXAMPLE 
!OPTION 2 
ip link add A1 type veth peer name cA1 netns cpns


! BRING UP INTEFACES 
ip link set dev A1 up
ip netns exec $ns1 ip link set dev cA1 up 

! CHECK INTEFACES 
ip netns exec $ns1 ip link 
docker exec CoreParser1 ip link 
`````


 
## Create master bridge : core_brdg

> command 
`````diff
! Create new bridge  
ip link add <BRIDGE_NAME>  type bridge
ip link set dev <BRIDGE_NAME>  up

! add hveth to bridge 
ip link set <HOST_VETH1> master <BRIDGE_NAME> 
ip link set <HOST_VETH2> master <BRIDGE_NAME> 

! bring up interfaces 
ip link set dev <HOST_VETH1> up 
ip link set dev <HOST_VETH2> up 

! CHECK BRIDGE 

brctl show 
ip netns exec cpns brctl show 

`````

> example : core_brdg
`````diff 
! Create new bridge  
ip link add core_brdg type bridge
ip link set dev core_brdg up

! add hveth to bridge 
ip link set A1 master core_brdg

`````


## [[container_creation_shell]]
## [[Linux_container_interface_connect]]
## Add Routing table in Test container  
Routing table for core network route in connected container such as sensors. 
````
ip netns exec [Container1] ip route add 10.0.0.0/8 via [Gateway]
````
