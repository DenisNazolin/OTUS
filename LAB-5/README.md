### Схема CLOS
### Цели
Построение BGP для Underlay
### Конфигурация устройств
#### pd01-clf-001
```
#### Включенные feature

feature ngmvpn
nv overlay evpn
feature bgp
feature fabric forwarding
feature private-vlan
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay


#### Настройки под Vxlan

hardware access-list tcam region racl 512
hardware access-list tcam region arp-ether 256 double-wide

interface Vlan1

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 10010
    suppress-arp
    ingress-replication protocol bgp
  member vni 10020
    suppress-arp
    ingress-replication protocol bgp

#### Настройка BGP 

ip route 10.0.1.0/30 Null0

interface loopback1
  ip address 10.0.1.2/32
icam monitor scale


route-map LEAF permit 10
  match as-number 65001-65999


router bgp 65001
  timers bgp 5 15
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.0.1.0/30
    maximum-paths 64
    maximum-paths ibgp 64
  address-family l2vpn evpn
    retain route-target all
  template peer OVER
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 10.0.0.0/8 remote-as route-map LEAF
    inherit peer OVER
    update-source loopback1
  neighbor 172.16.0.0/16 remote-as route-map LEAF
    address-family ipv4 unicast

```

#### pd01-clf-002
```
#### Настройки под Vxlan

hardware access-list tcam region racl 512
hardware access-list tcam region arp-ether 256 double-wide

interface Vlan1

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 10010
    suppress-arp
    ingress-replication protocol bgp
  member vni 10020
    suppress-arp
    ingress-replication protocol bgp

#### Настройка BGP 

ip route 10.0.2.0/30 Null0

interface loopback1
  ip address 10.0.2.2/32
icam monitor scale


route-map LEAF permit 10
  match as-number 65001-65999


router bgp 65001
  timers bgp 5 15
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.0.2.0/30
    maximum-paths 64
    maximum-paths ibgp 64
  address-family l2vpn evpn
    retain route-target all
  template peer OVER
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 10.0.0.0/8 remote-as route-map LEAF
    inherit peer OVER
    update-source loopback1
  neighbor 172.16.0.0/16 remote-as route-map LEAF
    address-family ipv4 unicast
```

#### pd01-clf-003
```
#### Настройки под Vxlan

hardware access-list tcam region racl 512
hardware access-list tcam region arp-ether 256 double-wide

interface Vlan1

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 10010
    suppress-arp
    ingress-replication protocol bgp
  member vni 10020
    suppress-arp
    ingress-replication protocol bgp

#### Настройка BGP 

ip route 10.0.3.0/30 Null0

interface loopback1
  ip address 10.0.3.2/32
icam monitor scale


route-map LEAF permit 10
  match as-number 65001-65999


router bgp 65001
  timers bgp 5 15
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.0.3.0/30
    maximum-paths 64
    maximum-paths ibgp 64
  address-family l2vpn evpn
    retain route-target all
  template peer OVER
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 10.0.0.0/8 remote-as route-map LEAF
    inherit peer OVER
    update-source loopback1
  neighbor 172.16.0.0/16 remote-as route-map LEAF
    address-family ipv4 unicast
```

#### pd01-elf-01
```
#### Настройки под Vxlan

hardware access-list tcam region racl 512
hardware access-list tcam region arp-ether 256 double-wide

vlan 1,10,20
vlan 10
  vn-segment 10010
vlan 20
  name 20
  vn-segment 10020

evpn
  vni 10010 l2
    rd 10.1.1.2:10010
    route-target import 1:10010
    route-target export 1:10010
  vni 10020 l2
    rd 10.1.1.2:10020
    route-target import 1:10020
    route-target export 1:10020

interface Vlan1

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 10010
    suppress-arp
    ingress-replication protocol bgp
  member vni 10020
    suppress-arp
    ingress-replication protocol bgp

#### Настройка BGP 

interface loopback1
  ip address 10.1.1.2/32
icam monitor scale


router bgp 65002
  router-id 10.1.1.1
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.1.1.1/32
    network 10.1.1.2/32
    maximum-paths 64
  address-family l2vpn evpn
    retain route-target all
  template peer OVER
    update-source loopback1
    ebgp-multihop 20
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 10.0.1.2
    inherit peer OVER
    remote-as 65001
    address-family l2vpn evpn
  neighbor 10.0.2.2
    inherit peer OVER
    remote-as 65001
    address-family l2vpn evpn
  neighbor 10.0.3.2
    inherit peer OVER
    remote-as 65001
    address-family l2vpn evpn
  neighbor 172.16.1.0
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.1.2
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.2.0
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.2.2
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.3.0
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.3.2
    remote-as 65001
    address-family ipv4 unicast
```
#### pd01-elf-02
```
#### Настройки под Vxlan

hardware access-list tcam region racl 512
hardware access-list tcam region arp-ether 256 double-wide

vlan 1,10,20
vlan 10
  vn-segment 10010
vlan 20
  name 20
  vn-segment 10020

evpn
  vni 10010 l2
    rd 10.2.2.3:10010
    route-target import 1:10010
    route-target export 1:10010
  vni 10020 l2
    rd 10.2.2.3:10020
    route-target import 1:10020
    route-target export 1:10020

interface Vlan1

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 10010
    suppress-arp
    ingress-replication protocol bgp
  member vni 10020
    suppress-arp
    ingress-replication protocol bgp

#### Настройка BGP 

interface loopback1
  ip address 10.2.2.3/32
icam monitor scale

router bgp 65003
  router-id 10.2.2.2
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.2.2.2/32
    network 10.2.2.3/32
    maximum-paths 64
  address-family l2vpn evpn
  template peer OVER
    update-source loopback1
    ebgp-multihop 20
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 10.0.1.2
    inherit peer OVER
    remote-as 65001
    address-family l2vpn evpn
  neighbor 10.0.2.2
    inherit peer OVER
    remote-as 65001
    address-family l2vpn evpn
  neighbor 10.0.3.2
    inherit peer OVER
    remote-as 65001
    address-family l2vpn evpn
  neighbor 172.16.1.4
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.1.6
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.2.4
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.2.6
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.3.4
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.3.6
    remote-as 65001
    address-family ipv4 unicast
```
#### pd01-elf-03
```
#### Настройки под Vxlan

hardware access-list tcam region racl 512
hardware access-list tcam region arp-ether 256 double-wide

vlan 1,10,20
vlan 10
  vn-segment 10010
vlan 20
  name 20
  vn-segment 10020

evpn
  vni 10010 l2
    rd 10.3.3.4:10010
    route-target import 1:10010
    route-target export 1:10010


interface Vlan1

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 10010
    suppress-arp
    ingress-replication protocol bgp
  member vni 10020
    suppress-arp
    ingress-replication protocol bgp

#### Настройка BGP 

interface loopback1
  ip address 10.3.3.4/32
icam monitor scale

router bgp 65004
  router-id 10.3.3.3
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.3.3.3/32
    network 10.3.3.4/32
    maximum-paths 64
  address-family l2vpn evpn
    retain route-target all
  template peer OVER
    update-source loopback1
    ebgp-multihop 20
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 10.0.1.2
    inherit peer OVER
    remote-as 65001
    address-family l2vpn evpn
  neighbor 10.0.2.2
    inherit peer OVER
    remote-as 65001
    address-family l2vpn evpn
  neighbor 10.0.3.2
    inherit peer OVER
    remote-as 65001
    address-family l2vpn evpn
  neighbor 172.16.1.8
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.1.10
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.2.8
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.2.10
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.3.8
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.3.10
    remote-as 65001
    address-family ipv4 unicast
```
#### pd01-elf-04
```
#### Настройки под Vxlan

hardware access-list tcam region racl 512
hardware access-list tcam region arp-ether 256 double-wide

vlan 1,10,20
vlan 10
  vn-segment 10010
vlan 20
  name 20
  vn-segment 10020

evpn
  vni 10010 l2
    rd 10.4.4.5:10010
    route-target import 1:10010
    route-target export 1:10010
  vni 10020 l2
    rd 10.4.4.5:10020
    route-target import 1:10020
    route-target export 1:10020


interface Vlan1

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 10010
    suppress-arp
    ingress-replication protocol bgp
  member vni 10020
    suppress-arp
    ingress-replication protocol bgp

#### Настройка BGP 

interface loopback1
  ip address 10.4.4.5/32
icam monitor scale

router bgp 65005
  router-id 10.4.4.4
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.4.4.4/32
    network 10.4.4.5/32
    maximum-paths 64
  address-family l2vpn evpn
    retain route-target all
  template peer OVER
    update-source loopback1
    ebgp-multihop 20
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 10.0.1.2
    inherit peer OVER
    remote-as 65001
    address-family l2vpn evpn
  neighbor 10.0.2.2
    inherit peer OVER
    remote-as 65001
    address-family l2vpn evpn
  neighbor 10.0.3.2
    inherit peer OVER
    remote-as 65001
    address-family l2vpn evpn
  neighbor 172.16.1.12
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.1.14
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.2.12
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.2.14
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.3.12
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.3.14
    remote-as 65001
    address-family ipv4 unicast
```

### Вывод  команд проверки 
#### pd01-clf-001
```
pd01-clf-01# sh bgp l2vpn evpn summary

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.1.1.2        4 65002    3565    3557       41    0    0 04:56:11 3
10.2.2.3        4 65003    3569    3569       41    0    0 04:57:13 2
10.3.3.4        4 65004    3573    3573       41    0    0 04:57:29 3
10.4.4.5        4 65005    3571    3561       41    0    0 04:56:33 5

pd01-clf-01# sh bgp l2vpn evpn

Route Distinguisher: 10.1.1.2:10020
*>e[2]:[0]:[0]:[48]:[5000.000e.0000]:[0]:[0.0.0.0]/216
                      10.1.1.2                                       0 65002 i
*>e[3]:[0]:[32]:[10.1.1.2]/88
                      10.1.1.2             


Route Distinguisher: 10.4.4.5:10020
*>e[2]:[0]:[0]:[48]:[5000.000b.0000]:[0]:[0.0.0.0]/216
                      10.4.4.5                                       0 65005 i
*>e[3]:[0]:[32]:[10.4.4.5]/88
                      10.4.4.5                                       0 65005 i

#### Типы 2 и 3 есть от leaf 1 и 4 

#### Так же соседство поднято со всеми leaf 

```
#### pd01-elf-001
```
pd01-elf-01# sh bgp l2vpn evpn  summary

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.0.1.2        4 65001    3664    3649       76    0    0 05:03:46 12
10.0.2.2        4 65001     415     405       76    0    0 00:33:20 12
10.0.3.2        4 65001     374     366       76    0    0 00:30:07 10

#### Поднято соседство со всеми spine 


pd01-elf-01# sh mac address-table
Legend:
        * - primary entry, G - Gateway MAC, (R) - Routed MAC, O - Overlay MAC
        age - seconds since last seen,+ - primary entry using vPC Peer-Link,
        (T) - True, (F) - False, C - ControlPlane MAC, ~ - vsan
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
*    1     5000.000e.0000   dynamic  0         F      F    Eth1/7
C   10     5000.000a.0000   dynamic  0         F      F    nve1(10.0.1.2)
C   10     5000.000c.0000   dynamic  0         F      F    nve1(10.0.1.2)
C   20     5000.000b.0000   dynamic  0         F      F    nve1(10.0.1.2)
*   20     5000.000e.0000   dynamic  0         F      F    Eth1/7
G    -     5002.0000.1b08   static   -         F      F    sup-eth1(R)

####  так же вижу мак адреса через nve (сверил они совпадают с подключёнными устройствами)
```
#### Оконечные устройства 
```
В качестве оконечных устройств взял циско роутеры.
вижу следуюшие 
устройство 1 находиться за elf-01

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.1.2 255.255.255.0
end

#### Задал в качестве инкапсуляции 20 влан на самом роутере. Коллеги подсказали что так будет работать.
но пинг не работает 
Router# ping 192.168.1.42
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.42, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)

#### Хотя при этом арп я вижу отчётливо и он совпадает с устройством на той стороне 

#### устройство 1. 
Router#sh arp
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  192.168.1.2             -   5000.000e.0000  ARPA   GigabitEthernet0/0.20
Internet  192.168.1.42          174   5000.000b.0000  ARPA   GigabitEthernet0/0.20

#### устройство 2. 
Router#sh arp
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  192.168.1.2           176   5000.000e.0000  ARPA   GigabitEthernet0/0.20
Internet  192.168.1.42            -   5000.000b.0000  ARPA   GigabitEthernet0/0.20

#### Пробовал почистить апр моментально прилетает обратно.

Router#clear arp
Router#sh arp
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  192.168.1.2             -   5000.000e.0000  ARPA   GigabitEthernet0/0.20
Internet  192.168.1.42            0   5000.000b.0000  ARPA   GigabitEthernet0/0.20

#### Проверил через wireshark прохождение пакета. (теряется на spine линке в сторону хоста назначения его нет, хотя на линк от куда должен идти запрос приходит)
Маршрут по маку назначения есть,  но не пересылается туда. 
Та же иногда мной замечено что пакеты проходят 

Router#ping 192.168.1.2 repeat 100
Type escape sequence to abort.
Sending 100, 100-byte ICMP Echos to 192.168.1.2, timeout is 2 seconds:
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!
```








