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




