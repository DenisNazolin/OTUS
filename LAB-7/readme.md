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


interface Vlan10
  no shutdown
  vrf member PROD
  no ip redirects
  ip address 192.168.10.253/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway


interface Vlan999
  no shutdown
  vrf member PROD
  no ip redirects
  ip forward
  no ipv6 redirects

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 999
  member vni 10010
    suppress-arp
    ingress-replication protocol bgp
  member vni 10999 associate-vrf

evpn
  vni 10010 l2
    rd 10.1.1.2:10010
    route-target import 1:10010
    route-target export 1:10010


#### Настройка BGP

router bgp 65000
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
  neighbor 172.16.1.0
    remote-as 65001
    address-family ipv4 unicast
      soft-reconfiguration inbound always

#### Настройка VPC 
vpc domain 10
  peer-switch
  role priority 10
  peer-keepalive destination 10.0.0.2 source 10.0.0.1
  peer-gateway

interface port-channel98
  switchport mode trunk
  vpc 10

interface Ethernet1/1
  switchport mode trunk
  channel-group 98 mode active

interface mgmt0
  vrf member management
  ip address 10.0.0.1/30

interface Ethernet1/4
  description vpc-peer-link
  switchport mode trunk
  channel-group 99 mode active

interface Ethernet1/5
  description vpc-peer-link
  switchport mode trunk
  channel-group 99 mode active

interface port-channel99
  description vpc-peer-link (eth1/4-5)
  switchport mode trunk
  spanning-tree port type network
  vpc peer-link
```

#### leaf-2
```
#### Настройки под Vxlan

fabric forwarding anycast-gateway-mac 0003.0002.0001
vlan 1,10,999
vlan 10
  vn-segment 10010
vlan 999
  vn-segment 10999

vrf context PROD
  vni 10999
  rd 10.2.2.3:10999
  address-family ipv4 unicast
    route-target import 10999:99
    route-target import 10999:99 evpn
    route-target export 10999:99
    route-target export 10999:99 evpn

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 999
  member vni 10010
    ingress-replication protocol bgp
  member vni 10999 associate-vrf

evpn
  vni 10010 l2
    rd 10.2.2.3:10010
    route-target import 1:10010
    route-target export 1:10010

#### Настройка BGP
router bgp 65000
  router-id 10.2.2.2
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.2.2.2/32
    network 10.2.2.3/32
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
  neighbor 172.16.1.2
    remote-as 65001
    address-family ipv4 unicast
  neighbor 172.16.2.2
    remote-as 65001
    address-family ipv4 unicast

#### Настройка VPC

vpc domain 10
  peer-switch
  role priority 20
  peer-keepalive destination 10.0.0.1 source 10.0.0.2
  peer-gateway

interface port-channel99
  description vpc-peer-link (eth1/4-5)
  switchport mode trunk
  spanning-tree port type network
  vpc peer-link

interface Ethernet1/4
  description vpc-peer-link
  switchport mode trunk
  channel-group 99 mode active

interface Ethernet1/5
  description vpc-peer-link
  switchport mode trunk
  channel-group 99 mode active


interface mgmt0
  vrf member management
  ip address 10.0.0.2/30


interface port-channel98
  switchport mode trunk
  vpc 10
```

#### leaf-3
```
#### Настройки под Vxlan
fabric forwarding anycast-gateway-mac 0003.0002.0001
vlan 1,10,999
vlan 10
  vn-segment 10010
vlan 999
  vn-segment 10999

vrf context PROD
  vni 10999
  rd 10.3.3.4:10999
  address-family ipv4 unicast
    route-target import 10999:99
    route-target import 10999:99 evpn
    route-target export 10999:99
    route-target export 10999:99 evpn

interface Vlan10
  no shutdown
  vrf member PROD
  no ip redirects
  ip address 192.168.10.252/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway

interface Vlan999
  no shutdown
  vrf member PROD
  no ip redirects
  ip forward
  no ipv6 redirects

  interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 10010
    suppress-arp
    ingress-replication protocol bgp
  member vni 10999 associate-vrf
  
  evpn
  vni 10010 l2
    rd 10.3.3.4:10010
    route-target import 1:10010
    route-target export 1:10010

#### Настройка BGP

router bgp 65004
  router-id 10.3.3.3
  bestpath as-path multipath-relax
  address-family ipv4 unicast
    network 10.3.3.3/32
    network 10.3.3.4/32
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
  neighbor 172.16.1.4
    remote-as 65001
    address-family ipv4 unicast
```
#### Настройка Свича

vlan 10
!
interface Port-Channel1
   switchport trunk allowed vlan 10
   switchport mode trunk
!
interface Ethernet1
   channel-group 1 mode active
!
interface Ethernet2
   channel-group 1 mode active
!
interface Ethernet3
   switchport mode trunk

#### Настройка локального хоста 1
```
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.11 255.255.255.0
 ```
 #### Настройка локального хоста 2
```
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
```

### Вывод  команд проверки 
```
Legend:
                (*) - local vPC is down, forwarding via vPC peer-link

vPC domain id                     : 10
Peer status                       : peer adjacency formed ok
vPC keep-alive status             : peer is alive
Configuration consistency status  : failed
Per-vlan consistency status       : success
Configuration inconsistency reason: Secondary IP address does not match
Type-2 consistency status         : failed
Type-2 inconsistency reason       : SVI type-2 configuration incompatible
vPC role                          : primary
Number of vPCs configured         : 1
Peer Gateway                      : Enabled
Dual-active excluded VLANs        : -
Graceful Consistency Check        : Enabled
Auto-recovery status              : Disabled
Delay-restore status              : Timer is off.(timeout = 30s)
Delay-restore SVI status          : Timer is off.(timeout = 10s)
Operational Layer3 Peer-router    : Disabled
Virtual-peerlink mode             : Disabled

vPC Peer-link status
---------------------------------------------------------------------
id    Port   Status Active vlans
--    ----   ------ -------------------------------------------------
1     Po99   up     1,10,999


vPC status
----------------------------------------------------------------------------
Id    Port          Status Consistency Reason                Active vlans
--    ------------  ------ ----------- ------                ---------------
10    Po98          up     failed      Global compat check   1,10,999

                                       failed


Please check "show vpc consistency-parameters vpc <vpc-num>" for the
consistency reason of down vpc and for type-2 consistency reasons for
any vpc.
```
### Вывод  команд проверки global

Leaf1# sh vpc consistency-parameters global

    Legend:
        Type 1 : vPC will be suspended in case of mismatch

Name                        Type  Local Value            Peer Value
-------------               ----  ---------------------- -----------------------
STP MST Simulate PVST       1     Enabled                Enabled
STP Port Type, Edge         1     Normal, Disabled,      Normal, Disabled,
BPDUFilter, Edge BPDUGuard        Disabled               Disabled
STP MST Region Name         1     ""                     ""
STP Disabled                1     None                   None
STP Mode                    1     Rapid-PVST             Rapid-PVST
STP Bridge Assurance        1     Enabled                Enabled
STP Loopguard               1     Disabled               Disabled
STP MST Region Instance to  1
 VLAN Mapping
STP MST Region Revision     1     0                      0
Interface-vlan admin up     2     10,999                 10
Interface-vlan routing      2     1,10,999               1,10
capability
Nve1 Adm St, Src Adm St,    1     Up, Up, 0.0.0.0, CP,   Up, Up, 0.0.0.0, CP,
Sec IP, Host Reach, VMAC          FALSE, Disabled,       FALSE, Disabled,
Adv, SA,mcast l2, mcast           0.0.0.0, 0.0.0.0,      0.0.0.0, 0.0.0.0,
l3, IR BGP,MS Adm St, Reo         Disabled, Down,        Disabled, Down,
                                  0.0.0.0                0.0.0.0
Xconnect Vlans              1
QoS (Cos)                   2     ([0-7], [], [], [],    ([0-7], [], [], [],
                                  [], [], [], [])        [], [], [], [])
Network QoS (MTU)           2     (1500, 1500, 1500,     (1500, 1500, 1500,
                                  1500, 0, 0, 0, 0)      1500, 0, 0, 0, 0)
Network Qos (Pause:         2     (F, F, F, F, F, F, F,  (F, F, F, F, F, F, F,
T->Enabled, F->Disabled)          F)                     F)
Input Queuing (Bandwidth)   2     (0, 0, 0, 0, 0, 0, 0,  (0, 0, 0, 0, 0, 0, 0,
                                  0)                     0)
Input Queuing (Absolute     2     (F, F, F, F, F, F, F,  (F, F, F, F, F, F, F,
Priority: T->Enabled,             F)                     F)
F->Disabled)
Output Queuing (Bandwidth   2     (100, 0, 0, 0, 0, 0,   (100, 0, 0, 0, 0, 0,
Remaining)                        0, 0)                  0, 0)
Output Queuing (Absolute    2     (F, F, F, T, F, F, F,  (F, F, F, T, F, F, F,
Priority: T->Enabled,             F)                     F)
F->Disabled)
Allowed VLANs               -     1,10,42,999            1,10,999
Local suspended VLANs       -     42                     -
