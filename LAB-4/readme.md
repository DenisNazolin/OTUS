### Схема CLOS
### Цели
Построение BGP для Underlay

### Конфигурация устройств
#### pd01-clf-001
```
feature bgp
router bgp 65001
  timers bgp 5 15
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.0.1.1/32
    maximum-paths 64
    maximum-paths ibgp 64
  neighbor 172.16.0.0/16 remote-as route-map LEAF
    address-family ipv4 unicast

route-map LEAF permit 10
  match as-number 65001-65999 
```

#### pd01-clf-002
```
feature bgp
router bgp 65001
  timers bgp 5 15
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.0.2.1/32
    maximum-paths 64
    maximum-paths ibgp 64
  neighbor 172.16.0.0/16 remote-as route-map LEAF
    address-family ipv4 unicast

route-map LEAF permit 10
  match as-number 65001-65999 
```
#### pd01-clf-003
```
feature bgp
router bgp 65001
  timers bgp 5 15
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.0.3.1/32
    maximum-paths 64
    maximum-paths ibgp 64
  neighbor 172.16.0.0/16 remote-as route-map LEAF
    address-family ipv4 unicast

route-map LEAF permit 10
  match as-number 65001-65999 
```
#### pd01-elf-001
```
feature bgp
router bgp 65002
  router-id 10.1.1.1
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.1.1.1/32
    maximum-paths 64
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
#### pd01-elf-002
```
feature bgp
router bgp 65003
  router-id 10.2.2.2
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.2.2.2/32
    maximum-paths 64
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
#### pd01-elf-003
```
feature bgp
router bgp 65004
  router-id 10.3.3.3
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.3.3.3/32
    maximum-paths 64
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

#### pd01-elf-004
```
feature bgp
router bgp 65005
  router-id 10.4.4.4
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.4.4.4/32
    maximum-paths 64
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
pd01-clf-001# sh ip bgp summary
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 10.0.1.1, local AS number 65001
BGP table version is 20, IPv4 Unicast config peers 9, capable peers 8
5 network entries and 9 paths using 1700 bytes of memory
BGP attribute entries [5/860], BGP AS path entries [4/24]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.16.1.1      4 65002    1170    1169       20    0    0 01:37:04 1
172.16.1.3      4 65002    1170    1169       20    0    0 01:37:05 1
172.16.1.5      4 65003    1165    1163       20    0    0 01:36:37 1
172.16.1.7      4 65003    1165    1164       20    0    0 01:36:40 1
172.16.1.9      4 65004    1183    1181       20    0    0 01:35:58 1
172.16.1.11     4 65004    1185    1184       20    0    0 01:35:59 1
172.16.1.13     4 65005    1153    1152       20    0    0 01:35:38 1
172.16.1.15     4 65005    1153    1152       20    0    0 01:35:38 1

```


#### pd01-elf-001
```
pd01-elf-001# sh ip bgp summary
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 10.1.1.1, local AS number 65002
BGP table version is 22, IPv4 Unicast config peers 6, capable peers 6
7 network entries and 25 paths using 3868 bytes of memory
BGP attribute entries [5/860], BGP AS path entries [4/36]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.16.1.0      4 65001    1368    1364       22    0    0 01:53:18 4
172.16.1.2      4 65001    1368    1364       22    0    0 01:53:19 4
172.16.2.0      4 65001    1366    1363       22    0    0 01:53:13 4
172.16.2.2      4 65001    1367    1364       22    0    0 01:53:18 4
172.16.3.0      4 65001    1367    1363       22    0    0 01:53:16 4
172.16.3.2      4 65001    1367    1363       22    0    0 01:53:16 4
```

#### Выводы 
```
связь есть со всеми лифами и спайнами 



