

# Ref 
**Bridge interface create  : https://seungjuitmemo.tistory.com/225

Scott's Weblog, Introducing Linux Network Namescpace : [https://blog.scottlowe.org/2013/09/04/introducing-linux-network-namespaces/](https://blog.scottlowe.org/2013/09/04/introducing-linux-network-namespaces/)

44bits, Network namespace and bridge network practice:[https://www.44bits.io/ko/post/container-network-2-ip-command-and-network-namespace](https://www.44bits.io/ko/post/container-network-2-ip-command-and-network-namespace)


# Link 
[[container_creation_shell]]


# How to access docker container network neamespace 

## get container process id 

>docker inspect [Docker] --format '{{.State.Pid}}'

>pid = "$(docker inspect -f '{{.State.Pid}}' "CONTAINER_NAME | UUID")"

```diff 
! Example  
pid="$(docker inspect -f '{{.State.Pid}}' "CoreParser1")"
echo $pid 


pid2="$(docker inspect -f '{{.State.Pid}}' "CoreParser2")"
echo $pid2
```

![HJ_Example](docker_inspect_State_Pid.png)

## Create Network Namespace : cpns 

### option1 : mount
```
! CREATE FILE 
touch /run/netns/<NETWORK_NAMESPACE>
! PERMIT MOUNT POINT DIRECTORY 
chmod 777 /run/netns/
!chmod  /run/netns/<NETWORK_NAMESPACE>

! CREATE MOUNT POINT FOR CONTAINER 
mount --bind /proc/$pid/ns/net /run/netns/<NETWORK_NAMESPACE>

ip netns list
```

### option2 : symbolic link ( NOT RECOMMEDNDED )
```
sudo ln -s /proc/$pid/ns/net cpns 
ip netns 
sudo ip netns exec cpns ip addr 
```


## Create veth peer in host and assign in netns 
> command 
````diff
! OPTION 1
ip link add <HOST_VETH> type veth peer name <CONTAINER_VETH>  
ip link set <CONTAINER_VETH> netns <NETWORK_NAMESPACE>

! OPTION2 
ip link add <HOST_VETH> type veth peer name <CONTAINER_VETH> netns <NETWORK_NAMESPACE>

! BRING INTERFACES UP 
ip link set dev <HOST_VETH> up 


````
> Example 
````diff
!OPTION 2 
sudo ip link add A1 type veth peer name cA1 netns cpns
sudo ip link add A2 type veth peer name cA2 netns cpns
sudo ip link add A3 type veth peer name cA3 netns cpns
sudo ip link add B1 type veth peer name cB1 netns cpns
sudo ip link add B2 type veth peer name cB2 netns cpns
sudo ip link add C1 type veth peer name cC1 netns cpns
sudo ip link add C2 type veth peer name cC2 netns cpns
sudo ip link add C3 type veth peer name cC3 netns cpns

! BRING UP INTEFACES 
sudo ip link set dev A1 up
sudo ip link set dev A2 up
sudo ip link set dev A3 up
sudo ip link set dev B1 up
sudo ip link set dev B2 up
sudo ip link set dev C1 up
sudo ip link set dev C2 up
sudo ip link set dev C3 up

! CHECK INTEFACES 
ip netns exec cpns ip link 
docker exec CoreParser1 ip link 
````


 
## Create master bridge : core_brdg

> command 
````diff
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

````

> example : core_brdg
````diff 
! Create new bridge  
ip link add core_brdg type bridge
ip link set dev core_brdg up

! add hveth to bridge 
ip link set hveth1 master core_brdg
ip link set hveth2 master core_brdg

! bring up interfaces 
ip link set dev hveth1 up 
ip link set dev hveth2 up 

````


  

