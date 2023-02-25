## TEST Linux container 
````diff
u1="$(docker inspect -f '{{.State.Pid}}' "U1")"
u2="$(docker inspect -f '{{.State.Pid}}' "U2")"

touch /run/netns/u1
touch /run/netns/u2

! CREATE MOUNT POINT FOR CONTAINER 
mount --bind /proc/$u1/ns/net /run/netns/u1
mount --bind /proc/$u2/ns/net /run/netns/u2

ip netns list



sudo ip link add U1h type veth peer name U1c netns u1
sudo ip link add U2h type veth peer name U2c netns u2

sudo ip link set dev U1h up
sudo ip link set dev U2h up

ip netns exec u1 ip link set dev U1c up
ip netns exec u2 ip link set dev U2c up


ip link set U1h master br_A
ip link set U2h master br_C 

ip link set U1h master br_A
ip link set U2h master br_C 



ip netns exec u1 ip add add 10.1.4.2/24 dev U1c
ip netns exec u2 ip add add 10.3.4.2/24 dev U2c
````