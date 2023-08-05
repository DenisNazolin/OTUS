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