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


#### Нстройки под Vxlan

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

#### настройка BGP 

ip route 10.0.1.0/30 Null0

interface loopback1
  ip address 10.0.1.2/32
icam monitor scale






