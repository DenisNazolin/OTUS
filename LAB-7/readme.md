### VPC. 
### Цели Настроить VPC на двух и более нексусах

### Коллеги,  сразу прошу прощения так как словил баг по невозможности поднять evnp в лабе в связи с тем что Lo добавленый как sourse nve сразу уходит в down пробовал перезагружать добовлять новые устроства и так далее, на данной конкретной лабе ничего не сработало. Но настройки верные.

### Конфигурация устройств

#### Spine
```
#### Включенные feature
nv overlay evpn
feature bgp
feature fabric forwarding
feature private-vlan
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay

#### Настройка BGP
router bgp 65001
  timers bgp 5 15
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.0.1.2/32
    maximum-paths 64
    maximum-paths ibgp 64
  address-family l2vpn evpn
    retain route-target all
  neighbor 10.0.0.0/8 remote-as route-map LEAF
    address-family l2vpn evpn
      send-community
      send-community extended
      route-map NEXT_HOP out
  neighbor 172.16.0.0/16 remote-as route-map LEAF
    address-family ipv4 unicast

route-map LEAF permit 10
  match as-number 65001-65999
route-map NEXT_HOP permit 10
  set ip next-hop unchanged

#### Интерфейсы 
Lo1                  10.0.1.2        protocol-up/link-up/admin-up
Eth1/1               172.16.1.0      protocol-up/link-up/admin-up
Eth1/2               172.16.1.2      protocol-up/link-up/admin-up
Eth1/3               172.16.1.4      protocol-up/link-up/admin-up
```
#### leaf-1
```
#### Настройки под Vxlan
vlan 1,10,42,999
vlan 10
  vn-segment 10010
vlan 999
  vn-segment 10999
vrf context PROD
  vni 10999
  rd 10.1.1.2:10999
  address-family ipv4 unicast
    route-target import 10999:99
    route-target import 10999:99 evpn
    route-target export 10999:99
    route-target export 10999:99 evpn





