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


5


6


